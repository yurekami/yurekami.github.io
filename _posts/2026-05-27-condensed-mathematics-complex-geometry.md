---
layout: post
title: "Rebuilding Complex Geometry from Scratch: Clausen & Scholze's Condensed Approach"
slug: condensed-mathematics-complex-geometry
date: 2026-05-27
description: A reading of Clausen and Scholze's 148-page lecture notes that reprove the pillars of complex geometry — finiteness of coherent cohomology, Serre duality, GAGA, and Grothendieck–Hirzebruch–Riemann–Roch — from condensed mathematics, revealing why this framework may be the right foundation for all flavors of geometry.
tags: [mathematics, algebraic-geometry, condensed-mathematics]
categories: [research]
giscus_comments: true
related_posts: true
toc:
  sidebar: left
---

What if the foundations of complex geometry — theorems that have stood for half a century — could be reproved more cleanly, more naturally, from a radically different starting point? That's exactly what Dustin Clausen and Peter Scholze set out to do in their 148-page lecture notes, _Condensed Mathematics and Complex Geometry_,[^paper] now posted as a stable citable version of their 2022 joint Bonn–Copenhagen summer course.

[^paper]: Dustin Clausen and Peter Scholze, "Condensed Mathematics and Complex Geometry," [arXiv:2605.11731](https://arxiv.org/abs/2605.11731v1) (May 2026). 148 pages.

This isn't a paper that claims new geometric results. It does something arguably more ambitious: it rebuilds the classical theory of compact complex manifolds from first principles using **condensed mathematics**, a framework that replaces the standard topological toolkit with something categorically superior. The theorems are the same — finiteness of coherent cohomology, Serre duality, GAGA, Grothendieck–Hirzebruch–Riemann–Roch — but the _way_ they fall out of the new framework reveals why condensed mathematics is more than a curiosity. It's a better language.

## The Problem with Topology (Yes, Really)

Here's a dirty secret of modern mathematics: the category of topological abelian groups is broken.

Not broken in the sense that it gives wrong answers — it gives correct answers just fine. Broken in the sense that it lacks the structural properties mathematicians need to do _algebra_ on _topological_ objects. Specifically, topological abelian groups don't form an **abelian category**. This means you can't freely take kernels, cokernels, exact sequences, or do homological algebra — the bread and butter of modern algebraic geometry and number theory.

For decades, mathematicians worked around this with a patchwork of ad hoc constructions: Fréchet spaces here, nuclear spaces there, careful functional analysis arguments everywhere. It worked, but it was ugly. Every time you wanted to apply an algebraic machine to an analytic object, you had to hand-roll the interface.

Condensed mathematics says: what if we just fix the category?

## What Is a Condensed Set?

The central move is deceptively simple. Instead of equipping a set with a topology (open sets, continuity, the whole classical apparatus), you encode the "topological" information differently.

A **condensed set** is a sheaf on the category of profinite sets (compact, totally disconnected Hausdorff spaces) — equipped with the Grothendieck topology of finite jointly surjective families. In concrete terms: rather than asking "which subsets are open?", you ask "what are the continuous maps from every profinite set into this thing?"

Every topological space gives rise to a condensed set (send a profinite set $$S$$ to the set of continuous maps $$S \to X$$). But the category of condensed sets is vastly better behaved. Crucially:

- **Condensed abelian groups form an abelian category.** You get kernels, cokernels, exact sequences, derived functors — the full homological algebra toolkit, for free.
- **Condensed rings and modules** work equally well. You can do commutative algebra over topological rings without ever leaving the formalism.
- **No pathologies.** The categorical properties just work, without the counterexamples and caveats that plague classical topological algebra.

Kiran Kedlaya described this neatly as "technology for doing commutative algebra over topological rings."[^kedlaya] That's an understatement, but it captures the flavor.

[^kedlaya]: Kiran Kedlaya, in his [review](https://en.wikipedia.org/wiki/Condensed_mathematics) of the condensed mathematics program.

## From Condensed Sets to Analytic Geometry

The paper doesn't stop at condensed sets. To do geometry, you need more structure:

**Solid modules** provide the right notion of "complete" modules in the condensed world. They form a full subcategory of condensed modules with excellent properties — think of them as the condensed analogue of complete topological vector spaces, but without the categorical defects. Solid modules enable derived categories that actually behave as algebraic geometers expect.

**Analytic rings** are the condensed version of rings equipped with analytic structure. They provide the foundation on which you can build genuine analytic geometry — not as a parallel theory to algebraic geometry, but as a _specialization_ of a common framework.

**Liquid vector spaces** — the concept whose proof was formally verified in the Liquid Tensor Experiment[^liquid] using Lean — serve as the condensed alternative to complete topological vector spaces. They retain the analytic content of functional analysis while living in a well-behaved abelian category.

[^liquid]: The Liquid Tensor Experiment: when Scholze completed the proofs incorporating functional analysis via liquid vector spaces in 2020, the arguments were subtle enough that he publicly requested formal verification. A team led by Johan Commelin spent six months formalizing the central proof in Lean, completing it in July 2022. This is one of the few times a Fields medalist has _asked_ the proof assistant community to check their work — and the proof survived. See the [Lean community project page](https://leanprover-community.github.io/liquid/) for details.

## Four Classical Theorems, One New Language

The heart of the paper is reproof. Four pillars of complex geometry, rebuilt from condensed foundations:

### 1. Finiteness of Coherent Cohomology

**Classical statement:** For a compact complex manifold $$X$$ and a coherent sheaf $$\mathcal{F}$$ on $$X$$, the cohomology groups $$H^q(X, \mathcal{F})$$ are finite-dimensional complex vector spaces.

This is a foundational finiteness result — it says that the topological complexity of coherent sheaves on compact spaces is controlled. The classical proof requires careful functional analysis (Fréchet spaces, open mapping theorems, Schwartz's theorem on compact operators). In the condensed framework, the finiteness falls out more naturally from the categorical machinery of solid modules, because the derived category already "knows about" the analytic structure.

### 2. Serre Duality

**Classical statement:** On a compact complex manifold of dimension $$n$$, there's a natural perfect pairing between $$H^q(X, \mathcal{F})$$ and $$H^{n-q}(X, \mathcal{F}^\vee \otimes \omega_X)$$, where $$\omega_X$$ is the canonical bundle.

Serre duality is the complex-geometric analogue of Poincaré duality — it says cohomology has a symmetry, with the canonical bundle playing the role of the "dualizing object." The condensed reproof leverages the fact that duality in a well-behaved abelian category is a formal consequence of having the right adjunctions, rather than requiring an independent analytic argument.

### 3. GAGA (Géométrie Algébrique et Géométrie Analytique)

**Classical statement:** For a projective complex algebraic variety $$X$$, the categories of coherent algebraic sheaves and coherent analytic sheaves are equivalent, and their cohomologies agree.

Serre's 1956 GAGA theorem[^serre] is one of the great bridge theorems of 20th-century mathematics — it says that for projective varieties, algebraic geometry and complex analytic geometry are the same thing. The condensed approach is particularly illuminating here because both the algebraic and analytic sides live in the _same_ categorical framework from the start. The equivalence becomes less of a miracle and more of a natural consequence of the formalism.

[^serre]: Jean-Pierre Serre, "Géométrie algébrique et géométrie analytique," _Annales de l'Institut Fourier_ **6** (1956), 1–42.

### 4. Grothendieck–Hirzebruch–Riemann–Roch

**Classical statement:** The Euler characteristic of a coherent sheaf on a smooth projective variety can be computed via characteristic classes — specifically, it equals the integral of $$\operatorname{ch}(\mathcal{F}) \cdot \operatorname{td}(X)$$ over $$X$$.

This is the grand unification theorem of classical algebraic geometry, connecting sheaf cohomology (algebra) to characteristic classes (topology) to intersection theory (geometry). The condensed reproof goes through the K-theory of the condensed framework, with solid modules providing the right categorical substrate for the necessary trace maps and Chern character constructions.

## Why Reproof Matters

A skeptic might ask: if the theorems are the same, why bother?

Three reasons.

**Conceptual clarity.** The condensed proofs reveal _why_ these theorems are true in a way the classical proofs obscure. When finiteness of coherent cohomology falls out of abstract categorical properties rather than hard analysis, you understand something deeper about the relationship between compactness and finiteness. The proofs become _explanations_.

**Unification.** The same condensed framework applies to $$p$$-adic geometry, non-Archimedean geometry, algebraic geometry, and now complex analytic geometry. Classical proofs of these four theorems use completely different techniques in the algebraic vs. analytic vs. $$p$$-adic settings. Condensed mathematics gives you one proof strategy for all of them. This is Scholze's long game — a unified framework for all flavors of geometry.

**Future foundations.** When you rebuild classical results in a new language and everything works, you've validated the language. The next step is to use that language to prove things that _couldn't_ be proved before. The condensed framework has already enabled new results in $$p$$-adic Hodge theory and $$p$$-adic geometry; having the complex-analytic case worked out in equal detail means the framework is ready for problems that straddle multiple geometric worlds.

## The Bigger Picture

Clausen and Scholze's project — of which this paper is one concrete manifestation — represents something rare in mathematics: a genuine shift in **foundations**. Not a new theorem, but a new way of _organizing_ theorems. The last time something comparable happened in algebraic geometry was Grothendieck's introduction of schemes in the 1960s, which similarly rebuilt classical algebraic geometry in a new language before extending it to territory the old language couldn't reach.

The analogy isn't perfect — schemes replaced varieties, while condensed sets don't _replace_ topological spaces so much as _refine_ them for algebraic purposes. But the structural move is the same: find the right categorical framework, rebuild the classics to validate it, then go further.

With this paper, the complex-analytic case is now solidly in hand. The four theorems proved here are among the most important in complex geometry, and the fact that they all admit clean condensed proofs is strong evidence that this framework is the right one.

## Practical Implications

If you work in:

- **Algebraic geometry**: The GAGA reproof gives a template for condensed comparison theorems. Expect more bridge results.
- **Complex analysis / several complex variables**: The finiteness and duality results may simplify arguments that currently require heavy functional analysis.
- **Number theory / arithmetic geometry**: The same framework already handles $$p$$-adic geometry. Having the Archimedean side worked out in parallel is essential for global (number field) applications.
- **K-theory / topology**: The Grothendieck–Hirzebruch–Riemann–Roch reproof shows how K-theoretic tools adapt to the condensed setting.
- **Formal verification**: The Liquid Tensor Experiment showed that core condensed mathematics is formalizable. These 148 pages are a natural next target for Lean formalization.

## Read the Paper

[arXiv:2605.11731v1](https://arxiv.org/abs/2605.11731v1) — _Condensed Mathematics and Complex Geometry_, Dustin Clausen and Peter Scholze, 148 pages.

The paper is structured as 15 lectures, making it more accessible than a typical research monograph. Lectures I–V cover condensed foundations, VI–X develop solid modules and analytic rings, and XI–XV apply everything to complex geometry. If you already know condensed basics, you can jump to Lecture XI.
