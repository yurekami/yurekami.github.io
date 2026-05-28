---
layout: post
title: "Algebraic Geometry's Secret Plot to Take Over Analysis: Scholze and Clausen's Analytic Geometry"
slug: analytic-geometry-scholze-clausen
date: 2026-05-28
description: A reading of Scholze and Clausen's 110-page lecture notes on analytic geometry — how condensed mathematics, liquid modules, entropy, and a mysterious arithmetic ring Z((T))_r turn functional analysis into commutative algebra and unify adic spaces with complex-analytic geometry.
tags: [mathematics, algebraic-geometry, condensed-mathematics, functional-analysis]
categories: [research]
giscus_comments: true
related_posts: true
toc:
  sidebar: left
---

Mumford once wrote that algebraic geometry "seems to have acquired the reputation of being esoteric, exclusive, and very abstract, with adherents who are secretly plotting to take over all the rest of mathematics. In one respect this last point is accurate."

Peter Scholze opens his lectures on analytic geometry[^notes] by quoting this line and adding: "For some reason, this secret plot has so far stopped short of taking over analysis. The goal of this course is to launch a new attack, turning functional analysis into a branch of commutative algebra, and various types of analytic geometry (like manifolds) into algebraic geometry."

[^notes]: Peter Scholze (joint with Dustin Clausen), _Lectures on Analytic Geometry_, lecture notes for a course at the University of Bonn, Winter 2019/20. Available at [https://www.math.uni-bonn.de/people/scholze/Analytic.pdf](https://www.math.uni-bonn.de/people/scholze/Analytic.pdf). 110 pages.

This is not hyperbole. These 110-page lecture notes — the second installment of Scholze and Clausen's condensed mathematics program, following the _Lectures on Condensed Mathematics_ from the preceding semester — construct a framework in which adic spaces (the foundation of $$p$$-adic geometry), complex-analytic spaces, smooth manifolds, and topological manifolds all become instances of a single notion of **analytic space**, built from the same categorical machinery as algebraic geometry: rings, modules, spectra, descent, quasicoherent sheaves.

The key innovation making this possible is the theory of **liquid modules** — a new category of "topological" real vector spaces that, unlike every previous attempt, forms an abelian category with a well-behaved tensor product. The obstruction to building this category turns out to be _entropy_. And the tool for overcoming entropy is an arithmetic ring $$\mathbb{Z}((T))_r$$ that simultaneously encodes all $$\ell^p$$-spaces and connects the archimedean world to $$p$$-adic arithmetic.

## The Broken Category

The dream is old: do algebraic geometry with convergent power series instead of polynomials, with "small" open sets instead of "nonvanishing" open sets. The obstacle is equally old.

Algebraic geometry is built on a foundation of commutative algebra. You start with the abelian category $$\mathrm{Ab}$$ of abelian groups, with its tensor product. Rings are monoid objects. Modules over rings give quasicoherent sheaves. Spec gives the space. Everything flows from the algebra.

To do _analytic_ geometry, you want the same workflow but with topological vector spaces. But topological abelian groups **do not form an abelian category**. The map

$$(\mathbb{R}, \text{discrete topology}) \to (\mathbb{R}, \text{natural topology})$$

has trivial kernel and cokernel, but is not an isomorphism. This means you cannot take kernels, cokernels, or do homological algebra in any straightforward way. For a century, analysts worked around this with ad hoc constructions — Fréchet spaces, nuclear spaces, bornological spaces — but none of them provided the categorical infrastructure that algebraic geometers take for granted.

Condensed mathematics fixes this, and these lecture notes build the analytic geometry that results.

## Condensed Sets: The Fix

A **condensed set** is a sheaf on the category of profinite sets (compact, totally disconnected Hausdorff spaces), with covers given by finite jointly surjective families. Concretely, it is a functor $$X: \mathrm{ProFin}^{\mathrm{op}} \to \mathrm{Sets}$$ satisfying a sheaf condition.

Every topological space $$T$$ gives a condensed set via $$T(S) = \mathrm{Cont}(S, T)$$. The category of condensed abelian groups is abelian — it has all kernels, cokernels, and exact sequences. Moreover, it embeds compactly generated topological spaces fully faithfully (Proposition 1.2), and is equivalent to the category of compact Hausdorff spaces on the quasicompact quasiseparated objects.

The philosophy: rather than asking "which subsets of $$X$$ are open?", ask "what are the continuous maps from every profinite set into $$X$$?" The shift from internal structure to external probes is what makes the category algebraic.

## Solid Modules: The $$p$$-adic World

The first new algebraic structure built on this foundation is the category of **solid abelian groups** (Lecture II). For a profinite set $$S = \varprojlim_i S_i$$, the **solid free module** is

$$\mathbb{Z}[S]^{\blacksquare} := \varprojlim_i \mathbb{Z}[S_i],$$

which can be thought of as the space of $$\mathbb{Z}$$-valued measures on $$S$$. A condensed abelian group $$M$$ is _solid_ if every continuous map $$f: S \to M$$ from a profinite set extends uniquely to a map $$\mathbb{Z}[S]^{\blacksquare} \to M$$ — in other words, you can "integrate" against arbitrary measures.

The motivating computation (which captures the entire spirit of the theory): take $$S = \mathbb{N} \cup \{\infty\}$$, the one-point compactification of $$\mathbb{N}$$. The solidification of $$\mathbb{Z}[T] = \mathbb{Z}[S]/([{\infty}] = 0)$$ is

$$\mathbb{Z}[T]^{\blacksquare} = \varprojlim_n \mathbb{Z}[T]/T^n = \mathbb{Z}[[T]].$$

Solidification converts polynomial rings into formal power series rings. This is exactly the passage from algebraic to $$p$$-adic geometry, performed categorically.

The category of solid abelian groups is abelian, closed under all limits and colimits, and carries a symmetric monoidal tensor product $$\otimes^{\blacksquare}$$ (Theorem 2.5). Over $$\mathbb{Q}_p$$, the solid tensor product gives $$\mathbb{Q}_p\langle T \rangle \otimes^{\blacksquare}_{\mathbb{Q}_p[T]} \mathbb{Q}_p\langle T \rangle \cong \mathbb{Q}_p\langle T \rangle$$ — the Tate algebra is an honest localization. This is what makes adic spaces work as algebraic geometry.

## The Archimedean Problem: Entropy

Solid modules handle the $$p$$-adic world beautifully. But $$\mathbb{R}$$ is not solid: $$T = 1/2$$ satisfies $$T^n \to 0$$, but not every series $$\sum r_n (1/2)^n$$ with $$r_n \in \mathbb{Z}$$ converges in $$\mathbb{R}$$. The solid framework fundamentally cannot handle archimedean geometry.

The first attempt at a replacement (Lecture IV) is the category of **$$\mathcal{M}$$-complete condensed $$\mathbb{R}$$-vector spaces** — those where integration against signed Radon measures $$\mathcal{M}(S)$$ is well-defined. This works better: Banach spaces are $$\mathcal{M}$$-complete, and the tensor product gives the projective tensor product of Banach spaces.

But $$\mathcal{M}$$-complete modules don't form an abelian category either. Lecture V identifies the obstruction: **entropy**.

The key construction is due to Ribe (1979). Consider the function

$$H: \ell^1(\mathbb{N}) \to \mathbb{R}, \quad H(x_0, x_1, \ldots) = s \log\lvert s \rvert - \sum_i x_i \log\lvert x_i \rvert, \quad s = \sum_i x_i.$$

This is the Shannon entropy of the distribution $$(\lvert x_i \rvert/s)$$, scaled by $$s$$. A direct estimate shows:

$$\lvert s \log\lvert s\rvert + t \log\lvert t\rvert - (s+t)\log\lvert s+t\rvert \rvert \leq 2\log 2 \cdot (\lvert s\rvert + \lvert t\rvert)$$

for all $$s, t \in \mathbb{R}$$. So $$H$$ is "almost linear" — it satisfies $$\lvert H(v + w) - H(v) - H(w) \rvert \leq C(\lVert v \rVert + \lVert w \rVert)$$. Such almost-linear functions give rise to extensions of topological vector spaces. But $$H$$ is not _globally_ close to any linear function: $$H(1/n, \ldots, 1/n, 0, \ldots) = \log n \to \infty$$, while any linear function $$\phi$$ with $$\phi(e_i) = \phi(e_j)$$ for all $$i,j$$ would give a bounded sequence. Therefore $$H$$ defines a **non-split extension**

$$0 \to \mathbb{R} \to \tilde{V} \to \ell^1(\mathbb{N}) \to 0$$

— Ribe's extension. The condensed formulation (Proposition 5.6) shows this extension exists in the category of Smith spaces (the duals of Banach spaces), proving that $$\mathcal{M}$$-complete modules are **not stable under extensions**.

Entropy — the function $$x \log\lvert x \rvert$$ — is the precise obstruction to building abelian categories from Banach spaces. This is one of the most surprising connections in the entire program: information theory enters the foundations of geometry.

## The Real $$B_{\mathrm{dR}}^+$$: Period Rings for the Reals

The entropy obstruction can be "thickened out" — extended infinitely in a nilpotent direction — to produce a real analogue of Fontaine's period ring $$B_{\mathrm{dR}}^+$$ from $$p$$-adic Hodge theory.

The idea: the function $$x \log\lvert x \rvert$$ encodes the first-order deviation from linearity. Its higher powers $$x \log^i\lvert x \rvert$$ encode higher-order deviations. The **Teichmüller-like representative**

$$[x] := x\lvert x \rvert^t = x + x\log\lvert x \rvert \cdot t + \tfrac{1}{2} x\log^2\lvert x \rvert \cdot t^2 + \cdots \in \mathbb{R}[[t]]$$

is a multiplicative map $$\mathbb{R} \to \mathbb{R}[[t]]$$ that captures all orders at once. In the coordinates given by the $$[\cdot]$$-map, the $$\ell^1$$-balls in $$\mathbb{R}[t]/t^n$$ can be defined:

$$ (\mathbb{R}[t]/t^n)[S]^{\ell^1 \leq c} := \left\{ \sum*i [x*{i,s}] t^i \;\middle\vert\; \sum*{i,s} \lvert x*{i,s}\rvert \leq c \right\} $$

The entropy estimate (Lemma 5.9) shows these subsets are stable under addition up to a constant $$C(n)$$ — but the constant depends on the number of nilpotent levels. For $$p = 1$$ (the $$\ell^1$$ case), the construction cannot be made functorial for all profinite sets $$S$$. This is a _fundamental_ obstruction, not a technical one.

## Liquid Modules: The Solution

The resolution (Lecture VI) is to relax from $$\ell^1$$ to $$\ell^p$$ for $$p < 1$$. Kalton's theorem (1981) states that any extension of $$p$$-Banach spaces is $$p'$$-Banach for all $$p' < p$$ — the three-space problem has a positive answer for quasi-Banach spaces. This suggests replacing the measure space $$\mathcal{M}(S)$$ with

$$\mathcal{M}_p(S) := \bigcup_{c > 0} \varprojlim_i \left\{ (a_s)_s \in \mathbb{R}[S_i] \;\middle\vert\; \sum \lvert a_s\rvert^p \leq c \right\}$$

— the space of "$$\ell^p$$-measures" on $$S$$. For $$p < 1$$, the $$[\cdot]$$-coordinates work functorially: the key bound $$\log^i\lvert x \rvert \leq \lvert x \rvert^{p-1}$$ for small $$\lvert x \rvert$$ (Proposition 6.4) ensures stability of the $$\ell^p$$-balls under all transition maps of profinite sets.

A condensed $$\mathbb{R}$$-vector space is **$$p$$-liquid** if integration against $$\ell^{p'}$$-measures is well-defined for all $$p' < p$$:

> **Theorem 6.5.** _Fix $$0 < p \leq 1$$. The category $$\mathrm{Liq}_p(\mathbb{R})$$ of $$p$$-liquid condensed $$\mathbb{R}$$-vector spaces is an abelian subcategory of $$\mathrm{Cond}(\mathbb{R})$$, stable under kernels, cokernels, and extensions. It has compact projective generators $$\mathcal{M}_{<p}(S)$$ for extremally disconnected $$S$$, a symmetric monoidal tensor product $$\otimes^{\mathrm{liq}}_p$$, and the derived category $$D(\mathrm{Liq}_p(\mathbb{R}))$$ embeds fully faithfully into $$D(\mathrm{Cond}(\mathbb{R}))$$.\_

This is the category that makes archimedean analytic geometry possible. All Banach spaces (and more generally all complete locally convex spaces) are $$p$$-liquid for all $$0 < p \leq 1$$. The category has all the properties that algebraic geometers need to run their machinery.

## The Arithmetic Ring $$\mathbb{Z}((T))_r$$

The most technically surprising aspect of the theory is that the real-variable problem reduces to an arithmetic one. The proof of Theorem 6.5 does not stay in the world of $$\mathbb{R}$$-vector spaces — it passes through the **condensed ring** $$\mathbb{Z}((T))_r$$, whose $$S$$-valued points are formal Laurent series $$\sum a_n T^n$$ with $$a_n \in C(S, \mathbb{Z})$$ and $$\sum \lvert a_n \rvert r^n < \infty$$.

This ring is entirely arithmetic — it is built from $$\mathbb{Z}$$ and formal variables — but it simultaneously encodes all the $$\ell^p$$-theories. The key theorem (6.9): for any $$0 < r' < r$$, the evaluation map $$\theta_{r'}: \mathbb{Z}((T))_r \to \mathbb{R}$$ sending $$T \mapsto r'$$ is surjective, with kernel generated by a single nonzerodivisor $$f_{r'}$$. Moreover:

