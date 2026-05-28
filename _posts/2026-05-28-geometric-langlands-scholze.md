---
layout: post
title: "The Langlands Program Proved (Geometrically): Scholze's Bourbaki Report"
slug: geometric-langlands-scholze
date: 2026-05-28
description: A reading of Peter Scholze's 56-page Séminaire Bourbaki report on the proof of the geometric Langlands conjecture by Gaitsgory, Raskin, and collaborators — from the Rosetta Stone of number theory to algebra with categories, the spectral action, and what it all means for the original arithmetic questions.
tags: [mathematics, algebraic-geometry, langlands-program, number-theory]
categories: [research]
giscus_comments: true
related_posts: true
toc:
  sidebar: left
---

When Peter Scholze says he couldn't understand something as a graduate student, you pay attention. In his Séminaire Bourbaki report on the proof of the geometric Langlands conjecture,[^scholze] he opens with a confession that is remarkable coming from one of the most technically powerful mathematicians alive:

> When I was a graduate student, working in the arithmetic Langlands program, I was not able to understand what the structures investigated in the geometric Langlands program are, why one should be interested in them, and how they relate to the questions of original interest.

[^scholze]: Peter Scholze, "Geometric Langlands [after Gaitsgory, Raskin, ...]," Séminaire Bourbaki, Mars 2026, 78e année, 2025–2026, n° 1252. Available at [https://people.mpim-bonn.mpg.de/scholze/Exp1252_Scholze.pdf](https://people.mpim-bonn.mpg.de/scholze/Exp1252_Scholze.pdf). 56 pages.

That has changed. "Over the last couple of years, the situation has changed drastically," Scholze writes. The geometric Langlands conjecture — a decades-old prediction relating two categories of mathematical objects on a smooth projective curve — has been proved. And the geometric result has been brought back to the arithmetic world that Langlands originally cared about, giving precise answers to some of the most basic questions in the theory of automorphic forms over function fields.

This report is Scholze's attempt to explain the proof — by Gaitsgory, Raskin, and a constellation of collaborators (Arinkin, Beraldo, Campbell, Chen, Færgeman, Lin, Rozenblyum, Varshavsky) across a series of five papers — to the mathematical community at large. At 56 pages, it is itself a substantial work of exposition. What follows is my attempt to trace the main line of the argument and highlight what makes it remarkable.

## The Central Problem

The Langlands program begins with an observation so natural it barely seems like mathematics. Some of the most beautiful spaces in geometry — hyperbolic surfaces, locally symmetric spaces — arise by taking a model geometry $$M$$ (say the hyperbolic plane) and quotienting by a discrete group $$\Gamma$$ of symmetries (say $$\mathrm{SL}_2(\mathbb{Z})$$). The resulting space $$M/\Gamma$$ is a locally symmetric space.

Now consider the vector space $$\mathcal{A}(M/\Gamma)$$ of "nice functions" on this space. This is the space of **automorphic functions** — just functions on a geometric object. The Laplacian acts on this space (from the differential geometry side), and so do **Hecke operators** (from the number-theoretic side, exploiting the arithmetic structure of $$\Gamma$$). These operators commute, yielding a large commutative algebra $$\mathbb{T}$$ acting on $$\mathcal{A}(M/\Gamma)$$.

> **Problem 1.2** (The central problem of the Langlands program): _Describe the vector space $$\mathcal{A}(M/\Gamma)$$, equipped with the action of $$\mathbb{T}$$._

Langlands' approach: decompose $$\mathcal{A}(M/\Gamma)$$ spectrally as a $$\mathbb{T}$$-module. What are the possible systems of eigenvalues $$\psi: \mathbb{T} \to \mathbb{C}$$ that appear?

The **Satake isomorphism** gives the Hecke algebra at each prime $$p$$ the form $$\mathcal{O}(\check{G}/\mathrm{ad}\,\check{G})$$ — conjugation-invariant functions on the **Langlands dual group** $$\check{G}$$. So each eigenvalue system $$\psi$$ produces, at each prime $$p$$, a conjugacy class $$\psi_p \in \check{G}(\mathbb{C})$$. Two deep conjectures govern what collections $$\{\psi_p\}$$ can actually appear:

- **Ramanujan**: Each $$\psi_p$$ lies in a compact subgroup of $$\check{G}(\mathbb{C})$$. (For $$G = \mathrm{SL}_2$$, this predicts the spectral gap $$\geq 1/4$$ for the Laplacian on all arithmetic hyperbolic surfaces.)

- **Reciprocity**: There is an irreducible $$\check{G}$$-local system on $$\mathrm{Spec}(\mathbb{Z})$$ whose monodromy at $$p$$ recovers $$\psi_p$$.

Scholze proposes an even more precise formulation (Conjecture 1.5): the entire space of automorphic functions should be isomorphic to the global sections of the **dualizing sheaf** on a stack $$\mathrm{LocSys}_{\check{G}}$$ parametrizing $$\check{G}$$-local systems. This would simultaneously encode both the "automorphic-to-Galois" direction (the spectral support) and the "Galois-to-automorphic" direction (the multiplicities).

## Weil's Rosetta Stone

None of this is what the paper proves. These are the _original_ questions, over number fields, which remain wide open. But Weil observed long ago that number theory has three parallel incarnations:

1. **Number fields** ($$\mathbb{Z}$$, $$\mathbb{Q}$$, ...) — the original setting
2. **Function fields over finite fields** ($$\mathbb{F}_q[t]$$, curves $$X/\mathbb{F}_q$$) — more tractable
3. **Curves over algebraically closed fields** ($$X/\mathbb{C}$$, Riemann surfaces) — the geometric setting

The Langlands program can be formulated in all three columns. The geometric Langlands conjecture lives in the third column. Its proof, combined with a bridge back to the second column, yields concrete arithmetic results.

In the function field setting, the space $$M/\Gamma$$ becomes $$\mathrm{BunG}(\mathbb{F}_q)$$ — the set of isomorphism classes of $$G$$-bundles on a curve $$X/\mathbb{F}_q$$. This is just a discrete set, so the space of automorphic functions $$\mathcal{A}_c(\mathrm{BunG}(\mathbb{F}_q))$$ is a vector space with no analytic subtleties. But it still carries a Hecke action, and the same structural questions arise.

The landmark result:

> **Theorem 1.7** (Gaitsgory–Raskin, 2025). _Let $$G$$ be a reductive group over $$\mathbb{F}_q$$ with $$p$$ sufficiently large. There is an open and closed substack $$\mathrm{LocSys}'_{\check{G}} \subset \mathrm{LocSys}_{\check{G}}$$ and a canonical $$\mathbb{T}$$-equivariant isomorphism_
>
> $$\mathcal{A}_c(\mathrm{BunG}(\mathbb{F}_q)) \cong \Gamma(\mathrm{LocSys}'_{\check{G}},\; \omega_{\mathrm{LocSys}'_{\check{G}}}).$$

