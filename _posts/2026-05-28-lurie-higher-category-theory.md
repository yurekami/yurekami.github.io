---
layout: post
title: "The Infrastructure of Modern Mathematics: Lurie's Higher Category Theory"
slug: lurie-higher-category-theory
date: 2026-05-28
description: A survey of Jacob Lurie's foundational program — Higher Topos Theory, Higher Algebra, and Spectral Algebraic Geometry — the 3,000+ pages of infrastructure that make condensed mathematics, six-functor formalisms, geometric Langlands, and p-adic Hodge theory possible.
tags: [mathematics, category-theory, homotopy-theory, algebraic-geometry]
categories: [research]
giscus_comments: true
related_posts: true
toc:
  sidebar: left
---

Every mathematical revolution needs infrastructure, and the infrastructure for the revolution documented across today's posts — [condensed mathematics](/blog/2026/condensed-mathematics-scholze/), [analytic geometry](/blog/2026/analytic-geometry-scholze-clausen/), [six-functor formalisms](/blog/2026/six-functor-formalisms-scholze/), [geometric Langlands](/blog/2026/geometric-langlands-scholze/), [p-adic Hodge theory](/blog/2026/p-adic-hodge-theory-bhatt/) — was built, in large part, by one person.

Jacob Lurie's homepage at the Institute for Advanced Study[^page] lists a body of work that is, by any measure, one of the most extraordinary individual contributions to mathematics in the 21st century: three massive books, a dozen major papers, lecture notes for a half-dozen courses, and an online textbook project — totaling well over 3,000 pages — that together provide the foundational language in which a generation of mathematics is being written.

[^page]: Jacob Lurie's homepage at IAS: [https://www.math.ias.edu/~lurie/](https://www.math.ias.edu/~lurie/).

This post is not a reading of a single document but an attempt to explain why this body of work exists, what it contains, and why it matters — told through the lens of the mathematics it enables.

## The Problem: Higher Coherences

The problem Lurie's work solves can be stated simply: **how do you do algebra when equalities are replaced by isomorphisms, and isomorphisms are replaced by higher isomorphisms, all the way up?**

In classical mathematics, two objects are either equal or not. In category theory, the right notion is isomorphism — objects can be "the same" without being identical. But isomorphisms themselves can be isomorphic (a natural transformation between functors is an "isomorphism between isomorphisms"), and *those* can be isomorphic too, ad infinitum.

This tower of "higher coherences" arises naturally whenever homotopy theory touches algebra. A topological space has not just a set of points but a fundamental groupoid (paths between points, homotopies between paths, homotopies between homotopies...). A derived category has not just morphisms but chain homotopies, homotopies between chain homotopies, and so on. A symmetric monoidal category has not just an associativity isomorphism $$(A \otimes B) \otimes C \cong A \otimes (B \otimes C)$$ but a pentagon of coherence isomorphisms, and then higher pentagons, forever.

Before Lurie, the standard approach was to either truncate (work with 1-categories and ignore the higher structure) or to use *ad hoc* models (model categories, simplicial categories, Segal spaces) that each captured some of the structure but none captured all of it in a way that was simultaneously rigorous, general, and usable.

## The Trilogy

Lurie's foundational program consists of three books, each building on the previous:

### Higher Topos Theory (HTT, 2009)[^htt]

949 pages. Published by Princeton University Press as Annals of Mathematics Studies #170.

