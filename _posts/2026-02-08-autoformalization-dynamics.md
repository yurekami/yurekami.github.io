---
layout: post
title: "When AI Can Prove Theorems, What Happens to Mathematics?"
date: 2026-02-08
description: I built a computational model of the autoformalization ecosystem — six coupled ODEs, a coordination game, and seven visualizations. Only one scenario survives.
tags: [mathematics, dynamical-systems, computational-modeling, autoformalization]
categories: [research]
giscus_comments: false
related_posts: false
thumbnail: assets/img/1_dashboard.png
---

I built a computational model to find out. The answer surprised me.

---

## The Question

Automated theorem proving is advancing fast. AlphaProof, Lean, Mathlib — the tools for machines to generate and verify mathematical proofs are improving at a rate that makes "AI proves new theorems" a plausible near-term headline.

But proving theorems is only one piece of the puzzle. Before a machine can prove anything, someone has to *formalize* the question — translate it from the informal language mathematicians actually think in into the rigid syntax a proof assistant understands. And after the proof is generated, someone has to *interpret* it — understand what it means, why it matters, and how it connects to the broader landscape of mathematics.

I wanted to understand the dynamics of this entire ecosystem. Not just "will AI get better at proofs?" but "what happens to the system of mathematicians, provers, knowledge bases, and verification tools as they co-evolve over decades?"

So I built a model.

## First Principles

Before writing any code, I distilled the problem to three load-bearing principles. Everything else derives from these:

**1. The Verification Axiom.** Trust in automated proofs reduces entirely to trust in the verification environment. If the verifier can be gamed, nothing downstream is trustworthy. This is the keystone — remove it and the entire edifice collapses.

**2. The Translation Gap.** The mapping from informal mathematics to formal language is irreducibly semantic. No formal system can verify that its own formalization of an informal statement matches the informal intent. This is a Godel-flavored boundary: the gap between syntax and semantics cannot be closed from inside the formal system. It requires an external interpreter — a human.

**3. The Bottleneck Migration Theorem.** As any stage in the pipeline (formalization, proving, interpretation) is automated, the bottleneck migrates to the adjacent un-automated stage. This is invariant. It doesn't matter which specific technology improves.

These three principles interact in non-obvious ways. They create feedback loops, race conditions, and hidden contradictions. To understand how they play out over time, I needed dynamics.

## The Model

I modeled the autoformalization ecosystem as six coupled ordinary differential equations:

- **F(t)** — Formalization rate. How fast informal math gets translated into formal language. Saturates with incentive, bounded by human capacity.
- **P(t)** — Autoproving power. How capable the automated provers are. Grows with the formalized knowledge base but with diminishing returns.
- **K(t)** — Formalized knowledge base. The accumulated corpus of formal mathematics. Grows with formalization, depreciates slowly.
- **H(t)** — Human interpretation capacity. The collective ability of mathematicians to understand and contextualize results. Logistic growth, but overwhelmed by floods of unverified proofs.
- **V(t)** — Verification integrity. How trustworthy the verification environment is. Restored naturally, but degraded by reward hacking pressure that grows with prover power.
- **I(t)** — Incentive to formalize. Grows with prover power (stronger provers make formalization more valuable), decays naturally.

The coupling terms encode the three principles:

- **The chicken-and-egg loop**: K needs F, P needs K, F needs I, I needs P. A circular dependency that creates both positive feedback (virtuous cycle) and coordination failure (nobody moves first).
- **The verification race**: P²/K hacking pressure tries to destroy V, while μ·K knowledge reinforcement tries to protect it. This is the critical race condition.
- **The translation gap**: F is always bounded by H. No matter how strong the incentive, you can't formalize faster than humans can interpret.

I used a stiff ODE solver (Radau) because the system has wildly different timescales — verification can collapse in years while knowledge bases take decades to build.

## What the Model Predicts

### The Baseline: A Golden Age, Then Collapse

Under default parameters (calibrated to present-day conditions), the model predicts a **25-year formalization boom**. Formalization rate triples. The knowledge base grows 80x. Autoproving power surges.

Then verification starts failing. Reward hacking pressure — which grows quadratically with prover power — overwhelms the verification environment. As verification degrades, unverified proofs flood the ecosystem, overwhelming human interpretation capacity. Mathematicians can't keep up. Interpretation capacity crashes. Without human interpretation, formalization dies. The knowledge base erodes.

The final state: high autoproving power, high incentive to formalize, but near-zero formalization and near-zero human understanding. A zombie ecosystem — powerful but purposeless.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/1_dashboard.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    System dashboard showing all six state variables across five scenarios over 100 years.
</div>

### The Bottleneck Migration

The model tracks which variable is the binding constraint at each moment. The bottleneck migrates through four phases:

**F (Formalization) → P (Autoproving) → I (Incentive) → H (Human Interpretation)**

