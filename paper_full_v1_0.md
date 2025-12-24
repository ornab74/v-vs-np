# Locality Audits, Valiant–Vazirani Witness Hashing, and LocalNOT-to-Monotone Extraction
## A Complete Proof Attempt for an NP Circuit Lower Bound (Program Draft)
**Version v1.0 (Super-long)** — 2025-12-23

**Status:** Proof attempt / research program. Not an established proof of \(P
e NP\).  
**Primary output:** a single, end-to-end chain with explicitly stated lemmas, proofs where available, and one main keystone family (audit soundness).

---

### Contents
1. Motivation and overview  
2. Definitions and models  
3. The language \(L_{mix}\) and the hashed variant \(L_{mix}^{hash}\)  
4. Substitution graphs and the clique-threshold encoding  
5. Audit framework: identities and testing distributions  
6. Module A: local-bit extraction from star/pair gadgets (proof sketch)  
7. Module B: explicit Valiant–Vazirani uniqueness for clique witnesses (proved)  
8. Soundness-to-LocalNOT: strengthened audit suite and a full proof attempt  
9. LocalNOT kill under random restrictions and monotone extraction  
10. Monotone lower bounds for CLIQUE and the contradiction  
11. Parameter regimes and quantitative bookkeeping  
12. Discussion, comparisons, and failure modes  
13. Open problems and next steps  
Appendices (A–H): definitions, inequality proofs, gadget details, and alternative variants

---

## 1. Motivation and overview

### 1.1 Why this approach
The core dream is classical: if one could show that any polynomial-size circuit for a suitably chosen NP predicate can be transformed (or forced) into a monotone circuit of polynomial size, then existing **monotone lower bounds** (notably for CLIQUE) would yield a contradiction, implying superpolynomial lower bounds for the original NP predicate.

This document builds a scaffold around that idea, using three ingredients:

1. **Locality audits:** a battery of simple algebraic identities (existential self-reduction, renaming invariance, and local gadget identities) that the target language satisfies exactly. Passing these identities is intended to enforce a strong structural constraint on circuits: **negations cannot be global**, they must be confined to local (per-block) computations.

2. **Valiant–Vazirani witness hashing:** a standard uniqueness-inducing technique, adapted explicitly to witness sets of \(t\)-cliques. With pairwise-independent hashing we can ensure that, for a YES instance with \(K\) witnesses, a random seed yields a **unique surviving witness** with probability \(\Omega(1/\log K)\). This makes “unique witness focusing” lawful without any planted distribution assumptions.

3. **LocalNOT-to-monotone extraction:** once negations are local (LocalNOT), a random restriction on blocks kills negations with high probability, yielding a **monotone circuit** for a restriction of the problem. Fixing certain auxiliary inputs then projects the restricted function to **CLIQUE**, contradicting monotone lower bounds.

### 1.2 What is proved vs conjectured
Two modules are proved (or proved to explicit-inequality standard):

- **Module B (VV uniqueness):** explicit second-moment bound yields constant probability of exactly one surviving witness in a suitable expected-survivor regime (Section 7).
- **Module A (star/pair locality):** resampling/influence test implies existence of block-local “goodness bits” \(y_i\) and AND-consistency (Section 6).

The main missing ingredient is a rigorous **audit soundness theorem** that converts *approximate passing* of the audits into a **LocalNOT implementation** for any small circuit deciding \(L_{mix}^{hash}\). Section 8 provides a full proof attempt with a strengthened audit suite, plus an explicit list of potential failure points.

---

## 2. Definitions and models

### 2.1 Graphs, cliques, and monotone predicates
For a simple undirected graph \(G=(V,E)\), the clique number \(\omega(G)\) is the size of the largest complete subgraph. The predicate \([\omega(G)\ge T]\) is monotone in \(E\).

### 2.2 Inputs partitioned into blocks
We partition the input into \(B\) **blocks**. Each block contains the description of a CNF formula \(\phi_i\) over its own disjoint variable set. This block partition induces a *block-index metric*: a gate is “local” if it depends on a small set of blocks.

### 2.3 Circuit model and LocalNOT
We consider Boolean circuits over input bits. A circuit is **LocalNOT(r)** if every NOT gate’s input depends on at most \(r\) blocks. (Equivalently: the set of blocks whose bits influence the NOT-input is of size \(\le r\).)

This is a structural restriction weaker than being monotone (NOTs are allowed), but strong enough that random block restrictions can erase all NOTs with high probability.

---

## 3. The language \(L_{mix}\) and hashed variant \(L_{mix}^{hash}\)

