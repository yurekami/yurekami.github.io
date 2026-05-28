---
layout: post
title: "Geometrizing p-adic Hodge Theory: Bhatt's Princeton Lectures"
slug: p-adic-hodge-theory-bhatt
date: 2026-05-28
description: A reading of Bhargav Bhatt's 138-page Princeton lecture notes on aspects of p-adic Hodge theory — perfectoid crystals, the Riemann–Hilbert functor, almost coherence, and a choicefree p-adic Simpson correspondence.
tags: [mathematics, algebraic-geometry, p-adic-hodge-theory]
categories: [research]
giscus_comments: true
related_posts: true
toc:
  sidebar: left
---

There is a recurring pattern in mathematics: a great comparison theorem appears first as a miracle — a deep and surprising bridge between two worlds — and then, decades later, someone finds the _right_ geometric object that makes the bridge inevitable. Bhargav Bhatt's 138-page lecture notes, _Aspects of p-adic Hodge Theory_,[^notes] are a case study in this second move.

[^notes]: Bhargav Bhatt, "Aspects of p-adic Hodge Theory," lecture notes for MAT 517, Princeton University, Fall 2025. Available at [https://www.math.ias.edu/~bhatt/teaching/mat517f25/pHT-notes.pdf](https://www.math.ias.edu/~bhatt/teaching/mat517f25/pHT-notes.pdf). 138 pages.

The notes present two bodies of joint work: with **Jacob Lurie** on a _p-adic Riemann–Hilbert correspondence_ (§1–11, dating to 2019), and with **Mingjia Zhang** on a _p-adic Simpson correspondence_ (§12, from 2024). The unifying theme is **geometrization**: constructing stacks and functors that make the comparison theorems of p-adic Hodge theory fall out as formal consequences of geometry, rather than requiring intricate period-ring calculations.

## The Problem: Three Worlds, No Canonical Bridge

Classical Hodge theory over the complex numbers gives us three ways to look at the same cohomological data on a smooth projective variety $$X/\mathbb{C}$$:

- **Betti cohomology** $$H^n(X(\mathbb{C}), \mathbb{Z})$$ — the topological invariant
- **de Rham cohomology** $$H^n_{\mathrm{dR}}(X/\mathbb{C})$$ — built from differential forms
- **Dolbeault cohomology** $$\bigoplus_{p+q=n} H^q(X, \Omega^p_X)$$ — the Hodge decomposition

The **Riemann–Hilbert correspondence** connects Betti data (local systems) to de Rham data (flat connections). The **Simpson correspondence** connects de Rham data to Dolbeault data (Higgs bundles). Together, they form the non-abelian Hodge trinity.

Over a p-adic field $$K/\mathbb{Q}_p$$, all three analogues exist — étale cohomology with $$\mathbb{Z}_p$$-coefficients replaces Betti, crystalline/prismatic cohomology replaces de Rham, Hodge–Tate cohomology replaces Dolbeault — but the bridges between them have historically required **auxiliary choices** (lifts to period rings like $$B_{\mathrm{dR}}^+$$, choices of Frobenius lifts, etc.) and worked only for **restricted classes** of coefficients (local systems rather than constructible sheaves).

Bhatt's notes eliminate both limitations.

## The Blueprint: Characteristic p

Before diving into the p-adic world, Bhatt spends §2 on the characteristic $$p$$ case, which serves as a blueprint for everything that follows. This is a pedagogical masterstroke — the ideas are the same, but the technical overhead vanishes.

For an $$\mathbb{F}_p$$-scheme $$X$$, the **Artin–Schreier sequence**

$$0 \to \mathbb{F}_p \to \mathcal{O}_X \xrightarrow{\phi - 1} \mathcal{O}_X \to 0$$

already links étale $$\mathbb{F}_p$$-cohomology with coherent cohomology. The key actor is the **perfection** $$X^{\mathrm{perf}}$$, obtained by inverting Frobenius:

$$X^{\mathrm{perf}} := \varprojlim\left(\cdots \xrightarrow{\phi} X \xrightarrow{\phi} X\right)$$

This is universal: $$X^{\mathrm{perf}} \to X$$ is the best approximation of $$X$$ by a perfect scheme. And quasi-coherent sheaves on the perfection turn out to be the right receptacle for a Riemann–Hilbert functor:

> **Theorem 2.1.3** (Bhatt–Lurie). _There is a colimit-preserving, symmetric monoidal, t-exact, fully faithful functor_
>
> $$\mathrm{RH}: D(X_{\mathrm{\acute{e}t}}, \mathbb{F}_p) \to D(X^{\mathrm{perf}})^{\phi=1}$$
>
> _compatible with proper pushforward and arbitrary pullback._

In words: every étale $$\mathbb{F}_p$$-sheaf becomes a quasi-coherent sheaf on $$X^{\mathrm{perf}}$$ equipped with a Frobenius action, and this assignment respects all the operations you care about.

The characteristic p story already has consequences. One of the most striking is what Bhatt calls **"evaporation"** (Proposition 2.2.1): for any proper map $$f: X \to \mathrm{Spec}(R)$$ of noetherian $$\mathbb{F}_p$$-schemes, higher coherent cohomology can always be killed by passing to a finite cover. The proof fits in half a page — but it uses the _full power_ of the Riemann–Hilbert functor (proper pushforward, $$t$$-exactness, symmetric monoidality) in an essential way.

## The Perfectization: Geometrizing Mixed Characteristic

Now fix a prime $$p$$ and work over a p-adic formal scheme $$X$$ — roughly, a scheme where we keep track of $$p$$-adic convergence. The central geometric construction of the notes is the **perfectization** $$X^{\mathrm{pfd}}$$, a stack that plays the role of $$X^{\mathrm{perf}}$$ in mixed characteristic.

The perfectization is a "perfectoid algebraic space": it has the form $$U/R$$ for a flat equivalence relation $$R \rightrightarrows U$$ on perfectoid formal schemes. Intuitively, $$X^{\mathrm{pfd}}$$ is the best possible approximation of $$X$$ by perfectoid spaces — objects where $$p$$-th power roots exist in abundance, enabling a tilt to characteristic $$p$$.

The quasi-coherent derived category of the perfectization is the category of **perfectoid crystals**:

$$\mathrm{Crys}(\mathcal{A}_X) := D_{\mathrm{qc}}(X^{\mathrm{pfd}})$$

These are pullback-compatible assignments of quasi-coherent sheaves along all maps from perfectoid test objects to $$X$$. Almost mathematics provides a localization

$$\mathrm{Crys}(\mathcal{A}_X) \to \mathrm{Crys}(\mathcal{A}_X)^a$$

whose objects are **almost perfectoid crystals** — the category where the p-adic Riemann–Hilbert functor lands.

### Two instructive examples

To build intuition for $$X^{\mathrm{pfd}}$$, two examples from §1 are worth lingering on.

**The torus.** Take $$X = \mathbb{G}_m$$ as a formal $$\mathcal{O}_K$$-scheme, where $$K/\mathbb{Q}_p$$ is perfectoid and contains all $$p$$-power roots of unity. The naive perfection $$\mathbb{G}_m^{\mathrm{perf}} = \varprojlim_{x \mapsto x^p} \mathbb{G}_m$$ carries an action of $$\mathbb{Z}_p(1) = \varprojlim_n \mu_{p^n}(K)$$, and the perfectization is _almost_ the quotient $$\mathbb{G}_m^{\mathrm{perf}}/\mathbb{Z}_p(1)$$. The Tate twist $$\mathbb{Z}_p(1)$$ — a purely arithmetic object — appears naturally as the structure group of the perfectization.

**A DVR.** For $$X = \mathrm{Spf}(\mathcal{O}_K)$$ where $$K/\mathbb{Q}_p$$ is discretely valued with completed algebraic closure $$C$$ and absolute Galois group $$G_K$$, the perfectization is almost $$\mathrm{Spf}(\mathcal{O}_C)/G_K$$. The Galois group — the central object of arithmetic geometry — governs the perfectization.

## The p-adic Riemann–Hilbert Functor

The construction culminates in §6 with the **p-adic Riemann–Hilbert functor**. Fix a nonarchimedean field $$K/\mathbb{Q}_p$$ and a formal scheme $$X/\mathcal{O}_K$$ with generic fibre $$\mathcal{X} = X_\eta$$. The functor takes the form:

$$\mathrm{RH}^a_\Delta: \widehat{D}(\mathcal{X}_{\mathrm{\acute{e}t}}, \mathbb{Z}_p)^{\mathrm{oc}} \to \mathrm{Crys}(\mathcal{A}_X)^{a, \phi=1}$$

This is symmetric monoidal, colimit-preserving, and fully faithful on dualizable objects (under mild assumptions on $$K$$). There is also a refinement without the "almost" when $$X$$ arises from a finite-type $$\mathcal{O}_K$$-scheme.

### What RH does to a point

The simplest case is already illuminating. Take $$K$$ algebraically closed and $$X = \mathrm{Spf}(\mathcal{O}_K)$$. Then $$X^{\mathrm{pfd}} \simeq X$$, so perfectoid crystals are just $$p$$-complete $$\mathcal{O}_K$$-modules. The generic fibre is a geometric point, so étale $$\mathbb{Z}_p$$-sheaves are just $$\mathbb{Z}_p$$-modules. The Riemann–Hilbert functor carries $$M \in \widehat{D}(\mathbb{Z}_p)$$ to

$$M \widehat{\otimes}_{\mathbb{Z}_p} \mathcal{O}_K \in \widehat{D}(\mathcal{O}_K)^a$$

— the most natural thing imaginable.

### The six-functor package

What makes $$\mathrm{RH}$$ genuinely powerful, beyond its construction, is the **six-functor formalism** established in §7–10. On the subcategory of Zariski constructible sheaves, $$\mathrm{RH}$$ satisfies:

- **Proper pushforward**: commutes with $$f_*$$ for proper $$f$$
- **Arbitrary pullback**: commutes with $$f^*$$
- **Almost coherence**: carries constructible sheaves to almost coherent crystals
- **Duality**: intertwines Verdier duality (étale side) with Grothendieck duality (crystal side)
- **t-structure exactness**: respects both the standard and perverse t-structures

This is the package that turns a construction into a tool.

## Almost Coherence: A Finiteness Miracle

One of the most beautiful aspects of the theory is the notion of **almost coherence** (§7). Over a perfectoid field $$K$$, perfectoid crystals on $$X$$ satisfy an intrinsic finiteness condition. An almost crystal $$M$$ is almost coherent if, for every $$\epsilon \in \mathfrak{m}_K$$, there exists a perfect $$\mathcal{O}_K$$-complex $$M_\epsilon$$ and a map $$M_\epsilon \to M$$ whose cokernel is killed by $$\epsilon$$.

This is weaker than coherence in the classical sense — the object

$$\bigoplus_n \mathcal{O}_K/(\epsilon_n)[a_n]$$

is almost coherent for any sequence $$\{\epsilon_n\}$$ in $$\mathfrak{m}_K$$ with $$|\epsilon_n| \to 1$$ and any integer shifts $$\{a_n\}$$, even though it is not coherent unless $$\epsilon_n$$ is a unit for large $$n$$.

Despite this permissiveness, almost coherent crystals have a **Grothendieck duality** autoequivalence, and pushforward along $$X^{\mathrm{pfd}} \to X$$ preserves and detects almost coherence. This finiteness theory, inspired by Zavyalov's work (itself inspired by unpublished work of Gabber), is what makes the applications in §8 and §11 possible.

## Application: Splinters and Vanishing Theorems

The geometric machinery developed in §1–7 has concrete payoffs.

### Derived splinters (§8)

A noetherian ring $$R$$ is a **splinter** if every finite injective map $$R \to S$$ has an $$R$$-linear splitting. Bhatt proves that in mixed characteristic, every splinter is a **derived splinter**: for any proper surjective $$f: X \to \mathrm{Spec}(R)$$, the map $$R \to R\Gamma(X, \mathcal{O}_X)$$ splits in $$D(R)$$. This implies, for instance, that $$R[1/p]$$ has rational singularities.

The proof uses $$\mathrm{RH}$$ to reduce to a statement about étale sheaves, where the argument parallels the characteristic $$p$$ "evaporation" proof from §2.

### Almost Kodaira vanishing (§11)

For a projective variety $$X/K$$ of dimension $$d$$ over a perfectoid field $$K$$, with formal model $$\mathcal{X}$$ carrying an ample line bundle $$\mathcal{L}$$: if $$X = \mathcal{X}_\eta$$ is smooth (or just lci), then

$$R\Gamma_{\mathrm{\acute{e}t}}\left(X, \mathcal{L} \otimes \mathcal{O}_X^+/p\right) \in D^{\geq d}.$$

This is an "almost" version of the Kodaira vanishing theorem — a statement that was previously out of reach in mixed characteristic.

## The Hodge–Tate Decomposition Falls Out

One of the most satisfying features of the theory is how the **Hodge–Tate decomposition** — a central result in p-adic Hodge theory — emerges as a natural consequence.

Take $$K$$ perfectoid and $$X/\mathcal{O}_K$$ a smooth formal scheme. The Riemann–Hilbert image $$\mathrm{RH}^a_\Delta(\mathbb{Z}_p)$$ is an almost coherent perfectoid crystal. Pushing forward to $$X$$ and inverting $$p$$ yields a coherent complex on the rigid space $$\mathcal{X} = X_\eta$$, which admits a filtration with graded pieces

$$\Omega^i_{\mathcal{X}/K}(-i)[-i],$$

the Deligne–du Bois $$i$$-forms with Tate twists. Choosing a lift of $$X$$ to $$B_{\mathrm{dR}}^+/(t)^2$$ splits the filtration, yielding:

$$R\Gamma(X^{\mathrm{pfd}}, \mathrm{RH}^a_\Delta(\mathbb{Z}_p))[1/p] \simeq \bigoplus_i R\Gamma(\mathcal{X}, \Omega^i_{\mathcal{X}/K})(-i)[-i].$$

When $$X/K$$ is proper and $$K = \overline{K}$$, the proper pushforward compatibility gives the classical Hodge–Tate decomposition

$$R\Gamma(\mathcal{X}_{\mathrm{\acute{e}t}}, \mathbb{Z}_p) \otimes_{\mathbb{Z}_p} K \simeq \bigoplus_i R\Gamma(\mathcal{X}, \Omega^i_{\mathcal{X}/K})(-i)[-i]$$

as established by Scholze and Faltings. But here it appears not as an independent theorem requiring its own proof, but as a _reading off_ from the geometric structure already built.

## The Simpson Gerbe: Choicefree Non-Abelian Hodge Theory

The final section (§12), joint with Mingjia Zhang, is perhaps the most conceptually striking. It constructs the **p-adic Corlette–Simpson correspondence** — an equivalence between two categories attached to a smooth rigid space $$X/K$$:

$$\mathrm{Vect}(X_{\mathrm{pro\acute{e}t}}, \widehat{\mathcal{O}}) \simeq \mathrm{Higgs}(X, P_X)$$

The left side is **pro-étale vector bundles** — an enlargement of the category of $$K$$-local systems on $$X$$. The right side is **$$P_X$$-twisted Higgs bundles**, where $$P_X$$ is a canonical $$\mathbb{G}_m$$-gerbe on the (Tate-twisted) cotangent bundle $$T^*X\{-1\}$$.

### What is the Simpson gerbe?

The $$\mathbb{G}_m$$-gerbe $$P_X \to T^*X\{-1\}$$ is constructed from the **obstruction to lifting** along the square-zero thickening $$A_2(\mathcal{O}_K) \to \mathcal{O}_K$$, where $$A_2$$ is the second Fontaine–Jannsen ring. The transitivity triangle for cotangent complexes produces a natural obstruction class

$$\mathrm{ob}_{A_2}: L_{\mathcal{O}^+/\mathcal{O}_K}\{-1\}[-2] \to \mathcal{O}^+$$

which, combined with the p-adic exponential, yields the gerbe. A key technical input is that **differentials are étale-locally zero modulo $$p$$** (Proposition 12.4.3): the map $$L_{\mathcal{O}^+/\mathcal{O}_K} \to L_{\mathcal{O}/K}$$ is an isomorphism. This is the p-adic incarnation of the general principle that differential forms become small on highly ramified covers.

### Why the gerbe matters

The Simpson gerbe $$P_X$$ is **never trivial** when $$\dim(X) > 0$$. In fact, its Brauer class $$[P_X] \in H^2(T^*X\{-1\}, \mathcal{O}^*)$$ spans a copy of $$K$$ — it is genuinely analytic and cannot be the analytification of any algebraic gerbe. This is a fundamental contrast with complex geometry, where the analogous object is always torsion.

The choicefree nature of this construction is the primary innovation. Previous results by Faltings and Heuer required **auxiliary choices** — lifts to $$B_{\mathrm{dR}}^+/(t)^2$$, trivializations of exponential sheaves — to state a clean equivalence. In the present framework, these choices have a precise meaning: they provide trivializations of the Simpson gerbe on proper subvarieties of $$T^*X\{-1\}$$. Theorem 12.5.7 gives a direct Hodge-theoretic construction of such trivializations from the choices, thus reproving the Faltings–Heuer theorem as a consequence.

### Beyond local systems

The functor extends beyond local systems. Every almost coherent perfectoid crystal on $$X$$ — including those arising from constructible $$\mathbb{Z}_p$$-sheaves via $$\mathrm{RH}$$ — has a functorially attached $$P_X$$-twisted coherent sheaf on $$T^*X$$. Pre-composing with $$\mathrm{RH}$$ gives a Simpson functor

$$S: D^b_{\mathrm{cons}}(\mathcal{X}_{\mathrm{\acute{e}t}}, \mathbb{Z}_p) \to D^b_{\mathrm{coh}}(T^*X, P_X).$$

In complex geometry, passing from a constructible sheaf to a coherent sheaf on the cotangent bundle requires additional choices (a good filtration on the associated D-module). In the p-adic world, the functor exists canonically. As Bhatt puts it, this can be regarded as "an example of the arithmetic origin of the Hodge filtration."

## The Architecture of the Argument

One way to appreciate the design of the notes is to look at the logical dependencies:

The characteristic $$p$$ story (§2) is self-contained and serves as a "toy model" that previews every construction and application. The perfectoid foundations (§3) feed into the perfectization (§4), which leads to the overconvergent equivalence (§5) and the Riemann–Hilbert functor (§6). Almost coherence (§7) is the finiteness theory that enables applications: splinters (§8), duality (§9), t-structures (§10), and vanishing theorems (§11). The non-abelian Simpson correspondence (§12) uses §4–§7 but is largely independent of §8–§11 — it builds on the crystal formalism but goes in a different direction.

The entire development is guided by a principle worth stating explicitly: **find the right geometric object, and the comparison theorem becomes a tautology**. The perfectization $$X^{\mathrm{pfd}}$$ is that object for the Riemann–Hilbert side. The Simpson gerbe $$P_X$$ is that object for the Higgs side.

## What This Changes

These notes consolidate — and substantially extend — a decade of rapid progress in p-adic Hodge theory (Scholze's perfectoid spaces, Bhatt–Scholze's prismatic cohomology, Faltings's and Heuer's p-adic Simpson work). Several features stand out:

**Constructible coefficients.** Previous p-adic Riemann–Hilbert constructions typically worked for local systems. The functor $$\mathrm{RH}$$ handles the full derived category of constructible $$\mathbb{Z}_p$$-sheaves — the analogue of working with all perverse sheaves, not just local systems.

**No auxiliary choices.** The Simpson gerbe $$P_X$$ is canonical. This is not just aesthetically satisfying — it reveals structural content. The fact that the gerbe is non-torsion, for instance, is invisible in formulations that trivialize it by fiat.

**Formal-scheme foundations.** By working with formal schemes rather than rigid spaces from the start, the theory is accessible to commutative algebraists (the splinter and vanishing theorem applications in §8 and §11), not just p-adic geometers.

**A model for what's next.** The notes explicitly mention ongoing work of Anschütz, Camargo, Le Bras, and Scholze on "analytic prismatization," which would give a rigid-analytic analogue of the formal-scheme picture in §12.2 — the Hodge–Tate stack as a torsor, with the Simpson gerbe as its Cartier dual. The geometric framework Bhatt has built is designed to interface with this future direction.

## Coda

What makes these notes compelling is not any single theorem, but the _shape_ of the whole. A dozen classical results in p-adic Hodge theory — the Hodge–Tate decomposition, Katz's theorem on unit-root F-crystals, Kodaira vanishing, the splinter conjecture, the Faltings–Heuer Simpson correspondence — all appear as consequences of a single geometric idea: that the perfectization $$X^{\mathrm{pfd}}$$ and the Simpson gerbe $$P_X$$ are the right objects to mediate between étale and coherent worlds.

Feynman is reputed to have said: "What I cannot create, I do not understand." These notes create the geometry from which p-adic Hodge theory follows. That is the deepest form of understanding.

---

_The notes are available at [https://www.math.ias.edu/~bhatt/teaching/mat517f25/pHT-notes.pdf](https://www.math.ias.edu/~bhatt/teaching/mat517f25/pHT-notes.pdf)._
