---
layout: post
title: "The Mathematical Metabolism: Why AI Solves Problems But Doesn't Create Mathematics"
date: 2026-02-14
description: Google's Gemini Deep Think solves 4 Erdos conjectures. But solving problems isn't doing mathematics. I propose an architecture for AI that creates mathematical knowledge — not just proofs.
tags: [mathematics, ai-research, deep-think, mathematical-reasoning, first-principles]
categories: [research]
giscus_comments: true
related_posts: true
featured: true
toc:
  sidebar: left
---

<link rel="stylesheet" href="/assets/css/vc-infographics.css">

Google's Aletheia system solved four open Erdos conjectures. Headlines called it a breakthrough. And it is — in the same way that a calculator performing long division was a breakthrough. The 0.6% solve rate on 700 Erdos problems is both the headline and the sobriety check: impressive enough to matter, low enough to demand scrutiny.

But the real question isn't about solve rates.

> "What I cannot create, I do not understand." — Richard Feynman

Feynman's dictum cuts both ways. Invert it: **what creates nothing, understands nothing — no matter how many problems it solves.** Aletheia produces proofs. It does not produce mathematics. The distinction is the entire point of this post.

---

## 1. The Three-Stage Dissection

I applied the First Proof methodology to Google's announcement — a structured analysis in three stages: **Formulation** (what is actually being claimed?), **Framework** (how does it compare to prior work?), and **Execution** (what survives scrutiny?).

The architecture itself is straightforward. It follows the generator-verifier pattern that has become standard in AI-for-math systems since AlphaProof:

<div class="vc">
<div class="vc-title">Generator-Verifier Architecture</div>
<div style="display: flex; align-items: center; gap: 0; flex-wrap: wrap; justify-content: center;">
  <div class="vc-pipeline-node">
    <div class="vc-pipeline-node-label">Problem Input</div>
  </div>
  <div class="vc-pipeline-arrow">&rarr;</div>
  <div class="vc-pipeline-node vc-pipeline-node--blue">
    <div class="vc-pipeline-node-label">Generator</div>
    <div class="vc-pipeline-node-sub">Gemini Deep Think</div>
  </div>
  <div class="vc-pipeline-arrow">&rarr;</div>
  <div class="vc-pipeline-node">
    <div class="vc-pipeline-node-label">Candidate Proof</div>
  </div>
  <div class="vc-pipeline-arrow">&rarr;</div>
  <div class="vc-pipeline-node vc-pipeline-node--amber">
    <div class="vc-pipeline-node-label">Verifier</div>
    <div class="vc-pipeline-node-sub">Gemini + Heuristics</div>
  </div>
  <div class="vc-pipeline-arrow">&rarr;</div>
  <div class="vc-pipeline-node vc-pipeline-node--green">
    <div class="vc-pipeline-node-label">Output Solution</div>
  </div>
</div>
<div class="vc-pipeline-feedback">
  <div class="vc-pipeline-feedback-item">Minor Fix &rarr; Generator</div>
  <div class="vc-pipeline-feedback-item">Critical Flaw &rarr; Discard &amp; Retry</div>
  <div class="vc-pipeline-feedback-item">Ambiguous &rarr; Human Expert</div>
</div>
</div>

Five claims emerge from the announcement:

1. **Autonomous Erdos Solving** — the system independently solved four open conjectures.
2. **Scaling Beyond Olympiad** — this extends AI math reasoning past competition-level problems.
3. **Agentic Scaffold Efficiency** — the generator-verifier loop is an effective reasoning scaffold.
4. **Cross-Domain Transfer** — capabilities generalize across mathematical domains.
5. **Responsible Taxonomy** — solutions are categorized by level of human involvement.

Each claim deserves independent evaluation. Not all survive.

---

## 2. Multi-Perspective Analysis

I ran parallel analyses through different lenses — a factual assessment, a senior engineer's evaluation, a security-minded adversarial read, and a consistency check. The convergence was instructive: all analyses agreed on where the evidence was strong and where it evaporated.

