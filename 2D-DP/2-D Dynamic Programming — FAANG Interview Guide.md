# 2-D Dynamic Programming — FAANG Interview Guide (Part 1)

> This document is **Part 1** of the complete guide.
>
> Covered Problems:
>
> 1. Unique Paths
> 2. Unique Paths II
> 3. Minimum Path Sum
> 4. Triangle

---

# 1. Unique Paths

- **Difficulty:** Medium
- **Pattern:** Grid DP
- **LeetCode:** https://leetcode.com/problems/unique-paths/
- **Asked By:** Google, Amazon, Microsoft, Meta, Apple, Adobe
- **LLM-Proof Variant:** Yes

---

## Problem Statement

Given an `m × n` grid, a robot starts at the top-left corner.

It can move only:

- Right
- Down

Return the total number of unique paths to reach the bottom-right cell.

---

## Why Interviewers Ask It

This is the first grid DP problem almost every company uses to test whether candidates can:

- Identify overlapping subproblems
- Define DP states
- Build recurrence
- Convert recursion into tabulation
- Optimize space

---

## Intuition

At any cell,

```
Current Cell
     ↑
 Left + Top
```

To reach `(i,j)`:

- Last move came from left
- OR from top

Therefore

```
dp[i][j] =
dp[i-1][j]
+
dp[i][j-1]
```

---

## DP State

```
dp[i][j]

=

Number of ways
to reach cell (i,j)
```

---

## Transition

```
dp[i][j]
=
dp[i-1][j]
+
dp[i][j-1]
```

---

## Base Case

First row

```
1 1 1 1 1
```

Only move right.

First column

```
1
1
1
1
```

Only move down.

---

## DP Table Visualization

For

```
m = 3
n = 4
```

Initial

```
1 1 1 1
1 . . .
1 . . .
```

Fill

```
1 1 1 1
1 2 3 4
1 3 6 10
```

Answer

```
10
```

---

## Memoization

```java
class Solution {

    private int[][] memo;

    public int uniquePaths(int m, int n) {

        memo = new int[m][n];

        for (int[] row : memo)
            Arrays.fill(row, -1);

        return dfs(m - 1, n - 1);
    }

    private int dfs(int i, int j) {

        if (i == 0 || j == 0)
            return 1;

        if (memo[i][j] != -1)
            return memo[i][j];

        memo[i][j] =
                dfs(i - 1, j)
              + dfs(i, j - 1);

        return memo[i][j];
    }
}
```

---

## Tabulation

```java
class Solution {

    public int uniquePaths(int m, int n) {

        int[][] dp = new int[m][n];

        for (int i = 0; i < m; i++)
            dp[i][0] = 1;

        for (int j = 0; j < n; j++)
            dp[0][j] = 1;

        for (int i = 1; i < m; i++) {

            for (int j = 1; j < n; j++) {

                dp[i][j] =
                        dp[i - 1][j]
                      + dp[i][j - 1];
            }
        }

        return dp[m - 1][n - 1];
    }
}
```

---

## Space Optimized

```java
class Solution {

    public int uniquePaths(int m, int n) {

        int[] dp = new int[n];

        Arrays.fill(dp, 1);

        for (int i = 1; i < m; i++) {

            for (int j = 1; j < n; j++) {

                dp[j] += dp[j - 1];
            }
        }

        return dp[n - 1];
    }
}
```

---

## Complexity

| Approach | Time | Space |
|-----------|------|--------|
| Memoization | O(MN) | O(MN) |
| Tabulation | O(MN) | O(MN) |
| Space Optimized | O(MN) | O(N) |

---

## Common Pitfalls

- Forgetting first row initialization
- Forgetting first column initialization
- Mixing row and column indices

---

## Follow-up Questions

- Print one valid path.
- Count paths with diagonal movement.
- Count paths with blocked cells.
- Count shortest paths with weighted moves.

---

## LLM-Proof Interview Variant

Instead of moving only right/down,

allow

- Down
- Right
- Diagonal

How does recurrence change?

```
dp[i][j] =
left
+
top
+
diagonal
```

---

# 2. Unique Paths II

- **Difficulty:** Medium
- **Pattern:** Grid DP with Obstacles
- **LeetCode:** https://leetcode.com/problems/unique-paths-ii/
- **Asked By:** Amazon, Microsoft, Meta, Google
- **LLM-Proof Variant:** Yes

---

## Problem Statement

Cells may contain obstacles.

```
0 = free

1 = obstacle
```

Return total valid paths.

---

## Interview Goal

Tests whether candidate can modify an existing recurrence instead of creating a new DP.

---

## Intuition

Obstacle cells contribute

```
0
```

paths.

Everything else behaves exactly like Unique Paths.

---

## DP State

```
dp[i][j]

=

ways to reach
cell(i,j)
```

---

## Transition

If obstacle

```
dp[i][j]=0
```

Otherwise

```
dp[i][j]
=
top
+
left
```

---

## Example

Grid

```
0 0 0

0 1 0

0 0 0
```

DP

```
1 1 1

1 0 1

1 1 2
```

Answer

```
2
```

---

## Memoization

```java
class Solution {

    private int[][] memo;
    private int[][] grid;

    public int uniquePathsWithObstacles(int[][] obstacleGrid) {

        grid = obstacleGrid;

        int m = grid.length;
        int n = grid[0].length;

        memo = new int[m][n];

        for (int[] row : memo)
            Arrays.fill(row, -1);

        return dfs(m - 1, n - 1);
    }

    private int dfs(int i, int j) {

        if (i < 0 || j < 0)
            return 0;

        if (grid[i][j] == 1)
            return 0;

        if (i == 0 && j == 0)
            return 1;

        if (memo[i][j] != -1)
            return memo[i][j];

        memo[i][j] =
                dfs(i - 1, j)
              + dfs(i, j - 1);

        return memo[i][j];
    }
}
```

---

## Tabulation

```java
class Solution {

    public int uniquePathsWithObstacles(int[][] grid) {

        int m = grid.length;
        int n = grid[0].length;

        int[][] dp = new int[m][n];

        if (grid[0][0] == 1)
            return 0;

        dp[0][0] = 1;

        for (int i = 0; i < m; i++) {

            for (int j = 0; j < n; j++) {

                if (grid[i][j] == 1) {

                    dp[i][j] = 0;

                    continue;
                }

                if (i > 0)
                    dp[i][j] += dp[i - 1][j];

                if (j > 0)
                    dp[i][j] += dp[i][j - 1];
            }
        }

        return dp[m - 1][n - 1];
    }
}
```

---

## Complexity

| Approach | Time | Space |
|-----------|------|--------|
| Memoization | O(MN) | O(MN) |
| Tabulation | O(MN) | O(MN) |

---

