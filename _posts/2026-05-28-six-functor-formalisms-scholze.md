---
layout: post
title: "What Is a Six-Functor Formalism? Scholze's Answer"
slug: six-functor-formalisms-scholze
date: 2026-05-28
description: A reading of Scholze's 111-page lecture notes on six-functor formalisms — from the abstract definition via correspondences to Poincaré duality, ring stacks, solid modules, motivic sheaves, and a new construction of arithmetic D-modules via tempered power series.
tags: [mathematics, algebraic-geometry, category-theory, cohomology]
categories: [research]
giscus_comments: true
related_posts: true
toc:
  sidebar: left
---

For decades, the answer to "what is a six-functor formalism?" was "you know it when you see it." Every cohomology theory worth its name — étale cohomology, singular cohomology, D-modules, mixed Hodge modules — came with the same package of six interrelated functors and a web of compatibilities between them. But writing down what that package _actually is_, as a mathematical object, turned out to be one of the most difficult problems in modern algebraic geometry.

Peter Scholze's lecture notes, _Six-Functor Formalisms_,[^notes] first taught in Winter 2022/23 and updated in October 2025, give a definitive answer. The notes define the abstract notion, develop its formal consequences (including an elegant reduction of Poincaré–Verdier duality to a simple adjunction problem), and then work through a remarkable procession of examples — topological spaces, coherent sheaves, solid modules, motivic sheaves, and a new construction of arithmetic D-modules — that all fit into the same framework. A recurring theme, first observed by Drinfeld, is that six-functor formalisms factor through **ring stacks**: commutative ring objects in the world of stacks.

