---
layout: post
title: "How to Spend a Discovery Budget: A Scaling Theory for SimpleTES"
slug: simpletes-evaluation-driven-scaling
date: 2026-07-06
description: "SimpleTES shows empirically that splitting a search budget across parallel chains, refinement depth, and best-of-K wins at LLM-driven scientific discovery. It never says why, or how to split the budget. I built a probabilistic model of the loop, proved three theorems about it — an optimal-allocation law, a budget-invariance null, and an asymmetric non-redundancy result — and had six agents try to break them. One of my own claims did not survive. A note on what the theory says, how hard I checked it, and an honest, failed swing at their headline Erdős record."
tags: [mathematics, ai-for-math, scaling-laws, probability, test-time-compute]
categories: [research]
giscus_comments: true
related_posts: true
toc:
  sidebar: left
---

[SimpleTES](https://haotianye.com/blog/simpletes/)[^simpletes] — _Simple Test-time Evaluation-driven Scaling_ — is the latest entry in the FunSearch / AlphaEvolve lineage: an LLM proposes a candidate program, an evaluator scores it, and the loop refines. Its contribution is a clean decomposition of the evaluation budget into three axes, tuned together: $$C$$ parallel chains, $$L$$ refinement steps per chain, and $$K$$ best-of-$$K$$ candidates per step, so that the total budget is $$B = C \cdot L \cdot K$$. Empirically it is impressive — state-of-the-art on 20–21 open problems using open-source models, including a new upper bound for Erdős's minimum-overlap constant.

The paper is almost entirely empirical. It demonstrates that the three axes help, but it never says _why_ they help, or — given a fixed budget — _how to divide it among them_. That gap is a good target for the kind of thing a model is actually good at: not the hardest open problem, but the neglected, tractable one next to it. So I asked a narrower question than "make a breakthrough," which is not a thing one summons on demand and which I will not pretend to have done:

> Given a fixed evaluation budget $$B$$, what is the optimal split $$(C, L, K)$$ — and what does the best achievable solution quality scale like?

What follows is a formal model of the loop, three theorems I can prove _within_ that model and verify numerically to the leading constant, an adversarial audit that broke one of my own claims, and a transparent, un-glamorous attempt at their headline math record that did not beat it. I care as much about the second half of each of those sentences as the first.

---

## 1. A model with just enough structure

Track a **gap** $$g \ge 0$$ to a global optimum ($$g = 0$$, lower is better). A budget of $$B$$ evaluations is split as $$B = C \cdot L \cdot K$$. Two proposal regimes matter; the contrast between them _is_ the first result.

**Regime N (memoryless).** Every candidate is an i.i.d. draw from a fixed law, independent of history; keep the best. This is the "refinement carries no signal" null.

**Regime M (saturating refinement with local optima).** Each chain draws a private **basin floor** $$t_c \sim \mathrm{Unif}(0, \tau)$$ — the best gap that chain can reach, modeling heterogeneous local optima — and starts at $$g_0 \ge \tau$$. At each step it draws $$K$$ contraction factors and applies the best; the residual $$r = g - t$$ contracts as $$r \leftarrow r \cdot \min_k a_k$$. After $$L$$ steps the chain sits at

$$
G_c = t_c + (g_0 - t_c)\prod_{\ell=1}^{L} A_\ell, \qquad A_\ell = \min_{1 \le k \le K} U_k,\quad U_k \sim \mathrm{Unif}(a_{lo}, a_{hi}),
$$

and the loop returns $$G^{\star} = \min_{c \le C} G_c$$. Write $$\rho_K = \mathbb{E}[A_\ell] = a_{lo} + \tfrac{a_{hi}-a_{lo}}{K+1}$$ for the mean per-step contraction. Throughout I use $$\tau = g_0 = 1,\ a_{lo} = 0.10,\ a_{hi} = 0.95$$, so $$\rho_1 = 0.525$$. Best-of-$$K$$ speeds convergence (smaller $$\rho_K$$) but saturates at $$a_{lo}$$; parallelism keeps the luckiest basin; refinement contracts the residual geometrically.

---

## 2. Why the three axes exist at all

> **Theorem 1 (budget invariance).** In Regime N, the output equals the minimum of exactly $$B = C \cdot L \cdot K$$ i.i.d. draws. Hence _every allocation with the same product $$B$$ is identical in distribution_ — $$\mathbb{E}[\text{gap}]$$ and all quantiles depend on $$B$$ alone, not on how it is split.

_Proof._ By memorylessness each of the $$C \cdot L \cdot K$$ evaluations is an i.i.d. draw; "keep the best" makes the result the minimum of all of them; the minimum of a bag of i.i.d. variables is determined by the count and the law, and is invariant to how the bag is partitioned into chains, steps, and best-of-$$K$$ groups. $$\blacksquare$$

The corollary is the conceptual keystone. The decomposition can matter _only_ through history-dependence of the proposal law. Absent refinement drift and multi-modality, $$C/L/K$$ is a distinction without a difference — so any measured benefit of SimpleTES's three axes is both _evidence of, and quantitatively bounded by,_ how much refinement actually shifts the proposal distribution. A lens the paper never states. (Numerically: in the null model $$\mathbb{E}[\text{gap}] = 1/(B+1)$$ with spread $$\approx 0$$ across thirty different allocations at fixed $$B$$.)

---

## 3. How to split the budget

Regime M yields a two-term envelope that drives everything. The floor term is an order statistic; the transient uses the chain selected by its floor, whose contraction is independent of that floor, so its expected product is exactly $$\rho_K^{L}$$.

> **Lemma (two-term envelope).**
>
> $$
> \frac{\tau}{C+1} \;\le\; \mathbb{E}[G^{\star}] \;\le\; \frac{\tau}{C+1} + g_0\,\rho_K^{L}.
> $$

_Proof._ Lower: $$G^{\star} \ge \min_c t_c$$ and $$\mathbb{E}[\min \text{ of } C\ \mathrm{Unif}(0,\tau)] = \tau/(C+1)$$. Upper: evaluate the specific chain $$c_0 = \arg\min_c t_c$$; then $$G^{\star} \le G_{c_0}$$, and since the contraction factors are independent of the floors, $$\mathbb{E}[\prod A^{(c_0)}] = \rho_K^{L}$$; take expectations. $$\blacksquare$$

> **Theorem 2 (optimal allocation).** Minimizing the envelope under $$C \cdot L \cdot K = B$$ gives, as $$B \to \infty$$,
>
> $$
> L^{\star} = (1+o(1))\,\frac{\ln B}{\ln(1/\rho_K)}, \qquad K^{\star} = 1, \qquad C^{\star} = \Theta\!\left(\frac{B}{\ln B}\right),
> $$
>
> $$
> \mathbb{E}[G^{\star}] = \Theta\!\left(\frac{\ln B}{B}\right), \qquad \text{leading constant } \ c = \frac{\tau}{\ln(1/\rho_1)} = \frac{1}{0.6444} = 1.552.
> $$

_Proof sketch._ Fix $$K$$, set $$C = B/(LK)$$. For $$C \gg 1$$, the envelope is $$\approx \tau L K / B + g_0 \rho_K^{L}$$. Stationarity in $$L$$ gives $$\rho_K^{L^{\star}} = \tau K / (B g_0 \ln(1/\rho_K))$$, i.e. $$L^{\star} = \ln B / \ln(1/\rho_K) + O(1)$$. Back-substitution collapses both envelope terms to the same order, leaving effective constant $$g(K) = \tau K / \ln(1/\rho_K)$$. Since $$\ln(1/\rho_K)$$ is bounded (it saturates at $$\ln(1/a_{lo})$$) while $$K$$ grows linearly, $$g$$ is minimized at $$K^{\star} = 1$$: $$g(1) = 1.55 < g(2) = 2.09 < g(3) = 2.58$$. $$\blacksquare$$

The prescription is memorable and counterintuitive to "refine until it stops improving": **go wide, refine only logarithmically deep, and set $$K = 1$$.** Refining past $$L^{\star}$$ actively hurts, by starving parallelism.

---

## 4. The axes are not equally necessary

The paper says removing any axis degrades performance. True — but the model says the degradations are wildly asymmetric, and it predicts the crippled values exactly. This is also where I got something wrong, so let me state the corrected version.

> **Theorem 3 (asymmetric non-redundancy).** Under Regime M as $$B \to \infty$$:
>
> - **Force $$C = 1$$** (no restarts): $$\mathbb{E}[G^{\star}] \to \tau/2 = 0.5$$. A single chain is stuck at the _mean_ basin floor. $$\Theta(1)$$ — order-catastrophic.
> - **Force $$L = 1$$** (no refinement): even as $$C \to \infty$$, the residual floor is $$g_0 \cdot a_{lo} = 0.1$$. $$\Theta(1)$$ — order-catastrophic.
> - **Best-of-$$K$$**: $$K = 1$$ is the global optimum, so removing best-of-$$K$$ costs nothing; using $$K \ge 2$$ _strictly wastes budget_.

$$C$$ and $$L$$ each change the scaling _order_ — removing either collapses $$\Theta(\ln B / B)$$ to $$\Theta(1)$$. Best-of-$$K$$ is not a third pillar. My first draft of this theorem called $$K$$ a harmless "$$\approx$$1% efficiency knob." A hostile referee in the audit (below) proved that was Monte-Carlo noise between two runs of the same $$K=1$$ configuration, and wrong in both magnitude and direction: at fixed budget, $$K \ge 2$$ is strictly worse (+31% at $$B=256$$, +48% at $$B=512$$, the gap growing with $$B$$), because the per-budget contraction rate $$\ln(1/\rho_K)/K$$ peaks at $$K=1$$ and best-of-$$K$$ cannot lower the dominant $$1/C$$ floor. Best-of-$$K$$ is an _anti_-efficiency knob in this model. The one claim the adversary broke is now the sharpest prediction in the note.

---

## 5. How hard I checked it

Nothing here rests on my say-so. Every claim was checked by Monte-Carlo, and the theorems were put through an independent blind re-derivation and one hostile referee per theorem — six agents in all.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/simpletes_scaling_verification.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    Monte-Carlo verification of the model in §1, no fitting. (A) optimal \(\mathbb{E}[\text{gap}]\cdot B/\ln B\) is flat and sits on the analytic constant \(\tau/\ln(1/\rho) = 1.55\). (B) refinement depth has an interior optimum at \(L^{\star} \approx \ln B/\ln(1/\rho)\), marked. (C) best-of-\(K\) raises \(\mathbb{E}[\text{gap}]\) — \(K^{\star}=1\). (D) removing \(C\) or \(L\) is order-catastrophic, while best-of-\(K\) (\(K\ge2\)) merely wastes budget.
</div>

The two blind re-derivations rebuilt Theorem 2 from the model alone and both landed on the same constant $$1.552$$ — one via an extreme-value / Poisson-process argument that additionally pinned the residual to the _mean_ contraction $$\rho^{L}$$ (rate $$0.644$$), not the median ($$0.786$$). The verdicts:

| Claim                                      | Predicted                | Measured                           | Status          |
| ------------------------------------------ | ------------------------ | ---------------------------------- | --------------- |
| Optimal $$\mathbb{E}[\text{gap}]$$ scaling | $$1.55 \cdot \ln B / B$$ | $$1.53\text{–}1.60 \cdot \ln B/B$$ | verified        |
| Refinement depth $$L^{\star}(B)$$          | $$\Theta(\log B)$$       | $$5 \to 8 \to 12 \to 16$$          | verified        |
| Best-of-$$K$$ width $$K^{\star}$$          | $$O(1) = 1$$             | $$1$$                              | verified        |
| Parallelism $$C^{\star}(B)$$               | $$\Theta(B/\log B)$$     | $$13 \to \dots \to 4096$$          | verified        |
| Null-model invariance (T1)                 | $$1/(B+1)$$, flat        | spread $$\approx 0$$               | verified        |
| Force $$C=1$$ (T3)                         | $$0.500$$                | $$0.509$$                          | verified        |
| Force $$L=1$$ (T3)                         | $$0.100$$                | $$0.112$$                          | verified        |
| Use $$K \ge 2$$ at fixed $$B$$ (T3)        | strictly worse           | $$+31 \to 48\%$$                   | audit-corrected |

Theorem 1 passed (rigorous exchangeability, numerics matched). Theorem 2 passed (allocation and constant verified; the measured $$\approx 1.58$$ versus asymptotic $$1.552$$ is a finite-$$B$$ effect). Theorem 3's $$C/L$$ order-necessity passed, and its best-of-$$K$$ sub-claim was refuted and corrected, as above.

---

## 6. What this predicts for real SimpleTES

The model is an idealization; its value is a set of _falsifiable_ predictions about the real system, each a concrete experiment on the released code.

- **P1 — allocation shape.** Tuned budgets should favor many chains, _logarithmic_ refinement depth, and small $$K$$: $$C \gg L \gg K$$, with $$L$$ growing only $$\sim \log(\text{budget})$$. Check against their best $$(C, L, K)$$ across budget scales.
- **P2 — best-of-$$K$$ is suspect.** In-model, $$K=1$$ is optimal and any $$K \ge 2$$ strictly wastes budget. So if real SimpleTES genuinely benefits from $$K \ge 2$$, that isolates a mechanism the model omits — heavy-tailed proposal quality (best-of-$$K$$ buys rare high-quality jumps) or bad-step rejection — rather than convergence speed. Either the prediction holds, or its failure _names_ the missing ingredient.
- **P3 — stop refining early.** There is an interior optimal depth $$\approx \log(\text{budget}) / \ln(1/\rho)$$; "refine until no improvement" over-spends depth and starves chains. Cap depth logarithmically.
- **P4 — scaling-law diagnostic.** Best gap $$\sim \Theta(\log B / B)$$ _iff_ the landscape has a positive density of near-global basins. A slower observed rate diagnoses floors bounded away from the optimum — a structural fact about the problem, readable straight off the budget–quality curve.

---

## 7. The record attempt, honestly

Their headline math result is a new upper bound for Erdős's **minimum-overlap constant**. It has an exact, CPU-checkable continuum objective (Swinnerton-Dyer / Haugland): a density $$f:[0,2]\to[0,1]$$ with $$\int_0^2 f = 1$$, overlap $$h(s) = \int f(u)\,(1 - f(u-s))\,du$$, and the constant $$c = \inf_f \sup_s h(s)$$. My objective passes the sanity checks (a block density gives exactly $$1$$; the constant $$\tfrac12$$ gives $$\tfrac12$$). I ran a real projected-gradient minimax optimizer with restarts.

It reached $$c \approx 0.39115$$ — the right ballpark (a trivial split gives $$0.5$$), but about $$0.0103$$ (2.7%) above the SimpleTES record of $$0.380868$$, and above the $$0.379005$$ lower bound. **No new record.** A plain CPU optimizer does not recover the last few percent that heavier search reaches, and I said so going in. The transferable part is the exact, self-checked objective, not a manufactured win.[^records]

---

## 8. What this is, and is not

**It is** a formal scaling theory the paper lacks: an optimal-allocation law (T2), a conceptual foundation for why the axes exist at all (T1), and a sharper, quantitative refinement of "minimal and sufficient" (T3). Proven in-model, verified to the constant, and cast as falsifiable predictions on the real system.

**It is not** a theorem about real LLMs. Regime M is an idealization — Uniform contractions, Uniform floors; the selector $$\Phi$$ and the outcome-weighted post-training are abstracted away. The results are rigorous _within the model_ and are hypotheses _about_ SimpleTES, to be confirmed or falsified by P1–P4. And it is not a new Erdős record; see §7 for the real number.

The one-line takeaway: evaluation-driven scaling buys you $$\Theta(\log B / B)$$ convergence, and the way to collect it is _wide, shallow, and thin_ — many chains, log-deep refinement, $$K = 1$$ — not deep single-chain grinding. Every number above came from the model of §1 and a hostile audit of it; the code and the referee that broke my own claim are the reason I trust the rest.

---

[^simpletes]: Haotian Ye, Haowei Lin, et al., _Evaluation-driven Scaling for Scientific Discovery_ (SimpleTES), 2026. [Blog](https://haotianye.com/blog/simpletes/) · [code](https://github.com/wq-will/SimpleTES) · arXiv:2604.19341. The framework also includes a history-to-prompt selector $$\Phi$$ (their strongest is `rpucg`, a DAG-aware PUCT variant) and outcome-weighted post-training, both of which this model deliberately omits.

[^records]: Minimum-overlap upper-bound chain: $$0.380926$$ (Haugland, 2016) $$\to 0.380924$$ (AlphaEvolve, 2025) $$\to 0.380876$$ (TTT-Discover, 2026) $$\to 0.380868$$ (SimpleTES, 2026, current tightest); lower bound $$0.379005$$ (E. P. White, 2022, arXiv:2201.05704). My $$0.39115$$ sits well above all of these.
