---
layout: post
title: "Mochizuki Turns to Lean: A Progress Report on Formalizing IUT"
slug: mochizuki-iut-lean-formalization
date: 2026-05-28
description: A reading of Mochizuki's April 2026 preliminary report on formalizing inter-universal Teichmüller theory in Lean — what it contains, what it reveals about the current state of the project, and what it means for the most contentious question in contemporary mathematics.
tags: [mathematics, number-theory, formal-verification, iut]
categories: [research]
giscus_comments: true
related_posts: true
toc:
  sidebar: left
---

In April 2026, Shinichi Mochizuki posted a 14-page document[^report] that may eventually rank among the most consequential in the history of formal verification: a preliminary progress report on formalizing his inter-universal Teichmüller theory (IUT) in the Lean proof assistant. The document is titled _On the Formalization of IUT: A Preliminary Progress Report_, and it describes joint work in progress with Yuichiro Hoshi, Go Yamashita, Yu Yang, and others at RIMS.

[^report]: Shinichi Mochizuki, "On the Formalization of IUT: A Preliminary Progress Report," April 2026. Available at [https://www.kurims.kyoto-u.ac.jp/~motizuki/Formalization%20of%20IUT%20(2026-04).pdf](<https://www.kurims.kyoto-u.ac.jp/~motizuki/Formalization%20of%20IUT%20(2026-04).pdf>). 14 pages.

This is not a completed formalization. The Lean code is described as "skeletal" — a bare-bones framework that captures the logical structure of a specific implication in IUT but does not yet contain all the details needed for public release. But the fact that this project exists, and that Mochizuki is framing it as a central priority, is itself significant.

## Context

Inter-universal Teichmüller theory (IUT) has been the most contentious topic in mathematics for over a decade. Mochizuki posted the four papers (IUTchI–IV) in 2012, claiming a proof of the abc conjecture — one of the deepest open problems in number theory. The papers are roughly 600 pages of highly idiosyncratic mathematics, built on a decades-long program in anabelian geometry.

The mathematical community has been unable to reach consensus on the proof. In 2018, Peter Scholze and Jakob Stix wrote a manuscript[^ss] identifying what they argued was a gap in Corollary 3.12 of IUTchIII — the precise point where the theory is supposed to yield a height inequality. Mochizuki has maintained that the objection reflects a misunderstanding. The journal PRIMS published IUTchI–IV in 2021, but the controversy has not been resolved. Multiple workshops, surveys, and reports (by Mochizuki and others) have appeared, none of which has shifted the positions of the principal parties.

[^ss]: Peter Scholze and Jakob Stix, "Why abc is still a conjecture," manuscript, 2018. Available at [https://www.math.uni-bonn.de/people/scholze/WhyABCisStillaConjecture.pdf](https://www.math.uni-bonn.de/people/scholze/WhyABCisStillaConjecture.pdf).

It is in this context that Mochizuki has turned to Lean.

## What the Document Says

The report has four sections. Let me trace each.

### §1: LeanForm as communication, not verification

Mochizuki makes an explicit and surprising framing choice. He argues that the primary significance of formalizing IUT in Lean is **not** verification of logical correctness — he considers the logic of IUT to be "very simple and elementary" — but rather **communication**:

> The significance of LeanForm lies in producing a precise record of the logical structure of IUT that is immune to false misinterpretations and hence can be used, in a pivotal way, to communicate this simplicity in a maximally efficient/precise way to other mathematicians.

He describes what he calls "vicious cycles" that have stymied communication:

1. _"The logic is simple"_ ↔ _"Don't insult my intelligence — the abc inequality can't follow from simple logic"_
2. _"One must study IUTchI–IV in detail to understand"_ ↔ _"One can't become motivated to study without confidence the theory is correct"_

LeanForm, he argues, is "the first technology that appears to have the technical capacity to allow us to break free of such vicious cycles" — by providing "a logically verified record that can be processed by a machine in a fraction of a second in a way that is entirely immune to nonmathematical accusations."

The social/political dimension is stated explicitly: "No one wants to invest substantial resources of time and effort in a 'sinking ship.' Ultimately, social/political power may be understood as a reflection of perceived capacity to reliably deliver/ensure long-term mathematical accountability for the mathematical correctness of results/assertions."

### §2: The five stages

The formalization strategy is laid out as five stages:

| Stage | Content                                                                 | Status              |
| ----- | ----------------------------------------------------------------------- | ------------------- |
| **1** | [IUTchIII] Theorem 3.11 ⟹ Corollary 3.12                                | Early skeletal code |
| **2** | Proof of Theorem 3.11 modulo IUTchI–II                                  | In progress         |
| **3** | IUTchI–II modulo earlier results (1995–2015)                            | Future              |
| **4** | Earlier results: anabelian geometry, Frobenioids, theta functions, etc. | Long-term           |
| **5** | Numerical aspects (IUTchIV, explicit estimates)                         | Long-term           |

The choice to begin with Stage 1 is deliberate: "since this has received the most public attention." Stage 1 is the implication from Theorem 3.11 to Corollary 3.12 — precisely the step Scholze and Stix challenged. Mochizuki frames this as "not so difficult" and argues that proper understanding reveals that "what one is really interested in is Stage 2."

An important technical point: Mochizuki initially wanted to formalize his notion of **species and mutations** — a framework of "types of mathematical objects" and "constructions between them" defined by set-theoretic formulas independent of any particular ZFC model. Species determine categories of species-objects; mutations determine functors. He notes that formalizing this requires ZFC as a first-order theory in Lean. A young Japanese researcher named Shogo Saito has achieved this, but it is not in MathLib (Lean's standard library). After consultation with Lean experts, Mochizuki concluded that the full species/mutations formalization "was most likely not necessary for the LeanForm of (at least Stages 1–2 of) IUT."

### §3: A review of IUT

Section 3 provides a brief, surprisingly readable summary of the core idea of IUT. The key construction:

Let $$R$$ be an integral domain with a group action $$G \curvearrowright R$$, and let $$N \geq 2$$. The $$N$$-th power map on $$R^{\triangleright} = R \setminus \{0\}$$ gives an isomorphism of multiplicative monoids:

$$R^{\triangleright} \xrightarrow{\;\sim\;} (R^{\triangleright})^N \subset R^{\triangleright}$$

This isomorphism is compatible with the group action but **not with addition** — it is not a ring homomorphism. The central problem of IUT is: given two distinct copies $${}^{\dagger}R$$ and $${}^{\ddagger}R$$ of the ring $$R$$, glued along the $$N$$-th power map on their multiplicative monoids, can one recover (portions of) the ring structure of $${}^{\dagger}R$$ from the ring structure of $${}^{\ddagger}R$$?

This seems "hopelessly intractable" in general. IUT claims to accomplish it for $$R = \mathcal{O}_F$$ (ring of integers of a number field) via:

- Anabelian geometry (reconstructing ring structures from group-theoretic data)
- The $$p$$-adic and complex logarithm
- Kummer theory
- The "multiradial representation" of the $$\Theta$$-pilot

The result, after passing to heights: the height of an elliptic curve equals $$N$$ times the height (up to a mild error term), which implies the height is bounded — the abc inequality.

The construction lives on the **log-theta-lattice**, an infinite diagram of ring structures connected horizontally by the $$\Theta$$-link (the $$N$$-th power map) and vertically by the log-link (the $$p$$-adic logarithm). The multiradial representation is obtained by a "descent" along this lattice, progressively introducing indeterminacies until one reaches data that is simultaneously compatible with both sides of the $$\Theta$$-link.

### §4: Skeletal Lean code for "3.11.5 ⟹ 3.12"

The final section describes what has actually been written in Lean. The implication from Theorem 3.11 to Corollary 3.12 has been decomposed into four commuting triangles in a diagram of species (boxes) and mutations (arrows):

1. The **first triangle**: expresses the multiradial algorithm — input is a basic prime-strip (BPS), output is the multiradial representation.

2. The **second triangle**: expresses the _descent_ aspect — the multiradial representation can be constructed from weaker, "vertically coric" (log-link invariant) étale data.

3. The **third triangle**: composes descent with an elementary "hull+det" operation (taking determinants of modules).

4. The **fourth triangle**: the crucial comparison — restricting to the case where input comes from the $$q$$-pilot, via "simultaneous holomorphic expressibility" (SHE).

The skeletal Lean code addresses the **fourth triangle** — the comparison of the $$\Theta$$-pilot and the $$q$$-pilot within a single ring structure. This is precisely the point where IUT claims that the height of the elliptic curve equals $$N$$ times the height up to a discrepancy, yielding the bound.

The RIMS group is currently making "substantial progress" on skeletal Lean code for the second and third triangles (the APT — "algorithmic parallel transport" — aspect).

## What the Document Reveals

Several things stand out:

**1. The formalization exists but is incomplete.** The Lean code is "skeletal" — it records the logical structure but not all the details. Mochizuki is explicit that "it will still take some more time before sufficiently many details can be added to 'flesh out' the Lean code to a degree that it is suitable for release to the general public."

**2. The starting point is strategically chosen.** By beginning with the implication 3.11 ⟹ 3.12, Mochizuki is directly addressing the step that Scholze and Stix challenged. If the formalization succeeds and the Lean code compiles, it would provide machine-verified evidence that the logic of this particular implication is sound.

**3. The scope is carefully bounded.** The formalization decomposes the implication into four pieces and addresses the fourth (and, arguably, most contested) first. This is a pragmatic strategy: rather than formalizing all 600 pages at once, isolate the critical step and verify it.

**4. The framing is revealing.** Mochizuki does not position this as a concession to skeptics. He positions it as a tool to overcome "nonmathematical" social/political barriers. Whether or not one agrees with this framing, it signals that Mochizuki recognizes the impasse and is taking concrete action to resolve it.

**5. The species/mutations detour is instructive.** Mochizuki's initial instinct was to formalize his categorical framework (species/mutations) first. Lean experts apparently convinced him this wasn't necessary for Stages 1–2. This is a meaningful data point: it suggests the logic of the critical steps may be expressible in standard Lean without the full species/mutations apparatus that has been one of the barriers to understanding IUT.

## What It Would Mean

A completed and compiled Lean formalization of "3.11.5 ⟹ 3.12" would not settle the abc conjecture. It would settle something narrower but important: whether the _logical implication_ from Theorem 3.11 to Corollary 3.12 is valid, given the definitions and hypotheses as Mochizuki states them.

The Scholze–Stix objection was not (primarily) that the logic of the implication is wrong, but that the hypotheses of Theorem 3.11 cannot be satisfied — that the "distinct ring structures" and their "gluing" do not have the properties Mochizuki claims. A formalization of Stage 1 alone would not address this objection directly; that would require Stages 2–3.

Nevertheless, a completed Stage 1 formalization would:

- Provide the first machine-verified component of IUT
- Demonstrate that the logical structure of the critical step is expressible in a standard proof assistant
- Create a precise target for mathematical scrutiny: either the Lean code compiles, or it doesn't

The mathematical community has been waiting for resolution for over a decade. A Lean formalization, if completed, would be the most decisive step yet toward one — not because machines are better at mathematics than humans, but because machines don't get tired, don't have political commitments, and don't participate in vicious cycles.

We are not there yet. The code is skeletal. The project is in its early stages. But the fact that it has begun — and that Mochizuki himself is leading it, with Lean experts involved — makes this the most concrete development in the IUT saga since the Scholze–Stix manuscript of 2018.

---

_The report is available at [https://www.kurims.kyoto-u.ac.jp/~motizuki/Formalization%20of%20IUT%20(2026-04).pdf](<https://www.kurims.kyoto-u.ac.jp/~motizuki/Formalization%20of%20IUT%20(2026-04).pdf>). For background, see also a [previous post](/blog/2026/mochizuki-iut-logical-structure/) on Mochizuki's report on the logical structure of IUT._
