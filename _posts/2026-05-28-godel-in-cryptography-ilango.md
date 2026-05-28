---
layout: post
title: "Gödel in Cryptography: The Impossible Zero-Knowledge Proof That Works Anyway"
slug: godel-in-cryptography-ilango
date: 2026-05-28
description: A reading of Rahul Ilango's Machtey Award-winning paper that uses Gödel's incompleteness theorem to construct "effectively zero-knowledge" proofs for NP — non-interactive, with perfect soundness, and no trusted setup — circumventing a classical impossibility result by exploiting the gap between truth and provability.
tags: [cryptography, logic, complexity-theory, zero-knowledge]
categories: [research]
giscus_comments: true
related_posts: true
toc:
  sidebar: left
---

There is a classical impossibility result in cryptography, due to Goldreich and Oren (1994), that says: you cannot have a zero-knowledge proof that is simultaneously **non-interactive** (a single message from prover to verifier), **perfectly sound** (no false proof exists), and **zero-knowledge** (reveals nothing beyond the truth of the statement). You must sacrifice at least one.

Rahul Ilango, in a paper that won the 2025 Machtey Award at FOCS,[^paper] shows that this impossibility can be circumvented — not by weakening any of the three properties in a conventional way, but by exploiting the gap between truth and provability that Gödel's incompleteness theorem guarantees. The result is a new cryptographic primitive: **effectively zero-knowledge proofs** for NP, with no interaction, no trusted setup, and perfect soundness.

