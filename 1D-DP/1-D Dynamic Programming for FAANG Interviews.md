# 1-D Dynamic Programming for FAANG Interviews
> **Focus:** LeetCode Patterns • Java • Interview-Oriented • GitHub Ready

---

# Problem 1 — Climbing Stairs

- **Difficulty:** Easy
- **Pattern:** Fibonacci DP
- **Companies:** Google, Amazon, Microsoft, Apple, Adobe
- **LeetCode:** https://leetcode.com/problems/climbing-stairs/
- **Interview Frequency:** ★★★★★
- **LLM-Proof Variant:** Yes

---

## Problem Statement

You are climbing a staircase.

It takes **n** steps to reach the top.

Every move you may climb either:

- 1 step
- 2 steps

Return the number of distinct ways to reach the top.

---

## Why FAANG Asks This

Although simple, this question tests whether candidates recognize a recurrence instead of brute force recursion.

Interviewers usually extend this into:

- Climb 1,2,3 steps
- Broken stairs
- Minimum cost climbing
- Matrix exponentiation optimization

---

## Intuition

Suppose you're standing at stair **i**.

The last move must have come from

```
i-1
or
i-2
```

Therefore

```
ways(i)=ways(i−1)+ways(i−2)
```

Exactly Fibonacci.

---

## DP Transition

```
dp[0]=1
dp[1]=1

dp[i]=dp[i-1]+dp[i-2]
```

---

## Visualization

For n = 6

```
Step

0 -> 1
1 -> 1
2 -> 2
3 -> 3
4 -> 5
5 -> 8
6 -> 13
```

```
          13
        /    \
      8       5
     / \     / \
    5  3    3  2
```

---

## Memoization

```java
class Solution {

    private int[] memo;

    public int climbStairs(int n) {
        memo = new int[n + 1];
        return dfs(n);
    }

    private int dfs(int n) {

        if (n <= 1)
            return 1;

        if (memo[n] != 0)
            return memo[n];

        memo[n] = dfs(n - 1) + dfs(n - 2);

        return memo[n];
    }
}
```

---

## Tabulation

```java
class Solution {

    public int climbStairs(int n) {

        if (n <= 1)
            return 1;

        int[] dp = new int[n + 1];

        dp[0] = 1;
        dp[1] = 1;

        for (int i = 2; i <= n; i++)
            dp[i] = dp[i - 1] + dp[i - 2];

        return dp[n];
    }
}
```

---

## Space Optimized

```java
class Solution {

    public int climbStairs(int n) {

        if (n <= 1)
            return 1;

        int prev2 = 1;
        int prev1 = 1;

        for (int i = 2; i <= n; i++) {

            int cur = prev1 + prev2;

            prev2 = prev1;
            prev1 = cur;
        }

        return prev1;
    }
}
```

---

## Complexity

| Approach | Time | Space |
|----------|------|-------|
| Recursion | O(2ⁿ) | O(n) |
| Memoization | O(n) | O(n) |
| Tabulation | O(n) | O(n) |
| Space Optimized | O(n) | O(1) |

---

## Edge Cases

```
n=1

n=2

Very large n

Stack overflow using recursion
```

---

## Interview Follow-ups

- Allowed steps = 1,2,3
- Broken stairs
- Cost associated with every stair
- Matrix exponentiation
- General k-step jumps

---

# Problem 2 — Min Cost Climbing Stairs

- **Difficulty:** Easy
- **Pattern:** Fibonacci DP with Costs
- **Companies:** Amazon, Meta, Microsoft, Google
- **LeetCode:** https://leetcode.com/problems/min-cost-climbing-stairs/
- **Interview Frequency:** ★★★★★
- **LLM-Proof Variant:** Yes

---

## Problem Statement

Each stair has a cost.

You may begin at

```
0
or
1
```

Each move:

```
+1
or
+2
```

Return the minimum cost to reach the top.

---

## Interview Goal

Tests whether candidates can modify a counting DP into an optimization DP.

---

## Intuition

Instead of counting paths,

store

```
minimum cost to reach i
```

Transition

```
dp[i]=cost[i]+min(dp[i−1],dp[i−2])
```

Final answer

```
min(dp[n−1],dp[n−2])
```

---

## DP Table

Example

```
Cost

10 15 20
```

```
dp

10
15
30

Answer=min(15,30)=15
```

---

## Memoization

```java
class Solution {

    private int[] memo;
    private int[] cost;

    public int minCostClimbingStairs(int[] cost) {

        this.cost = cost;
        memo = new int[cost.length];

        java.util.Arrays.fill(memo, -1);

        return Math.min(dfs(cost.length - 1),
                        dfs(cost.length - 2));
    }

    private int dfs(int i) {

        if (i <= 1)
            return cost[i];

        if (memo[i] != -1)
            return memo[i];

        memo[i] = cost[i] +
                Math.min(dfs(i - 1), dfs(i - 2));

        return memo[i];
    }
}
```

---

## Tabulation

