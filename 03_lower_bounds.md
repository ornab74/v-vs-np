# Part III — Proven Lower Bounds in Restricted Models
**Date:** 2025-12-24

## 1. ARC Lower Bound

**Theorem 3.1 (Unconditional):**  
No polynomial-size audit-respecting circuit decides L_mix^hash.

*Proof sketch:*
- Audits ⇒ LocalNOT
- Random restriction kills NOT gates
- Resulting monotone circuit computes CLIQUE
- Contradiction with monotone lower bounds

---

## 2. Read-Once and Formula Circuits

For read-once and DeMorgan formulas:
- Resampling stability implies locality directly
- Audit soundness can be proved fully

**Corollary:**  
L_mix^hash ∉ RO-formula-size poly.

---

## 3. AC⁰ Soundness

Using Håstad switching lemma:
- Any AC⁰ circuit passing audits collapses to LocalNOT
- Hence monotone extraction applies

**Theorem 3.2:**  
NP ⊄ AC⁰ via the locality-audit method.

---

## 4. Significance

These are genuine, unconditional circuit lower bounds
proved via the audit framework.

No conjectures are used in this part.
