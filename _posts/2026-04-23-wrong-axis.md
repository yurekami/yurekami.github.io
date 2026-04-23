---
layout: post
title: "The Wrong Axis"
date: 2026-04-23
description: A half-dozen recent results in cryptography, post-quantum protocols, and automated bug discovery look like unrelated breakthroughs. They are the same move — a field's legible benchmark decoupling from the underlying cost surface. On why this keeps happening, and what to do about it.
tags: [cryptography, post-quantum, quantum-computing, ai-safety, first-principles, benchmarks]
categories: [research]
giscus_comments: true
related_posts: true
featured: true
slug: wrong-axis
toc:
  sidebar: left
---

A half-dozen technical artifacts from the last eighteen months read as unrelated news. Chevignard-Fouque-Schrottenloher cut ECDLP qubit counts nearly in half. Gidney explained why factoring 21 is ten thousand times harder than factoring 15. Caltech's architecture paper dropped RSA-2048 to ten thousand atoms. Chrome announced MTC. Cloudflare wrote long-form about Q-Day proximity. Mozilla's Mythos model found 271 Firefox bugs in a single generation. Knuth revised his position on generative AI. Google published a zero-knowledge proof of a P-256 attack, which Trail of Bits promptly forged.

They are not unrelated. They are the same move, executed against different fields, in quick succession.

The move is this: a field commits to a legible benchmark. The benchmark approximates a cost function with hidden structure. For a long time the benchmark appears to track reality. Someone publishes the correct coordinate system. The benchmark stops mattering, usually instantly.

This essay is about why the move keeps working, why it will keep working, and what it implies for anyone making decisions against adversarial capabilities.

## First principles

Three axioms.

**One.** Any adversarial capability has a cost function with multiple independent axes — time, space, fidelity, connectivity, parallelism, communication. "Cost" is a point on a surface, never a number.

**Two.** Public benchmarks are selected for legibility, not accuracy. Communities converge on the axis easiest to measure and report, then treat progress along that axis as progress on the capability. This is a social optimization, not a technical one.

**Three.** Capabilities that require fixed infrastructure investment to become operational — quantum error correction, post-quantum protocol deployment, automated reasoning over source — have threshold structure. Below the threshold, progress on the legible axis does not produce capability. Above it, capability arrives discontinuously.

From these three, most of the recent literature falls out.

## Derivations

From (1) and (2), benchmarks track the capability only on the subspace of the cost function where the hidden axes are roughly constant. As long as no one varies architecture, connectivity, or algorithmic structure, a single-axis benchmark can track real progress. The moment someone does vary those, the benchmark discontinuously stops tracking.

From (3), any "progress curve" drawn through pre-threshold demonstrations is near-zero information about post-threshold performance. A field cannot extrapolate from factoring 15 to factoring 2048 because the threshold separates the two regimes entirely. Aaronson's criticality analogy — subcritical mass behaves as nothing forever, critical mass is fission, no intermediate — is the exact geometry.

From (1), (2), and (3) together, expect periodic discontinuous revisions of timelines, not gradual updates. The revision looks like a breakthrough from outside the field but reads, inside the field, as a quiet observation that the benchmark had been measuring the wrong corner of the cost surface.

## Cases

**Qubit count.** Gidney shows that the gate count of factoring N depends on the algebraic structure of N — order of 2 modulo N, proximity to 2ᵏ−1, small-factor content — not on log N. "Largest number factored" tracked instance selection, not algorithm scaling, for two decades. Caltech's companion move: the qubit cost of RSA-2048 depends on rate × distance × processor-size × parallelism × cycle-time. Draw that surface honestly and the minimum-qubit corner is ~10⁴ atoms, and always was. No physics changed between 2019 and 2025. The coordinate system did.

**Signature size.** Chrome's MTC drops the wire cost of a TLS handshake below the size of the underlying post-quantum signature. The mechanism is a Merkle inclusion proof of log(N) rather than transmission of the signature itself. The "PQ signatures are too bulky for TLS" debate lasted five years and was operating on a wrong-axis assumption — that signature size and wire size were coupled. Decoupling them does not improve PQ signatures. It reveals that the bandwidth constraint was never on the signatures.

**Bug discovery.** Mozilla's Mythos Preview found 271 bugs in Firefox in one generation, against 22 for the previous. The legible benchmark — "bugs found per year" — was calibrated to the scarcity of elite human reviewers. When reasoning-over-source becomes machine-scalable, the benchmark stops measuring vulnerability backlog depth (what defenders care about) and starts measuring reviewer throughput (what attackers were paying for). The offense-defense inversion Mozilla describes is the axis-flip made visible.

