---
layout: post
title: "The Sum-Product Conjecture Is False: How Number Fields Broke a 50-Year-Old Problem"
slug: sum-product-conjecture-disproved
date: 2026-05-28
description: A reading of Bloom, Sawin, Schildkraut, and Zhelezov's disproof of the Erdős sum-product conjecture for real numbers — using totally real number fields of large degree to build sets where both A+A and AA are simultaneously small, settling a central question in additive combinatorics.
tags: [mathematics, combinatorics, number-theory, additive-combinatorics]
categories: [research]
giscus_comments: true
related_posts: true
toc:
  sidebar: left
---

In 1974, Paul Erdős conjectured that for any finite set $$A$$ of real numbers, at least one of the sum set $$A + A$$ or the product set $$AA$$ must be nearly as large as possible:

$$\max(|A + A|, |AA|) \geq |A|^{2 - o(1)}.$$

This is the **sum-product conjecture** — the assertion that addition and multiplication cannot simultaneously compress a finite set. It has been one of the central open problems in additive combinatorics for half a century, with the best known lower bound being $$|A|^{4/3 + c}$$ for a small constant $$c > 0$$ (Konyagin–Shkredov, improved by Cushman to $$c = 10/4407$$).

Thomas Bloom, Will Sawin, Carl Schildkraut, and Dmitrii Zhelezov have just disproved it.[^paper]

[^paper]: Thomas F. Bloom, Will Sawin, Carl Schildkraut, and Dmitrii Zhelezov, "The sum-product conjecture is false for real numbers," arXiv:2605.28781 (May 2026). 25 pages.

> **Theorem 1.1.** _There exists an absolute constant $$c > 0$$ such that there are arbitrarily large finite $$A \subset \mathbb{R}$$ with_
>
> $$\max(|A + A|, |AA|) \leq |A|^{2-c}.$$

The constant $$c$$ is explicit but very small — the authors sketch that $$c \geq 0.00000087$$ is achievable, though they emphasize that "this should not be taken too seriously, and can certainly be improved with a little more effort."

