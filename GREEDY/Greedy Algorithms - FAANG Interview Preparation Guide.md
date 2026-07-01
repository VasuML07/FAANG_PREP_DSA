# Greedy Algorithms - FAANG Interview Preparation Guide

> **Topic:** Greedy Algorithms
>
> **Language:** Java
>
> **Problems:** 15 (This document - Part 1 covers Problems 1-4)
>
> **Focus:** LeetCode + FAANG Interviews + LLM Interview Readiness

---

# Easy Problems

---

# Problem 1 — LeetCode 455. Assign Cookies

**Difficulty:** Easy

**Pattern:** Greedy Matching

**Companies**

- Google
- Amazon
- Microsoft
- Meta
- Adobe

**Problem**

https://leetcode.com/problems/assign-cookies/

You have children with greed factors and cookies with sizes.

A child is satisfied only if assigned a cookie whose size ≥ greed factor.

Return the maximum number of satisfied children.

---

# Greedy Observation

A large cookie can satisfy both small and large greed.

A small cookie can satisfy only small greed.

Therefore:

> Always give the **smallest possible cookie** that satisfies the current child.

Sorting allows us to efficiently perform this assignment.

---

# Why Greedy Works

Suppose

```
Children:
1 2 4

Cookies:
1 3
```

Sorted:

```
Child 1 ← Cookie 1 ✓

Child 2 ← Cookie 3 ✓

Answer = 2
```

If we wasted cookie 3 on child 1,

```
Child 1 ← Cookie 3

Child 2 ← none

Answer = 1
```

Clearly worse.

Greedy never wastes a larger cookie.

---

# Visualization

```
Children

1 ----\
        Cookie 1

2 ----\
        Cookie 3

4 ---- X
```

---

# Algorithm

1. Sort greed array.
2. Sort cookie array.
3. Use two pointers.
4. Match smallest available cookie.
5. Move to next child.

---

# Java Solution

```java
import java.util.Arrays;

class Solution {
    public int findContentChildren(int[] g, int[] s) {

        Arrays.sort(g);
        Arrays.sort(s);

        int child = 0;
        int cookie = 0;

        while (child < g.length && cookie < s.length) {

            if (s[cookie] >= g[child]) {
                child++;
            }

            cookie++;
        }

        return child;
    }
}
```

---

# Complexity

Sorting

```
O(n log n + m log m)
```

Traversal

```
O(n+m)
```

Space

```
O(1)
```

---

# Why Greedy Is Correct

Invariant:

```
Current cookie
=
Smallest unused cookie
```

Using a larger cookie early can only reduce future options.

Thus greedy is always optimal.

---

# Interview Follow-ups

- What if cookies can be split?
- What if each child needs two cookies?
- What if cookies have costs?

These variations usually require DP or Binary Search.

---

# LLM Interview Angle

Interviewers often ask:

> Explain WHY smallest cookie is always optimal.

The expected explanation is the exchange argument:

Replacing a larger cookie with a smaller sufficient cookie never hurts future assignments.

---

---

# Problem 2 — LeetCode 605. Can Place Flowers

**Difficulty:** Easy

**Pattern:** Local Greedy Decision

**Companies**

- Amazon
- Google
- Apple

**Problem**

https://leetcode.com/problems/can-place-flowers/

Determine whether n flowers can be planted without violating the rule that adjacent plots cannot both contain flowers.

---

# Greedy Observation

Whenever a position can safely hold a flower,

```
Plant immediately.
```

Why?

Waiting never creates a better opportunity later.

---

Example

```
0 0 0
```

Plant at first position?

```
1 0 0
```

Still optimal.

Plant middle?

```
0 1 0
```

Same count.

Greedy remains optimal.

---

# Visualization

Before

```
0 0 1 0 0
```

After

```
1 0 1 0 1
```

Maximum flowers planted.

---

# Algorithm

Traverse array.

For each position,

check

```
left == empty

current == empty

right == empty
```

If true,

```
Plant.

count++
```

---

# Java Solution

```java
class Solution {

    public boolean canPlaceFlowers(int[] flowerbed, int n) {

        int count = 0;

        for (int i = 0; i < flowerbed.length; i++) {

            if (flowerbed[i] == 0) {

                int left = (i == 0) ? 0 : flowerbed[i - 1];
                int right = (i == flowerbed.length - 1) ? 0 : flowerbed[i + 1];

                if (left == 0 && right == 0) {

                    flowerbed[i] = 1;
                    count++;

                    if (count >= n)
                        return true;
                }
            }
        }

        return count >= n;
    }
}
```

---

# Complexity

Time

```
O(n)
```

Space

```
O(1)
```

---

# Why Greedy Works

Each planted flower blocks only immediate neighbors.

Delaying placement never increases future capacity.

Hence immediate planting is optimal.

---

# Interview Follow-up

Suppose planting costs differ.

Greedy no longer works.

Need DP.

---

# LLM Interview Angle

Interviewers often ask:

Why doesn't early planting reduce later opportunities?

Expected answer:

Because each placement only affects adjacent cells and every feasible placement consumes identical resources.

---

---

# Problem 3 — LeetCode 860. Lemonade Change