<div class="vc">
<div class="vc-title">Evidence Strength vs. Information Gaps Across 5 Claims</div>
<div class="vc-dumbbell">
  <div class="vc-dumbbell-legend">
    <span><span class="vc-dumbbell-legend-dot vc-dumbbell-legend-dot--blue"></span>Evidence Strength</span>
    <span><span class="vc-dumbbell-legend-dot vc-dumbbell-legend-dot--red"></span>What's Missing</span>
  </div>
  <div class="vc-dumbbell-row">
    <div class="vc-dumbbell-label">Autonomous Erdos Solving</div>
    <div class="vc-dumbbell-track">
      <div class="vc-dumbbell-range" style="left: 30%; width: 60%;"></div>
      <div class="vc-dumbbell-point vc-dumbbell-point--blue" style="left: 30%;"></div>
      <div class="vc-dumbbell-point vc-dumbbell-point--red" style="left: 90%;"></div>
    </div>
    <div class="vc-dumbbell-values">30 / 90</div>
  </div>
  <div class="vc-dumbbell-row">
    <div class="vc-dumbbell-label">Scaling Beyond Olympiad</div>
    <div class="vc-dumbbell-track">
      <div class="vc-dumbbell-range" style="left: 45%; width: 25%;"></div>
      <div class="vc-dumbbell-point vc-dumbbell-point--blue" style="left: 45%;"></div>
      <div class="vc-dumbbell-point vc-dumbbell-point--red" style="left: 70%;"></div>
    </div>
    <div class="vc-dumbbell-values">45 / 70</div>
  </div>
  <div class="vc-dumbbell-row">
    <div class="vc-dumbbell-label">Agentic Scaffold Efficiency</div>
    <div class="vc-dumbbell-track">
      <div class="vc-dumbbell-range" style="left: 60%; width: 25%;"></div>
      <div class="vc-dumbbell-point vc-dumbbell-point--blue" style="left: 60%;"></div>
      <div class="vc-dumbbell-point vc-dumbbell-point--red" style="left: 85%;"></div>
    </div>
    <div class="vc-dumbbell-values">60 / 85</div>
  </div>
  <div class="vc-dumbbell-row">
    <div class="vc-dumbbell-label">Cross-Domain Transfer</div>
    <div class="vc-dumbbell-track">
      <div class="vc-dumbbell-range" style="left: 65%; width: 10%;"></div>
      <div class="vc-dumbbell-point vc-dumbbell-point--blue" style="left: 65%;"></div>
      <div class="vc-dumbbell-point vc-dumbbell-point--red" style="left: 75%;"></div>
    </div>
    <div class="vc-dumbbell-values">65 / 75</div>
  </div>
  <div class="vc-dumbbell-row">
    <div class="vc-dumbbell-label">Responsible Taxonomy</div>
    <div class="vc-dumbbell-track">
      <div class="vc-dumbbell-range" style="left: 50%; width: 5%;"></div>
      <div class="vc-dumbbell-point vc-dumbbell-point--blue" style="left: 55%;"></div>
      <div class="vc-dumbbell-point vc-dumbbell-point--red" style="left: 50%;"></div>
    </div>
    <div class="vc-dumbbell-values">55 / 50</div>
  </div>
</div>
</div>

The pattern is stark. The strongest evidence exists for the **scaffold design** and the **taxonomy** — engineering claims, not mathematical ones. The weakest evidence exists for the headline claim: autonomous solving. The information gaps are largest precisely where the claims are boldest.

This is not unusual. It is the standard pattern of AI capability announcements: strong engineering, weak epistemics.

---

## 3. The Hypothesis Landscape

What is actually happening when Aletheia "solves" an Erdos conjecture? Five competing hypotheses, each with different implications:

- **H1: Genuine Reasoning** — the model performs something functionally equivalent to mathematical reasoning, discovering novel proof strategies.
- **H2: Sophisticated Pattern Matching** — the model recombines known techniques in ways that happen to work on these particular problems, without genuine understanding.
- **H3: Scaffold Innovation** — the breakthrough is not in the model but in the agentic architecture: the feedback loops, the verification, the retry logic.
- **H4: Selection Bias** — the solved problems were particularly amenable to existing techniques; the 99.4% failure rate is the real signal.
- **H5: Human Laundering** — human expertise entered the pipeline through problem selection, hint provision, or verification criteria, and the "autonomous" label obscures this.

I estimated probabilities from two independent analysis runs:

