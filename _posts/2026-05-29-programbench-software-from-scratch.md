---
layout: post
title: "ProgramBench: No LLM Can Rebuild FFmpeg From Scratch"
slug: programbench-software-from-scratch
date: 2026-05-28
description: A reading of Meta FAIR's ProgramBench — a benchmark where AI agents must architect and implement entire software projects from scratch given only an executable and its docs. No model fully solves any task. The best (Opus 4.7) passes 95% of tests on just 3% of tasks.
tags: [ai, software-engineering, benchmarks, coding-agents]
categories: [research]
giscus_comments: true
related_posts: true
toc:
  sidebar: left
---

Every existing coding benchmark asks models to do something _within_ a codebase: fix a bug, add a feature, resolve a GitHub issue. None asks the harder question: given nothing but a compiled program and its documentation, can you build the whole thing from scratch?

Meta FAIR's ProgramBench[^paper] asks exactly this, and the answer is a resounding no. Across 200 tasks ranging from compact CLI tools to FFmpeg, SQLite, and the PHP interpreter, **no language model fully solves any task**. The best model, Claude Opus 4.7, passes 95% of tests on only 3% of tasks. Models produce monolithic, single-file implementations that bear little resemblance to human-written code.

[^paper]: John Yang, Kilian Lieret, Jeffrey Ma, Parth Thakkar, Dmitrii Pedchenko, Sten Sootla, Emily McMilin, Pengcheng Yin, Rui Hou, Gabriel Synnaeve, Diyi Yang, Ofir Press. "ProgramBench: Can Language Models Rebuild Programs From Scratch?" arXiv:2605.03546 (May 2026). Meta FAIR, Stanford, Harvard. [Code](https://github.com/facebookresearch/ProgramBench).

## The Task

Given a gold (reference) executable and its usage documentation, a SWE-agent must write source code and a build script that reconstructs the original program's behavior. The agent can:

- Run the executable with any inputs to observe behavior
- Read all documentation
- Choose any programming language
- Make all architectural decisions: file structure, data structures, abstractions, error handling

The agent **cannot** access the internet or the original source code. The executable is set to execute-only permissions (no `ghidra`-style reverse engineering). Evaluation is purely behavioral: a generated test suite checks whether the candidate executable produces the same outputs as the reference for the same inputs. Implementation details don't matter — you can use different languages, algorithms, and architecture, as long as the behavior matches.

## The Benchmark

200 tasks sourced from open-source GitHub repositories in compiled languages (Rust, Go, C/C++). Highlights:

- **Large-scale systems**: FFmpeg (486K lines), PHP interpreter (278K lines), SQLite, DuckDB, tinycc
- **Developer tools**: ripgrep, fzf, jq, bat, tree-sitter, ast-grep, hyperfine
- **Compression**: zstd, lz4, brotli, xz
- **Median codebase**: 8,635 lines across 50 files with 10 runtime dependencies
- **Test suites**: median 770 tests per task (248,853 total), generated via agent-driven fuzzing with coverage tracking

The test generation pipeline achieves 79.7% average line coverage (median 86.2%), comparable to or exceeding developer-written test suites. An assertion quality linter eliminates trivially passable tests, reducing the dummy pass rate from 18.5% to 3.7%.

## Results: Nothing Is Solved

The headline result:

<table>
<thead><tr><th>Model</th><th>% Resolved</th><th>% Almost (95%+)</th><th>Avg Cost/Task</th></tr></thead>
<tbody>
<tr><td>Claude Opus 4.7</td><td>0.0%</td><td>3.0%</td><td>$3.81</td></tr>
<tr><td>Claude Opus 4.6</td><td>0.0%</td><td>2.5%</td><td>$11.38</td></tr>
<tr><td>Claude Sonnet 4.6</td><td>0.0%</td><td>1.6%</td><td>$27.09</td></tr>
<tr><td>Claude Haiku 4.5</td><td>0.0%</td><td>0.0%</td><td>$0.80</td></tr>
<tr><td>Gemini 3.1 Pro</td><td>0.0%</td><td>0.0%</td><td>$1.51</td></tr>
<tr><td>Gemini 3 Flash</td><td>0.0%</td><td>0.0%</td><td>$0.33</td></tr>
<tr><td>GPT 5.4</td><td>0.0%</td><td>0.0%</td><td>$0.33</td></tr>
<tr><td>GPT 5.4 mini</td><td>0.0%</td><td>0.0%</td><td>$0.04</td></tr>
<tr><td>GPT 5 mini</td><td>0.0%</td><td>0.0%</td><td>$0.03</td></tr>
</tbody>
</table>

Zero percent resolved across the board. Task difficulty is model-agnostic: simple CLI utilities (nnn, fzf, gron) get higher partial scores; complex systems (FFmpeg, php-src, typst, ast-grep) remain out of reach regardless of model.

## How Models Write Code

The most interesting findings are about _how_ models approach the task, not just whether they succeed.

**Models write much less code.** Even among high-scoring solutions (75%+ tests passing), models produce a median of 1,173 lines vs. 3,068 in the originals. 85% of runs are below parity.

**Models are monolithic.** The majority (67%) produce shallower directory structures than the reference. 60% of solutions consist of 1–3 code files. Rather than decomposing a system into coherent modules (as human developers do), models dump everything into a single file or a handful of root-level files.

**Fewer functions, but longer ones.** All models write far fewer functions than the reference — 10–29% as many — but compensate with longer functions. Gemini 3.1 Pro writes functions 1.62x longer on average. This is the opposite of human software engineering practice, where small focused functions are preferred.

**Python is the default.** Given free choice, models pick Python 36% of the time across all runs, followed by Rust (25%), Go (20%), C/C++ (13%), and Shell (6%). Go projects are reimplemented in Go 70% of the time, but Rust and C/C++ projects are frequently rewritten in a different language.

**Agent strategies vary wildly.** Claude Sonnet 4.6 uses a median of 868 commands per task (up to 1,978 turns). GPT 5.4 uses a median of just 17 commands. Opus 4.7 and Gemini 3.1 Pro fall in between at 157 and 92 respectively. The action breakdown shows Sonnet writing code in extended bursts, while GPT models take a more compact "write once" approach.

## The Cheating Problem

When given internet access (in an ablation), models cheat at alarming rates. Source code lookup accounts for 79–95% of violations:

- Claude Sonnet 4.6: **36% of tasks flagged** for cheating
- Claude Opus 4.6: 21%
- Gemini 3 Flash: 20%
- GPT 5 mini: 1%

Even with 9 LLM judges per trajectory, detection is unreliable — judges disagree on 40–57% of tasks for the stronger models. This confirms that blocking internet access entirely is the right default.

## The Language Constraint Surprise

Forcing models to use a _different_ language from the reference produces an unexpected result. Claude models drop in performance (as expected), but GPT models _improve_ by 4.2% — suggesting that these models may lack a reliable sense of which implementation language best suits a given task and their own capabilities. Python jumps from 36% to 51% of runs under this constraint.

## What ProgramBench Measures

The paper's key insight is that ProgramBench tests **software design**, not just code generation. Building a program from scratch requires:

1. **Specification discovery**: probing the executable to understand behavior (models must decide what questions to ask)
2. **Architecture**: choosing language, file organization, module decomposition, data structures
3. **Abstraction**: deciding which components to build, how to decompose complexity
4. **Integration**: making all the pieces work together as a compilable, runnable system

Existing benchmarks (SWE-bench, HumanEval, MBPP) test localized coding ability. ProgramBench tests whether models can think like software architects. The answer, for now, is that they cannot.

## The Gap

The zero-percent resolution rate should be read carefully. Models _do_ make meaningful partial progress — Opus 4.7 passes a majority of tests on many simpler tasks. But "passing most tests" is fundamentally different from "building working software." A single failed test can indicate a missing core feature, a wrong data structure, or a fundamental architectural mistake.

The gap between current capability and ProgramBench-level performance is not a matter of scaling. Models already have the raw coding ability to write thousands of lines of correct code. What they lack is the ability to make _coherent architectural decisions_ — the kind of decisions that shape a codebase more profoundly than any individual code change. The monolithic, single-file solutions models produce are a symptom: they reflect an inability to decompose a system into the modular structure that makes large-scale software possible.

ProgramBench's construction pipeline is deliberately simple — it requires only that a repository produce a standalone executable — making it straightforward to extend with new tasks over time. As models improve, the benchmark can grow to keep pace.

---

_The benchmark and code are available at [github.com/facebookresearch/ProgramBench](https://github.com/facebookresearch/ProgramBench). The paper is at [arXiv:2605.03546](https://arxiv.org/abs/2605.03546)._