**Difficulty:** Easy

**Pattern:** Resource Allocation

**Companies**

- Amazon
- Google
- Microsoft

**Problem**

https://leetcode.com/problems/lemonade-change/

Each lemonade costs $5.

Customers pay with

```
5
10
20
```

Return true if change can always be given.

---

# Greedy Observation

When receiving $20,

prefer

```
10 + 5
```

instead of

```
5 + 5 + 5
```

Why?

A $10 bill has fewer future uses.

Saving $5 bills increases flexibility.

---

Example

Bills

```
5 5 10 20
```

Change

```
20

↓

10+5
```

Correct.

---

# Visualization

Wallet

```
5 : 3

10 : 1
```

Customer gives

```
20
```

Greedy

```
Use

10

+

5
```

Save remaining fives.

---

# Algorithm

Maintain

```
five

ten
```

For each customer,

update counts.

For $20,

```
if possible

10+5

else

5+5+5
```

---

# Java Solution

```java
class Solution {

    public boolean lemonadeChange(int[] bills) {

        int five = 0;
        int ten = 0;

        for (int bill : bills) {

            if (bill == 5) {

                five++;

            } else if (bill == 10) {

                if (five == 0)
                    return false;

                five--;
                ten++;

            } else {

                if (ten > 0 && five > 0) {

                    ten--;
                    five--;

                } else if (five >= 3) {

                    five -= 3;

                } else {

                    return false;
                }
            }
        }

        return true;
    }
}
```

---

# Complexity

Time

```
O(n)
```

Space

```
O(1)
```

---

# Why Greedy Works

Using

```
10+5
```

preserves more $5 bills.

Future customers paying with $10 always require a $5 bill.

Thus greedy maximizes future flexibility.

---

# Interview Follow-up

Suppose customers can pay with

```
50
```

Now strategy changes.

Need generalized coin system reasoning.

---

# LLM Interview Angle

Classic proof question:

Why preserve smaller denomination?

Expected answer:

Because $5 bills satisfy more future payment combinations than $10 bills.

---

---

# Problem 4 — LeetCode 392. Is Subsequence

**Difficulty:** Easy

**Pattern:** Greedy Two Pointers

**Companies**

- Meta
- Google
- Microsoft
- Amazon

**Problem**

https://leetcode.com/problems/is-subsequence/

Determine whether string

```
s
```

is a subsequence of

```
t
```

---

# Greedy Observation

Always match the earliest possible character.

Never skip an available match.

---

Example

```
s

abc

t

ahbgdc
```

Greedy

```
a ✓

b ✓

c ✓
```

Answer

```
true
```

---

# Why Earliest Match?

Suppose

```
s

ac

t

abbbc
```

Choosing the earliest

```
a
```

cannot hurt.

Choosing a later

```
a
```

only reduces remaining characters.

Thus earliest match is optimal.

---

# Visualization

```
t

a h b g d c

^

match a

move

-----------

a h b g d c

        ^

match c
```

---

# Algorithm

Maintain

```
i → s

j → t
```

If

```
s[i]==t[j]
```

advance both.

Else

advance

```
j
```

---

# Java Solution

```java
class Solution {

    public boolean isSubsequence(String s, String t) {

        int i = 0;
        int j = 0;

        while (i < s.length() && j < t.length()) {

            if (s.charAt(i) == t.charAt(j))
                i++;

            j++;
        }

        return i == s.length();
    }
}
```

---

# Complexity

Time

```
O(n)
```

Space

```
O(1)
```

---

# Why Greedy Works

Matching earlier leaves maximum suffix available.

Any delayed match cannot improve future matching possibilities.

This is a classic greedy exchange argument.

---

# Interview Follow-up

Suppose there are

```
1 billion
```

queries.

Can we preprocess?

Expected answer:

Store character indices and answer each query using Binary Search.

---

# LLM Interview Angle

Frequently used to evaluate reasoning quality.

Candidates should explain:

> Why earliest matching is always optimal.

---

## Progress

Completed **4 / 15** problems.

### Covered Patterns

| Problem | Greedy Pattern |
|----------|----------------|
|455|Matching|
|605|Local Decision|
|860|Resource Allocation|
|392|Earliest Valid Choice|

**Next Part (Part 2):**
- 134. Gas Station
- 409. Longest Palindrome
- 1005. Maximize Sum After K Negations
- 1710. Maximum Units on a Truck
 
---

# Problem 5 — LeetCode 134. Gas Station

**Difficulty:** Medium

**Pattern:** Greedy + Prefix Sum

**Companies**

- Google
- Amazon
- Microsoft
- Meta
- Apple

**Problem**

https://leetcode.com/problems/gas-station/

There are `n` gas stations arranged in a circle.

- `gas[i]` = gas available at station `i`
- `cost[i]` = gas required to travel to the next station

Return the starting station index if you can travel around the circuit exactly once; otherwise return `-1`.

---

# Greedy Observation

A brute-force solution tries every station:

```
Start 0
Start 1
Start 2
...
```

Time Complexity:

```
O(n²)
```

Greedy discovers an important fact:

> If you fail while starting from station **A**, then **every station between A and the failure point also fails**.