[^paper]: Rahul Ilango, "Gödel in Cryptography: Effectively Zero-Knowledge Proofs for NP with No Interaction, No Setup, and Perfect Soundness," IACR ePrint 2025/1296 (July 2025). Winner of the 2025 Machtey Award at FOCS. [IACR ePrint](https://eprint.iacr.org/2025/1296).

As Amit Sahai (UCLA) put it: "It is pretty mind-bending. The first time you see it, you're like, 'Wait, what?'"[^quanta]

[^quanta]: Kevin Hartnett, "How Unknowable Math Can Help Hide Secrets," *Quanta Magazine*, May 11, 2026. [Article](https://www.quantamagazine.org/how-unknowable-math-can-help-hide-secrets-20260511/).

## Zero-Knowledge Proofs: The Setup

A **zero-knowledge proof** lets a prover convince a verifier that a statement is true (say, "this graph is 3-colorable") without revealing *any* information beyond the truth of the statement (in particular, without revealing *which* 3-coloring works).

The standard construction for NP-complete problems (Goldreich, Micali, Wigderson 1986) achieves this through interaction: the prover and verifier exchange multiple messages, with the verifier issuing random challenges. The proof is:

- **Complete**: A true statement always has a convincing proof
- **Sound**: A false statement is (almost) never accepted (computational soundness — a cheating prover with unbounded computation could fool the verifier)
- **Zero-knowledge**: There exists a **simulator** — an efficient algorithm that can produce fake transcripts indistinguishable from real ones, without knowing the secret witness

The simulator is the key to the definition: its existence proves that the proof transcript carries no information about the witness, because the simulator generates equally convincing transcripts from scratch.

## The Impossibility

Three properties of classical mathematical proofs are desirable for zero-knowledge proofs:

1. **Non-interactivity**: A proof is a single message from prover to verifier (no back-and-forth)
2. **Perfect soundness**: No false proof exists, period — not even for a computationally unbounded adversary
3. **No trusted setup**: No common reference string or shared randomness required

Goldreich and Oren (1994) proved that you cannot have all three simultaneously with zero-knowledge. The intuition: if the proof is a single, fixed message with perfect soundness, then either the message itself *is* the witness (not zero-knowledge), or there must be some structure that lets a simulator produce fake proofs — but with perfect soundness, fake proofs don't exist.

Existing systems sacrifice something. SNARKs (used in blockchains) sacrifice perfect soundness and require trusted setup. Interactive proofs sacrifice non-interactivity. The Fiat-Shamir heuristic gives non-interactivity but relies on random oracle assumptions and doesn't achieve perfect soundness.

## The Crack in the Door

Ilango's insight is to change what "zero-knowledge" means — not by weakening it in a conventional cryptographic sense, but by invoking Gödel's incompleteness theorem to create a scenario where the proof system might not *technically* be zero-knowledge, but nobody can ever *prove* it isn't.

The key idea in three steps:

### Step 1: The two-worlds argument

Consider two possible worlds:

- **World 1** (the real world): The standard axioms of mathematics (ZFC) are consistent. No contradictions exist. The proof system has no simulator — as the Goldreich-Oren impossibility demands.

- **World 2** (the counterfactual): ZFC is inconsistent. From a contradiction, anything follows (*ex falso quodlibet*), including the existence of a simulator. In this world, the proof system *is* zero-knowledge.

### Step 2: Gödel's gift

Gödel's second incompleteness theorem says: if ZFC is consistent, it cannot prove its own consistency. In particular, ZFC cannot *rule out* World 2. No matter how much mathematics you do within ZFC, you will never produce a proof that World 2 doesn't hold — because such a proof would itself constitute a proof of ZFC's consistency, which Gödel says is impossible (assuming ZFC is consistent).

### Step 3: The definition

A proof system is **effectively zero-knowledge** if no one can prove (in any reasonable formal system) that a simulator *doesn't* exist.

In World 1, there is no simulator. But nobody can prove there isn't one, because proving "no simulator exists" would require proving "ZFC is consistent," which Gödel forbids. In World 2, there is a simulator, and the system is traditionally zero-knowledge. Since nobody can distinguish which world they inhabit, the system is effectively zero-knowledge in either case.

## What This Achieves

The construction achieves all three "impossible" properties:

- **Non-interactive**: A single message from prover to verifier
- **Perfectly sound**: No false proofs exist
- **Effectively zero-knowledge**: No formal system extending ZFC can prove the non-existence of a simulator

The catch — and it's a subtle one — is that the prover's statement is modified. Instead of proving "this graph is 3-colorable," the prover proves "this graph is 3-colorable, **assuming there is no efficient algorithm that finds contradictions in ZFC.**"

This assumption is widely believed to be true (ZFC has been studied for over a century without finding contradictions, and finding them efficiently would be an extraordinary computational feat). But it cannot be *proved* — that's the whole point. The assumption is the Gödelian lever that creates the gap between "the system might not be zero-knowledge" and "nobody can prove it isn't."

## Proof-Theoretic Hardness: A New Flavor

What makes this paper conceptually novel is that it exploits a new kind of hardness — not computational hardness (factoring is hard) but **proof-theoretic hardness** (certain truths are unprovable).

Classical cryptography rests on computational assumptions: "no efficient algorithm can factor large semiprimes," "no efficient algorithm can compute discrete logarithms." These are conjectures about what computers can *do*.

Ilango's construction rests on a proof-theoretic assumption: "no efficient algorithm can find contradictions in ZFC." This is a conjecture about what formal systems can *prove*. It is, arguably, a stronger assumption — not in the sense of being less likely, but in the sense of being harder to refute. Even if P = NP (which would break most computational cryptography), the proof-theoretic hardness of ZFC-consistency would still hold.

As Marco Carmosino (IBM) asked: "Can we benefit from that kind of hardness?" — suggesting that proof-theoretic hardness might circumvent other cryptographic impossibility results as well.

## Historical Significance

The paper resolves a question that was open for over 30 years. Non-interactive witness hiding (a weaker property than zero-knowledge, where the proof hides the specific witness but not necessarily all information) was previously unknown for NP with perfect soundness. Ilango's effectively zero-knowledge proofs are strictly stronger than witness hiding, and they achieve it.

The result also creates a new bridge between two fields:

- **Proof complexity**: the study of how long proofs must be for various formal systems
- **Cryptography**: the study of what can be hidden and from whom

These fields have historically developed in parallel. Ilango's work shows that proof-theoretic phenomena (unprovability, independence) have direct cryptographic applications — a connection that was not previously known to be productive.

## The Honest Assessment

Several things this paper is **not**:

**Not a practical protocol (yet).** The construction is a feasibility result, not an efficient implementation. The proofs are likely enormous. Making this practical would require substantial additional work.

**Not unconditional.** The construction assumes "no efficient algorithm finds ZFC contradictions." This is widely believed but unproven — and by its nature, unprovable within ZFC.

**Not traditional zero-knowledge.** The system is *effectively* zero-knowledge, not zero-knowledge in the standard Goldreich-Micali-Wigderson sense. A simulator may not exist. The guarantee is that nobody can *prove* it doesn't exist. Whether this distinction matters depends on one's threat model. For an adversary operating within any reasonable formal system, the distinction is invisible.

What the paper **is**: a conceptual breakthrough that uses the deepest result in mathematical logic (Gödel's incompleteness) to circumvent the deepest impossibility in cryptographic foundations (Goldreich-Oren), creating a new primitive that was previously thought to be impossible. The Machtey Award was well-earned.

Gödel showed in 1931 that truth outruns provability. Ilango shows in 2025 that this gap — the space between what is true and what can be proved — is large enough to hide secrets in.

---

_The paper is available at [IACR ePrint 2025/1296](https://eprint.iacr.org/2025/1296). For an accessible introduction, see the Quanta Magazine article ["How Unknowable Math Can Help Hide Secrets"](https://www.quantamagazine.org/how-unknowable-math-can-help-hide-secrets-20260511/) (May 2026)._