## Common Pitfalls

- Starting cell blocked
- Ending cell blocked
- Forgetting to zero obstacle cells

---

## Interview Follow-ups

- Variable obstacle costs
- Dynamic obstacles
- Recover actual path
- Min obstacle removals

---

## LLM-Proof Variant

Some obstacles disappear after K steps.

State becomes

```
dp[row][col][k]
```

---

# 3. Minimum Path Sum

- **Difficulty:** Medium
- **Pattern:** Cost Grid DP
- **LeetCode:** https://leetcode.com/problems/minimum-path-sum/
- **Asked By:** Google, Amazon, Meta, Microsoft
- **LLM-Proof Variant:** Yes

---

## Problem Statement

Each cell has a cost.

Find the minimum sum from

```
Top Left

↓

Bottom Right
```

Moving only

- Right
- Down

---

## Why It's Asked

Interviewers test whether candidates can recognize when a DP state represents an **optimization objective** instead of a **count**.

---

## Intuition

Instead of counting paths,

store

```
minimum cost
```

to reach every cell.

---

## DP State

```
dp[i][j]

=

minimum cost
to reach
(i,j)
```

---

## Transition

```
dp[i][j]

=
grid[i][j]

+

min(

top,

left

)
```

---

## Example

Grid

```
1 3 1

1 5 1

4 2 1
```

DP

```
1 4 5

2 7 6

6 8 7
```

Answer

```
7
```

---

## Memoization

```java
class Solution {

    private int[][] memo;
    private int[][] grid;

    public int minPathSum(int[][] grid) {

        this.grid = grid;

        int m = grid.length;
        int n = grid[0].length;

        memo = new int[m][n];

        for (int[] row : memo)
            Arrays.fill(row, -1);

        return dfs(m - 1, n - 1);
    }

    private int dfs(int i, int j) {

        if (i == 0 && j == 0)
            return grid[0][0];

        if (i < 0 || j < 0)
            return Integer.MAX_VALUE / 2;

        if (memo[i][j] != -1)
            return memo[i][j];

        memo[i][j] = grid[i][j] + Math.min(
                dfs(i - 1, j),
                dfs(i, j - 1));

        return memo[i][j];
    }
}
```

---

## Tabulation

```java
class Solution {

    public int minPathSum(int[][] grid) {

        int m = grid.length;
        int n = grid[0].length;

        int[][] dp = new int[m][n];

        dp[0][0] = grid[0][0];

        for (int i = 1; i < m; i++)
            dp[i][0] = dp[i - 1][0] + grid[i][0];

        for (int j = 1; j < n; j++)
            dp[0][j] = dp[0][j - 1] + grid[0][j];

        for (int i = 1; i < m; i++) {

            for (int j = 1; j < n; j++) {

                dp[i][j] =
                        grid[i][j]
                      + Math.min(dp[i - 1][j],
                                 dp[i][j - 1]);
            }
        }

        return dp[m - 1][n - 1];
    }
}
```

---

## Space Optimization

```java
class Solution {

    public int minPathSum(int[][] grid) {

        int m = grid.length;
        int n = grid[0].length;

        int[] dp = new int[n];

        dp[0] = grid[0][0];

        for (int j = 1; j < n; j++)
            dp[j] = dp[j - 1] + grid[0][j];

        for (int i = 1; i < m; i++) {

            dp[0] += grid[i][0];

            for (int j = 1; j < n; j++) {

                dp[j] = grid[i][j] +
                        Math.min(dp[j],
                                 dp[j - 1]);
            }
        }

        return dp[n - 1];
    }
}
```

---

## Complexity

| Approach | Time | Space |
|-----------|------|--------|
| Memoization | O(MN) | O(MN) |
| Tabulation | O(MN) | O(MN) |
| Space Optimized | O(MN) | O(N) |

---

## Common Pitfalls

- Integer overflow in recursive invalid states.
- Incorrect initialization of first row and first column.
- Forgetting that this is a **minimization DP**, not counting DP.

---

## Interview Follow-ups

- Recover the actual minimum-cost path.
- Allow diagonal movement.
- Maximum path sum instead of minimum.
- Count how many minimum-cost paths exist.

---

## LLM-Proof Variant

Allow movement in four directions without revisiting a cell. Explain why ordinary grid DP no longer works and why graph algorithms (e.g., Dijkstra for non-negative weights) become necessary.

---

# 4. Triangle

- **Difficulty:** Medium
- **Pattern:** Triangular Grid DP
- **LeetCode:** https://leetcode.com/problems/triangle/
- **Asked By:** Google, Amazon, Bloomberg, Microsoft, Apple
- **LLM-Proof Variant:** Yes

---

## Problem Statement

Given a triangle of integers, start from the top and move to the bottom.

From position `(i, j)` you may move only to:

- `(i + 1, j)`
- `(i + 1, j + 1)`

Return the minimum path sum.

---

## Why Interviewers Ask It

Although the structure is not rectangular, the underlying recurrence remains a 2-D DP. This tests whether candidates can define states independent of the data layout.

---

## Intuition

Every position chooses the cheaper of its two children.

```
      2
     / \
    3   4
   / \ / \
  6  5 7  8
```

Recurrence:

```
current +
min(leftChild,
    rightChild)
```

---

## DP State

```
dp[i][j]

=

minimum cost

from

(i,j)

to bottom
```

Unlike previous grid problems, computing **bottom-up** is more natural.

---

## Transition

```
dp[i][j]

=

triangle[i][j]

+

min(

dp[i+1][j],

dp[i+1][j+1]

)
```

---

## DP Visualization

Triangle

```
      2
     3 4
    6 5 7
   4 1 8 3
```

Bottom row initializes the DP.

```
4 1 8 3
```

Next row

```
7 6 10
```

Next

```
9 10
```

Top

```
11
```

Answer

```
11
```

---

## Memoization

```java
class Solution {

    private List<List<Integer>> triangle;
    private Integer[][] memo;

    public int minimumTotal(List<List<Integer>> triangle) {

        this.triangle = triangle;
        int n = triangle.size();

        memo = new Integer[n][n];

        return dfs(0, 0);
    }

    private int dfs(int row, int col) {

        if (row == triangle.size() - 1)
            return triangle.get(row).get(col);

        if (memo[row][col] != null)
            return memo[row][col];

        int left = dfs(row + 1, col);
        int right = dfs(row + 1, col + 1);

        memo[row][col] =
                triangle.get(row).get(col)
                + Math.min(left, right);

        return memo[row][col];
    }
}
```

---

## Bottom-Up DP