```java
class Solution {

    public int minCostClimbingStairs(int[] cost) {

        int n = cost.length;

        int[] dp = new int[n];

        dp[0] = cost[0];
        dp[1] = cost[1];

        for (int i = 2; i < n; i++) {

            dp[i] = cost[i] +
                    Math.min(dp[i - 1], dp[i - 2]);
        }

        return Math.min(dp[n - 1], dp[n - 2]);
    }
}
```

---

## Space Optimized

```java
class Solution {

    public int minCostClimbingStairs(int[] cost) {

        int first = cost[0];
        int second = cost[1];

        for (int i = 2; i < cost.length; i++) {

            int cur = cost[i] + Math.min(first, second);

            first = second;
            second = cur;
        }

        return Math.min(first, second);
    }
}
```

---

## Complexity

| Approach | Time | Space |
|----------|------|-------|
| Memoization | O(n) | O(n) |
| Tabulation | O(n) | O(n) |
| Optimized | O(n) | O(1) |

---

## Interview Follow-ups

- Jump 1–3 stairs
- Variable jump size
- Recover chosen path
- Circular staircase

---

# Problem 3 — House Robber

- **Difficulty:** Medium
- **Pattern:** Pick / Skip DP
- **Companies:** Amazon, Google, Microsoft, Meta, Bloomberg
- **LeetCode:** https://leetcode.com/problems/house-robber/
- **Interview Frequency:** ★★★★★
- **LLM-Proof Variant:** Yes

---

## Problem Statement

Given money in houses,

Adjacent houses cannot both be robbed.

Return maximum amount.

---

## Why Interviewers Love It

This is one of the most fundamental DP interview questions.

Candidates must identify:

```
Take

or

Skip
```

---

## Intuition

For every house

```
Take

nums[i]+dp[i-2]
```

or

```
Skip

dp[i-1]
```

Transition

```
dp[i]=max(skip,take)
```

---

## DP Visualization

Example

```
2 7 9 3 1
```

```
House

2
7
9
3
1
```

```
DP

2
7
11
11
12
```

---

## Memoization

```java
class Solution {

    private int[] nums;
    private int[] memo;

    public int rob(int[] nums) {

        this.nums = nums;
        memo = new int[nums.length];

        java.util.Arrays.fill(memo, -1);

        return dfs(nums.length - 1);
    }

    private int dfs(int i) {

        if (i < 0)
            return 0;

        if (memo[i] != -1)
            return memo[i];

        int take = nums[i] + dfs(i - 2);

        int skip = dfs(i - 1);

        memo[i] = Math.max(take, skip);

        return memo[i];
    }
}
```

---

## Tabulation

```java
class Solution {

    public int rob(int[] nums) {

        if (nums.length == 1)
            return nums[0];

        int[] dp = new int[nums.length];

        dp[0] = nums[0];

        dp[1] = Math.max(nums[0], nums[1]);

        for (int i = 2; i < nums.length; i++) {

            dp[i] = Math.max(dp[i - 1],
                    nums[i] + dp[i - 2]);
        }

        return dp[nums.length - 1];
    }
}
```

---

## Space Optimized

```java
class Solution {

    public int rob(int[] nums) {

        int prev2 = 0;
        int prev1 = 0;

        for (int money : nums) {

            int cur = Math.max(prev1,
                    money + prev2);

            prev2 = prev1;
            prev1 = cur;
        }

        return prev1;
    }
}
```

---

## Complexity

| Approach | Time | Space |
|----------|------|-------|
| Memo | O(n) | O(n) |
| Tabulation | O(n) | O(n) |
| Optimized | O(n) | O(1) |

---

## Common Mistakes

- Taking adjacent houses
- Wrong initialization
- Forgetting n=1

---

## LLM-Proof Follow-ups

- Return robbed indices
- Distance between robbed houses = k
- Circular houses
- Tree houses

---

# Problem 4 — House Robber II

- **Difficulty:** Medium
- **Pattern:** Circular DP
- **Companies:** Amazon, Google, Meta, Apple
- **LeetCode:** https://leetcode.com/problems/house-robber-ii/
- **Interview Frequency:** ★★★★☆
- **LLM-Proof Variant:** Yes

---

## Problem Statement

All houses are arranged in a circle.

First and last houses are adjacent.

Return maximum robbed amount.

---

## Interview Goal

Tests whether candidates can reduce a circular problem into multiple linear DP problems.

---

## Key Observation

You cannot rob

```
First
and
Last
```

together.

Therefore solve two cases.

```
Case 1

0...
n-2
```

```
Case 2

1...
n-1
```

Answer

```
max(case1,case2)
```

---

## Visualization

```
1 2 3 1
```

Case A

```
1 2 3
```

Case B

```
2 3 1
```

Take maximum.

---

## Java Solution