The space of compactly supported automorphic functions _is_ the global sections of the dualizing sheaf on the moduli of local systems. This essentially resolves Conjecture 1.5 in the function field case. Moreover, Raskin has announced joint work with Gaitsgory and V. Lafforgue deducing the **Arthur–Ramanujan conjecture** in this setting.

As Scholze emphasizes: "Thus, at the end of the day, one proves concrete statements about actual numbers, namely eigenvalues of Hecke operators!"

## Categorification: Algebra with Categories

The proof strategy is a four-step program:

1. Formulate the geometric Langlands correspondence as an **equivalence of categories** relating sheaves on $$\mathrm{BunG}$$ and quasi-coherent sheaves on $$\mathrm{LocSys}_{\check{G}}$$.
2. Show that **taking trace of Frobenius** on this categorical equivalence recovers the desired isomorphism of vector spaces.
3. Show the conjecture is essentially **independent of the sheaf theory** (de Rham, Betti, or étale).
4. **Prove** the equivalence in characteristic 0, for D-modules, then deform to positive characteristic.

The key conceptual move is _categorification_. Where the arithmetic Langlands program works with vector spaces and functions, the geometric Langlands program works with **categories and sheaves**. Scholze describes this as "algebra with categories":

> When roughly a century ago one was gradually going from doing algebra with numbers (adding, multiplying, ...) to doing algebra with modules (direct sums, tensor products, ...), here one is doing algebra with categories (which also admit direct sums, tensor products, ...).

