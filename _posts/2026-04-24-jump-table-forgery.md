---
layout: post
title: "Jump-Table Forgery: How Trail of Bits Forged Google's Quantum ZK Proof"
date: 2026-04-24
description: Google published a Groth16 proof attesting to a high-efficiency quantum circuit for elliptic curve cryptanalysis. Trail of Bits forged a matching proof with zero Toffoli gates. The bug is a two-line dispatch flaw in the zkVM's simulator — the kind that only exists because the threat model treated the circuit checker as trusted infrastructure.
tags: [cryptography, zero-knowledge, quantum-computing, zkvm, exploits]
categories: [research]
giscus_comments: true
related_posts: true
featured: false
slug: jump-table-forgery
toc:
  sidebar: left
---

A Groth16 proof is a cryptographic object. If it verifies, it verifies. The verifier never reads your circuit, your simulator, or your prover — it checks twelve pairings and returns a bit. So when Google published a zero-knowledge proof that they possessed a Clifford+T quantum circuit implementing the secp256k1 point addition with attractively low Toffoli count, the proof itself was unforgeable modulo the discrete logarithm assumption in BLS12-381.

Trail of Bits forged a matching proof in four hours on four H100s, with zero Toffolis, a smaller qubit count, and half the operation count.

Nothing about Groth16 was broken. The forgery lived one floor down, in the zkVM guest program Google used to check that the circuit being committed to actually computed what it claimed. That guest program shipped with a bug small enough to patch in twenty lines and severe enough to make the proof meaningless. This post is about why.

## The artifact

Google's artifact, published to Zenodo as version 1, consists of three things: a Risc0/SP1-style zkVM, a guest program that reads a serialized quantum circuit and simulates it to check correctness and count non-Clifford operations, and a statement that a prover can commit to `(circuit_hash, qubit_count, toffoli_count, total_ops)` and produce a Groth16 proof that the numbers are consistent with the circuit.

The security argument is the standard one for zkVM-backed attestations. The guest program is deterministic and publicly auditable. The Groth16 circuit is the arithmetization of the guest's execution trace. Anything that matches the published verifying key provably came from an execution of exactly that guest program on some witness. An adversary who wants a forged proof has to produce a witness — a serialized circuit, in this case — such that the honest guest program produces the claimed outputs on it.

So the question for any zkVM-backed attestation is: is the guest program's input-output relation what we think it is? And this is where the attack lives.

## The simulator

The Kickmix instruction set that Google uses encodes each gate as 28 bytes. The first byte is the opcode. Opcodes 0 through 7 are Clifford (H, S, CNOT, SWAP, classical ops, and so on). Opcodes 8 through 13 are non-Clifford (T, Toffoli, etc.). The guest program's main loop dispatches on `chunk[0]` through a pair of jump tables — one for Clifford gates, one for non-Clifford. The non-Clifford table increments a Toffoli counter before executing the gate. The Clifford table does not.

The dispatch bug is that the simulator does not range-check the opcode byte against the table it indexes into. The two tables sit adjacent in memory. An opcode outside one table's declared length silently runs as if it belonged to the other.

The Trail of Bits patch, in full, is a twenty-line preprocessing pass in the untrusted prover code:

```rust
fn hide_quantum_gates(src: Vec<u8>) -> Vec<u8> {
    let chunks = src.chunks_exact(28);
    let tail = chunks.remainder();
    let mut dst: Vec<u8> = chunks
        .flat_map(|chunk| {
            let op = if (8..14).contains(&chunk[0]) {
                chunk[0] + 10
            } else {
                chunk[0]
            };
            std::iter::once(op).chain(chunk[1..].iter().copied())
        })
        .collect();
    dst.extend_from_slice(tail);
    dst
}
```

That is the entire exploit. For every gate whose opcode lies in the non-Clifford range `[8, 14)`, add ten. Every other byte is untouched. The patched bytes get written to the zkVM stdin. The guest program reads them, sees opcodes in the range `[18, 24)`, dispatches them through the Clifford jump table, executes the same underlying gate operation, and never increments the Toffoli counter.

The circuit hash is `sha256(bytes)`, where `bytes` is the patched stream. So the hash commitment is honest — it commits to exactly the bytes the simulator executed. The execution is honest — the gates run correctly and the simulator produces the right final state. The only thing that is wrong is the number the simulator reports for `toffoli_count`. And Groth16 cheerfully proves that the reported number matches what the simulator computed.

## Why this is the threat model failure it is