Therefore we can safely skip them.

---

Example

```
Gas

1 2 3 4 5

Cost

3 4 5 1 2
```

Difference

```
-2 -2 -2 +3 +3
```

Start at 0

```
Tank

0

↓

-2

Fail
```

Stations

```
0
1
2
```

can never become valid starts.

Jump directly to station 3.

---

# Why Greedy Works

Suppose

```
Start = A
```

and first failure occurs at

```
B
```

Then

```
Gas(A...B)

<

Cost(A...B)
```

Now assume some station between

```
A and B
```

could succeed.

That would imply

```
Gas(mid...B)

>=

Cost(mid...B)
```

Contradiction.

Hence every intermediate station is impossible.

---

# Visualization

```
0 ----X

1 ----X

2 ----X

3 ---------> Success

4 ---------/
```

---

# Algorithm

Maintain

```
tank

total

start
```

For every station

```
tank += gas[i]-cost[i]

total += gas[i]-cost[i]
```

If

```
tank < 0
```

then

```
start = i+1

tank = 0
```

Finally

```
if total >= 0

return start

else

return -1
```

---

# Java Solution

```java
class Solution {

    public int canCompleteCircuit(int[] gas, int[] cost) {

        int total = 0;
        int tank = 0;
        int start = 0;

        for (int i = 0; i < gas.length; i++) {

            int diff = gas[i] - cost[i];

            total += diff;
            tank += diff;

            if (tank < 0) {

                start = i + 1;
                tank = 0;
            }
        }

        return total >= 0 ? start : -1;
    }
}
```

---

# Complexity

Time

```
O(n)
```

Space

```
O(1)
```

---

# Why Greedy Is Correct

Two observations prove correctness.

### Observation 1

If total gas is less than total cost,

```
Impossible
```

for every starting point.

---

### Observation 2

Whenever cumulative fuel becomes negative,

all previous candidates are permanently discarded.

This reduces

```
O(n²)

↓

O(n)
```

---

# Interview Follow-ups

- Return all possible starting stations.
- Stations arranged in a line instead of a circle.
- Each station has capacity limits.

---

# LLM Interview Angle

Interviewers frequently ask:

> Why can we skip all failed stations?

A correct exchange-proof explanation is more important than memorizing the algorithm.

---

---

# Problem 6 — LeetCode 409. Longest Palindrome

**Difficulty:** Easy

**Pattern:** Frequency Greedy

**Companies**

- Google
- Amazon
- Apple
- Meta

**Problem**

https://leetcode.com/problems/longest-palindrome/

Given a string,

return the maximum possible palindrome length that can be built using its letters.

---

# Greedy Observation

Palindrome rule:

```
Even counts

↓

Always usable
```

Odd counts

```
7

↓

Use only 6

Keep one for center
```

At most one odd character may occupy the center.

---

Example

```
abccccdd
```

Counts

```
a =1

b =1

c =4

d =2
```

Palindrome

```
dccaccd

Length = 7
```

---

# Visualization

```
Left

abc

|

Center

a

|

Right

cba
```

Only one odd frequency can remain.

---

# Algorithm

For every frequency

```
Even

↓

Take all
```

```
Odd

↓

Take freq-1
```

After processing,

if any odd existed,

```
+

1
```

---

# Java Solution

```java
class Solution {

    public int longestPalindrome(String s) {

        int[] freq = new int[128];

        for (char c : s.toCharArray())
            freq[c]++;

        int ans = 0;
        boolean odd = false;

        for (int f : freq) {

            if (f % 2 == 0) {

                ans += f;

            } else {

                ans += f - 1;
                odd = true;
            }
        }

        if (odd)
            ans++;

        return ans;
    }
}
```

---

# Complexity

Time

```
O(n)
```

Space

```
O(1)
```

---

# Why Greedy Works

Every even frequency contributes completely.

For odd frequencies,

keeping more than one unmatched character is impossible.

Thus removing exactly one from every odd count is always optimal.

---

# Interview Follow-up

Construct the palindrome instead of only returning its length.

---

# LLM Interview Angle

Tests whether candidates understand

> why only one odd frequency is allowed.

---

---

# Problem 7 — LeetCode 1005. Maximize Sum Of Array After K Negations

**Difficulty:** Easy

**Pattern:** Greedy Transformation

**Companies**

- Amazon
- Microsoft
- Google

**Problem**

https://leetcode.com/problems/maximize-sum-of-array-after-k-negations/

Negate exactly

```
K
```

elements.

Return the maximum possible array sum.

---

# Greedy Observation

Negative numbers hurt the sum.

Always flip the **most negative** number first.

After all negatives disappear,

if

```
K

is odd
```

flip the smallest absolute value.

---

Example

```
[-5,-3,-1,4]

K=2
```

Flip

```
-5

↓

5
```

Flip

```
-3

↓

3
```

Largest gain.

---

# Visualization

```
Before

-5 -3 -1 4

↓

5 3 -1 4

↓

5 3 1 4
```

---

# Algorithm

Sort array.

Flip negatives while

```
K>0
```

Re-sort.

If

```
K

odd
```

flip smallest element.

Return sum.

---

# Java Solution