```java
class Solution {

    public int rob(int[] nums) {

        int n = nums.length;

        if (n == 1)
            return nums[0];

        return Math.max(
                robLinear(nums, 0, n - 2),
                robLinear(nums, 1, n - 1)
        );
    }

    private int robLinear(int[] nums,
                          int start,
                          int end) {

        int prev2 = 0;
        int prev1 = 0;

        for (int i = start; i <= end; i++) {

            int cur = Math.max(prev1,
                    nums[i] + prev2);

            prev2 = prev1;
            prev1 = cur;
        }

        return prev1;
    }
}
```

---

## Why This Works

Circular dependency disappears once either

- first house
- last house

is excluded.

Each remaining subproblem becomes the standard House Robber problem.

---

## Complexity

| Approach | Time | Space |
|----------|------|-------|
| DP | O(n) | O(1) |

---

## Edge Cases

```
1 house

2 houses

All equal values

Large input
```

---

## Interview Follow-ups

- Houses arranged in a binary tree (**House Robber III**)
- Houses form a graph
- Return robbed indices
- Minimum distance between robberies > 2

---

# Progress

- [x] Problem 1 — Climbing Stairs
- [x] Problem 2 — Min Cost Climbing Stairs
- [x] Problem 3 — House Robber
- [x] Problem 4 — House Robber II

**Next Part (Problems 5–8):**
- Delete and Earn
- Maximum Product Subarray
- Decode Ways
- Word Break


---

# Problem 5 — Delete and Earn

- **Difficulty:** Medium
- **Pattern:** House Robber Transformation
- **Companies:** Google, Amazon, Microsoft, Meta
- **LeetCode:** https://leetcode.com/problems/delete-and-earn/
- **Interview Frequency:** ★★★★☆
- **LLM-Proof Variant:** Yes

---

## Problem Statement

You are given an integer array `nums`.

When you pick a number `x`:

- You earn `x` points for every occurrence of `x`.
- Every occurrence of `x-1` and `x+1` is deleted.

Return the maximum points you can earn.

---

## Why FAANG Asks This

This problem tests whether candidates can recognize that a seemingly unrelated problem can be transformed into a familiar DP pattern.

Interviewers are checking:

- Pattern recognition
- State transformation
- DP modeling

---

## Key Insight

Instead of working directly on the array, combine equal values.

Example:

```
nums

2 2 3 3 3 4
```

Transform into

```
Value : Total Points

2 -> 4
3 -> 9
4 -> 4
```

Now observe:

Choosing value **3**

means

```
Cannot choose

2

or

4
```

Exactly the **House Robber** problem.

---

## DP State

```
points[i]

=

total score obtainable from value i
```

Transition

```
dp[i]

=

max(

dp[i-1],

dp[i-2]+points[i]

)
```

---

## Visualization

Example

```
nums

2 2 3 3 3 4
```

```
Points

0
0
4
9
4
```

DP

```
0
0
4
9
9
```

Maximum = **9**

---

## Java Solution (Space Optimized)

```java
class Solution {

    public int deleteAndEarn(int[] nums) {

        int max = 0;

        for (int num : nums)
            max = Math.max(max, num);

        int[] points = new int[max + 1];

        for (int num : nums)
            points[num] += num;

        int prev2 = 0;
        int prev1 = 0;

        for (int point : points) {

            int cur = Math.max(prev1, prev2 + point);

            prev2 = prev1;
            prev1 = cur;
        }

        return prev1;
    }
}
```

---

## Complexity

| Approach | Time | Space |
|----------|------|-------|
| DP | O(n + maxValue) | O(maxValue) |

---

## Edge Cases

```
Single number

All identical numbers

Large maximum value

Sparse values
```

---

## Common Interview Follow-ups

- Numbers up to 10⁹
- Return selected values
- Circular value dependency
- Choosing removes x±k instead of x±1

---

# Problem 6 — Maximum Product Subarray

- **Difficulty:** Medium
- **Pattern:** DP with Two States
- **Companies:** Amazon, Meta, Apple, Microsoft, Google
- **LeetCode:** https://leetcode.com/problems/maximum-product-subarray/
- **Interview Frequency:** ★★★★★
- **LLM-Proof Variant:** Yes

---

## Problem Statement

Given an integer array,

return the maximum product of a contiguous subarray.

---

## Why This Is Tricky

Unlike Kadane's Algorithm,

negative numbers can flip

```
smallest

↓

largest
```

Example

```
-2 × -3 = 6
```

Therefore one DP state is insufficient.

---

## Intuition

Maintain

```
maxEndingHere

minEndingHere
```

Why?

A negative number swaps them.

Transition

```
max = max(

num,

num * previousMax,

num * previousMin

)

min = min(

num,

num * previousMax,

num * previousMin

)
```

---

## Visualization

```
Array

2 3 -2 4
```

```
Index

2

Max=2

Min=2
```

↓

```
3

Max=6

Min=3
```

↓

```
-2

Max=-2

Min=-12
```

↓

```
4

Max=4

Min=-48
```

Answer = **6**

---

## Java Solution