[^notes]: Peter Scholze, _Six-Functor Formalisms_, lecture notes for a course at the University of Bonn, Winter 2022/23, updated October 2025. Available at [https://people.mpim-bonn.mpg.de/scholze/SixFunctors.pdf](https://people.mpim-bonn.mpg.de/scholze/SixFunctors.pdf). 111 pages.

## The Six Functors, from Scratch

Start with a category $$\mathcal{C}$$ of geometric objects — topological spaces, schemes, analytic spaces — and an assignment $$X \mapsto D(X)$$ to some derived category. Six functors arise:

| Functor     | Description        | Right adjoint                             |
| ----------- | ------------------ | ----------------------------------------- |
| $$f^*$$     | Pullback           | $$f_*$$ (pushforward)                     |
| $$\otimes$$ | Tensor product     | $$\mathcal{H}\mathrm{om}$$ (internal Hom) |
| $$f_!$$     | Proper pushforward | $$f^!$$ (exceptional inverse image)       |

Each even-numbered functor is the right adjoint of the odd-numbered one above it. Pullback $$f^*$$ is symmetric monoidal ($$f^*(A \otimes B) \cong f^*A \otimes f^*B$$). And $$f_!$$ satisfies the **base change formula** ($$g^* f_! \cong f'_! g'^*$$ for Cartesian squares) and the **projection formula** ($$f_! A \otimes B \cong f_!(A \otimes f^*B)$$).

The first four functors are easy to formalize: $$(f^*, f_*, \otimes, \mathcal{H}\mathrm{om})$$ are encoded by a functor $$\mathcal{C}^{\mathrm{op}} \to \mathrm{CMon}(\mathrm{Cat}_\infty)$$ from the opposite category to symmetric monoidal $$\infty$$-categories. The right adjoints $$f_*$$ and $$\mathcal{H}\mathrm{om}$$ are just conditions (existence of adjoints), not extra data.

The entire difficulty lies in $$f_!$$. Its base change and projection formula are not just conditions — they are isomorphisms that must satisfy higher coherences, and spelling these out explicitly (as Ayoub attempted in 2007) leads to an infinite regress of compatibility diagrams that becomes unmanageable in the $$\infty$$-categorical setting.

## The Definition: Correspondences

The solution, following Mann (building on Liu–Zheng and Gaitsgory–Rozenblyum), is elegant. Let $$E \subset \mathcal{C}$$ be the class of morphisms for which $$f_!$$ is defined.

> **Definition 2.4.** A **3-functor formalism** is a lax symmetric monoidal functor
>
> $$D: \mathrm{Corr}(\mathcal{C}, E) \to \mathrm{Cat}_\infty$$
>
> where $$\mathrm{Corr}(\mathcal{C}, E)$$ is the $$\infty$$-category of **correspondences**.

The category of correspondences $$\mathrm{Corr}(\mathcal{C}, E)$$ has the same objects as $$\mathcal{C}$$, but a morphism $$X \to Y$$ is a **span** $$X \xleftarrow{g} W \xrightarrow{f} Y$$ with $$f \in E$$. Composition is by Cartesian squares. Three kinds of morphisms coexist in $$\mathrm{Corr}(\mathcal{C}, E)^{\otimes}$$:

1. The **symmetric monoidal structure** on $$D(X)$$ — encoded by the diagonal $$X \to X \times X$$
2. **Pullback** $$f^*$$ — from contravariant morphisms (the left leg of a span)
3. **Proper pushforward** $$f_!$$ — from covariant $$E$$-morphisms (the right leg)

All the coherences — base change, projection formula, associativity — are encoded _automatically_ by the functoriality of $$D$$ on correspondences. No additional data or diagrams needed.

> **Definition 2.5.** A **6-functor formalism** is a 3-functor formalism for which $$- \otimes A$$, $$f^*$$, and $$f_!$$ all admit right adjoints.

The right adjoints inherit all coherences from the left adjoints by abstract nonsense. Three functors specified, three obtained for free. The name "six-functor formalism" is earned.

## Construction in Practice

In practice, one doesn't build a functor on $$\mathrm{Corr}(\mathcal{C}, E)$$ directly. Instead, one starts with $$f^*$$ and $$\otimes$$, then constructs $$f_!$$ from a factorization system.

> **Theorem 4.6.** Suppose every $$f \in E$$ factors as $$f = \bar{f} \circ j$$ with $$j$$ an "open immersion" ($$j^*$$ admits a left adjoint $$j_!$$) and $$\bar{f}$$ "proper" ($$\bar{f}_*$$ admits a right adjoint, and satisfies proper base change and the projection formula). Then there exists a unique extension to a 3-functor formalism, with $$f_! = \bar{f}_* j_!$$.

This is the workhorse construction. The minimal hypotheses are surprisingly weak: base change for $$j_!$$ and $$\bar{f}_*$$, the projection formula, and one mixed compatibility. Even without a canonical compactification, $$f_!$$ is canonically defined as a functor of $$\infty$$-categories.

## Poincaré Duality: An Adjunction Problem

The most elegant result in the abstract theory is the reduction of Poincaré–Verdier duality to a simple categorical criterion.

Scholze constructs a **2-category of kernels** $$\mathcal{LZ}_D$$: objects are objects of $$\mathcal{C}$$, and $$\mathrm{Hom}_{\mathcal{LZ}_D}(X, Y) = D(X \times Y)$$. Composition is the "kernel composition" $$A \star B = (p_{13})_!(p_{12}^* A \otimes p_{23}^* B)$$.

> **Theorem 5.5.** A morphism $$f: X \to Y$$ is **cohomologically smooth** if and only if there exists an invertible object $$L \in D(X)$$ and maps $$\alpha: R\Gamma_c(X, L) \to \mathbb{1}_Y$$ and $$\beta: \Delta_! \mathbb{1}_X \to p_1^* L$$ satisfying the triangle identities of an adjunction (up to a correction that can always be made — Lemma 5.6). In that case, $$f^! \cong f^* \otimes L$$ and $$L = f^! \mathbb{1}_Y$$ is the **dualizing complex**.

The key simplification (Lemma 5.6/5.11): one does _not_ need the triangle identities to hold on the nose — it suffices that the composites are isomorphisms (not necessarily the identity). The correction to a genuine adjunction is then automatic. This "Lax Zorro" lemma drastically simplifies verification in practice.

**Example:** For $$f: \mathbb{R} \to *$$ and $$D = D(\mathrm{Ab}(-))$$, the dualizing complex is $$\mathbb{Z}[1]$$. The trace map $$\alpha$$ comes from $$R\Gamma_c(\mathbb{R}, \mathbb{Z}[1]) \cong \mathbb{Z}$$. The coproduct $$\beta$$ comes from the computation $$R\mathcal{H}\mathrm{om}(\Delta_! \mathbb{Z}[-1], \mathbb{Z}) \cong \mathbb{Z}$$ via the triangle $$\Delta_! \mathbb{Z} \to \mathbb{Z} \to j_! \mathbb{Z}$$ for $$j: \mathbb{R}^2 \setminus \Delta \hookrightarrow \mathbb{R}^2$$.

## The Ring Stack Phenomenon

A recurring observation, first due to Drinfeld, is that most six-functor formalisms factor through a **geometric intermediary**:

$$\mathcal{C} \xrightarrow{F} \{\text{analytic stacks}\} \xrightarrow{D_{\mathrm{qc}}} \mathrm{Cat}_\infty$$

The first functor $$F$$ is a "transmutation" $$X \mapsto X_R$$ determined by a **ring stack** $$R$$ — a commutative ring object in the category of stacks over some base.

> **Theorem 10.6.** A $$k$$-algebra stack $$R$$ yields a transmutation if: (1) $$R$$ is cohomologically smooth with trivial homology; (2) the zero section is a closed immersion; (3) the units are open; and (4) excision holds.

The examples are striking:

- **The Betti stack** $$X_{\mathrm{Betti}}$$: For a topological space $$X$$, the functor $$S \mapsto \mathrm{Cont}(\lvert S \rvert, X)$$ on schemes gives a pro-étale algebraic space, and $$D(X, \mathbb{Z}) \cong D_{\mathrm{qc}}(X_{\mathrm{Betti}})$$. This is Exercise 1.7 in the notes — the six-functor formalism on topological spaces _is_ the quasicoherent sheaf theory on Betti stacks.

- **The de Rham stack** $$X_{\mathrm{dR}}$$: Simpson's de Rham stack, the quotient $$X/(X \subset X \times X)^{\wedge}$$. Quasicoherent sheaves on $$X_{\mathrm{dR}}$$ recover algebraic D-modules.

- **The crystalline stack**: Recovers crystalline cohomology.

- **Analytic de Rham stacks** of Fargues–Fontaine curves: New $$p$$-adic examples.

In each case, the ring stack encodes the "coefficients" of the cohomology theory, and the six-functor formalism is a consequence of the geometry of the stack.

## Solid Modules as a Six-Functor Formalism

Lecture IX develops the six-functor formalism for **solid modules** — the condensed/analytic framework from [the Analytic Geometry lectures](/blog/2026/analytic-geometry-scholze-clausen/). A curious phenomenon arises that Scholze flags explicitly:

> "Something weird happens here": for coherent sheaves, it is the _proper_ maps where $$f_*$$ admits a left adjoint, not the open immersions — the opposite of the topological and étale settings.

In the topological setting, open immersions $$j$$ give $$j_! = j_*$$ (extension by zero), and proper maps give $$f_! = f_*$$. For coherent sheaves, open immersions $$j^* = -\otimes_A A[1/f]$$ do _not_ commute with products, so $$j^*$$ has no left adjoint — the roles of "proper" and "open" are swapped. The solid formalism (working with condensed modules) resolves this by making completions and localizations behave algebraically.

> **Theorem 9.17.** In the solid formalism, any smooth map $$f: X \to Y$$ is cohomologically smooth with dualizing complex $$\Omega^d_{X/Y}[d]$$. For lci maps, $$f^!\mathbb{1} = \det(L_{X/Y})$$ is invertible.

The payoff (Theorem 9.18): finiteness of coherent cohomology — proper pushforward preserves pseudocoherent and coherent objects — follows _formally_ from the abstract duality theory plus the cohomological smoothness of $$\mathbb{A}^1_{\mathbb{Z}} \to \mathrm{Spec}(\mathbb{Z})$$ in the solid formalism.

## Motivic Sheaves: The Universal Example

Lecture XI gives the following remarkable characterization:

> **Theorem 11.1** (Drew–Gallauer). _The $$\infty$$-category of stable presentable 6-functor formalisms on $$\mathrm{Sch}/k$$ satisfying (1) smooth maps are cohomologically smooth, (2) excision, and (3) $$\mathbb{A}^1$$-triviality, has an **initial object**: the Morel–Voevodsky stable homotopy category $$\mathrm{SH}_k$$._

In other words: $$\mathrm{SH}_k$$ is the _universal_ six-functor formalism. Any ring stack satisfying the conditions of Theorem 10.6 receives a unique map from $$\mathrm{SH}$$ (Corollary 11.3). This gives motivic sheaves their definitive characterization — not as a construction, but as a universal property.

Aoki's thesis (announced in these notes) makes this even sharper:

> **Theorem 11.4** (Aoki). _The presentable symmetric monoidal $$(\infty,2)$$-category $$\mathrm{Pr}_{\mathrm{SH}_k}$$ is the initial such equipped with a $$k$$-algebra object $$[\mathbb{A}^1]$$ that is cohomologically smooth with trivial homology, has 0 as closed subset, units as open, and satisfies excision._

Ring stacks satisfying Theorem 10.6 are precisely the "Tannakian points" of the higher category of motivic sheaves.

## Arithmetic D-modules via Tempered Power Series

The final lecture (XII) is the most speculative and perhaps the most exciting. Scholze constructs a new cohomology theory — **arithmetic D-modules** — by building a "tempered" ring stack over $$\mathbb{Q}_p^{\blacksquare}$$.

The construction uses power series with controlled growth:

- **Tempered open disk**: $$\mathcal{D}^{\circ,\mathrm{temp}}_{\mathbb{Q}_p}$$ consists of power series $$\sum a_n T^n$$ with $$\lvert a_n \rvert$$ of at most polynomial growth in $$n$$.
- **Tempered closed disk**: $$\mathcal{D}^{\mathrm{temp}}_{\mathbb{Q}_p} = \mathrm{colim}_{k > 0}$$ of power series with $$\lvert a_n \rvert = o(n^{-k})$$.
- **Ring stack**: $$(\mathbb{A}^1_{\mathbb{F}_p})^{\mathrm{temp}} = \mathcal{D}^{\mathrm{temp}}_{\mathbb{Q}_p} / \mathcal{D}^{\circ,\mathrm{temp}}_{\mathbb{Q}_p}$$, an $$\mathbb{F}_p$$-algebra stack over $$\mathbb{Q}_p^{\blacksquare}$$.

This ring stack satisfies the conditions of Theorem 10.6, yielding a transmutation $$X \mapsto X^{\mathrm{temp}}$$ and a realization $$\mathrm{SH} \to D^{\blacksquare}(X^{\mathrm{temp}})$$ that factors through arithmetic D-modules.

The motivation for "tempered": the logarithm $$\log(1+x) = \sum (-1)^{n+1} x^n/n$$ and polylogarithms $$\mathrm{Li}_k(x) = \sum x^n/n^k$$ have coefficients of polynomial growth in $$n$$, matching the convergence conditions of arithmetic D-modules with Frobenius structure. The tempered growth condition is precisely what makes these functions — and the cohomology theories they represent — fit into the six-functor framework.

## The View from $$\mathcal{C} = *$$

The notes end with a delightful section on what happens when $$\mathcal{C}$$ is just a point. A 3-functor formalism is then a single symmetric monoidal $$\infty$$-category $$D$$. Extending to stacks gives $$D(X) = \mathrm{Fun}(X, D)$$ — parametrized spectra. The notion of "cohomologically étale" becomes: all $$n$$-truncated maps with finite homotopy group fibers.

The deepest result in this toy case is Hopkins–Lurie **ambidexterity** (Theorem 13.3): for $$K(n)$$-local spectra, any $$n$$-truncated map with finite $$\pi_i$$ fibers ($$0 \leq i \leq n$$) is simultaneously cohomologically étale _and_ cohomologically proper — the proper and étale directions coincide. Ambidexterity is the chromatic homotopy theory analogue of the "weird" role reversal Scholze noted for coherent sheaves.

## What the Notes Achieve

These notes accomplish three things:

1. **A clean definition.** The notion of a six-functor formalism, which resisted formalization for 50 years, is captured in a single sentence: a lax symmetric monoidal functor on correspondences, with right adjoints. All coherences are automatic.

2. **A uniform proof of duality.** Poincaré–Verdier duality, in all its incarnations, reduces to checking that a specific map is an adjunction in the kernel 2-category — and the Lax Zorro lemma makes even this check forgiving.

3. **A geometric picture.** Six-functor formalisms are not arbitrary categorical data — they arise from ring stacks, and the motivic stable homotopy category is the universal source. This gives every cohomology theory a geometric home.

The classification is far from complete. The tempered arithmetic D-modules of Lecture XII are a hint of what remains to be built. But the language is now precise enough that the remaining questions are mathematical, not foundational.

---

_The notes are available at [https://people.mpim-bonn.mpg.de/scholze/SixFunctors.pdf](https://people.mpim-bonn.mpg.de/scholze/SixFunctors.pdf). For a more detailed treatment, Scholze recommends Heyer–Mann [arXiv:2410.13038](https://arxiv.org/abs/2410.13038)._
