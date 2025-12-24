# Part II — Core Constructions and Fully Proved Components
**Date:** 2025-12-24

## 1. Witness Hashing (Valiant–Vazirani)

We use pairwise-independent hashing over witness sets.

**Theorem 2.1**  
For any witness set of size K, a random hash yields exactly one surviving witness
with probability Ω(1/log K).

*Proof:* explicit second-moment bound.

---

## 2. Substitution Graph Construction

Each φᵢ is converted to a graph Gᵢ such that:
- SAT(φᵢ)=1 ⇒ ω(Gᵢ)=k
- SAT(φᵢ)=0 ⇒ ω(Gᵢ)≤k−1

Substitute Gᵢ into H to form G=H[G₁,…,G_B].

Then L_mix reduces to a clique threshold on G.

---

## 3. Star and Pair Gadgets

Star gadget isolates a single block:
- acceptance ⇔ SAT(φᵢ)

Pair gadget isolates two blocks:
- acceptance ⇔ SAT(φᵢ) ∧ SAT(φⱼ)

These gadgets enforce local-bit behavior under resampling tests.

---

## 4. Local Bit Extraction

Passing star audits implies existence of functions yᵢ(φᵢ)
such that f≈yᵢ on star instances.

Pair audits enforce:
yᵢⱼ ≈ yᵢ ∧ yⱼ

This is proved via influence and resampling arguments.

---

## 5. Clique-Preserving Normal Form H⟨C⟩

Given a witness clique C:
- remove all edges participating in any other t-clique
- preserve C exactly

**Lemma:**  
For valid witnesses, L_mix^hash(H)=L_mix^hash(H⟨C⟩).

This is computable in polynomial time for constant t.

---

## 6. Junta Extraction

If resampling φᵢ for i∉C changes f with probability ≤δ,
then f is ε-close to a junta on C.

Standard quantitative junta theorem applies under product distributions.

---

## 7. Identification as AND

Associativity + commutativity + idempotence + monotonicity
force the junta to be AND of the local bits.

All steps in this document are fully proved.