```java
class Solution {

    public int maxProduct(int[] nums) {

        int maxEnding = nums[0];
        int minEnding = nums[0];
        int answer = nums[0];

        for (int i = 1; i < nums.length; i++) {

            int num = nums[i];

            if (num < 0) {

                int temp = maxEnding;
                maxEnding = minEnding;
                minEnding = temp;
            }

            maxEnding = Math.max(num,
                    maxEnding * num);

            minEnding = Math.min(num,
                    minEnding * num);

            answer = Math.max(answer,
                    maxEnding);
        }

        return answer;
    }
}
```

---

## Complexity

| Time | Space |
|------|-------|
| O(n) | O(1) |

---

## Edge Cases

```
Contains zero

Single element

All negatives

One positive only
```

---

## Interview Follow-ups

- Return the subarray
- Maximum product with one deletion
- Circular array
- Floating-point numbers

---

# Problem 7 — Decode Ways

- **Difficulty:** Medium
- **Pattern:** Prefix DP
- **Companies:** Amazon, Google, Microsoft, Meta, Apple
- **LeetCode:** https://leetcode.com/problems/decode-ways/
- **Interview Frequency:** ★★★★★
- **LLM-Proof Variant:** Yes

---

## Problem Statement

'A' -> 1

'B' -> 2

...

'Z' -> 26

Given a numeric string,

return the number of valid decodings.

---

## Why Interviewers Ask It

Tests whether candidates correctly define DP states and carefully handle invalid cases.

---

## Intuition

At every position,

two possibilities exist.

Take

```
1 digit
```

or

```
2 digits
```

if valid.

---

## DP State

```
dp[i]

=

ways to decode

substring(0...i)
```

Transition

```
Single digit valid

↓

dp[i]+=dp[i-1]
```

```
Two digits valid

↓

dp[i]+=dp[i-2]
```

---

## Visualization

Example

```
226
```

```
2|2|6

2|26

22|6
```

Answer = **3**

---

## Java Solution

```java
class Solution {

    public int numDecodings(String s) {

        int n = s.length();

        int[] dp = new int[n + 1];

        dp[0] = 1;

        dp[1] = s.charAt(0) == '0' ? 0 : 1;

        for (int i = 2; i <= n; i++) {

            int one = s.charAt(i - 1) - '0';

            int two = Integer.parseInt(
                    s.substring(i - 2, i));

            if (one >= 1)
                dp[i] += dp[i - 1];

            if (two >= 10 && two <= 26)
                dp[i] += dp[i - 2];
        }

        return dp[n];
    }
}
```

---

## Complexity

| Time | Space |
|------|-------|
| O(n) | O(n) |

---

## Common Pitfalls

```
"06"

"100"

"2101"

Leading zero

Trailing zero
```

---

## Interview Follow-ups

- Print every decoding
- Wildcard '*'
- Count modulo M
- Decode with custom alphabet

---

# Problem 8 — Word Break

- **Difficulty:** Medium
- **Pattern:** Prefix DP
- **Companies:** Amazon, Google, Meta, Microsoft, Bloomberg
- **LeetCode:** https://leetcode.com/problems/word-break/
- **Interview Frequency:** ★★★★★
- **LLM-Proof Variant:** Yes

---

## Problem Statement

Given

```
String s

Dictionary wordDict
```

Determine whether

the string can be segmented into valid dictionary words.

---

## Why FAANG Asks This

Tests

- DP state design
- String manipulation
- HashSet optimization

Very common interview question.

---

## Intuition

Suppose

```
dp[i]

=

Can first i characters be segmented?
```

Try every previous cut.

```
j

↓

i
```

If

```
dp[j]

=

true

AND

substring(j,i)

exists

↓

dp[i]=true
```

---

## DP Visualization

Example

```
leetcode
```

Dictionary

```
leet

code
```

```
Index

0 1 2 3 4 5 6 7 8
```

```
dp

T F F F T F F F T
```

Answer = True

---

## Java Solution

```java
class Solution {

    public boolean wordBreak(String s,
                             List<String> wordDict) {

        Set<String> set = new HashSet<>(wordDict);

        boolean[] dp = new boolean[s.length() + 1];

        dp[0] = true;

        for (int i = 1; i <= s.length(); i++) {

            for (int j = 0; j < i; j++) {

                if (dp[j] &&
                    set.contains(s.substring(j, i))) {

                    dp[i] = true;
                    break;
                }
            }
        }

        return dp[s.length()];
    }
}
```

---

## Complexity

| Approach | Time | Space |
|----------|------|-------|
| Basic DP | O(n²) | O(n) |

> If substring creation is considered O(k), the practical runtime can approach O(n³). Using a Trie or limiting word lengths can improve performance.

---

## Edge Cases

```
Empty string

Duplicate words

Long dictionary

Repeated prefixes

Impossible segmentation
```

---

## LLM-Proof Follow-ups

- Print one valid segmentation
- Print all segmentations (**Word Break II**)
- Minimum number of words
- Trie optimization
- Dictionary updates during queries

---

# Progress