The possibility of doing this relies on Lurie's $$(\infty,1)$$-categorical foundations. And the proofs, Scholze notes, "show an extreme virtuosity in doing algebra with categories — all the key steps of the proofs consist in clever definitions and manipulations with categories... It is remarkable and a bit counterintuitive that these manipulations can eventually prove something very concrete, such as the Arthur–Ramanujan conjecture."

## The Geometric Langlands Equivalence

The conjecture, in its geometric form:

> **Conjecture 2.10** (Geometric Langlands). _There is an equivalence of $$D_{\mathrm{qc}}(\mathrm{LocSys}_{\check{G}})$$-linear $$(\infty,1)$$-categories_
>
> $$D_{\mathrm{Nilp}}(\mathrm{BunG}) \cong \mathrm{IndCoh}_{\mathrm{Nilp}}(\mathrm{LocSys}_{\check{G}}).$$

On the left: sheaves on the moduli stack of $$G$$-bundles, with nilpotent singular support. On the right: ind-coherent sheaves on the moduli stack of $$\check{G}$$-local systems, with nilpotent singular support. The equivalence is normalized by sending the structure sheaf on the right to the **Whittaker sheaf** on the left.

Let me unpack the key pieces.

### Sheaves on $$\mathrm{BunG}$$: nilpotent singular support

Not all sheaves on $$\mathrm{BunG}$$ participate in Langlands — one must restrict to the subcategory $$D_{\mathrm{Nilp}}(\mathrm{BunG})$$ of sheaves with **nilpotent singular support**. Singular support is a subset of the cotangent bundle $$T^*\mathrm{BunG}$$, which in this case is the moduli of **Higgs bundles**. "Nilpotent" means the Higgs field lies in the nilpotent cone — the zero fiber of the Hitchin fibration.

A beautiful theorem (Arinkin–Gaitsgory–Kazhdan–Raskin–Rozenblyum–Varshavsky) shows three equivalent characterizations of this subcategory:

1. **Nilpotent singular support** (microlocal condition)
2. **Hecke-lisse**: for all representations $$V$$ of $$\check{G}$$, the Hecke translate is locally constant along the curve (spectral condition)
3. **Ind-prim**: lies in the ind-completion of prim sheaves (categorical adjointness condition)

The equivalence of these conditions — connecting microlocal geometry, representation theory, and categorical algebra — is one of the foundational results underlying the proof.

### The spectral action

The first main theorem (logically, not historically) is that $$D_{\mathrm{qc}}(\mathrm{LocSys}_{\check{G}})$$ **acts** on $$D_{\mathrm{Nilp}}(\mathrm{BunG})$$, refining the Hecke action. This is the categorical analogue of saying the space of automorphic forms lifts from a $$\mathbb{T}$$-module to a module over the stack of local systems — already giving the "automorphic-to-Galois" direction.

The construction uses a phenomenon with no arithmetic analogue: **fusion**. The Hecke operators at different points $$x \in X$$ can be varied in families as $$x$$ moves on the curve. When two points collide, the corresponding Hecke operators fuse. This process — invisible in arithmetic, where primes can't "collide" — is what makes geometric Langlands more tractable than its arithmetic parent.

### The geometric Satake equivalence

The bridge between $$G$$ and its Langlands dual $$\check{G}$$ is the **geometric Satake equivalence** (Mirković–Vilonen): an equivalence of monoidal categories

$$\mathrm{Perv}(\mathrm{Hck}^{\mathrm{loc}}_G) \cong \mathrm{Rep}(\check{G})$$

between perverse sheaves on the local Hecke stack and representations of the dual group. This is a categorification of the classical Satake isomorphism. Crucially, it shows that the perverse sheaves are _miraculously_ symmetric monoidal — the commutativity of Hecke operators, which in arithmetic is a computation, here becomes a consequence of fusion.

