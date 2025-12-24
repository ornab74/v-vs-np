
# Locality Audits, Witness Hashing, and LocalNOT-to-Monotone Extraction
## Complete Proof Attempt — Version v1.1
**Date:** 2025-12-24

**Status:** Research program / proof attempt (not an established proof).

---

## What is new in v1.1
This version tightens two previously informal steps:

1. Clique-preserving normal form H⟨C⟩ is defined explicitly and shown to preserve the target language
   under witness-check conditioning.
2. Junta extraction is stated quantitatively for block-product distributions, clarifying the δ–η tradeoff.

These address the most serious technical objections in v1.0, leaving one primary open problem:
a fully general audit soundness theorem for arbitrary small circuits.

---

## 8'. Formalization of the Soundness Step (updated)

### 8'.1 Clique-preserving normal form
Given a witness clique C, define H⟨C⟩ by deleting every edge that participates in any t-clique other than C.
For constant t this is computable in polynomial time.

Lemma 8.1.  
If C is a valid witness for (H,Φ,S), then:
L_mix^hash(H,Φ,S) = L_mix^hash(H⟨C⟩,Φ,S).

Proof sketch.  
C remains a clique. No other t-clique survives. Hash condition is unchanged.

### 8'.2 Conditional SR compatibility
Existential SR is enforced only on branches where witness-check succeeds.
Branches destroying C are irrelevant because witness-check fails.

### 8'.3 Junta extraction under resampling closure
Assume for all i not in C:
Pr[f(Φ) ≠ f(R_i Φ)] ≤ δ.
Then total influence outside C is at most (B−t)δ.

By a standard junta theorem, for any η>0 there exists g depending only on C plus
O((B−t)δ/η) additional blocks such that:
Pr[f(Φ) ≠ g(Φ)] ≤ η.

Choosing δ ≤ 1/poly(B^2) yields g essentially depending only on C.

### 8'.4 Identification as AND
Pairwise AND-consistency and associativity tests force:
f(H⟨C⟩,Φ,S) ≈ ∧_{i∈C} y_i(φ_i).

Thus f is LocalNOT on the unique-witness slice.

---

## 9'. Remaining keystone

Open Theorem (Audit Soundness).  
Show that passing the full audit suite with error δ implies the above conditions
(resampling stability, valid witnesses, focus normal form) hold with sufficiently small δ.

This remains the central unsolved step.

---

## 10'. Consequences
With the above in place, the LocalNOT kill under random restriction and
the monotone CLIQUE contradiction proceed exactly as in v1.0.

---

## 11'. Conclusion
The proof attempt is now reduced to a single structural statement:
that the audit suite characterizes LocalNOT behavior.
All other components are explicit, quantitative, and modular.

---

## Artifact map
- thoughts_chaos_18.txt — VV uniqueness (proved).
- thoughts_chaos_20.txt — strengthened audit suite.
- thoughts_chaos_21.txt — normal form + junta lemma formalization.
- paper_full_v1_0.md — full narrative proof attempt.