```java
class Solution {

    public int minimumTotal(List<List<Integer>> triangle) {

        int n = triangle.size();

        int[][] dp = new int[n][n];

        for (int j = 0; j < n; j++)
            dp[n - 1][j] =
                    triangle.get(n - 1).get(j);

        for (int i = n - 2; i >= 0; i--) {

            for (int j = 0; j <= i; j++) {

                dp[i][j] =
                        triangle.get(i).get(j)
                      + Math.min(dp[i + 1][j],
                                 dp[i + 1][j + 1]);
            }
        }

        return dp[0][0];
    }
}
```

---

## Space Optimized

```java
class Solution {

    public int minimumTotal(List<List<Integer>> triangle) {

        int n = triangle.size();

        int[] dp = new int[n];

        for (int j = 0; j < n; j++)
            dp[j] = triangle.get(n - 1).get(j);

        for (int i = n - 2; i >= 0; i--) {

            for (int j = 0; j <= i; j++) {

                dp[j] =
                        triangle.get(i).get(j)
                      + Math.min(dp[j],
                                 dp[j + 1]);
            }
        }

        return dp[0];
    }
}
```

---

## Complexity

| Approach | Time | Space |
|-----------|------|--------|
| Memoization | O(N²) | O(N²) |
| Bottom-Up | O(N²) | O(N²) |
| Space Optimized | O(N²) | O(N) |

---

## Common Pitfalls

- Iterating top-down for tabulation instead of bottom-up.
- Using a rectangular traversal for a triangular structure.
- Incorrect loop boundary (`j <= i`).

---

## Interview Follow-ups

- Return the actual path instead of only the minimum sum.
- Compute the maximum path sum.
- Allow skipping one row exactly once.
- Count the number of minimum-sum paths.

---

## LLM-Proof Variant

Each move may go to:

- `(i + 1, j - 1)`
- `(i + 1, j)`
- `(i + 1, j + 1)`

Derive the new recurrence and explain how the boundary conditions change.

---

# Part 1 Summary

| Problem | Pattern | Core Recurrence |
|----------|---------|-----------------|
| Unique Paths | Grid Counting | `dp[i][j] = top + left` |
| Unique Paths II | Grid with Obstacles | `obstacle ? 0 : top + left` |
| Minimum Path Sum | Cost Grid | `grid + min(top, left)` |
| Triangle | Triangular DP | `value + min(down, diagonal)` |

**Patterns Mastered**

- Grid traversal DP
- Counting DP
- Cost minimization DP
- Obstacle handling
- Bottom-up state transitions
- 2-D → 1-D space optimization
- Recursive memoization vs. iterative tabulation


# 2-D Dynamic Programming — FAANG Interview Guide (Part 2)

> Covered Problems:
>
> 5. Minimum Falling Path Sum
> 6. Longest Common Subsequence
> 7. Longest Palindromic Subsequence
> 8. Edit Distance

---

# 5. Minimum Falling Path Sum

- **Difficulty:** Medium
- **Pattern:** Grid DP (Three-Direction Transition)
- **LeetCode:** https://leetcode.com/problems/minimum-falling-path-sum/
- **Asked By:** Google, Amazon, Apple, Microsoft
- **LLM-Proof Variant:** Yes

---

## Problem Statement

Given an `n × n` matrix, start from **any cell in the first row**.

At every step you may move to:

- Down
- Down-left
- Down-right

Return the minimum falling path sum.

---

## Why Interviewers Ask It

Unlike previous grid problems where movement begins from one fixed cell, this problem tests whether candidates can:

- Handle multiple starting states
- Manage boundary conditions correctly
- Generalize grid DP transitions

---

## Intuition

For every cell,

```
      ↑
   ↖  ↑  ↗
```

The current value depends on the cheapest of the three possible parent cells.

---

## DP State

```
dp[i][j]

=

Minimum cost
to reach
cell (i,j)
```

---

## Transition

```
dp[i][j]

=

matrix[i][j]

+

min(

top,

top-left,

top-right

)
```

---

## DP Visualization

Matrix

```
2 1 3
6 5 4
7 8 9
```

DP

```
2 1 3
7 6 5
13 13 14
```

Answer

```
13
```

---

## Memoization

```java
class Solution {

    private int[][] matrix;
    private Integer[][] memo;
    private int n;

    public int minFallingPathSum(int[][] matrix) {

        this.matrix = matrix;
        n = matrix.length;

        memo = new Integer[n][n];

        int ans = Integer.MAX_VALUE;

        for (int j = 0; j < n; j++)
            ans = Math.min(ans, dfs(n - 1, j));

        return ans;
    }

    private int dfs(int i, int j) {

        if (j < 0 || j >= n)
            return Integer.MAX_VALUE / 2;

        if (i == 0)
            return matrix[0][j];

        if (memo[i][j] != null)
            return memo[i][j];

        memo[i][j] =
                matrix[i][j] +
                Math.min(
                    dfs(i - 1, j),
                    Math.min(
                        dfs(i - 1, j - 1),
                        dfs(i - 1, j + 1)
                    )
                );

        return memo[i][j];
    }
}
```

---

## Bottom-Up DP

```java
class Solution {

    public int minFallingPathSum(int[][] matrix) {

        int n = matrix.length;

        int[][] dp = new int[n][n];

        for (int j = 0; j < n; j++)
            dp[0][j] = matrix[0][j];

        for (int i = 1; i < n; i++) {

            for (int j = 0; j < n; j++) {

                int up = dp[i - 1][j];

                int left = j > 0
                        ? dp[i - 1][j - 1]
                        : Integer.MAX_VALUE / 2;

                int right = j + 1 < n
                        ? dp[i - 1][j + 1]
                        : Integer.MAX_VALUE / 2;

                dp[i][j] =
                        matrix[i][j]
                      + Math.min(up,
                          Math.min(left, right));
            }
        }

        int ans = Integer.MAX_VALUE;

        for (int value : dp[n - 1])
            ans = Math.min(ans, value);

        return ans;
    }
}
```

---

## Space Optimized

```java
class Solution {

    public int minFallingPathSum(int[][] matrix) {

        int n = matrix.length;

        int[] prev = matrix[0].clone();

        for (int i = 1; i < n; i++) {

            int[] curr = new int[n];

            for (int j = 0; j < n; j++) {

                int up = prev[j];

                int left = j > 0
                        ? prev[j - 1]
                        : Integer.MAX_VALUE / 2;

                int right = j + 1 < n
                        ? prev[j + 1]
                        : Integer.MAX_VALUE / 2;

                curr[j] =
                        matrix[i][j]
                      + Math.min(up,
                          Math.min(left, right));
            }

            prev = curr;
        }

        int ans = Integer.MAX_VALUE;

        for (int value : prev)
            ans = Math.min(ans, value);

        return ans;
    }
}
```

---

## Complexity

