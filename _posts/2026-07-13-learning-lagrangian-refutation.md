---
layout: post
title: "The Learning Lagrangian Is Vacuous: Refuting a Paper, Then Getting Refuted Myself"
slug: learning-lagrangian-refutation
date: 2026-07-13
description: "A recent preprint claims Adam is the least-action trajectory of a 'learning Lagrangian.' I set out to build on it and ended up with a three-part machine-checked refutation: the Euler–Lagrange equation is a Bartlett-identity tautology, the Lagrangian is degenerate and static, and a 615-run GPU sweep shows the exponent it 'derives' is not a constant. Then an adversarial literature sweep dismantled my replacement theory just as thoroughly. A post about both halves — and why the second half is the part worth writing down."
tags: [machine-learning, optimization, adam, natural-gradient, variational-principles, ai-for-research]
categories: [research]
giscus_comments: true
related_posts: true
toc:
  sidebar: left
---

There is a preprint by Guo and Schölkopf, _Physics of Learning: A Lagrangian perspective to different learning paradigms_,[^paper] with an irresistible thesis: learning algorithms are stationary paths of a variational principle. Fermat's principle gives optimal experimental design, a Hamiltonian gives Bellman's equation, and — the flagship claim — a postulated "learning Lagrangian" yields Adam. Not "Adam is sort of like a physical system": _derived from first principles_, as the abstract puts it.