- [x] Problem 1 — Climbing Stairs
- [x] Problem 2 — Min Cost Climbing Stairs
- [x] Problem 3 — House Robber
- [x] Problem 4 — House Robber II
- [x] Problem 5 — Delete and Earn
- [x] Problem 6 — Maximum Product Subarray
- [x] Problem 7 — Decode Ways
- [x] Problem 8 — Word Break

**Next Part (Problems 9–12):**
1. Longest Increasing Subsequence
2. Number of Longest Increasing Subsequence
3. Longest Arithmetic Subsequence
4. Partition Equal Subset Sum

---

# Problem 9 — Longest Increasing Subsequence (LIS)

- **Difficulty:** Medium
- **Pattern:** 1-D DP + Binary Search Optimization
- **Companies:** Google, Amazon, Meta, Microsoft, Apple
- **LeetCode:** https://leetcode.com/problems/longest-increasing-subsequence/
- **Interview Frequency:** ★★★★★
- **LLM-Proof Variant:** Yes

---

## Problem Statement

Given an integer array `nums`, return the length of the **longest strictly increasing subsequence**.

A subsequence does **not** need to be contiguous.

---

## Why FAANG Asks This

LIS is one of the most important Dynamic Programming interview problems.

Interviewers evaluate whether candidates can:

- Define DP states correctly
- Optimize from O(n²) to O(n log n)
- Explain why Binary Search works

---

## Intuition

Suppose LIS ends at index **i**.

Check every previous element.

If

```
nums[j] < nums[i]
```

then

```
dp[i]

=

max(

dp[i],

dp[j]+1

)
```

---

## DP State

```
dp[i]

=

Length of LIS ending at i
```

---

## DP Visualization

Example

```
10 9 2 5 3 7 101 18
```

```
DP

1
1
1
2
2
3
4
4
```

Answer

```
4
```

---

## Java (O(n²))

```java
class Solution {

    public int lengthOfLIS(int[] nums) {

        int n = nums.length;

        int[] dp = new int[n];

        Arrays.fill(dp, 1);

        int answer = 1;

        for (int i = 1; i < n; i++) {

            for (int j = 0; j < i; j++) {

                if (nums[j] < nums[i]) {

                    dp[i] = Math.max(dp[i],
                            dp[j] + 1);
                }
            }

            answer = Math.max(answer, dp[i]);
        }

        return answer;
    }
}
```

---

## Optimized O(n log n)

Maintain

```
tails[i]

=

Smallest ending value

for LIS of length i+1
```

Binary search finds the replacement position.

```java
class Solution {

    public int lengthOfLIS(int[] nums) {

        int[] tails = new int[nums.length];

        int size = 0;

        for (int num : nums) {

            int left = 0;
            int right = size;

            while (left < right) {

                int mid = (left + right) / 2;

                if (tails[mid] < num)
                    left = mid + 1;
                else
                    right = mid;
            }

            tails[left] = num;

            if (left == size)
                size++;
        }

        return size;
    }
}
```

---

## Binary Search Visualization

```
tails

2
5
7
18
```

Insert

```
6
```

After replacement

```
2
5
6
18
```

Length remains

```
4
```

but future growth becomes easier.

---

## Complexity

| Approach | Time | Space |
|----------|------|-------|
| DP | O(n²) | O(n) |
| Binary Search | O(n log n) | O(n) |

---

## Common Interview Follow-ups

- Print LIS
- Count LIS
- Non-decreasing subsequence
- Circular LIS

---

# Problem 10 — Number of Longest Increasing Subsequence

- **Difficulty:** Medium
- **Pattern:** Dual-State DP
- **Companies:** Google, Amazon, Microsoft
- **LeetCode:** https://leetcode.com/problems/number-of-longest-increasing-subsequence/
- **Interview Frequency:** ★★★★☆
- **LLM-Proof Variant:** Yes

---

## Problem Statement

Return

```
Number

of

Longest Increasing Subsequences
```

---

## Why It's Asked

Candidates must maintain **two DP arrays** simultaneously.

Many candidates only compute the LIS length.

---

## DP States

```
length[i]

=

LIS ending at i
```

```
count[i]

=

Number of LIS ending at i
```

---

## Transition

If longer sequence found

```
length[i]

=

length[j]+1

count[i]

=

count[j]
```

If equally long

```
count[i]+=count[j]
```

---

## Visualization

```
1 3 5 4 7
```

```
Length

1
2
3
3
4
```

```
Count

1
1
1
1
2
```

Answer

```
2
```

---

## Java Solution

```java
class Solution {

    public int findNumberOfLIS(int[] nums) {

        int n = nums.length;

        int[] length = new int[n];
        int[] count = new int[n];

        Arrays.fill(length, 1);
        Arrays.fill(count, 1);

        int maxLen = 1;

        for (int i = 0; i < n; i++) {

            for (int j = 0; j < i; j++) {

                if (nums[j] < nums[i]) {

                    if (length[j] + 1 > length[i]) {

                        length[i] = length[j] + 1;
                        count[i] = count[j];

                    } else if (length[j] + 1 == length[i]) {

                        count[i] += count[j];
                    }
                }
            }

            maxLen = Math.max(maxLen, length[i]);
        }

        int answer = 0;

        for (int i = 0; i < n; i++) {

            if (length[i] == maxLen)
                answer += count[i];
        }

        return answer;
    }
}
```

