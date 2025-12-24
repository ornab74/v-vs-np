# Part I — Definitions and Computational Model
**Date:** 2025-12-24

## 1. Purpose
This document defines the exact computational model used throughout the paper.
All later theorems are proved *only* within this model.

No complexity-class separation is claimed here.

---

## 2. Block-Structured Inputs
An input consists of:
- A graph H on vertex set [B]
- A tuple of CNF formulas Φ = (φ₁,…,φ_B) with disjoint variable sets
- Optional hash seed S

Each φᵢ forms a **block**. Circuits may inspect bits across blocks, but locality will be audited.

---

## 3. The Language L_mix

For fixed parameter t:

L_mix(H,Φ)=1 iff  
there exists C⊆[B], |C|=t, such that:
- C is a clique in H
- SAT(φᵢ)=1 for all i∈C

Witness = (C, satisfying assignments).

---

## 4. Hashed Variant L_mix^hash

Add a seed S defining a pairwise-independent hash h_S.

L_mix^hash(H,Φ,S)=1 iff  
∃ witness C with h_S(C)=0^m.

This is an NP language with polynomial-time verification.

---

## 5. Audit-Respecting Circuits (ARC)

A circuit family is **audit-respecting** if it passes all audits:
- existential self-reduction
- witness verification
- renaming invariance
- star / pair locality
- resampling closure
- focus normal form
- associativity tests

Audits are enforced probabilistically with error δ(n).

---

## 6. LocalNOT Circuits

A circuit is LocalNOT(r) if every NOT gate depends on at most r blocks.

LocalNOT is strictly weaker than monotonicity but strong enough for restriction arguments.

---

## 7. Scope
All unconditional lower bounds proven later apply to ARC circuits.
No claim is made yet about all polynomial-size circuits.
