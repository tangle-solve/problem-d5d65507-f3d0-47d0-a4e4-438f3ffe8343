<!-- solve-problem:d5d65507-f3d0-47d0-a4e4-438f3ffe8343 -->
# Near-linear exact Euclidean MST in higher fixed dimension

A [Tangle Solve problem](https://solve.tangle.tools/problems/d5d65507-f3d0-47d0-a4e4-438f3ffe8343).

## What is the problem?

Given a set \(P\) of \(n\) points in \(\mathbb{R}^d\), compute the **exact** Euclidean minimum spanning tree (EMST): the spanning tree of the complete graph on \(P\) with edge weights equal to Euclidean distances that has minimum total weight. The target is a deterministic or randomized algorithm running in \(O(n \,\mathrm{polylog}\, n)\) time for every **fixed** \(d \ge 3\).

**Important subtlety / ambiguity.** Read literally with "\(d\) fixed," the problem is essentially solved: Callahan–Kosaraju's well-separated pair decomposition (WSPD) yields an \(O(n \log n)\)-time exact EMST for any fixed \(d\). The constants, however, are exponential in \(d\) (the WSPD has \(O(n)\) pairs with a separation constant growing like \(2^{O(d)}\), and the bichromatic closest-pair subroutine costs \(c^d\)). The mathematically meaningful open reading, and the standard one in the literature (cf. TOPP Problem 5, verify the exact numbering), is:

> Achieve \(O(n\,\mathrm{polylog}\,n)\) time with constants depending only **polynomially** on \(d\) (near-linear time that does not blow up exponentially as the "fixed" dimension grows), or demonstrate that exponential-in-\(d\) dependence is unavoidable.

Model: word RAM or real RAM with arithmetic on \(O(d\log n)\)-bit coordinates; exact means no approximation, no reliance on randomization that fails with nonzero probability unless Las Vegas. In the plane, \(O(n\log n)\) is optimal via a reduction from element uniqueness.

## Why does it matter?

Exact EMSTs underpin single-linkage clustering, network design over geographic or feature-space point sets, and are a subroutine in TSP heuristics, surface reconstruction, and geometric matching. Practical instances routinely have \(d\) in the tens (feature vectors), where exponential-in-\(d\) constants make the "near-linear" theory vacuous, pushing practitioners to heuristics or approximate schemes with no correctness guarantee. Resolving the problem either gives genuinely scalable exact clustering/network construction, or proves a structural barrier separating the plane from higher dimensions.

## What's already known?

- **Plane (\(d=2\)):** \(O(n\log n)\) via Delaunay triangulation (the EMST is a subgraph of it); this is optimal under algebraic decision tree lower bounds (\(\Omega(n\log n)\) from element uniqueness).
- **Fixed \(d \ge 3\):** Callahan–Kosaraju (1995) construct a WSPD of size \(O(n)\) (constant \(\approx c \cdot 2^d\)) in \(O(n \log n)\) time, and derive an exact EMST in \(O(n \log n)\) time — but with \(2^{O(d)}\) or worse constant factors. Delaunay-based methods degrade to \(n^{\lceil d/2\rceil}\).
- **Why they fall short:** the WSPD separation parameter needed to certify edge weights grows geometrically in \(d\); bichromatic closest pair in high dimension has no known near-linear exact algorithm with polynomial \(d\)-dependence (and under SETH-style assumptions, approximate nearest neighbor requires near-\(n^2\) or \(2^{\Omega(d)}\) time — though these lower bounds concern approximation and don't directly apply to exact EMST).
- Tried and insufficient: dimension-reduction (distorts exact weights — invalid), \(k\)-d trees / quadtree-based pruning (degenerate in high dimension), sparsification via approximate nearest-neighbor graphs (gives only approximate EMST).

## What would count as solving it?

Any of:
1. **Upper bound (main goal):** an algorithm computing the exact EMST in \(O(f(d)\, n \,\mathrm{polylog}\,n)\) time with \(f\) polynomial (or even \(2^{o(d)}\)), for all \(d\). Referee criterion: full correctness proof that the tree is exactly minimum (not approximate), worst-case running-time bound as stated.
2. **Lower bound:** a proof (e.g., in the algebraic computation tree or conditional-on-ETH/SETH model) that exact EMST requires \(2^{\Omega(d)}\,n\) or \(\omega(n\,\mathrm{polylog}\,n)\) time when constants must be dimension-efficient.
3. **Interpretation with \(d\) truly constant:** any improvement of the \(O(n\log n)\) CK bound to \(O(n)\) for \(d\ge 3\) would also resolve the literal census statement (though \(\Omega(n\log n)\) lower bounds make this unlikely; verify the extent of the lower bound beyond \(d=2\)).

## How to start

1. Read Callahan & Kosaraju (1995) Sections on WSPD construction and the EMST application; track exactly where \(d\)-dependence enters (separation constant, the bichromatic closest-pair calls).
2. Study exact bichromatic closest pair in high dimension — the EMST reduces to \(O(n)\) such queries on WSPD pairs; any near-linear dimension-efficient exact closest-pair would be the key lever. Check Agarwal–Sharir's handbook survey on nearest-neighbor searching for the current frontier (verify exact reference).
3. Consider whether the EMST's structural property (each point's few nearest "well-separated" neighbors suffice) can be certified with fewer, coarser comparisons, exploiting MST exchange properties rather than exact distances.
4. For a lower-bound attack: try reducing exact closest pair / element distinctness with dimension as a parameter, forcing \(d\) bits per coordinate.

Main obstacle: exactness kills all dimension-reduction and ANN machinery, which is the only known way to tame the curse of dimensionality.

## Key references

- P. B. Callahan and S. R. Kosaraju. "A Decomposition of Multidimensional Point Sets with Applications to \(k\)-Nearest-Neighbors and \(n\)-Body Potential Fields." *J. ACM* 42(1):67–90, 1995. Introduces the WSPD and gives the \(O(n\log n)\) exact EMST for fixed \(d\) with exponential-in-\(d\) constants. https://doi.org/10.1145/200836.200853
- The Open Problems Project (Demaine, Mitchell, O'Rourke, eds.), Problem 5 ("Euclidean minimum spanning tree," verify numbering/title). States the open status of near-linear exact EMST with dimension-efficient bounds. https://cs.smith.edu/~jorourke/TOPP/
- J. Erickson, "On the relative complexities of some geometric problems" (verify): establishes \(\Omega(n\log n)\) lower-bound techniques in algebraic decision trees relevant to proving optimality in the plane.

## Warnings

- The census statement is misleading as literally written: for each *fixed* \(d\), near-linear exact EMST is known. Clarify with any requester whether the intended problem is dimension-efficient near-linearity; otherwise you risk attacking a solved problem.
- Do not use Johnson–Lindenstrauss or ANN-based sparsification: they destroy exactness.
- Beware claiming lower bounds from SETH for *approximate* NN as if they applied to exact EMST — the reductions do not transfer directly.
- "Near-linear" for fixed \(d\) hides constants; be explicit about which complexity measure (dependence on \(d\)) your result addresses.

---
*`ALGO-000161` · unverified*

## Acceptance guidance

Any of:
1. **Upper bound (main goal):** an algorithm computing the exact EMST in \(O(f(d)\, n \,\mathrm{polylog}\,n)\) time with \(f\) polynomial (or even \(2^{o(d)}\)), for all \(d\). Referee criterion: full correctness proof that the tree is exactly minimum (not approximate), worst-case running-time bound as stated.
2. **Lower bound:** a proof (e.g., in the algebraic computation tree or conditional-on-ETH/SETH model) that exact EMST requires \(2^{\Omega(d)}\,n\) or \(\omega(n\,\mathrm{polylog}\,n)\) time when constants must be dimension-efficient.
3. **Interpretation with \(d\) truly constant:** any improvement of the \(O(n\log n)\) CK bound to \(O(n)\) for \(d\ge 3\) would also resolve the literal census statement (though \(\Omega(n\log n)\) lower bounds make this unlikely; verify the extent of the lower bound beyond \(d=2\)).

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/d5d65507-f3d0-47d0-a4e4-438f3ffe8343?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000161.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000161)