| Approach | Time | Space |
|----------|------|--------|
| Memoization | O(N²) | O(N²) |
| Tabulation | O(N²) | O(N²) |
| Space Optimized | O(N²) | O(N) |

---

## Common Pitfalls

- Ignoring boundary columns.
- Starting only from `(0,0)` instead of every first-row cell.
- Forgetting to scan the final row for the answer.

---

## Interview Follow-ups

- Maximum falling path.
- Falling path with blocked cells.
- Recover the actual path.
- Circular column movement.

---

## LLM-Proof Variant

Allow jumps of up to **K columns** in the next row.

How does the transition change?

---

# 6. Longest Common Subsequence

- **Difficulty:** Medium
- **Pattern:** String DP
- **LeetCode:** https://leetcode.com/problems/longest-common-subsequence/
- **Asked By:** Google, Amazon, Meta, Microsoft, Apple
- **LLM-Proof Variant:** Extremely Common

---

## Problem Statement

Given two strings,

find the length of their longest common subsequence.

Characters do **not** need to be contiguous.

---

## Why Interviewers Ask It

This is arguably the most important 2-D DP interview problem.

Interviewers evaluate whether candidates can derive DP states from two independent variables.

---

## Intuition

Suppose

```
text1 = abcde

text2 = ace
```

Compare the last characters.

If equal,

```
take both
```

Otherwise,

remove one character from either string.

---

## DP State

```
dp[i][j]

=

LCS length

between

text1[0...i)

and

text2[0...j)
```

---

## Transition

If equal

```
dp[i][j]

=

1

+

dp[i-1][j-1]
```

Else

```
max(

top,

left

)
```

---

## DP Visualization

Example

```
abc

ac
```

```
      "" a c

""    0 0 0

a     0 1 1

b     0 1 1

c     0 1 2
```

Answer

```
2
```

---

## Memoization

```java
class Solution {

    private Integer[][] memo;
    private String s1, s2;

    public int longestCommonSubsequence(String text1, String text2) {

        s1 = text1;
        s2 = text2;

        memo = new Integer[s1.length()][s2.length()];

        return dfs(0, 0);
    }

    private int dfs(int i, int j) {

        if (i == s1.length() || j == s2.length())
            return 0;

        if (memo[i][j] != null)
            return memo[i][j];

        if (s1.charAt(i) == s2.charAt(j))

            return memo[i][j] =
                    1 + dfs(i + 1, j + 1);

        return memo[i][j] =
                Math.max(
                        dfs(i + 1, j),
                        dfs(i, j + 1)
                );
    }
}
```

---

## Bottom-Up DP

```java
class Solution {

    public int longestCommonSubsequence(String a, String b) {

        int m = a.length();
        int n = b.length();

        int[][] dp = new int[m + 1][n + 1];

        for (int i = 1; i <= m; i++) {

            for (int j = 1; j <= n; j++) {

                if (a.charAt(i - 1) == b.charAt(j - 1))

                    dp[i][j] =
                            dp[i - 1][j - 1] + 1;

                else

                    dp[i][j] =
                            Math.max(dp[i - 1][j],
                                     dp[i][j - 1]);
            }
        }

        return dp[m][n];
    }
}
```

---

## Space Optimization

```java
class Solution {

    public int longestCommonSubsequence(String a, String b) {

        int n = b.length();

        int[] prev = new int[n + 1];

        for (int i = 1; i <= a.length(); i++) {

            int[] curr = new int[n + 1];

            for (int j = 1; j <= n; j++) {

                if (a.charAt(i - 1) == b.charAt(j - 1))

                    curr[j] =
                            prev[j - 1] + 1;

                else

                    curr[j] =
                            Math.max(prev[j],
                                     curr[j - 1]);
            }

            prev = curr;
        }

        return prev[n];
    }
}
```

---

## Complexity

| Approach | Time | Space |
|----------|------|--------|
| Memoization | O(MN) | O(MN) |
| Tabulation | O(MN) | O(MN) |
| Space Optimized | O(Min(M,N)) | O(Min(M,N)) |

---

## Common Pitfalls

- Confusing subsequence with substring.
- Incorrect indexing.
- Missing extra row and column.

---

## Interview Follow-ups

- Print the LCS.
- Count all LCSs.
- Lexicographically smallest LCS.
- Multiple strings LCS.

---

## LLM-Proof Variant

Return one valid LCS instead of only its length.

---

# 7. Longest Palindromic Subsequence

- **Difficulty:** Medium
- **Pattern:** Interval DP / String DP
- **LeetCode:** https://leetcode.com/problems/longest-palindromic-subsequence/
- **Asked By:** Google, Amazon, Microsoft
- **LLM-Proof Variant:** Yes

---

## Problem Statement

Return the length of the longest palindromic subsequence.

---

## Interview Goal

Candidates must realize that the interval

```
(i,j)
```

forms the DP state.

---

## DP State

```
dp[i][j]

=

Longest palindrome

inside

i...j
```

---

## Transition

If

```
s[i]==s[j]
```

```
2 + dp[i+1][j-1]
```

Otherwise

```
max(

dp[i+1][j],

dp[i][j-1]

)
```

---

## Visualization

```
bbbab
```

Fill intervals

```
Length 1

↓

Length 2

↓

Length 3

↓

...

Length n
```

---

## Bottom-Up Java

```java
class Solution {

    public int longestPalindromeSubseq(String s) {

        int n = s.length();

        int[][] dp = new int[n][n];

        for (int i = 0; i < n; i++)
            dp[i][i] = 1;

        for (int len = 2; len <= n; len++) {

            for (int i = 0; i + len - 1 < n; i++) {

                int j = i + len - 1;

                if (s.charAt(i) == s.charAt(j)) {

                    if (len == 2)
                        dp[i][j] = 2;
                    else
                        dp[i][j] =
                                2 + dp[i + 1][j - 1];

                } else {

                    dp[i][j] =
                            Math.max(
                                    dp[i + 1][j],
                                    dp[i][j - 1]
                            );
                }
            }
        }

        return dp[0][n - 1];
    }
}
```

---

## Complexity

| Time | Space |
|------|--------|
| O(N²) | O(N²) |

---

## Common Pitfalls

- Wrong iteration order.
- Forgetting interval DP.
- Missing length-2 case.

---

## Interview Follow-ups

- Print the palindrome.
- Count palindromic subsequences.
- Longest palindromic substring.

---

## LLM-Proof Variant

Allow changing one character anywhere.

How does the state change?

---

# 8. Edit Distance

- **Difficulty:** Medium
- **Pattern:** String Transformation DP
- **LeetCode:** https://leetcode.com/problems/edit-distance/
- **Asked By:** Google, Amazon, Microsoft, Meta, Apple
- **LLM-Proof Variant:** Extremely Common

---

## Problem Statement

Convert one string into another using:

- Insert
- Delete
- Replace

Return the minimum number of operations.

---

## Why Interviewers Ask It

Tests whether candidates can model multiple operations in a single recurrence.

---

## DP State

```
dp[i][j]

=

Minimum edits

to convert

word1[0...i)

into

word2[0...j)
```

---

## Transition

Characters equal

```
dp[i-1][j-1]
```

Otherwise

```
1 +

min(

insert,

delete,

replace

)
```

---

## DP Visualization

```
horse

ros
```

```
      r o s

""    0 1 2 3

h     1

o     2

r     3

s     4

e     5
```

---

## Bottom-Up Java

```java
class Solution {

    public int minDistance(String a, String b) {

        int m = a.length();
        int n = b.length();

        int[][] dp = new int[m + 1][n + 1];

        for (int i = 0; i <= m; i++)
            dp[i][0] = i;

        for (int j = 0; j <= n; j++)
            dp[0][j] = j;

        for (int i = 1; i <= m; i++) {

            for (int j = 1; j <= n; j++) {

                if (a.charAt(i - 1) == b.charAt(j - 1))

                    dp[i][j] =
                            dp[i - 1][j - 1];

                else {

                    dp[i][j] =
                            1 + Math.min(

                                    dp[i - 1][j - 1],

                                    Math.min(
                                            dp[i - 1][j],
                                            dp[i][j - 1]
                                    )
                            );
                }
            }
        }

        return dp[m][n];
    }
}
```

---

## Space Optimization

```java
class Solution {

    public int minDistance(String a, String b) {

        int n = b.length();

        int[] prev = new int[n + 1];

        for (int j = 0; j <= n; j++)
            prev[j] = j;

        for (int i = 1; i <= a.length(); i++) {

            int[] curr = new int[n + 1];

            curr[0] = i;

            for (int j = 1; j <= n; j++) {

                if (a.charAt(i - 1) == b.charAt(j - 1))

                    curr[j] = prev[j - 1];

                else {

                    curr[j] =
                            1 + Math.min(

                                    prev[j - 1],

                                    Math.min(
                                            prev[j],
                                            curr[j - 1]
                                    )
                            );
                }
            }

            prev = curr;
        }

        return prev[n];
    }
}
```

---

## Complexity

| Approach | Time | Space |
|----------|------|--------|
| Tabulation | O(MN) | O(MN) |
| Space Optimized | O(MN) | O(N) |

---

## Common Pitfalls

- Forgetting initialization of the first row and first column.
- Mixing insert and delete transitions.
- Incorrect indexing.

---

## Interview Follow-ups

- Recover the edit sequence.
- Weighted edit operations.
- Damerau-Levenshtein distance (adjacent swap allowed).
- Edit distance with wildcard characters.

---

## LLM-Proof Variant

Only **K** edits are allowed.

Determine whether the transformation is possible without computing the full DP table.

---

# Part 2 Summary

| Problem | Pattern | Core Transition |
|----------|---------|-----------------|
| Minimum Falling Path Sum | Grid DP | `value + min(up, up-left, up-right)` |
| Longest Common Subsequence | Two-String DP | `match ? 1+diag : max(up,left)` |
| Longest Palindromic Subsequence | Interval DP | `match ? 2+inside : max(left,right)` |
| Edit Distance | String Transformation | `match ? diag : 1+min(insert, delete, replace)` |

## Patterns Mastered

- Multi-direction grid DP
- Two-string dynamic programming
- Interval DP over substrings
- String transformation DP
- State definitions involving two indices
- Bottom-up interval traversal
- Space optimization for string DP

# 2-D Dynamic Programming — FAANG Interview Guide (Part 3)

> Covered Problems:
>
> 9. Distinct Subsequences
> 10. Interleaving String
> 11. Stone Game
> 12. Cherry Pickup II

---

# 9. Distinct Subsequences

- **Difficulty:** Hard
- **Pattern:** Two-String DP (Counting)
- **LeetCode:** https://leetcode.com/problems/distinct-subsequences/
- **Asked By:** Google, Amazon, Meta, Microsoft, Apple
- **LLM-Proof Variant:** Very Common

---

## Problem Statement

Given two strings:

- `s` (source)
- `t` (target)

Return the number of distinct subsequences of `s` that equal `t`.

---

## Why Interviewers Ask It

Unlike **Longest Common Subsequence**, this is a **counting DP** rather than an optimization DP. It tests whether candidates can distinguish between maximizing and counting recurrences.

---

## Intuition

At every character of `s`:

- Skip it.
- Use it (only if it matches the current character of `t`).

---

## DP State

```
dp[i][j]

=

Number of ways

to form

t[0...j)

using

s[0...i)
```

---

## Transition

If characters match:

```
dp[i][j]

=

dp[i-1][j]

+

dp[i-1][j-1]
```

Otherwise:

```
dp[i][j]

=

dp[i-1][j]
```

---

## DP Visualization

Example

```
s = rabbbit

t = rabbit
```

Every matching character branches into:

```
Take

OR

Skip
```

---

## Bottom-Up Java

```java
class Solution {

    public int numDistinct(String s, String t) {

        int m = s.length();
        int n = t.length();

        long[][] dp = new long[m + 1][n + 1];

        for (int i = 0; i <= m; i++)
            dp[i][0] = 1;

        for (int i = 1; i <= m; i++) {

            for (int j = 1; j <= n; j++) {

                dp[i][j] = dp[i - 1][j];

                if (s.charAt(i - 1) == t.charAt(j - 1))

                    dp[i][j] += dp[i - 1][j - 1];
            }
        }

        return (int) dp[m][n];
    }
}
```

---

## Memoization

```java
class Solution {

    private Long[][] memo;
    private String s, t;

    public int numDistinct(String s, String t) {

        this.s = s;
        this.t = t;

        memo = new Long[s.length()][t.length()];

        return (int) dfs(0, 0);
    }

    private long dfs(int i, int j) {

        if (j == t.length())
            return 1;

        if (i == s.length())
            return 0;

        if (memo[i][j] != null)
            return memo[i][j];

        long ans = dfs(i + 1, j);

        if (s.charAt(i) == t.charAt(j))
            ans += dfs(i + 1, j + 1);

        return memo[i][j] = ans;
    }
}
```

---

## Complexity

| Approach | Time | Space |
|----------|------|--------|
| Memoization | O(MN) | O(MN) |
| Tabulation | O(MN) | O(MN) |

---

## Common Pitfalls

- Using `int` instead of `long`.
- Incorrect base case for empty target.
- Confusing with LCS.

---

## Interview Follow-ups

- Return one valid subsequence.
- Enumerate all subsequences.
- Modulo arithmetic for large answers.

---

## LLM-Proof Variant