```java
import java.util.Arrays;

class Solution {

    public int largestSumAfterKNegations(int[] nums, int k) {

        Arrays.sort(nums);

        for (int i = 0; i < nums.length && k > 0; i++) {

            if (nums[i] < 0) {

                nums[i] = -nums[i];
                k--;
            }
        }

        Arrays.sort(nums);

        if (k % 2 == 1)
            nums[0] = -nums[0];

        int sum = 0;

        for (int num : nums)
            sum += num;

        return sum;
    }
}
```

---

# Complexity

Sorting

```
O(n log n)
```

Space

```
O(1)
```

---

# Why Greedy Works

Flipping a negative changes the sum by

```
2 × |value|
```

Largest negative

↓

Largest improvement.

After no negatives remain,

only parity of

```
K
```

matters.

---

# Interview Follow-ups

- Unlimited negations.
- Negation cost associated with each index.
- Streaming array.

---

# LLM Interview Angle

Candidates should explain

why flipping the most negative element always gives the largest increase.

---

---

# Problem 8 — LeetCode 1710. Maximum Units on a Truck

**Difficulty:** Easy

**Pattern:** Greedy Sorting

**Companies**

- Amazon
- Google
- Meta
- Microsoft

**Problem**

https://leetcode.com/problems/maximum-units-on-a-truck/

Each box type contains

```
[numberOfBoxes, unitsPerBox]
```

A truck can carry at most

```
truckSize
```

boxes.

Return the maximum total units.

---

# Greedy Observation

Each box consumes identical capacity:

```
1 box
```

Therefore always choose the box type with the highest

```
units/box
```

first.

This is identical to selecting the highest value density when all weights are equal.

---

Example

```
Boxes

5 → 10 units

2 → 8 units

4 → 7 units
```

Load order

```
10

↓

8

↓

7
```

Never the reverse.

---

# Visualization

```
Units

10 ██████████

8  ████████

7  ███████
```

Always fill from top.

---

# Algorithm

Sort descending by

```
unitsPerBox
```

For every box type

```
Take

min(boxes, truckSize)
```

Add units.

Decrease capacity.

---

# Java Solution

```java
import java.util.Arrays;

class Solution {

    public int maximumUnits(int[][] boxTypes, int truckSize) {

        Arrays.sort(boxTypes, (a, b) -> b[1] - a[1]);

        int answer = 0;

        for (int[] box : boxTypes) {

            int boxes = Math.min(box[0], truckSize);

            answer += boxes * box[1];

            truckSize -= boxes;

            if (truckSize == 0)
                break;
        }

        return answer;
    }
}
```

---

# Complexity

Sorting

```
O(n log n)
```

Traversal

```
O(n)
```

Space

```
O(1)
```

---

# Why Greedy Works

Each box occupies exactly one slot.

Choosing a lower-value box before a higher-value box can never improve the answer.

This satisfies the greedy-choice property.

---

# Interview Follow-ups

- Boxes have different weights.
- Fractional boxes allowed.
- Two trucks with independent capacities.

These variants transition toward Knapsack problems.

---

# LLM Interview Angle

Interviewers often ask candidates to distinguish this problem from 0/1 Knapsack.

Expected reasoning:

- Equal box weights → Greedy works.
- Arbitrary weights → Dynamic Programming is generally required.

---

## Progress

Completed **8 / 15** problems.

### Greedy Patterns Covered

| # | Problem | Pattern |
|---|---------|---------|
|455|Assign Cookies|Greedy Matching|
|605|Can Place Flowers|Local Greedy|
|860|Lemonade Change|Resource Allocation|
|392|Is Subsequence|Earliest Match|
|134|Gas Station|Greedy + Prefix Sum|
|409|Longest Palindrome|Frequency Greedy|
|1005|K Negations|Greedy Transformation|
|1710|Maximum Units on a Truck|Greedy Sorting|

**Next (Part 3):**
- 435. Non-overlapping Intervals
- 452. Minimum Number of Arrows to Burst Balloons
- 646. Maximum Length of Pair Chain
- 763. Partition Labels
 
---

# Problem 9 — LeetCode 435. Non-overlapping Intervals

**Difficulty:** Medium

**Pattern:** Interval Scheduling

**Companies**

- Google
- Meta
- Amazon
- Microsoft
- Apple

**Problem**

https://leetcode.com/problems/non-overlapping-intervals/

Given an array of intervals where `intervals[i] = [start, end]`, return the **minimum number of intervals to remove** so the remaining intervals are non-overlapping.

---

# Greedy Observation

Instead of deciding **which interval to remove**, decide **which interval to keep**.

Always keep the interval with the **smallest ending time**.

Why?

A smaller ending time leaves maximum room for future intervals.

This is the classic **Activity Selection** greedy strategy.

---

Example

```
Intervals

[1,2]
[2,3]
[3,4]
[1,3]
```

Sort by end

```
[1,2]

[2,3]

[1,3]

[3,4]
```

Keep

```
[1,2]

↓

[2,3]

↓

[3,4]
```

Remove

```
[1,3]
```

Answer = 1

---

# Visualization

```
Timeline

1----2

     2----3

          3----4

1---------3

Remove the longest overlapping interval.
```