$$\mathcal{M}(S, \mathbb{Z}((T))_r)/(f_{r'}) \cong \mathcal{M}_p(S)$$

where $$p$$ is determined by $$(r')^p = r$$. Different choices of $$r'$$ give different values of $$p$$. The ring $$\mathbb{Z}((T))_r$$ sits above all of them.

The pivotal computation (Lecture VII): **$$\mathbb{Z}((T))_{>r}$$ is a principal ideal domain.** This is the technical heart of the proof — it reduces the entire liquid module theory to algebra over a PID. The result was essentially known to Harbater (1984) in a different context, but its role here is completely new.

## Analytic Spaces

With liquid modules in hand, Lecture XII defines **analytic rings** — pairs $$(A, \mathcal{M})$$ of a condensed ring $$A$$ and a functor $$\mathcal{M}$$ assigning to each extremally disconnected profinite set $$S$$ a condensed $$A$$-module $$\mathcal{M}[S]$$, subject to axioms encoding "convergence." The key examples:

- **Solid rings** $$(A, A)^{\blacksquare}$$: the $$p$$-adic world. $$\mathbb{Z}_p$$, $$\mathbb{Q}_p$$, Tate algebras.
- **Liquid rings** $$(\mathbb{R}, \mathcal{M}_{<p})$$: the archimedean world. Banach spaces, Fréchet spaces.
- **$$\mathbb{Z}((T))_{>r}$$**: the arithmetic ring that connects them.

Lecture XIII defines **analytic spaces** — sheaves on the opposite category of analytic rings that are locally affine. The theory of **quasicoherent sheaves** (Definition 13.7) is defined by descent, exactly as in algebraic geometry.

The payoff comes in Lecture XIV:

- **Adic spaces embed fully faithfully** into analytic spaces (Proposition 14.7). Every Huber pair $$(A, A^+)$$ gives an analytic ring $$(A, A^+)^{\blacksquare}$$, and the structure sheaf on rational subsets is recovered.

- **Complex-analytic spaces embed** into analytic spaces over $$(\mathbb{C}, \mathcal{M}_{<p})$$ for any $$0 < p \leq 1$$ (Proposition 14.3). So do real-analytic, smooth, and topological manifolds.

- **Algebraic varieties embed** into analytic spaces over any analytic ring (Proposition 14.5), with a fully faithful functor on quasicoherent sheaves.

- **Dualizable objects** in $$D((A, A^+)^{\blacksquare})$$ are exactly perfect complexes (Theorem 14.9) — the same answer as in algebraic geometry.

All flavors of geometry under one roof.

## The Berkovich Spectrum of $$\mathbb{Z}$$

The final vision (pages 106–107) is the most striking. Scholze introduces the **Novikov-type ring** $$\mathbb{Z}((T^{\mathbb{R}}))_{>r}$$, consisting of formal sums $$\sum_{x \in \mathbb{R}} a_x T^x$$ with $$a_x \in \mathbb{Z}$$ and $$\sum \lvert a_x \rvert r^x < \infty$$. This ring carries an $$\mathbb{R}_{\geq 1}$$-action via $$T \mapsto T^t$$, making it a $$\lambda$$-ring with continuous Frobenius lifts.

There is a natural map from the underlying space $$\lvert \mathrm{AnSpec}(\mathbb{Z}((T^{\mathbb{R}}))_{>r}) \rvert$$ to $$\mathcal{M}(\mathbb{Z})$$ — the **Berkovich spectrum of $$\mathbb{Z}$$**, the space of all multiplicative seminorms on $$\mathbb{Z}$$. This space contains:

- A point for each prime $$p$$ (the $$p$$-adic valuation)
- A real interval $$(0, 1]$$ (the archimedean valuations, parametrized by $$p$$ in the liquid theory $$(\mathbb{R}, \mathcal{M}_{<p})$$)
- Points for the completions $$\mathbb{Q}_p$$, $$\mathbb{F}_p$$, and $$\mathbb{Q}$$

The family of liquid $$\mathbb{R}$$-vector space structures $$(\mathbb{R}, \mathcal{M}_{<p})$$, parametrized by $$p \in (0, 1]$$, traces out the archimedean segment of $$\mathcal{M}(\mathbb{Z})$$. As $$p \to 0$$, one enters the arithmetic part — the $$p$$-adic completions.

Scholze concludes: "the analytic ring structures on $$\mathbb{R}$$ are inextricably linked to arithmetic, which arises in the limit $$p \to 0$$."

This is the deepest punchline of the entire program. The passage from analysis to arithmetic is not an analogy — it is a _deformation_. The archimedean and non-archimedean worlds are literally connected by a one-parameter family of analytic ring structures, all living over the Berkovich spectrum of the most fundamental ring in mathematics.

## What Happened Next

These notes are from 2019/20. Since then:

- The **Liquid Tensor Experiment** (2020–2022): Scholze publicly challenged the formal verification community to verify the key technical theorem (Theorem 9.1 in these notes — the vanishing of certain Ext groups for liquid modules). A team led by Johan Commelin formalized the proof in Lean, completing it in July 2022.[^liquid] This was one of the first times a Fields medalist asked proof assistants to check a result he wasn't fully confident about.

- **Condensed Mathematics and Complex Geometry** (Clausen–Scholze, 2022/2026): The program was applied to reprove the pillars of complex geometry — finiteness, Serre duality, GAGA, Riemann–Roch — from condensed foundations, as discussed in a [previous post](/blog/2026/condensed-mathematics-complex-geometry/).

- **Geometric Langlands**: The six-functor formalism for condensed/analytic sheaves became a key tool in the proof of the geometric Langlands conjecture, as discussed in [today's other post](/blog/2026/geometric-langlands-scholze/).

- **Light condensed sets**: Clausen and Scholze refined the set-theoretic foundations, replacing the universe-enlargement trick with a cleaner notion of "light" condensed sets based on countable profinite sets.

[^liquid]: The Liquid Tensor Experiment: [https://leanprover-community.github.io/liquid/](https://leanprover-community.github.io/liquid/).

## The Shape of the Attack

What makes these notes extraordinary is not any single theorem but the _arc_ of the argument. You start with a broken category (topological abelian groups). You fix it (condensed sets). You build the $$p$$-adic theory cleanly (solid modules). You try to do the same for $$\mathbb{R}$$ and fail — and the failure is _entropy_, the function $$x \log\lvert x \rvert$$. You thicken the failure into a period ring (the real $$B_{\mathrm{dR}}^+$$). You relax from $$\ell^1$$ to $$\ell^p$$ and the obstruction vanishes (liquid modules). The proof reduces not to functional analysis but to arithmetic — the ring $$\mathbb{Z}((T))_r$$ is a PID. And at the end, you discover that the archimedean and non-archimedean worlds are connected by a deformation over the Berkovich spectrum of $$\mathbb{Z}$$.

The secret plot has been launched. Algebraic geometry now has analysis in its sights.

---

_The notes are available at [https://www.math.uni-bonn.de/people/scholze/Analytic.pdf](https://www.math.uni-bonn.de/people/scholze/Analytic.pdf). The prerequisite [Lectures on Condensed Mathematics](https://www.math.uni-bonn.de/people/scholze/Condensed.pdf) is also available from Scholze's page._