In Scholze's view, this is "in fact the correct definition of the Langlands dual group — it is the Tannakian group that arises in the geometric Satake equivalence."

## Three Key Ingredients

The proof of the equivalence, in the de Rham setting for characteristic 0, uses three main ingredients:

### 1. Whittaker conservativity

The **Whittaker sheaf** $$W$$ plays the role of a "base object" — it normalizes the equivalence. Færgeman–Raskin prove that Whittaker coefficients are **conservative** on tempered (e.g. cuspidal) sheaves: if the Whittaker coefficient of a tempered sheaf vanishes, the sheaf itself vanishes. This is the detection mechanism that ensures no cuspidal sheaves are missed.

The Whittaker sheaf is constructed from the Artin–Schreier sheaf (or exponential D-module) via the correspondence

$$\mathrm{Bun}^{\Theta}_U \xrightarrow{p} \mathrm{BunG}, \quad \mathrm{Bun}^{\Theta}_U \xrightarrow{\psi} \mathbb{A}^1,$$

as $$W = p_! \psi^* \mathrm{exp}$$. For $$g \geq 2$$, this is completely explicit — it factors through the Harder–Narasimhan stratification — and its endomorphisms are 1-dimensional. This explicitness is a key ingredient in the final step.

### 2. Kac–Moody localization

This is the "Galois-to-automorphic" engine. Starting from **opers** — a special type of $$\check{G}$$-local system with rigid structure — Beilinson and Drinfeld's construction produces sheaves on $$\mathrm{BunG}$$ via a localization procedure involving Kac–Moody algebras at the critical level. This works only in the D-module setting (exploiting the fact that D-modules have a special relationship with connections), and is ultimately responsible for the surjectivity of the Langlands functor.

### 3. Geometry of $$\mathrm{LocSys}_{\check{G}}$$

The final argument identifies the cuspidal part of the equivalence by an interpolation on the locus $$\mathrm{LocSys}^{\mathrm{irred}}_{\check{G}}$$ of irreducible local systems. In stark contrast to the arithmetic case — where cuspidal automorphic forms and their L-parameters form a discrete spectrum — in the geometric case this locus is **connected**, forming a continuous family. This connectedness enables an interpolation argument that reduces the proof to a computation of endomorphisms of the Whittaker sheaf, which is 1-dimensional.

## From Geometry to Arithmetic

The geometric result is over an algebraically closed field. To get the arithmetic Theorem 1.7, one takes the geometric equivalence and applies **trace of Frobenius**.

The key theorem (Arinkin–Gaitsgory–Kazhdan–Raskin–Rozenblyum–Varshavsky, 2021) is that this trace operation is well-behaved:

$$\mathrm{tr}(\mathrm{Fr} \mid D(\mathrm{BunG})) \xrightarrow{\;\sim\;} \mathcal{A}_c(\mathrm{BunG}(\mathbb{F}_q)).$$

The trace of Frobenius on the category of sheaves recovers the vector space of automorphic functions. Applying this to both sides of the geometric equivalence yields the arithmetic isomorphism.

This is a categorification of Grothendieck's sheaf-function dictionary: where classically a sheaf $$\mathcal{F}$$ gives a function via $$x \mapsto \mathrm{tr}(\mathrm{Frob}_x \mid \mathcal{F}_x)$$, here an entire _category_ of sheaves gives a vector space of functions via the categorical trace.

## What Doesn't Work (Yet)

Scholze is candid about the limitations:

**The number field case.** The four-step strategy "irreparably breaks down at the very first step" for number fields, because there is no analogue of $$X_{\overline{\mathbb{F}}_q}$$. For a long time, this was thought to be a fundamental barrier. But Fargues proposed studying geometric Langlands on the **Fargues–Fontaine curve** — a "curve" that geometrizes $$p$$-adic numbers — and this has led to a precise version of the **local** Langlands conjectures (Fargues–Scholze 2024). The global number-field case, however, remains open. Scholze notes that "genuinely new ideas are still needed."