---

# Algorithm

1. Sort intervals by ending time.
2. Select the first interval.
3. For every remaining interval:
   - If it doesn't overlap, keep it.
   - Otherwise, increment removal count.

---

# Java Solution

```java
import java.util.Arrays;

class Solution {

    public int eraseOverlapIntervals(int[][] intervals) {

        Arrays.sort(intervals, (a, b) -> Integer.compare(a[1], b[1]));

        int removed = 0;
        int prevEnd = intervals[0][1];

        for (int i = 1; i < intervals.length; i++) {

            if (intervals[i][0] < prevEnd) {

                removed++;

            } else {

                prevEnd = intervals[i][1];
            }
        }

        return removed;
    }
}
```

---

# Complexity

Sorting

```
O(n log n)
```

Traversal

```
O(n)
```

Space

```
O(1)
```

---

# Why Greedy Works

Suppose two overlapping intervals exist.

Keeping the one finishing earlier always leaves at least as much room for every future interval.

Any optimal solution can be transformed into this greedy solution without reducing the answer.

This is the classic **exchange argument**.

---

# ASCII Decision

```
Overlap?

      Yes
       │
Keep Earlier Ending

       │
More Space Later
```

---

# Interview Follow-ups

- Return remaining intervals.
- Weighted intervals.
- Maximum total duration.

Weighted versions require Dynamic Programming.

---

# LLM Interview Angle

Candidates should explain why

> minimizing ending time maximizes future choices.

---

---

# Problem 10 — LeetCode 452. Minimum Number of Arrows to Burst Balloons

**Difficulty:** Medium

**Pattern:** Interval Greedy

**Companies**

- Google
- Amazon
- Microsoft
- Meta

**Problem**

https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/

Each balloon is represented as an interval.

One arrow shot at position `x` bursts every balloon containing `x`.

Return the minimum number of arrows required.

---

# Greedy Observation

Shoot each arrow at the **end of the earliest finishing balloon**.

That single arrow bursts the maximum possible future balloons.

Exactly the same reasoning as interval scheduling.

---

Example

```
[1,6]

[2,8]

[7,12]

[10,16]
```

Arrow

```
↓

6
```

Bursts

```
[1,6]

[2,8]
```

Next arrow

```
↓

12
```

Bursts remaining.

Answer = 2

---

# Visualization

```
1------6

 2--------8

       7------12

          10-------16

Arrow

      ↓6

Arrow

            ↓12
```

---

# Algorithm

1. Sort by interval end.
2. Shoot first arrow at first end.
3. Skip every balloon containing that point.
4. Shoot another arrow only when necessary.

---

# Java Solution

```java
import java.util.Arrays;

class Solution {

    public int findMinArrowShots(int[][] points) {

        Arrays.sort(points, (a, b) -> Long.compare((long)a[1], (long)b[1]));

        int arrows = 1;
        long end = points[0][1];

        for (int i = 1; i < points.length; i++) {

            if (points[i][0] > end) {

                arrows++;
                end = points[i][1];
            }
        }

        return arrows;
    }
}
```

---

# Complexity

Sorting

```
O(n log n)
```

Traversal

```
O(n)
```

Space

```
O(1)
```

---

# Why Greedy Works

Shooting earlier than the earliest end may miss future balloons.

Shooting later cannot burst the earliest balloon.

Therefore the earliest ending coordinate is the unique safe greedy choice.

---

# ASCII Flow

```
Sort

↓

Shoot at End

↓

Overlap?

├── Yes → Same Arrow
└── No  → New Arrow
```

---

# Interview Follow-ups

- Minimize arrow cost.
- Arrow length constraints.
- 2D balloons.

---

# LLM Interview Angle

Tests recognition that this is mathematically identical to Activity Selection.

---

---

# Problem 11 — LeetCode 646. Maximum Length of Pair Chain

**Difficulty:** Medium

**Pattern:** Activity Selection

**Companies**

- Google
- Microsoft
- Amazon

**Problem**

https://leetcode.com/problems/maximum-length-of-pair-chain/

Each pair

```
[a,b]
```

can be followed by

```
[c,d]
```

only if

```
b < c
```

Return the maximum chain length.

---

# Greedy Observation

Again,

choose the pair with the smallest ending value.

Earlier endings maximize remaining options.

Exactly the same proof as Activity Selection.

---

Example

```
[1,2]

[2,3]

[3,4]
```

Chain

```
[1,2]

↓

[3,4]
```

Length = 2

---

# Visualization

```
1---2

    2---3

         3---4
```

---

# Algorithm

Sort by second value.

Take first pair.

Whenever

```
start > previousEnd
```

extend chain.

---

# Java Solution

```java
import java.util.Arrays;

class Solution {

    public int findLongestChain(int[][] pairs) {

        Arrays.sort(pairs, (a, b) -> Integer.compare(a[1], b[1]));

        int count = 1;
        int end = pairs[0][1];

        for (int i = 1; i < pairs.length; i++) {

            if (pairs[i][0] > end) {

                count++;
                end = pairs[i][1];
            }
        }

        return count;
    }
}
```

---

# Complexity