---

## Complexity

| Time | Space |
|------|-------|
| O(n²) | O(n) |

---

## Interview Follow-ups

- Print every LIS
- Lexicographically smallest LIS
- Modulo arithmetic
- Weighted LIS

---

# Problem 11 — Longest Arithmetic Subsequence

- **Difficulty:** Medium
- **Pattern:** DP with Difference States
- **Companies:** Google, Meta, Amazon
- **LeetCode:** https://leetcode.com/problems/longest-arithmetic-subsequence/
- **Interview Frequency:** ★★★★☆
- **LLM-Proof Variant:** Yes

---

## Problem Statement

Return the length of the longest arithmetic subsequence.

Arithmetic means

```
Difference remains constant.
```

---

## Intuition

State depends on

```
Index

+

Difference
```

Each index stores

```
difference

→

best length
```

---

## DP Visualization

Example

```
3 6 9 12
```

Difference

```
3
```

Lengths

```
1
2
3
4
```

---

## Java Solution

```java
class Solution {

    public int longestArithSeqLength(int[] nums) {

        int n = nums.length;

        HashMap<Integer, Integer>[] dp = new HashMap[n];

        for (int i = 0; i < n; i++)
            dp[i] = new HashMap<>();

        int answer = 2;

        for (int i = 0; i < n; i++) {

            for (int j = 0; j < i; j++) {

                int diff = nums[i] - nums[j];

                int len = dp[j].getOrDefault(diff, 1) + 1;

                dp[i].put(diff,
                        Math.max(dp[i].getOrDefault(diff, 1), len));

                answer = Math.max(answer, len);
            }
        }

        return answer;
    }
}
```

---

## Complexity

| Time | Space |
|------|-------|
| O(n²) | O(n²) |

---

## Edge Cases

```
Repeated numbers

Negative difference

Large values

All equal numbers
```

---

## Interview Follow-ups

- Print subsequence
- Fixed difference
- Longest geometric subsequence
- Streaming version

---

# Problem 12 — Partition Equal Subset Sum

- **Difficulty:** Medium
- **Pattern:** 1-D Knapsack DP
- **Companies:** Amazon, Google, Meta, Microsoft
- **LeetCode:** https://leetcode.com/problems/partition-equal-subset-sum/
- **Interview Frequency:** ★★★★★
- **LLM-Proof Variant:** Yes

---

## Problem Statement

Determine whether an array can be partitioned into

```
Two subsets

having equal sum.
```

---

## Key Observation

If total sum is odd

```
Impossible
```

Otherwise

Target

```
sum / 2
```

Now ask

```
Can we build

target?
```

---

## DP State

```
dp[s]

=

Can sum s be formed?
```

Reverse iteration prevents using one element multiple times.

---

## Visualization

Example

```
1 5 11 5
```

Target

```
11
```

```
0 ✔

1 ✔

6 ✔

11 ✔
```

Answer

```
True
```

---

## Java Solution

```java
class Solution {

    public boolean canPartition(int[] nums) {

        int sum = 0;

        for (int num : nums)
            sum += num;

        if (sum % 2 != 0)
            return false;

        int target = sum / 2;

        boolean[] dp = new boolean[target + 1];

        dp[0] = true;

        for (int num : nums) {

            for (int s = target; s >= num; s--) {

                dp[s] = dp[s] || dp[s - num];
            }
        }

        return dp[target];
    }
}
```

---

## DP Transition

```
Current number

↓

num
```

```
dp[s]

=

dp[s]

OR

dp[s-num]
```

Reverse iteration

```
Target

↓

0
```

avoids reusing the same element.

---

## Complexity

| Time | Space |
|------|-------|
| O(n × target) | O(target) |

---

## Common Pitfalls

- Forward iteration (incorrect)
- Forgetting odd total sum
- Reusing elements
- Incorrect base case

---

## Interview Follow-ups

- Return the subsets
- Count total partitions
- Minimum subset difference
- Partition into k subsets
- Target Sum (LeetCode 494)

---

# Progress

- [x] Problem 1 — Climbing Stairs
- [x] Problem 2 — Min Cost Climbing Stairs
- [x] Problem 3 — House Robber
- [x] Problem 4 — House Robber II
- [x] Problem 5 — Delete and Earn
- [x] Problem 6 — Maximum Product Subarray
- [x] Problem 7 — Decode Ways
- [x] Problem 8 — Word Break
- [x] Problem 9 — Longest Increasing Subsequence
- [x] Problem 10 — Number of Longest Increasing Subsequence
- [x] Problem 11 — Longest Arithmetic Subsequence
- [x] Problem 12 — Partition Equal Subset Sum