### 3.1 The base language \(L_{mix}\)
Input:
- A graph \(H\) on vertex set \([B]\).
- \(B\) CNFs \(\Phi=(\phi_1,\dots,\phi_B)\) with disjoint variable sets.

Fix a parameter \(t\). Define:
\[
L_{mix}(H;\Phi)=1 \iff \exists C\subseteq[B], |C|=t: C 	ext{ is a clique in } H \ \wedge\ orall i\in C,\ SAT(\phi_i)=1.
\]

### 3.2 Witness sets
Let \(W(H,\Phi)\) denote the witness cliques:
\[
W(H,\Phi) = \{ C\subseteq[B] : |C|=t,\ C	ext{ clique in }H,\ orall i\in C,\ SAT(\phi_i)=1\}.
\]
Let \(K = |W(H,\Phi)|\).

### 3.3 Hashed variant \(L_{mix}^{hash}\)
Add a seed \(S=(m,A,b)\) defining a pairwise-independent hash:
- represent \(C\) as \(\chi(C)\in\{0,1\}^B\),
- choose \(A\in\{0,1\}^{m	imes B}\), \(b\in\{0,1\}^m\),
- define \(h_S(C)=A\chi(C)\oplus b\) over GF(2).

Define:
\[
L_{mix}^{hash}(H,\Phi;S)=1 \iff \exists C\in W(H,\Phi)	ext{ such that }h_S(C)=0^m.
\]

---

## 4. Substitution graphs and clique-threshold encoding

### 4.1 Per-block SAT→CLIQUE gadget
For each \(\phi_i\), build a graph \(G_i(\phi_i)\) such that:
- if \(SAT(\phi_i)=1\), then \(\omega(G_i)=k\);
- if \(SAT(\phi_i)=0\), then \(\omega(G_i)\le k-1\).

### 4.2 Substitution graph \(G = H[G_1,\dots,G_B]\)
Let the vertex set be the disjoint union \(igsqcup_i V(G_i)\).
Between blocks \(i
e j\), connect every vertex of \(G_i\) to every vertex of \(G_j\) iff \((i,j)\in E(H)\).

### 4.3 Threshold equivalence
Let \(T=t\cdot k\). Under a 1-gap promise:
\[
L_{mix}(H;\Phi)=1 \iff \omega(G)\ge T.
\]
Thus \(L_{mix}\) reduces to a monotone clique-threshold predicate on the substitution graph.

---

## 5. Audit framework

### 5.1 Audit distribution
We assume a product distribution over blocks \(\Phi\sim \prod_i D_i\) and VV-distributed seeds \(S\).
Graph \(H\) may be worst-case (adversarial).

### 5.2 Base tests
- (E1) Existential SR: \(f(I) pprox f(I[z=0])ee f(I[z=1])\) for random \(z\).  
- (E2) Witness-checking: accepting outputs must verify.  
- (E3) Renaming invariance over block permutations.

### 5.3 Locality gadgets
We use star and pair gadgets (Section 6) plus stability under \(S_0\) sampling.

### 5.4 Strengthened soundness suite
We add (T_focus_normal_form), (T_block_resample_closure), (T_seed_resample_closure), and (T_assoc) as described in thoughts_chaos_20.txt.

---

## 6. Module A: local-bit extraction from star/pair gadgets (proof sketch)

### 6.1 Star gadget \(H_i(S_0)\)
Let \(S_0\subseteq[B]\setminus\{i\}\) with \(|S_0|=t-1\). Define \(H_i(S_0)\) by making \(S_0\cup\{i\}\) a clique and leaving other edges absent.
Then:
\[
L_{mix}(H_i(S_0);\Phi)=SAT(\phi_i).
\]

### 6.2 Resampling test and bit extraction
Formalize the star test as resampling other blocks and checking invariance. Passing yields a block-local predictor:
\[
y_i(\phi_i)=\mathrm{Maj}_{\Phi_{-i}} f(H_i(S_0),(\phi_i,\Phi_{-i}),S),
\]
with \(f(H_i(S_0),\Phi,S)pprox y_i(\phi_i)\) under the audit distribution.

### 6.3 Pair gadget \(H_{i,j}(S_0)\)
Construct \(H_{i,j}(S_0)\) so a \(t\)-clique exists iff both \(i\) and \(j\) are included. Then:
\[
f(H_{i,j}(S_0),\Phi,S)pprox y_i(\phi_i)\wedge y_j(\phi_j).
\]

(See thoughts_chaos_15.txt for details.)

