# Part IV — The Audit Soundness Conjecture and Full Extension
**Date:** 2025-12-24

## 1. The Remaining Barrier

All results so far depend on one missing implication:

> Passing the full audit suite ⇒ LocalNOT

This is not known for general circuits.

---

## 2. Audit Soundness Conjecture (ASC)

**Conjecture:**  
Any polynomial-size Boolean circuit that passes all locality audits
with inverse-polynomial error is LocalNOT.

---

## 3. Consequence of ASC

**Theorem (Conditional):**  
ASC ⇒ NP ⊄ P/poly.

*Reason:*  
Once LocalNOT holds, monotone extraction yields a contradiction
with monotone CLIQUE lower bounds.

---

## 4. Why This Is Hard

General circuits may:
- Encode global negations via correlated gates
- Hide dependence on hash seeds
- Exploit non-product distributions internally

No existing lower-bound technique resolves this.

---

## 5. Why This Is Progress

- P vs NP is reduced to **one explicit structural conjecture**
- All other components are complete and modular
- Any partial proof of ASC yields new lower bounds

---

## 6. Outlook

Future work directions:
- Prove ASC for bounded-depth circuits
- Strengthen audits into full characterizations
- Search for counterexamples to ASC

This concludes the framework.