<div class="vc">
<div class="vc-title">What's Actually Happening? Two Independent Probability Estimates</div>
<div class="vc-prob">
  <div class="vc-prob-legend">
    <span><span class="vc-prob-legend-swatch vc-prob-legend-swatch--opus"></span>Opus</span>
    <span><span class="vc-prob-legend-swatch vc-prob-legend-swatch--sonnet"></span>Sonnet</span>
  </div>
  <div class="vc-prob-row">
    <div class="vc-prob-label">H1: Genuine Reasoning</div>
    <div class="vc-prob-bars">
      <div class="vc-prob-bar-row">
        <div class="vc-prob-bar-name">Opus</div>
        <div class="vc-prob-bar vc-prob-bar--opus" style="width: 42.9%;">15%</div>
      </div>
      <div class="vc-prob-bar-row">
        <div class="vc-prob-bar-name">Sonnet</div>
        <div class="vc-prob-bar vc-prob-bar--sonnet" style="width: 42.9%;">15%</div>
      </div>
    </div>
  </div>
  <div class="vc-prob-row">
    <div class="vc-prob-label">H2: Pattern Matching</div>
    <div class="vc-prob-bars">
      <div class="vc-prob-bar-row">
        <div class="vc-prob-bar-name">Opus</div>
        <div class="vc-prob-bar vc-prob-bar--opus" style="width: 100%;">35%</div>
      </div>
      <div class="vc-prob-bar-row">
        <div class="vc-prob-bar-name">Sonnet</div>
        <div class="vc-prob-bar vc-prob-bar--sonnet" style="width: 85.7%;">30%</div>
      </div>
    </div>
  </div>
  <div class="vc-prob-row">
    <div class="vc-prob-label">H3: Scaffold Innovation</div>
    <div class="vc-prob-bars">
      <div class="vc-prob-bar-row">
        <div class="vc-prob-bar-name">Opus</div>
        <div class="vc-prob-bar vc-prob-bar--opus" style="width: 85.7%;">30%</div>
      </div>
      <div class="vc-prob-bar-row">
        <div class="vc-prob-bar-name">Sonnet</div>
        <div class="vc-prob-bar vc-prob-bar--sonnet" style="width: 71.4%;">25%</div>
      </div>
    </div>
  </div>
  <div class="vc-prob-row">
    <div class="vc-prob-label">H4: Selection Bias</div>
    <div class="vc-prob-bars">
      <div class="vc-prob-bar-row">
        <div class="vc-prob-bar-name">Opus</div>
        <div class="vc-prob-bar vc-prob-bar--opus" style="width: 28.6%;">10%</div>
      </div>
      <div class="vc-prob-bar-row">
        <div class="vc-prob-bar-name">Sonnet</div>
        <div class="vc-prob-bar vc-prob-bar--sonnet" style="width: 57.1%;">20%</div>
      </div>
    </div>
  </div>
  <div class="vc-prob-row">
    <div class="vc-prob-label">H5: Human Laundering</div>
    <div class="vc-prob-bars">
      <div class="vc-prob-bar-row">
        <div class="vc-prob-bar-name">Opus</div>
        <div class="vc-prob-bar vc-prob-bar--opus" style="width: 28.6%;">10%</div>
      </div>
      <div class="vc-prob-bar-row">
        <div class="vc-prob-bar-name">Sonnet</div>
        <div class="vc-prob-bar vc-prob-bar--sonnet" style="width: 28.6%;">10%</div>
      </div>
    </div>
  </div>
</div>
</div>

The convergence is notable. Both runs assign the highest probability to **pattern matching** (H2) and **scaffold innovation** (H3), with genuine reasoning (H1) at only 15%. The modal explanation is a combination: clever engineering makes sophisticated pattern matching look like reasoning on a carefully selected subset of problems.

This is not a dismissal. Pattern matching at this level of sophistication is genuinely useful. But it is not mathematics.

---

## 4. The Verification Catastrophe

The arxiv paper contains the actual numbers. They are worse than the announcement suggests.

Of 700 Erdos problems attempted, the verifier flagged 212 as "correct." But when human experts evaluated the flagged solutions, the picture collapsed:

