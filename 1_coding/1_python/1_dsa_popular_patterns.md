# DSA Patterns — Popular Interview Coding Patterns

This document lists the common data-structures & algorithms (DSA) coding patterns frequently asked in technical interviews. For each pattern you'll find: a short description, typical problems/examples, complexity notes, common pitfalls, and quick study tips.

Use this as a checklist when preparing: pick 2–3 patterns per day, implement 3–5 problems per pattern, and review common variations.

---

## 1. Two Pointers

Description: Use two indices (often left/right) to traverse an array/string, typically when array is sorted or when looking at pair relationships.

Typical problems: pair sum in sorted array, container with most water, remove duplicates in-place, 3-sum (with sorting + two pointers)

Complexity: O(n) to O(n log n) if sorting required.

Tip: When you see "sorted" or "pairs/triplets" look for two-pointer. Be careful with duplicate handling and in-place index updates.

```python
# Template: 2-Sum Sorted
def two_sum_sorted(arr, target):
    left, right = 0, len(arr) - 1
    while left < right:
        curr = arr[left] + arr[right]
        if curr == target:
            return [left, right]
        if curr < target:
            left += 1
        else:
            right -= 1
    return []
```

---

## 2. Sliding Window

Description: Maintain a window (contiguous subarray) and expand/contract it to satisfy constraints (max/min sum, distinct characters, etc.).

Typical problems: longest substring without repeating characters, min subarray sum ≥ S, max sum subarray of size K, sliding window maximum.

Complexity: O(n) usually.

Tip: Distinguish between fixed-size and variable-size window problems. Use frequency maps for variable windows.

```python
# Template: Variable Size Window
def sliding_window(s):
    left = 0
    longest = 0
    window = {} # or set()
    
    for right in range(len(s)):
        # Add s[right] to window
        char = s[right]
        window[char] = window.get(char, 0) + 1
        
        # Contract window if invalid
        while window[char] > 1: # Example condition: distinct chars
            left_char = s[left]
            window[left_char] -= 1
            left += 1
            
        longest = max(longest, right - left + 1)
    return longest
```

---

## 3. Fast & Slow Pointers (Floyd’s Tortoise & Hare)

Description: Two pointers moving at different speeds; useful to detect cycles, find middle of linked list, or detect meeting point.

Typical problems: detect cycle in linked list, find start of cycle, find middle node, palindrome linked list (with reverse-second-half trick).

Complexity: O(n), O(1) extra space.

Tip: After detecting a cycle, reset one pointer to head and move both at same speed to find cycle start.

---

## 4. Binary Search (classic + variants)

Description: Search on sorted arrays or search on an answer space (binary search on monotonic function).

Typical problems: find first/last position, square root, split array to minimize largest sum (binary search on answer), peak element.

Complexity: O(log n)

Tip: Master boundary conditions and left/right invariants; practice both index-based and value-based searches.

---

## 5. Binary Search on Answer / Parametric Search

Description: Use binary search over a numeric answer space where feasibility is monotonic.

Typical problems: minimize maximum distance/partition problems, capacity to ship packages within D days.

Tip: Always map the predicate clearly: is(mid) -> feasible? Then set bounds accordingly.

---

## 6. Prefix Sum / Cumulative Sum

Description: Precompute cumulative values to answer range-sum queries or reduce running computation cost.

Typical problems: subarray sum equals K (with hash map), range sum queries, matrix prefix sums.

Complexity: O(n) preprocessing, O(1) queries.

Tip: Combine with hash maps for variable-length subarray sum problems.

---

## 7. Hashing / Frequency Map

Description: Use hash tables (dicts/maps) to count frequencies, check membership, or cache intermediate results.

Typical problems: two-sum (unsorted), group anagrams, subarray sum equals K, majority element.

Complexity: O(n) average.

Tip: Be mindful of collisions (language-level). For complex keys (tuples), ensure they are hashable.

---

## 8. Heap (Priority Queue)

Description: Use heap for top-k problems, streaming median, k-way merge, scheduling problems.

Typical problems: top-k frequent elements, merge k sorted lists, sliding window median, kth largest element.