**Public timelines.** Cloudflare, reading between lines in their own post: "researchers will stop publishing resource estimates once they become actionable." Public quantum timelines were always forecasting not the arrival of a cryptographically relevant quantum computer but the moment public forecasts go dark. Google pulling its internal PQ-auth target to 2029 is a revealed preference. The 2035+ public consensus is residual.

**Proofs of capability.** Google's ZK proof of a P-256 attack and its forgery by Trail of Bits expose a deeper version of the same pattern. The legible benchmark ("was the proof verified?") failed to track the capability ("was the claimed algorithm real?") because the proof system could not distinguish a valid circuit from a compiler artifact on an out-of-range enum. The axis of verification and the axis of truth were decoupled by a bounds-check optimization in rkyv. The forgery was information-theoretically indistinguishable from the real proof. Only domain literacy closed the gap.

Across all five cases the pattern is identical. Benchmark converges. Benchmark is legible. Benchmark is plausible. Benchmark decouples. Someone publishes the correct surface. The decoupling looks like a breakthrough. It is a measurement.

## Consequences

Four, in order of increasing discomfort.

**Pre-threshold demonstrations carry near-zero information about post-threshold capability.** Not new — Aaronson has made this point for a decade — but it has operational force now that several fields are near their thresholds simultaneously. Asking how close AGI is by measuring task completion, how close a cryptographically relevant quantum computer is by measuring factored integers, or how close autonomous exploit generation is by counting handcrafted CVEs is a category error. The measurement that matters is whichever axis is still hidden.

**Rational migration must precede the legible signal.** If the benchmark is near-coincident with the capability itself — factoring RSA-2048 and getting to factoring RSA-2048 are the same event, separated by microseconds — then waiting for the signal is waiting for the event. Anyone planning PQ migration against "the first RSA-2048 break demonstration" has already lost. The only rational trigger is upstream: architecture papers, rate improvements, duty-cycle engineering. Cloudflare, Chrome, and Google migrate on those signals. Most enterprises still wait for the demonstration. The gap between those two populations is the window in which a lot of value transfers.

**Revealed preferences of builders dominate public statements.** Google builds both quantum hardware and PQ defenses. When Google pulls its internal authentication migration forward, that is a stronger signal than any timeline Google publishes. The asymmetry is structural: the builder has more accurate private information and costly incentives not to disclose it. Reading the field correctly means treating public consensus as a lower bound on concern, not an estimate.

**The meta-level is the same problem.** The most disquieting implication: this essay is vulnerable to its own thesis. The axes we have chosen to evaluate AI capabilities — benchmark scores, parameter counts, training compute — are the same kind of legibility-selected proxies that gave us "largest number factored" for quantum. If prior fields are evidence, the AI cost function has hidden axes that someone will publish in a short paper that appears to compress timelines by a factor of 10³. The compression will not be a breakthrough. It will be a measurement.

## What to do with this

If you are deciding anything that depends on the timing of an adversarial capability — cryptographic migration, security posture, investment allocation, research prioritization — three moves are structurally correct.

First, identify the hidden axes. Any field with a single dominant benchmark has them. Ask: what has every paper held constant? What would it cost to vary? The cost surface lives in the variables no one has been publishing.

Second, watch the architecture papers, not the demonstration papers. An architectural result collapses the cost function instantly. A demonstration result moves one point on the surface by a constant factor.

Third, treat public timelines as the upper bound, not the estimate. The sum of all incentives — researcher self-promotion, vendor competition, regulatory caution, adversary operational security — biases public claims toward "sooner than you said, later than reality." The rational prior is that any public capability timeline is off by a year in the direction the disclosing party prefers.

## Coda

The pattern is old. Kuhn described it in the physical sciences; the mechanism repeats in any discipline where a legible metric becomes load-bearing for planning. What is new is compression. Multiple fields are hitting the axis problem simultaneously — cryptography, AI, security, infrastructure — and the revisions are arriving within months of one another rather than decades.

For anyone operating across these fields at once, which is increasingly everyone who reads this, the practical implication is that the 2026–2029 window contains more coordinate-system publications than threshold crossings. The map is being redrawn faster than the territory is changing. Treating every such paper as a breakthrough will exhaust you. Treating them as measurements — reports from people who finally drew the right curve — is calmer and more accurate.

The wrong axis is a feature, not a bug. Every field has one. The discipline is identifying yours before someone else publishes it.