Sorting

```
O(n log n)
```

Traversal

```
O(n)
```

Space

```
O(1)
```

---

# Why Greedy Works

Choosing a later-ending pair only reduces future possibilities.

Choosing the earliest-ending compatible pair is always safe.

---

# Interview Follow-ups

- Weighted chain.
- Maximum chain sum.
- Return actual chain.

Weighted versions require DP.

---

# LLM Interview Angle

Candidates should recognize this as the same greedy proof used in interval scheduling.

---

---

# Problem 12 — LeetCode 763. Partition Labels

**Difficulty:** Medium

**Pattern:** Greedy Boundary Expansion

**Companies**

- Google
- Meta
- Amazon
- Microsoft

**Problem**

https://leetcode.com/problems/partition-labels/

Partition a string into as many parts as possible so that each character appears in **at most one** partition.

Return the sizes of the partitions.

---

# Greedy Observation

Every partition must extend to include the **last occurrence** of every character inside it.

While scanning,

continuously expand the current partition.

Once the current index reaches the farthest required position,

close the partition immediately.

---

Example

```
ababcbacadefegdehijhklij
```

Last occurrences

```
a → 8

b → 5

c → 7
```

Current boundary

```
0

↓

8

↓

Close
```

First partition

```
ababcbaca
```

---

# Visualization

```
a -----------8

b -------5

c --------7

Current End = 8

Reach 8

↓

Partition
```

---

# Algorithm

1. Store last occurrence of every character.
2. Scan string.
3. Maintain current partition end.
4. When index equals end,
   create a partition.

---

# Java Solution

```java
import java.util.ArrayList;
import java.util.List;

class Solution {

    public List<Integer> partitionLabels(String s) {

        int[] last = new int[26];

        for (int i = 0; i < s.length(); i++)
            last[s.charAt(i) - 'a'] = i;

        List<Integer> answer = new ArrayList<>();

        int start = 0;
        int end = 0;

        for (int i = 0; i < s.length(); i++) {

            end = Math.max(end, last[s.charAt(i) - 'a']);

            if (i == end) {

                answer.add(end - start + 1);
                start = i + 1;
            }
        }

        return answer;
    }
}
```

---

# Complexity

Time

```
O(n)
```

Space

```
O(1)
```

---

# Why Greedy Works

Closing the partition before reaching the farthest last occurrence would split a character across two partitions.

Keeping it open longer only reduces the number of partitions.

Therefore closing **at the earliest valid point** is optimal.

---

# ASCII Flow

```
Read Character

↓

Update Last Position

↓

Reached Boundary?

├── No → Continue
└── Yes → Partition
```

---

# Interview Follow-ups

- Return substrings instead of lengths.
- Unicode characters.
- Online streaming version.

---

# LLM Interview Angle

A common explanation-focused question.

Interviewers want candidates to justify why

> the earliest valid cut always maximizes the total number of partitions.

---

## Progress

Completed **12 / 15** problems.

### Greedy Patterns Covered

| # | Problem | Pattern |
|---|---------|---------|
|455|Assign Cookies|Matching|
|605|Can Place Flowers|Local Greedy|
|860|Lemonade Change|Resource Allocation|
|392|Is Subsequence|Earliest Match|
|134|Gas Station|Greedy + Prefix Sum|
|409|Longest Palindrome|Frequency Greedy|
|1005|K Negations|Greedy Transformation|
|1710|Maximum Units on a Truck|Sorting by Profit|
|435|Non-overlapping Intervals|Interval Scheduling|
|452|Minimum Arrows|Interval Cover|
|646|Maximum Pair Chain|Activity Selection|
|763|Partition Labels|Boundary Expansion|

**Next (Part 4):**
- 135. Candy (Hard Greedy)
- 330. Patching Array (Hard)
- 871. Minimum Number of Refueling Stops (Hard)
- FAANG Greedy Recognition Cheat Sheet
- Interview Pattern Summary

---

# Hard Problems

---

# Problem 13 — LeetCode 135. Candy

**Difficulty:** Hard

**Pattern:** Two-Pass Greedy

**Companies**

- Google
- Meta
- Amazon
- Microsoft
- Apple

**Problem**

https://leetcode.com/problems/candy/

There are `n` children standing in a line.

- Every child must receive **at least one candy**.
- A child with a **higher rating** than an adjacent child must receive **more candies**.

Return the **minimum** candies required.

---

# Greedy Observation

A single left-to-right traversal is **not enough**.

Example

```
Ratings

1 2 3 2 1
```

Left pass gives

```
1 2 3 1 1
```

The decreasing sequence violates the rule.

Similarly,

a right-only pass fails for increasing sequences.

Therefore we combine **two greedy passes**.

---

# Why Two Passes?

### Left Pass

Guarantees

```
Higher than left neighbor

↓

More candies
```

Example

```
Ratings

1 2 2

Candies

1 2 1
```

---

### Right Pass

Guarantees

```
Higher than right neighbor

↓

More candies
```

Take

```
max(leftPass, rightPass)
```

to satisfy both constraints.

---

# Visualization

```
Ratings

1 2 3 2 1

Left

1 2 3 1 1

Right

1 1 3 2 1

Final

1 2 3 2 1
```