**Next Part (Problems 13–15):**
1. Coin Change
2. Combination Sum IV
3. Best Time to Buy and Sell Stock with Cooldown

---

# Problem 13 — Coin Change

- **Difficulty:** Medium
- **Pattern:** Unbounded 1-D Dynamic Programming
- **Companies:** Google, Amazon, Microsoft, Meta, Apple
- **LeetCode:** https://leetcode.com/problems/coin-change/
- **Interview Frequency:** ★★★★★
- **LLM-Proof Variant:** Yes

---

## Problem Statement

You are given an array `coins` representing different coin denominations and an integer `amount`.

Return the **minimum number of coins** required to make up the amount.

If it is impossible, return **-1**.

Each coin may be used **unlimited times**.

---

## Why FAANG Asks This

This question tests whether candidates can distinguish between:

- Unbounded DP
- 0/1 Knapsack
- Minimum optimization DP

It is also frequently used as the base for several harder interview questions.

---

## Intuition

Suppose

```
dp[x]

=

Minimum coins needed

to form amount x
```

For every coin,

try taking it as the **last coin**.

```
dp[x]

=

min(

dp[x],

dp[x-coin]+1

)
```

---

## DP Transition

```
Base

dp[0]=0
```

Transition

```
for coin

    for amount

        dp[amount]

        =

        min(

        dp[amount],

        dp[amount-coin]+1
        )
```

---

## Visualization

Example

```
Coins

1 2 5

Amount

11
```

```
dp

0
1
1
2
2
1
2
2
3
3
2
3
```

Answer

```
5+5+1

=

3 coins
```

---

## Java Solution

```java
class Solution {

    public int coinChange(int[] coins, int amount) {

        int[] dp = new int[amount + 1];

        Arrays.fill(dp, amount + 1);

        dp[0] = 0;

        for (int coin : coins) {

            for (int i = coin; i <= amount; i++) {

                dp[i] = Math.min(dp[i],
                        dp[i - coin] + 1);
            }
        }

        return dp[amount] > amount ? -1 : dp[amount];
    }
}
```

---

## Complexity

| Time | Space |
|------|-------|
| O(n × amount) | O(amount) |

---

## Edge Cases

```
amount = 0

Impossible answer

Single denomination

Duplicate coins

Large amount
```

---

## Common Interview Follow-ups

- Print selected coins
- Limited supply of each coin
- Count total combinations
- Count total permutations
- Exactly K coins

---

# Problem 14 — Combination Sum IV

- **Difficulty:** Medium
- **Pattern:** Ordered DP (Permutations)
- **Companies:** Google, Amazon, Microsoft, Meta
- **LeetCode:** https://leetcode.com/problems/combination-sum-iv/
- **Interview Frequency:** ★★★★☆
- **LLM-Proof Variant:** Yes

---

## Problem Statement

Given

```
nums

target
```

Return the number of ordered combinations that sum to the target.

Different orders count as different answers.

---

## Why This Is Important

Many candidates confuse this with Coin Change.

Difference:

Coin Change

```
1+2

=

2+1
```

Combination Sum IV

```
1+2

≠

2+1
```

Order matters.

---

## DP State

```
dp[i]

=

Number of ordered ways

to form i
```

---

## Transition

```
For every target

↓

Try every number

↓

dp[target]

+=

dp[target-num]
```

---

## Visualization

Example

```
nums

1 2 3

target

4
```

Possible

```
1 1 1 1

1 1 2

1 2 1

2 1 1

2 2

1 3

3 1
```

Answer

```
7
```

---

## Java Solution

```java
class Solution {

    public int combinationSum4(int[] nums, int target) {

        int[] dp = new int[target + 1];

        dp[0] = 1;

        for (int i = 1; i <= target; i++) {

            for (int num : nums) {

                if (i >= num)

                    dp[i] += dp[i - num];
            }
        }

        return dp[target];
    }
}
```

---

## Complexity

| Time | Space |
|------|-------|
| O(target × n) | O(target) |

---

## Common Mistakes

- Using Coin Change loop order
- Forgetting `dp[0]=1`
- Integer overflow
- Ignoring order requirement

---

## Interview Follow-ups

- Count only unique combinations
- Limit number usage
- Print all sequences
- Modulo arithmetic
- Negative numbers allowed

---

# Problem 15 — Best Time to Buy and Sell Stock with Cooldown

- **Difficulty:** Medium
- **Pattern:** State Machine DP
- **Companies:** Google, Meta, Amazon, Apple, Microsoft
- **LeetCode:** https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/
- **Interview Frequency:** ★★★★★
- **LLM-Proof Variant:** Yes

---

## Problem Statement

You may complete as many transactions as you like.

Constraints:

- Must sell before buying again.
- After selling, you must wait **one day** before buying.

Return the maximum profit.

---

## Why FAANG Loves This Problem

Interviewers want to see whether candidates can model DP using **states** instead of arrays.

It forms the basis of nearly every advanced stock DP problem.

---

## State Definition

Three states exist on every day.