Complexity: O(log k) per operation.

Tip: Use min-heap for top-k largest (size k) or max-heap for smallest; be careful with push/pop cost.

---

## 9. Monotonic Stack / Stack Patterns

Description: Maintain a stack that is strictly increasing/decreasing to compute next greater/smaller elements, histogram area, or validate parentheses.

Typical problems: largest rectangle in histogram, next greater element, daily temperatures, trap rain water (variation uses two pointers or stack).

Complexity: O(n)

Tip: Draw indices and values; the stack typically stores indices.

---

## 10. Monotonic Queue (Deque)

Description: Maintain a deque with monotonic order to support sliding-window maximum/minimum queries efficiently.

Typical problems: sliding window maximum, min-queue for variable window optimizations.

Complexity: O(n)

Tip: Use language deque; maintain indices and pop from both ends as window slides.

---

## 11. Greedy Algorithms

Description: Make locally optimal choices hoping to reach global optimum. Requires proof or counterexample discussion in interviews.

Typical problems: interval scheduling, jump game, coin change (if canonical coins), activity selection, fractional knapsack.

Tip: Always argue correctness or show counterexamples; many DP problems are greedy-like but need proof.

---

## 12. Dynamic Programming (DP) — Patterns

Common DP patterns:
- 0/1 knapsack and variations — state by capacity and items.
- Unbounded knapsack / coin change — allow repeated items.
- Longest Increasing Subsequence (LIS) — O(n log n) patience sorting trick.
- DP on strings (edit distance, LCS)
- DP on intervals (matrix chain multiplication, burst balloons)
- DP on trees (tree DP)
- DP with bitmasking (TSP-like small N)

Tip: Define state clearly (what your DP array represents), write recurrence, and pick memoization vs bottom-up.

---

## 13. Backtracking / DFS with Pruning

Description: Explore search space with recursion and prune invalid branches early.

Typical problems: permutations, combinations, subset sum, N-Queens, word search.

Complexity: Exponential in general, but pruning often reduces search.

Tip: Sort choices to allow early pruning; use boolean arrays to mark usage to reduce allocations.

---

## 14. BFS / Shortest Path (Unweighted)

Description: Breadth-first traversal for shortest-path in unweighted graphs or level-order traversals.

Typical problems: shortest path in grid, word ladder, bipartite check, minimum steps transformations.

Complexity: O(V + E)

Tip: Use visited set; when tracking parents, you can reconstruct shortest paths.

---

## 15. Dijkstra / A* (Weighted shortest paths)

Description: Use priority queue to find shortest paths in weighted graphs. A* adds heuristic to guide search.

Typical problems: shortest path on weighted grid, network latency minimization.

Complexity: O(E log V)

Tip: Use adjacency lists; be careful with large sparse graphs.

---

## 16. Topological Sort / DAG DP

Description: Linear order of DAG nodes; useful for dependency resolution and DP on DAGs.

Typical problems: course schedule, build order, task scheduling with prerequisites.

Tip: Use Kahn's algorithm (queue) or DFS-based order. Detect cycles by leftover nodes.

---

## 17. Union-Find / Disjoint Set Union (DSU)

Description: Maintain disjoint sets with union and find operations; used for connectivity queries, Kruskal MST, and grouping.

Typical problems: number of connected components, redundant connection, accounts merge.

Complexity: nearly O(1) amortized with union by rank and path compression.

Tip: Add union-by-size/rank and path compression for best performance.

---

## 18. Trie (Prefix Tree)

Description: Tree for storing strings by prefix; supports fast prefix search, autocomplete, and lexicographic operations.

Typical problems: implement trie, word search II, autocomplete, prefix counting.

Complexity: O(length of word) per operation.

Tip: Use arrays or dicts for children depending on alphabet size.

---

## 19. Fenwick Tree (Binary Indexed Tree) / Segment Tree

Description: Data structures for dynamic range queries and updates (sum/min/max).

Typical problems: range sum queries with point updates, range update/range query (with segment tree lazy propagation), count inversions (Fenwick).

Complexity: O(log n) per update/query.