What changes if characters may be replaced once?

---

# 10. Interleaving String

- **Difficulty:** Medium
- **Pattern:** State-Space DP
- **LeetCode:** https://leetcode.com/problems/interleaving-string/
- **Asked By:** Google, Amazon, Microsoft, Apple
- **LLM-Proof Variant:** Yes

---

## Problem Statement

Determine whether `s3` is formed by interleaving:

- `s1`
- `s2`

while preserving the relative order of characters in each string.

---

## Why Interviewers Ask It

This problem checks whether candidates can reason about **multiple dimensions of state** rather than just one sequence.

---

## DP State

```
dp[i][j]

=

Can

s1[0...i)

and

s2[0...j)

form

s3[0...i+j)
```

---

## Transition

Take character from `s1`

OR

Take character from `s2`

```
dp[i][j]

=

(fromTop)

OR

(fromLeft)
```

---

## Visualization

```
s1

↓

s2 →

Every state

(i,j)

represents

prefix length

i+j
```

---

## Bottom-Up Java

```java
class Solution {

    public boolean isInterleave(String s1,
                                String s2,
                                String s3) {

        if (s1.length() + s2.length() != s3.length())
            return false;

        int m = s1.length();
        int n = s2.length();

        boolean[][] dp = new boolean[m + 1][n + 1];

        dp[0][0] = true;

        for (int i = 0; i <= m; i++) {

            for (int j = 0; j <= n; j++) {

                if (i > 0)

                    dp[i][j] |=
                            dp[i - 1][j]
                            &&
                            s1.charAt(i - 1)
                            ==
                            s3.charAt(i + j - 1);

                if (j > 0)

                    dp[i][j] |=
                            dp[i][j - 1]
                            &&
                            s2.charAt(j - 1)
                            ==
                            s3.charAt(i + j - 1);
            }
        }

        return dp[m][n];
    }
}
```

---

## Memoization

```java
class Solution {

    private Boolean[][] memo;
    private String s1, s2, s3;

    public boolean isInterleave(String a,
                                String b,
                                String c) {

        s1 = a;
        s2 = b;
        s3 = c;

        if (a.length() + b.length() != c.length())
            return false;

        memo = new Boolean[a.length() + 1][b.length() + 1];

        return dfs(0, 0);
    }

    private boolean dfs(int i, int j) {

        if (i == s1.length() &&
            j == s2.length())
            return true;

        if (memo[i][j] != null)
            return memo[i][j];

        boolean ans = false;

        if (i < s1.length() &&
            s1.charAt(i) ==
            s3.charAt(i + j))

            ans |= dfs(i + 1, j);

        if (j < s2.length() &&
            s2.charAt(j) ==
            s3.charAt(i + j))

            ans |= dfs(i, j + 1);

        return memo[i][j] = ans;
    }
}
```

---

## Complexity

| Approach | Time | Space |
|----------|------|--------|
| Memoization | O(MN) | O(MN) |
| Tabulation | O(MN) | O(MN) |

---

## Common Pitfalls

- Forgetting length validation.
- Incorrect `i+j-1` indexing.
- Using greedy matching.

---

## Interview Follow-ups

- Count all interleavings.
- Print one valid interleaving.
- Three-string interleaving.

---

## LLM-Proof Variant

Characters may be swapped once inside either source string.

---

# 11. Stone Game

- **Difficulty:** Medium
- **Pattern:** Game DP / Interval DP
- **LeetCode:** https://leetcode.com/problems/stone-game/
- **Asked By:** Google, Amazon, Microsoft, Jane Street
- **LLM-Proof Variant:** Yes

---

## Problem Statement

Two players alternately take stones from either end of the array.

Both play optimally.

Determine whether Alice wins.

---

## Why Interviewers Ask It

This introduces **Game Theory DP**, where every move assumes the opponent also plays optimally.

---

## DP State

```
dp[i][j]

=

Maximum score difference

current player

can achieve

on interval

[i,j]
```

---

## Transition

Choose left

```
pile[i]

-

dp[i+1][j]
```

Choose right

```
pile[j]

-

dp[i][j-1]
```

Take maximum.

---

## Visualization

```
8 15 3 7

↓

Choose

Left

or

Right

↓

Remaining interval
```

---

## Bottom-Up Java

```java
class Solution {

    public boolean stoneGame(int[] piles) {

        int n = piles.length;

        int[][] dp = new int[n][n];

        for (int i = 0; i < n; i++)
            dp[i][i] = piles[i];

        for (int len = 2; len <= n; len++) {

            for (int i = 0;
                 i + len - 1 < n;
                 i++) {

                int j = i + len - 1;

                dp[i][j] =
                        Math.max(

                                piles[i]
                                - dp[i + 1][j],

                                piles[j]
                                - dp[i][j - 1]
                        );
            }
        }

        return dp[0][n - 1] > 0;
    }
}
```

---

## Complexity

| Time | Space |
|------|--------|
| O(N²) | O(N²) |

---

## Common Pitfalls

- Maximizing score instead of score difference.
- Ignoring optimal opponent moves.
- Wrong interval traversal.

---

## Interview Follow-ups

- Return Alice's maximum score.
- Three-player version.
- Variable number of picks.

---

## LLM-Proof Variant

Players may remove one or two stones from either side.

How does the recurrence expand?

---

# 12. Cherry Pickup II

- **Difficulty:** Hard
- **Pattern:** Multi-Agent Grid DP
- **LeetCode:** https://leetcode.com/problems/cherry-pickup-ii/
- **Asked By:** Google, Amazon, Meta
- **LLM-Proof Variant:** Very Common

---

## Problem Statement

Two robots start at:

```
Robot A

(0,0)

Robot B

(0,n-1)
```

Both move one row downward each step.

Each may move:

- Left diagonal
- Down
- Right diagonal

Collect the maximum cherries.

---

## Why Interviewers Ask It

This is a classic example of **state explosion** in DP.

A naïve solution is exponential.

---

## DP State

```
dp[row][col1][col2]

=

Maximum cherries

starting from

current row

with robots

at

(col1,col2)
```

---

## Transition

Try all

```
3 × 3 = 9

possible

move combinations
```

---

## Visualization

```
Row r

A      B

↓

↓

9 possible transitions

↓

Row r+1
```

---

## Memoization Java