---

# Algorithm

1. Initialize every child with one candy.
2. Traverse left → right.
3. Traverse right → left.
4. Sum all candies.

---

# Java Solution

```java
import java.util.Arrays;

class Solution {

    public int candy(int[] ratings) {

        int n = ratings.length;
        int[] candy = new int[n];

        Arrays.fill(candy, 1);

        // Left to Right
        for (int i = 1; i < n; i++) {

            if (ratings[i] > ratings[i - 1]) {

                candy[i] = candy[i - 1] + 1;
            }
        }

        // Right to Left
        for (int i = n - 2; i >= 0; i--) {

            if (ratings[i] > ratings[i + 1]) {

                candy[i] = Math.max(candy[i], candy[i + 1] + 1);
            }
        }

        int total = 0;

        for (int c : candy)
            total += c;

        return total;
    }
}
```

---

# Complexity

Time

```
O(n)
```

Space

```
O(n)
```

---

# Why Greedy Works

Each pass fixes one direction independently.

Taking the maximum preserves both constraints while still assigning the minimum possible candies.

This is an example of **local optimality from two complementary greedy passes**.

---

# ASCII Diagram

```
Left Rule

1 → 2 → 3

Right Rule

3 ← 2 ← 1

Combine

↓

Maximum of both
```

---

# Interview Follow-ups

- Circular arrangement.
- Variable minimum candies.
- Children connected as a tree.

These variants generally require graph algorithms or DP.

---

# LLM Interview Angle

A common reasoning question.

Interviewers ask:

> Why doesn't one pass work?

Expected answer:

One traversal can only enforce one directional constraint.

---

---

# Problem 14 — LeetCode 330. Patching Array

**Difficulty:** Hard

**Pattern:** Range Expansion Greedy

**Companies**

- Google
- Meta
- Microsoft

**Problem**

https://leetcode.com/problems/patching-array/

Given a sorted array `nums` and an integer `n`, add the minimum number of numbers so every value in `[1, n]` can be formed.

Return the minimum patches.

---

# Greedy Observation

Suppose we can currently build

```
[1 ... miss)
```

where

```
miss
```

is the smallest missing number.

If

```
nums[i] <= miss
```

then using that number expands coverage.

Otherwise,

the **best patch is exactly `miss`**.

---

Example

```
Covered

[1,4)

Missing

4
```

Patch

```
4
```

Coverage becomes

```
[1,8)
```

No other number expands farther.

---

# Visualization

```
Current Coverage

1------4

Missing

4

Patch

4

↓

New Coverage

1--------------8
```

---

# Algorithm

Maintain

```
miss

index

patches
```

While

```
miss <= n
```

- If current number ≤ miss

```
Extend coverage
```

Else

```
Insert miss
```

---

# Java Solution

```java
class Solution {

    public int minPatches(int[] nums, int n) {

        long miss = 1;
        int patches = 0;
        int i = 0;

        while (miss <= n) {

            if (i < nums.length && nums[i] <= miss) {

                miss += nums[i];
                i++;

            } else {

                miss += miss;
                patches++;
            }
        }

        return patches;
    }
}
```

---

# Complexity

Time

```
O(n)
```

Space

```
O(1)
```

---

# Why Greedy Works

If `miss` cannot be formed,

adding any value larger than `miss` leaves `miss` impossible.

Adding a smaller value expands less coverage.

Therefore patching exactly `miss` maximizes reachable range.

---

# ASCII Flow

```
Can cover

[1, miss)

↓

Next Number <= miss ?

├── Yes → Extend
└── No  → Patch miss
```

---

# Interview Follow-ups

- Unsorted array.
- Minimize patch sum.
- Online insertion.

---

# LLM Interview Angle

Tests proof-based reasoning.

Candidates should justify why

```
Patch = miss
```

is uniquely optimal.

---

---

# Problem 15 — LeetCode 871. Minimum Number of Refueling Stops

**Difficulty:** Hard

**Pattern:** Greedy + Priority Queue

**Companies**

- Google
- Amazon
- Microsoft
- Apple

**Problem**

https://leetcode.com/problems/minimum-number-of-refueling-stops/

A car starts with `startFuel`.

Stations are represented as

```
[position, fuel]
```

Return the minimum number of refueling stops needed to reach the destination.

---

# Greedy Observation

Never decide to refuel immediately.

Instead,

whenever fuel becomes insufficient,

retroactively choose the **largest fuel station already passed**.

A max-heap efficiently supports this.

---

Example

```
Fuel

10

Stations

10 → 60

20 → 30

30 → 30

60 → 40
```

When fuel runs out,

pick

```
60
```

instead of

```
30
```

---

# Visualization

```
Road

Start

↓

10 (60)

↓

20 (30)

↓

30 (30)

↓

Destination

Heap

60

30

30

↓

Always choose largest
```

---

# Algorithm

1. Traverse stations.
2. Push passed station fuel into max-heap.
3. If current fuel is insufficient,
   repeatedly remove the largest fuel amount.
4. Count refuels.

---

# Java Solution