[^paper]: Siyuan Guo and Bernhard Schölkopf, "Physics of Learning: A Lagrangian perspective to different learning paradigms," [arXiv:2509.21049](https://arxiv.org/abs/2509.21049), September 2025. Marked "work in progress." The paper was submitted to ICLR 2026 and rejected — verifiable from the `Rejected_Submission` venue id on its [OpenReview forum](https://openreview.net/forum?id=FUEzlNM4jx). The review texts are not exposed through the public API, so I can't quote them; the rejection itself is a matter of record.

I went in trying to make a breakthrough _on top of_ this paper. What I got instead was a dissection in two acts. In the first act, the paper's central derivation fell apart three separate ways, two of them machine-checked symbolically and one of them falsified with about $80 of rented B200 time. In the second act — the one I actually want to talk about — an adversarial literature sweep I ran against _my own_ replacement theory found that nearly all of it was already published, and that my single best idea is refuted by a theorem from the 1830s.

The paper, it turns out, was already rejected from ICLR 2026 before I started. So the first act is a post-mortem, not a takedown of a live claim. But the anatomy of the failure is instructive, the falsification experiment is the one the paper itself called for and never ran, and the second act is a case study in why you should always pay someone (or something) to try to scoop you before you claim anything.

## The claim

The paper treats the loss $$\ell(\theta, t)$$ as a scalar _field_ over parameter space and postulates the Lagrangian density

$$
\mathcal{L}(\ell, \nabla_\theta \ell) \;=\; T - V \;=\; \frac{1}{2P}\,(\nabla_\theta \ell)^\top F(\theta)^{-1}\,(\nabla_\theta \ell)\;-\;\ell(\theta; x),
$$

with $$F$$ the Fisher information and $$P$$ the parameter count, under the action $$S = \int dt \int d\theta \int p(x)\,dx\; \mathcal{L}$$. Two conclusions are drawn:

1. The Euler–Lagrange equation of this action forces stationary solutions to be the **maximum-likelihood estimator**.
2. The stationary path is $$\dot\theta = F^{-1/2}\nabla_\theta \ell$$ — "which Adam approximates with diagonalized Fisher."

Table 1 of the paper marks this Lagrangian as novel: "to the best of our knowledge, no prior published work exists as of September 2025." Its conclusion says the natural next step is to design verifiable experiments. Both sentences will come back to haunt it.

## Kill one: the Euler–Lagrange equation is a tautology

The paper computes $$\partial\mathcal{L}/\partial\ell = -1$$, notes there is no $$\dot\ell$$ term, and arrives at

$$
-1 \;=\; \frac{1}{P}\,\mathbb{E}\!\left[\nabla_\theta \cdot \big(F^{-1}\nabla_\theta \ell\big)\right].
$$

It then argues this holds _because_ $$\mathbb{E}[\nabla\ell] = 0$$ and $$\mathbb{E}[\nabla^2\ell] = -F$$, so the right side is $$\tfrac{1}{P}\mathrm{tr}(F^{-1}(-F)) = -1$$, and concludes the stationary point "needs to be a maximum likelihood estimator."

But those two facts are **Bartlett's first and second identities**. Under the model's own distribution $$p_\theta$$ they hold at _every_ $$\theta$$, not just at the MLE:

$$
\mathbb{E}_{p_\theta}[\nabla_\theta \ell] = \nabla_\theta \!\int\! p_\theta\,dx = \nabla_\theta(1) = 0
\qquad\text{and}\qquad
\mathbb{E}_{p_\theta}[\nabla^2_\theta \ell] = -F(\theta),
$$

for all $$\theta$$, under the usual regularity conditions. So the Euler–Lagrange equation reduces to $$-1 = -1$$ **identically on the whole parameter space**. It is an identity, not an equation. It constrains nothing, selects no point, and certainly does not single out the MLE. The paper's own annotation — an underbrace reading "$$=0$$ at stationary points" — is the tell: the score has zero mean _everywhere_ under $$p_\theta$$; that is precisely why the equation carries no information.

I verified this symbolically with sympy in three exponential families — $$N(\mu, 1)$$, $$N(m, \sigma^2)$$ with both parameters free (so the $$\nabla F^{-1}$$ term is genuinely non-trivial), and $$\mathrm{Exponential}(\lambda)$$ — and an independent auditing agent reproduced it with its own script, adding the curved model $$N(\theta, \theta^2)$$. In every case the paper's E-L quantity evaluates to $$-1$$ at every point of the parameter space, MLE or not.

There is one interesting scrap in the wreckage: under the _data_ distribution $$p \ne p_\theta$$, the identity breaks and the E-L condition becomes $$\mathrm{tr}(F^{-1}H) = P$$ — which is White's information-matrix misspecification test. So the postulated Lagrangian's stationarity condition is, at best, a specification diagnostic. It is not an optimizer.

## Kill two: the Lagrangian is degenerate and static

The paper states, correctly, that $$\mathcal{L}$$ contains no $$\dot\ell$$. It does not notice the price. With no time derivative, the conjugate momentum vanishes identically — $$\pi := \partial\mathcal{L}/\partial\dot\ell \equiv 0$$, a Dirac primary constraint — so there is no Legendre transform and no field Hamiltonian dynamics. The Euler–Lagrange equation,

$$
\nabla_\theta \cdot \big(F(\theta)^{-1}\nabla_\theta \ell\big) = -P,
$$

is a second-order **elliptic PDE in $$\theta$$ with no time in it at all**. Its solution set factorizes over time slices: any assignment $$t \mapsto \ell_t$$ where each slice solves the same static constraint is a solution, with completely arbitrary time dependence. Concretely, with $$F = I$$ it is solved by $$\ell(\theta, t) = -\tfrac12\lvert\theta - c(t)\rvert^2 + h(\theta, t)$$ for _any_ curve $$c(t)$$ and any $$\theta$$-harmonic $$h$$. The "dynamics" is pure gauge. A static elliptic constraint cannot generate, select, or even constrain a trajectory $$\theta(t)$$ — so nothing about Adam, or any optimizer, follows from this action.

Where does the $$F^{-1/2}$$ actually come from, then? From a separate step the paper calls "operationalizing": it hand-matches the field kinetic term against a particle kinetic energy, $$\tfrac12 m\,\dot\theta^\top\dot\theta \equiv \tfrac{1}{2P}(\nabla\ell)^\top F^{-1}(\nabla\ell)$$ with $$m = 1/P$$, and reads off $$\dot\theta = F^{-1/2}\nabla\ell$$. That is a definition, not a derivation — and not even a well-posed one, since matching a quadratic form fixes $$\dot\theta$$ only up to an arbitrary orthogonal rotation: $$\dot\theta = R\,F^{-1/2}\nabla\ell$$ has identical kinetic energy for every $$R$$ with $$R^\top R = I$$. The step is underdetermined by $$O(P)$$ degrees of freedom, and the particular representative that happens to be Adam is chosen by hand. The Lagrangian is reverse-engineered from the answer.

There is also a structural reason this was never going to work: Hamilton's principle with $$L = T - V$$ generates second-order, conservative, time-reversible dynamics. SGD, Adam, RMSProp, and natural gradient are first-order, dissipative, irreversible flows. No non-degenerate $$L = T - V$$ produces a first-order gradient flow — this is the classical inverse problem of the calculus of variations (Helmholtz 1887, Douglas 1941). The variational home of such flows is Onsager's principle: stationarity of the Rayleighian $$\mathcal{R}[\dot\theta] = \tfrac12\dot\theta^\top M \dot\theta + \nabla V\cdot\dot\theta$$ gives $$\dot\theta = -M^{-1}\nabla V$$ by construction.

## Kill three: the paper's own premise selects the wrong exponent

The paper motivates the $$F^{-1}$$ in its kinetic term via the Cramér–Rao bound ("we are looking for an efficient statistical estimator") and frames efficient learning as **least time** — fewest observations to a target error. But steepest descent in the Fisher–Rao metric, the flow that is Fisher-efficient and asymptotically attains Cramér–Rao, is Amari's **natural gradient**: $$\dot\theta = F^{-1}\nabla\ell$$. Exponent one.[^amari] Setting the least-time problem up directly — maximize $$g \cdot d$$ subject to the Fisher speed limit $$d^\top F d \le 1$$ — and solving it numerically gives $$\cos(d^\ast, F^{-1}g) = 1.000000$$. The paper's stated principle derives $$p = 1$$; the paper announces $$p = 1/2$$. The premise and the conclusion disagree with each other, and the mismatch is papered over by exactly the hand-matching step from kill two.

[^amari]: Shun-ichi Amari, "Natural Gradient Works Efficiently in Learning," _Neural Computation_ 10(2), 1998. The tension between the exponent-1 answer that efficiency arguments give and the exponent-½ that Adam uses is not a new observation either — it is the explicit subject of Lin et al., "Can We Remove the Square-Root in Adaptive Gradient Methods?" (ICML 2024, [arXiv:2402.03496](https://arxiv.org/abs/2402.03496)) and of FAdam ([arXiv:2405.12807](https://arxiv.org/abs/2405.12807)).

## The experiment the paper asked for and never ran

A fixed Lagrangian has one stationary path, hence one preconditioner exponent. The paper is therefore committed to a falsifiable prediction: $$p = 1/2$$, universally. Its conclusion explicitly calls for verifiable experiments. So I ran the obvious one.

Embed Adam in the one-parameter family $$P = F^{-p}$$ — realized as Adam with a free exponent, $$\theta \leftarrow \theta - \alpha\,\hat m / (\hat v^{\,p} + \epsilon)$$, so that $$p = 1/2$$ _is_ Adam exactly, $$p = 0$$ is momentum-SGD, and $$p = 1$$ is a diagonal natural-gradient-like method — and measure the optimal exponent $$p^\ast$$ as conditions vary. The fairness trap is the learning rate: the update magnitude scales like $$g^{1-2p}$$, so a shared LR grid across $$p$$ rigs the comparison. I parametrized the grid by target RMS update size (a trust region), which is commensurate across exponents, tuned it per cell over nine points, and checked that winners sat in the grid interior.

On a 6-layer GPT (d=384, 256-token context, GPT-2 tokenizer, 158M tokens of FineWeb-Edu), 615 runs across 8×B200,[^infra] sweeping $$\beta_2$$ — the horizon of the second-moment EMA, i.e. **the quality of the curvature estimate** — at fixed batch 64:

[^infra]: Rented for a few hours from a GPU marketplace; roughly $80 total, including my mistakes. The synthetic sweeps, the sympy checks, the sweep harness, and all 615 run records are archived; the whole falsification is reproducible on one node in an afternoon.

| $$\beta_2$$        | p=0   | p=0.25 | **p=0.5 (Adam)** | p=0.75    | p=1.0 | $$p^\ast$$ | Adam − best |
| ------------------ | ----- | ------ | ---------------- | --------- | ----- | ---------- | ----------- |
| 0.9 (noisiest v̂)   | 8.922 | 6.752  | **5.891**        | 6.487     | 8.069 | **0.50**   | +0.000      |
| 0.99               | 8.922 | 6.843  | 6.437            | **6.197** | 46.46 | **0.75**   | **+0.240**  |
| 0.999 (cleanest v̂) | 8.922 | 7.025  | 6.529            | **6.188** | 61.76 | **0.75**   | **+0.341**  |

(Validation loss at an equal observation budget, median of two seeds.) As the curvature estimate gets cleaner, the optimal exponent lifts off $$1/2$$ and Adam's deficit against the best exponent grows monotonically: 0.000 → 0.240 → 0.341 nats. Adam's exponent is optimal in exactly one cell — the noisiest one. The controlled synthetic version (quadratic MLE with $$H = F$$ and gradient-noise covariance $$F/B$$, sweeping the relative error of the Fisher estimate) shows the same thing across three Fisher spectra, with $$p^\ast$$ sweeping the whole range from ≈0.9 down to 0 as the estimate degrades.

An exponent that moves with the noise level cannot be the output of any fixed Lagrangian. That is the falsification, on the paper's own stated terms.

Two honest caveats, because a falsification post that hides its own weak cells would be committing the sin it describes. First, my second arm — sweeping batch size at fixed $$\beta_2$$ under an equal _token_ budget — came out non-monotone ($$p^\ast$$ = 0.5, 0.5, 0.75, 0.5), and I think the design was confounded: equal tokens means the batch-512 runs got only 375 optimizer steps and were barely trained, which independently pushes $$p^\ast$$ back down. That arm is not evidence for either side, and it is my error. Second, the ugly blow-ups at $$p = 1.0$$ in the table (losses of 46–96) are partly an artifact: I gave the natural-gradient endpoint essentially no damping ($$\epsilon = 10^{-12}$$), and undamped inverse-curvature methods are known to need it. The $$p=1$$ column should not be read as evidence against natural gradient.

## Act two: the literature refutes me

At this point I had what looked like a tidy positive theory to put in the rubble's place: $$F^{-1/2}$$ is the unique symmetric-PSD preconditioner that _whitens_ the gradient noise (one line of algebra: $$PFP = cI \Rightarrow P = \sqrt{c}\,F^{-1/2}$$); the stationary excess risk in the quadratic model obeys $$R(p) = \tfrac{\alpha}{4B}\mathrm{tr}(F^{1-p})$$, which my simulations matched to under a percent; and the optimal exponent is set by a tradeoff between conditioning (wants $$p \to 1$$) and noise amplification through an estimated preconditioner (wants $$p \to 0$$). I even had a romantic hypothesis for _why_ a square root: Fermat's least-time functional is degree-1 homogeneous in $$\dot\theta$$ where Hamilton's action is degree-2, and the Fisher–Rao line element $$ds = \sqrt{d\theta^\top F\, d\theta}$$ has a square root sitting right in it.

Before writing any of that down as mine, I ran a five-angle adversarial literature sweep with one instruction: _treat missing prior work as the worst possible failure._ It was a massacre.

- **The whitening theorem is published.** Staib, Reddi, Kale, Kumar and Sra (ICML 2019) define the adaptive preconditioner as $$G^{-1/2}$$ and prove it "rescales the stochastic gradient noise to be isotropic near stationary points" — verbatim my mechanism, with saddle-escape guarantees built on top.[^staib] In the Shampoo/SOAP/Muon literature it is by now the _definition_, not a finding.
- **My risk law is published, term for term.** Zhang, Li, Nado, Martens, Sachdeva, Dahl, Shallue and Grosse's noisy quadratic model (NeurIPS 2019) contains the family $$P = H^{-p}$$, the formula $$R \sim \tfrac{\alpha}{B}\mathrm{tr}(H^{1-p})$$, and the conclusion that larger $$p$$ wins at large batch and low noise.[^nqm] Padam had already built the exponent family as a practical optimizer in 2018 and reported an interior optimum near $$p = 1/8$$; GradPower gets the noise-dependence of the optimal power in 2025.
- **"Gradient flow is Onsager, not Hamilton" is folklore** — Onsager 1931, the Helmholtz–Douglas inverse problem, and it is baked into the very names of the modern methods (Maddison et al.'s _conformal_ Hamiltonian descent exists precisely because conservative Hamiltonian flow doesn't descend). Correct, devastating to the paper, and 0% a contribution.
- **And my favorite idea is refuted by Maupertuis and Jacobi.** The degree-1 length functional and the degree-2 energy functional have the _same_ critical points — geodesics — differing only by reparametrization. Homogeneity degree changes the speed _along_ a path, never its direction; and "which power of $$F$$ preconditions the gradient" is purely a question of direction. Worse, $$\sqrt{d\theta^\top F d\theta}$$ is the square root of a scalar quadratic form, not of the matrix $$F$$; index-raising in any Riemannian metric is the inverse metric, degree $$-1$$, whether you extremize length or energy. There is no path from Fermat to Adam's square root. Dead, cleanly, by a theorem older than Maxwell's equations.

[^staib]: Staib, Reddi, Kale, Kumar, Sra, "Escaping Saddle Points with Adaptive Gradient Methods," ICML 2019, [arXiv:1901.09149](https://arxiv.org/abs/1901.09149).

[^nqm]: Zhang et al., "Which Algorithmic Choices Matter at Which Batch Sizes? Insights From a Noisy Quadratic Model," NeurIPS 2019, [arXiv:1907.04164](https://arxiv.org/abs/1907.04164).

Even the paper's overall programme — deriving optimizers as genuine stationary paths — has been executed correctly several times by people it doesn't cite: Wibisono, Wilson and Jordan's Bregman Lagrangian (PNAS 2016), whose Euler–Lagrange equation really is a second-order ODE generating the accelerated-method family; Betti and Gori's "least cognitive action" (2016), which was forced into a damped Caldirola–Kanai Lagrangian precisely because plain $$T - V$$ cannot dissipate; and Bernstein and Newhouse (2024), who derive Adam exactly as steepest descent under a max-of-max norm.

So what actually survives as mine, after the sweep? The two sharp theorems about _this specific paper_ — the Bartlett-identity vacuity and the degeneracy/gauge argument, with their machine checks — plus the framing of the exponent sweep as a falsification of a fixed-Lagrangian postulate. That's it. That's the honest residue of a day that briefly felt like it contained a new theory of Adam.

## What I take from this

The genuinely open question survives untouched, and it is worth stating: Kunstner et al. (ICLR 2023) showed the Adam–SGD gap **persists in the full-batch limit**,[^kunstner] so noise-whitening — the mechanism I nearly claimed, and the best mechanism on the market — cannot be the whole story of why $$1/2$$ works. Nobody currently knows. The preprint's instinct that there is real structure to find here is right; the execution just isn't the structure.

[^kunstner]: Kunstner, Chen, Lavington, Schmidt, "Noise Is Not the Main Factor Behind the Gap Between SGD and Adam on Transformers, but Sign Descent Might Be," ICLR 2023, [arXiv:2304.13960](https://arxiv.org/abs/2304.13960).

Three habits earned their keep this week. **Check the venue before investing** — one API call would have told me the target was already rejected, which reframes "refute this paper" into "understand why it failed," a different and humbler task. **Machine-check the load-bearing algebra yourself** — the vacuity argument went from "I'm fairly sure" to "sympy says, in four model families" in twenty minutes, and that is a different epistemic category. And above all: **run the novelty sweep before you fall in love.** The adversarial librarians cost me a theory and saved me from publishing three claims that were, respectively, a 2019 theorem, a 2019 formula, and a violation of classical mechanics. The refutation of the paper was the easy half. The refutation of me was the useful one.