**The notion of $$\check{G}$$-local system over $$\mathbb{Z}$$.** Even stating the reciprocity conjecture precisely requires a notion of $$\check{G}$$-local system on $$\mathrm{Spec}(\mathbb{Z})$$ — and this notion doesn't exist yet. Defining it is "essentially equivalent to defining some Tannakian category of $$\mathbb{C}$$-local systems on $$\mathrm{Spec}(\mathbb{Z})$$," which in turn requires a conjectural "Langlands group." Scholze suggests that the right framework may be a six-functor formalism of "variations of twistor structure" over $$\mathrm{Spec}(\mathbb{Z})$$, allowing Hodge numbers to be complex numbers instead of integers.

**The open substack.** The positive-characteristic result uses an open and closed substack $$\mathrm{LocSys}'_{\check{G}} \subset \mathrm{LocSys}_{\check{G}}$$ rather than the full stack. For $$G = \mathrm{SL}_n$$, equality is known. In general, the discrepancy comes from subtleties with the theory of singular support for étale sheaves in mixed characteristic.

## A Confession and Its Meaning

At one point in the paper, Scholze writes:

> I must admit that I find the $$(-, !, -, *)$$-formalism confusing, to the point that I cannot read the series of papers [AGKRRV] beyond a superficial level.

This is about the choice of six-functor formalism: whether to use $$(f^*, f_!)$$ or $$(f^!, f_*)$$ as the basic operations. The two choices are related by Verdier duality and agree on compact objects, but diverge after passing to ind-categories and stacks. Scholze finds the second convention unnatural (he uses the first in his report), and admits he cannot fully follow the foundational papers that use it.

This is revealing not as a limitation of Scholze but as a statement about the _state of the field_. The proof of geometric Langlands operates at such a level of categorical abstraction — "algebra with categories" in the literal sense, where the basic operations are tensor products and colimits of entire $$(\infty,1)$$-categories — that even the most technically gifted mathematicians can struggle with conventions and foundations. The machine works. But the machine is not yet easy to read.

## The Shape of Things

What Gaitsgory, Raskin, and collaborators have accomplished is, by any measure, one of the great achievements of 21st-century mathematics. They have:

1. **Proved a fundamental conjecture** — the geometric Langlands equivalence — that had been open for decades and guides an enormous amount of mathematical research.

2. **Brought the geometric results back to arithmetic**, resolving the function-field analogue of Conjecture 1.5 and the Arthur–Ramanujan conjecture for function fields.

3. **Demonstrated that "algebra with categories" can prove concrete things** — that manipulations at the level of $$(\infty,1)$$-categories, carried out with sufficient virtuosity, can descend through the trace of Frobenius to produce statements about actual numbers.

What they have not done — and what remains the deepest open problem — is extend any of this to number fields. The geometric Langlands correspondence is a theorem about curves. The Langlands program for $$\mathrm{Spec}(\mathbb{Z})$$ — the original dream, the one that connects to the Riemann Hypothesis and the distribution of primes — remains as distant as ever.

But the geometric proof has changed the landscape. As Scholze notes, "much of the current research in local arithmetic Langlands now builds on the paradigm of geometric Langlands." The Fargues–Fontaine curve gives a geometric incarnation of local questions. The categorical methods are being applied to $$p$$-adic and mod-$$p$$ representations. Even if the global number-field case requires "genuinely new ideas," the tools are sharper, the analogies are more precise, and the confidence that the Langlands vision is correct has never been higher.

Scholze ends his acknowledgments by apologizing for "any misrepresentations." One suspects the misrepresentations are few. This is a report by someone who has internalized the ideas deeply enough to rephrase them in his own language — sometimes disagreeing with conventions, sometimes confessing confusion, always insisting on clarity about what is known, what is conjectured, and what is merely hoped for. It is, in other words, exactly the kind of mathematics that the Séminaire Bourbaki was created to produce.

---

_The report is available at [https://people.mpim-bonn.mpg.de/scholze/Exp1252_Scholze.pdf](https://people.mpim-bonn.mpg.de/scholze/Exp1252_Scholze.pdf). For another survey of the proof, Scholze recommends David Ben-Zvi's "What is the geometric Langlands correspondence about?", Current Events Bulletin of the AMS (2026)._
