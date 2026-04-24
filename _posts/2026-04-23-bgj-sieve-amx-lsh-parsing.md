---
layout: post
title: "A Silent Off-by-One in the World's Fastest Lattice Sieve"
date: 2026-04-23
description: A 35,000-line SIMD-heavy BGJ sieve implementation ships with a quiet bug in its command-line parser. The lsh flag fails to advance its argument index, and the default-recovery guard excludes the one branch that needs it. The result is a silent misconfiguration of the final-pump LSH ratio — zero where the author expected 0.1 or 0.2 — passed directly into the critical path of an SVP challenge run. A note on why argv parsers deserve the same care as inner loops.
tags: [lattice-cryptography, svp, bgj-sieve, cli-parsing, post-quantum, code-review]
categories: [cryptography]
giscus_comments: true
related_posts: true
featured: false
slug: bgj-sieve-amx-lsh-parsing
toc:
  sidebar: left
---

[BGJ-Sieve-AMX](https://github.com/zhaoziyu0008/BGJ-Sieve-AMX) is the reference implementation that accompanies Zhao, Ding and Yang's *Sieving with Streaming Memory Access*. It is, by several metrics, the fastest publicly available BGJ sieve for the Shortest Vector Problem — hand-tuned for Intel's AVX512-VNNI and AMX-INT8 instruction sets, roughly 35,000 lines of C++ with non-trivial inline assembly, and the kind of code where you expect the bugs to live inside the bucketing kernels or the quadratic-float arithmetic.

They don't. The bug I want to describe is in a twelve-line argv loop, and it quietly corrupts the parameters fed to the final pump — the innermost, most expensive phase of an SVP challenge run.

## The parser

The entry point is `app/bin_svp_tool.cpp`. The argument loop uses the common `atol(argv[++i])` idiom for every multi-arg option except one:

```cpp
} else if (!strcasecmp(argv[i], "-lsh") || !strcasecmp(argv[i], "--lsh")) {
    lsh = 1;
    if (i < argc - 1) lsh_qr = atof(argv[i+1]);
    if (i < argc - 2) lsh_er = atof(argv[i+2]);
}
```

Note what is absent. `i` is not advanced past the two positional numbers. Every other flag — `-l`, `-r`, `-s`, `-t`, `-v`, `-msd`, `-esd`, `-ssd`, `-ds` — uses `atol(argv[++i])`. This one peeks.

The loop is a plain `if/else if` chain with no terminal `else` and no unknown-flag warning, so tokens that fall off the end are silently discarded. This means the happy path still *appears* to work: invoke `--lsh 0.15 0.2`, and on the next two iterations `0.15` and `0.2` match nothing and vanish, leaving `lsh_qr = 0.15` and `lsh_er = 0.2` as intended. The bug does not fire on the example the author tested.

## Two ways the bug fires

**Shape 1: adjacent flags.** Invoke `--lsh -t 4`. The parser evaluates `atof("-t") == 0.0` and `atof("4") == 4.0`. `lsh_qr` is set to zero; `lsh_er` is set to the *thread count*. The loop then advances to `-t`, which is handled correctly, so `num_threads = 4` also. The user sees a run that launches with four threads and believes they passed no LSH parameters. They did not. `lsh_er = 4.0` is about twenty times the intended value, and is about to be passed into the LSH filter as a radius scaling constant.

**Shape 2: the `--final` trapdoor.** A few lines later, the defaults guard reads:

```cpp
if (lsh && lsh_qr == 0.0 && !fin) lsh_qr = amx ? 0.1 : 0.2;
if (lsh && lsh_er == 0.0 && !fin) lsh_er = amx ? 0.1 : 0.2;
```

The `!fin` predicate is the defect. `lsh_er` is referenced in exactly one place in the program — the final-pump call site:

```cpp
__last_lsh_pump_amx(&L, num_threads, lsh_qr, lsh_er, msd, esd, log_level, 0, ssd);
__last_lsh_pump_epi8(&L, num_threads, lsh_qr, lsh_er, msd, esd, log_level, 0, ssd);
```

These are the `fin == 1` branch. In `!fin` runs, the defaults exist, but `lsh_er` is dead; in `fin` runs, where `lsh_er` is live and on the critical path, the defaults are disabled. The two guards have it precisely backwards.

Running `./svp_tool basis -f --lsh --msd 120 --esd 20` — a plausible invocation for a large SVP attempt — produces `lsh_qr = 0.0` and `lsh_er = 0.0`, both of which flow unmodified into `__last_lsh_pump_*`. A ratio of zero in an LSH filter is not a conservative fallback; it is an argument out of the tested regime, and the sieve will either over-reject every candidate pair or divide through a quantity that was assumed strictly positive.

## Why it matters more than it looks

Two compounding properties make this worse than a typical argv bug.

First, **SVP runs are long**. A dimension-120 pump on consumer hardware is hours; on a server it is minutes but on a workload you care about it is a day. A silent misconfiguration is not caught in fifteen seconds the way a unit-test failure is. The user waits, the run completes with plausible-looking logs, and the output is a basis that is subtly less reduced than the theoretical cost model predicted. There is no assertion, anywhere in the path, that `lsh_er > 0`.

Second, **benchmarks are the downstream consumer**. The BGJ-Sieve-AMX README positions the library as the reference against which other sieves should compare. A CLI that silently runs with `lsh_er = 0` in the final phase means every independent reproduction of the paper's numbers is conditioned on the reproducer happening to pass all three arguments in the right order, with no intervening flags. This is the kind of configuration fragility that produces "we couldn't reproduce" papers three years later, and nobody ever traces the discrepancy to `argv[i+1]`.

## The fix

Three lines:

```cpp
} else if (!strcasecmp(argv[i], "-lsh") || !strcasecmp(argv[i], "--lsh")) {
    lsh = 1;
    if (i + 1 < argc && argv[i+1][0] != '-') lsh_qr = atof(argv[++i]);
    if (i + 1 < argc && argv[i+1][0] != '-') lsh_er = atof(argv[++i]);
}
```

And remove the `!fin` predicate from the default-recovery lines. `lsh_er` wants its default whether or not we are in final mode; in fact especially in final mode.

A belt-and-suspenders version would also emit `fprintf(stderr, "warning: unrecognized argument %s\n", argv[i])` in the terminal `else` — the loop's lack of one is why neither shape of the bug ever surfaced as output.

## Three smaller things on the same page

While we are in the file:

- Line 117: `if (msd == 0) help = 1;` silently redirects to the help screen when the required flag is missing. The user reads the help text and assumes they formatted the flag wrong; they did not, the flag is simply required and the error message is absent.
- Lines 141, 144, 149, 152, 263, 266, 271, 274: `(msd - 50)` is passed into every pump as the left-shift parameter with no floor at zero. `--msd 30` sends `-20` into a function that almost certainly treats it as an unsigned count.
- Lines 283–284: `char output_file[256]; sprintf(output_file, "%s%s", argv[1], sr ? "" : "r");` is a fixed-buffer `sprintf`. The tool trusts that nobody will pass it a 255-character input filename. In a research context this is fine. In any pipeline where the filename is derived from a hash or a UUID prefix, it is not.

None of these three are the bug above. They are reminders that the argv layer gets less scrutiny than the inner loop, and the inner loop is where this codebase is otherwise superb.

## A closing note on attention allocation

The sieving kernels in this repository are beautiful. They dispatch AMX tile configurations based on compile-time pool sizes, they pack signed-int8 lattice vectors into exactly the layout the bucketing logic wants, and the 3Sieve variants compose cleanly with the progressive-BKZ wrapper. Whoever wrote `bucket_amx.cpp` is operating several tiers above my pay grade.

And it still ships with a broken argument parser, because argument parsers are boring, and we save our attention for the parts that are hard. This is the general shape of most real-world software bugs. The cost function is not uniform over the codebase; the attention allocation is also not uniform; and the bugs survive in the delta.

If you are running SVP benchmarks against this library and you use `-f --lsh`, pass both numeric arguments explicitly, keep them adjacent to the flag, and check your `lsh_er` default before you trust the comparison.