```java
class Solution {

    private int[][] grid;
    private Integer[][][] memo;
    private int rows, cols;

    public int cherryPickup(int[][] grid) {

        this.grid = grid;

        rows = grid.length;
        cols = grid[0].length;

        memo = new Integer[rows][cols][cols];

        return dfs(0, 0, cols - 1);
    }

    private int dfs(int row,
                    int c1,
                    int c2) {

        if (c1 < 0 || c1 >= cols ||
            c2 < 0 || c2 >= cols)
            return Integer.MIN_VALUE;

        if (row == rows)
            return 0;

        if (memo[row][c1][c2] != null)
            return memo[row][c1][c2];

        int cherries =
                grid[row][c1];

        if (c1 != c2)
            cherries += grid[row][c2];

        int best = 0;

        for (int d1 = -1; d1 <= 1; d1++) {

            for (int d2 = -1; d2 <= 1; d2++) {

                best = Math.max(best,
                        dfs(row + 1,
                            c1 + d1,
                            c2 + d2));
            }
        }

        memo[row][c1][c2] =
                cherries + best;

        return memo[row][c1][c2];
    }
}
```

---

## Complexity

| Time | Space |
|------|--------|
| O(R × C² × 9) | O(R × C²) |

---

## Common Pitfalls

- Double counting when both robots occupy the same cell.
- Missing one of the nine transitions.
- Incorrect boundary checks.

---

## Interview Follow-ups

- Three robots.
- Obstacles.
- Negative cherries.
- Recover both robot paths.

---

## LLM-Proof Variant

Robots may skip one row exactly once.

How should the DP state be extended?

---

# Part 3 Summary

| Problem | Pattern | Core Transition |
|----------|---------|-----------------|
| Distinct Subsequences | Counting DP | `match ? skip + take : skip` |
| Interleaving String | State DP | `top OR left` |
| Stone Game | Interval/Game DP | `max(left-dp, right-dp)` |
| Cherry Pickup II | 3D DP | `current + max(9 next states)` |

## Patterns Mastered

- Counting DP on two strings
- State-space exploration
- Game theory DP
- Interval optimization
- Multi-agent dynamic programming
- Three-dimensional DP states
- State explosion handling with memoization

# 2-D Dynamic Programming — FAANG Interview Guide (Part 4)

> Covered Problems:
>
> 13. Burst Balloons
> 14. Minimum Score Triangulation of Polygon
> 15. Strange Printer
>
> Includes:
>
> - Final Pattern Summary
> - Company-wise Revision Table
> - 2-D DP Interview Cheat Sheet

---

# 13. Burst Balloons

- **Difficulty:** Hard
- **Pattern:** Interval DP (Matrix Chain Multiplication Pattern)
- **LeetCode:** https://leetcode.com/problems/burst-balloons/
- **Asked By:** Google, Amazon, Meta, Microsoft
- **LLM-Proof Variant:** Extremely Common

---

## Problem Statement

You are given `n` balloons.

Bursting balloon `k` earns:

```
nums[left]
×

nums[k]

×

nums[right]
```

where `left` and `right` are the nearest remaining balloons.

Return the maximum coins obtainable.

---

## Why Interviewers Ask It

Most candidates attempt to decide **which balloon to burst first**, which leads to complicated state transitions.

The key insight is to choose **the last balloon burst** in an interval.

This is the classic **Matrix Chain Multiplication (MCM)** DP pattern.

---

## Intuition

Instead of

```
First Balloon
```

think

```
Last Balloon
```

For interval

```
(i...j)

Try

k

as last balloon.
```

---

## DP State

```
dp[i][j]

=

Maximum coins

obtained by

bursting

(i...j)
```

---

## Transition

```
dp[i][j]

=

max(

left interval

+

right interval

+

last burst gain

)
```

Mathematically,

```
dp[i][j]

=

max over k

dp[i][k-1]

+

dp[k+1][j]

+

nums[i-1]

×

nums[k]

×

nums[j+1]
```

---

## Interval Visualization

```
1 3 1 5 8 1

      k

<----->

interval
```

Every balloon is tried as the **last** one.

---

## Bottom-Up Java

```java
class Solution {

    public int maxCoins(int[] nums) {

        int n = nums.length;

        int[] arr = new int[n + 2];

        arr[0] = 1;
        arr[n + 1] = 1;

        for (int i = 0; i < n; i++)
            arr[i + 1] = nums[i];

        int[][] dp = new int[n + 2][n + 2];

        for (int len = 1; len <= n; len++) {

            for (int left = 1;
                 left + len - 1 <= n;
                 left++) {

                int right = left + len - 1;

                for (int k = left;
                     k <= right;
                     k++) {

                    dp[left][right] =
                            Math.max(

                                    dp[left][right],

                                    dp[left][k - 1]
                                            + dp[k + 1][right]
                                            + arr[left - 1]
                                            * arr[k]
                                            * arr[right + 1]
                            );
                }
            }
        }

        return dp[1][n];
    }
}
```

---

## Complexity

| Time | Space |
|------|--------|
| O(N³) | O(N²) |

---

## Common Pitfalls

- Bursting first instead of last.
- Forgetting virtual balloons (`1`) at both ends.
- Incorrect interval traversal.

---

## Interview Follow-ups

- Return bursting order.
- Minimize coins instead.
- Circular balloons.

---

## LLM-Proof Variant

Allow bursting **two adjacent balloons together**.

How would the recurrence change?

---

# 14. Minimum Score Triangulation of Polygon

- **Difficulty:** Medium
- **Pattern:** Interval DP / Matrix Chain Multiplication
- **LeetCode:** https://leetcode.com/problems/minimum-score-triangulation-of-polygon/
- **Asked By:** Google, Amazon, Bloomberg
- **LLM-Proof Variant:** Yes

---

## Problem Statement

A convex polygon has values on its vertices.

Triangulate the polygon.

Cost of one triangle:

```
a × b × c
```

Return the minimum total cost.

---

## Why Interviewers Ask It

This is structurally identical to **Matrix Chain Multiplication**.

Interviewers check whether candidates recognize patterns instead of memorizing individual problems.

---

## DP State

```
dp[i][j]

=

Minimum score

to triangulate

polygon

between

i and j
```

---

## Transition

Choose every

```
k

between

i

and

j
```

```
left

+

right

+

triangle cost
```

---

## Visualization

```
i

●-------● j

 \     /

  \ k /

```

---

## Bottom-Up Java

```java
class Solution {

    public int minScoreTriangulation(int[] values) {

        int n = values.length;

        int[][] dp = new int[n][n];

        for (int len = 3; len <= n; len++) {

            for (int i = 0;
                 i + len - 1 < n;
                 i++) {

                int j = i + len - 1;

                dp[i][j] = Integer.MAX_VALUE;

                for (int k = i + 1;
                     k < j;
                     k++) {

                    dp[i][j] =
                            Math.min(

                                    dp[i][j],

                                    dp[i][k]
                                            + dp[k][j]
                                            + values[i]
                                            * values[j]
                                            * values[k]
                            );
                }
            }
        }

        return dp[0][n - 1];
    }
}
```

---

## Complexity

| Time | Space |
|------|--------|
| O(N³) | O(N²) |