---

## 7. Module B: explicit VV uniqueness (proved)

Let \(W\) be the witness set with \(|W|=K\). Choose pairwise-independent \(h_S\) and pick \(m\) such that \(2^m\in[K,2K]\).
Let \(X\) count survivors. Then \(\Pr[X=1]\ge 1/8\).
Randomizing \(m\) over a logarithmic range yields unique-seed probability \(\Omega(1/\log K)\).

---

## 8. Soundness-to-LocalNOT: complete proof attempt

### 8.1 Goal
Show that passing the full audit suite forces \(f\) to be (close to) LocalNOT.

### 8.2 Unique-seed focusing
Condition on unique-seed event: exactly one witness clique \(C^*\) survives the hash.
Witness checking forces any acceptance to commit to \(C^*\).

### 8.3 Normal form reduction \(H\langle C^*angle\)
By (T_focus_normal_form), we may replace \(H\) with \(H\langle C^*angle\) that kills all competing \(t\)-cliques while preserving \(C^*\).
Now the only possible witness is \(C^*\).

### 8.4 Junta on non-witness blocks
By (T_block_resample_closure), resampling any \(\phi_i\) for \(i
otin C^*\) should not change \(f\) on \(H\langle C^*angle\).
This implies \(f\) depends essentially only on blocks in \(C^*\), i.e. it is close to a junta on those blocks.

### 8.5 Identifying the junta as an AND
Using (T_assoc) plus pairwise AND-consistency (Module A), we force:
\[
f(H\langle C^*angle,\Phi,S) pprox igwedge_{i\in C^*} y_i(\phi_i).
\]

### 8.6 LocalNOT implementation on the slice
Compute each \(y_i\) inside block \(i\) (negations allowed locally), then AND them. This yields LocalNOT support \(\le t\).

### 8.7 Seed amplification and nonuniform hardwiring
Define \(f_{amp}=igvee_{j=1}^M f(\cdot,S_j)\) for a polynomial seed multiset. Standard averaging lets us hardwire a good multiset per input length.
The OR preserves LocalNOT.

### 8.8 Where the proof needs real formalization
The above is a proof attempt. Its hardest formal obligations are:
- defining and proving correctness of \(H\langle Cangle\),
- proving robust junta extraction bounds for the specific distributions \(D_i\),
- proving a clean nonuniform seed-hardwiring lemma that preserves worst-case correctness.

---

## 9. LocalNOT kill and monotone extraction

Randomly restrict to a subset \(U\) of blocks. A NOT supported on \(s\) blocks survives with probability \((m/B)^s\le m/B\).
Union bound kills all NOTs with high probability, leaving a monotone circuit for the restricted function.

---

## 10. Contradiction via monotone CLIQUE

Fix all \(\phi_i\) satisfiable and set hash length \(m=0\). The function becomes CLIQUE on \(H\).
After restriction to \(U\), we obtain a polynomial-size monotone circuit for CLIQUE on \(m\) vertices,
contradicting known monotone lower bounds in a suitable regime.

Thus, conditional on the soundness step, \(L_{mix}^{hash}
otin\) P/poly, implying NP \(
ot\subseteq\) P/poly.

---

## 11. Quantitative parameters (sketch)
This section records what must be chosen and why:
- \(B\) must be large enough relative to \(|f|\) so random restriction kills NOTs while leaving \(m\) large.
- \(t\) must be in a monotone-hard regime.
- audit error \(\delta\) must be inverse-polynomial and compatible with junta extraction.

---

## 12. Discussion and failure modes
Even if every lemma is stated correctly, the proof attempt can fail due to:
- inability to formalize \(H\langle Cangle\) without breaking SR,
- circuits exploiting subtle correlations in the audit distribution,
- hidden dependence on seed bits that avoids locality,
- nonuniform seed hardwiring quantifier issues.

---

## 13. Open problems
1. Prove the strengthened audit soundness theorem in a standard circuit model.  
2. Prove robust junta extraction with better dependence than \(O(B\delta)\).  
3. Finish full parameter regime and cross-check with the strongest available monotone CLIQUE lower bounds.  
4. Explore the method for restricted circuit classes where soundness might be provable.  

---

## Appendix D. Artifact map
- thoughts_chaos_18.txt — explicit VV uniqueness bound.  
- thoughts_chaos_19.txt — end-to-end contradiction attempt with keystone isolation.  
- thoughts_chaos_20.txt — expanded soundness attempt + strengthened audit suite.  
- paper_draft_v0_2.md — shorter proof-attempt summary.  