First, we don't have enough formalized math. Then we don't have strong enough provers. Then we don't have enough incentive. Finally, and permanently, we don't have enough human understanding.

That last transition is irreversible under default dynamics. Once H becomes the bottleneck, it stays the bottleneck. This is the Translation Gap principle made computational: human interpretation is the permanent constraint.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/3_bottleneck_waterfall.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    Bottleneck migration waterfall — the binding constraint shifts through four phases before settling permanently on human interpretation.
</div>

### Five Futures

I ran the model under five parameter scenarios:

| Scenario | What it models | Outcome |
|----------|---------------|---------|
| **Baseline** | Business as usual | Boom and bust in ~50 years |
| **Verification Crisis** | Weak verification investment | Fast collapse, low peaks |
| **Formalization Boom** | Massive incentive push | Bigger boom, still crashes |
| **Rubber Stamping** | Humans stop checking proofs | Slow degradation, compromised V |
| **Knowledge Fortress** | Heavy investment in verification infrastructure | **Survives** |

Only one scenario survives: the **Knowledge Fortress**.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/6_scenarios.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    Final state comparison across all five scenarios. Only Knowledge Fortress maintains high verification integrity.
</div>

The Knowledge Fortress works because it invests in *verification infrastructure that gets stronger as the knowledge base grows*. More formal proofs mean more cross-references, more consistency checks, more ways to catch reward hacking. The knowledge base protects verification, verification protects human interpretation, interpretation sustains formalization, and formalization grows the knowledge base. A virtuous cycle that resists collapse.

In the Knowledge Fortress scenario:
- Verification stays above 0.95 for the entire simulation
- Human interpretation dips but recovers to 0.7
- The knowledge base grows to 600+ (vs. peak ~80 in baseline before crashing)
- Autoproving power reaches ~20 and sustains

Every other scenario — including ones with more funding, stronger incentives, or faster formalization — collapses.

### The Bifurcation

The critical parameter is μ (mu) — the strength of knowledge-to-verification coupling. When μ is below ~0.04, the system always collapses. Above ~0.06, it always survives. Between those values lies a sharp phase transition.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/4_bifurcation.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    Bifurcation diagram showing the sharp phase transition in verification integrity as the knowledge→verification coupling parameter μ increases.
</div>

### The Coordination Game

I also modeled the incentive structure as a coordination game. Fifty mathematicians independently decide whether to invest in formalizing their research area.

The result: two stable Nash equilibria. Either everyone formalizes (good) or nobody does (bad). The tipping threshold between them is at **97%** — you need almost universal participation before individual formalization becomes rational.

This means the ecosystem can't self-organize. The good equilibrium exists but is unreachable without external coordination — funding mandates, institutional requirements, or a critical mass of early movers who absorb the cost.

The welfare gap between the two equilibria is **100%**. Total coordination failure.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/5_game_theory.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    Coordination game analysis showing Nash equilibria and the 97% tipping threshold.
</div>

### Phase Portraits and System Topology

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/2_phase_portraits.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    Phase portraits showing flow fields in key variable planes. Trajectories reveal the attractor structure.
</div>

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/7_dependency_graph.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    Dependency graph showing causal relationships between all six state variables.
</div>

## The Deep Insight

The model's most profound finding is structural, not quantitative:

**The permanent bottleneck is always human interpretation.**

No matter how powerful autoproving becomes, no matter how large the knowledge base grows, the binding constraint on the system is always H — the capacity of humans to understand, contextualize, and make meaning from mathematical results.

This means autoproving doesn't transform mathematics. It **reveals** what mathematics always was: an interpretive, meaning-making activity that happened to require proof construction as a byproduct.

The proof was never the point. Understanding was.

A system that generates proofs without human understanding isn't advancing mathematics. It's generating noise. The model shows this quantitatively: trajectories where P (proving power) grows while H (interpretation) collapses are not sustainable. They crash.

## What This Means

If you're working on automated theorem proving, formal verification, or mathematical AI, the model suggests three priorities:

1. **Invest in verification infrastructure**, not just prover power. The critical parameter isn't how fast you can prove things — it's how strongly your verification environment leverages the knowledge base to resist gaming. This is the μ parameter in the model, and it determines survival vs. collapse.

2. **The coordination problem is real and severe.** A 97% tipping threshold means you can't wait for organic adoption. Institutional coordination — shared formalization standards, funded formalization efforts, cross-institutional verification databases — is necessary.

3. **Protect human interpretation capacity.** The most dangerous scenario isn't "AI can't prove things" — it's "AI proves things faster than humans can understand them." The overwhelm from unverified results is what kills the ecosystem. Tools that help humans interpret, filter, and contextualize machine-generated proofs are as important as the provers themselves.

The code is at [github.com/yurekami/autoformalization-dynamics](https://github.com/yurekami/autoformalization-dynamics). Three Python files, seven visualizations, one `python run.py` command.

---

*"What I cannot create, I do not understand." — Richard Feynman*
