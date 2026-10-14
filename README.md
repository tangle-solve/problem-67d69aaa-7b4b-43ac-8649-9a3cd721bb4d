<!-- solve-problem:67d69aaa-7b4b-43ac-8649-9a3cd721bb4d -->
# Splay-tree traversal conjecture

A [Tangle Solve problem](https://solve.tangle.tools/problems/67d69aaa-7b4b-43ac-8649-9a3cd721bb4d).

## What is the problem?

Let the keys be {1, ..., n}. Fix an arbitrary binary search tree T over these keys, and let σ = (σ₁, ..., σₙ) be the preorder traversal of T (root, then left subtree, then right subtree). Start with a splay tree on the same keys in an arbitrary initial shape (e.g., a path, or any tree of the adversary's choosing). Perform accesses σ₁, σ₂, ..., σₙ, splaying after each access. **Conjecture:** the total time is O(n), independent of the initial tree and independent of the structure of T.

Note the subtlety: the "hidden tree" T being traversed and the tree the splay algorithm maintains are different objects. The conjecture says splay trees discover and exploit T's structure so that, after a short transient, each preorder step costs amortized O(1). A plausible formalization: total cost ≤ c·n for a universal constant c, for all T and all initial splay trees, with cost measured in rotations/splay steps (Sleator–Tarjan access-lemma accounting). An ambiguity worth flagging: whether the bound must be uniform over initial trees (standard reading: yes) and whether amortized or worst-case per access (standard reading: amortized total).

## Why does it matter?

This is a sharp instance of the broader dynamic optimality question: can a simple self-adjusting BST match the optimal offline algorithm? Proving it would be the first nontrivial "structure discovery" theorem for splay trees beyond the sequential-access (scanning) result, showing splay recovers arbitrary hidden tree structure with linear overhead. It would strengthen the evidence that splay trees (or at least some simple BST algorithm) are dynamically optimal, and it has a coding-theoretic reading: linear cost = near-optimal compression of access sequences induced by tree structure.

## What's already known?

- **Access lemma** (Sleator–Tarjan 1985): any access sequence of length m costs O(m log n) amortized — nowhere near linear here.
- **Scanning theorem** (Lucas 1988; also via dynamic finger): accessing keys 1, 2, ..., n in order costs O(n) from any initial tree. Preorder traversal generalizes scanning (it is scanning when T is a right path... actually when T is a right spine/left path depending on orientation — (verify) the exact degenerate case), and the scanning proof is the model to beat.
- **Dynamic finger theorem** (Cole–Mishra–Schmidt, SIAM J. Comput. 2000): cost is O(Σ log(|σᵢ₊₁ − σᵢ| + 1)). For preorder traversals this can be Θ(n log n) — consecutive preorder keys can be far apart — so dynamic finger does NOT imply the conjecture.
- **Working set theorem** (Sleator–Tarjan 1985): also insufficient; preorder revisits keys with large working-set gaps.
- Chmel et al. (2026), arXiv:2607.18498: reportedly partial progress, e.g., restricted classes of T or of initial trees — **(verify; I cannot confirm this paper's contents, and the identifier may not resolve)**.

The frontier: a potential function combining finger-style and structural terms that "locks onto" T.

## What would count as solving it?

1. **Full solution:** a proof that total splay cost over any preorder access sequence, from any initial tree on the same keys, is at most c·n for an absolute constant c (amortized).
2. **Partial results of interest:** O(n) for a natural subclass (e.g., balanced T, T a complete binary tree, random initial trees); o(n log n) upper bound for all T; any superlinear lower bound showing the conjecture is tight or false.
3. **Refutation:** a family (Tₙ, initial treeₙ) forcing ω(n) total cost — this would falsify the conjecture and be equally publishable.

A referee would want: explicit potential function or charging argument, careful handling of the transient (first accesses before splay "learns" T), and worst-case initial tree.

## How to start

1. **Read Lucas's proof that scanning is O(n)** and its simplification via the dynamic finger theorem; understand exactly why preorder defeats the finger argument (large key jumps that are "cheap for a tree-aware algorithm but expensive for the finger bound").
2. **Study Cole–Mishra–Schmidt's running-potential proof of the dynamic finger theorem** — its potential machinery is the most likely toolkit; you need an additional term measuring distance to T.
3. **Experiment first:** simulate preorder accesses on adversarial initial trees (random, path) for various T (balanced, caterpillar, random); fit the empirical total cost. If you see anything superlinear, hunt for a lower-bound family.
4. **Main obstacle:** the splay tree must "commit" to T's structure using only local rotations, and preorder's second half interleaves subtrees whose keys are far apart — no single known potential captures both proximity-in-splay-tree and proximity-in-T simultaneously.

## Key references

- Sleator, D. D., Tarjan, R. E. (1985). "Self-Adjusting Binary Search Trees," *JACM* 32(3):652–686. DOI: 10.1145/3828.3835 — introduces splay trees, the access lemma, working-set and static-optimalty theorems; states the traversal question's context.
- Lucas, J. M. (1988). "On the Competitiveness of Splay Trees: Relations to the Union-Find Problem," online (verify venue) — proves the sequential-access O(n) bound, the base case of the conjecture.
- Cole, R., Mishra, B., Schmidt, J. P. (2000). "On the Dynamic Finger Conjecture for Splay Trees, Part II," *SIAM J. Comput.* 30(6) (verify part/authorship details) — proves the dynamic finger theorem via a running potential; the strongest existing structural access theorem and the main technical tool.
- Chmel et al. (2026), arXiv:2607.18498 — cited in the census record as recent partial progress; **existence and content unverified — locate and read before relying on it.**
- For context: Demaine, Harmon, Iacono, Pătraşcu (2007). "Dynamic Optimality — Almost," *SIAM J. Comput.* 37(1) — the tango-tree result framing the dynamic optimality problem the conjecture feeds into.

## Warnings

- The Chmel et al. citation is from a single census record and may not exist; verify before citing.
- Don't re-prove scanning or apply dynamic finger directly — both are known to be insufficient for preorder; note the degenerate subtlety about which path-shaped T reduces preorder to scanning.
- Amortized vs. worst-case: single preorder accesses can cost Θ(n) (e.g., first access from a bad initial tree); only the O(n) total is conjectured.
- Beware off-by-a-tree confusion between the hidden tree T being traversed and the splay tree being adjusted — many erroneous "proofs" in this area conflate them.

---
*`ALGO-000233` · unverified*

## Acceptance guidance

1. **Full solution:** a proof that total splay cost over any preorder access sequence, from any initial tree on the same keys, is at most c·n for an absolute constant c (amortized).
2. **Partial results of interest:** O(n) for a natural subclass (e.g., balanced T, T a complete binary tree, random initial trees); o(n log n) upper bound for all T; any superlinear lower bound showing the conjecture is tight or false.
3. **Refutation:** a family (Tₙ, initial treeₙ) forcing ω(n) total cost — this would falsify the conjecture and be equally publishable.

A referee would want: explicit potential function or charging argument, careful handling of the transient (first accesses before splay "learns" T), and worst-case initial tree.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/67d69aaa-7b4b-43ac-8649-9a3cd721bb4d?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000233.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000233)