```java
import java.util.PriorityQueue;

class Solution {

    public int minRefuelStops(int target, int startFuel, int[][] stations) {

        PriorityQueue<Integer> maxHeap =
                new PriorityQueue<>((a, b) -> b - a);

        int fuel = startFuel;
        int stops = 0;
        int i = 0;

        while (fuel < target) {

            while (i < stations.length && stations[i][0] <= fuel) {

                maxHeap.offer(stations[i][1]);
                i++;
            }

            if (maxHeap.isEmpty())
                return -1;

            fuel += maxHeap.poll();
            stops++;
        }

        return stops;
    }
}
```

---

# Complexity

Time

```
O(n log n)
```

Space

```
O(n)
```

---

# Why Greedy Works

Whenever refueling becomes necessary,

choosing the largest available fuel postpones the next stop the farthest.

Any smaller choice cannot improve the answer.

The max-heap guarantees the optimal previous station is selected.

---

# ASCII Diagram

```
Passed Stations

60

30

30

↓

Heap

↓

Take Largest

↓

Maximum Reach
```

---

# Interview Follow-ups

- Fuel tank capacity limit.
- Fuel prices differ.
- Multiple vehicles.
- Electric charging stations.

These variants often require shortest-path algorithms, DP, or graph modeling.

---

# LLM Interview Angle

Interviewers frequently ask:

> Why is it valid to delay the refueling decision?

Expected explanation:

Only the set of reachable stations matters. Delaying lets you choose the best previously available station without changing reachability.

---

# FAANG Greedy Recognition Cheat Sheet

| Clue | Likely Greedy Strategy |
|------|-------------------------|
| Maximize number of intervals | Earliest finishing time |
| Minimize removals | Keep earliest ending interval |
| Cover continuous range | Expand current reachable range |
| Assign resources | Smallest sufficient resource |
| Equal weights | Highest value first |
| Increasing/decreasing constraints | Two-pass greedy |
| Online best previous choice | Priority Queue |
| Local decision never hurts future | Exchange Argument |

---

# Common Greedy Proof Techniques

### 1. Exchange Argument

Replace a non-greedy choice with the greedy choice and show the solution does not become worse.

Examples:

- Assign Cookies
- Interval Scheduling
- Pair Chain
- Arrows

---

### 2. Stay Ahead Argument

Show that after every step, the greedy algorithm is at least as good as any other algorithm.

Examples:

- Gas Station
- Maximum Units on Truck
- Patching Array

---

### 3. Cut Property

Once a decision is made, crossing that boundary differently cannot improve the solution.

Examples:

- Partition Labels
- Candy

---

### 4. Greedy + Data Structure

Greedy choice supported by an efficient structure.

Examples:

- Minimum Refueling Stops → Max Heap
- (Related) Course Schedule III → Max Heap

---

# Complete Problem List

| # | Problem | Difficulty | Core Pattern |
|---|---------|------------|--------------|
|1|455. Assign Cookies|Easy|Greedy Matching|
|2|605. Can Place Flowers|Easy|Local Greedy|
|3|860. Lemonade Change|Easy|Resource Allocation|
|4|392. Is Subsequence|Easy|Earliest Match|
|5|134. Gas Station|Medium|Prefix Sum + Greedy|
|6|409. Longest Palindrome|Easy|Frequency Greedy|
|7|1005. Maximize Sum After K Negations|Easy|Greedy Transformation|
|8|1710. Maximum Units on a Truck|Easy|Sorting by Profit|
|9|435. Non-overlapping Intervals|Medium|Interval Scheduling|
|10|452. Minimum Number of Arrows|Medium|Interval Cover|
|11|646. Maximum Length of Pair Chain|Medium|Activity Selection|
|12|763. Partition Labels|Medium|Boundary Expansion|
|13|135. Candy|Hard|Two-Pass Greedy|
|14|330. Patching Array|Hard|Range Expansion|
|15|871. Minimum Refueling Stops|Hard|Greedy + Max Heap|

---

# Interview Revision Checklist

Before an interview, verify that you can answer the following:

- Explain **why** a greedy choice is optimal instead of just implementing it.
- Identify whether the proof uses an **exchange argument**, **stay-ahead argument**, or **cut property**.
- Distinguish Greedy from Dynamic Programming for similar problems.
- Recognize interval scheduling patterns immediately.
- Know when sorting is sufficient and when a Priority Queue is required.
- Handle follow-up variations where greedy no longer applies.

---

# Greedy Pattern Summary

| Pattern | Representative Problems |
|----------|-------------------------|
| Matching | 455 |
| Local Decision | 605 |
| Resource Allocation | 860 |
| Earliest Match | 392 |
| Prefix Greedy | 134 |
| Frequency Greedy | 409 |
| Value Maximization | 1005, 1710 |
| Interval Scheduling | 435, 452, 646 |
| Boundary Expansion | 763 |
| Two-Pass Greedy | 135 |
| Coverage Expansion | 330 |
| Greedy + Heap | 871 |

---

**End of Greedy Interview Guide**

This completes the full set of **15 carefully selected LeetCode Greedy problems** with Java solutions, complexity analysis, FAANG company tags, LLM interview notes, ASCII visualizations, and interview-oriented reasoning.