The construction uses sets of algebraic integers in totally real number fields of degree $$d \to \infty$$. The elements of the counterexample set $$A$$ are real numbers, but they are algebraic integers in a number field whose degree grows like $$\log|A|$$. In particular, the conjecture may still be true for sets $$A \subset \mathbb{Z}$$ (Erdős's original setting) or for sets in number fields of bounded degree.

## The Idea in One Paragraph

Take a totally real number field $$K$$ of large degree $$d$$, with $$d$$ real embeddings $$\sigma_1, \ldots, \sigma_d: K \hookrightarrow \mathbb{R}$$. The ring of integers $$\mathcal{O}_K$$ is a $$d$$-dimensional lattice in $$\mathbb{R}^d$$ (via the Minkowski embedding), and the group of units $$\mathcal{O}_K^\times$$ is a lattice of rank $$d-1$$ in the logarithmic hyperplane (via Dirichlet's unit theorem). Build $$A = GP$$ where $$G$$ is a box of units (controlling multiplicative structure) and $$P$$ is a box of integers (controlling additive structure). Units don't change the multiplicative structure much ($$|GG| \leq O(1)^d |G|$$), and integers in a small interval don't change the additive structure much ($$A + A$$ stays in a bounded box). The tension between these two constraints is what gives the saving.

## The Construction in Detail

### The additive part: a box of integers

Fix a large parameter $$X$$. Define

$$P = \{ \alpha \in \mathcal{O}_K : |\sigma_i(\alpha) - X| \leq \epsilon X \text{ for all } 1 \leq i \leq d \}$$

for a small constant $$\epsilon > 0$$. By the geometry of numbers (Blichfeldt's lemma), the number of lattice points in this box is

$$|P| \geq X^d \Delta_K^{-1/2}$$

where $$\Delta_K$$ is the discriminant. Since all elements of $$P$$ have embeddings close to $$X$$, the sum set $$P + P$$ stays in a box of side length $$\sim 2X$$ — the additive structure is tight.

### The multiplicative part: a box of units

Fix a parameter $$Y$$. Define

$$G = \{ u \in \mathcal{O}_K^\times : |\log|\sigma_i(u)|| \leq Y \text{ for all } 1 \leq i \leq d \}.$$

By Dirichlet's unit theorem, the logarithmic embeddings of units form a rank-$$(d-1)$$ lattice in the hyperplane $$x_1 + \cdots + x_d = 0$$. The covolume is $$\sqrt{d} R_K$$ (where $$R_K$$ is the regulator), so

$$|G| \geq Y^{d-1} d^{-1/2} R_K^{-1}.$$

The key multiplicative property: since $$GG \subseteq B^\times(2Y)$$ (doubling the logarithmic radius at most doubles the box), we get

$$|GG| \leq O(1)^d |G|.$$

The product set of $$G$$ is only a constant multiple of $$G$$ itself — an extreme form of multiplicative small-doubling.

### Putting them together

Set $$A = GP = \{up : u \in G, p \in P\}$$. A crucial claim: this product is **direct** — $$|A| = |G| \cdot |P|$$. This follows from the separation of units: if $$u_1 p_1 = u_2 p_2$$, then $$u_1/u_2 = p_2/p_1$$ is a unit with all embeddings close to 1. But by a lemma (Lemma 3.4, suggested by GPT-5.5 Pro — more on this below), any unit $$u$$ with $$\phi^{-1} < |\sigma_i(u)| < \phi$$ for all $$i$$ (where $$\phi = (1+\sqrt{5})/2$$ is the golden ratio) must be $$\pm 1$$. So the factorization $$A = GP$$ is essentially unique.

**Sum set bound:** Multiplication by a unit in $$G$$ stretches each embedding by at most $$e^Y$$. So every element of $$A = GP$$ has embeddings in a box of side $$\sim Xe^Y$$. The number of algebraic integers in such a box is at most $$(Xe^Y)^d$$, giving

$$|A + A| \leq O(e^Y)^d |A|.$$

**Product set bound:** $$AA \subseteq GG \cdot PP$$. Since $$|GG| \leq O(1)^d |G|$$ and $$|PP| \leq |P|^2$$ trivially,

$$|AA| \leq O(1)^d |G| \cdot |P|^2 = O(1/Y)^{d-1} |A|^2.$$

The saving in the product set is the size of the unit box — roughly $$Y^{d-1}$$. Choosing $$Y$$ as a sufficiently large absolute constant makes $$|AA| \leq 2^{-d} |A|^2$$, which is an exponential saving. Then $$X$$ can be chosen large enough to make the additive blow-up $$O(e^Y)^d$$ at most $$|A|^\epsilon$$ for any prescribed $$\epsilon > 0$$.

This gives:

> **Corollary 1.3.** _For any $$\epsilon \in (0,1)$$, there are arbitrarily large $$A \subset \mathbb{R}$$ with_
>
> $$|A + A| \leq |A|^{1+\epsilon} \quad \text{and} \quad |AA| \leq |A|^{2 - c\epsilon}$$
>
> _for some absolute constant $$c > 0$$._

### What makes it work: towers of number fields

The construction requires $$d \to \infty$$ (since $$|A|$$ grows like $$O(1)^d$$). This means we need totally real number fields of arbitrarily large degree with discriminant $$\Delta_K \leq C^d$$ — so that the lattice geometry doesn't degrade. Such **bounded-root-discriminant towers** were constructed by Martinet using class field towers.

The regulator $$R_K$$ must also be controlled. The paper notes (Lemma 3.1) that $$R_K \leq \Delta_K$$ for totally real fields of degree $$\geq 2$$, so discriminant control suffices. This regulator control has found applications before — it was used to construct explicit lattice sphere packings.

## Further Results

The paper doesn't stop at Theorem 1.1. The same ideas give:

**Many sums and products (Theorem 1.4).** For $$k$$-fold sum and product sets: $$\max(|kA|, |A^{(k)}|) \leq |A|^{C \log k / \log \log k}$$. This disproves Erdős's stronger conjecture that the exponent should grow like $$k$$. The upper bound matches (up to log log factors) the lower bound from recent work of Mudgal combined with the Gowers–Green–Manners–Tao resolution of the weak polynomial Freiman–Ruzsa conjecture.

**Linear equations in multiplicative groups (Theorem 1.5).** New lower bounds showing that the dependence on the rank $$d$$ in the Evertse–Schlickewei–Schmidt upper bound is best possible.

**Unit equations (Theorem 1.6).** For sufficiently large $$k$$, there exist number fields of degree $$d$$ where the unit equation $$x_1 + \cdots + x_k = 1$$ has at least $$C^d$$ solutions.

**Sum-product in $$\mathbb{F}_p$$ (Theorem 1.7).** Sets $$A \subset \mathbb{F}_p$$ with $$p^c < |A| < p^{1/2}$$ and $$\max(|A+A|, |AA|) \leq |A|^{2-c}$$. The best lower bound is $$|A|^{5/4 - o(1)}$$ (Mohammadi–Stevens).

**Sum-product in function fields (Theorem 1.8).** Counterexamples in $$\mathbb{F}_q((t))$$ with $$\max(|A+A|, |AA|) \leq |A|^{2 - c/\log p}$$.

## The Role of AI

The paper includes a forthright disclosure:

> *The authors were inspired to revisit the possibility of disproving the sum-product conjecture using number fields of large degree by the recent OpenAI counterexample to the unit distance conjecture. GPT-5.5 Pro was used as a sounding board in the early stages of the development of this proof, but the final proof, including all the main ideas, was almost entirely human-generated (the exception being the suggestion of Lemma 3.4, which replaced a more complicated result of Schinzel with a short elementary argument). Everything in this paper was written by the authors.*

Lemma 3.4 is the unit separation lemma: if $$u \in \mathcal{O}_K^\times$$ and $$\phi^{-1} < |\sigma_i(u)| < \phi$$ for all $$i$$, then $$u = \pm 1$$. The proof is four lines: consider $$\alpha = u^2 + u^{-2} - 2$$; if $$\alpha \neq 0$$ then all embeddings lie in $$(0,1)$$, contradicting $$N(\alpha) \in \mathbb{Z}$$. This replaced a citation to a deeper result of Schinzel.

The paper notes it was "inspired" by the recent AI-assisted counterexample to the unit distance conjecture (Ambrus et al., 2025), but "curiously, the final construction given here required far less number theoretic input than the unit distance counterexample."

## What Survives

The conjecture is dead over $$\mathbb{R}$$. But several natural variants survive:

- **Over $$\mathbb{Z}$$**: The counterexamples use algebraic integers in number fields of degree $$\sim \log |A|$$. The conjecture may still hold for ordinary integers.

- **In bounded degree**: For $$A$$ contained in a number field of fixed degree (e.g., $$A \subset \mathbb{Q}$$), the conjecture is open. The construction fundamentally requires $$d \to \infty$$.

- **The Solymosi barrier**: Solymosi proved $$|A+A|^2 |AA| \geq |A|^{4 - o(1)}$$ for any $$A \subset \mathbb{R}$$. This is *not* contradicted by the new counterexamples (which have large sum set and small product set or vice versa, but cannot violate this multiplicative constraint).

- **The $$4/3$$ barrier over $$\mathbb{R}$$**: The best lower bound $$|A|^{4/3+c}$$ holds for *all* $$A \subset \mathbb{R}$$ and is not affected by the counterexample. What dies is the conjecture that the exponent can be pushed to $$2 - o(1)$$.

## The Architecture of a Counterexample

What makes this paper beautiful is the simplicity of the underlying idea. The construction is a high-dimensional version of the classical Balog–Wooley example $$A = GP$$ (geometric progression times arithmetic progression). In one dimension, this only saves a logarithmic factor. The insight is that in $$d$$ dimensions, the unit group provides exponentially many "directions" in which to absorb multiplicative structure, while the integer lattice provides exponentially many "positions" in which to control additive structure. The tension between $$e^Y$$ (additive blow-up per dimension) and $$Y^{d-1}$$ (multiplicative saving from the unit group) resolves in favor of the product set as soon as $$Y$$ is a sufficiently large constant and $$d \to \infty$$.

The number theory input is minimal — Martinet's towers and Blichfeldt's lemma — but the *idea* of using high-dimensional lattices to amplify a one-dimensional trick is what makes it work. As the authors note, "the final construction given here required far less number theoretic input than the unit distance counterexample." The difficulty was not in the proof but in the *decision to try this approach* — a decision that, by their own account, was prompted by the AI-assisted resolution of a different longstanding conjecture.

Fifty years of positive results had built an expectation that the exponent 2 was the truth. Now we know it isn't.

---

_The paper is available at [arXiv:2605.28781](https://arxiv.org/abs/2605.28781)._