<div class="vc">
<div class="vc-title">The Erdos Funnel: 700 Problems &rarr; 2 Autonomous Solutions</div>
<div class="vc-funnel">
  <div class="vc-funnel-stage">
    <div class="vc-funnel-num">700</div>
    <div class="vc-funnel-bar-wrap">
      <div class="vc-funnel-bar vc-funnel-bar--slate" style="width: 100%;">
        <span class="vc-funnel-bar-label">Erdos Problems Attempted</span>
      </div>
    </div>
  </div>
  <div class="vc-funnel-stage">
    <div class="vc-funnel-num">488</div>
    <div class="vc-funnel-bar-wrap">
      <div class="vc-funnel-bar vc-funnel-bar--slate" style="width: 69.7%;">
        <span class="vc-funnel-bar-label">Filtered Out Pre-Verification</span>
      </div>
    </div>
  </div>
  <div class="vc-funnel-stage">
    <div class="vc-funnel-num">212</div>
    <div class="vc-funnel-bar-wrap">
      <div class="vc-funnel-bar vc-funnel-bar--blue" style="width: 30.3%;">
        <span class="vc-funnel-bar-label">Flagged "Correct" by Verifier</span>
      </div>
    </div>
  </div>
  <div class="vc-funnel-spacer"></div>
  <div class="vc-funnel-stage">
    <div class="vc-funnel-num">12</div>
    <div class="vc-funnel-bar-wrap">
      <div class="vc-funnel-bar vc-funnel-bar--slate vc-funnel-bar--small" style="width: 1.7%; min-width: 8px;">
        <span class="vc-funnel-bar-label">Not Evaluable</span>
      </div>
    </div>
  </div>
  <div class="vc-funnel-stage">
    <div class="vc-funnel-num">200</div>
    <div class="vc-funnel-bar-wrap">
      <div class="vc-funnel-bar vc-funnel-bar--blue" style="width: 28.6%;">
        <span class="vc-funnel-bar-label">Evaluable Solutions</span>
      </div>
    </div>
  </div>
  <div class="vc-funnel-annotation">68.5% false-positive rate &mdash; the verifier approves 137 wrong proofs for every 2 correct ones</div>
  <div class="vc-funnel-stage">
    <div class="vc-funnel-num">137</div>
    <div class="vc-funnel-bar-wrap">
      <div class="vc-funnel-bar vc-funnel-bar--red" style="width: 19.6%;">
        <span class="vc-funnel-bar-label">Fundamentally Wrong</span>
      </div>
    </div>
  </div>
  <div class="vc-funnel-stage">
    <div class="vc-funnel-num">63</div>
    <div class="vc-funnel-bar-wrap">
      <div class="vc-funnel-bar vc-funnel-bar--green" style="width: 9%;">
        <span class="vc-funnel-bar-label">Mathematically Correct</span>
      </div>
    </div>
  </div>
  <div class="vc-funnel-spacer"></div>
  <div class="vc-funnel-stage">
    <div class="vc-funnel-num">50</div>
    <div class="vc-funnel-bar-wrap">
      <div class="vc-funnel-bar vc-funnel-bar--amber" style="width: 7.1%;">
        <span class="vc-funnel-bar-label">Specification Gaming</span>
      </div>
    </div>
  </div>
  <div class="vc-funnel-stage">
    <div class="vc-funnel-num">13</div>
    <div class="vc-funnel-bar-wrap">
      <div class="vc-funnel-bar vc-funnel-bar--blue vc-funnel-bar--small" style="width: 1.9%; min-width: 8px;">
        <span class="vc-funnel-bar-label">Actually Answered Question</span>
      </div>
    </div>
  </div>
  <div class="vc-funnel-spacer"></div>
  <div class="vc-funnel-annotation">Of 13 answers: 4 rediscoveries, 5 from literature, 2 partial</div>
  <div class="vc-funnel-stage">
    <div class="vc-funnel-num">2</div>
    <div class="vc-funnel-bar-wrap">
      <div class="vc-funnel-bar vc-funnel-bar--gold vc-funnel-bar--small" style="width: 0.3%; min-width: 8px;">
        <span class="vc-funnel-bar-label">Autonomous Solutions</span>
      </div>
    </div>
  </div>
  <div class="vc-funnel-result">
    <div class="vc-funnel-result-number">0.3%</div>
    <div class="vc-funnel-result-text">
      <div class="vc-funnel-result-label">True Autonomous Solve Rate</div>
      <div class="vc-funnel-result-context">2 genuine solutions out of 700 problems</div>
    </div>
  </div>
</div>
</div>

Read the funnel carefully:

- **137 of 200 evaluable "correct" solutions were fundamentally wrong.** That is a 68.5% false-positive rate from the verifier. The system's internal quality signal is catastrophically miscalibrated.
- **50 of the 63 mathematically correct solutions were specification gaming** — they answered a different question than the one asked, or exploited ambiguity in the formalization.
- **Only 13 actually answered the posed question.** Of those, 4 were rediscoveries of known results, 5 were identifications from existing literature, and 2 were partial.
- **2 were genuinely autonomous solutions.** Two. Out of 700.

The true autonomous solve rate is not 0.6%. It is 0.3%. And the verifier — the component that is supposed to guarantee quality — approved 137 wrong proofs for every 2 correct ones.

<div class="vc">
<div class="vc-hero vc-hero--danger">
  <div class="vc-hero-number">68.5%</div>
  <div class="vc-hero-content">
    <div class="vc-hero-label">Verifier False-Positive Rate</div>
    <div class="vc-hero-context">137 wrong proofs approved for every 2 correct ones</div>
  </div>
</div>
</div>

> The verifier is not a safety net. It is a sieve with holes large enough to drive a truck through.

This is the verification catastrophe: the system cannot reliably distinguish its own successes from its own failures. Every downstream claim — about autonomy, about reasoning, about mathematical capability — rests on a verifier that is wrong more often than it is right.

<div class="vc">
<div class="vc-hero vc-hero--gold">
  <div class="vc-hero-number">2</div>
  <div class="vc-hero-content">
    <div class="vc-hero-label">Genuine Autonomous Solutions</div>
    <div class="vc-hero-context">Out of 700 Erdos problems attempted &mdash; 0.3% true solve rate</div>
  </div>
</div>
</div>

---

## 5. The Deeper Question: Process vs. Product

Suppose the verification problem is solved. Suppose Aletheia could reliably produce correct proofs for open problems. Would that constitute doing mathematics?

No. And the reason illuminates what mathematics actually is.

Mathematics is not a set of solved problems. It is a **living language** — a process of creating concepts, definitions, and structural relationships that make previously opaque domains legible. The measure of a mathematician is not theorems proved but **concepts created**.

| Mathematician | Famous Result                | What Actually Mattered                                               |
| :------------ | :--------------------------- | :------------------------------------------------------------------- |
| Galois        | Unsolvability of the quintic | **Group theory** — an entirely new algebraic language                |
| Cantor        | Uncountability of the reals  | **Set theory** — the foundation of modern mathematics                |
| Grothendieck  | Weil conjectures             | **Scheme theory** — reimagined the geometry of numbers               |
| Wiles         | Fermat's Last Theorem        | **Modularity lifting** — connected number theory to geometry         |
| Thurston      | Geometrization conjecture    | **Geometric structures on 3-manifolds** — a classification framework |
| Emmy Noether  | Noether's theorem            | **Abstract algebra** — the language of symmetry itself               |