---

## Common Pitfalls

- Missing interval traversal.
- Wrong triangle cost.
- Incorrect loop ordering.

---

## Interview Follow-ups

- Return triangulation.
- Maximum score triangulation.
- Polygon with weighted edges.

---

## LLM-Proof Variant

Each triangle has an additional penalty.

Update the recurrence.

---

# 15. Strange Printer

- **Difficulty:** Hard
- **Pattern:** Interval DP
- **LeetCode:** https://leetcode.com/problems/strange-printer/
- **Asked By:** Google, Meta, Amazon
- **LLM-Proof Variant:** Very Common

---

## Problem Statement

A printer can print only one repeated character during each turn.

It may overwrite existing characters.

Return the minimum number of turns required.

---

## Why Interviewers Ask It

This problem evaluates deep interval-DP reasoning.

Greedy approaches fail.

---

## DP State

```
dp[i][j]

=

Minimum turns

needed

to print

substring

i...j
```

---

## Transition

Initially,

```
1

+

dp[i+1][j]
```

If matching characters exist,

merge operations.

```
if

s[i]

==

s[k]

then

combine

intervals
```

---

## Visualization

```
aba

Print

aaa

↓

Overwrite

middle
```

Instead of

```
a

↓

b

↓

a
```

---

## Bottom-Up Java

```java
class Solution {

    public int strangePrinter(String s) {

        int n = s.length();

        if (n == 0)
            return 0;

        int[][] dp = new int[n][n];

        for (int i = n - 1; i >= 0; i--) {

            dp[i][i] = 1;

            for (int j = i + 1; j < n; j++) {

                dp[i][j] = 1 + dp[i + 1][j];

                for (int k = i + 1; k <= j; k++) {

                    if (s.charAt(i) == s.charAt(k)) {

                        dp[i][j] =
                                Math.min(

                                        dp[i][j],

                                        dp[i + 1][k - 1]
                                                + dp[k][j]
                                );
                    }
                }
            }
        }

        return dp[0][n - 1];
    }
}
```

---

## Complexity

| Time | Space |
|------|--------|
| O(N³) | O(N²) |

---

## Common Pitfalls

- Treating this as ordinary substring DP.
- Missing merge optimization.
- Wrong interval iteration.

---

## Interview Follow-ups

- Recover printing sequence.
- Multiple printers.
- Colored printer variant.

---

## LLM-Proof Variant

Printer may print **two distinct characters** in one operation.

Design the new state.

---

# 2-D DP Pattern Summary

| Pattern | Representative Problem | Core State |
|----------|------------------------|------------|
| Grid Counting | Unique Paths | `dp[row][col]` |
| Grid with Obstacles | Unique Paths II | `dp[row][col]` |
| Cost Grid | Minimum Path Sum | `dp[row][col]` |
| Triangle DP | Triangle | `dp[row][col]` |
| Multi-Direction Grid | Minimum Falling Path Sum | `dp[row][col]` |
| Two-String DP | Longest Common Subsequence | `dp[i][j]` |
| Interval String DP | Longest Palindromic Subsequence | `dp[i][j]` |
| Transformation DP | Edit Distance | `dp[i][j]` |
| Counting DP | Distinct Subsequences | `dp[i][j]` |
| State-Space DP | Interleaving String | `dp[i][j]` |
| Game DP | Stone Game | `dp[left][right]` |
| Multi-Agent DP | Cherry Pickup II | `dp[row][c1][c2]` |
| Matrix Chain Pattern | Burst Balloons | `dp[left][right]` |
| Matrix Chain Pattern | Polygon Triangulation | `dp[left][right]` |
| Advanced Interval DP | Strange Printer | `dp[left][right]` |

---

# Company-Wise Revision Table

| Company | Frequently Asked Problems |
|----------|---------------------------|
| Google | LCS, Edit Distance, Burst Balloons, Strange Printer, Cherry Pickup II |
| Amazon | Unique Paths, Minimum Path Sum, Triangle, Distinct Subsequences, Stone Game |
| Meta | LCS, Edit Distance, Cherry Pickup II, Burst Balloons |
| Microsoft | Unique Paths, LCS, Edit Distance, Stone Game |
| Apple | Minimum Falling Path Sum, LCS, Edit Distance, Triangle |

---

# Pattern Recognition Cheat Sheet

| If You See... | Think... |
|---------------|----------|
| Grid + Right/Down | Grid DP |
| Grid + Cost | Min/Max Grid DP |
| Grid + Three Directions | Multi-direction Grid DP |
| Two Strings | 2-D String DP |
| Transform String | Edit Distance Pattern |
| Count Ways | Counting DP |
| Interval `[l...r]` | Interval DP |
| Pick Ends | Game DP |
| Two Robots | Multi-Agent DP |
| Burst/Partition | Matrix Chain Multiplication Pattern |

---

# Common DP State Templates

## Grid DP

```text
dp[row][col]
```

---

## Two-String DP

```text
dp[i][j]
```

---

## Interval DP

```text
dp[left][right]
```

---

## Multi-Agent DP

```text
dp[row][col1][col2]
```

---

## Game DP

```text
dp[left][right]

=

Best score difference
```

---

# Interview Tips

## 1. Define the State First

Before writing any recurrence, ask:

- What does `dp[i][j]` represent?
- Is it a count, minimum, maximum, or boolean?

---

## 2. Identify the Transition

Typical transitions include:

- Top / Left
- Top / Left / Diagonal
- Match / Mismatch
- Interval split
- Take / Skip
- Min / Max over choices

---

## 3. Determine Traversal Order

- Grid DP → top to bottom, left to right.
- Interval DP → increasing interval length.
- Reverse dependencies → bottom-up or reverse iteration.
- Memoization → recursive with caching.

---

## 4. Optimize Space

If each state depends only on the previous row:

```text
O(M × N)

↓

O(N)
```

---

## 5. Watch Boundary Conditions

Common sources of bugs:

- Empty strings
- Single-row/column grids
- Interval length = 1 or 2
- Obstacles
- Out-of-bounds transitions

---

# Final Revision Checklist

- Grid Counting DP
- Grid Cost DP
- Triangle DP
- Multi-Direction Grid DP
- Longest Common Subsequence
- Longest Palindromic Subsequence
- Edit Distance
- Distinct Subsequences
- Interleaving String
- Stone Game
- Cherry Pickup II
- Burst Balloons
- Minimum Score Triangulation
- Strange Printer
- Memoization vs. Tabulation
- Space Optimization
- Interval Traversal
- Matrix Chain Multiplication Pattern
- Multi-Agent DP
- Game Theory DP

---

> **End of Complete 2-D Dynamic Programming FAANG Interview Guide (15 LeetCode Problems).**