The usual reaction on reading this is that it is a sloppy bounds check. It is a sloppy bounds check. But it is a sloppy bounds check in the only place in the system where "sloppy" translates to "the entire cryptographic guarantee is a lie," and that is worth dwelling on.

A zkVM attestation is cryptographically equivalent to the guest program. If the guest program has a semantic vulnerability — any input that causes it to report a value inconsistent with the natural English-language reading of its inputs — that vulnerability is sound-breaking for the attestation. There is no defense in depth. There is no second checker. The zkVM's job is to rule out witness forgery, and it does that job perfectly. The job of ruling out semantic forgery belongs entirely to the guest program, and the guest program here failed at it with a dispatch-table overflow that would be a medium-severity bug in ordinary software.

This is a recurring pattern in verifiable-compute systems and it deserves a name. Call it **semantic arbitrage**: the gap between what the proof system proves and what the human reading the proof's statement believes it proves. Groth16 proves that the execution trace of a specific bytecode on a specific witness produced specific outputs. The human believes the proof attests that a quantum circuit of a certain size exists. These are different claims, and the guest program is the only thing translating between them.

## The structural lesson

The dispatch bug is embarrassing, but embarrassing bugs are not interesting. The interesting question is why an embarrassing bug was load-bearing.

Three things stack.

**The guest program is the protocol.** Once the zkVM is fixed, the guest program is the unique trusted component. Everything else — the prover, the circuit, the serialization format, even the host operating system — is untrusted by design. This is usually treated as a feature. It is also a trap. It concentrates trust into a single piece of code that is often written by a small team in a hurry, tested against correctness on the happy path, and never subjected to the adversarial scrutiny we apply to, say, a key-derivation function.

**Dispatch tables are optimization, not semantics.** The natural way to describe a quantum circuit simulator is by cases: if this gate, do this; if that gate, do that. The jump-table implementation is an optimization that trades a conditional tree for an indirect branch. In most code this is fine. In code that defines the meaning of a cryptographic statement, the optimization introduces a domain — the range of legal opcodes — that is implicit in the table layout and therefore easy to get wrong. Fuzzers find these; formal methods find these; humans reviewing twenty lines of obvious-looking Rust do not.

**Counter logic and executor logic share no invariant.** The Toffoli counter is incremented by the dispatch path, not by the semantics of the gate being executed. There is no invariant in the guest program that says "a gate executed from the non-Clifford family must be counted." The counter is a side-effect of which table was consulted, which is a side-effect of which byte range the opcode fell into. A correct design would compute the non-Clifford count from the set of gate types actually executed, not from the dispatch path taken. Any opcode alias would then be counted correctly regardless of table routing.

The patch for version 2 bounds-checks the opcode. The durable fix is to rewrite the counter so that it cannot diverge from execution. Trail of Bits' writeup doesn't dwell on this, but it is the fix that matters.

## Generalization

Every verifiable-compute system has a guest program and a reported statistic. If the statistic is computed by the dispatch path rather than by the execution semantics, the same attack applies. Snark-backed private smart contracts, TEE-style remote attestation, proof-of-computation marketplaces, RISC-V ZK prover farms — all of them have a guest, and all of them can ship a guest that reports a number nobody actually computed.

The defense is not "audit the guest." The defense is: make the reported statistic a function of the execution, not of the dispatch. Make the counter intrinsic to the gate definition, so that executing a gate necessarily increments the right counter. Make the table layout explicit, so that out-of-range opcodes are syntax errors rather than silent aliases. Treat the guest program as a small, adversarial-grade specification — which is what it is.

For the Trail of Bits attack specifically, four H100s and four hours were enough to produce a Groth16 proof that Google's original zkVM would have accepted as valid evidence of a circuit that beat their own best results. If you had been making a strategic decision based on that attestation — say, a Q-Day timing estimate, or a migration schedule to PQC — you would have made the wrong decision.

The takeaway for defenders is not that Groth16 is weak. It is that Groth16 is exactly as strong as the guest program, and the guest program was not strong.

## Credits

Trail of Bits published the forgery on April 17, 2026. Their [blog post](https://blog.trailofbits.com/2026/04/17/we-beat-googles-zero-knowledge-proof-of-quantum-cryptanalysis/) and [PoC repo](https://github.com/trailofbits/quantum-zk-proof-poc) include the full circuit generator, the prover patch, and a 142 MB serialized witness. Google's version-2 artifacts, at the same Zenodo DOI, contain the bounds-check fix.