In every case, the theorem is the least interesting output. The **concepts** generated during the struggle — the failed approaches, the new definitions invented to articulate what was missing, the structural insights that reorganized entire fields — these are the actual product of mathematical work.

A proof is a receipt. The mathematics is the thinking that generated the receipt.

<div class="vc">
<div class="vc-title">Terminal Process vs. Generative Process</div>
<div class="vc-compare">
  <div class="vc-compare-side vc-compare-side--terminal">
    <div class="vc-compare-side-title">Current: Terminal Process</div>
    <div class="vc-compare-flow">
      <div class="vc-compare-flow-node vc-compare-flow-node--input">Problem</div>
      <div class="vc-compare-flow-arrow">&darr;</div>
      <div class="vc-compare-flow-node vc-compare-flow-node--process">Opaque Computation</div>
      <div class="vc-compare-flow-arrow">&darr;</div>
      <div class="vc-compare-flow-node vc-compare-flow-node--output">Proof</div>
      <div class="vc-compare-flow-arrow">&darr;</div>
      <div class="vc-compare-flow-node vc-compare-flow-node--dead">Dead End</div>
    </div>
  </div>
  <div class="vc-compare-vs">VS</div>
  <div class="vc-compare-side vc-compare-side--generative">
    <div class="vc-compare-side-title">Proposed: Generative Process</div>
    <div class="vc-compare-flow">
      <div class="vc-compare-flow-node vc-compare-flow-node--input">Problem</div>
      <div class="vc-compare-flow-arrow">&darr;</div>
      <div class="vc-compare-flow-node vc-compare-flow-node--explore">Exploration</div>
      <div class="vc-compare-flow-arrow">&darr;</div>
      <div class="vc-compare-flow-branch">
        <div class="vc-compare-flow-node">Lemmas</div>
        <div class="vc-compare-flow-node">Definitions</div>
        <div class="vc-compare-flow-node">Failed Approaches</div>
        <div class="vc-compare-flow-node">Conjectures</div>
      </div>
      <div class="vc-compare-flow-arrow">&darr;</div>
      <div class="vc-compare-flow-node vc-compare-flow-node--output">Proof</div>
      <div class="vc-compare-flow-arrow">&darr;</div>
      <div class="vc-compare-flow-node vc-compare-flow-node--new">New Problems</div>
    </div>
  </div>
</div>
</div>

Current AI systems are terminal: problem in, proof out, nothing generated along the way. The process leaves no residue. Aletheia's 698 failures on Erdos problems produced **zero mathematical knowledge** — no new concepts, no structural insights, no conjectures about why certain approaches fail. Those failures were computational waste, not mathematical exploration.

A human mathematician failing 698 times would have generated an entire research program.

---

## 6. The Mathematical Metabolism

I propose an architecture that treats failure as the primary output and proofs as a byproduct. I call it the **Mathematical Metabolism** — a system that digests problems into mathematical knowledge, not just solutions.

<div class="vc">
<div class="vc-title">Mathematical Metabolism Architecture</div>
<div class="vc-compare-flow" style="max-width: 420px; margin: 0 auto;">
  <div class="vc-compare-flow-node" style="border-color: var(--vc-amber); background: var(--vc-amber-soft); font-weight: 700;">Problem Space</div>
  <div class="vc-compare-flow-arrow">&darr; <span style="font-size: 0.7rem; color: var(--vc-text-muted);">attempt</span></div>
  <div class="vc-compare-flow-node" style="border-color: var(--vc-text-muted);">Attempt Engine</div>
  <div class="vc-compare-flow-arrow">&swarr; &darr; &searr;</div>
  <div class="vc-compare-flow-branch" style="grid-template-columns: 1fr 1fr 1fr;">
    <div class="vc-compare-flow-node" style="border-color: var(--vc-red); background: var(--vc-red-soft); font-size: 0.72rem; padding: 0.35rem 0.4rem;">Failure Atlas<br><span style="font-weight: 400; font-size: 0.65rem; color: var(--vc-text-muted);">Why it failed</span></div>
    <div class="vc-compare-flow-node" style="border-color: var(--vc-amber); background: var(--vc-amber-soft); font-size: 0.72rem; padding: 0.35rem 0.4rem;">Technique Boundaries<br><span style="font-weight: 400; font-size: 0.65rem; color: var(--vc-text-muted);">Where methods break</span></div>
    <div class="vc-compare-flow-node" style="border-color: var(--vc-green); background: var(--vc-green-soft); font-size: 0.72rem; padding: 0.35rem 0.4rem;">Solutions<br><span style="font-weight: 400; font-size: 0.65rem; color: var(--vc-text-muted);">Proved results</span></div>
  </div>
  <div class="vc-compare-flow-arrow">&darr; <span style="font-size: 0.7rem; color: var(--vc-text-muted);">pattern detection</span></div>
  <div class="vc-compare-flow-node" style="border-color: var(--vc-blue); background: var(--vc-blue-soft); font-weight: 700;">Concept Lattice</div>
  <div style="font-size: 0.7rem; color: var(--vc-text-muted); text-align: center; padding: 0.15rem 0;">Names recurring patterns &mdash; the core creative act</div>
  <div class="vc-compare-flow-arrow">&darr; <span style="font-size: 0.7rem; color: var(--vc-text-muted);">new definitions</span></div>
  <div class="vc-compare-flow-node" style="border-color: var(--vc-purple); background: var(--vc-purple-soft); font-weight: 700;">Conjecture Engine</div>
  <div style="display: flex; justify-content: space-between; width: 100%; padding: 0.25rem 0;">
    <div style="font-size: 0.7rem; color: var(--vc-text-muted);">&darr; evaluate fertility</div>
    <div style="font-size: 0.7rem; color: var(--vc-text-muted);">&nearr; new questions &rarr; Problem Space</div>
  </div>
  <div class="vc-compare-flow-node" style="border-color: var(--vc-green); background: var(--vc-green-soft);">Fertility Evaluator</div>
  <div style="font-size: 0.7rem; color: var(--vc-text-muted); text-align: center; padding: 0.15rem 0;">&uarr; prune/promote &rarr; Concept Lattice</div>
