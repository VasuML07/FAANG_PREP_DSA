# The Definitive 2D Dynamic Programming Mastery Guide

> **A publication-quality reference for FAANG interviews, competitive programming, and computer science education.**
>
> After completing this guide, a student should be able to **invent** (not memorize) solutions for **90–95% of unseen 2D DP problems** by deriving states, transitions, and iteration orders from first principles.

---

## Table of Contents

1. [Philosophy & How to Use This Guide](#1-philosophy--how-to-use-this-guide)
2. [Learning Roadmap](#2-learning-roadmap)
3. [Dynamic Programming Foundations](#3-dynamic-programming-foundations)
4. [2D State Design Masterclass](#4-2d-state-design-masterclass)
5. [Transition Design Masterclass](#5-transition-design-masterclass)
6. [Dependency Graph Visualization](#6-dependency-graph-visualization)
7. [Iteration Order Masterclass](#7-iteration-order-masterclass)
8. [Memoization to Tabulation](#8-memoization-to-tabulation)
9. [Space Optimization](#9-space-optimization)
10. [Pattern Recognition Guide (60+ Patterns)](#10-pattern-recognition-guide)
11. [2D DP Pattern Atlas](#11-2d-dp-pattern-atlas)
    - 11.1 [Grid DP — Minimum Path Sum](#111-grid-dp--minimum-path-sum)
    - 11.2 [Grid DP — Unique Paths](#112-grid-dp--unique-paths)
    - 11.3 [Grid DP — Unique Paths II (Obstacles)](#113-grid-dp--unique-paths-ii)
    - 11.4 [Grid DP — Dungeon Game](#114-grid-dp--dungeon-game)
    - 11.5 [Grid DP — Cherry Pickup](#115-grid-dp--cherry-pickup)
    - 11.6 [Triangle DP — Triangle Minimum Path](#116-triangle-dp--triangle-minimum-path)
    - 11.7 [Grid DP — Minimum Falling Path Sum](#117-grid-dp--minimum-falling-path-sum)
    - 11.8 [Sequence DP — Longest Common Subsequence](#118-sequence-dp--longest-common-subsequence)
    - 11.9 [Sequence DP — Longest Common Substring](#119-sequence-dp--longest-common-substring)
    - 11.10 [Edit Distance](#1110-edit-distance)
    - 11.11 [Distinct Subsequences](#1111-distinct-subsequences)
    - 11.12 [Shortest Common Supersequence](#1112-shortest-common-supersequence)
    - 11.13 [Longest Palindromic Subsequence](#1113-longest-palindromic-subsequence)
    - 11.14 [Palindrome Partitioning II](#1114-palindrome-partitioning-ii)
    - 11.15 [Matrix Chain Multiplication](#1115-matrix-chain-multiplication)
    - 11.16 [Burst Balloons](#1116-burst-balloons)
    - 11.17 [Boolean Parenthesization](#1117-boolean-parenthesization)
    - 11.18 [Egg Dropping](#1118-egg-dropping)
    - 11.19 [Partition DP — Word Break II Count](#1119-partition-dp--word-break-count)
    - 11.20 [Interleaving String](#1120-interleaving-string)
12. [DP Visualization Lab](#12-dp-visualization-lab)
13. [Master Decision Tree](#13-master-decision-tree)
14. [Complexity Masterclass](#14-complexity-masterclass)
15. [Common Mistakes (50+)](#15-common-mistakes)
16. [Edge Cases (50+)](#16-edge-cases)
17. [Interview Thinking Process](#17-interview-thinking-process)
18. [Pattern Comparison](#18-pattern-comparison)
19. [Problem Progression](#19-problem-progression)
20. [Debugging Guide](#20-debugging-guide)
21. [Pattern Summary Table](#21-pattern-summary-table)
22. [Cheat Sheet](#22-cheat-sheet)
23. [Interview Cheat Codes (100+)](#23-interview-cheat-codes)
24. [FAQ (50+)](#24-faq)
25. [Final Challenge — 30 Unseen Problems](#25-final-challenge)
26. [Final Interview Checklist](#26-final-interview-checklist)

---

## 1. Philosophy & How to Use This Guide

### 1.1 The Goal Is Invention, Not Memorization

Most DP resources fail because they teach **pattern matching**: "If you see a grid, apply grid DP. If you see two strings, apply LCS." Pattern matching breaks down the moment an interviewer changes one word in a problem statement. The interviewer is not testing whether you have seen the problem before; the interviewer is testing whether you can **derive** a solution under pressure.

This guide trains a different reflex. Instead of "what pattern is this?", we train "what is the **state**?" and "what are the **transitions**?". A state is a snapshot of all information needed to make future decisions. A transition is the rule by which one state becomes another. Every DP solution, no matter how famous, is just a state definition plus a transition plus an iteration order. If you can derive those three things from the problem, you do not need the pattern.

The mental model we will use throughout:

```
Problem
   |
   v
What decisions exist?
   |
   v
What information must I remember to make future decisions?
   |
   v
That information IS the state.
   |
   v
How does one state flow into another?
   |
   v
That flow IS the transition.
   |
   v
In what order must states be computed so dependencies are ready?
   |
   v
That order IS the iteration.
```

If you internalize this chain, you can solve problems you have never seen. The patterns become shortcuts, not crutches.

### 1.2 Why 2D DP Specifically

One-dimensional DP is powerful but limited. A 1D state `dp[i]` answers a question about a prefix `s[0..i]` or a count `i`. Many real problems involve **two simultaneously changing quantities**: two indices into two strings, a row and a column, a left and right boundary, an index and a count, an index and a previous choice, a position and a bitmask. These problems require a state with two degrees of freedom, and that is what 2D DP models.

The jump from 1D to 2D is conceptually small (just add another index) but practically huge. The number of states multiplies, the iteration order becomes non-trivial, dependencies can flow in multiple directions, and space optimization becomes a real concern. Most students who freeze on hard DP problems freeze not because the recurrence is hard, but because they cannot decide what the second dimension should represent. This guide exists to fix exactly that.

### 1.3 How to Read This Guide

Read sections 3 through 9 in order — they build the theoretical foundation. Sections 10 and 11 are the pattern atlas; treat them as a reference, not a tutorial. Read one atlas chapter per day and **rederive the recurrence on paper before reading the solution**. Sections 12 through 22 are reference material you will return to. Sections 23 and 24 (cheat codes and FAQ) are best skimmed once, then re-read the night before an interview. Section 25 (final challenge) is your self-test.

Whenever you see a recurrence in this guide, do not memorize it. Instead, ask:
- What does each index mean?
- Why this exact set of choices at this state?
- Why this iteration order and not another?
- Could I drop a dimension? Why or why not?

If you can answer those four questions for every recurrence in this guide, you are interview-ready.

---

## 2. Learning Roadmap

The roadmap below is the order in which concepts should be learned. Each step assumes mastery of the previous one. Skipping steps is the single most common reason students struggle with DP.

```
                +-------------------+
                |   Recursion       |
                |  (the bedrock)    |
                +---------+---------+
                          |
                          v
                +-------------------+
                |   Memoization     |
                |  (caching leaves) |
                +---------+---------+
                          |
                          v
                +-------------------+
                |   1D DP           |
                | (single index)    |
                +---------+---------+
                          |
                          v
                +-------------------+
                |   2D DP           |
                | (two indices)     |
                +---------+---------+
                          |
            +-------------+-------------+
            |             |             |
            v             v             v
        +-------+    +--------+    +---------+
        | Grid  |    |Sequence|    | Interval|
        |  DP   |    |   DP   |    |   DP    |
        +---+---+    +---+----+    +----+----+
            |             |             |
            +------+------+------+------+
                   |             |
                   v             v
              +---------+   +-----------+
              |Partition|   |  State    |
              |   DP    |   |Compress   |
              +----+----+   +-----+-----+
                   |              |
                   +------+-------+
                          |
                          v
                  +---------------+
                  | Advanced DP   |
                  | (Digit/Bitmask|
                  |  /Graph/Tree) |
                  +---------------+
```

**Why this order?**

- **Recursion first** because every DP solution is a recursion with caching. If you cannot write the recursive version, you cannot write the DP version.
- **Memoization next** because it is the bridge: same recursion, just cache results by argument tuple. This teaches you to identify state variables naturally.
- **1D DP** then teaches you to convert memoization to tabulation and reason about iteration order with a single index.
- **2D DP** is the central topic. With two indices, iteration order and dependencies become interesting.
- **Grid / Sequence / Interval / Partition** are the four canonical 2D DP families. Each has its own dependency shape and iteration order conventions.
- **State compression** (bitmask DP) and **advanced DP** (digit, graph, tree) extend the same ideas to higher dimensions or unusual structures.

If you find yourself stuck on a 2D DP problem, the bug is almost always in an earlier layer of this roadmap. Trace backward: is your recursion correct? Is your memoization key complete? Is your 1D reasoning sound? Fix the foundation before fixing the tower.

---

## 3. Dynamic Programming Foundations

### 3.1 What Dynamic Programming Actually Is

Dynamic programming is the technique of solving a problem by:
1. Breaking it into overlapping subproblems,
2. Solving each subproblem exactly once,
3. Storing the result, and
4. Combining stored results to answer larger subproblems.

Two properties must hold for DP to apply:

**Optimal substructure.** The optimal solution to a problem can be built from optimal solutions to its subproblems. If a problem's optimal solution depends on non-optimal subproblem solutions, plain DP will not work (you may need DP with extra state, or a different algorithm).

**Overlapping subproblems.** The same subproblem is reached via multiple paths in the recursion tree. If subproblems do not overlap, you should use divide-and-conquer, not DP. The overlap is what makes caching worthwhile.

For 2D DP, both properties typically manifest as: "the answer for the pair `(i, j)` can be expressed in terms of answers for nearby pairs like `(i-1, j)`, `(i, j-1)`, `(i-1, j-1)`, or `(i+1, j-1)`." The pair `(i, j)` is the subproblem; the nearby pairs are the overlap.

### 3.2 Why One Dimension Is Insufficient

A 1D state `dp[i]` captures information about a single index. It works when the problem's future decisions depend only on a single positional quantity — typically "how far through the input have I processed" or "how many items have I used."

But many problems have **two independent quantities** that both evolve:

- Two pointers moving through two strings (LCS, Edit Distance).
- A row index and a column index moving through a grid (Unique Paths, Minimum Path Sum).
- A left boundary and a right boundary defining a range (Interval DP, palindrome problems).
- An item index and a capacity/count (knapsack, egg drop, transactions).
- A position and a "previous choice" or "color of the last house" (paint house, no-adjacent-equal constraints).

When two quantities evolve independently, a 1D state cannot distinguish configurations that differ only in the second quantity. For example, in edit distance, knowing only that you have processed `i` characters of string A is insufficient — the answer also depends on how many characters of B you have consumed. Two different `(i, j)` configurations with the same `i` can have wildly different answers, so collapsing to `dp[i]` loses information and produces wrong answers.

The diagnostic question is: **"If I freeze one variable and let the other vary, do I get different answers?"** If yes, you need at least two dimensions.

### 3.3 When Multiple Variables Define a State

A state is the **minimal set of variables** such that, given their values, the future does not depend on the past. This is the Markov property applied to DP: the future depends only on the current state, not on the path taken to reach it.

Concretely, when designing a state, ask: "What do I need to know about the past to decide optimally from here?" Everything you need becomes a dimension. Everything you do not need must be excluded — extra dimensions blow up memory and time.

For example, in LCS of two strings A and B:
- I need to know how much of A I have consumed (call it `i`).
- I need to know how much of B I have consumed (call it `j`).
- I do **not** need to know *which* characters I matched, only the length so far. So the LCS length itself is the *value* `dp[i][j]`, not a dimension.
- I do **not** need to know the order in which I consumed characters, because all that matters for the future is "where am I now."

So the state is `(i, j)` and the value is the LCS length. Two dimensions, exactly what is needed.

### 3.4 When Additional Dimensions Become Necessary

You start with two dimensions and add a third only when you find a configuration that the two-dimensional state cannot disambiguate. Examples:

- **Stock problems with a transaction limit.** `dp[i]` cannot distinguish "0 transactions used" from "1 transaction used" from "2 used." You add a dimension: `dp[i][k]` = best profit using at most `k` transactions up to day `i`.
- **Stock problems with a holding flag.** `dp[i]` cannot distinguish "holding a share" from "not holding." You add a boolean dimension: `dp[i][hold]`.
- **Egg drop.** `dp[k][n]` = minimum trials with `k` eggs and `n` floors. Both `k` and `n` must be dimensions; reducing either loses information.
- **Painting houses with no-two-adjacent-same.** `dp[i]` cannot distinguish which color house `i-1` was painted, which constrains house `i`. You add the previous color: `dp[i][c]`.
- **Traveling salesman.** `dp[i][mask]` = best path ending at city `i` having visited exactly the cities in `mask`. Both `i` and the set of visited cities matter.

The diagnostic question for adding a dimension: **"Does my transition depend on a piece of information that my current state does not capture?"** If yes, that information must become a dimension (or be encoded into the value).

### 3.5 When NOT to Use 2D DP

2D DP is not always the right tool. Use it cautiously when:

- **The problem has greedy structure.** If a locally optimal choice leads to a globally optimal solution, DP is overkill. Examples: activity selection, Huffman coding, fractional knapsack, scheduling with earliest-deadline-first.
- **The problem is a shortest path on a small graph.** Dijkstra or BFS is simpler and faster than modeling it as DP.
- **The problem has no overlapping subproblems.** Pure divide-and-conquer (merge sort, FFT) does not benefit from caching.
- **The state space explodes.** If your 2D state has 10^4 × 10^4 = 10^8 cells and you cannot compress, DP will TLE/MLE. Look for a greedy, math, or different algorithmic insight.
- **The problem is really 1D.** Many problems *look* 2D but the second dimension is determined by the first (e.g., prefix sums with a fixed window). Adding a phantom second dimension wastes memory.
- **The problem admits a closed-form answer.** Combinatorics, number theory, or matrix exponentiation may give O(log n) answers where DP gives O(n).

A useful sanity check: before committing to 2D DP, try to write a brute-force recursion. If the recursion has only one varying parameter, you need 1D DP. If it has two, you need 2D DP. If it has three or more, ask whether one of them can be eliminated by a smarter state definition.

### 3.6 `dp[i]` vs `dp[i][j]`

| Aspect | `dp[i]` (1D) | `dp[i][j]` (2D) |
|---|---|---|
| State count | O(n) | O(n × m) |
| Memory | O(n) | O(n × m), often compressible to O(m) |
| Iteration order | Single loop | Nested loops, order matters |
| Dependency shape | Linear chain | Grid / DAG with multiple incoming edges |
| Typical use | Single sequence, single counter | Two sequences, grid, interval, index+counter |
| Conversion to memo | `f(i)` | `f(i, j)` |
| Space optimization | Trivial (rolling var) | Rolling row, sometimes in-place |
| Failure mode | Cannot capture second evolving quantity | Forgets needed information if dimensions wrong |

The transition from 1D to 2D is not just "add another loop." It changes the **dependency graph** from a chain to a planar DAG, which is why iteration order and space optimization become non-trivial.

### 3.7 Visual Dependency Diagrams

A 1D DP table is a row of cells, each depending on one or two previous cells:

```
1D dependency (Fibonacci-like):
+-----+-----+-----+-----+-----+-----+-----+
|dp[0]|dp[1]|dp[2]|dp[3]|dp[4]|dp[5]|dp[6]|
+-----+-----+-----+-----+-----+-----+-----+
  |--->|     |     |     |     |     |
        |--->|     |     |     |     |
              |--->|     |     |     |
                    |--->|     |     |
                          |--->|     |
                                |--->|
```

A 2D DP table is a grid where each cell depends on cells above, left, or diagonal:

```
2D dependency (LCS-like):
      j=0   j=1   j=2   j=3   j=4   j=5
i=0  [base][base][base][base][base][base]
i=1  [base] [<---][<---][<---][<---][<---]
i=2  [base] [^   ][^\\  ][^\\  ][^\\  ][^\\  ]
i=3  [base] [^   ][^\\  ][^\\  ][^\\  ][^\\  ]
i=4  [base] [^   ][^\\  ][^\\  ][^\\  ][^\\  ]

Each cell (i,j) reads from:
  (i-1, j)   -- above
  (i,   j-1) -- left
  (i-1, j-1) -- diagonal (only when chars match, in LCS)
```

The shape of the dependency graph dictates the iteration order. In the 1D case you iterate left to right. In the 2D case you typically iterate row by row, left to right within each row, because that order guarantees all dependencies are computed before they are needed. We will explore exceptions (diagonal, gap-based, reverse) in the Iteration Order Masterclass.

### 3.8 The Three Questions of DP Design

Before writing any DP code, answer these three questions on paper:

**Q1. What is the state?** A tuple of variables that fully describes the situation. For 2D DP, this is usually `(i, j)` plus possibly a small flag or counter.

**Q2. What is the transition?** A formula expressing `dp[i][j]` in terms of other states. Each term in the formula corresponds to a *choice* you can make at this state.

**Q3. What is the base case?** The values of `dp` at states that have no dependencies — typically `i=0` or `j=0` rows/columns, or `i==j` diagonal cells.

If you cannot crisply answer all three, you are not ready to code. Most buggy DP solutions trace back to a fuzzy answer on one of these three questions.

---


## 4. 2D State Design Masterclass

This section teaches you how to **invent** multidimensional states from scratch. We will walk through the canonical state shapes, explain what each dimension represents, why it is required, why it cannot be removed, and whether it can be compressed. We will also examine incorrect states and explain why they fail.

### 4.1 The State Design Procedure

When you encounter a problem, run this procedure:

**Step 1. Write the brute-force recursion.** Do not optimize yet. Just write `solve(...)` recursively, with as many arguments as needed. The arguments are your candidate state variables.

**Step 2. Identify which arguments vary.** Static inputs (the array, the strings) are not state. Only arguments that change between recursive calls become dimensions.

**Step 3. Check for redundancy.** If two arguments always satisfy a relation (e.g., `right = left + 2*depth - 1`), one of them is redundant. Eliminate it.

**Step 4. Check for missing information.** If your recursion branches based on a piece of information not in the arguments, you must add it as a dimension. For example, if you branch on "am I holding a stock," you must add a holding flag.

**Step 5. Decide value vs. state.** The quantity you want to optimize (length, count, cost) is the **value** `dp[state]`. The state is what indexes the table. Mixing these up is a common beginner error.

**Step 6. Sanity check.** For two different states, can the value differ? For two identical states, must the value be the same? If yes to both, your state is well-formed.

### 4.2 `dp[row][col]` — Grid Position

**What it represents.** "I am standing at row `row`, column `col` of a grid. What is the best value achievable starting from here (or ending here, depending on direction)?"

**Why it is required.** The position in the grid is exactly what determines future moves. You cannot collapse row and column into a single index because the grid is 2D — there is no natural linear ordering that preserves locality.

**Why it cannot be removed.** A 1D state cannot distinguish "row 3, col 5" from "row 5, col 3." They are different positions with different neighbors, so they need separate cells.

**Whether it can be compressed.** Often yes — if transitions only depend on the previous row, you can compress `dp[row][col]` to `dp[col]` (rolling row). We will see this in detail in the Space Optimization section.

**Examples.** Unique Paths, Minimum Path Sum, Dungeon Game, Cherry Pickup, Minimum Falling Path Sum.

**Incorrect state.** `dp[i]` where `i = row * cols + col` (a flat index). This is technically 1D but it doesn't help — you still need O(rows × cols) cells, you lose the locality that makes rolling-row compression possible, and the code is harder to read. Don't do this unless the problem genuinely is 1D.

### 4.3 `dp[i][j]` — Two Indices into Two Sequences

**What it represents.** "I have consumed `i` characters of sequence A and `j` characters of sequence B. What is the answer for the prefixes A[0..i-1] and B[0..j-1]?"

**Why it is required.** Two independent indices because two sequences are being processed. The answer depends on both prefixes simultaneously.

**Why it cannot be removed.** The two indices evolve independently. Sometimes you advance only in A, sometimes only in B, sometimes in both. A single index cannot capture "3 of A consumed, 5 of B consumed" and "5 of A consumed, 3 of B consumed" as the same state — they are different.

**Whether it can be compressed.** Yes, if transitions only look at row `i-1` and row `i`, you can compress to a single row of size `len(B)+1`. This is the standard LCS/Edit Distance optimization.

**Examples.** Longest Common Subsequence, Edit Distance, Distinct Subsequences, Shortest Common Supersequence, Interleaving String.

**Incorrect state.** `dp[i+j]` where `i+j` is the total characters consumed. This loses information: many `(i, j)` pairs sum to the same `i+j` but have different answers. Edit distance between "abc"/"xyz" (i=3,j=3) and "abcxyz"/"" (i=6, j=0) both have i+j=6 but completely different answers.

### 4.4 `dp[index][sum]` — Index and Running Sum

**What it represents.** "I am considering item at `index`. So far the running sum (or count, or weight) is `sum`. What is the best achievable from here?"

**Why it is required.** Many problems have a constraint on the sum: knapsack weight limit, subset sum target, count of subsets summing to k, number of ways to make change. Both "which items have I considered" and "what is the running total" must be tracked.

**Why it cannot be removed.** The running sum constrains future choices (you cannot exceed the budget), and the index determines what items remain. Either alone is insufficient.

**Whether it can be compressed.** Yes — if transitions only depend on `index-1`, you can compress to a 1D array indexed by `sum`. Important: when compressing, you must iterate `sum` in the **reverse** direction to avoid using the same item twice. This is the classic 0/1 knapsack optimization.

**Examples.** 0/1 Knapsack, Subset Sum, Target Sum, Coin Change II, Partition Equal Subset Sum.

**Incorrect state.** `dp[index]` where the value is "list of all achievable sums." This works in principle but blows up exponentially in the worst case and is not really DP. The sum must be a dimension, not encoded in the value.

### 4.5 `dp[left][right]` — Interval Boundaries

**What it represents.** "I am solving the problem on the subarray/range `[left, right]`. What is the optimal value for this range?"

**Why it is required.** Interval problems recurse by splitting or shrinking a range. The two endpoints `left` and `right` define the range, and both change as you recurse.

**Why it cannot be removed.** A single endpoint cannot describe an arbitrary sub-interval. The pair `(left, right)` is the minimal description.

**Whether it can be compressed.** Usually no — interval DP needs both endpoints visible simultaneously because transitions often split the interval at a point `k` between `left` and `right`. However, if you only need the final answer for the full range, you can sometimes avoid storing all `(left, right)` pairs by processing in gap-increasing order, but the memory is still O(n^2) in the worst case.

**Examples.** Matrix Chain Multiplication, Burst Balloons, Boolean Parenthesization, Longest Palindromic Subsequence, Palindrome Partitioning II, Optimal BST.

**Iteration order.** Intervals must be processed by **increasing length** (gap), so that all smaller sub-intervals needed by a larger interval are already computed. This is the "gap technique" — see Section 7.

**Incorrect state.** `dp[left][length]` where length = right - left + 1. This is mathematically equivalent to `dp[left][right]` but adds an indirection. Use `right` directly for clarity.

### 4.6 `dp[i][transactions]` — Index and Resource Count

**What it represents.** "I am at time/index `i`. I have used exactly `transactions` units of some bounded resource (transactions, eggs, jumps, fuel). What is the best value?"

**Why it is required.** When a resource is bounded and consumed by choices, both the time and the resource count must be tracked. Future choices depend on both.

**Why it cannot be removed.** A 1D state cannot distinguish "day 5 with 0 transactions used" from "day 5 with 2 transactions used." The optimal future action depends on how many transactions remain.

**Whether it can be compressed.** Yes — typically the outer loop over `i` can be replaced by a rolling update, leaving `dp[transactions]`. The compression direction depends on whether the resource is consumed (reverse iteration) or produced (forward iteration).

**Examples.** Best Time to Buy and Sell Stock IV (`dp[i][k]`), Egg Dropping (`dp[k][n]`), Jump Game variants, restricted knapsack.

**Incorrect state.** `dp[i]` plus a separate greedy that picks the best time to use a transaction. Greedy fails because the optimal transaction boundaries are interdependent; only DP can capture all combinations.

### 4.7 `dp[i][previous]` — Index and Previous Choice

**What it represents.** "I am deciding for position `i`. The previous position's choice was `previous`. What is the best value from here?"

**Why it is required.** When the choice at position `i` is constrained by the choice at position `i-1` (no two adjacent same color, no two adjacent selected, etc.), you must remember the previous choice.

**Why it cannot be removed.** Without remembering the previous choice, you cannot enforce the adjacency constraint.

**Whether it can be compressed.** Yes — if `previous` takes values in a small set (e.g., 3 colors, or 2 binary flags), the second dimension is tiny and the whole table is essentially 1D in `i` with a small constant factor.

**Examples.** Paint House (`dp[i][color]`), House Robber II variants, Decode Ways II with restricted digits, Domino Tilings.

**Incorrect state.** `dp[i]` storing the best choice at position `i` and assuming you can backtrack. This loses information about what was chosen at `i-1` when computing `i`, leading to infeasible combinations.

### 4.8 `dp[position][mask]` — Position and Subset (Bitmask)

**What it represents.** "I am at `position`. I have used exactly the items/elements whose bits are set in `mask`. What is the best value?"

**Why it is required.** When the order of selections matters and each item can be used at most once, you must remember exactly which items have been used. A single count is insufficient because different subsets of the same size can lead to different futures.

**Why it cannot be removed.** The mask is the only way to encode "which items used" compactly. Removing it forces you to enumerate subsets explicitly, which is exponential.

**Whether it can be compressed.** Sometimes — if the position is determined by the mask (e.g., position = number of set bits in mask), you can drop the position dimension. This is common in TSP-like problems.

**Examples.** Traveling Salesman, Assignment Problem, Smallest Sufficient Team, Partition to K Equal Sum Subsets (with bitmask), Number of Ways to Wear Different Hats.

**Incorrect state.** `dp[position][count]` where count = number of items used. This loses the identity of items, which matters when items have different costs/values.

### 4.9 A Catalog of Incorrect States

**Incorrect 1: Missing dimension.** Solving LCS with `dp[i]`. Symptom: wrong answer because two configurations with same `i` but different `j` collapse to one cell.

**Incorrect 2: Phantom dimension.** Solving minimum path sum with `dp[row][col][steps]` where `steps = row + col`. The `steps` is fully determined by `(row, col)`, so it is redundant. Symptom: 4× memory, no benefit, harder to code.

**Incorrect 3: Value as dimension.** Solving subset sum with `dp[i][j][achieved_sum]` where the value is supposed to be `achieved_sum`. You have folded the answer into the state, leaving nothing to compute. Symptom: every cell has the same value.

**Incorrect 4: Wrong direction.** Solving "minimum path from top-left to bottom-right" with `dp[i][j]` representing "best path from bottom-right to (i,j)". Not incorrect per se, but the iteration direction and base cases flip, and bugs creep in.

**Incorrect 5: Inconsistent dimension meaning.** Using `dp[i][j]` where sometimes `i` means "first i of A" and sometimes "first i of B." Symptom: confusion, off-by-one errors.

**Incorrect 6: Off-by-one in base case.** Initializing `dp[0][0]` to the wrong value because you forgot whether index 0 means "no characters consumed" or "first character consumed."

**Incorrect 7: Including static input as a dimension.** Adding a dimension for "which string" when there are only two strings. Wastes memory; just use two separate tables.

### 4.10 State Design Checklist

Before finalizing your state, verify:

- [ ] Every dimension corresponds to a quantity that genuinely varies between recursive calls.
- [ ] No two dimensions are functionally dependent on each other.
- [ ] The value is what you want to compute, not a piece of the state.
- [ ] The state captures every piece of information needed by the transitions.
- [ ] The state is minimal — no dimension can be removed without losing information.
- [ ] The base cases are unambiguous for the chosen state.
- [ ] The state count is acceptable: `product of dimension sizes` should fit in time and memory budgets.

If you tick all seven boxes, your state is well-formed. Now you can move on to transitions.

---

## 5. Transition Design Masterclass

A transition is the rule by which one state's value is computed from other states' values. Designing transitions is the heart of DP. This section teaches the systematic procedure.

### 5.1 The Transition Design Procedure

```
Current State (i, j)
        |
        v
What choices do I have at this state?
        |
        +---> Choice 1 ---> leads to state (i', j') with cost/gain c1
        +---> Choice 2 ---> leads to state (i', j') with cost/gain c2
        +---> Choice 3 ---> leads to state (i', j') with cost/gain c3
        |
        v
For each choice, combine:
    value of next state  +  cost/gain of this choice
        |
        v
Combine all choices:
    min (for minimization) or max (for maximization) or sum (for counting)
        |
        v
Store the combined result in dp[i][j]
```

### 5.2 Three Questions to Derive Any Transition

**Q1. At state `(i, j)`, what are all the choices I can make?** Enumerate them exhaustively. Missing a choice is the most common transition bug.

**Q2. For each choice, what is the resulting next state, and what is the immediate cost/gain?** The next state is found by applying the choice to the current state. The immediate cost/gain is whatever the choice adds or subtracts.

**Q3. How do I combine the results of all choices?**
- For optimization (min/max): take the min or max.
- For counting (number of ways): sum, often modulo a prime.
- For feasibility (yes/no): logical OR (any choice works → true).

### 5.3 Worked Example — Minimum Path Sum

State: `dp[i][j]` = minimum path sum from `(0,0)` to `(i,j)`.

**Q1. Choices at `(i,j)`?** You arrived at `(i,j)` from either `(i-1, j)` (came from above) or `(i, j-1)` (came from the left). Two choices.

**Q2. Next states and costs?**
- From `(i-1, j)`: previous state is `(i-1, j)`, immediate cost is `grid[i][j]`.
- From `(i, j-1)`: previous state is `(i, j-1)`, immediate cost is `grid[i][j]`.

**Q3. Combine?** Minimize: `dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1])`.

Recurrence derived. No memorization needed.

### 5.4 Worked Example — Longest Common Subsequence

State: `dp[i][j]` = LCS length of `A[0..i-1]` and `B[0..j-1]`.

**Q1. Choices at `(i,j)`?** Compare `A[i-1]` and `B[j-1]`:
- If they match: you can extend the LCS by this character.
- If they don't match: you cannot extend; you must drop one character from either A or B.

**Q2. Next states and costs?**
- Match case: next state is `(i-1, j-1)`, gain is +1.
- Mismatch case, drop from A: next state is `(i-1, j)`, gain is 0.
- Mismatch case, drop from B: next state is `(i, j-1)`, gain is 0.

**Q3. Combine?**
- If match: `dp[i][j] = 1 + dp[i-1][j-1]`.
- If mismatch: `dp[i][j] = max(dp[i-1][j], dp[i][j-1])`.

Recurrence derived.

### 5.5 Worked Example — Edit Distance

State: `dp[i][j]` = edit distance between `A[0..i-1]` and `B[0..j-1]`.

**Q1. Choices at `(i,j)`?**
- If `A[i-1] == B[j-1]`: no edit needed, advance both.
- If `A[i-1] != B[j-1]`: one of insert, delete, replace.

**Q2. Next states and costs?**
- Match: next state `(i-1, j-1)`, cost 0.
- Insert (into A, conceptually): next state `(i, j-1)`, cost 1.
- Delete (from A): next state `(i-1, j)`, cost 1.
- Replace: next state `(i-1, j-1)`, cost 1.

**Q3. Combine?**
- If match: `dp[i][j] = dp[i-1][j-1]`.
- If mismatch: `dp[i][j] = 1 + min(dp[i-1][j-1], dp[i-1][j], dp[i][j-1])`.

Recurrence derived.

### 5.6 Worked Example — Burst Balloons (Reverse Thinking)

This is a famous problem where the transition is non-obvious. The trick is to think in reverse: instead of "which balloon to burst first," think "which balloon to burst last in this range."

State: `dp[left][right]` = max coins from bursting all balloons in `(left, right)` (open interval, boundaries preserved).

**Q1. Choices at `(left, right)`?** Pick a balloon `k` in `(left, right)` to be the **last** one burst in this range.

**Q2. Next states and cost?**
- If `k` is last, then when `k` bursts, its neighbors are `left` and `right` (everything else in the range is already gone).
- Immediate gain: `nums[left] * nums[k] * nums[right]`.
- Subproblems: `dp[left][k]` (burst everything in `(left, k)`) and `dp[k][right]` (burst everything in `(k, right)`).

**Q3. Combine?**
- `dp[left][right] = max over k in (left, right) of (dp[left][k] + dp[k][right] + nums[left]*nums[k]*nums[right])`.

Recurrence derived. The key insight is the **reverse ordering** — without it, the subproblems are not independent.

### 5.7 Common Transition Patterns

| Pattern | Transition Shape | Example |
|---|---|---|
| **Take/skip** | `dp[i] = best(dp[i-1] + skip_gain, dp[i-1] + take_gain)` | House Robber |
| **Two-source min/max** | `dp[i][j] = cost + min(dp[i-1][j], dp[i][j-1])` | Minimum Path Sum |
| **Three-source min/max** | `dp[i][j] = 1 + min(dp[i-1][j-1], dp[i-1][j], dp[i][j-1])` | Edit Distance |
| **Match/mismatch** | if match: `dp[i-1][j-1] + 1`; else: `max(dp[i-1][j], dp[i][j-1])` | LCS |
| **Split at k** | `dp[l][r] = min/max over k of (dp[l][k] + dp[k+1][r] + cost)` | Matrix Chain |
| **Last-element trick** | `dp[l][r] = max over k in (l,r) of (dp[l][k] + dp[k][r] + gain)` | Burst Balloons |
| **Subset enumeration** | `dp[mask] = best over submasks of (dp[submask] + cost)` | TSP, Partition |
| **Resource consumption** | `dp[i][k] = best(dp[i-1][k], dp[i-1][k-1] + gain)` | Stock IV |

### 5.8 Anti-Patterns in Transition Design

**Anti-pattern 1: Hidden choice.** Forgetting that "do nothing" is also a choice. In House Robber, you can either rob house `i` or skip it. Forgetting the skip branch gives wrong answers.

**Anti-pattern 2: Double counting.** In counting problems, two choices leading to the same state both get counted, inflating the total. Verify choices are mutually exclusive.

**Anti-pattern 3: Wrong direction of recurrence.** Writing `dp[i][j]` in terms of `dp[i+1][j+1]` when the iteration goes forward. Either change the recurrence or change the iteration.

**Anti-pattern 4: Forgetting the cost.** Adding `dp[i-1][j]` without adding the immediate cost `grid[i][j]`. The recurrence looks right but answers are systematically off.

**Anti-pattern 5: Using a stale subproblem.** Referencing `dp[i-2][j]` when only `dp[i-1][*]` is the table currently being filled. Make sure all referenced cells are already computed.

---

## 6. Dependency Graph Visualization

Every 2D DP recurrence defines a **dependency graph** over states. An edge `u → v` means "computing `v` requires `u`." This graph must be a DAG (no cycles), and the iteration order must be a topological sort of this DAG. Understanding the shape of the dependency graph tells you the iteration order, the space optimization opportunities, and the boundary conditions.

### 6.1 The Four Canonical Dependency Shapes

**Shape A: Forward-down (LCS / Edit Distance).** Each cell depends on the cell above, the cell to the left, and possibly the diagonal.

```
+---+---+---+---+
| . | . | . | . |
+---+---+---+---+
| . | X |   |   |    X depends on:
+---+---+---+---+      - cell above (same column, previous row)
| . |   |   |   |      - cell left (same row, previous column)
+---+---+---+---+      - cell diagonal (previous row, previous column)
| . |   |   |   |
+---+---+---+---+
```

Iteration: top to bottom, left to right within each row.

**Shape B: Falling (Minimum Falling Path Sum).** Each cell depends on three cells in the row above.

```
+---+---+---+---+
| . | . | . | . |
+---+---+---+---+
|   | X |   |   |    X depends on:
+---+---+---+---+      - above-left
|   |   |   |   |      - above
+---+---+---+---+      - above-right
|   |   |   |   |
+---+---+---+---+
```

Iteration: top to bottom, left to right. Compresses to a single row.

**Shape C: Diagonal-gap (Interval DP).** `dp[l][r]` depends on smaller intervals `(l, k)` and `(k, r)` for various `k`.

```
Length 0:  dp[i][i]      (single element)
Length 1:  dp[i][i+1]    depends on dp[i][i] and dp[i+1][i+1]
Length 2:  dp[i][i+2]    depends on dp[i][i+1], dp[i+1][i+2], dp[i][i], ...
...

Process by increasing length so dependencies are ready.
```

Iteration: by gap length, outer loop on length, inner loop on left.

**Shape D: Reverse-down (Dungeon Game).** `dp[i][j]` represents "minimum HP needed at (i,j) to reach the end." Dependencies flow backward.

```
+---+---+---+---+
| X |   |   |   |    X depends on:
+---+---+---+---+      - cell to the right (next step)
|   |   |   |   |      - cell below (next step)
+---+---+---+---+
|   |   |   |   |
+---+---+---+---+
|   |   |   |END|
+---+---+---+---+
```

Iteration: bottom to top, right to left. Base case at the end.

### 6.2 Drawing the Dependency Graph

When designing a DP, draw the dependency graph on paper before coding. Steps:

1. Draw an empty grid.
2. Mark the base cells (where the answer is known without recurrence).
3. Pick a generic interior cell `(i, j)`.
4. List all cells the recurrence references.
5. Draw arrows from referenced cells to `(i, j)`.
6. Verify arrows all point "backward" in some consistent direction (no cycles).
7. The iteration order is the topological order of this DAG.

### 6.3 Why Cycles Are Catastrophic

If the dependency graph has a cycle, then `dp[a]` depends on `dp[b]` depends on `dp[a]`, and you cannot compute either first. This means:
- The DP is **wrong** — your state design or transition has an error.
- The fix is to redefine the state so dependencies become acyclic.

Example of accidental cycle: in interval DP, writing `dp[l][r] = dp[l+1][r-1] + ...` and also `dp[l+1][r-1] = ... dp[l][r]`. The second equation is the bug — interval DP subproblems must always shrink the interval.

### 6.4 Dependency Graph as a Sanity Check

Before coding, trace the dependency graph for one specific example. Are all referenced cells "earlier" in your iteration order? If not, your iteration order is wrong. This 30-second check catches 80% of iteration order bugs.

---

## 7. Iteration Order Masterclass

Iteration order is the order in which you fill the DP table. The rule is simple: **a cell must be filled after all cells it depends on.** But applying this rule to 2D DP requires care, because there are several valid and several invalid orderings, and the choice interacts with space optimization.

### 7.1 The Universal Rule

> Iterate in any order that is a topological sort of the dependency graph.

For most 2D DPs, the dependency graph is a planar DAG with edges flowing "up-and-left" (or some rotation). The natural topological sorts are:
- Row-major: outer loop on rows (top to bottom), inner loop on columns (left to right).
- Column-major: outer loop on columns (left to right), inner loop on rows (top to bottom).

Both are valid; row-major is conventional.

### 7.2 Valid Iteration Orders for the Four Shapes

**Shape A (Forward-down, e.g., LCS):**
- Valid: row-major top-to-bottom, left-to-right within row.
- Valid: column-major left-to-right, top-to-bottom within column.
- Invalid: bottom-to-top (dependencies come from above).
- Invalid: right-to-left (dependencies come from the left).

**Shape B (Falling, e.g., Minimum Falling Path Sum):**
- Valid: row-major top-to-bottom.
- Invalid: bottom-to-top.

**Shape C (Interval, e.g., Matrix Chain, Burst Balloons):**
- Valid: outer loop on gap length (1 to n-1), inner loop on left index.
- Valid (alternative): outer loop on left index decreasing, inner loop on right index increasing — produces the same order.
- Invalid: row-major without regard to gap. If you process `(0, 5)` before `(0, 2)`, you reference uninitialized cells.

**Shape D (Reverse-down, e.g., Dungeon Game):**
- Valid: row-major bottom-to-top, right-to-left within row.
- Invalid: top-to-bottom (dependencies come from below/right).

### 7.3 The Gap Technique (Interval DP)

Interval DP requires the gap technique. The pseudocode:

```
for length = 1 to n:                  // length of interval
    for left = 0 to n - length:        // left endpoint
        right = left + length - 1       // right endpoint
        if length == 1:
            dp[left][right] = base case
        else:
            for k = left to right - 1:  // split point
                dp[left][right] = combine(dp[left][k], dp[k+1][right], cost)
```

**Why gap-outer?** Because `dp[left][right]` of length `L` depends on intervals of length `< L`. So all intervals of length `< L` must be computed before any interval of length `L`.

**Common mistake:** iterating `for left ... for right ...` without ensuring length is increasing. This processes `(0, 5)` before `(0, 2)`, referencing uninitialized cells.

### 7.4 Diagonal Traversal (Variant of Gap)

For palindromic substring problems, the iteration is over diagonals:

```
for diff = 0 to n-1:           // difference j - i
    for i = 0 to n-1-diff:
        j = i + diff
        if diff == 0:
            dp[i][j] = true (single char is palindrome)
        elif diff == 1:
            dp[i][j] = (s[i] == s[j])
        else:
            dp[i][j] = (s[i] == s[j]) and dp[i+1][j-1]
```

This is mathematically the same as the gap technique but expressed via `j - i`.

### 7.5 Reverse Traversal (for Space Optimization)

When compressing 2D DP to 1D, the inner loop's direction matters:
- **Forward iteration** means each cell uses the *updated* value of the previous cell in the same pass — this models **unbounded** knapsack (item can be reused).
- **Reverse iteration** means each cell uses the *original* value from the previous pass — this models **0/1** knapsack (item can be used at most once).

```
0/1 knapsack (each item once):
for i = 1 to n:
    for w = W down to weight[i]:        // REVERSE
        dp[w] = max(dp[w], dp[w - weight[i]] + value[i])

Unbounded knapsack (each item unlimited):
for i = 1 to n:
    for w = weight[i] to W:              // FORWARD
        dp[w] = max(dp[w], dp[w - weight[i]] + value[i])
```

The direction flips the meaning of the recurrence. This is one of the most subtle and most-tested concepts in DP interviews.

### 7.6 Two-Pass Iteration

Some problems require two passes:
- **Left-to-right** to compute prefix information.
- **Right-to-left** to compute suffix information.
- Combine at each index.

Example: Trapping Rain Water uses two passes (max-so-far from each side). Candy (LeetCode 135) uses two passes to satisfy both "left neighbor constraint" and "right neighbor constraint."

### 7.7 Invalid Iteration Orders and Why They Fail

**Invalid 1: Bottom-to-top for forward-down DP.** Symptom: cells reference uninitialized `dp[i-1][j]` (the row above, which hasn't been computed). Result: garbage values, often `IndexOutOfBounds` or zeros.

**Invalid 2: Processing intervals in arbitrary order.** Symptom: `dp[0][n-1]` references `dp[0][k]` for various `k`, but those subproblems haven't been computed. Result: zero or garbage.

**Invalid 3: Forward iteration in 0/1 knapsack after compression.** Symptom: the same item is used multiple times because the updated `dp[w - weight[i]]` already includes item `i`. Result: overcounting, inflated values.

**Invalid 4: Reverse iteration in unbounded knapsack.** Symptom: each item only counted once, undercounting.

**Invalid 5: Right-to-left in a left-dependency DP.** Symptom: cell `(i, j)` depends on `(i, j-1)`, but `(i, j-1)` hasn't been filled yet. Result: wrong values.

### 7.8 A Diagnostic Test for Iteration Order

Before coding, write down the recurrence and circle every referenced cell on a small grid. Then pick the iteration order and walk through it mentally: "When I am about to fill cell `(i, j)`, are all circled cells already filled?" If yes for every cell, the order is valid. If no for any cell, the order is wrong.

### 7.9 Summary of Iteration Orders

| Problem Family | Outer Loop | Inner Loop | Notes |
|---|---|---|---|
| Grid DP (forward) | rows top→bottom | cols left→right | Standard |
| Grid DP (reverse, e.g., Dungeon) | rows bottom→top | cols right→left | Reverse direction |
| Sequence DP (LCS, Edit) | i = 1..n | j = 1..m | Row-major |
| Interval DP | length = 1..n | left = 0..n-length | Gap technique |
| Partition DP | length = 1..n | left = 0..n-length, then k | Like interval |
| 0/1 Knapsack (compressed) | i = 1..n | w = W down to weight[i] | Reverse inner |
| Unbounded Knapsack (compressed) | i = 1..n | w = weight[i] to W | Forward inner |

---


## 8. Memoization to Tabulation

The bridge between recursive thinking and iterative DP is systematic conversion. This section teaches the conversion procedure step by step.

### 8.1 The Conversion Procedure

```
Recursive function f(args)
        |
        v
Identify which args vary  -->  these become state dimensions
        |
        v
dp[i][j] = ?               -->  DP table of size product(dimensions)
        |
        v
Base cases of recursion    -->  Initial values of table
        |
        v
Recursive calls reference  -->  Iteration order must compute these
        |                       before they are referenced
        v
return statement           -->  Final cell to return
```

### 8.2 Step-by-Step: Edit Distance

**Step 1: Recursive function.**
```java
int editDistance(String A, String B, int i, int j) {
    if (i == 0) return j;       // insert all of B
    if (j == 0) return i;       // delete all of A
    if (A.charAt(i-1) == B.charAt(j-1))
        return editDistance(A, B, i-1, j-1);
    return 1 + min(
        editDistance(A, B, i-1, j-1),    // replace
        editDistance(A, B, i-1, j),      // delete from A
        editDistance(A, B, i, j-1)       // insert into A
    );
}
```

**Step 2: Varying args.** `i` ranges `0..A.length()`, `j` ranges `0..B.length()`. Two dimensions.

**Step 3: Table.** `dp[A.length()+1][B.length()+1]`.

**Step 4: Base cases.** `dp[0][j] = j` (insert j characters); `dp[i][0] = i` (delete i characters).

**Step 5: Iteration order.** Recurrence references `(i-1, j-1)`, `(i-1, j)`, `(i, j-1)` — all "smaller" in some sense. Row-major top-to-bottom, left-to-right works.

**Step 6: Transition.**
```java
if (A.charAt(i-1) == B.charAt(j-1))
    dp[i][j] = dp[i-1][j-1];
else
    dp[i][j] = 1 + min(dp[i-1][j-1], dp[i-1][j], dp[i][j-1]);
```

**Step 7: Final cell.** `return dp[A.length()][B.length()];`

### 8.3 Common Conversion Pitfalls

**Pitfall 1: Off-by-one in indices.** Recursion often uses 1-based "characters consumed" semantics, while strings are 0-indexed. Always double-check: `dp[i][j]` refers to `A[0..i-1]` and `B[0..j-1]`, so the character being compared is `A.charAt(i-1)` and `B.charAt(j-1)`. Off-by-one here is the most common DP bug.

**Pitfall 2: Wrong base case propagation.** The recursive `if (i == 0) return j;` becomes a loop filling the entire row `dp[0][*] = j`. Forgetting to fill this row gives wrong answers.

**Pitfall 3: Missing the final cell.** Sometimes the answer is `dp[n][m]`, sometimes `dp[n-1][m-1]`, sometimes `max over dp[*]`. Confirm by re-reading the state definition.

**Pitfall 4: Translating early returns.** A recursion with `if (cond) return 0;` must translate to setting those base cells to 0 explicitly in the table before the main loops.

### 8.4 Conversion Checklist

- [ ] All varying recursion arguments are table dimensions.
- [ ] All recursion base cases have corresponding table initializations.
- [ ] The iteration order is a topological sort of the dependency graph.
- [ ] Index translations (1-based to 0-based) are correct.
- [ ] The returned cell matches the state definition.
- [ ] A small example dry-runs correctly through both versions.

### 8.5 When to Prefer Memoization vs Tabulation

| Aspect | Memoization (top-down) | Tabulation (bottom-up) |
|---|---|---|
| Implementation ease | Easier — mirrors recursion | Harder — must order states |
| Stack overflow | Risk for deep recursions | No risk |
| Constant factor | Slower (function call overhead) | Faster (tight loops) |
| Memory | Only visited states | All states |
| Space optimization | Hard | Easy (rolling rows) |
| Debugging | Easier (can print call tree) | Harder (table is opaque) |
| Interview impression | Shows recursion mastery | Shows DP mastery |

For interviews, the recommendation is: start with memoization to verify correctness, then convert to tabulation if asked about optimization. Many interviewers will explicitly ask you to "convert to bottom-up" — practice the conversion.

---

## 9. Space Optimization

2D DP tables can be large (10^4 × 10^4 = 10^8 cells). Space optimization reduces this without changing the time complexity. This section covers the full ladder of optimizations.

### 9.1 The Optimization Ladder

```
Full 2D Matrix         O(n*m) memory
       |
       v
Previous Row + Current Row   O(2*m) = O(m) memory
       |
       v
Single Rolling Row            O(m) memory
       |
       v
In-place Update               O(1) extra memory (modify input)
```

### 9.2 From Full Matrix to Two Rows

If `dp[i][j]` depends only on `dp[i-1][*]` (the previous row), then once row `i` is computed, row `i-2` is never needed again. We can keep just two rows: `prev` and `curr`.

```java
// Before: int[][] dp = new int[n][m];
// After:
int[] prev = new int[m];
int[] curr = new int[m];

for (int i = 0; i < n; i++) {
    for (int j = 0; j < m; j++) {
        curr[j] = compute(prev, curr, i, j);
    }
    int[] tmp = prev; prev = curr; curr = tmp;  // swap
}
return prev[m-1];  // answer is in the last "prev" after the final swap
```

Memory: O(2m) = O(m). Time: unchanged.

### 9.3 From Two Rows to One Row

If `curr[j]` depends only on `prev[j]`, `prev[j-1]`, and `curr[j-1]` (i.e., the current row's left neighbor is okay but no other current row cells), we can collapse to a single row and update it in place.

```java
int[] dp = new int[m];
for (int i = 0; i < n; i++) {
    for (int j = 0; j < m; j++) {
        // dp[j] currently holds the value from row i-1 (the "prev" value)
        // dp[j-1] currently holds the value from row i (the "curr" value)
        int fromAbove = dp[j];
        int fromLeft  = (j > 0) ? dp[j-1] : 0;
        dp[j] = ... // combine fromAbove, fromLeft, etc.
    }
}
```

This works because, when filling `dp[j]`, the value `dp[j]` still holds the previous row's value (the "above" source), and `dp[j-1]` has already been updated to the current row's value (the "left" source). Both needed values are available.

**Overwrite hazard:** If `dp[j]` depends on the *current* row's `dp[j+1]` (right neighbor), this trick fails because `dp[j+1]` hasn't been filled yet. You must either use a separate variable to carry the "above" value, or revert to two rows.

### 9.4 The "Save Before Overwrite" Trick

For shapes where `dp[j]` needs both `dp[j-1]` (new, current row) and `dp[j]` (old, previous row) and possibly `dp[j-1]` (old, previous row, the diagonal), you need to save the diagonal before it gets overwritten.

```java
int[] dp = new int[m];
for (int i = 1; i < n; i++) {
    int prevDiagonal = dp[0];  // save before overwriting
    for (int j = 1; j < m; j++) {
        int temp = dp[j];      // current value will become the next diagonal
        dp[j] = combine(dp[j], dp[j-1], prevDiagonal);
        prevDiagonal = temp;
    }
}
```

This is the canonical pattern for LCS and Edit Distance space optimization.

### 9.5 In-Place Update

If the input grid can be mutated, you can store DP values directly in the input grid, using O(1) extra memory. This is common for Minimum Path Sum and similar problems where the input grid is not needed after the DP completes.

```java
// Minimum Path Sum, in-place:
for (int i = 0; i < grid.length; i++) {
    for (int j = 0; j < grid[0].length; j++) {
        if (i == 0 && j == 0) continue;
        else if (i == 0) grid[i][j] += grid[i][j-1];
        else if (j == 0) grid[i][j] += grid[i-1][j];
        else grid[i][j] += Math.min(grid[i-1][j], grid[i][j-1]);
    }
}
return grid[grid.length-1][grid[0].length-1];
```

**Caveat:** Mutating the input is often considered bad practice in interviews unless explicitly allowed. Mention the trade-off to the interviewer.

### 9.6 Reverse Iteration for 0/1 Knapsack Compression

For knapsack-style problems where each item can be used at most once, the 1D compression requires reverse inner iteration:

```java
int[] dp = new int[W+1];
for (int i = 0; i < n; i++) {
    for (int w = W; w >= weight[i]; w--) {  // REVERSE
        dp[w] = Math.max(dp[w], dp[w - weight[i]] + value[i]);
    }
}
```

**Why reverse?** When computing `dp[w]`, we need `dp[w - weight[i]]` from the *previous* iteration (without item `i`). If we iterate forward, `dp[w - weight[i]]` has already been updated to include item `i`, leading to item `i` being used multiple times.

### 9.7 Memory Before and After — A Concrete Example

LCS of two strings of length 1000 each.

- Full 2D table: 1000 × 1000 × 4 bytes = 4 MB.
- Two rows: 2 × 1000 × 4 = 8 KB.
- One row: 1000 × 4 = 4 KB.
- One row with diagonal save: same 4 KB.

For 10^5 × 10^5 inputs (common in competitive programming), the differences are dramatic:
- Full table: 4 × 10^10 bytes = 40 GB (infeasible).
- One row: 4 × 10^5 bytes = 400 KB (trivial).

This is why space optimization is not optional in competitive programming and is increasingly expected in FAANG interviews.

### 9.8 When Space Optimization Breaks

Some 2D DPs cannot be compressed to 1D:
- **Interval DP.** `dp[l][r]` depends on sub-intervals of various lengths; you need the whole upper triangle of the table. No compression to O(n) is possible in general.
- **Problems where `dp[i][j]` depends on `dp[i-1][j+1]` (anti-diagonal).** The "next row, previous column" dependency prevents simple rolling.
- **Problems where you need to reconstruct the path/solution.** Reconstruction often needs the full table to trace back; you may need to keep all rows for path recovery (or use divide-and-conquer reconstruction).

### 9.9 Path Reconstruction with Compressed Space

If you need to reconstruct the actual solution (not just the optimal value), compressing space makes reconstruction harder. Two approaches:

**Approach 1: Store the full table for reconstruction only.** Use O(nm) memory but compute values using the compressed version. Hybrid approach.

**Approach 2: Divide-and-conquer reconstruction (Hirschberg's algorithm).** For LCS, Hirschberg's algorithm reconstructs the actual subsequence in O(nm) time and O(min(n,m)) space. This is advanced; mention it if the interviewer asks about extreme space optimization.

---


## 10. Pattern Recognition Guide

This section catalogs **60+ recognition patterns** — observations in the problem statement that hint at the right DP family. Use this as a lookup table when reading a new problem.

### 10.1 Recognition Patterns Table

| # | If you see... | Think... |
|---|---|---|
| 1 | A 2D grid | Grid DP |
| 2 | A matrix of values | Grid DP |
| 3 | "Move only right or down" | Grid DP |
| 4 | "Number of ways to reach cell" | Grid DP (Unique Paths) |
| 5 | "Minimum cost to reach bottom-right" | Grid DP (Minimum Path Sum) |
| 6 | "Obstacles in a grid" | Grid DP with obstacle handling |
| 7 | "Health/HP needed to survive" | Reverse Grid DP (Dungeon) |
| 8 | "Collect maximum cherries" | Two-pass Grid DP (Cherry Pickup) |
| 9 | "Triangle of numbers, top to bottom" | Triangle DP |
| 10 | "Falling path through matrix" | Falling Path DP |
| 11 | Two strings | Sequence DP |
| 12 | "Common subsequence" | LCS |
| 13 | "Common substring" (contiguous) | Longest Common Substring DP |
| 14 | "Edit operations" (insert/delete/replace) | Edit Distance |
| 15 | "Number of ways T appears in S" | Distinct Subsequences |
| 16 | "Shortest string containing both" | Shortest Common Supersequence |
| 17 | "Interleaving of two strings" | Interleaving String DP |
| 18 | "Palindrome" + subsequence | Longest Palindromic Subsequence |
| 19 | "Palindrome" + substring | Palindromic Substrings (expand or DP) |
| 20 | "Minimum cuts to make palindromes" | Palindrome Partitioning II |
| 21 | "Left and right boundary" | Interval DP |
| 22 | "Subarray of subarray" | Interval DP |
| 23 | "Cost of merging matrices" | Matrix Chain Multiplication |
| 24 | "Burst balloons" / "remove last" | Burst Balloons (reverse thinking) |
| 25 | "Parenthesize expression" | Boolean Parenthesization |
| 26 | "Eggs and floors" | Egg Dropping |
| 27 | "Optimal BST" | Interval DP with root iteration |
| 28 | "Partition array into k parts" | Partition DP |
| 29 | "Split array at any point" | Partition DP |
| 30 | "Maximum path sum" (grid) | Grid DP |
| 31 | "Maximum path sum" (tree) | Tree DP (not 2D) |
| 32 | "Maximum path sum" (matrix, 4-dir) | Dijkstra/DFS with memo |
| 33 | "Two independent indices" | 2D DP |
| 34 | "Index and a count" | 2D DP (index, count) |
| 35 | "Index and previous choice" | 2D DP (index, prev) |
| 36 | "Index and remaining resource" | 2D DP (index, resource) |
| 37 | "Index and bitmask of used items" | Bitmask DP |
| 38 | "Rectangle" / "matrix" + "max sum" | Kadane 2D or DP |
| 39 | "Square submatrix of all 1s" | Count Square Submatrices (DP) |
| 40 | "Maximal rectangle of 1s" | Histogram + DP |
| 41 | "Number of paths" | Counting DP (mod prime) |
| 42 | "Distinct ways" | Counting DP |
| 43 | "Is it possible?" | Boolean DP |
| 44 | "Minimize / Maximize" | Optimization DP |
| 45 | "k transactions" | 2D DP (day, transactions) |
| 46 | "k eggs / k attempts" | 2D DP (eggs, floors) |
| 47 | "Cooldown between actions" | 2D DP (day, state) |
| 48 | "Two players alternate" | Game DP (minimax) |
| 49 | "Predict winner" | Game DP |
| 50 | "Stone game" | Interval Game DP |
| 51 | "Burst / remove in any order" | Reverse-interval DP |
| 52 | "Strangers leave one by one" | Reverse-interval DP |
| 53 | "Paint houses, no two adjacent same" | 2D DP (house, color) |
| 54 | "Buy/sell with fee" | 2D DP (day, holding) |
| 55 | "Buy/sell with cooldown" | 2D DP (day, state) |
| 56 | "Subset with sum" | Subset Sum DP |
| 57 | "Partition into equal sum" | Subset Sum DP |
| 58 | "Target sum with +/-" | Subset Sum DP (offset) |
| 59 | "Coin change - number of ways" | Counting DP (unbounded) |
| 60 | "Coin change - minimum coins" | Optimization DP (unbounded) |
| 61 | "Word break" | Partition DP / 1D DP with set |
| 62 | "Word break - all sentences" | Partition DP with backtracking |
| 63 | "Regex matching (* and .)" | 2D DP (i, j) with star handling |
| 64 | "Wildcard matching" | 2D DP (i, j) |
| 65 | "Distinct subsequences of length k" | 2D DP (i, k) |
| 66 | "Number of music playlists" | 2D DP (i, j) with unique constraint |
| 67 | "Count of range sum" | 1D DP + merge sort or BIT |
| 68 | "Number of longest increasing subseq" | 2D DP (length, count) |
| 69 | "Russian doll envelopes" | 2D sort + 1D LIS DP |
| 70 | "Box stacking" | Variation DP |
| 71 | "Highest 3-product" | Often greedy / sort, not DP |
| 72 | "TSP / visit all cities" | Bitmask DP |
| 73 | "Matchsticks to square" | Partition DP / Bitmask |
| 74 | "Sudoku solver" | Backtracking, not DP |
| 75 | "N-Queens" | Backtracking, not DP |
| 76 | "Climbing stairs" | 1D Fibonacci-like DP |
| 77 | "Min cost climbing stairs" | 1D DP |
| 78 | "House robber" | 1D DP (skip/take) |
| 79 | "House robber in circle" | 1D DP, twice on subarrays |
| 80 | "Decode ways" | 1D DP with constraints |
| 81 | "Number of ways to partition" | Partition DP |
| 82 | "Best time to buy/sell stock I" | Greedy (min so far), not DP |
| 83 | "Best time to buy/sell stock II" | Greedy (sum of rises), not DP |
| 84 | "Best time to buy/sell stock III" (2 txn) | 2D DP (day, txn) |
| 85 | "Best time to buy/sell stock IV" (k txn) | 2D DP (day, txn) |
| 86 | "Best time to buy/sell with cooldown" | 2D DP (day, state) |
| 87 | "Best time to buy/sell with fee" | 2D DP (day, holding) |
| 88 | "Frog jump" | 1D DP with set |
| 89 | "Array with steps of 1..k" | 1D DP, sliding max |
| 90 | "Minimum difficulty of job schedule" | 2D DP (day, job index) |
| 91 | "Number of ways to stay in place" | 2D DP (steps, position) |
| 92 | "Knights dialer" | 2D DP (steps, digit) |
| 93 | "Domino and tromino tiling" | 1D DP with extension |
| 94 | "Soup serving" | DP with floats / memoization |
| 95 | "Out of boundary paths" | 2D DP (steps, position) |
| 96 | "Tiling a rectangle with fewest squares" | Hard DP + backtracking |
| 97 | "Cherry pickup II" (two robots) | 3D DP (row, col1, col2) |
| 98 | "Number of ways to form target string" | 2D DP (target index, word index) |
| 99 | "Form largest integer with k swaps" | Backtracking, not DP |
| 100 | "Predict the winner" | Interval Game DP |

### 10.2 Negative Patterns — What NOT to Use DP For

| If you see... | Use instead of DP... |
|---|---|
| "Find any path" (not optimal) | BFS / DFS |
| "Is there a path?" | Union-Find or BFS |
| "Shortest path, positive weights" | Dijkstra |
| "Shortest path, possibly negative" | Bellman-Ford |
| "All-pairs shortest" | Floyd-Warshall |
| "Maximum flow" | Max-flow algorithms |
| "Topological sort" | Kahn's or DFS |
| "Count connected components" | Union-Find |
| "Range sum query, static" | Prefix sum |
| "Range minimum query, static" | Sparse table |
| "Range update, point query" | Difference array |
| "Subarray sum equals k" | Hash map + prefix sum |
| "Two-sum / three-sum" | Hash map / sort + two-pointer |
| "Sliding window maximum" | Deque |
| "Longest substring without repeating" | Sliding window |
| "Maximum subarray" | Kadane's algorithm (which is technically 1D DP) |

### 10.3 Pattern Combinations

Many hard problems are combinations:
- **DP + Binary Search:** Split Array Largest Sum, Capacity To Ship Packages.
- **DP + Bitmask:** TSP, Smallest Sufficient Team.
- **DP + Trie:** Word Break II (efficient), Concatenated Words.
- **DP + Math/Combinatorics:** Number of Music Playlists, Count of n-digit numbers.
- **DP + Matrix Exponentiation:** Count ways for huge n (Fibonacci variants, tiling).
- **DP + Game Theory:** Predict Winner, Stone Game, Nim variants.
- **DP + Graph:** Longest path in DAG, Skienna-style problems.
- **DP + Greedy preprocessing:** Russian Doll Envelopes (sort then LIS).

### 10.4 Recognition Heuristics

When reading a problem, run these heuristics in order:

1. **Is it an optimization or counting problem on a sequence/grid?** If yes, DP is a candidate.
2. **Are there overlapping subproblems?** Try writing the brute-force recursion; if it visits the same arguments repeatedly, DP applies.
3. **Does it have optimal substructure?** Does the optimal solution for size n depend on optimal solutions for smaller sizes?
4. **How many state variables does the recursion need?** One → 1D DP. Two → 2D DP. Three+ → multidimensional DP, look for compression.
5. **Is the dependency shape regular?** Grid, sequence, interval, partition — these determine the iteration order.

If after this you are unsure, write a 5-line recursive solution and see what arguments vary. The varying arguments become your state.

---

## 11. 2D DP Pattern Atlas

This is the heart of the guide. Each subsection covers one canonical problem with full theoretical treatment, Java code, complexity analysis, and discussion of common mistakes.


### 11.1 Grid DP — Minimum Path Sum

**Problem Statement.** Given an `m × n` grid of non-negative numbers, find a path from the top-left to the bottom-right which minimizes the sum of all numbers along the path. You may only move right or down at any step.

**Recognition Clues.** 2D grid; "minimum sum"; "only right or down" → Grid DP.

**Pattern.** Grid DP, forward direction.

**Difficulty.** Easy.

**Companies.** Google, Amazon, Microsoft, Bloomberg, Apple.

**Constraints.** `m, n ≥ 1`; values fit in int; result fits in int.

**Observations.**
- Each cell `(i, j)` can be reached only from `(i-1, j)` (above) or `(i, j-1)` (left).
- The minimum path sum to `(i, j)` is the cell's value plus the minimum of the two incoming path sums.
- The first row can only be reached from the left; the first column can only be reached from above.

**Brute Force.** Enumerate all 2^(m+n) paths; compute each sum; pick the minimum. Time O(2^(m+n)).

**Better Solution.** Recursion with memoization. `f(i, j)` = min path sum from `(0,0)` to `(i,j)`. Memoize on `(i,j)`. Time O(mn).

**Optimal Solution.** Tabulation, with optional in-place update.

**State Design.** `dp[i][j]` = minimum path sum from `(0,0)` to `(i,j)`.

**Transition Derivation.**
- `dp[0][0] = grid[0][0]`.
- `dp[0][j] = dp[0][j-1] + grid[0][j]` for `j > 0` (first row, only from left).
- `dp[i][0] = dp[i-1][0] + grid[i][0]` for `i > 0` (first column, only from above).
- `dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1])` for `i, j > 0`.

**Dependency Graph.**
```
       j-1     j
i-1            +---> depends on dp[i-1][j]
i     +---> depends on dp[i][j-1]
              dp[i][j] = grid[i][j] + min(above, left)
```

**DP Table Evolution (Example).** Grid:
```
1  3  1
1  5  1
4  2  1
```
After filling:
```
1  4  5
2  7  6
6  8  7
```
Answer = 7 (path: 1→3→1→1→1 — wait, that's 1+3+1+1+1=7? Actually: 1→1→4→2→1? Let me retrace: 1→3→1→1→1 = 7; 1→1→4→2→1 = 9; 1→1→5→1→1 = 9; 1→3→5→1→1=11. Min is 7.)

**Dry Run.** Start with `dp[0][0]=1`. Row 0: 1, 1+3=4, 4+1=5. Row 1: 1+1=2, min(2,4)+5=7, min(7,5)+1=6. Row 2: 2+4=6, min(6,7)+2=8, min(8,6)+1=7. Return `dp[2][2]=7`.

**Memory Layout.** O(mn) full table; O(n) with rolling row; O(1) with in-place.

**Java Solution (in-place).**
```java
class Solution {
    public int minPathSum(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (i == 0 && j == 0) continue;
                else if (i == 0) grid[i][j] += grid[i][j-1];
                else if (j == 0) grid[i][j] += grid[i-1][j];
                else grid[i][j] += Math.min(grid[i-1][j], grid[i][j-1]);
            }
        }
        return grid[m-1][n-1];
    }
}
```

**Java Solution (1D rolling).**
```java
class Solution {
    public int minPathSum(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[] dp = new int[n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (i == 0 && j == 0) dp[j] = grid[i][j];
                else if (i == 0) dp[j] = dp[j-1] + grid[i][j];
                else if (j == 0) dp[j] = dp[j] + grid[i][j];
                else dp[j] = Math.min(dp[j], dp[j-1]) + grid[i][j];
            }
        }
        return dp[n-1];
    }
}
```

**Complexity.** Time O(mn). Space O(mn) full / O(n) rolling / O(1) in-place.

**Common Mistakes.**
- Forgetting the first-row/first-column base cases.
- Iterating bottom-to-top (dependencies come from above).
- Mixing up `dp[i-1][j]` (above) and `dp[i][j-1]` (left).
- Off-by-one: writing `grid[i-1][j]` instead of `dp[i-1][j]` after the first row.

**Follow-up Questions.**
- What if you can move in all 4 directions? → Use Dijkstra (no DAG).
- What if obstacles exist? → See Unique Paths II.
- What if you need to print the path? → Store predecessor or backtrack through `dp`.

**Similar Problems.** Unique Paths, Dungeon Game, Cherry Pickup, Gold Mine Problem, Minimum Falling Path Sum.

---

### 11.2 Grid DP — Unique Paths

**Problem Statement.** A robot is at the top-left corner of an `m × n` grid. It can only move right or down. How many unique paths are there to the bottom-right corner?

**Recognition Clues.** Grid; "how many paths"; "only right or down" → Grid DP (counting).

**Pattern.** Grid DP, counting variant.

**Difficulty.** Easy.

**Companies.** Google, Apple, Bloomberg, Goldman Sachs.

**Constraints.** `m, n ≤ 100`; result fits in int (use long for safety in some variants).

**Observations.**
- Each cell's path count = (paths to cell above) + (paths to cell to the left).
- First row and first column have exactly 1 path each (only one direction possible).

**Brute Force.** Recursion: `f(i,j) = f(i-1,j) + f(i,j-1)`. Time O(2^(m+n)).

**Optimal Solution.** DP or combinatorics.

**State Design.** `dp[i][j]` = number of unique paths from `(0,0)` to `(i,j)`.

**Transition.** `dp[i][j] = dp[i-1][j] + dp[i][j-1]`, with `dp[0][j] = dp[i][0] = 1`.

**Dependency Graph.** Same as Minimum Path Sum (above + left).

**DP Table Evolution (m=3, n=4).**
```
1  1  1  1
1  2  3  4
1  3  6  10
```
Answer = 10. (Each cell is the sum of the cell above and the cell to the left.)

**Java Solution (1D rolling).**
```java
class Solution {
    public int uniquePaths(int m, int n) {
        int[] dp = new int[n];
        java.util.Arrays.fill(dp, 1);
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                dp[j] += dp[j-1];
            }
        }
        return dp[n-1];
    }
}
```

**Combinatorics Solution.** The number of paths is `C(m+n-2, m-1)`. Compute via Pascal's triangle or multiplicative formula. Time O(min(m,n)), space O(1).

**Complexity.** DP: Time O(mn), Space O(n) rolling. Combinatorics: Time O(min(m,n)), Space O(1).

**Common Mistakes.**
- Initializing `dp[0][0] = 0` instead of 1 (the starting cell has 1 way to reach itself).
- Integer overflow for large m, n — use long or BigInteger.
- Confusing row and column counts.

**Follow-up Questions.**
- Print all paths? → Backtracking.
- Path with obstacles? → Unique Paths II.
- Random path generation? → Reservoir sampling on the path space.

**Similar Problems.** Unique Paths II, Minimum Path Sum, Dungeon Game, Climbing Stairs.

---

### 11.3 Grid DP — Unique Paths II

**Problem Statement.** Same as Unique Paths, but with obstacles (cells marked 1 are blocked). Count paths avoiding obstacles.

**Recognition Clues.** Grid + obstacles + "count paths" → Grid DP with obstacle handling.

**Pattern.** Grid DP, counting with forbidden cells.

**Difficulty.** Medium.

**Companies.** Google, Bloomberg, Microsoft.

**Constraints.** `m, n ≤ 100`.

**Observations.**
- If a cell is an obstacle, `dp[i][j] = 0` (no paths through it).
- Otherwise, `dp[i][j] = dp[i-1][j] + dp[i][j-1]`.
- If the starting cell or ending cell is an obstacle, answer is 0.
- First row/column: once an obstacle is hit, all subsequent cells in that row/column are 0.

**State Design.** `dp[i][j]` = number of paths from `(0,0)` to `(i,j)` avoiding obstacles.

**Transition.**
```
if obstacleGrid[i][j] == 1:
    dp[i][j] = 0
else if i == 0 and j == 0:
    dp[i][j] = 1
else:
    dp[i][j] = (i > 0 ? dp[i-1][j] : 0) + (j > 0 ? dp[i][j-1] : 0)
```

**DP Table Evolution.** Grid:
```
0  0  0
0  1  0
0  0  0
```
After filling:
```
1  1  1
1  0  1
1  1  2
```
Answer = 2.

**Java Solution.**
```java
class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.length, n = obstacleGrid[0].length;
        if (obstacleGrid[0][0] == 1 || obstacleGrid[m-1][n-1] == 1) return 0;
        int[] dp = new int[n];
        dp[0] = 1;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (obstacleGrid[i][j] == 1) dp[j] = 0;
                else if (j > 0) dp[j] += dp[j-1];
                // j == 0: dp[j] keeps its previous row value (or 0 if obstacle in this col)
            }
        }
        return dp[n-1];
    }
}
```

**Complexity.** Time O(mn). Space O(n).

**Common Mistakes.**
- Not checking start/end for obstacles.
- In 1D rolling, forgetting to reset `dp[j]` to 0 when an obstacle is hit.
- First-row propagation: once an obstacle in the first row, all subsequent first-row cells are 0.

**Follow-up.** Path printing with obstacles; obstacle removal cost.

**Similar Problems.** Unique Paths, Minimum Path Sum with obstacles, The Maze.

---

### 11.4 Grid DP — Dungeon Game

**Problem Statement.** A princess is imprisoned in the bottom-right corner of a dungeon. The knight starts at the top-left. Each cell has a number (positive = heal, negative = damage). The knight must reach the princess with HP > 0 at every cell. Find the minimum initial HP needed.

**Recognition Clues.** Grid; "minimum initial HP"; "must stay alive" → Reverse Grid DP.

**Pattern.** Grid DP, reverse direction.

**Difficulty.** Hard.

**Companies.** Google, Microsoft, Amazon.

**Constraints.** `m, n ≤ 200`; values fit in int.

**Observations.**
- Forward DP (top-left to bottom-right) does not work because the optimal "current HP" depends on what comes *after*, not what came before.
- The natural state is reverse: "minimum HP needed at cell (i,j) to reach the end alive."
- Base case at the bottom-right, iterate backward.

**State Design.** `dp[i][j]` = minimum HP the knight must have *upon entering* cell `(i,j)` to reach the princess alive.

**Transition.**
- The knight needs HP `h` such that `h + dungeon[i][j] >= 1` (alive at this cell) and `h + dungeon[i][j] >= next_required` (enough to proceed).
- The minimum HP at `(i,j)` is `max(1, next_required - dungeon[i][j])`, where `next_required = min(dp[i+1][j], dp[i][j+1])` (choose the easier next step).
- Boundary: for the last row, only `dp[i][j+1]`; for the last column, only `dp[i+1][j]`; for the bottom-right cell, `dp[m-1][n-1] = max(1, 1 - dungeon[m-1][n-1])`.

**Dependency Graph (Reverse).**
```
       j       j+1
i              +---> dp[i][j] depends on dp[i][j+1] (right)
i+1   +---> dp[i][j] depends on dp[i+1][j] (below)
```

**Iteration Order.** Bottom-to-top, right-to-left within row.

**DP Table Evolution.** Dungeon:
```
-2  -3   3
-5  -10  1
10  30  -5
```
Filled `dp` (min HP on entry):
```
7   5   2
6   11  5
1   1   6
```
Answer = 7.

**Dry Run.** `dp[2][2] = max(1, 1-(-5)) = 6`. `dp[2][1] = max(1, dp[2][2] - 30) = max(1, -24) = 1`. `dp[2][0] = max(1, dp[2][1] - 10) = max(1, -9) = 1`. `dp[1][2] = max(1, dp[2][2] - 1) = max(1, 5) = 5`. `dp[1][1] = max(1, min(dp[2][1], dp[1][2]) - (-10)) = max(1, min(1, 5) + 10) = max(1, 11) = 11`. `dp[1][0] = max(1, min(dp[2][0], dp[1][1]) - (-5)) = max(1, min(1, 11) + 5) = max(1, 6) = 6`. `dp[0][2] = max(1, dp[1][2] - 3) = max(1, 2) = 2`. `dp[0][1] = max(1, min(dp[1][1], dp[0][2]) - (-3)) = max(1, min(11, 2) + 3) = max(1, 5) = 5`. `dp[0][0] = max(1, min(dp[1][0], dp[0][1]) - (-2)) = max(1, min(6, 5) + 2) = max(1, 7) = 7`. Return 7.

**Java Solution.**
```java
class Solution {
    public int calculateMinimumHP(int[][] dungeon) {
        int m = dungeon.length, n = dungeon[0].length;
        int[][] dp = new int[m][n];
        for (int i = m - 1; i >= 0; i--) {
            for (int j = n - 1; j >= 0; j--) {
                if (i == m - 1 && j == n - 1) {
                    dp[i][j] = Math.max(1, 1 - dungeon[i][j]);
                } else if (i == m - 1) {
                    dp[i][j] = Math.max(1, dp[i][j+1] - dungeon[i][j]);
                } else if (j == n - 1) {
                    dp[i][j] = Math.max(1, dp[i+1][j] - dungeon[i][j]);
                } else {
                    int nextRequired = Math.min(dp[i+1][j], dp[i][j+1]);
                    dp[i][j] = Math.max(1, nextRequired - dungeon[i][j]);
                }
            }
        }
        return dp[0][0];
    }
}
```

**Complexity.** Time O(mn). Space O(mn) full / O(n) rolling (tricky with reverse direction — keep two rows).

**Common Mistakes.**
- Trying forward DP — wrong because initial HP depends on future.
- Forgetting the `max(1, ...)` — HP must always be at least 1.
- Using `min` where `max` is needed (and vice versa) — the logic is "minimum HP required," which is `max(1, ...)`, but the choice of next cell is the easier one, `min`.

**Follow-up.** What if you can move in all 4 directions? → Harder; needs Dijkstra-like with HP state. What if you can skip one cell? → Add a dimension.

**Similar Problems.** Cherry Pickup, Minimum Path Sum, Dungeon variants with extra items.

---

### 11.5 Grid DP — Cherry Pickup

**Problem Statement.** An `n × n` grid has cells with 0, 1, or -1. 1 = cherry, -1 = thorn (blocked). You go from `(0,0)` to `(n-1, n-1)` moving right/down, then back to `(0,0)` moving left/up. Collect maximum cherries (a cell's cherry is collected once even if visited twice).

**Recognition Clues.** Grid; "go and return"; "maximum collected" → Two-pass Grid DP.

**Pattern.** Grid DP with two synchronized paths.

**Difficulty.** Hard.

**Companies.** Google, Microsoft.

**Constraints.** `n ≤ 50`.

**Observations.**
- Going there and back is equivalent to sending two people from `(0,0)` to `(n-1,n-1)` simultaneously.
- After `t` total steps, both robots are at row `r1 = r2 = t - c` for some columns `c1, c2`.
- Equivalently: track `(r1, c1, r2)` with `c2 = r1 + c1 - r2` (same total steps).

**State Design.** `dp[r1][c1][r2]` = max cherries collected by two paths from `(0,0)` to `(r1,c1)` and `(r2,c2)`, where `c2 = r1 + c1 - r2`.

**Transition.** Both robots move right or down (4 combinations). For each, sum cherries at the two new cells (don't double-count if same cell). Take max of valid moves.

**Iteration Order.** Forward, by total steps (= r1 + c1 = r2 + c2).

**Java Solution (3D).**
```java
class Solution {
    public int cherryPickup(int[][] grid) {
        int n = grid.length;
        int[][][] dp = new int[n][n][n];
        for (int[][] a : dp) for (int[] b : a) java.util.Arrays.fill(b, Integer.MIN_VALUE);
        dp[0][0][0] = grid[0][0];
        for (int r1 = 0; r1 < n; r1++) {
            for (int c1 = 0; c1 < n; c1++) {
                for (int r2 = 0; r2 < n; r2++) {
                    int c2 = r1 + c1 - r2;
                    if (c2 < 0 || c2 >= n) continue;
                    if (grid[r1][c1] == -1 || grid[r2][c2] == -1) continue;
                    int best = Integer.MIN_VALUE;
                    int[][] prev = {{r1-1, c1, r2-1}, {r1-1, c1, r2}, {r1, c1-1, r2-1}, {r1, c1-1, r2}};
                    for (int[] p : prev) {
                        int pr1 = p[0], pc1 = p[1], pr2 = p[2];
                        int pc2 = pr1 + pc1 - pr2;
                        if (pr1 < 0 || pc1 < 0 || pr2 < 0 || pc2 < 0) continue;
                        if (dp[pr1][pc1][pr2] == Integer.MIN_VALUE) continue;
                        best = Math.max(best, dp[pr1][pc1][pr2]);
                    }
                    if (best == Integer.MIN_VALUE) continue;  // unreachable
                    int cherries = grid[r1][c1] + (r1 == r2 ? 0 : grid[r2][c2]);
                    dp[r1][c1][r2] = best + cherries;
                }
            }
        }
        return Math.max(0, dp[n-1][n-1][n-1]);
    }
}
```

**Complexity.** Time O(n^3). Space O(n^3) / O(n^2) rolling.

**Common Mistakes.**
- Trying to solve it as two separate DPs — the two paths interact (same cherry can't be counted twice).
- Forgetting to handle the "same cell" case (don't double-count).
- Not handling -1 (thorn) cells correctly.

**Follow-up.** Cherry Pickup II (two robots in a 2D grid with different mechanics) — LeetCode 1463.

---

### 11.6 Triangle DP — Triangle Minimum Path

**Problem Statement.** Given a triangle of numbers, find the minimum path sum from top to bottom. Each step you may move to the adjacent number on the row below (i.e., from `(i,j)` you can go to `(i+1,j)` or `(i+1,j+1)`).

**Recognition Clues.** Triangular structure; "top to bottom"; "adjacent on next row" → Triangle DP.

**Pattern.** Grid DP variant with triangular shape.

**Difficulty.** Medium.

**Companies.** Google, Amazon, Apple, Bloomberg.

**Constraints.** `n ≤ 200`; values fit in int.

**Observations.**
- Standard grid DP, but each row has different length.
- Two directions: top-down (from row 0 to row n-1) or bottom-up (from row n-1 to row 0).
- Bottom-up is simpler: no need to track "minimum of two ending positions."

**State Design.** `dp[i][j]` = minimum path sum from `(i,j)` to the bottom.

**Transition.** `dp[i][j] = triangle[i][j] + min(dp[i+1][j], dp[i+1][j+1])`.

**Base case.** `dp[n-1][j] = triangle[n-1][j]` for all `j`.

**Iteration Order.** Bottom-to-top.

**Java Solution (in-place, bottom-up).**
```java
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int n = triangle.size();
        int[] dp = new int[n];
        for (int j = 0; j < n; j++) dp[j] = triangle.get(n-1).get(j);
        for (int i = n - 2; i >= 0; i--) {
            for (int j = 0; j <= i; j++) {
                dp[j] = triangle.get(i).get(j) + Math.min(dp[j], dp[j+1]);
            }
        }
        return dp[0];
    }
}
```

**Complexity.** Time O(n^2). Space O(n) rolling.

**Common Mistakes.**
- Top-down vs bottom-up index confusion.
- Forgetting the in-place overwriting: when going top-down, you need a separate array; when going bottom-up, in-place works.

**Follow-up.** Print the path; triangle with obstacles.

---

### 11.7 Grid DP — Minimum Falling Path Sum

**Problem Statement.** Given an `n × n` matrix, find the minimum sum of a falling path from top to bottom. From `(i,j)` you can fall to `(i+1, j-1)`, `(i+1, j)`, or `(i+1, j+1)` (if valid).

**Recognition Clues.** Matrix; "falling path"; three downward choices → Falling Path DP.

**Pattern.** Grid DP, three-source transition.

**Difficulty.** Medium.

**Companies.** Google, Amazon, Microsoft.

**Constraints.** `n ≤ 100`.

**Observations.**
- Each cell depends on three cells above (above-left, above, above-right).
- First row is the base case.
- Compresses to a single row easily.

**State Design.** `dp[i][j]` = minimum falling path sum ending at `(i,j)`.

**Transition.** `dp[i][j] = matrix[i][j] + min(dp[i-1][j-1], dp[i-1][j], dp[i-1][j+1])` (ignoring invalid indices).

**Java Solution (1D rolling).**
```java
class Solution {
    public int minFallingPathSum(int[][] matrix) {
        int n = matrix.length;
        int[] dp = matrix[0].clone();
        for (int i = 1; i < n; i++) {
            int[] next = new int[n];
            for (int j = 0; j < n; j++) {
                int best = dp[j];
                if (j > 0) best = Math.min(best, dp[j-1]);
                if (j < n-1) best = Math.min(best, dp[j+1]);
                next[j] = matrix[i][j] + best;
            }
            dp = next;
        }
        int ans = Integer.MAX_VALUE;
        for (int v : dp) ans = Math.min(ans, v);
        return ans;
    }
}
```

**Complexity.** Time O(n^2). Space O(n).

**Common Mistakes.**
- Boundary check for `j-1` and `j+1`.
- Returning the last cell instead of the minimum of the last row.

**Follow-up.** Minimum falling path sum with up to k column shifts; falling path with two robots (Cherry Pickup II).

**Similar Problems.** Triangle, Cherry Pickup II, Minimum Path Sum.


### 11.8 Sequence DP — Longest Common Subsequence

**Problem Statement.** Given two strings `text1` and `text2`, return the length of their longest common subsequence (LCS). A subsequence is derived by deleting some characters without changing the order of the remaining characters.

**Recognition Clues.** Two strings; "common subsequence"; "longest" → Sequence DP (LCS).

**Pattern.** Sequence DP, match/mismatch transition.

**Difficulty.** Medium.

**Companies.** Google, Amazon, Microsoft, Apple, Bloomberg, Goldman Sachs, Meta, Adobe.

**Constraints.** `1 ≤ text1.length, text2.length ≤ 1000`.

**Observations.**
- Two indices evolving independently.
- If `text1[i-1] == text2[j-1]`, this character can extend the LCS of the smaller prefixes.
- If not, the LCS is the best of dropping one character from either side.

**Brute Force.** Recursion: `f(i, j)` exploring all pairs. Time O(2^(m+n)).

**Optimal Solution.** 2D DP, with 1D space optimization.

**State Design.** `dp[i][j]` = LCS length of `text1[0..i-1]` and `text2[0..j-1]`.

**Transition Derivation.**
- Match case (`text1[i-1] == text2[j-1]`): `dp[i][j] = 1 + dp[i-1][j-1]`.
- Mismatch case: `dp[i][j] = max(dp[i-1][j], dp[i][j-1])`.

**Dependency Graph.**
```
       j-1     j
i-1   [d]    [a]
i     [l]    [X]   X reads d (diagonal, if match), a (above), l (left)
```

**DP Table Evolution.** `text1 = "abcde"`, `text2 = "ace"`:
```
    ""  a  c  e
""   0  0  0  0
a    0  1  1  1
b    0  1  1  1
c    0  1  2  2
d    0  1  2  2
e    0  1  2  3
```
Answer = 3 (the LCS is "ace").

**Memory Layout.** O(mn) full / O(min(m,n)) with rolling + diagonal save.

**Java Solution (2D).**
```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int m = text1.length(), n = text2.length();
        int[][] dp = new int[m+1][n+1];
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (text1.charAt(i-1) == text2.charAt(j-1))
                    dp[i][j] = 1 + dp[i-1][j-1];
                else
                    dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
            }
        }
        return dp[m][n];
    }
}
```

**Java Solution (1D with diagonal save).**
```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int m = text1.length(), n = text2.length();
        // Ensure text2 is the shorter one for O(min(m,n)) space
        if (m < n) return longestCommonSubsequence(text2, text1);
        int[] dp = new int[n+1];
        for (int i = 1; i <= m; i++) {
            int prevDiag = dp[0];
            for (int j = 1; j <= n; j++) {
                int temp = dp[j];
                if (text1.charAt(i-1) == text2.charAt(j-1))
                    dp[j] = 1 + prevDiag;
                else
                    dp[j] = Math.max(dp[j], dp[j-1]);
                prevDiag = temp;
            }
        }
        return dp[n];
    }
}
```

**Complexity.** Time O(mn). Space O(mn) full / O(min(m,n)) optimized.

**Common Mistakes.**
- Index confusion: comparing `text1.charAt(i-1)` because `dp[i]` represents first `i` characters.
- Forgetting the diagonal save in 1D optimization — leads to using the wrong (already-overwritten) value.
- Using `dp[i-1][j-1]` instead of `dp[i][j-1]` or `dp[i-1][j]` in the mismatch case.

**Follow-up Questions.**
- Print the LCS string (not just length)? → Backtrack through `dp`.
- O(min(m,n)) space and O(mn) time with full reconstruction? → Hirschberg's algorithm.
- LCS of k strings? → k-dimensional DP, exponential in k.
- LCS with at most k mismatches allowed? → Add a dimension for mismatch count.

**Similar Problems.** Longest Common Substring, Shortest Common Supersequence, Edit Distance, Delete Operation for Two Strings (LCS variant), Minimum ASCII Delete Sum.

---

### 11.9 Sequence DP — Longest Common Substring

**Problem Statement.** Given two strings, find the length of their longest common substring (contiguous).

**Recognition Clues.** Two strings; "common substring" (contiguous); "longest" → Sequence DP, but with reset on mismatch.

**Pattern.** Sequence DP, no skip allowed.

**Difficulty.** Medium.

**Companies.** Google, Amazon, Microsoft, Adobe.

**Constraints.** Lengths up to 10^4 (use space optimization).

**Observations.**
- Unlike LCS, the substring must be contiguous, so a mismatch resets the count.
- `dp[i][j]` = length of the longest common suffix of `text1[0..i-1]` and `text2[0..j-1]`.

**State Design.** `dp[i][j]` = length of the longest common suffix of prefixes `text1[0..i-1]` and `text2[0..j-1]`.

**Transition.**
- If `text1[i-1] == text2[j-1]`: `dp[i][j] = 1 + dp[i-1][j-1]`.
- Else: `dp[i][j] = 0` (reset).
- Track the global max during the fill.

**Java Solution (1D).**
```java
class Solution {
    public int longestCommonSubstring(String text1, String text2) {
        int m = text1.length(), n = text2.length(), ans = 0;
        int[] dp = new int[n+1];
        for (int i = 1; i <= m; i++) {
            int prevDiag = dp[0];
            for (int j = 1; j <= n; j++) {
                int temp = dp[j];
                if (text1.charAt(i-1) == text2.charAt(j-1)) {
                    dp[j] = 1 + prevDiag;
                    ans = Math.max(ans, dp[j]);
                } else {
                    dp[j] = 0;
                }
                prevDiag = temp;
            }
        }
        return ans;
    }
}
```

**Complexity.** Time O(mn). Space O(min(m,n)).

**Common Mistakes.**
- Forgetting to reset to 0 on mismatch (unlike LCS where you take max).
- Forgetting to track the global max (the answer is not in `dp[m][n]`, it's the max over all cells).

**Follow-up.** Print the substring; multiple common substrings; longest common substring of k strings.

---

### 11.10 Edit Distance

**Problem Statement.** Given two strings `word1` and `word2`, return the minimum number of operations (insert, delete, replace) to convert `word1` to `word2`.

**Recognition Clues.** Two strings; "minimum operations"; "insert/delete/replace" → Edit Distance.

**Pattern.** Sequence DP, three-source transition.

**Difficulty.** Medium-Hard.

**Companies.** Google, Microsoft, Amazon, Meta, Bloomberg, LinkedIn.

**Constraints.** Lengths up to 500 (10^3 in some variants).

**Observations.**
- At each `(i,j)`, three operations are possible if characters mismatch.
- Match case: free (no operation).

**State Design.** `dp[i][j]` = edit distance between `word1[0..i-1]` and `word2[0..j-1]`.

**Transition.**
- Match: `dp[i][j] = dp[i-1][j-1]`.
- Mismatch: `dp[i][j] = 1 + min(dp[i-1][j-1], dp[i-1][j], dp[i][j-1])` corresponding to replace, delete, insert.
- Base: `dp[0][j] = j` (insert all of word2); `dp[i][0] = i` (delete all of word1).

**DP Table Evolution.** `word1 = "horse"`, `word2 = "ros"`:
```
    ""  r  o  s
""   0  1  2  3
h    1  1  2  3
o    2  2  1  2
r    3  2  2  2
s    4  3  3  2
e    5  4  4  3
```
Answer = 3 (horse → rorse → rose → ros).

**Java Solution (1D with diagonal save).**
```java
class Solution {
    public int minDistance(String word1, String word2) {
        int m = word1.length(), n = word2.length();
        int[] dp = new int[n+1];
        for (int j = 0; j <= n; j++) dp[j] = j;
        for (int i = 1; i <= m; i++) {
            int prevDiag = dp[0];
            dp[0] = i;
            for (int j = 1; j <= n; j++) {
                int temp = dp[j];
                if (word1.charAt(i-1) == word2.charAt(j-1))
                    dp[j] = prevDiag;
                else
                    dp[j] = 1 + Math.min(prevDiag, Math.min(dp[j], dp[j-1]));
                prevDiag = temp;
            }
        }
        return dp[n];
    }
}
```

**Complexity.** Time O(mn). Space O(min(m,n)).

**Common Mistakes.**
- Forgetting the `dp[0] = i` update at the start of each row in 1D optimization.
- Using `dp[i-1][j-1]` for the insert/delete cases instead of `dp[i-1][j]` / `dp[i][j-1]`.
- Confusing which operation each diagonal corresponds to.

**Follow-up.** Only insert/delete (no replace) → reduces to LCS-based solution. Variable operation costs. Edit distance with k operations allowed.

---

### 11.11 Distinct Subsequences

**Problem Statement.** Given strings `s` and `t`, return the number of distinct subsequences of `s` which equal `t`.

**Recognition Clues.** Two strings; "number of ways"; "subsequence equals" → Distinct Subsequences.

**Pattern.** Sequence DP, counting variant.

**Difficulty.** Hard.

**Companies.** Google, Bloomberg, Meta.

**Constraints.** Lengths up to 1000.

**Observations.**
- For each character of `s`, decide: use it (if it matches the current `t` character) or skip it.
- Counting DP: sum over valid choices.

**State Design.** `dp[i][j]` = number of distinct subsequences of `s[0..i-1]` that equal `t[0..j-1]`.

**Transition.**
- If `s[i-1] == t[j-1]`: `dp[i][j] = dp[i-1][j-1]` (use this character) `+ dp[i-1][j]` (skip it).
- Else: `dp[i][j] = dp[i-1][j]` (must skip).
- Base: `dp[i][0] = 1` (one way to form empty t: skip everything); `dp[0][j] = 0` for `j > 0`.

**Java Solution.**
```java
class Solution {
    public int numDistinct(String s, String t) {
        int m = s.length(), n = t.length();
        int[] dp = new int[n+1];
        dp[0] = 1;
        for (int i = 1; i <= m; i++) {
            int prevDiag = dp[0];
            for (int j = 1; j <= n; j++) {
                int temp = dp[j];
                if (s.charAt(i-1) == t.charAt(j-1))
                    dp[j] = dp[j] + prevDiag;
                // else: dp[j] stays (skip)
                prevDiag = temp;
            }
        }
        return dp[n];
    }
}
```

**Complexity.** Time O(mn). Space O(n).

**Common Mistakes.**
- Order of summation — must use the *previous* row's `dp[j-1]`, hence the diagonal save.
- Modulo arithmetic for large counts.
- Base case `dp[0][0] = 1` (empty s forms empty t in one way).

**Follow-up.** Number of distinct subsequences of a single string (1D DP with last-occurrence tracking).

---

### 11.12 Shortest Common Supersequence

**Problem Statement.** Given two strings `str1` and `str2`, return the shortest string that has both `str1` and `str2` as subsequences.

**Recognition Clues.** Two strings; "shortest common supersequence" → SCS.

**Pattern.** Sequence DP, derived from LCS.

**Difficulty.** Hard.

**Companies.** Google, Microsoft, Meta.

**Constraints.** Lengths up to 1000.

**Observations.**
- SCS length = `len(str1) + len(str2) - LCS(str1, str2)`.
- To construct the actual SCS, use the LCS table to merge the two strings.

**State Design.** Same as LCS, but with path reconstruction.

**Java Solution.**
```java
class Solution {
    public String shortestCommonSupersequence(String str1, String str2) {
        int m = str1.length(), n = str2.length();
        int[][] dp = new int[m+1][n+1];
        for (int i = 1; i <= m; i++)
            for (int j = 1; j <= n; j++)
                if (str1.charAt(i-1) == str2.charAt(j-1))
                    dp[i][j] = 1 + dp[i-1][j-1];
                else
                    dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);

        // Reconstruct
        StringBuilder sb = new StringBuilder();
        int i = m, j = n;
        while (i > 0 || j > 0) {
            if (i == 0) sb.append(str2.charAt(--j));
            else if (j == 0) sb.append(str1.charAt(--i));
            else if (str1.charAt(i-1) == str2.charAt(j-1)) {
                sb.append(str1.charAt(i-1)); i--; j--;
            } else if (dp[i-1][j] > dp[i][j-1]) {
                sb.append(str1.charAt(i-1)); i--;
            } else {
                sb.append(str2.charAt(j-1)); j--;
            }
        }
        return sb.reverse().toString();
    }
}
```

**Complexity.** Time O(mn). Space O(mn) for reconstruction.

**Common Mistakes.**
- Forgetting that SCS reconstruction needs the full 2D table (cannot easily do with 1D).
- Index confusion during reconstruction.

---

### 11.13 Longest Palindromic Subsequence

**Problem Statement.** Given a string `s`, find the length of the longest palindromic subsequence.

**Recognition Clues.** Single string; "palindromic subsequence"; "longest" → LPS.

**Pattern.** Interval DP on a single string, or LCS of `s` and `reverse(s)`.

**Difficulty.** Medium.

**Companies.** Google, Amazon, Microsoft, Bloomberg, Apple.

**Constraints.** Length up to 1000.

**Observations.**
- Two equivalent approaches:
  - **Approach 1 (Interval DP):** `dp[i][j]` = LPS of `s[i..j]`.
  - **Approach 2 (LCS):** `LPS(s) = LCS(s, reverse(s))`.

**State Design (Interval).** `dp[i][j]` = LPS length of `s[i..j]`.

**Transition.**
- If `s[i] == s[j]`: `dp[i][j] = 2 + dp[i+1][j-1]` (for `i < j`) or `1` (for `i == j`).
- Else: `dp[i][j] = max(dp[i+1][j], dp[i][j-1])`.
- Base: `dp[i][i] = 1`.

**Iteration Order.** By gap length (`j - i`), increasing.

**DP Table Evolution.** `s = "bbbab"`:
```
gap 0: dp[i][i] = 1
gap 1: dp[0][1] = 2 (bb match), dp[1][2] = 2 (bb match), dp[2][3] = 1, dp[3][4] = 1
gap 2: dp[0][2] = 3 (bbb), dp[1][3] = 2, dp[2][4] = 1
gap 3: dp[0][3] = 3, dp[1][4] = 2
gap 4: dp[0][4] = 4 (bbbb)
```
Answer = 4 (the LPS is "bbbb").

**Java Solution (Interval DP).**
```java
class Solution {
    public int longestPalindromeSubseq(String s) {
        int n = s.length();
        int[][] dp = new int[n][n];
        for (int i = n - 1; i >= 0; i--) {
            dp[i][i] = 1;
            for (int j = i + 1; j < n; j++) {
                if (s.charAt(i) == s.charAt(j))
                    dp[i][j] = 2 + dp[i+1][j-1];
                else
                    dp[i][j] = Math.max(dp[i+1][j], dp[i][j-1]);
            }
        }
        return dp[0][n-1];
    }
}
```

**Alternative (LCS).** `return lcs(s, new StringBuilder(s).reverse().toString());`

**Complexity.** Time O(n^2). Space O(n^2) / O(n) rolling.

**Common Mistakes.**
- Wrong iteration order — must ensure `dp[i+1][j-1]` is computed before `dp[i][j]`.
- Forgetting the `i == j` base case.
- Using `dp[i+1][j-1]` when `i+1 > j-1` (out of order) — handle the gap-1 case separately.

---

### 11.14 Palindrome Partitioning II

**Problem Statement.** Given a string `s`, return the minimum number of cuts needed to partition `s` into palindromic substrings.

**Recognition Clues.** Single string; "minimum cuts"; "palindrome partition" → Palindrome Partitioning II.

**Pattern.** Interval DP + 1D DP combination.

**Difficulty.** Hard.

**Companies.** Google, Amazon, Bloomberg, Apple, Microsoft.

**Constraints.** Length up to 2000.

**Observations.**
- Two-step approach:
  - Precompute `isPal[i][j]` = true if `s[i..j]` is palindrome. O(n^2).
  - Compute `dp[i]` = minimum cuts for `s[0..i]`. O(n^2).
- `isPal[i][j] = (s[i] == s[j]) and (j - i < 2 or isPal[i+1][j-1])`.

**State Design.**
- `isPal[i][j]` — boolean.
- `dp[i]` — minimum cuts for prefix `s[0..i]`.

**Transition.**
- `dp[i] = min over j in [0..i] of (dp[j-1] + 1 if isPal[j][i] else INF)`, with `dp[-1] = -1`.

**Java Solution.**
```java
class Solution {
    public int minCut(String s) {
        int n = s.length();
        boolean[][] isPal = new boolean[n][n];
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i; j < n; j++) {
                if (s.charAt(i) == s.charAt(j) && (j - i < 2 || isPal[i+1][j-1]))
                    isPal[i][j] = true;
            }
        }
        int[] dp = new int[n];
        for (int i = 0; i < n; i++) dp[i] = i;  // worst case: cut between every char
        for (int i = 0; i < n; i++) {
            if (isPal[0][i]) { dp[i] = 0; continue; }
            for (int j = 1; j <= i; j++) {
                if (isPal[j][i]) dp[i] = Math.min(dp[i], dp[j-1] + 1);
            }
        }
        return dp[n-1];
    }
}
```

**Complexity.** Time O(n^2). Space O(n^2) for `isPal`, O(n) for `dp`.

**Common Mistakes.**
- Forgetting that `dp[-1] = -1` (no cut needed for full palindrome prefix).
- Computing `isPal` in the wrong order (must iterate `i` from end so `isPal[i+1][j-1]` is ready).

---

### 11.15 Matrix Chain Multiplication

**Problem Statement.** Given dimensions `p[0..n]` representing `n` matrices where matrix `i` has dimensions `p[i-1] × p[i]`, find the minimum number of scalar multiplications to multiply all matrices.

**Recognition Clues.** "Cost of multiplying matrices"; "minimum" → Matrix Chain Multiplication.

**Pattern.** Interval DP, split at k.

**Difficulty.** Hard.

**Companies.** Google, Microsoft, Amazon (rare in interviews, common in academia).

**Constraints.** `n ≤ 100`.

**Observations.**
- Multiplying a `p × q` matrix by a `q × r` matrix costs `pqr` and produces a `p × r` matrix.
- The optimal order splits the chain at some point `k`, recursively solving left and right.

**State Design.** `dp[i][j]` = minimum multiplications to multiply matrices `i..j`.

**Transition.** `dp[i][j] = min over k in [i..j-1] of (dp[i][k] + dp[k+1][j] + p[i-1]*p[k]*p[j])`.

**Base case.** `dp[i][i] = 0` (single matrix, no multiplication).

**Iteration Order.** By interval length, increasing.

**Java Solution.**
```java
class Solution {
    public int matrixChainOrder(int[] p) {
        int n = p.length - 1;
        int[][] dp = new int[n][n];
        for (int len = 2; len <= n; len++) {
            for (int i = 0; i + len - 1 < n; i++) {
                int j = i + len - 1;
                dp[i][j] = Integer.MAX_VALUE;
                for (int k = i; k < j; k++) {
                    int cost = dp[i][k] + dp[k+1][j] + p[i] * p[k+1] * p[j+1];
                    dp[i][j] = Math.min(dp[i][j], cost);
                }
            }
        }
        return dp[0][n-1];
    }
}
```

**Complexity.** Time O(n^3). Space O(n^2).

**Common Mistakes.**
- Index confusion with `p[i]`, `p[k+1]`, `p[j+1]` (the dimensions array is offset by 1 from the matrix indices).
- Forgetting the `k` loop range `i to j-1`.
- Not initializing `dp[i][j]` to infinity before the `k` loop.

---

### 11.16 Burst Balloons

**Problem Statement.** Given `n` balloons with values `nums[i]`, bursting balloon `i` gives `nums[left] * nums[i] * nums[right]` where `left` and `right` are the un-burst neighbors. Find the maximum coins collectible by bursting all balloons in some order.

**Recognition Clues.** "Burst in any order"; "maximum coins" → Burst Balloons (reverse thinking).

**Pattern.** Interval DP with reverse-order thinking.

**Difficulty.** Hard.

**Companies.** Google, Amazon, Meta, Microsoft.

**Constraints.** `n ≤ 500`.

**Observations.**
- Direct simulation is factorial-time.
- Key insight: think of which balloon to burst **last** in the interval `[left, right]`. When `k` bursts last, its neighbors are `left` and `right` (everything else in the interval is already gone).
- Add sentinel values `nums[-1] = nums[n] = 1` to handle boundaries.

**State Design.** `dp[left][right]` = max coins from bursting all balloons in the open interval `(left, right)`, where `left` and `right` themselves are not burst.

**Transition.** `dp[left][right] = max over k in (left, right) of (dp[left][k] + dp[k][right] + nums[left]*nums[k]*nums[right])`.

**Base case.** `dp[left][right] = 0` if `left + 1 >= right` (no balloons in between).

**Iteration Order.** By interval length, increasing.

**Java Solution.**
```java
class Solution {
    public int maxCoins(int[] nums) {
        int n = nums.length;
        int[] arr = new int[n+2];
        arr[0] = arr[n+1] = 1;
        for (int i = 0; i < n; i++) arr[i+1] = nums[i];
        int[][] dp = new int[n+2][n+2];
        for (int len = 2; len <= n+1; len++) {
            for (int left = 0; left + len <= n+1; left++) {
                int right = left + len;
                for (int k = left + 1; k < right; k++) {
                    int coins = arr[left] * arr[k] * arr[right] + dp[left][k] + dp[k][right];
                    dp[left][right] = Math.max(dp[left][right], coins);
                }
            }
        }
        return dp[0][n+1];
    }
}
```

**Complexity.** Time O(n^3). Space O(n^2).

**Common Mistakes.**
- Trying to think "which balloon to burst first" — subproblems become non-independent.
- Boundary sentinel values forgotten.
- Wrong interval type (open vs closed).

---

### 11.17 Boolean Parenthesization

**Problem Statement.** Given a boolean expression with symbols (T/F) and operators (&, |, ^), count the number of ways to parenthesize it to evaluate to true.

**Recognition Clues.** Boolean expression; "number of ways"; "parenthesize" → Boolean Parenthesization.

**Pattern.** Interval DP, two-value state.

**Difficulty.** Hard.

**Companies.** Google, Microsoft, Amazon (more common in India-based interviews).

**Constraints.** Length up to 100-200.

**Observations.**
- For each interval, track both the number of ways to be true AND to be false (you need both to combine).
- Split at each operator position.

**State Design.** `dpT[i][j]` = ways to parenthesize `s[i..j]` to true; `dpF[i][j]` = ways to be false.

**Transition.** For each operator at position `k` (between `i` and `j`):
- AND: T if both T; F otherwise.
- OR: T if any T; F if both F.
- XOR: T if different; F if same.
- Sum the valid combinations.

**Iteration Order.** By interval length, increasing; only over odd-length intervals (symbols at even, operators at odd).

**Java Solution (sketch).**
```java
class Solution {
    public int countWays(String s) {
        int n = s.length();
        int mod = 1000000007;
        long[][] dpT = new long[n][n];
        long[][] dpF = new long[n][n];
        for (int i = 0; i < n; i += 2) {
            dpT[i][i] = s.charAt(i) == 'T' ? 1 : 0;
            dpF[i][i] = s.charAt(i) == 'F' ? 1 : 0;
        }
        for (int len = 3; len <= n; len += 2) {
            for (int i = 0; i + len - 1 < n; i++) {
                int j = i + len - 1;
                for (int k = i + 1; k < j; k += 2) {
                    char op = s.charAt(k);
                    long lt = dpT[i][k-1], lf = dpF[i][k-1];
                    long rt = dpT[k+1][j], rf = dpF[k+1][j];
                    if (op == '&') {
                        dpT[i][j] = (dpT[i][j] + lt * rt) % mod;
                        dpF[i][j] = (dpF[i][j] + lt * rf + lf * rt + lf * rf) % mod;
                    } else if (op == '|') {
                        dpT[i][j] = (dpT[i][j] + lt * rt + lt * rf + lf * rt) % mod;
                        dpF[i][j] = (dpF[i][j] + lf * rf) % mod;
                    } else {  // '^'
                        dpT[i][j] = (dpT[i][j] + lt * rf + lf * rt) % mod;
                        dpF[i][j] = (dpF[i][j] + lt * rt + lf * rf) % mod;
                    }
                }
            }
        }
        return (int) dpT[0][n-1];
    }
}
```

**Complexity.** Time O(n^3). Space O(n^2).

**Common Mistakes.**
- Tracking only `true` count — you need `false` too for combinations.
- Modulo arithmetic forgotten.
- Wrong operator-symbol indices (must skip 2 because of symbol-operator-symbol pattern).

---

### 11.18 Egg Dropping

**Problem Statement.** Given `k` eggs and `n` floors, find the minimum number of moves to determine the highest floor from which an egg can be dropped without breaking.

**Recognition Clues.** Two parameters (eggs, floors); "minimum moves"; "worst case" → Egg Dropping.

**Pattern.** 2D DP, min-max (worst case over choices).

**Difficulty.** Hard.

**Companies.** Google, Microsoft, Amazon, Apple, Goldman Sachs.

**Constraints.** `k ≤ 100`, `n ≤ 10^4` (or higher with math optimization).

**Observations.**
- Drop from floor `f`. If egg breaks, you have `k-1` eggs and `f-1` floors below. If not, you have `k` eggs and `n-f` floors above.
- Worst case: `1 + max(solve(k-1, f-1), solve(k, n-f))`.
- Minimize over `f`.

**State Design.** `dp[k][n]` = minimum moves with `k` eggs and `n` floors.

**Transition.** `dp[k][n] = 1 + min over f in [1..n] of max(dp[k-1][f-1], dp[k][n-f])`.

**Optimization.** The function `dp[k-1][f-1]` is increasing in `f`, `dp[k][n-f]` is decreasing in `f`. They cross at one point — use binary search to find it. Time O(k * n * log n) → O(k * n) with monotonicity.

**Alternative formulation.** Define `dp[k][m]` = max floors solvable with `k` eggs and `m` moves. `dp[k][m] = dp[k-1][m-1] + dp[k][m-1] + 1`. Iterate until `dp[k][m] >= n`. Time O(k * sqrt(n)) effectively.

**Java Solution (binary search optimization).**
```java
class Solution {
    public int superEggDrop(int k, int n) {
        int[][] dp = new int[k+1][n+1];
        for (int i = 1; i <= k; i++) {
            for (int j = 1; j <= n; j++) {
                if (i == 1) dp[i][j] = j;
                else if (j == 1) dp[i][j] = 1;
                else {
                    int lo = 1, hi = j;
                    while (lo + 1 < hi) {
                        int mid = (lo + hi) / 2;
                        int broken = dp[i-1][mid-1];
                        int notBroken = dp[i][j-mid];
                        if (broken < notBroken) lo = mid;
                        else hi = mid;
                    }
                    int b1 = Math.max(dp[i-1][lo-1], dp[i][j-lo]);
                    int b2 = Math.max(dp[i-1][hi-1], dp[i][j-hi]);
                    dp[i][j] = 1 + Math.min(b1, b2);
                }
            }
        }
        return dp[k][n];
    }
}
```

**Complexity.** Time O(kn log n). Space O(kn).

**Common Mistakes.**
- Linear search over `f` (TLE for large n).
- Confusing `dp[k-1][f-1]` (breaks, fewer eggs, fewer floors) with `dp[k][n-f]` (no break, same eggs, fewer floors above).
- Not using binary search for the cross point.

---

### 11.19 Partition DP — Word Break Count

**Problem Statement.** Given a string `s` and a dictionary of words, count the number of ways to segment `s` into dictionary words.

**Recognition Clues.** String + dictionary; "number of ways to segment" → Partition DP.

**Pattern.** 1D DP, but can be extended to 2D for related problems.

**Difficulty.** Medium.

**Companies.** Google, Amazon, Meta, Microsoft, Apple.

**Constraints.** Length up to 300-1000.

**Observations.**
- For each split point `j` in `[0..i]`, if `s[j..i]` is in dictionary, add `dp[j-1]` to `dp[i]`.
- Equivalent to: 2D DP where one dimension is the start of the last word.

**State Design (1D).** `dp[i]` = number of ways to segment `s[0..i-1]`.

**Transition.** `dp[i] = sum over j in [0..i-1] of dp[j] if s[j..i-1] is in dictionary`.

**Java Solution.**
```java
class Solution {
    public int wordBreakCount(String s, Set<String> dict) {
        int n = s.length();
        int[] dp = new int[n+1];
        dp[0] = 1;
        for (int i = 1; i <= n; i++) {
            for (int j = 0; j < i; j++) {
                if (dict.contains(s.substring(j, i))) {
                    dp[i] += dp[j];
                }
            }
        }
        return dp[n];
    }
}
```

**Complexity.** Time O(n^2 * word_length). Space O(n).

**Common Mistakes.**
- Not initializing `dp[0] = 1` (one way to segment empty string).
- Substring bounds (substring is `[j, i)` in Java).

---

### 11.20 Interleaving String

**Problem Statement.** Given strings `s1`, `s2`, and `s3`, determine if `s3` is formed by interleaving `s1` and `s2` while preserving the relative order of characters in each.

**Recognition Clues.** Two source strings + target; "interleaving" → 2D DP.

**Pattern.** Sequence DP, two-source reachability.

**Difficulty.** Medium.

**Companies.** Google, Meta, Microsoft, Bloomberg.

**Constraints.** Lengths up to 100.

**Observations.**
- `s3[i+j]` must equal either `s1[i]` or `s2[j]`.
- Track which `(i, j)` pairs are reachable.

**State Design.** `dp[i][j]` = true if `s3[0..i+j-1]` is an interleaving of `s1[0..i-1]` and `s2[0..j-1]`.

**Transition.**
- `dp[i][j] = (dp[i-1][j] and s1[i-1] == s3[i+j-1]) or (dp[i][j-1] and s2[j-1] == s3[i+j-1])`.
- Base: `dp[0][0] = true`.

**Java Solution (1D).**
```java
class Solution {
    public boolean isInterleave(String s1, String s2, String s3) {
        int m = s1.length(), n = s2.length();
        if (m + n != s3.length()) return false;
        boolean[] dp = new boolean[n+1];
        dp[0] = true;
        for (int j = 1; j <= n; j++) dp[j] = dp[j-1] && s2.charAt(j-1) == s3.charAt(j-1);
        for (int i = 1; i <= m; i++) {
            dp[0] = dp[0] && s1.charAt(i-1) == s3.charAt(i-1);
            for (int j = 1; j <= n; j++) {
                dp[j] = (dp[j] && s1.charAt(i-1) == s3.charAt(i+j-1))
                     || (dp[j-1] && s2.charAt(j-1) == s3.charAt(i+j-1));
            }
        }
        return dp[n];
    }
}
```

**Complexity.** Time O(mn). Space O(min(m,n)).

**Common Mistakes.**
- Not checking length condition `m + n == len(s3)` upfront.
- Wrong index into `s3` (it's `i+j-1`, not `i` or `j`).
- Forgetting the boundary updates in 1D optimization.

---


## 12. DP Visualization Lab

This section provides ASCII visualizations for every algorithm in the atlas. Use these as mental models when you encounter similar problems.

### 12.1 Recursive Tree for Fibonacci-style 2D DP

Consider Edit Distance of "ab" and "ac". The recursion explores:

```
                         ED(2,2)
                        /        \
                  match? yes      (treat as mismatch for branching)
                  ED(1,1)         ED(1,2)        ED(2,1)
                  /    \         /     \         /     \
              ED(0,0) ED(0,1) ED(0,1) ED(1,1) ED(1,1) ED(2,0)
                        ...     ...    [cached]  [cached]
```

Without memoization, the tree is exponential. With memoization, each `(i,j)` pair is computed once.

### 12.2 Memo Table (Visualization)

For Edit Distance of "horse" and "ros", the memo table fills as the recursion unwinds:

```
Step 1:  ED(5,3) called. Branches to ED(4,2), ED(4,3), ED(5,2).
Step 2:  ED(4,2) branches to ED(3,1), ED(3,2), ED(4,1).
Step 3:  ED(3,1) branches to ED(2,0), ED(2,1), ED(3,0).
Step 4:  ED(2,0) returns 2 (delete all 2 chars of "ho").
         ED(2,1): branches, eventually returns 2.
         ED(3,0) returns 3.
         ED(3,1) = 1 + min(2, 2, 3) = 3.
... etc.
```

The memo table gradually fills until `ED(5,3)` is computed.

### 12.3 DP Matrix Evolution — LCS of "abcde" and "ace"

```
After row 0 (i=0):                After row 1 (i=1, char 'a'):
   ""  a  c  e                       ""  a  c  e
""  0  0  0  0                    ""  0  0  0  0
                                   a  0  1  1  1

After row 2 (i=2, char 'b'):     After row 3 (i=3, char 'c'):
   ""  a  c  e                       ""  a  c  e
""  0  0  0  0                    ""  0  0  0  0
a   0  1  1  1                    a   0  1  1  1
b   0  1  1  1                    b   0  1  1  1
                                   c   0  1  2  2

After row 4 (i=4, char 'd'):     After row 5 (i=5, char 'e'):
   ""  a  c  e                       ""  a  c  e
""  0  0  0  0                    ""  0  0  0  0
a   0  1  1  1                    a   0  1  1  1
b   0  1  1  1                    b   0  1  1  1
c   0  1  2  2                    c   0  1  2  2
d   0  1  2  2                    d   0  1  2  2
                                   e   0  1  2  3   <- answer
```

### 12.4 Dependency Arrows for LCS

```
         dp[i-1][j-1]
              \
               \
                v
              dp[i][j]
               ^
              /
         dp[i-1][j]   (above)
              ^
              |
              | (when match, use diagonal; when mismatch, use above or left)
         dp[i][j-1]   (left)
```

### 12.5 Filled Cells — Interval DP (MCM)

For matrices `p = [10, 30, 5, 60]` (3 matrices):

```
            j=0    j=1    j=2
i=0         0      1500   4500
i=1         -      0      9000
i=2         -      -      0

Filling order:
  Length 1: dp[0][0], dp[1][1], dp[2][2] = 0
  Length 2: dp[0][1] = dp[0][0] + dp[1][1] + 10*30*5 = 1500
            dp[1][2] = dp[1][1] + dp[2][2] + 30*5*60 = 9000
  Length 3: dp[0][2] = min(
              dp[0][0] + dp[1][2] + 10*30*60,    // = 0 + 9000 + 18000 = 27000
              dp[0][1] + dp[2][2] + 10*5*60      // = 1500 + 0 + 3000 = 4500
            ) = 4500
```

### 12.6 Memory Layout — 2D Matrix

```
Row-major in memory:
[dp[0][0], dp[0][1], dp[0][2], ..., dp[0][m-1],
 dp[1][0], dp[1][1], dp[1][2], ..., dp[1][m-1],
 ...
 dp[n-1][0], ..., dp[n-1][m-1]]

Cache-friendly access: row-major iteration (inner loop on j) is faster
than column-major iteration for large tables, due to spatial locality.
```

### 12.7 Space Optimization Visualization — LCS

Full table → two rows → one row:

```
Full table (5x4):
+---+---+---+---+
| 0 | 0 | 0 | 0 |  <- row 0
+---+---+---+---+
| 0 | 1 | 1 | 1 |  <- row 1
+---+---+---+---+
| 0 | 1 | 1 | 1 |  <- row 2
+---+---+---+---+
| 0 | 1 | 2 | 2 |  <- row 3
+---+---+---+---+
| 0 | 1 | 2 | 2 |  <- row 4
+---+---+---+---+
| 0 | 1 | 2 | 3 |  <- row 5
+---+---+---+---+

Two rows (prev and curr):
+---+---+---+---+    +---+---+---+---+
| 0 | 1 | 1 | 1 | -> | 0 | 1 | 1 | 1 |  (prev -> curr)
+---+---+---+---+    +---+---+---+---+

One row (rolling):
+---+---+---+---+
| 0 | 1 | 1 | 1 |   <- after processing row 1
+---+---+---+---+
Then update in place for row 2, etc.
Final: +---+---+---+---+
       | 0 | 1 | 2 | 3 |
       +---+---+---+---+
```

### 12.8 Falling Path DP — Three-Arrow Dependency

```
+---+---+---+---+
| . | . | . | . |   row i-1
+---+---+---+---+
|   | X |   |   |   X reads from (i-1, j-1), (i-1, j), (i-1, j+1)
+---+---+---+---+
   \  |  /
    \ | /
     \|/
+---+---+---+---+
|   | X |   |   |
+---+---+---+---+
```

### 12.9 Interval DP — Triangle Fill

```
For length L = 1:    fill diagonal dp[i][i]
For length L = 2:    fill dp[i][i+1] (uses dp[i][i] and dp[i+1][i+1])
For length L = 3:    fill dp[i][i+2] (uses dp[i][i+1], dp[i+1][i+2], dp[i][i], ...)
...

Visualization (n=5):
L=1:   X . . . .
L=2:   X X . . .
L=3:   X X X . .
L=4:   X X X X .
L=5:   X X X X X   <- answer at dp[0][n-1]

The triangle fills from the diagonal outward.
```

### 12.10 Reverse DP — Dungeon Game

```
Iteration order: bottom-to-top, right-to-left.

Start:                        End:
+---+---+---+                 +---+---+---+
| . | . | . |                 | 7 | 5 | 2 |
+---+---+---+                 +---+---+---+
| . | . | . |                 | 6 | 11| 5 |
+---+---+---+                 +---+---+---+
| . | . |END|                 | 1 | 1 | 6 |
+---+---+---+                 +---+---+---+

Dependencies flow upward and leftward (from END toward START).
Answer: dp[0][0] = 7.
```

### 12.11 Burst Balloons — Open Interval

```
Original array with sentinels: [1, 3, 1, 5, 8, 1]
                                 ^              ^
                                 left sentinel  right sentinel

dp[left][right] = max coins from bursting everything strictly between left and right.

To compute dp[0][5] (the answer), iterate k from 1 to 4:
  k=1: coins = arr[0]*arr[1]*arr[5] + dp[0][1] + dp[1][5]
  k=2: coins = arr[0]*arr[2]*arr[5] + dp[0][2] + dp[2][5]
  k=3: coins = arr[0]*arr[3]*arr[5] + dp[0][3] + dp[3][5]
  k=4: coins = arr[0]*arr[4]*arr[5] + dp[0][4] + dp[4][5]

Take max.
```

### 12.12 Cherry Pickup — Two Synchronized Paths

```
Two robots, both starting at (0,0), both ending at (n-1, n-1).
At time t, both have made t steps, so r1 + c1 = r2 + c2 = t.
State: (r1, c1, r2) with c2 = r1 + c1 - r2.

Time 0:  both at (0,0).
Time 1:  both at (0,1) or (1,0) -> 4 combinations.
Time 2:  each at one of 4 positions -> 16 combinations (some infeasible).
...
Time 2(n-1): both at (n-1, n-1).
```

---

## 13. Master Decision Tree

This is the global decision tree for choosing a 2D DP family.

```
                            [Problem]
                                |
                                v
                  Is it optimization/counting
                  on a sequence or grid?
                                |
              +-----------------+-----------------+
              |                                   |
             YES                                  NO
              |                                   |
              v                                   v
       How many state                Consider: greedy, BFS,
       variables vary?               Dijkstra, divide-conquer,
              |                       backtracking, math.
              v
         1 or 2 or 3+
              |
              v
       Is the input a grid?
              |
       +-----+-----+
       |           |
      YES          NO
       |           |
       v           v
   Moves limited  Is it two strings?
   to right/down  |
   or similar?    +-----+-----+
       |          |           |
      YES         YES          NO
       |          |           |
       v          v           v
   GRID DP    SEQUENCE DP   Is it a single string
   (Section   (Section      with range/substring?
   11.1-11.7) 11.8-11.12,   |
              11.20)        +-----+-----+
                            |           |
                           YES          NO
                            |           |
                            v           v
                        Is palindrome, Is it "split array
                        substring,     into parts"?
                        substring       |
                        partitioning?   +-----+-----+
                            |           |           |
                       +----+----+     YES          NO
                       |         |      |           |
                      YES       NO     v           v
                       |         |   PARTITION DP  Is there a
                       v         v   (Section      resource limit
                   INTERVAL DP   ?   11.19)        (k transactions,
                   (Section                        k eggs, etc.)?
                   11.13-11.18)                    |
                                              +----+----+
                                              |         |
                                             YES        NO
                                              |         |
                                              v         v
                                         2D DP     BITMASK DP / GRAPH DP
                                         (index,   / TREE DP / DIGIT DP
                                         resource)
```

### 13.1 Detailed Branches

**If grid with restricted moves → Grid DP.** Confirm direction (forward for "minimum to reach end", reverse for "minimum needed to survive"). Confirm obstacle handling. Confirm space optimization (almost always applicable).

**If two strings → Sequence DP.** Determine the comparison: longest common, edit distance, distinct subsequences, interleaving, regex match. All have similar `(i, j)` state but different transitions.

**If single string with substring/range → Interval DP.** Use the gap technique. Identify whether the interval closes (palindrome) or splits (MCM, Burst Balloons).

**If splitting into parts → Partition DP.** Identify the resource being partitioned (k parts, k transactions, k eggs). Add a dimension for the resource count.

**If extreme space constraints → Bitmask or digit DP.** Used when n is small (≤ 20) for bitmask, or when counting numbers in a range with digit constraints.

**If tree-structured input → Tree DP.** Different paradigm; the "2D" comes from (node, state) where state is typically "selected / not selected."

---

## 14. Complexity Masterclass

### 14.1 The Fundamental Law of DP Complexity

> DP time = (number of states) × (work per state).
>
> DP space = number of states (or less, after optimization).

For 2D DP, the number of states is `dim1 × dim2`. The work per state is usually O(1) (simple transition) or O(n) (transition with a loop, e.g., interval DP with a `k` split).

### 14.2 Why Dimensions Multiply

A 2D state `(i, j)` with `i ∈ [0, n)` and `j ∈ [0, m)` has `n × m` cells because every combination of `i` and `j` is a distinct state. They are independent dimensions — `i` and `j` vary freely within their bounds.

This is the critical insight that distinguishes 2D DP from 1D DP. A 1D DP with `n` states has `n` cells. A 2D DP with two dimensions of size `n` and `m` has `n × m` cells. If you accidentally have three dimensions of size `n`, `m`, `k`, you have `n × m × k` cells.

**Example:**
- LCS of strings of length 1000: `1000 × 1000 = 10^6` cells. Each cell does O(1) work. Total: 10^6 operations. Fast.
- Edit Distance on strings of length 10^4: `10^4 × 10^4 = 10^8` cells. Each cell O(1). Total: 10^8 — borderline, might TLE in 1 second.
- Matrix Chain with 1000 matrices: `1000 × 1000 = 10^6` cells, but each cell does O(n) = 1000 work for the `k` split. Total: 10^9 — too slow, need to optimize.

### 14.3 Common Time Complexities in 2D DP

| Pattern | States | Work per state | Total time |
|---|---|---|---|
| Grid DP (path sum) | O(mn) | O(1) | O(mn) |
| Sequence DP (LCS) | O(mn) | O(1) | O(mn) |
| Edit Distance | O(mn) | O(1) | O(mn) |
| Interval DP (MCM) | O(n^2) | O(n) (k split) | O(n^3) |
| Burst Balloons | O(n^2) | O(n) (k split) | O(n^3) |
| Boolean Parenthesization | O(n^2) | O(n) | O(n^3) |
| Egg Dropping (naive) | O(kn) | O(n) | O(kn^2) |
| Egg Dropping (binary search) | O(kn) | O(log n) | O(kn log n) |
| Bitmask DP (TSP) | O(n × 2^n) | O(n) | O(n^2 × 2^n) |

### 14.4 Space Complexity

| Pattern | Naive space | Optimized space |
|---|---|---|
| Grid DP | O(mn) | O(n) or O(1) in-place |
| Sequence DP | O(mn) | O(min(m,n)) |
| Edit Distance | O(mn) | O(min(m,n)) |
| Interval DP | O(n^2) | O(n^2) (often no compression) |
| Egg Dropping | O(kn) | O(n) (rolling) |
| Bitmask DP | O(n × 2^n) | O(2^n) (rolling) |

### 14.5 Best, Average, Worst Case

DP complexities are typically **worst-case** bounds because the algorithm fills the entire table regardless of input. There is usually no "average case" improvement. However:
- **Best case:** Sometimes early termination is possible (e.g., if a state's answer is "obviously" 0, skip). Rare in 2D DP.
- **Average case:** Same as worst case for most 2D DPs.
- **Worst case:** The standard quoted complexity.

### 14.6 Reasoning Rather Than Formulas

Don't memorize "LCS is O(mn)." Instead, reason: "LCS has `(m+1) × (n+1)` states. Each state's transition is O(1) — just a comparison and a max. So total work is `(m+1) × (n+1) × O(1) = O(mn)`."

This reasoning generalizes. When you encounter a new problem:
1. Count the states: multiply all dimension sizes.
2. Estimate the work per state: count operations in the transition.
3. Multiply.

If the result is more than ~10^8, expect TLE in 1 second and look for optimizations.

### 14.7 Optimization Strategies

When your DP is too slow:

1. **Drop a dimension.** Re-examine the state; can a dimension be derived from others?
2. **Binary search the inner loop.** Common in egg dropping and similar min-max problems where the transition is monotonic.
3. **Use prefix sums.** If the transition sums a range, precompute prefix sums to make it O(1).
4. **Convex hull trick / divide-and-conquer optimization.** Advanced techniques for specific recurrence shapes.
5. **Knuth's optimization.** For certain interval DP problems where the optimal split point is monotonic.
6. **Matrix exponentiation.** For linear recurrences with huge n.
7. **Bitset optimization.** For subset-sum-like problems; pack 64 bits per word.

### 14.8 The 10^8 Rule

In competitive programming and interviews:
- 10^6 operations: trivial (milliseconds).
- 10^7 operations: comfortable (< 100 ms).
- 10^8 operations: borderline (about 1 second in Java/C++).
- 10^9 operations: too slow for 1-second limits.

If your DP complexity exceeds 10^8, look for an optimization or a different algorithm. The 10^8 threshold is a useful heuristic for time budgeting.

---


## 15. Common Mistakes

This section catalogs **50+ common mistakes** in 2D DP. Each mistake is presented as: **Wrong → Why Wrong → Correct**.

### 15.1 State Design Mistakes

**Mistake 1. Missing a needed dimension.**
- Wrong: Solving LCS with `dp[i]`.
- Why: Cannot distinguish configurations with the same `i` but different `j`.
- Correct: Use `dp[i][j]`.

**Mistake 2. Adding a redundant dimension.**
- Wrong: Solving min path sum with `dp[i][j][i+j]`.
- Why: `i+j` is determined by `(i,j)`. Wastes memory.
- Correct: Use `dp[i][j]`.

**Mistake 3. Treating the value as a dimension.**
- Wrong: `dp[i][j][current_lcs_length]`.
- Why: The LCS length is the value, not state.
- Correct: `dp[i][j]` = LCS length.

**Mistake 4. Wrong dimension meaning.**
- Wrong: `dp[i][j]` where `i` is sometimes "characters in A" and sometimes "characters in B."
- Why: Inconsistent semantics cause bugs.
- Correct: Pick a convention and stick to it (e.g., `i` always = first string).

**Mistake 5. Including static input as a dimension.**
- Wrong: `dp[string][i][j]` when there are only two fixed strings.
- Why: Wastes memory; the string identity is not a state variable.
- Correct: Use two separate 2D tables or just `dp[i][j]` for the relevant pair.

**Mistake 6. State does not capture previous choice.**
- Wrong: Solving "paint houses no two adjacent same" with `dp[i]`.
- Why: Cannot enforce adjacency without knowing the previous color.
- Correct: `dp[i][color]`.

**Mistake 7. State captures too much previous info.**
- Wrong: `dp[i][entire previous sequence]`.
- Why: Exponential state space.
- Correct: Distill to `dp[i][last element]` or `dp[i][last few elements]`.

### 15.2 Transition Mistakes

**Mistake 8. Missing a choice.**
- Wrong: In House Robber, only considering "rob current."
- Why: Skipping "don't rob current" loses valid solutions.
- Correct: `dp[i] = max(dp[i-1], dp[i-2] + nums[i])`.

**Mistake 9. Double counting.**
- Wrong: In counting DP, two choices leading to the same state both counted.
- Why: Inflated count.
- Correct: Ensure choices are mutually exclusive and exhaustive.

**Mistake 10. Forgetting immediate cost.**
- Wrong: `dp[i][j] = min(dp[i-1][j], dp[i][j-1])` for min path sum.
- Why: Missing `grid[i][j]` addition.
- Correct: `dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1])`.

**Mistake 11. Wrong direction of recurrence.**
- Wrong: `dp[i][j]` depends on `dp[i+1][j+1]` with forward iteration.
- Why: Future state referenced before computed.
- Correct: Either change recurrence to depend on past, or iterate backward.

**Mistake 12. Using uninitialized cells.**
- Wrong: Reference `dp[i-1][j-1]` when `i=0` or `j=0`.
- Why: Out of bounds or garbage value.
- Correct: Add boundary checks or pad the table.

**Mistake 13. Wrong base case value.**
- Wrong: `dp[0][0] = 0` for counting paths.
- Why: There is 1 path from start to itself (do nothing).
- Correct: `dp[0][0] = 1`.

**Mistake 14. Off-by-one in index translation.**
- Wrong: Comparing `s.charAt(i)` instead of `s.charAt(i-1)` in `dp[i][j]`.
- Why: `dp[i]` represents first `i` chars, so the relevant char is at index `i-1`.
- Correct: Always use `i-1` and `j-1` when comparing characters.

**Mistake 15. Modulo arithmetic forgotten.**
- Wrong: Counting DP without mod when numbers can be huge.
- Why: Integer overflow.
- Correct: Apply `mod` after each addition.

### 15.3 Iteration Order Mistakes

**Mistake 16. Bottom-up iteration for top-down dependencies.**
- Wrong: Iterating `i` from `n-1` down to `0` when `dp[i][j]` depends on `dp[i-1][j]`.
- Why: `dp[i-1][j]` is uninitialized when needed.
- Correct: Iterate `i` from `0` to `n-1`.

**Mistake 17. Wrong inner-loop direction in 0/1 knapsack.**
- Wrong: Forward iteration after compression.
- Why: Item can be used multiple times.
- Correct: Reverse iteration.

**Mistake 18. Wrong inner-loop direction in unbounded knapsack.**
- Wrong: Reverse iteration after compression.
- Why: Item only used once.
- Correct: Forward iteration.

**Mistake 19. Interval DP without gap ordering.**
- Wrong: `for i ... for j ...` without ensuring length increasing.
- Why: Subintervals not computed.
- Correct: Outer loop on length, inner loop on start.

**Mistake 20. Wrong diagonal order for palindrome.**
- Wrong: Iterating `i, j` in row-major when `dp[i][j]` depends on `dp[i+1][j-1]`.
- Why: `(i+1, j-1)` not yet computed.
- Correct: Iterate by gap `j - i` increasing.

**Mistake 21. Iterating cells that should be skipped.**
- Wrong: Computing `dp[i][i]` inside the main loop without base case.
- Why: `dp[i+1][i-1]` may be referenced (out of range).
- Correct: Set `dp[i][i] = 1` first, then iterate `gap >= 1`.

### 15.4 Initialization Mistakes

**Mistake 22. Forgetting to initialize the base row/column.**
- Wrong: Only filling `dp[0][0]` and expecting the rest to propagate.
- Why: First row and first column often have special transitions.
- Correct: Explicitly fill `dp[0][*]` and `dp[*][0]` before the main loop.

**Mistake 23. Wrong initialization of base row.**
- Wrong: `dp[0][j] = 0` for edit distance.
- Why: Converting empty string to `B[0..j-1]` requires `j` insertions.
- Correct: `dp[0][j] = j`.

**Mistake 24. Not initializing unreachable cells.**
- Wrong: Leaving `dp[i][j] = 0` for impossible states.
- Why: 0 may be a valid answer (e.g., for counting); impossible should be `-infinity` or a sentinel.
- Correct: Use `Integer.MIN_VALUE` or `-1` to mark unreachable, and check before use.

**Mistake 25. Initialization order wrong for 1D rolling.**
- Wrong: Updating `dp[0]` after the inner loop instead of before.
- Why: First column depends on the previous row's first column.
- Correct: Update `dp[0]` at the start of each outer iteration.

### 15.5 Boundary / Off-by-One Mistakes

**Mistake 26. Confusing `n` and `n+1` for table size.**
- Wrong: `int[][] dp = new int[n][m];` when state goes up to `dp[n][m]`.
- Why: Index out of bounds.
- Correct: `int[][] dp = new int[n+1][m+1];`.

**Mistake 27. Loop range excluding the last cell.**
- Wrong: `for (int i = 0; i < n; i++)` when the answer is at `dp[n][m]`.
- Why: Missing the final row/column.
- Correct: `for (int i = 0; i <= n; i++)`.

**Mistake 28. Wrong return cell.**
- Wrong: Returning `dp[n-1][m-1]` when the state is "first `i` chars" (so answer is `dp[n][m]`).
- Why: Off-by-one.
- Correct: Match the return cell to the state definition.

**Mistake 29. Wrong substring indices.**
- Wrong: `s.substring(i, j)` interpreted as `[i..j]` instead of `[i..j)`.
- Why: Java's `substring` is half-open.
- Correct: `s.substring(i, j+1)` for inclusive `[i..j]`.

**Mistake 30. Wrong matrix dimension index in MCM.**
- Wrong: Using `p[i] * p[k] * p[j]` for the merge cost.
- Why: The merge cost is `p[i-1] * p[k] * p[j]` (or equivalent, depending on convention).
- Correct: Carefully trace which dimension is shared.

### 15.6 Space Optimization Mistakes

**Mistake 31. Overwriting a value before it's used.**
- Wrong: `dp[j] = dp[j] + dp[j-1]` when `dp[j-1]` was already updated this iteration.
- Why: Uses the new (current row) value instead of old (previous row).
- Correct: Save the diagonal in a temp variable.

**Mistake 32. Forgetting to update `dp[0]` per row.**
- Wrong: Leaving `dp[0]` stale from initialization.
- Why: First column often has a row-dependent value.
- Correct: Update `dp[0]` at the start of each row.

**Mistake 33. Wrong rolling direction.**
- Wrong: Compressing `dp[i][j]` that depends on `dp[i+1][j+1]` (forward) to a single row.
- Why: The dependency is on the *next* row, which is not yet computed.
- Correct: Use two-row rolling or reverse the iteration.

**Mistake 34. In-place update destroys needed input.**
- Wrong: Mutating the input grid when later steps need original values.
- Why: Wrong answers or crash.
- Correct: Use a separate table, or carefully order updates so destroyed values are not needed.

**Mistake 35. Path reconstruction with compressed space.**
- Wrong: Reconstructing the path from a 1D compressed table.
- Why: Backtracking needs the full 2D history.
- Correct: Keep the full 2D table for reconstruction, or use Hirschberg's divide-and-conquer.

### 15.7 Conceptual Mistakes

**Mistake 36. Greedy where DP is needed.**
- Wrong: Greedily picking the smallest cell at each step for min path sum.
- Why: Local minima don't lead to global minimum.
- Correct: Use DP.

**Mistake 37. DP where greedy works.**
- Wrong: DP for activity selection.
- Why: Greedy is correct and faster.
- Correct: Greedy by earliest finish time.

**Mistake 38. DP where math works.**
- Wrong: DP for Fibonacci of n = 10^18.
- Why: O(n) DP is too slow.
- Correct: Matrix exponentiation, O(log n).

**Mistake 39. DP without overlapping subproblems.**
- Wrong: DP for merge sort.
- Why: Subproblems don't overlap; divide-and-conquer is sufficient.
- Correct: Use divide-and-conquer.

**Mistake 40. Misidentifying the state count.**
- Wrong: Calling LCS "1D DP" because it's about one sequence.
- Why: There are two sequences, hence two indices.
- Correct: 2D DP.

### 15.8 Code-Level Mistakes

**Mistake 41. Using `int` when `long` is needed.**
- Wrong: `int dp[][]` for counting problems with large outputs.
- Why: Overflow.
- Correct: Use `long` or `BigInteger`.

**Mistake 42. Modulo applied at the wrong time.**
- Wrong: Applying mod after a sum of multiple terms without intermediate mods.
- Why: Intermediate overflow.
- Correct: Apply mod after each addition.

**Mistake 43. Wrong comparator in `min`/`max`.**
- Wrong: `Math.min(a, b)` when one of them is `Integer.MIN_VALUE` sentinel.
- Why: The sentinel wins.
- Correct: Check for sentinel before comparing.

**Mistake 44. Forgetting to handle empty input.**
- Wrong: Assuming `n >= 1` without checking.
- Why: Crash on empty input.
- Correct: Early return for empty input.

**Mistake 45. Off-by-one in interval bounds.**
- Wrong: `for (int k = i; k <= j; k++)` when `k` should range over `(i, j)` (exclusive).
- Why: Includes boundary sentinels.
- Correct: `for (int k = i + 1; k < j; k++)`.

### 15.9 Communication Mistakes (In Interviews)

**Mistake 46. Coding before explaining the state.**
- Wrong: Jumping straight to code.
- Why: Interviewer cannot follow; bugs go unnoticed.
- Correct: Explain state, transition, base case, iteration order on the whiteboard first.

**Mistake 47. Not dry-running on an example.**
- Wrong: Submitting code without tracing through a small example.
- Why: Bugs slip through.
- Correct: Trace one small example end-to-end before saying "done."

**Mistake 48. Not discussing complexity.**
- Wrong: Saying "it's polynomial" without specifying.
- Why: Vague; interviewer wants exact O(...).
- Correct: State time and space complexity explicitly.

**Mistake 49. Not mentioning optimizations.**
- Wrong: Stopping at the O(mn) space solution.
- Why: Misses the chance to show optimization skill.
- Correct: Mention the O(min(m,n)) space optimization even if you don't code it.

**Mistake 50. Not handling edge cases.**
- Wrong: Forgetting empty strings, single-character strings, identical strings.
- Why: Test failures.
- Correct: List edge cases before coding; handle them explicitly.

**Mistake 51. Mispronouncing or misnaming the DP family.**
- Wrong: Calling LCS "interval DP."
- Why: Confuses the interviewer.
- Correct: Use correct terminology (sequence DP for LCS).

**Mistake 52. Not relating to similar problems.**
- Wrong: Solving each problem in isolation.
- Why: Misses the chance to show pattern recognition.
- Correct: Mention similar problems (e.g., "this is like Edit Distance but...").

**Mistake 53. Copy-pasting code without understanding.**
- Wrong: Reciting a memorized solution.
- Why: Interviewer will probe; you cannot adapt.
- Correct: Derive the solution live.

---

## 16. Edge Cases

This section catalogs **50+ edge cases** to consider when implementing 2D DP. Always test your code against these.

### 16.1 Empty / Single-Element Inputs

1. **Empty grid** (0 rows or 0 columns). Return 0 or appropriate default.
2. **Single-cell grid** (1x1). Answer is the cell value itself.
3. **Single row** (1xN). Linear, no vertical moves possible.
4. **Single column** (Nx1). Linear, no horizontal moves possible.
5. **Empty string** (one or both). LCS = 0, edit distance = length of non-empty.
6. **Single-character strings.** Trivial cases; useful for base case verification.
7. **Identical strings.** LCS = length, edit distance = 0.
8. **Disjoint strings (no common chars).** LCS = 0.

### 16.2 Boundary Conditions

9. **Start cell is obstacle.** Return 0 immediately (no paths).
10. **End cell is obstacle.** Return 0 immediately.
11. **Start and end are the same cell.** Path length 0; count = 1.
12. **All cells are obstacles.** Return 0.
13. **First row entirely obstacles after some column.** All subsequent first-row cells are 0.
14. **First column entirely obstacles after some row.** All subsequent first-column cells are 0.
15. **Only one valid path.** Verify the algorithm finds it.

### 16.3 Value Extremes

16. **All zeros.** Trivial; useful for sanity.
17. **All negative values.** Path sum could be very negative; ensure correct sign handling.
18. **Mixed positive and negative.** Especially tricky in Dungeon Game (HP must stay positive).
19. **Single huge value.** Ensure no overflow.
20. **All same values.** Symmetric; useful for verification.
21. **Maximum integer values.** Test overflow behavior.
22. **Minimum integer values.** Test sentinel handling.

### 16.4 Dimensional Extremes

23. **Very large grid (10^4 × 10^4).** Tests space optimization; full table is 4 × 10^8 = 400 MB.
24. **Very long string (10^5).** Tests 1D optimization.
25. **Two strings of very different lengths.** Tests asymmetry.
26. **n = 1 for interval DP.** Trivial; just the single element.
27. **n = 2 for interval DP.** Smallest non-trivial case.

### 16.5 Duplicate / Symmetric Inputs

28. **Duplicate characters in strings.** LCS still works; verify.
29. **Palindrome input.** LPS = full length.
30. **Already-palindrome input for partitioning.** 0 cuts needed.
31. **Reverse input.** Some DPs are symmetric, others are not.
32. **Two identical strings.** Various special cases.

### 16.6 Impossible / No-Solution Cases

33. **No path exists** (obstacles block). Return 0 or `Integer.MAX_VALUE` as appropriate.
34. **Target string cannot be formed.** Distinct subsequences returns 0.
35. **Dictionary has no useful words.** Word break returns 0.
36. **k = 0 transactions.** Profit is 0.
37. **k >= n/2 transactions.** Effectively unlimited; switch to greedy.

### 16.7 Multiple Optimal Answers

38. **Multiple LCS of the same length.** Algorithm returns length only; reconstruction may give any.
39. **Multiple paths with the same sum.** Algorithm returns sum; path may differ.
40. **Multiple ways to parenthesize to true.** Count must include all.

### 16.8 Numerical / Modular

41. **Count exceeds `Integer.MAX_VALUE`.** Use `long` or apply mod.
42. **Count exceeds `Long.MAX_VALUE`.** Use `BigInteger` or apply mod.
43. **Negative values in subset sum.** Offset by sum of negatives; standard trick.
44. **Zero in the input.** Affects subset sum (don't divide by zero, handle zero items).
45. **Modulo equals 1.** All answers become 0.

### 16.9 Iteration / Index Edge Cases

46. **n = 1 in interval DP.** Loop bounds must handle.
47. **n = 2 in interval DP.** Smallest split case.
48. **k = 1 split point.** Verify the `k` loop runs.
49. **Single operator in boolean parenthesization.** Trivial.
50. **Operator at the boundary.** Verify index handling.

### 16.10 Type / Format Edge Cases

51. **Input is `null`.** Throw or return default.
52. **Input contains uppercase and lowercase.** Verify case sensitivity.
53. **Input has whitespace.** Verify trim behavior.
54. **Input has Unicode characters.** Verify `char` handling (Java `char` is 16-bit; supplementary characters need `codePoint`).
55. **Input is a List, not an array.** Adapt indexing.

### 16.11 Edge Case Strategy

When implementing, write a checklist:
- [ ] Empty input
- [ ] Single-element input
- [ ] All-same input
- [ ] All-zero input
- [ ] Already-optimal input
- [ ] Impossible input
- [ ] Maximum size input
- [ ] Overflow-prone input

Run your solution against each. This catches 90% of edge case bugs.

---


## 17. Interview Thinking Process

This section teaches the **step-by-step process** to use during an interview. Follow it religiously.

### 17.1 The 10-Step Process

```
Step 1: OBSERVATION
   - Read the problem twice.
   - Identify input types (grid, strings, array, tree).
   - Identify output type (number, count, string, boolean).
   - Note constraints (sizes, value ranges).

Step 2: BRUTE FORCE
   - Verbally describe the brute-force solution.
   - Estimate its complexity.
   - Confirm with the interviewer that it's too slow.

Step 3: RECURSIVE STATE
   - Write a recursive function signature.
   - Identify which arguments vary between recursive calls.
   - These are candidate state variables.

Step 4: 2D STATE
   - Decide on the final state (e.g., dp[i][j]).
   - Write the state definition on the whiteboard:
     "dp[i][j] = <meaning>"
   - Confirm with the interviewer.

Step 5: TRANSITION
   - Enumerate choices at a generic state.
   - For each choice, identify the next state and immediate cost.
   - Write the recurrence on the whiteboard.

Step 6: MEMOIZATION
   - Convert the recursion to memoized form.
   - Verify base cases.

Step 7: TABULATION
   - Convert to bottom-up.
   - Decide iteration order based on dependencies.
   - Initialize base row/column.

Step 8: OPTIMIZATION
   - Identify space optimization opportunities.
   - Discuss trade-offs (path reconstruction needs full table).

Step 9: CODE
   - Write clean, named code.
   - Use meaningful variable names.
   - Comment the state definition and recurrence.

Step 10: EXPLAIN TO INTERVIEWER
   - Walk through the code on a small example.
   - State time and space complexity.
   - Mention similar problems and follow-ups.
```

### 17.2 Worked Example: Interview Process for "Minimum Path Sum"

**Interviewer:** "Given a grid, find the minimum path sum from top-left to bottom-right, moving only right or down."

**Step 1 (Observation):** Input is a 2D grid of non-negative integers. Output is a single number (the minimum sum). Constraints: typical grid sizes up to 200x200. Movement restricted to right or down — this restriction is crucial because it makes the problem a DAG.

**Step 2 (Brute force):** Enumerate all paths; each path has length `(m-1) + (n-1)` steps, and there are `C(m+n-2, m-1)` paths. Exponential. Too slow.

**Step 3 (Recursive state):** Let `f(i, j)` = minimum path sum from `(0,0)` to `(i,j)`. The recursion is `f(i, j) = grid[i][j] + min(f(i-1, j), f(i, j-1))`, with base cases `f(0, 0) = grid[0][0]`, `f(0, j) = sum of grid[0][0..j]`, `f(i, 0) = sum of grid[0..i][0]`. The varying arguments are `i` and `j`.

**Step 4 (2D state):** `dp[i][j]` = minimum path sum from `(0,0)` to `(i,j)`. Confirmed.

**Step 5 (Transition):** As above. Choices at `(i, j)`: came from above `(i-1, j)` or from left `(i, j-1)`. Pick the smaller, add `grid[i][j]`.

**Step 6 (Memoization):** Top-down with cache. Time O(mn), space O(mn).

**Step 7 (Tabulation):** Bottom-up, row-major. Initialize first row and first column. Iterate `i` from 1 to m-1, `j` from 1 to n-1.

**Step 8 (Optimization):** Since `dp[i][j]` depends only on row `i-1`, we can compress to O(n) space by keeping a single rolling row. We can even do it in-place by mutating the grid.

**Step 9 (Code):** Write the in-place solution.

**Step 10 (Explain):** "The state `dp[i][j]` represents the minimum path sum to reach cell `(i,j)` from `(0,0)`. We iterate top-to-bottom, left-to-right, computing each cell as the cell value plus the minimum of the cell above and the cell to the left. The first row and first column are initialized by cumulative sums. Time complexity is O(mn) since each cell is computed once in O(1). Space is O(1) since we mutate the grid in place; if mutation is not allowed, O(n) with a rolling row. A similar problem is Unique Paths, which counts paths instead of minimizing sum."

This 10-step process works for any DP problem. Practice it until it's automatic.

### 17.3 Time Budgeting in a 45-Minute Interview

- **First 5 minutes:** Understand the problem, ask clarifying questions, identify DP applicability.
- **Next 10 minutes:** Steps 1-5 (observation, brute force, state, transition).
- **Next 15 minutes:** Steps 6-9 (memoization, tabulation, optimization, code).
- **Next 10 minutes:** Step 10 (explain, dry run, discuss follow-ups).
- **Last 5 minutes:** Q&A and similar problems.

If you spend more than 5 minutes on state design without progress, ask the interviewer for a hint. Most interviewers will give one.

### 17.4 Communication Tips

- **Think out loud.** Interviewers cannot read your mind; verbalize your reasoning.
- **Use the whiteboard.** Write state, transition, complexity on the board.
- **Acknowledge uncertainty.** "I'm not sure about the iteration order; let me trace through an example."
- **Don't hide bugs.** If you find a bug while explaining, fix it openly.
- **End with confidence.** Even if the solution isn't perfect, end with a clear statement of what you've done and what's left.

---

## 18. Pattern Comparison

This section compares the major 2D DP pattern families.

### 18.1 Comparison Table

| Family | Input Shape | State Shape | Iteration Order | Space Optimization | Typical Complexity |
|---|---|---|---|---|---|
| **Grid DP** | 2D grid | `dp[r][c]` | Row-major (or reverse for "survival") | Rolling row, often in-place | O(mn) time, O(n) space |
| **Sequence DP** | Two strings | `dp[i][j]` | Row-major over (i, j) | Rolling row + diagonal save | O(mn) time, O(min(m,n)) space |
| **Interval DP** | One sequence/array | `dp[l][r]` | By interval length (gap) | Usually no compression | O(n^2) or O(n^3) time, O(n^2) space |
| **Partition DP** | Sequence + resource count | `dp[i][k]` | By index, by resource | Rolling if resource fixed | O(nk) time, O(k) space |
| **Bitmask DP** | Small set + position | `dp[mask][i]` or `dp[mask]` | By mask size (or any topological order) | Rolling if position is determined | O(n × 2^n) time, O(2^n) space |
| **Digit DP** | A number range | `dp[pos][tight][other]` | By position from most significant | Rolling if no reconstruction | O(digits × state_size) |
| **Graph DP** | DAG | `dp[node]` or `dp[node][k]` | Topological order | Varies | O(V + E) or O(VE) |
| **Tree DP** | Tree | `dp[node][state]` | Post-order traversal | Recursive stack | O(V) typical |

### 18.2 When to Choose Each

**Grid DP** when:
- Input is a 2D grid or matrix.
- Movement is restricted (e.g., right/down).
- The answer is the value at one corner given the other corner.

**Sequence DP** when:
- Input is two sequences (strings, arrays).
- The answer compares them in some way.
- Two indices evolve independently.

**Interval DP** when:
- Input is a single sequence.
- The answer is for a range/subarray.
- Transitions split the range.

**Partition DP** when:
- Input is a sequence plus a resource count.
- The resource is consumed by choices.
- Examples: k transactions, k eggs, k partitions.

**Bitmask DP** when:
- n is small (≤ 20).
- The state is "which items have been used."
- TSP and assignment problems.

**Digit DP** when:
- Counting numbers in a range with digit constraints.
- "How many numbers between L and R have property P?"

**Graph DP** when:
- Input is a DAG (or can be topologically sorted).
- The answer is a longest/shortest path.

**Tree DP** when:
- Input is a tree.
- The state at a node depends on the states of its children.
- Examples: diameter, max independent set on trees.

### 18.3 Hybrid Problems

Many hard problems combine multiple families:
- **Cherry Pickup II:** Grid DP + 3D state (two synchronized paths).
- **Word Break II:** Partition DP + backtracking.
- **Russian Doll Envelopes:** Sort (greedy) + 1D LIS DP.
- **Concatenated Words:** DP + Trie.
- **Number of Music Playlists:** 2D DP + combinatorics.

For hybrids, identify the dominant pattern and treat the other as a sub-routine.

### 18.4 What Makes 2D DP "Hard"

The hardest 2D DP problems share these traits:
- **Non-obvious state.** The state has an unusual dimension (e.g., "current HP," "number of transactions used," "previous color").
- **Reverse thinking required.** Burst Balloons requires thinking "last to burst," not "first to burst."
- **Multi-pass or multi-dimensional.** Cherry Pickup needs two synchronized paths.
- **Reconstruction required.** Not just the value, but the actual solution (string, path, parenthesization).
- **Constraint coupling.** Constraints that link multiple variables (e.g., "no two adjacent same," "k total transactions across all buys and sells").

When you encounter a hard problem, identify which of these traits it has. Each trait has a standard mitigation:
- Non-obvious state → write brute force, see what varies.
- Reverse thinking → ask "what's the last operation?"
- Multi-pass → ask "can two agents solve this together?"
- Reconstruction → store predecessor or backtrack through dp.
- Constraint coupling → add a dimension for the constrained quantity.

---

## 19. Problem Progression

This section arranges 2D DP problems by difficulty. Each problem builds on previous concepts.

### 19.1 Very Easy (Warm-Up)

1. **Climbing Stairs** — 1D Fibonacci-like.
2. **Min Cost Climbing Stairs** — 1D DP with two-source transition.
3. **House Robber** — 1D take/skip.
4. **N-th Tribonacci Number** — 1D three-source.

These establish the basics; if you struggle here, review 1D DP first.

### 19.2 Easy (Introduction to 2D)

5. **Unique Paths** — Grid DP, counting.
6. **Minimum Path Sum** — Grid DP, optimization.
7. **Triangle Minimum Path** — Triangular grid DP.
8. **Longest Common Subsequence** — Sequence DP.
9. **Maximum Square Submatrix** — Grid DP with square constraint.

These introduce the four canonical 2D DP shapes. Master them before proceeding.

### 19.3 Medium (Standard Patterns)

10. **Unique Paths II** — Grid DP with obstacles.
11. **Minimum Falling Path Sum** — Falling DP.
12. **Longest Common Substring** — Sequence DP with reset.
13. **Edit Distance** — Three-source transition.
14. **Distinct Subsequences** — Counting variant.
15. **Longest Palindromic Subsequence** — Interval DP on a single string.
16. **Palindromic Substrings** — Interval DP for counting.
17. **Interleaving String** — 2D reachability.
18. **Word Break** — 1D/Partition DP.

### 19.4 Medium+ (Slightly Harder)

19. **Shortest Common Supersequence** — LCS + reconstruction.
20. **Decode Ways II** — 1D DP with state.
21. **Best Time to Buy and Sell Stock III** — 2D DP with 2 transactions.
22. **Best Time to Buy and Sell Stock IV** — 2D DP with k transactions.
23. **Best Time to Buy and Sell Stock with Cooldown** — 2D DP with state.
24. **Count Square Submatrices with All Ones** — Grid DP.
25. **Longest Increasing Path in a Matrix** — DP + DFS.

### 19.5 Hard (Interview Hard)

26. **Dungeon Game** — Reverse grid DP.
27. **Cherry Pickup** — Two-path 3D DP.
28. **Cherry Pickup II** — Two robots, 3D DP.
29. **Burst Balloons** — Reverse interval DP.
30. **Palindrome Partitioning II** — Interval DP + 1D DP.
31. **Matrix Chain Multiplication** — Classic interval DP.
32. **Boolean Parenthesization** — Interval DP with two values.
33. **Egg Dropping** — 2D DP with binary search optimization.
34. **Number of Ways to Stay in Place** — 2D DP with steps.
35. **Minimum Difficulty of Job Schedule** — 2D DP.
36. **Out of Boundary Paths** — 2D DP with steps.
37. **Soup Servings** — DP with floats and early termination.
38. **Knight Dialer** — 2D DP with steps and digits.
39. **Domino and Tromino Tiling** — 1D DP with extension.
40. **Number of Ways to Form Target String** — 2D DP with character counts.

### 19.6 Interview Hard (Challenge)

41. **Regex Matching** (`.` and `*`) — 2D DP with star handling.
42. **Wildcard Matching** (`?` and `*`) — 2D DP.
43. **Optimal BST** — Interval DP with root iteration.
44. **Smallest Sufficient Team** — Bitmask DP.
45. **TSP (Traveling Salesman)** — Bitmask DP.
46. **Partition to K Equal Sum Subsets** — Bitmask DP.
47. **Number of Music Playlists** — 2D DP with combinatorics.
48. **Maximum Number of Achievable Transfer States** — Bitmask + DP.
49. **Strange Printer** — Interval DP variant.
50. **Number of Ways to Paint N × 3 Grid** — State-transition DP.

### 19.7 How to Use This Progression

Work through problems in order. For each:
1. Attempt the problem for 30 minutes without help.
2. If stuck, re-read the relevant section of this guide.
3. After solving, compare with the optimal solution.
4. Re-derive the solution from scratch the next day.

Aim for **5 problems per week** at the easy/medium level, **3 per week** at medium+/hard, **1 per week** at interview-hard. This pace gives deep mastery in 3-4 months.

---

## 20. Debugging Guide

This section teaches how to debug 2D DP code systematically.

### 20.1 Symptom → Cause Flowchart

```
[Wrong Answer]
     |
     +---> Is it off by a constant?
     |        - Check base case values
     |        - Check if returning dp[n][m] vs dp[n-1][m-1]
     |
     +---> Is it off by one cell?
     |        - Check loop bounds (<= vs <)
     |        - Check substring indices
     |
     +---> Is it wrong only for some inputs?
     |        - Check edge cases (empty, single, etc.)
     |        - Check obstacle handling
     |
     +---> Is it always zero?
     |        - Check initialization
     |        - Check base case propagation
     |
     +---> Is it too large / negative?
            - Check overflow
            - Check sentinel values
            - Check mod arithmetic

[TLE (Time Limit Exceeded)]
     |
     +---> Is the time complexity correct?
     |        - Count states × work per state
     |
     +---> Are you re-computing states?
     |        - Check memoization cache
     |        - Check tabulation order
     |
     +---> Can you optimize the inner loop?
            - Binary search
            - Prefix sums
            - Bitset

[MLE (Memory Limit Exceeded)]
     |
     +---> Can you compress to 1D?
     |        - Check dependency on previous row only
     |        - Apply rolling row
     |
     +---> Can you use a sparse structure?
            - HashMap for memoization
            - Bitset for boolean tables

[ArrayIndexOutOfBounds]
     |
     +---> Is the table sized correctly?
     |        - Should be [n+1][m+1] for "first i chars" semantics
     |
     +---> Are boundary checks in place?
            - Check i > 0, j > 0 before accessing i-1, j-1
```

### 20.2 Debugging Techniques

**Technique 1: Print the DP table.** After filling, print the entire table. Compare with a hand-computed small example. Discrepancies pinpoint the bug.

**Technique 2: Assert invariants.** Add assertions for properties that should hold (e.g., `dp[i][j] >= 0` for non-negative inputs). Violations reveal bugs.

**Technique 3: Compare with brute force.** For small inputs, compute the answer by brute force and compare. This catches systematic errors.

**Technique 4: Test edge cases.** Empty input, single element, all obstacles, all same, etc.

**Technique 5: Trace one cell.** Pick a single cell and manually verify its value by re-deriving it from the recurrence.

**Technique 6: Check the iteration order.** Walk through the loop and verify every cell's dependencies are computed before it.

**Technique 7: Isolate the recurrence.** Implement the recurrence in a top-down memoized version. If the answers match, the bug is in the tabulation (iteration order or initialization). If they differ, the bug is in the recurrence itself.

### 20.3 Common Bug Patterns and Fixes

**Bug: Always returns 0.** Cause: Base case not initialized, or main loop overwrites base cells with 0. Fix: Initialize base cells outside the main loop; check that the main loop doesn't touch them.

**Bug: Always returns Integer.MAX_VALUE or MIN_VALUE.** Cause: Sentinels not handled correctly. Fix: Check for sentinels before arithmetic; use a separate "reachable" boolean table if needed.

**Bug: Stack overflow in memoization.** Cause: Recursion too deep (e.g., 10^5 deep). Fix: Convert to tabulation, or increase stack size with `Xss` JVM flag.

**Bug: Works on small inputs, fails on large.** Cause: Integer overflow, or off-by-one that only manifests at boundaries. Fix: Use `long`; test on edge sizes.

**Bug: Wrong answer only for palindromes / symmetric inputs.** Cause: Diagonal handling incorrect. Fix: Verify `dp[i][i]` initialization; verify `dp[i][i+1]` for gap-1 case.

**Bug: TLE on the largest input.** Cause: Hidden O(n^3) when O(n^2) is expected, or constant factor too high. Fix: Profile; look for unnecessary work in the inner loop.

### 20.4 Pre-Submission Checklist

Before declaring your solution done:
- [ ] Code compiles without warnings.
- [ ] All test cases pass.
- [ ] Edge cases (empty, single, max size) handled.
- [ ] No integer overflow (use `long` if needed).
- [ ] Complexity stated and acceptable.
- [ ] Variable names are descriptive.
- [ ] Comments explain the state and recurrence.
- [ ] Dry-run on a small example completed.
- [ ] Similar problems mentioned.

If all boxes are ticked, you're interview-ready.

---


## 21. Pattern Summary Table

This is a one-page summary of all major 2D DP patterns. Use it as a quick reference.

| Pattern | Recognition | State | Transition | Iteration Order | Complexity | Optimization |
|---|---|---|---|---|---|---|
| **Grid DP (forward)** | Grid + right/down moves | `dp[i][j]` = best to reach `(i,j)` | `dp[i][j] = grid[i][j] + min/max(dp[i-1][j], dp[i][j-1])` | Row-major top-to-bottom | O(mn) time, O(mn) space | Rolling row → O(n) space; in-place → O(1) |
| **Grid DP (reverse)** | Grid + "survive to end" | `dp[i][j]` = min needed at `(i,j)` | `dp[i][j] = max(1, min(dp[i+1][j], dp[i][j+1]) - grid[i][j])` | Bottom-to-top, right-to-left | O(mn) time, O(mn) space | Rolling row (with reverse direction) |
| **Triangle DP** | Triangular grid | `dp[i][j]` = best from `(i,j)` to bottom | `dp[i][j] = tri[i][j] + min(dp[i+1][j], dp[i+1][j+1])` | Bottom-to-top | O(n^2) time, O(n) space | In-place or single row |
| **Falling Path** | Grid + 3 downward moves | `dp[i][j]` = best falling sum ending at `(i,j)` | `dp[i][j] = grid[i][j] + min(dp[i-1][j-1], dp[i-1][j], dp[i-1][j+1])` | Row-major top-to-bottom | O(n^2) time | Rolling row → O(n) |
| **LCS** | Two strings + "common subsequence" | `dp[i][j]` = LCS length of prefixes | Match: `1 + dp[i-1][j-1]`; else: `max(dp[i-1][j], dp[i][j-1])` | Row-major | O(mn) time | Rolling row + diagonal save → O(min(m,n)) |
| **Longest Common Substring** | Two strings + "contiguous" | `dp[i][j]` = longest common suffix length | Match: `1 + dp[i-1][j-1]`; else: `0`; track global max | Row-major | O(mn) time | Rolling row → O(min(m,n)) |
| **Edit Distance** | Two strings + "operations" | `dp[i][j]` = edit distance of prefixes | Match: `dp[i-1][j-1]`; else: `1 + min(dp[i-1][j-1], dp[i-1][j], dp[i][j-1])` | Row-major | O(mn) time | Rolling row + diagonal save |
| **Distinct Subsequences** | Two strings + "count" | `dp[i][j]` = # ways to form `t[0..j-1]` from `s[0..i-1]` | Match: `dp[i-1][j-1] + dp[i-1][j]`; else: `dp[i-1][j]` | Row-major | O(mn) time | Rolling row + diagonal save |
| **Shortest Common Supersequence** | Two strings + "shortest common super" | Same as LCS, then reconstruct | LCS + path reconstruction | Row-major + backtrack | O(mn) time, O(mn) space | Needs full table for reconstruction |
| **Interleaving String** | Two strings + target | `dp[i][j]` = true if `s3[0..i+j-1]` interleaves | `dp[i][j] = (dp[i-1][j] and s1[i-1]==s3[i+j-1]) or (dp[i][j-1] and s2[j-1]==s3[i+j-1])` | Row-major | O(mn) time | Rolling row → O(min(m,n)) |
| **Longest Palindromic Subsequence** | One string + "palindromic subseq" | `dp[i][j]` = LPS of `s[i..j]` | Match: `2 + dp[i+1][j-1]`; else: `max(dp[i+1][j], dp[i][j-1])` | By gap length | O(n^2) time | Usually no compression |
| **Palindrome Partitioning II** | One string + "min cuts" | `dp[i]` = min cuts for `s[0..i]`; `isPal[i][j]` | Precompute `isPal`; then `dp[i] = min(dp[j-1] + 1)` for `j` where `isPal[j][i]` | Forward for `isPal` (reverse i); forward for `dp` | O(n^2) time, O(n^2) space | None typical |
| **Matrix Chain Multiplication** | Array of dimensions + "min mults" | `dp[i][j]` = min mults for matrices `i..j` | `dp[i][j] = min over k of dp[i][k] + dp[k+1][j] + p[i-1]*p[k]*p[j]` | By interval length | O(n^3) time, O(n^2) space | Knuth optimization in special cases |
| **Burst Balloons** | Array + "burst in any order" | `dp[l][r]` = max coins in open interval `(l,r)` | `dp[l][r] = max over k of dp[l][k] + dp[k][r] + nums[l]*nums[k]*nums[r]` | By interval length | O(n^3) time, O(n^2) space | None typical |
| **Boolean Parenthesization** | Boolean expr + "count true" | `dpT[i][j]`, `dpF[i][j]` | Combine based on operator at k | By interval length | O(n^3) time, O(n^2) space | None typical |
| **Egg Dropping** | k eggs, n floors | `dp[k][n]` = min moves | `dp[k][n] = 1 + min over f of max(dp[k-1][f-1], dp[k][n-f])` | By k, then n | O(kn log n) with binary search | Alternative formulation: O(k * answer) |
| **Word Break** | String + dictionary | `dp[i]` = can segment `s[0..i-1]` | `dp[i] = any(dp[j] and s[j..i-1] in dict)` | Forward | O(n^2 * word_len) time, O(n) space | Trie for dictionary lookup |
| **Stock with k transactions** | Prices + k | `dp[i][k][hold]` = best profit | Buy/sell/hold transitions | By day, by transaction | O(nk) time, O(k) space | Rolling |

### 21.1 Reading the Table

When you encounter a new problem:
1. Identify the input shape (grid, two strings, single string, sequence + resource).
2. Match to a row in the table.
3. Use the state, transition, and iteration order from that row as a starting point.
4. Adapt to the specifics of the problem.

The table is a starting point, not a complete answer. Most interview problems will require some adaptation — a small twist on the canonical pattern.

---

## 22. Cheat Sheet

A one-screen revision sheet for the night before the interview.

### 22.1 The 4 Canonical 2D DP Families

```
GRID DP:        dp[i][j] = grid[i][j] + min/max(dp[i-1][j], dp[i][j-1])
                Iterate: row-major, top-to-bottom

SEQUENCE DP:    if match: dp[i][j] = 1 + dp[i-1][j-1]
                else:     dp[i][j] = max(dp[i-1][j], dp[i][j-1])
                Iterate: row-major

INTERVAL DP:    dp[l][r] = min/max over k of (dp[l][k] + dp[k+1][r] + cost)
                Iterate: by gap length, outer; by left, inner

PARTITION DP:   dp[i][k] = best over choices of (dp[...][k-1] + gain)
                Iterate: by i, by k
```

### 22.2 Space Optimization Rules of Thumb

```
If dp[i][j] depends only on row i-1:
    -> Rolling row (single array of size n)

If dp[i][j] also depends on dp[i-1][j-1] (diagonal):
    -> Save the diagonal in a temp variable before overwriting

If dp[i][j] depends on dp[i-1][j+1] (anti-diagonal):
    -> Cannot easily compress; use two rows

For 0/1 knapsack compression:
    -> Inner loop iterates BACKWARDS (so item is used at most once)

For unbounded knapsack compression:
    -> Inner loop iterates FORWARDS (so item can be reused)
```

### 22.3 Iteration Order Rules

```
FORWARD-DOWN dependencies (LCS, Edit Distance, Min Path Sum):
    Iterate i = 1..n, j = 1..m (row-major, top-to-bottom)

REVERSE dependencies (Dungeon Game):
    Iterate i = n..1, j = m..1 (bottom-to-top, right-to-left)

INTERVAL dependencies (MCM, Burst Balloons, LPS):
    Iterate length = 1..n, then left = 0..n-length

ANTI-DIAGONAL dependencies (rare):
    Iterate by anti-diagonal: i + j = constant
```

### 22.4 Complexity Formulas

```
Time  = (number of states) × (work per state)
Space = number of states (or less after optimization)

Grid DP:      O(mn) time, O(n) space optimized
Sequence DP:  O(mn) time, O(min(m,n)) space optimized
Interval DP:  O(n^2) or O(n^3) time, O(n^2) space
Bitmask DP:   O(n × 2^n) time, O(2^n) space

10^6: trivial. 10^7: comfortable. 10^8: borderline. 10^9: too slow.
```

### 22.5 Base Case Patterns

```
Empty prefix (i=0 or j=0): usually 0 for optimization, 1 for counting
Single element (i==j): usually the element itself or 1
Empty interval (i > j): usually 0
Obstacle cell: usually 0 for counting, INF for optimization
```

### 22.6 Common Sentinels

```
Integer.MIN_VALUE: "unreachable" for maximization
Integer.MAX_VALUE: "unreachable" for minimization (careful with +1 overflow)
-1: "unvisited" for memoization
0: "no paths" or "empty" depending on context
```

### 22.7 Interview Time Allocation (45 min)

```
5 min:  Understand + clarify
10 min: State + transition (whiteboard)
15 min: Code
10 min: Dry run + explain
5 min:  Follow-ups + similar problems
```

### 22.8 Red Flags (Don't Do These)

```
- Coding before explaining the state
- Skipping the dry run
- Not stating complexity
- Forgetting base cases
- Using 1D where 2D is needed (or vice versa)
- Wrong iteration order
- Forgetting space optimization when asked
- Not handling edge cases
```

---

## 23. Interview Cheat Codes

**100+ expert observations** specific to multidimensional DP. Internalize these; they trigger insights during interviews.

### 23.1 State Recognition (1-20)

1. **Two changing variables → think 2D DP.** If the recursion has two arguments that vary, you need 2D.
2. **Compare two sequences → sequence DP.** Two strings, two arrays.
3. **Rectangle or matrix → grid DP.** Even if "matrix" is mentioned, look for grid DP.
4. **Subarray boundaries → interval DP.** Left and right endpoints.
5. **Left and right pointers define state → interval DP.** Classic palindrome / range problem.
6. **Transition depends on three neighbors → matrix DP.** Above, left, diagonal.
7. **Dependencies form a DAG → bottom-up DP works.** Otherwise need topological sort first.
8. **"Number of ways" → counting DP, often with mod.** Initialize base cases to 1.
9. **"Is it possible?" → boolean DP.** Initialize base cases appropriately.
10. **"Minimum/Maximum" → optimization DP.** Initialize with sentinels.
11. **k transactions / k eggs / k parts → add resource dimension.** `dp[i][k]`.
12. **"Previous choice" mentioned → add prev dimension.** Paint houses, no-adjacent-same.
13. **Subset selection with n ≤ 20 → bitmask DP.** TSP-style.
14. **Huge n, counting numbers in range → digit DP.** Position-by-position.
15. **Tree structure → tree DP.** Post-order traversal.
16. **DAG → graph DP.** Topological sort then process.
17. **Game with alternating players → minimax DP.** Each player picks the best for themselves.
18. **"Last to burst/remove" → reverse interval DP.** Burst Balloons pattern.
19. **"First to burst/remove" → harder, often different.** Be careful.
20. **Two synchronized paths → 3D state.** Cherry Pickup pattern.

### 23.2 Transition Design (21-40)

21. **Enumerate choices exhaustively.** Missing a choice = wrong answer.
22. **For each choice, identify next state and cost.** This is the transition.
23. **Match case often gives `1 + dp[i-1][j-1]`.** Subsequence extension.
24. **Mismatch case often gives `max(dp[i-1][j], dp[i][j-1])`.** Skip from either side.
25. **Edit operations: each costs 1.** Insert, delete, replace.
26. **Split at k: combine sub-solutions + merge cost.** MCM pattern.
27. **Cost of multiplying p×q by q×r is pqr.** Matrix chain.
28. **"Use it or skip it" pattern.** House Robber, Knapsack.
29. **"Both move forward" → 4 combinations.** Two-path DP.
30. **Modulo after each addition.** Avoids overflow in counting DP.
31. **Use long for safety.** Even when int seems enough.
32. **Sentinel for unreachable.** Use `Integer.MIN_VALUE` for max, `MAX_VALUE` for min.
33. **Check sentinel before arithmetic.** `MIN_VALUE + 1` overflows.
34. **Base case dp[0][0] = 1 for counting.** One way to do nothing.
35. **Base case dp[0][0] = 0 for optimization.** No cost to do nothing.
36. **Base case dp[0][j] = j for edit distance.** Insert j characters.
37. **Boundary check before accessing i-1, j-1.** Avoid out-of-bounds.
38. **Pad the table with an extra row/column.** Simplifies boundary handling.
39. **In-place update only if input is mutable.** Confirm with interviewer.
40. **Reconstruction needs full table or divide-and-conquer.** Hirschberg for LCS.

### 23.3 Iteration Order (41-60)

41. **Forward-down deps → row-major top-to-bottom.** Standard.
42. **Reverse deps → bottom-to-top, right-to-left.** Dungeon Game.
43. **Interval deps → by gap length.** MCM, LPS.
44. **Anti-diagonal deps → by i+j.** Rare; some grid problems.
45. **0/1 knapsack compressed → reverse inner.** Don't reuse items.
46. **Unbounded knapsack compressed → forward inner.** Allow reuse.
47. **Two-pass for left/right constraints.** Candy problem.
48. **Topological order for graph DP.** Kahn's algorithm.
49. **Post-order for tree DP.** Process children before parent.
50. **Pre-order rare in DP.** Usually bottom-up.
51. **Iteration order must respect dependencies.** Walk through mentally.
52. **When unsure, draw the dep graph.** Visual check.
53. **Cached memoization doesn't need order.** Top-down just works.
54. **Tabulation requires careful order.** Easier to get wrong.
55. **Wrong order often gives 0 or INF.** Symptom of bug.
56. **Test on tiny example (n=2 or 3).** Catches order bugs fast.
57. **Print the table to verify.** Visual inspection.
58. **For interval DP, gap-outer is non-negotiable.** Otherwise wrong.
59. **For 3D DP, iterate outer dimensions in dependency order.** Same principle.
60. **Two-row rolling: swap, don't copy.** `tmp = prev; prev = curr; curr = tmp;`.

### 23.4 Space Optimization (61-75)

61. **Rolling row when only previous row needed.** Most grid/sequence DP.
62. **Diagonal save for LCS-style.** `prevDiag = dp[0]; ... dp[j] = 1 + prevDiag;`.
63. **In-place if input mutable.** Often acceptable in interviews.
64. **Two rows when next-row dep exists.** Cannot compress further.
65. **O(n) space typical for 2D DP.** After compression.
66. **O(1) space possible for some grid DP.** In-place mutation.
67. **Memory before time, usually.** MLE is harder to fix than TLE.
68. **HashMap for sparse states.** When most cells are 0/unreachable.
69. **BitSet for boolean DP.** 64x compression.
70. **Don't compress if reconstruction needed.** Unless using Hirschberg.
71. **Compress only after correctness.** Optimize at the end.
72. **Mention the optimization even if not coded.** Shows awareness.
73. **Rolling direction matters.** 0/1 vs unbounded.
74. **Reset stale values when rolling.** Avoid carry-over.
75. **Test with maximum-size input.** Verify memory.

### 23.5 Complexity Reasoning (76-85)

76. **States × work per state = total time.** Always.
77. **Grid DP: O(mn) time, O(n) space.** Standard.
78. **Interval DP: O(n^3) if k-split, O(n^2) otherwise.** Check the k loop.
79. **Bitmask DP: O(n × 2^n).** Use only for n ≤ 20.
80. **10^8 is the 1-second threshold.** Aim below.
81. **Constant factors matter.** O(mn) with heavy constant can still TLE.
82. **Binary search inside DP can save a factor.** Egg dropping.
83. **Prefix sums can save a factor.** Range-sum queries.
84. **Matrix exponentiation for linear recurrences.** O(log n).
85. **Always state time and space.** Interviewers expect it.

### 23.6 Interview Communication (86-100)

86. **State the state explicitly.** "dp[i][j] means..."
87. **State the transition explicitly.** "The recurrence is..."
88. **State the base cases.** "For i=0, dp[0][j] = ..."
89. **State the iteration order.** "We iterate i from 1 to n..."
90. **State the final answer cell.** "Return dp[n][m]."
91. **Dry-run on a tiny example.** Walk through one cell.
92. **Mention complexity.** "Time O(mn), space O(min(m,n)) optimized."
93. **Mention space optimization.** Even if not coded.
94. **Mention similar problems.** "This is like Edit Distance but..."
95. **Mention follow-ups.** "If we wanted the path, we'd..."
96. **Acknowledge edge cases.** "For empty input, we return 0."
97. **Don't memorize; derive.** Interviewers can tell.
98. **Use whiteboard for state and recurrence.** Code on computer.
99. **End with confidence.** "This solution is correct and runs in..."
100. **Ask if the interviewer has questions.** Engage them.

### 23.7 Pattern-Specific Insights (101-110)

101. **LCS of s and reverse(s) = LPS of s.** Useful shortcut.
102. **Edit distance with only insert/delete = m + n - 2*LCS.** Common variant.
103. **SCS length = m + n - LCS.** Direct formula.
104. **Number of distinct subsequences: dp[0] = 1 always.** Empty target.
105. **Interval DP triangle fills from diagonal.** Visualize.
106. **Burst Balloons: add sentinels at boundaries.** Value 1.
107. **Egg dropping: binary search the floor.** O(kn log n).
108. **Word break: use Trie for O(word_len) lookup.** Faster than set.
109. **Stock with k transactions: if k >= n/2, switch to greedy.** Avoid TLE.
110. **Regex matching: handle `*` as zero-or-more of previous.** Tricky transition.

### 23.8 Anti-Patterns (111-120)

111. **Don't use DP for greedy problems.** Activity selection, Huffman.
112. **Don't use DP for shortest path in general graphs.** Use Dijkstra.
113. **Don't use DP for n-Queens.** Backtracking.
114. **Don't use DP for sorting.** O(n log n) algorithms suffice.
115. **Don't use DP for two-sum.** Hash map.
116. **Don't use DP for subarray sum equals k.** Prefix sum + hash.
117. **Don't use DP for sliding window max.** Deque.
118. **Don't use DP for max subarray.** Kadane's (technically 1D DP).
119. **Don't use DP for parenthesis matching.** Stack.
120. **Don't use DP for tree traversal.** Recursion.

---

## 24. FAQ

**50+ interview FAQs about 2D DP.**

### 24.1 General DP Questions

**Q1. What is dynamic programming?**
DP is a technique for solving problems with overlapping subproblems and optimal substructure by solving each subproblem once and storing the result.

**Q2. What's the difference between memoization and tabulation?**
Memoization is top-down (recursive with cache); tabulation is bottom-up (iterative, filling a table). Both produce the same answers; tabulation is usually faster and easier to optimize for space.

**Q3. When should I use DP vs. divide-and-conquer?**
Use DP when subproblems overlap (the same subproblem is reached multiple times). Use divide-and-conquer when subproblems are independent (no overlap).

**Q4. How do I know if a problem has optimal substructure?**
Check whether the optimal solution can be built from optimal solutions to subproblems. If yes, DP applies. If a subproblem's optimal solution depends on the larger problem's solution, you may need a different approach.

**Q5. What's the difference between DP and greedy?**
Greedy makes an irrevocable local choice at each step; DP explores all choices. Greedy is faster but only works when the local choice is always part of some global optimum. DP is more general.

### 24.2 2D DP Specifics

**Q6. How do I know if I need 2D DP instead of 1D?**
If your recursion has two arguments that vary independently, you need 2D. If only one varies, 1D suffices.

**Q7. How do I choose the dimensions?**
The dimensions are the variables that change between recursive calls. List them; each becomes a dimension.

**Q8. Can I always compress 2D DP to 1D?**
No. Only when the recurrence references only the previous row (or column). Interval DP and some grid DP cannot be compressed.

**Q9. What's the "diagonal save" trick?**
When compressing LCS or Edit Distance to 1D, you need the previous row's `dp[j-1]` value before it's overwritten. Save it in a temp variable.

**Q10. Why does the inner loop direction matter for knapsack compression?**
Reverse iteration uses the previous row's value (item used at most once = 0/1 knapsack). Forward iteration uses the current row's value (item can be reused = unbounded).

### 24.3 Interview Strategy

**Q11. Should I start with memoization or tabulation in an interview?**
Start with memoization to verify correctness, then convert to tabulation if asked about optimization or if the recursion is too deep.

**Q12. How do I explain DP to an interviewer who isn't familiar with the problem?**
Walk through: (1) brute force, (2) overlapping subproblems, (3) state definition, (4) recurrence, (5) iteration, (6) complexity.

**Q13. What if I can't figure out the state?**
Write the brute-force recursion. The arguments that vary are your state dimensions.

**Q14. What if I can't figure out the transition?**
At a generic state, enumerate all choices. For each, identify the next state and cost. Combine (min/max/sum).

**Q15. What if my DP is too slow?**
Count states × work per state. If too high: drop a dimension, use binary search, use prefix sums, or look for a different algorithm.

**Q16. What if my DP uses too much memory?**
Compress to 1D (rolling row), use a sparse structure (HashMap), or use BitSet for boolean DP.

**Q17. Should I write tests in an interview?**
Usually no, but mention edge cases you'd test: empty, single, max size, all same, all zero.

**Q18. What if I run out of time?**
Have a partial solution (correct but slow) is better than no solution. Mention the optimization you'd add.

**Q19. Should I write the brute force first?**
Verbally describe it and state its complexity, but don't code it. Move to DP after the interviewer agrees brute force is too slow.

**Q20. How do I handle follow-up questions?**
Listen carefully, ask clarifying questions, and adapt your solution. Don't be afraid to say "let me think about that for a moment."

### 24.4 Common Confusions

**Q21. Is LCS the same as longest common substring?**
No. Subsequence can skip characters; substring must be contiguous. Different recurrences.

**Q22. Is edit distance symmetric?**
Yes. ED(A, B) = ED(B, A). The recurrence is symmetric.

**Q23. Why does Dungeon Game need reverse DP?**
Because the HP needed at a cell depends on what comes after, not before. Forward DP cannot know the future.

**Q24. Why does Burst Balloons use "last to burst" thinking?**
Because the subproblems become independent. If you think "first to burst," the boundaries of the remaining subproblems are unclear.

**Q25. What's the difference between interval DP and partition DP?**
Interval DP splits a range at a point (l, k, r). Partition DP splits a sequence into k contiguous parts. They overlap but emphasize different aspects.

### 24.5 Technical Questions

**Q26. Why do I need to add 1 to array dimensions in DP tables?**
Because the state "first i characters" includes i=0 (empty prefix). So the table size is `(n+1) × (m+1)`.

**Q27. Why use `Integer.MIN_VALUE` instead of -1 for "unreachable"?**
Because -1 might be a valid answer (e.g., for HP). MIN_VALUE is unambiguous for maximization problems.

**Q28. When should I use `long` instead of `int`?**
When the answer can exceed 2^31 - 1. Counting problems often need `long`. Always apply mod for very large counts.

**Q29. How do I handle negative numbers in subset sum?**
Offset by the sum of negatives. Map each value `v` to `v - min_value`, and adjust the target sum accordingly.

**Q30. How do I reconstruct the path/solution from a DP table?**
Either store predecessors during the fill, or backtrack through the table after the fill by reversing the recurrence.

### 24.6 Advanced Topics

**Q31. What is Hirschberg's algorithm?**
An algorithm for LCS that uses O(min(m,n)) space while still producing the actual subsequence. Uses divide-and-conquer.

**Q32. What is Knuth's optimization?**
For certain interval DP problems where the optimal split point k is monotonic, you can reduce the time from O(n^3) to O(n^2).

**Q33. What is the convex hull trick?**
An optimization for DP recurrences of the form `dp[i] = min over j of (m[j] * x[i] + b[j])`. Maintains a convex hull of lines for O(log n) per query.

**Q34. What is divide-and-conquer DP optimization?**
For recurrences like `dp[i][j] = min over k < j of (dp[i-1][k] + cost(k, j))` where the optimal k is monotonic in j. Reduces O(n^2 k) to O(n k log n).

**Q35. What is digit DP?**
A technique for counting numbers in a range [L, R] that satisfy a digit-based property. State is (position, tight, other constraints).

**Q36. What is bitmask DP?**
DP where one dimension is a bitmask representing a subset. Used for TSP, assignment, and small-n subset problems.

**Q37. When is DP not the right approach?**
When the problem has greedy structure, when subproblems don't overlap, when a closed-form formula exists, or when n is too large (e.g., 10^18 needs matrix exponentiation).

**Q38. How do I DP on trees?**
Use post-order traversal. State at each node combines states of children. Examples: diameter, max independent set, max path sum.

**Q39. How do I DP on graphs?**
For DAGs: topological sort, then process in order. For general graphs: usually need a different approach (Dijkstra, Bellman-Ford).

**Q40. Can DP be parallelized?**
Sometimes. Cells with no dependencies on each other can be computed in parallel (e.g., cells in the same anti-diagonal of a grid DP). In practice, rare in interviews.

### 24.7 Meta Questions

**Q41. How many DP problems should I solve before interviews?**
Aim for 50-100, covering all major patterns. Quality over quantity — rederive solutions rather than memorize.

**Q42. What's the best way to practice?**
Pick a pattern, solve 5-10 problems in that pattern, then move on. Revisit each pattern weekly for retention.

**Q43. Should I use LeetCode, HackerRank, or Codeforces?**
LeetCode for interview prep; Codeforces for competitive programming depth. HackerRank is okay but less representative.

**Q44. How do I handle DP problems I've never seen?**
Apply the 10-step process: observation, brute force, state, transition, memoization, tabulation, optimization, code, explain.

**Q45. What if I blank out during the interview?**
Take a breath. Start with the brute force. Write the recursion. The state will emerge.

**Q46. Is it okay to ask for hints?**
Yes, after you've shown genuine effort. Interviewers prefer engaged candidates who ask good questions over stuck candidates who suffer in silence.

**Q47. Should I mention time/space complexity if not asked?**
Yes. Always. It shows engineering maturity.

**Q48. What if my solution has a bug?**
Acknowledge it, explain the fix, and re-verify. Interviewers care about how you handle bugs, not whether you never make them.

**Q49. How do I end the interview?**
Summarize what you've built, mention complexities and trade-offs, suggest follow-ups, and ask the interviewer if they have questions.

**Q50. What's the single most important thing in DP interviews?**
State design. Get the state right, and the rest follows. Get it wrong, and no amount of coding will save you.

### 24.8 Bonus Questions

**Q51. Why is DP called "dynamic programming"?**
Bellman chose "dynamic" because it sounded impressive and "programming" referred to optimization planning, not coding. The name is historical.

**Q52. What's the hardest 2D DP problem you've seen?**
Subjective, but candidates include: TSP with side constraints, multi-dimensional state with many constraints, and DP with adversarial inputs.

**Q53. Are there problems where DP is the only known approach?**
Yes — many problems provably require exponential time unless P=NP, and DP gives the best known polynomial-for-fixed-parameter algorithms.

**Q54. How does DP relate to reinforcement learning?**
RL is essentially DP with unknown transition/reward functions, learned through exploration. The Bellman equation is the same.

**Q55. What's the future of DP in the age of LLMs?**
LLMs can solve many standard DP problems, but the underlying reasoning (state, transition, iteration) remains a foundational skill. Understanding DP helps you verify and debug LLM-generated solutions.

---

## 25. Final Challenge

**30 unseen 2D DP practice problems**, arranged by difficulty. Do NOT look up solutions; derive them yourself using the 10-step process.

### 25.1 Easy (1-8)

1. **Grid Path Count with Restrictions.** In an `n × n` grid, count paths from top-left to bottom-right moving only right or down, but you cannot step on cells where `(row + col) % 3 == 0`.

2. **Two-String Subsequence Check.** Given three strings A, B, C, count the number of distinct ways to interleave A and B to form C. (Hint: This is Interleaving String but counting.)

3. **Max Sum Path in Triangle with K Skips.** Find the maximum sum path through a triangle from top to bottom, where you can skip at most k cells.

4. **Longest Common Subsequence of Three Strings.** Given three strings, find the length of their longest common subsequence. (3D DP.)

5. **Edit Distance with Custom Costs.** Edit distance where insert costs 2, delete costs 3, replace costs 5. Return the minimum cost.

6. **Number of Palindromic Substrings.** Count the number of palindromic substrings in a given string.

7. **Unique Paths III Variant.** In a grid, count paths from start to end visiting every empty cell exactly once. (Backtracking, but think about how DP could help.)

8. **Minimum Cost to Merge Stones.** Given N stones with costs, merge them into one stone with minimum total cost. Each merge combines K consecutive piles.

### 25.2 Medium (9-18)

9. **Palindromic Subsequence Count.** Count the number of distinct palindromic subsequences of a string (mod 10^9 + 7).

10. **Maximum Submatrix Sum.** Find the submatrix with the maximum sum in a 2D matrix. (Kadane's 2D, but think DP.)

11. **Count Square Submatrices with All Ones.** Given a binary matrix, count the number of square submatrices with all ones.

12. **Number of Ways to Stay in Place.** A pointer at position 0 in an array of size `arrLen`. At each step, you can move left, right, or stay. Return the number of ways to be at position 0 after `steps` steps.

13. **Longest Line of Consecutive Ones in Matrix.** Given a binary matrix, find the longest line of consecutive ones (horizontal, vertical, diagonal, anti-diagonal).

14. **Knight's Dialer.** A knight on a phone pad dialer (1-9, with 0 = 10). Count distinct numbers of length N that can be dialed.

15. **Domino and Tromino Tiling.** Count the number of ways to tile a 2 × N board with dominos and trominos.

16. **Minimum Difficulty of Job Schedule.** Schedule jobs over D days; each day's difficulty is the max job that day. Minimize the total difficulty.

17. **Out of Boundary Paths.** Count paths starting at (i, j) in an m × n grid that leave the boundary in exactly N steps.

18. **Soup Servings.** Two soups A and B with various serving operations. Return the probability that soup A is empty first.

### 25.3 Hard (19-25)

19. **Cherry Pickup II.** Two robots in an `m × n` grid, starting at top-left and top-right corners of row 0. Each moves down one row per step, with column shifts of -1, 0, +1. Collect maximum cherries (a cell's cherry is collected once if both land on it).

20. **Regex Matching.** Implement regex matching with `.` (any single char) and `*` (zero or more of preceding). Match the entire string.

21. **Wildcard Matching.** Implement matching with `?` (any single char) and `*` (any sequence, including empty).

22. **Strange Printer.** A printer that can print a sequence of characters; each turn it can print a contiguous substring of one character. Minimum turns to print the target string.

23. **Number of Ways to Form Target String.** Given a list of words of equal length and a target string, count the number of ways to form the target by picking characters from columns of the word list.

24. **Painting N × 3 Grid.** Count the number of ways to paint an N × 3 grid with 3 colors such that no two adjacent cells share a color.

25. **Number of Music Playlists.** Count the number of playlists of length `goal` using `k` different songs, where each song must be played, and between two plays of the same song there must be at least `k` other songs.

### 25.4 Interview Hard (26-30)

26. **Optimal Binary Search Tree.** Given keys and their search frequencies, build a BST that minimizes the total search cost.

27. **Smallest Sufficient Team.** Given a list of people and the skills each has, find the smallest team that covers all required skills.

28. **Traveling Salesman Problem.** Given a distance matrix between cities, find the minimum-cost tour that visits every city exactly once and returns to the start.

29. **Partition to K Equal Sum Subsets.** Given an array, can it be partitioned into K subsets each with equal sum?

30. **Maximum Number of Ways to Partition an Array.** Given an array and a value K, count the number of ways to partition the array into two non-empty parts such that the difference of sums can be changed to K by replacing at most one element.

### 25.5 How to Use These Problems

For each problem:
1. Spend 30-60 minutes attempting it without help.
2. If stuck, re-read the relevant section of this guide.
3. After solving (or giving up), write down what made the problem hard.
4. Categorize the problem (grid, sequence, interval, partition, etc.).
5. Identify which pattern it most resembles.
6. Note any unique insights or tricks.
7. Re-derive the solution from scratch the next day.

If you can solve 25 of the 30, you're interview-ready. If you can solve all 30, you're competitive-programming-ready.

---

## 26. Final Interview Checklist

Before coding any 2D DP solution in an interview, verify:

### 26.1 State

- [ ] State dimensions are minimal (no redundant dimensions).
- [ ] State captures all information needed by transitions.
- [ ] State is clearly defined and written on the whiteboard.
- [ ] State does not include the value being optimized.

### 26.2 Transition

- [ ] All choices at a generic state are enumerated.
- [ ] For each choice, the next state and cost are identified.
- [ ] Choices are mutually exclusive (no double counting).
- [ ] Choices are exhaustive (no missing case).
- [ ] Recurrence is written on the whiteboard.

### 26.3 Dependency Graph

- [ ] Dependencies are drawn on a small example grid.
- [ ] Dependency graph is acyclic.
- [ ] Iteration order is a topological sort of the dependency graph.

### 26.4 Base Cases

- [ ] Base cases for `i=0` are correct.
- [ ] Base cases for `j=0` are correct.
- [ ] Base cases for diagonal (`i==j`) are correct.
- [ ] Special base cases (obstacles, etc.) are handled.

### 26.5 Iteration Order

- [ ] Outer loop variable is correct (rows for forward, length for interval).
- [ ] Inner loop variable is correct.
- [ ] Loop bounds are inclusive/exclusive as intended.
- [ ] For 1D compression: inner direction is correct (forward or reverse).

### 26.6 Space Optimization

- [ ] Mentioned even if not coded.
- [ ] If coded: rolling row / diagonal save / in-place as appropriate.
- [ ] If reconstruction needed: full table or Hirschberg.

### 26.7 Complexity

- [ ] Time complexity stated (states × work per state).
- [ ] Space complexity stated (full vs. optimized).
- [ ] Complexity is acceptable for the constraints (under 10^8 for 1 second).

### 26.8 Boundary Conditions

- [ ] Empty input handled.
- [ ] Single-element input handled.
- [ ] Maximum-size input handled (no overflow).
- [ ] All-zero / all-same input handled.

### 26.9 Dry Run

- [ ] Walked through the algorithm on a small example.
- [ ] Verified the answer matches hand computation.
- [ ] Checked at least one edge case.

### 26.10 Communication

- [ ] State, transition, base case, iteration order all explained before coding.
- [ ] Code uses meaningful variable names.
- [ ] Comments explain key steps.
- [ ] Complexity stated at the end.
- [ ] Similar problems mentioned.
- [ ] Follow-up questions discussed.

If every box is ticked, you have done a complete, professional job. The interviewer will be impressed regardless of whether the code runs perfectly on the first try.

---

## Epilogue

2D Dynamic Programming is not a bag of tricks to memorize. It is a discipline of clear thinking: identify the state, derive the transition, choose the iteration order, optimize the space. Every DP problem, no matter how intimidating, yields to this discipline.

The students who master DP are not the ones who have memorized the most solutions. They are the ones who have internalized the **process** of solving: observation, brute force, state design, transition derivation, iteration, optimization. With this process, no 2D DP problem is unseen — only problems whose state and transition you have not yet derived.

Go forth and derive.

---

**End of the Definitive 2D Dynamic Programming Mastery Guide.**

