# Competitive Programming Toolkit & C++ Library

A high-performance, battle-tested competitive programming library and workspace featuring **274 production-grade C++ snippets** organized across **36 modular files in 5 categories**, automated compilation scripts with strict GCC sanitizer enforcement, automated differential stress-testing tooling, and editor optimizations for VS Code.

---

## Table of Contents

- [Overview](#overview)
- [Repository Structure](#repository-structure)
- [Featured Components](#featured-components)
  - [1. Advanced Coordinate Compression (`Compressor`)](#1-advanced-coordinate-compression-compressor)
  - [2. Persistent Segment Trees & Lazy PST](#2-persistent-segment-trees--lazy-pst)
  - [3. Dynamic & Static Merge Sort Trees](#3-dynamic--static-merge-sort-trees)
  - [4. Ultra-Fast Collision-Safe Hash Table (`fast_hash_table`)](#4-ultra-fast-collision-safe-hash-table-fast_hash_table)
  - [5. Portable Treap-Based Ordered Set & Multiset](#5-portable-treap-based-ordered-set--multiset)
  - [6. High-Performance Algebra & Polynomials](#6-high-performance-algebra--polynomials)
- [Modular Snippets Catalog (274 Snippets across 36 Files)](#modular-snippets-catalog-274-snippets-across-36-files)
  - [Category Overview](#category-overview)
  - [Detailed Catalog by Category](#detailed-catalog-by-category)
- [Developer Tools & Automation](#developer-tools--automation)
  - [1. Build & Run Helper (`tools/run.sh`)](#1-build--run-helper-toolsrunsh)
  - [2. Automated Differential Stress Tester (`tools/stress.sh`)](#2-automated-differential-stress-tester-toolsstresssh)
- [VS Code Integration & Settings](#vs-code-integration--settings)
- [Recommended Contest Workflow](#recommended-contest-workflow)
- [Prerequisites & Setup](#prerequisites--setup)

---

## Overview

Designed for competitive programming on platforms like **Codeforces**, **AtCoder**, **CSES**, **CodeChef**, and **USACO**:

- **Safety-First Local Execution**: Catches segmentation faults, out-of-bounds indexing, memory leaks, and undefined behavior locally via AddressSanitizer (`-fsanitize=address,undefined`) and GLIBCXX debug checks (`-D_GLIBCXX_DEBUG`).
- **Differential Stress Testing**: Automated verification of candidate solutions against slow brute-force implementations on randomized inputs, producing minimal failing testcases on failure.
- **274 Modular Snippets across 36 Categories**: Logically organized snippet files with clear, predictable triggers for instant tab-completion during timed contests.
- **Modern C++17/20 Implementation**: Optimized for clean interfaces, minimal boilerplate, zero-overhead abstraction, and fast execution.

---

## Repository Structure

```text
.
├── .vscode/
│   ├── settings.json                           # VS Code snippet and editor configuration
│   ├── algorithms/                             # Core algorithms & techniques (6 files, 50 snippets)
│   │   ├── cp-backtracking.code-snippets
│   │   ├── cp-dp.code-snippets
│   │   ├── cp-searching.code-snippets
│   │   ├── cp-sorting.code-snippets
│   │   ├── cp-strings.code-snippets
│   │   └── cp-techniques.code-snippets
│   ├── data-structures/                        # Advanced data structures (17 files, 120 snippets)
│   │   ├── cp-advanced-trees.code-snippets
│   │   ├── cp-dsu.code-snippets
│   │   ├── cp-fenwick.code-snippets
│   │   ├── cp-hash.code-snippets
│   │   ├── cp-heap.code-snippets
│   │   ├── cp-lazy-segtree.code-snippets
│   │   ├── cp-lichao-tree.code-snippets
│   │   ├── cp-linked-list.code-snippets
│   │   ├── cp-merge-sort-tree.code-snippets
│   │   ├── cp-ordered-set.code-snippets
│   │   ├── cp-pbds.code-snippets
│   │   ├── cp-persistent-segtree.code-snippets
│   │   ├── cp-queue.code-snippets
│   │   ├── cp-segtree.code-snippets
│   │   ├── cp-sparse-table.code-snippets
│   │   ├── cp-stack.code-snippets
│   │   └── cp-trie.code-snippets
│   ├── graphs/                                 # Graph theory & trees (4 files, 26 snippets)
│   │   ├── cp-graph-connectivity.code-snippets
│   │   ├── cp-graph-flows.code-snippets
│   │   ├── cp-graph-paths.code-snippets
│   │   └── cp-graph-trees.code-snippets
│   ├── math/                                   # Number theory, algebra, geometry (8 files, 75 snippets)
│   │   ├── cp-algebra-polynomials.code-snippets
│   │   ├── cp-combinatorics.code-snippets
│   │   ├── cp-diophantine-floorsum.code-snippets
│   │   ├── cp-divisors-gcd.code-snippets
│   │   ├── cp-geometry.code-snippets
│   │   ├── cp-mobius-harmonic.code-snippets
│   │   ├── cp-modular-arithmetic.code-snippets
│   │   └── cp-primes-sieves.code-snippets
│   └── templates/                              # Contest boilerplates & prelude (1 file, 3 snippets)
│       └── cp-templates.code-snippets
├── tools/
│   ├── run.sh                                  # Compilation runner (sanitizers or fast -O2)
│   └── stress.sh                               # Automated differential stress tester
└── README.md
```

---

## Featured Components

### 1. Advanced Coordinate Compression (`Compressor`)

Prefixes: `cp-coordinate-compression`, `cp-compress`

An idiomatic, cache-friendly template struct that avoids repetitive binary searches by precomputing ranks for all array elements in $\mathcal{O}(1)$ time, while offering dedicated methods for dynamic and arbitrary query values.

```cpp
template <class T> struct Compressor {
    vector<T> vals;   // sorted distinct values (reverse lookup: vals[rank])
    vector<int> rnk;  // rnk[i] = rank of a[i] in O(1) without binary search

    Compressor() = default;
    Compressor(const vector<T> &a) { init(a); }

    void init(const vector<T> &a);
    int size() const;
    int operator[](int i) const;          // O(1) rank of original array element a[i]
    const T &value(int rank) const;       // O(1) reverse lookup (rank -> original value)

    // Binary search methods for arbitrary / dynamic query values (e.g. k-query):
    int rank(const T &x) const;           // Rank of x if present, else largest rank <= x (-1 if x < min)
    int rank_leq(const T &x) const;       // Largest rank with value <= x (-1 if x < min)
    int rank_lt(const T &x) const;        // Largest rank with value < x (-1 if x <= min)
    int rank_geq(const T &x) const;       // Smallest rank with value >= x (size() if x > max)
    int rank_gt(const T &x) const;        // Smallest rank with value > x (size() if x >= max)
    int rank_exact(const T &x) const;     // Exact rank if present, -1 if absent
};
```

#### Usage Example

```cpp
vector<ll> a = {10, 50, 20, 50, 30};
Compressor<ll> C(a);

// O(1) lookup for array elements:
int r = C[2];             // Rank of a[2] (20) -> 1 (zero binary search!)
ll orig = C.value(1);     // Original value at rank 1 -> 20

// Binary search for dynamic query values:
int r1 = C.rank_leq(25);  // Largest value <= 25 is 20 -> rank 1
int r2 = C.rank_geq(25);  // Smallest value >= 25 is 30 -> rank 2
```

---

### 2. Persistent Segment Trees & Lazy PST

Prefixes: `cp-persistent-segtree`, `cp-pst`, `cp-lazy-pst-flip`, `cp-lazy-pst-sum`

Path-copying persistent segment tree built over value universes with support for static point-update versions, dynamic binary range flips with rolling string hashing, and historical range-add range-sum queries with version rollback:

- **Point Update & Version Indexing (`cp-persistent-segtree`)**: Answer subarray $k$-th smallest, count $\le x$, and range counts $[lo, hi]$ on any historical version in $\mathcal{O}(\log M)$ time.
- **Lazy PST Binary Flips (`cp-lazy-pst-flip`)**: Range bitwise inversion on persistent binary sequences with $\mathcal{O}(\log n)$ lexicographical version comparison.
- **Lazy PST Range Add & Sum (`cp-lazy-pst-sum`)**: Range additions with full historical range sum evaluation and snapshot rollbacks.

```cpp
// 1. Build persistent tree over compressed values:
Compressor<ll> C(a);
PersistentSegTree pst(C.size());
pst.build(C.rnk);

// 2. Query k-th smallest element in subarray a[l ... r]:
ll val = C.value(pst.kth(l, r, k));

// 3. Count elements <= x in subarray a[l ... r]:
int cnt = pst.count_leq(l, r, C.rank_leq(x));
```

---

### 3. Dynamic & Static Merge Sort Trees

Prefixes: `cp-merge-sort-tree`, `cp-point-update-mst`, `cp-pmst`

Two comprehensive implementations for range order statistics:

1. **Static Merge Sort Tree (`cp-merge-sort-tree`)**:
   - $\mathcal{O}(n \log n)$ space with compact `vector<T> tree[]`.
   - $\mathcal{O}(\log^2 n)$ queries for count $\le x$, $< x$, $\ge x$, $> x$, and range count $[x, y]$.
   - $\mathcal{O}(\log^3 n)$ range $k$-th smallest element search via binary search.
2. **Point-Update Dynamic Merge Sort Tree (`cp-point-update-mst`)**:
   - Replaces static sorted vectors with GNU PBDS `ordered_set` of pairs `(value, unique_id)`.
   - Supports online point updates $a[idx] \leftarrow \text{new\_val}$ in $\mathcal{O}(\log^2 n)$.
   - $\mathcal{O}(\log^2 n)$ dynamic order counts and $\mathcal{O}(\log^3 n)$ dynamic range $k$-th smallest queries.

---

### 4. Ultra-Fast Collision-Safe Hash Table (`fast_hash_table`)

Prefixes: `cp-fast-hash-table`, `cp-fast-map`, `cp-gp-hash-table`, `cp-fast-hash-init`, `cp-fast-set-init`

A high-performance wrapper around GCC Policy-Based Data Structure `__gnu_pbds::gp_hash_table` paired with an irrational-multiplier custom hash (`chash`). Preallocates $2^{16}$ buckets by default to eliminate rehashing overhead while executing operations 3–6$\times$ faster than `std::unordered_map` with defense against hash collision testcases:

```cpp
// Instantiate fast collision-safe hash map:
fast_hash_table<ll, int> mp;          // O(1) avg lookup/insert with 2^16 preallocated buckets
fast_hash_table<ll, int> mp(1 << 20);  // Custom preallocated bucket capacity

// Instantiate fast collision-safe hash set:
fast_hash_set<ll> st;
```

---

### 5. Portable Treap-Based Ordered Set & Multiset

Prefixes: `cp-ordered-set`, `cp-ordered-multiset`, `cp-portable-ordered-set`

A self-contained, header-free randomized Cartesian tree (Treap) implementation of order-statistic trees. Provides 100% portable functionality without any compiler-specific PBDS dependencies:

- Supports both unique `ordered_set` and duplicate-aware `ordered_multiset`.
- Safe single-copy removal in multisets (unlike naive `std::multiset::erase` which deletes all matching values).
- $\mathcal{O}(\log n)$ `kth(k)` (0-indexed $k$-th element) and `rank(val)` (number of strictly smaller elements).
- $\mathcal{O}(\log n)$ lower bound, upper bound, predecessor, successor, and count operations.

---

### 6. High-Performance Algebra & Polynomials

Prefixes: `cp-poly-ntt`, `cp-poly-mtt`, `cp-poly-conv`, `cp-poly-inv`, `cp-poly-bostan-mori`, `cp-alg-xor-basis`, `cp-alg-matrix`, `cp-alg-solve-linear`

Complete coverage of advanced modular arithmetic, linear algebra, and discrete transforms:

- **Number Theoretic Transform (NTT)**: In-place transform modulo $998244353$ with automatic fall-back to schoolbook multiplication for small degrees.
- **Arbitrary Modulus Convolution (MTT / Split FFT)**: Splitting coefficients into $C = 2^{15}$ components and 4 packed complex FFTs to multiply polynomials modulo any arbitrary modulus $M \le 10^9$ (e.g., $10^9+7$).
- **Formal Power Series Inversion**: Newton-Raphson iteration computing $g(x) \equiv f(x)^{-1} \pmod{x^n}$ in $\mathcal{O}(n \log n)$.
- **Fast Linear Recurrences**: Bostan-Mori and Fiduccia's algorithms solving for the $k$-th term of a degree-$d$ recurrence in $\mathcal{O}(d \log d \log k)$, vastly outperforming $\mathcal{O}(d^3 \log k)$ matrix exponentiation for large $d$.
- **Linear XOR Basis**: Minimal basis over $\text{GF}(2)$ for maximum subset XOR, $k$-th smallest XOR in span, and basis reduction.
- **Gaussian Elimination**: Robust row reduction over $\mathbb{Z}_p$ for determining unique, infinite ($p^{N-\text{rank}}$), or inconsistent solutions.

---

## Modular Snippets Catalog (274 Snippets across 36 Files)

### Category Overview

| Category | Snippet File | Count | Primary Focus & Algorithms |
| :--- | :--- | :---: | :--- |
| **Templates** | [cp-templates.code-snippets](.vscode/templates/cp-templates.code-snippets) | **3** | Full contest template, short competitive template, common types, macros, debug printer. |
| **Algorithms** | [cp-backtracking.code-snippets](.vscode/algorithms/cp-backtracking.code-snippets) | **4** | N-Queens, Knight’s Tour, K-Knights, Sudoku solver. |
| | [cp-dp.code-snippets](.vscode/algorithms/cp-dp.code-snippets) | **12** | LIS, LCS, Levenshtein Edit Distance, 0/1 Knapsack, Unbounded Knapsack, Bitmask TSP, SOS DP, Digit DP. |
| | [cp-searching.code-snippets](.vscode/algorithms/cp-searching.code-snippets) | **5** | Monotone predicate binary search (`first-true`), ternary search (integer/real), linear search. |
| | [cp-sorting.code-snippets](.vscode/algorithms/cp-sorting.code-snippets) | **10** | Inversion counting via Merge Sort, Quick Sort, Heap Sort, Counting Sort, Radix Sort, Bucket Sort. |
| | [cp-strings.code-snippets](.vscode/algorithms/cp-strings.code-snippets) | **9** | KMP, Z-Algorithm, Manacher’s palindrome radii, Collision-Resistant Rolling Hash, Suffix Array + LCP, Aho-Corasick. |
| | [cp-techniques.code-snippets](.vscode/algorithms/cp-techniques.code-snippets) | **10** | Coordinate compression (`Compressor`), 2D prefix sums, Kadane's max subarray, sliding window, submask iteration. |
| **Data Structures** | [cp-advanced-trees.code-snippets](.vscode/data-structures/cp-advanced-trees.code-snippets) | **3** | Binary Search Tree (BST), AVL Tree (self-balancing), Red-Black Tree. |
| | [cp-dsu.code-snippets](.vscode/data-structures/cp-dsu.code-snippets) | **6** | Standard DSU (rank/size), Rollback DSU (offline dynamic connectivity), Weighted Potential DSU. |
| | [cp-fenwick.code-snippets](.vscode/data-structures/cp-fenwick.code-snippets) | **6** | Standard 1D Fenwick Tree (BIT), Range-Add Range-Sum BIT, 2D Fenwick Tree (BIT2D). |
| | [cp-hash.code-snippets](.vscode/data-structures/cp-hash.code-snippets) | **8** | Ultra-Fast PBDS `gp_hash_table` (`fast_hash_table`, `fast_hash_set`) with `chash`, Separate Chaining, `splitmix64` hash. |
| | [cp-heap.code-snippets](.vscode/data-structures/cp-heap.code-snippets) | **3** | Binary Min-Heap, Binary Max-Heap with 0-indexed arithmetic and $\mathcal{O}(n)$ build. |
| | [cp-lazy-segtree.code-snippets](.vscode/data-structures/cp-lazy-segtree.code-snippets) | **9** | ACL-style Lazy SegTree: Range Add/Sum, Range Add/Min, Range Set/Sum, Range Affine/Sum. |
| | [cp-lichao-tree.code-snippets](.vscode/data-structures/cp-lichao-tree.code-snippets) | **2** | Dynamic line insertion $y = mx + b$ and point minimum/maximum queries. |
| | [cp-linked-list.code-snippets](.vscode/data-structures/cp-linked-list.code-snippets) | **4** | Singly Linked List, Doubly Linked List, Circular Linked List, ListStack. |
| | [cp-merge-sort-tree.code-snippets](.vscode/data-structures/cp-merge-sort-tree.code-snippets) | **20** | Static Merge Sort Tree & Point-Update Dynamic PBDS Merge Sort Tree with order queries. |
| | [cp-ordered-set.code-snippets](.vscode/data-structures/cp-ordered-set.code-snippets) | **11** | Portable Treap-based `ordered_set` and `ordered_multiset` with rank and $k$-th element queries. |
| | [cp-pbds.code-snippets](.vscode/data-structures/cp-pbds.code-snippets) | **8** | GNU Policy-Based Data Structure Order-Statistics Tree (Set & Multiset) with duplicate erasure. |
| | [cp-persistent-segtree.code-snippets](.vscode/data-structures/cp-persistent-segtree.code-snippets) | **15** | Standard PST, Lazy PST Range Flip & Hash, Persistent Lazy SegTree Range Add & Sum. |
| | [cp-queue.code-snippets](.vscode/data-structures/cp-queue.code-snippets) | **2** | Circular Buffer Queue (FIFO ring buffer), Circular Array Deque. |
| | [cp-segtree.code-snippets](.vscode/data-structures/cp-segtree.code-snippets) | **10** | Iterative SegTree (Monoid): Sum, Min, Max, GCD, Point Set/Add, Range Queries. |
| | [cp-sparse-table.code-snippets](.vscode/data-structures/cp-sparse-table.code-snippets) | **6** | Static Range Minimum Queries (RMQ), Range Max, Range GCD in $\mathcal{O}(1)$ time. |
| | [cp-stack.code-snippets](.vscode/data-structures/cp-stack.code-snippets) | **5** | Dynamic Array Stack, Monotonic Stack (NGE/NSE), Infix to Postfix conversion, Expression evaluation. |
| | [cp-trie.code-snippets](.vscode/data-structures/cp-trie.code-snippets) | **2** | 26-way lowercase Prefix Trie, Binary XOR Trie ($< 2^B$). |
| **Graphs** | [cp-graph-connectivity.code-snippets](.vscode/graphs/cp-graph-connectivity.code-snippets) | **9** | Kruskal, Prim, Borůvka, Tarjan SCC, 2-SAT, Bridges & Articulation Points, Kosaraju SCC, BCC. |
| | [cp-graph-flows.code-snippets](.vscode/graphs/cp-graph-flows.code-snippets) | **3** | Dinic Max Flow & Min Cut, Min-Cost Max-Flow (SPFA), Hopcroft-Karp Maximum Matching. |
| | [cp-graph-paths.code-snippets](.vscode/graphs/cp-graph-paths.code-snippets) | **9** | BFS, Dijkstra, 0-1 BFS, Bellman-Ford, Floyd-Warshall, Kahn Topological Sort, Bipartite Coloring. |
| | [cp-graph-trees.code-snippets](.vscode/graphs/cp-graph-trees.code-snippets) | **5** | Heavy-Light Decomposition (HLD), Centroid Decomposition, LCA (Binary Lifting), Euler Tour. |
| **Math** | [cp-algebra-polynomials.code-snippets](.vscode/math/cp-algebra-polynomials.code-snippets) | **14** | Linear XOR Basis, Matrix Exponentiation, Gaussian Elimination, NTT, MTT, Inversion, Bostan-Mori, Lagrange, String Wildcards. |
| | [cp-combinatorics.code-snippets](.vscode/math/cp-combinatorics.code-snippets) | **12** | Binomial coefficients (multiplicative, Pascal, modulo $p$), Catalan numbers, Permutations, Combinations. |
| | [cp-diophantine-floorsum.code-snippets](.vscode/math/cp-diophantine-floorsum.code-snippets) | **6** | Two-variable and $N$-variable Linear Diophantine solvers, Frobenius problem, Floor Sum $\mathcal{O}(\log m)$. |
| | [cp-divisors-gcd.code-snippets](.vscode/math/cp-divisors-gcd.code-snippets) | **9** | All Divisors (trial & harmonic sieve), Prime Factorization, GCD/LCM, Extended Euclidean algorithm. |
| | [cp-geometry.code-snippets](.vscode/math/cp-geometry.code-snippets) | **8** | Point2D struct, Andrew’s Monotone Chain Convex Hull, Shoelace Polygon Area, Closest Pair of Points. |
| | [cp-mobius-harmonic.code-snippets](.vscode/math/cp-mobius-harmonic.code-snippets) | **7** | Linear Sieve for $\phi$ and $\mu$, Harmonic Lemma quotient chunking ($O(\sqrt{N})$), Mertens function. |
| | [cp-modular-arithmetic.code-snippets](.vscode/math/cp-modular-arithmetic.code-snippets) | **11** | Modular Integer (`mint`), Modular Exponentiation, Modular Inverse, CRT, Baby-Step Giant-Step, Tonelli-Shanks. |
| | [cp-primes-sieves.code-snippets](.vscode/math/cp-primes-sieves.code-snippets) | **8** | Sieve of Eratosthenes, Linear Sieve (SPF), Segmented Sieve, Deterministic 64-bit Miller-Rabin, Pollard's Rho. |
| **Total** | **36 Files** | **274** | **100% Modern C++17/20, Zero Compilation Warnings** |

---

### Detailed Catalog by Category

<details>
<summary><b>1. Templates & Core Prelude (1 file, 3 snippets)</b></summary>

- `cp-template`, `cpplate`: Full competitive programming boilerplate with fast I/O synchronization, multi-testcase loop (`click_clack_boom`), 64-bit/128-bit aliases, and math macros.
- `cp-template-short`: Lean contest boilerplate for quick script testing.
- `cp-common-types`, `cp-prelude`: Type shortcuts (`vll`, `pll`, `vpll`), 4-way and 8-way directional arrays (`dx`/`dy`), and bit manipulation helper macros.

</details>

<details>
<summary><b>2. Algorithms & Techniques (6 files, 50 snippets)</b></summary>

- **Techniques (`cp-techniques`)**:
  - `cp-coordinate-compression`, `cp-compress`: $\mathcal{O}(1)$ rank lookup on array elements with dedicated binary search methods.
  - `cp-prefix-sums-2d`, `pref2d`: 2D prefix sums table with $\mathcal{O}(1)$ rectangle sum queries.
  - `cp-sliding-window`: Monotonic deque sliding window for finding minimum/maximum over all subarrays of length $k$.
  - `cp-kadane`: Kadane's algorithm for maximum subarray sum.
  - `cp-submask-iteration`, `submasks`: Efficient $\mathcal{O}(3^n)$ iteration through all submasks of all masks.
  - `cp-meet-in-the-middle`: Split-and-search subset sum counter for $N \le 40$.
- **Dynamic Programming (`cp-dp`)**:
  - `cp-lis`: Longest Increasing Subsequence in $\mathcal{O}(n \log n)$ via patience sorting.
  - `cp-lcs`: Longest Common Subsequence with path reconstruction.
  - `cp-edit-distance`: Levenshtein edit distance with insert, delete, and replace costs.
  - `cp-knapsack-01`, `cp-knapsack-unbounded`: Standard 0/1 and unbounded knapsacks.
  - `cp-digit-dp`: Digit DP template for counting numbers satisfying digit conditions in range $[L, R]$.
  - `cp-sos-dp`: Sum Over Subsets (SOS) DP in $\mathcal{O}(n \cdot 2^n)$.
  - `cp-tsp-bitmask`: Travelling Salesperson Problem in $\mathcal{O}(n^2 2^n)$.
- **Strings (`cp-strings`)**:
  - `cp-kmp`: Knuth-Morris-Pratt prefix function ($\pi$) and linear pattern search.
  - `cp-z-algo`: Z-Algorithm computing longest common prefix between $S$ and suffixes.
  - `cp-manacher`: Manacher’s algorithm for linear time odd and even palindrome detection.
  - `cp-rolling-hash`: Collision-resistant polynomial double rolling hash.
  - `cp-suffix-array`: Suffix Array and Kasai LCP construction in $\mathcal{O}(n \log n)$.
  - `cp-aho-corasick`: Multi-pattern dictionary automaton with failure transitions.
- **Searching & Sorting (`cp-searching`, `cp-sorting`)**:
  - `cp-binary-search`: Monotone predicate binary search (`first-true` pattern).
  - `cp-ternary-search`: Extrema search for unimodal functions (integer and real).
  - `cp-inversions`: Inversion counting via Merge Sort.
  - Quick Sort, Heap Sort, Counting Sort, Radix Sort, Bucket Sort.
- **Backtracking (`cp-backtracking`)**:
  - `cp-nqueens`: $N$-Queens solver using bitmask column, diagonal, and anti-diagonal states.
  - `cp-knights-tour`: Warnsdorff's heuristic for knight's tour.
  - `cp-kknights`: Independent knights placement problem on a chessboard grid.
  - `cp-sudoku`: Constraint-propagation and backtracking Sudoku solver.

</details>

<details>
<summary><b>3. Advanced Data Structures (17 files, 120 snippets)</b></summary>

- **Segment Trees & Variations (`cp-segtree`, `cp-lazy-segtree`, `cp-lichao-tree`)**:
  - `cp-segtree`: Iterative Point-Update Range-Query Monoid Segment Tree (Sum, Min, Max, GCD).
  - `cp-lazy-segtree`: AtCoder Library style lazy segment tree with range add, range set, and range affine updates.
  - `cp-lichao-tree`: Li Chao Segment Tree for dynamic line insertion and point evaluation.
- **Persistent Segment Trees (`cp-persistent-segtree`)**:
  - `cp-persistent-segtree`: Path-copying persistent segment tree over value universes.
  - `cp-pst-init`: 3-line compress and build using `Compressor`.
  - `cp-pst-kth`: $k$-th smallest query in subarray $a[l \dots r]$ translated back to value.
  - `cp-pst-leq`: Count elements $\le x$ in subarray $a[l \dots r]$.
  - `cp-lazy-pst-flip`: Binary range flips with rolling hash comparison in $\mathcal{O}(\log n)$.
  - `cp-lazy-pst-sum`: Persistent Lazy Segment Tree with Range Add and Range Sum with version rollbacks.
- **Merge Sort Trees (`cp-merge-sort-tree`)**:
  - Static Merge Sort Tree: Range count $\le x$, $< x$, $\ge x$, $> x$, and range $k$-th smallest in $\mathcal{O}(\log^3 n)$.
  - Point-Update Dynamic Merge Sort Tree: GNU PBDS-backed tree with point updates in $\mathcal{O}(\log^2 n)$.
- **Disjoint Set Union (`cp-dsu`)**:
  - `cp-dsu`: Path compression + union-by-size/rank.
  - `cp-rollback-dsu`: Stack-based undoable DSU for offline dynamic connectivity.
  - `cp-potential-dsu`: Weighted DSU tracking potential differences ($pot[v] - pot[u] = w$).
- **Fenwick Trees / BIT (`cp-fenwick`)**:
  - Standard 1D Fenwick tree with binary lifting for $k$-th smallest prefix sum.
  - Range-Add Range-Sum Fenwick tree via two linear BITs.
  - 2D Fenwick tree for 2D point update and rectangle sum.
- **Ordered Sets & Hash Tables (`cp-ordered-set`, `cp-pbds`, `cp-hash`)**:
  - `cp-ordered-set`: Portable Treap-based order-statistic tree (`ordered_set` and `ordered_multiset`) without PBDS dependency.
  - `cp-pbds`: GNU Policy-Based `tree` (`find_by_order`, `order_of_key`) with safe multiset duplicate handling.
  - `cp-fast-hash-table`: Ultra-fast GNU PBDS `gp_hash_table` with `chash` (3–6$\times$ faster than `std::unordered_map`).
- **Standard & Search Trees (`cp-advanced-trees`, `cp-heap`, `cp-trie`, `cp-sparse-table`)**:
  - BST, AVL Tree, Red-Black Tree, Min/Max Binary Heap.
  - 26-way lowercase Prefix Trie and Binary XOR Trie.
  - Sparse Table for static $\mathcal{O}(1)$ Range Minimum Queries (RMQ), Range Max, Range GCD.
  - Circular Buffer Queue, Circular Array Deque, Linked Lists, Monotonic Stacks.

</details>

<details>
<summary><b>4. Graph Theory & Trees (4 files, 26 snippets)</b></summary>

- **Shortest Paths & Traversals (`cp-graph-paths`)**:
  - `cp-bfs`: Queue-based BFS for unweighted graphs.
  - `cp-dijkstra`: Priority-queue Dijkstra with path reconstruction.
  - `cp-01-bfs`: Double-ended queue BFS for graphs with $0/1$ edge weights.
  - `cp-bellman-ford`, `cp-spfa`: Bellman-Ford and Shortest Path Faster Algorithm with negative cycle detection.
  - `cp-floyd-warshall`: $\mathcal{O}(V^3)$ all-pairs shortest paths.
  - `cp-topological-sort`: Kahn's indegree-based topological sort.
  - `cp-bipartite-check`: 2-coloring bipartite verification.
- **Connectivity & Decompositions (`cp-graph-connectivity`)**:
  - `cp-kruskal`, `cp-prim`, `cp-boruvka`: Minimum Spanning Tree algorithms.
  - `cp-tarjan-scc`, `cp-kosaraju-scc`: Strongly Connected Components solvers.
  - `cp-2sat`: 2-Satisfiability solver with variable truth assignments.
  - `cp-bridges-art`: Bridges and articulation points discovery in undirected graphs.
  - `cp-bcc-edges`, `cp-bcc-vertices`: Biconnected component decompositions.
- **Flows & Matching (`cp-graph-flows`)**:
  - `cp-dinic`: $\mathcal{O}(V^2 E)$ Dinic's Maximum Flow and Minimum Cut.
  - `cp-mcmf`: Min-Cost Max-Flow using SPFA potential augmentation.
  - `cp-hopcroft-karp`: $\mathcal{O}(E \sqrt{V})$ maximum cardinality bipartite matching.
- **Tree Algorithms (`cp-graph-trees`)**:
  - `cp-hld`: Heavy-Light Decomposition with path and subtree query mappings.
  - `cp-centroid-decomp`: Centroid Decomposition for path queries on trees.
  - `cp-lca`: Binary lifting Lowest Common Ancestor with tree distance queries.
  - `cp-euler-tour`: Tree flattening into 1D interval representation.

</details>

<details>
<summary><b>5. Mathematics, Number Theory & Geometry (8 files, 75 snippets)</b></summary>

- **Algebra & Polynomials (`cp-algebra-polynomials`)**:
  - `cp-alg-xor-basis`: Linear XOR basis over $\text{GF}(2)$ (insert, max XOR, $k$-th smallest).
  - `cp-alg-matrix`: Matrix multiplication with cache optimization and binary matrix exponentiation.
  - `cp-alg-solve-linear`: Gaussian elimination over $\mathbb{Z}_p$ for linear systems.
  - `cp-poly-ntt`: Number Theoretic Transform mod $998244353$.
  - `cp-poly-mtt`: Arbitrary modulus convolution (Split FFT) for $M \le 10^9$.
  - `cp-poly-inv`: Formal power series inversion via Newton's method.
  - `cp-poly-bostan-mori`: Bostan-Mori algorithm for $k$-th term of linear recurrence.
  - `cp-poly-lagrange`: Linear time Lagrange polynomial interpolation over consecutive points.
  - `cp-poly-all-pairs-sum`, `cp-poly-cyclic-conv`, `cp-poly-wildcard-match`: Frequency convolutions, circular scalar products, and wildcard string matching.
- **Modular Arithmetic (`cp-modular-arithmetic`)**:
  - `cp-mint`: Modular integer (`mint`) struct with automatic reduction, division, and inverse.
  - `cp-power-mod`: Fast binary modular exponentiation.
  - `cp-crt`: Chinese Remainder Theorem solver.
  - `cp-baby-giant`: Baby-Step Giant-Step discrete logarithm solver.
  - `cp-tonelli-shanks`: Tonelli-Shanks quadratic residue / modular square root solver.
- **Primes & Sieves (`cp-primes-sieves`)**:
  - `cp-linear-sieve`: $\mathcal{O}(n)$ linear sieve computing Smallest Prime Factor (SPF).
  - `cp-miller-rabin`: Deterministic 64-bit Miller-Rabin primality test.
  - `cp-pollard-rho`: $\mathcal{O}(n^{1/4})$ integer factorization algorithm.
  - `cp-segmented-sieve`: Segmented sieve for primes in range $[L, R]$.
- **Möbius & Harmonic Lemma (`cp-mobius-harmonic`)**:
  - Linear sieve computing Euler’s Totient ($\phi$) and Möbius ($\mu$).
  - Harmonic Lemma 1D and 2D quotient chunking in $\mathcal{O}(\sqrt{N})$ time.
- **Combinatorics & Diophantine (`cp-combinatorics`, `cp-diophantine-floorsum`)**:
  - Binomial coefficients, Catalan numbers, combinations table.
  - 2-variable and $N$-variable Linear Diophantine solvers.
  - Frobenius coin problem solver.
  - Floor Sum $\sum_{i=0}^{n-1} \lfloor \frac{ai + b}{m} \rfloor$ in $\mathcal{O}(\log m)$ time.
- **Geometry (`cp-geometry`)**:
  - `cp-point-2d`: 2D point and vector struct with cross product, dot product, and orientation.
  - `cp-convex-hull`: Andrew's Monotone Chain convex hull in $\mathcal{O}(n \log n)$.
  - `cp-polygon-area`: Shoelace formula for polygon area.
  - `cp-closest-pair`: Sweep-line $\mathcal{O}(n \log n)$ closest pair of points.

</details>

---

## Developer Tools & Automation

### 1. Build & Run Helper (`tools/run.sh`)

[`tools/run.sh`](tools/run.sh) automates compilation and execution with GCC sanitizers or optimized release flags.

#### Syntax

```bash
tools/run.sh <source.cpp> [input_file] [mode: debug|fast]
```

#### Modes

| Mode | Flags | Purpose |
| :--- | :--- | :--- |
| `debug` *(default)* | `-std=c++17 -g -O1 -Wall -Wextra -Wno-unused-result -Wshadow -D_GLIBCXX_DEBUG -fsanitize=address,undefined` | Identifies out-of-bounds indexing, dangling pointers, memory corruption, and undefined behavior locally with line numbers. |
| `fast` | `-std=c++17 -O2 -Wall -Wextra -Wno-unused-result` | Emulates online judge execution for accurate runtime and benchmark measurements. |

#### Input Redirection

- Reads from `input_file` if explicitly specified.
- Automatically reads from `input.txt` if present in the current working directory.
- Falls back to interactive standard input (`stdin`).

#### Examples

```bash
# Debug mode with sanitizers (reads from input.txt if present):
tools/run.sh solution.cpp

# Fast mode for time benchmarking:
tools/run.sh solution.cpp input.txt fast
```

---

### 2. Automated Differential Stress Tester (`tools/stress.sh`)

[`tools/stress.sh`](tools/stress.sh) conducts differential testing between an optimized solution and a brute-force solution on randomized testcases.

#### Syntax

```bash
tools/stress.sh <sol.cpp> <brute.cpp> <gen.cpp> [iterations=500]
```

- `sol.cpp`: Optimized candidate solution (compiled with AddressSanitizer and UndefinedBehaviorSanitizer).
- `brute.cpp`: Slow but verified reference brute-force solution.
- `gen.cpp`: Randomized test generator (accepts a random seed as `argv[1]`).
- `iterations`: Number of randomized runs (defaults to `500`).

#### Failure Artifacts

Upon mismatch, crash, or timeout (5s for `sol.cpp`, 20s for `brute.cpp`):
1. Execution halts immediately with a diff preview in the terminal.
2. Minimal reproducible artifacts are saved directly to disk:
   - `failing_input.txt`: Exact input triggering the failure.
   - `failing_sol.txt`: Output produced by the candidate solution.
   - `failing_brute.txt`: Expected output produced by the brute-force solution.

---

## VS Code Integration & Settings

[`.vscode/settings.json`](.vscode/settings.json) is preconfigured for instantaneous snippet expansion:

```json
{
  "editor.snippetSuggestions": "inline",
  "editor.suggest.showSnippets": true,
  "editor.suggestSelection": "first",
  "editor.tabCompletion": "on",
  "editor.acceptSuggestionOnEnter": "smart",
  "editor.suggest.snippetsPreventQuickSuggestions": true,
  "editor.quickSuggestions": {
    "other": "on",
    "comments": "off",
    "strings": "off"
  },
  "[cpp]": {
    "editor.wordSeparators": "`~!@#$%^&*()=+[{]}\\|;:'\",.<>/?"
  }
}
```

---

## Recommended Contest Workflow

1. **Initialize Problem**: Create `sol.cpp`, type `cpplate` or `cp-template`, and hit `Tab`.
2. **Inject Data Structures**: Type snippet prefixes (e.g., `cp-coordinate-compression`, `cp-pst-init`, `cp-dsu`, `cp-lazy-segtree`, `cp-poly-ntt`) and tab through placeholders.
3. **Execute & Debug**:
   ```bash
   tools/run.sh sol.cpp
   ```
4. **Stress Test Edge Cases** (if receiving Wrong Answer on hidden testcases):
   ```bash
   tools/stress.sh sol.cpp brute.cpp gen.cpp 1000
   ```

---

## Prerequisites & Setup

Install GCC, GDB, and core utilities on Linux or WSL:

```bash
sudo apt update
sudo apt install -y build-essential g++ gdb coreutils diffutils
chmod +x tools/*.sh
```