```
Hold

↓

Currently holding stock
```

```
Sold

↓

Sold today
```

```
Rest

↓

No stock

Cooldown completed
```

---

## State Transition Diagram

```text
          Buy
Rest -------------> Hold
 ^                   |
 |                   |
 |                   | Sell
 |                   v
 +--------------- Sold
        Cooldown
```

---

## DP Equations

```
hold

=

max(

hold,

rest-price

)
```

```
sold

=

hold+price
```

```
rest

=

max(

rest,

previousSold

)
```

---

## Visualization

Example

```
Prices

1 2 3 0 2
```

```
Day

1

Hold=-1

Sold=0

Rest=0
```

↓

```
Day

2

Hold=-1

Sold=1

Rest=0
```

↓

```
Day

3

Hold=-1

Sold=2

Rest=1
```

↓

```
Day

4

Hold=1

Sold=-1

Rest=2
```

↓

```
Day

5

Hold=1

Sold=3

Rest=2
```

Answer

```
3
```

---

## Java Solution

```java
class Solution {

    public int maxProfit(int[] prices) {

        int hold = -prices[0];
        int sold = 0;
        int rest = 0;

        for (int i = 1; i < prices.length; i++) {

            int previousSold = sold;

            sold = hold + prices[i];

            hold = Math.max(hold,
                    rest - prices[i]);

            rest = Math.max(rest,
                    previousSold);
        }

        return Math.max(sold, rest);
    }
}
```

---

## Complexity

| Time | Space |
|------|-------|
| O(n) | O(1) |

---

## Edge Cases

```
One day

Always decreasing

Always increasing

Repeated prices

Very large input
```

---

## Common Interview Follow-ups

- Transaction fee
- At most K transactions
- Unlimited transactions
- Two transactions
- Cooldown of K days
- Return transaction days

---

# 1-D Dynamic Programming Interview Pattern Cheat Sheet

| Pattern | Representative Problems |
|----------|--------------------------|
| Fibonacci DP | Climbing Stairs, Min Cost Climbing Stairs |
| Pick / Skip | House Robber I & II, Delete and Earn |
| Prefix DP | Decode Ways, Word Break |
| Sequence DP | LIS, Number of LIS |
| Difference DP | Longest Arithmetic Subsequence |
| Knapsack-style DP | Partition Equal Subset Sum, Coin Change |
| Ordered DP | Combination Sum IV |
| State Machine DP | Best Time to Buy & Sell Stock with Cooldown |
| Product DP | Maximum Product Subarray |

---

# Common Interview Mistakes

### 1. Incorrect DP State

Many candidates define the wrong state before writing transitions.

Always ask:

```
"What exactly does dp[i] represent?"
```

---

### 2. Wrong Traversal Direction

For 0/1 problems:

```
Target

↓

Reverse
```

For unbounded problems:

```
Target

↓

Forward
```

---

### 3. Incorrect Base Cases

Typical mistakes:

```
dp[0]

=

0

instead of

1
```

or

forgetting

```
Empty string

Empty array

Target = 0
```

---

### 4. Missing Space Optimization

Many 1-D DP problems only require:

```
previous

current
```

instead of the full DP array.

---

### 5. Confusing Subsequence vs Subarray

Subsequence

```
May skip elements
```

Subarray

```
Must remain contiguous
```

---

# Final Revision Checklist

Before a FAANG interview, ensure you can:

- Explain the DP state before writing code.
- Derive the recurrence without memorization.
- Identify whether the problem is optimization, counting, or decision DP.
- Recognize transformations (e.g., Delete and Earn → House Robber).
- Know when to iterate forward vs. backward.
- Optimize space from O(n) to O(1) when possible.
- Explain why greedy approaches fail for these problems.
- Handle edge cases without modifying the recurrence.

---

# Complete Problem List

| # | Problem | Difficulty | Pattern |
|---|---------|------------|---------|
| 1 | Climbing Stairs | Easy | Fibonacci DP |
| 2 | Min Cost Climbing Stairs | Easy | Cost DP |
| 3 | House Robber | Medium | Pick/Skip |
| 4 | House Robber II | Medium | Circular DP |
| 5 | Delete and Earn | Medium | House Robber Transformation |
| 6 | Maximum Product Subarray | Medium | Dual-State DP |
| 7 | Decode Ways | Medium | Prefix DP |
| 8 | Word Break | Medium | Prefix DP |
| 9 | Longest Increasing Subsequence | Medium | Sequence DP |
| 10 | Number of Longest Increasing Subsequence | Medium | Dual-State DP |
| 11 | Longest Arithmetic Subsequence | Medium | Difference DP |
| 12 | Partition Equal Subset Sum | Medium | Knapsack DP |
| 13 | Coin Change | Medium | Unbounded DP |
| 14 | Combination Sum IV | Medium | Ordered DP |
| 15 | Best Time to Buy & Sell Stock with Cooldown | Medium | State Machine DP |

---
**End of 1-D Dynamic Programming Interview Guide**