Tip: Master index manipulations for BIT (LSB trick). Segment trees are more general but heavier to implement.

---

## 20. Graph Patterns — Representations & Traversals

Description: Adjacency lists vs matrices, directed vs undirected, weighted vs unweighted.

Typical problems: connectivity, cycles, shortest path, MST, strongly connected components (Kosaraju/Tarjan), bipartite checking.

Tip: Pick representation according to density; use iterative DFS to avoid recursion limits in large graphs.

---

## 21. Bit Manipulation

Description: Use bitwise operators for speed and compactness; common in combinatorics and low-level problems.

Typical problems: single-number (XOR), subsets generation, bit DP, counting set bits, bit tricks (swap, clear lowest set bit).

Tip: Be fluent with XOR properties and bit masks. Use builtins (popcount) where available.

---

## 22. Reservoir Sampling & Randomized Algorithms

Description: Techniques to sample uniformly from a stream when size is unknown or large.

Typical problems: pick random node from linked list, random sampling from stream.

Tip: Understand probability invariants for correctness.

---

## 23. Sorting Patterns & Custom Comparators

Description: Sorting is a tool; recognize when sorting simplifies the problem (two-pointer after sort, grouping, sweep line).

Typical problems: sort intervals by start/end, merge intervals, interval scheduling.

Tip: Consider stable vs unstable sorts and comparator edge cases.

---

## 24. Sweep Line / Events

Description: Convert interval problems to event points (start/end) and sweep across them to maintain active sets.

Typical problems: maximum number of overlapping intervals, area of union of rectangles.

Tip: Sort events carefully and handle ties (start before end or vice versa depending on problem).

---

## 25. Matrix / Grid DFS-BFS Patterns

Description: 2D grid traversal with boundary checks, visited marking, and direction arrays.

Typical problems: number of islands, flood fill, shortest path in binary matrix, island perimeter.

Tip: Use dr/dc arrays for clarity; watch recursion depth for large grids.

---

## 26. Pattern: Transform & Reduce

Description: Transform input (hashing, sorting, mapping) to reduce problem to a known pattern.

Typical problems: convert string problems to frequency arrays, coordinate compression, index mapping for sparse ranges.

Tip: Coordinate compression is handy for reducing large ranges to dense indices.

---

## 27. Mathematical / Number Theory

Description: GCD/LCM, modular arithmetic, prime sieves, modular inverses, chinese remainder theorem.

Typical problems: count divisors, repeating decimals, modular exponentiation, primality tests.

Tip: Learn fast exponentiation, Euclid's algorithm, and sieve of Eratosthenes.

---

## 28. Design Patterns for Coding Interviews

Description: High-level patterns about how to present solutions: clarify, propose multiple approaches, analyze complexity, and choose tradeoffs.

Typical expectations: explain constraints, pick appropriate data structures, discuss correctness and edge cases, incremental improvements.

Tip: Practice communicating assumptions and complexity bounds; write clean, idiomatic code and include tests for edge cases.

---

## Practice plan & resources

- Daily routine: pick a pattern, read its short cheatsheet, solve 3 problems of increasing difficulty, then review optimal solutions.
- Problem sources: LeetCode (Explore & company-tagged problems), HackerRank, Codeforces (for speed), Educative "Grokking the Coding Interview" for pattern-focused learning.
- Books: "Elements of Programming Interviews", "Cracking the Coding Interview" (good for behavioral + pattern mapping), "Competitive Programming" for algorithmic depth.

## Quick checklist when you see a problem

1. Clarify constraints (n, memory, sorted? duplicates?)
2. Identify pattern by keywords (sorted, window, graph, prefix, pair/triplet, top-k)
3. Sketch high-level approach and complexity
4. Edge cases & tests
5. Implement cleanly and iterate to optimize

---

If you'd like, I can:
- Expand each pattern into its own `topics/dsa/<pattern>.md` with 5 curated problems and solution sketches.
- Generate a 30-day practice calendar mapping patterns to specific LeetCode problems and difficulty levels.

Tell me which of the two you'd like next and I'll add files/PRs accordingly.