</div>
</div>

Four components, each doing something no current system does:

### Failure Atlas

Every failed proof attempt is stored, analyzed, and classified. Not just "this didn't work" but **why** it didn't work — what structural property of the problem resisted the technique. Failures are grouped into **obstruction classes**: families of problems that resist the same approaches for the same structural reasons.

This is what human mathematicians do instinctively. When a technique fails, a good mathematician asks: "What would need to be true about this problem for this technique to work?" The gap between what is true and what would need to be true is a **structural insight**. Accumulate enough of them and you have a new concept.

### Concept Lattice

An evolving vocabulary of mathematical definitions, organized by logical dependency and structural similarity. When the Failure Atlas detects a recurring obstruction pattern, the Concept Lattice attempts to **name it** — to create a definition that captures the common structure.

This is concept formation. It is, arguably, the core creative act in mathematics. Galois did it when he invented groups. Grothendieck did it when he invented schemes. The act of creating the right definition — one that carves nature at its joints — is worth more than any number of proofs.

### Conjecture Engine

New definitions generate new questions. If you have defined a new concept $C$, the Conjecture Engine asks: What is the distribution of $C$ across known mathematical structures? What properties are implied by $C$? What existing theorems can be strengthened by adding $C$ as a hypothesis? What problems become tractable when reformulated in terms of $C$?

Conjectures are the research agenda. A system that generates conjectures is a system that does mathematics.

### Fertility Evaluator

Not all concepts are productive. The Fertility Evaluator estimates which definitions in the Concept Lattice are worth keeping. The metric is **compressive power**: a concept is fertile if it makes the description of some mathematical domain shorter.

---

## 7. The Erdos Machine

What would the Mathematical Metabolism produce on the same 700 Erdos problems? We cannot know precisely, but we can estimate the **types** of output:

<div class="vc">
<div class="vc-title">Mathematical Output: Solver vs. Metabolism on 700 Erdos Problems</div>
<div class="vc-output">
  <div class="vc-output-legend">
    <span><span class="vc-output-legend-swatch vc-output-legend-swatch--red"></span>Aletheia (Actual)</span>
    <span><span class="vc-output-legend-swatch vc-output-legend-swatch--blue"></span>Math. Metabolism (Projected)</span>
  </div>
  <div class="vc-output-row">
    <div class="vc-output-label">Solutions</div>
    <div class="vc-output-bars">
      <div class="vc-output-bar-row">
        <div class="vc-output-bar-name">Aletheia</div>
        <div class="vc-output-bar vc-output-bar--red" style="width: 8%;">4</div>
      </div>
      <div class="vc-output-bar-row">
        <div class="vc-output-bar-name">Metabolism</div>
        <div class="vc-output-bar vc-output-bar--blue" style="width: 8%;">4</div>
      </div>
    </div>
  </div>
  <div class="vc-output-row">
    <div class="vc-output-label">Obstruction Classes</div>
    <div class="vc-output-bars">
      <div class="vc-output-bar-row">
        <div class="vc-output-bar-name">Aletheia</div>
        <div class="vc-output-bar vc-output-bar--zero">0</div>
      </div>
      <div class="vc-output-bar-row">
        <div class="vc-output-bar-name">Metabolism</div>
        <div class="vc-output-bar vc-output-bar--blue" style="width: 100%;">~50</div>
      </div>
    </div>
  </div>
  <div class="vc-output-row">
    <div class="vc-output-label">New Concepts</div>
    <div class="vc-output-bars">
      <div class="vc-output-bar-row">
        <div class="vc-output-bar-name">Aletheia</div>
        <div class="vc-output-bar vc-output-bar--zero">0</div>
      </div>
      <div class="vc-output-bar-row">
        <div class="vc-output-bar-name">Metabolism</div>
        <div class="vc-output-bar vc-output-bar--blue" style="width: 20%;">~10</div>
      </div>
    </div>
  </div>
  <div class="vc-output-row">
    <div class="vc-output-label">New Conjectures</div>
    <div class="vc-output-bars">
      <div class="vc-output-bar-row">
        <div class="vc-output-bar-name">Aletheia</div>
        <div class="vc-output-bar vc-output-bar--zero">0</div>
      </div>
      <div class="vc-output-bar-row">
        <div class="vc-output-bar-name">Metabolism</div>
        <div class="vc-output-bar vc-output-bar--blue" style="width: 70%;">~35</div>
      </div>
    </div>
  </div>
  <div class="vc-output-row">
    <div class="vc-output-label">Technique Boundaries</div>
    <div class="vc-output-bars">
      <div class="vc-output-bar-row">
        <div class="vc-output-bar-name">Aletheia</div>
        <div class="vc-output-bar vc-output-bar--zero">0</div>
      </div>
      <div class="vc-output-bar-row">
        <div class="vc-output-bar-name">Metabolism</div>
        <div class="vc-output-bar vc-output-bar--blue" style="width: 50%;">~25</div>
      </div>
    </div>
  </div>
  <div class="vc-output-row">
    <div class="vc-output-label">Structural Isomorphisms</div>
    <div class="vc-output-bars">
      <div class="vc-output-bar-row">
        <div class="vc-output-bar-name">Aletheia</div>
        <div class="vc-output-bar vc-output-bar--zero">0</div>
      </div>
      <div class="vc-output-bar-row">
        <div class="vc-output-bar-name">Metabolism</div>
        <div class="vc-output-bar vc-output-bar--blue" style="width: 16%;">~8</div>
      </div>
    </div>
  </div>
</div>
</div>

The projections are conservative estimates based on the density of structural relationships in the Erdos problem corpus. The key observation: even if the Metabolism solves exactly the same 4 problems, it would additionally produce:

- **~50 obstruction classes** — families of problems that resist similar techniques, clustered by structural similarity.
- **~10 new concepts** — definitions that capture recurring structural patterns across the failure space.
- **~35 new conjectures** — questions generated by applying new concepts to known structures.
- **~25 technique boundaries** — precise characterizations of where known proof strategies break down.
- **~8 structural isomorphisms** — unexpected connections between seemingly unrelated problems.

Each of these is a **mathematical object**. Each contributes to the field. The 696 "failures" are no longer waste — they are the raw material for mathematical knowledge.

> The difference is not in what is solved. It is in what is produced by not solving.

---

## 8. The Training Problem

The Mathematical Metabolism cannot be trained on proof correctness alone. You need a training signal for **conceptual fertility** — a reward for creating useful abstractions, not just correct deductions.

Four approaches:

### Historical Replay

Train on the actual historical development of mathematical fields. Feed the system problems in the order they were posed, and reward it for reinventing the concepts that historically proved essential. If a system working on 19th-century analysis independently arrives at something resembling the $\epsilon$-$\delta$ definition of limits, that is signal.

### Downstream Solvability

A concept is fertile if problems formulated using that concept are easier to solve than problems formulated without it. This is directly measurable: define concept $C$, reformulate a problem set using $C$, and measure solve-rate improvement.

### Compression as Fertility

The core metric. A concept's fertility is its compressive power — how much shorter the description of a mathematical domain becomes when the concept is available:

$$\text{Fertility}(C) = \frac{|\text{domain description without } C| - |\text{domain description with } C|}{|C|}$$

where $|C|$ is the complexity of the concept's definition and the domain descriptions are measured in some formal language. This captures the intuition that a good definition "pays for itself" — its definitional cost is small relative to the descriptive savings it enables.

A concept with high fertility is one that carves reality at a joint. Groups, manifolds, categories — these are all high-fertility concepts. They cost little to define and simplify vast stretches of mathematics.

### Adversarial Concept Evolution

Two systems compete: one generates concepts, the other generates problems designed to make those concepts useless. The concept generator wins by creating definitions that remain useful across an expanding problem distribution. The adversary wins by finding domains where the concepts provide no compression.

This is a GAN for mathematics. The equilibrium, if it exists, would be a concept vocabulary that is robust across mathematical domains — exactly what we want.

---

## 9. The Limit

Intellectual honesty requires mapping where this architecture fails.

The Mathematical Metabolism can generate new concepts **within the space of concepts expressible in its formal language**. It can combine, recombine, and compose existing mathematical primitives in novel ways. It can discover that two seemingly different domains share hidden structure.