[^htt]: Jacob Lurie, *Higher Topos Theory*, Annals of Mathematics Studies 170, Princeton University Press (2009). Updated April 2017. [PDF](https://www.math.ias.edu/~lurie/papers/HTT.pdf).

HTT develops the theory of **$$(\infty,1)$$-categories** — higher categories in which all $$k$$-morphisms for $$k > 1$$ are invertible. The model is **quasi-categories** (a.k.a. weak Kan complexes): simplicial sets satisfying a weakened horn-filling condition. This is the "operating system" on which everything else runs.

The book opens with one of the clearest motivations in all of mathematics. Lurie asks: what is the cohomology class $$\eta \in H^n(X; G)$$? For $$n = 1$$, it classifies $$G$$-torsors — a purely sheaf-theoretic notion that makes sense on any topos. For $$n = 2$$, it classifies gerbes — "groupoid-valued sheaves." For general $$n$$, it should classify $$n$$-stacks. But to make this precise, you need a theory of sheaves valued not in sets but in $$n$$-types — and the natural home for such sheaves is an $$\infty$$-category.

The main result is an **$$\infty$$-categorical Giraud theorem** (Theorem 6.1.0.6): characterizing $$\infty$$-topoi (the $$\infty$$-categorical analogues of Grothendieck topoi) by intrinsic properties — descent, colimit conditions, generation by a set of objects — exactly paralleling the classical theorem. An $$\infty$$-topos is "a world in which one can do homotopy theory," just as a classical topos is a world in which one can do set-theoretic mathematics.

Along the way, HTT develops the entire categorical toolkit: limits and colimits, adjoint functors, presheaves, accessible and presentable categories, Kan extensions, localization — all in the $$\infty$$-categorical setting.

### Higher Algebra (HA, 2017)[^ha]

1553 pages (unpublished, but universally cited).

[^ha]: Jacob Lurie, *Higher Algebra*, updated September 2017. [PDF](https://www.math.ias.edu/~lurie/papers/HA.pdf).

Where HTT develops the "topology" of $$\infty$$-categories (limits, colimits, descent), HA develops the "algebra": **$$\infty$$-operads**, **stable $$\infty$$-categories**, **ring spectra**, and **modules** over them.

The key concept is the **stable $$\infty$$-category** — the $$\infty$$-categorical analogue of an abelian category, where the role of chain complexes is absorbed into the categorical structure itself. The derived category of an abelian category is a stable $$\infty$$-category; but so are the categories of spectra in stable homotopy theory, the categories of modules over ring spectra, and the categories that arise in Scholze's condensed mathematics.

HA also develops the theory of **symmetric monoidal $$\infty$$-categories** and **commutative algebra objects** in them. This is where the notion of a "ring" is generalized to the $$\infty$$-categorical setting — an $$\mathbb{E}_\infty$$-ring is a commutative ring up to all higher coherences. The classical notion of a commutative ring is recovered as the special case of a *discrete* $$\mathbb{E}_\infty$$-ring.

This is the book that makes the following sentence from Scholze's [six-functor formalism notes](/blog/2026/six-functor-formalisms-scholze/) possible: "a commutative algebra in categories is the same thing as a symmetric monoidal category." That sentence encodes months of foundational work, all of which is done in HA.

### Spectral Algebraic Geometry (SAG, 2018, ~67% complete)[^sag]

~1700 pages (and growing). Not yet published; approximately two-thirds complete as of February 2018.

[^sag]: Jacob Lurie, *Spectral Algebraic Geometry (Under Construction)*, updated February 2018. [PDF](https://www.math.ias.edu/~lurie/papers/SAG-rootfile.pdf).

SAG builds algebraic geometry on the foundations of HA — replacing commutative rings with $$\mathbb{E}_\infty$$-ring spectra, and schemes with **spectral schemes**. The resulting theory subsumes classical algebraic geometry (which is recovered by restricting to discrete rings) and derived algebraic geometry (which uses simplicial commutative rings), while also encompassing the "brave new algebra" of stable homotopy theory.

SAG is the book that makes the notion of "derived algebraic geometry" rigorous at the level of generality needed for modern applications: the stack of local systems in the [geometric Langlands correspondence](/blog/2026/geometric-langlands-scholze/) lives in derived algebraic geometry. The analytic rings of [condensed mathematics](/blog/2026/condensed-mathematics-scholze/) are animated commutative rings — objects defined using the language of SAG.

## The Supporting Cast

Beyond the trilogy, Lurie's homepage lists works that have each independently reshaped their fields:

- **Elliptic Cohomology I–III**: A construction of the derived moduli stack of elliptic curves, solving a problem that motivated much of the development of derived algebraic geometry.

- **Ambidexterity in $$K(n)$$-local stable homotopy theory** (with Mike Hopkins): The result that appears in Scholze's six-functor notes as Theorem 13.3 — the statement that for chromatic homotopy theory, proper and étale coincide.

- **A Riemann-Hilbert correspondence in characteristic $$p$$** (with Bhargav Bhatt): The foundational result that Bhatt's [p-adic Hodge theory lectures](/blog/2026/p-adic-hodge-theory-bhatt/) build on — the characteristic $$p$$ model for the mixed-characteristic Riemann-Hilbert functor.

- **Revisiting the de Rham-Witt complex** (with Bhatt and Mathew): Foundational for prismatic cohomology.

- **Kerodon** ([kerodon.net](https://kerodon.net)): An online, open-source textbook on $$\infty$$-category theory, intended as the definitive reference — a kind of Stacks Project for higher category theory.

## Why It Matters: The Language Tax

Mathematics has a "language tax": before you can state a theorem, you need a language in which to state it, and before you can prove it, you need foundational results in that language. Lurie's work pays this tax for an entire generation.

Consider what is needed to even *state* the geometric Langlands conjecture:

- $$D_{\mathrm{Nilp}}(\mathrm{BunG})$$ — a category of sheaves with nilpotent singular support on an Artin stack. Defining this requires: stable $$\infty$$-categories (HA), descent for sheaves on stacks (HTT), the notion of singular support in the derived setting.

- $$\mathrm{IndCoh}_{\mathrm{Nilp}}(\mathrm{LocSys}_{\check{G}})$$ — ind-coherent sheaves on a derived stack. Defining this requires: the theory of ind-coherent sheaves (developed by Gaitsgory–Rozenblyum using the language of HA/SAG), derived algebraic geometry (SAG), the notion of singular support for coherent sheaves.

- The equivalence itself is an equivalence of $$D_{\mathrm{qc}}(\mathrm{LocSys}_{\check{G}})$$-**linear** $$(\infty,1)$$-categories — a module over a symmetric monoidal $$\infty$$-category. This notion only makes sense with the full machinery of HA.

Without Lurie's foundations, the conjecture cannot be precisely stated, let alone proved. The same is true for Scholze's six-functor formalisms (which are functors on $$\infty$$-categories of correspondences), Clausen-Scholze's analytic geometry (which uses animated commutative rings and the derived $$\infty$$-category throughout), and Bhatt's p-adic Hodge theory (which uses the language of $$\infty$$-categories at every turn).

Scholze says as much in his [geometric Langlands report](/blog/2026/geometric-langlands-scholze/): "The possibility of doing [algebra with categories] relies on Lurie's foundational works, in particular [HA]."

## The Style

Lurie's writing has a distinctive character. The books are long — forbiddingly so — but they are not verbose. Every page earns its place. The proofs are complete, the definitions are precise, and the logical dependencies are tracked with unusual care. There are very few forward references and essentially no gaps.

The cost is that the material is dense. HTT is 949 pages; HA is 1553; SAG is ~1700 and growing. Reading them cover-to-cover is a multi-year project that few have completed. But this is partly a reflection of the subject: the tower of higher coherences that $$\infty$$-category theory manages is genuinely infinite, and managing it requires genuine work.

The benefit is that mathematicians who need a specific result can find it, precisely stated and fully proved, in a single reference. This has made Lurie's books the de facto foundations for a generation of algebraic geometry, algebraic topology, and number theory — cited by essentially every major paper in these fields since 2010.

## The Kerodon Project

Lurie's most recent major effort is **Kerodon** ([kerodon.net](https://kerodon.net)) — an open-source, collaboratively maintained online textbook on $$\infty$$-category theory. Kerodon is modeled on the Stacks Project: a living document with a tag system, cross-references, and a web-based interface.

The ambition is to provide a definitive, up-to-date reference that supersedes the now-aging versions of HTT and HA on Lurie's webpage. It is a recognition that a 949-page book published in 2009 (even if updated in 2017) cannot be the permanent foundation for a rapidly evolving field — the material needs to be alive, correctable, and extensible.

## Coda

Every post in today's series cites Lurie's work, most of them multiple times:

- [Condensed Mathematics](/blog/2026/condensed-mathematics-scholze/): the derived category $$D(\mathrm{Cond}(\mathrm{Ab}))$$ is a stable $$\infty$$-category (HA)
- [Analytic Geometry](/blog/2026/analytic-geometry-scholze-clausen/): analytic rings use animated commutative rings (SAG)
- [Six-Functor Formalisms](/blog/2026/six-functor-formalisms-scholze/): correspondences, symmetric monoidal $$\infty$$-categories, presentable categories (HTT + HA)
- [Geometric Langlands](/blog/2026/geometric-langlands-scholze/): the entire proof is "algebra with categories" in the sense of HA
- [p-adic Hodge Theory](/blog/2026/p-adic-hodge-theory-bhatt/): the Bhatt-Lurie Riemann-Hilbert functor in characteristic $$p$$ is the model case

This is the pattern of a foundational contribution: the work is invisible at the surface — nobody reads a paper "about" $$\infty$$-categories — but it is load-bearing everywhere underneath.

Lurie has built the language in which 21st-century mathematics speaks. The posts today are reports from the frontier of what that language can say.

---

_Lurie's works are freely available at [https://www.math.ias.edu/~lurie/](https://www.math.ias.edu/~lurie/). Kerodon, the online textbook project, is at [https://kerodon.net](https://kerodon.net)._