What it cannot do:

- **Paradigm shifts.** The invention of non-Euclidean geometry required questioning an axiom that had been unquestioned for two millennia. The Metabolism operates within its axiom system; it cannot step outside it.
- **Genuinely new mathematical language.** Category theory was not a concept within set theory — it was a new way of looking at mathematical structure itself. The Metabolism can create concepts; it cannot create new kinds of concepts.
- **Expanding the output space at runtime.** The types of mathematical objects the system can produce (definitions, conjectures, obstruction classes) are fixed at design time. A human mathematician can invent a new type of mathematical output — a new kind of thing to say about mathematics.

<div class="vc">
<div class="vc-title">Capability Frontier: What Each Architecture Can Achieve</div>
<table class="vc-matrix">
<thead>
<tr><th></th><th>Solve Problems</th><th>Generate Concepts</th><th>Create Language</th><th>Paradigm Shift</th></tr>
</thead>
<tbody>
<tr><td>Aletheia</td><td class="vc-matrix-90">90%</td><td class="vc-matrix-5">5%</td><td class="vc-matrix-0">0%</td><td class="vc-matrix-0">0%</td></tr>
<tr><td>Math Metabolism</td><td class="vc-matrix-85">85%</td><td class="vc-matrix-70">70%</td><td class="vc-matrix-15">15%</td><td class="vc-matrix-0">0%</td></tr>
<tr><td>Unknown Future</td><td class="vc-matrix-95">95%</td><td class="vc-matrix-90">90%</td><td class="vc-matrix-60">60%</td><td class="vc-matrix-10">10%</td></tr>
</tbody>
</table>
</div>

The heatmap makes the hierarchy visible. Aletheia is a powerful solver trapped in a barren generative space. The Mathematical Metabolism would dramatically expand the generative capability — but would still be bounded by its formal language and fixed output types. The truly unknown future system, one that could shift paradigms, remains beyond any architecture we know how to design.

This is not a failure of ambition. It is a precise statement of what is and is not achievable with current theoretical understanding. Knowing the boundary is itself mathematical knowledge.

---

## 10. The Metric Inversion

Everything in AI-for-mathematics is currently measured wrong. The metrics reward terminal behavior and punish generative behavior. Consider:

| Current Metric    | What It Rewards               | What It Misses                    |
| :---------------- | :---------------------------- | :-------------------------------- |
| Solve rate        | Answering posed questions     | Questions never asked             |
| Proof correctness | Logical validity              | Conceptual novelty                |
| Speed to solution | Computational efficiency      | Depth of exploration              |
| Benchmark score   | Performance on known problems | Ability to pose new problems      |
| Autonomy level    | Minimal human involvement     | Productive human-AI collaboration |

The Mathematical Metabolism demands inverted metrics:

| Generative Metric           | What It Measures                                      |
| :-------------------------- | :---------------------------------------------------- |
| Concept fertility           | Compressive power of generated definitions            |
| Conjecture quality          | Solve rate of generated questions by external solvers |
| Failure information density | Structural insights per failed proof attempt          |
| Cross-domain bridge count   | New connections discovered between unrelated areas    |
| Research program viability  | Downstream productivity of generated research agendas |

Under current metrics, the Mathematical Metabolism would score **worse** than Aletheia on every benchmark. It would solve fewer problems per unit compute, because it would spend compute on exploration, failure analysis, and concept formation instead of brute-force proof search. It would appear slower, less efficient, less capable.

And it would be doing mathematics.

> The system that would advance mathematics fastest is the one that would score worst on every existing benchmark — because it would spend its compute understanding problems rather than answering them.

---

## The Instrument We Lack

What Google has built is a powerful telescope. It can see far into the space of mathematical truth, resolving details that were previously invisible. This is genuinely valuable.

But mathematics was never about seeing far. It was about **creating the language to describe what you see.** Galileo did not advance astronomy merely by pointing a telescope at Jupiter — he advanced it by creating the conceptual vocabulary (moons as independent bodies orbiting a planet, not fixed to a crystal sphere) that made the observation meaningful.

The telescope and the cartography require fundamentally different instruments. One is a feat of optics. The other is a feat of language.

We have the telescope. The mathematical metabolism — the instrument that creates understanding — remains unbuilt.

Building it is, I believe, the central open problem in AI-for-mathematics. Not "can we solve more problems?" but "can we build a system whose failures are as productive as a mathematician's failures?" The answer to the first question is obviously yes — scale the compute, improve the verifier, expand the training data. The answer to the second question is unknown, because we do not yet understand what makes a failure productive.

That understanding — of productive failure, of fertile concepts, of the metabolism that converts confusion into clarity — would itself be a mathematical contribution of the first order. The tool that creates mathematical knowledge would, in its creation, require us to formalize what mathematical knowledge _is_.

Which is, of course, exactly the kind of problem that no existing AI system can solve. Not because it is hard. Because it requires creating something that does not yet exist.
