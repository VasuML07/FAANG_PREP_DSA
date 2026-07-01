# LeetCode #1 — Two Sum

> **Difficulty:** Easy  
> **Pattern:** HashMap • Complement Lookup • One-Pass Hashing  
> **Companies:** Amazon, Google, Meta, Microsoft, Apple, Uber, Bloomberg, Adobe, Oracle  
> **Importance:** ⭐⭐⭐⭐⭐ (Must Know)

---

# Table of Contents

- Problem Statement
- Why This Problem Matters
- Pattern Recognition
- Brute Force Solution
- Better Solution
- Why HashMap?
- Optimal Solution Overview
- HashMap Internals (Interview Perspective)
- Complexity Comparison

---

# Problem Statement

Given an array of integers `nums` and an integer `target`, return the **indices** of the two numbers such that they add up to `target`.

Assumptions:

- Exactly one solution exists.
- The same element cannot be used twice.
- Return indices in any order.

Example:

```text
nums = [2,7,11,15]
target = 9

Output

[0,1]
```

Explanation

```
2 + 7 = 9
```

---

Another Example

```text
nums = [3,2,4]

target = 6

Output

[1,2]
```

---

# Why This Problem Matters

This is arguably the **most important hashing interview question**.

It teaches one of the most common interview transformations:

```text
Nested Loops

↓

Store Previous Information

↓

HashMap Lookup

↓

Linear Time
```

This pattern appears repeatedly in:

- Pair Sum
- Pair Difference
- Pair Product
- Two Sum II
- Three Sum
- Four Sum
- Subarray Sum Equals K
- Continuous Subarray Sum
- Prefix Sum problems

Mastering this problem makes many future hashing problems much easier.

---

# Pattern Recognition

Whenever you see:

> Find two numbers...

or

> Find two indices...

or

> Does there exist a pair...

Immediately think:

```
HashMap
```

because instead of searching every previous element repeatedly, we can remember what we've already seen.

---

# Understanding the Brute Force Approach

Suppose

```text
nums = [2,7,11,15]

target = 9
```

We compare every pair.

```
2 + 7

YES

Done
```

But imagine

```
nums =

1
5
8
12
13
16
20
...

```

For every element we search every remaining element.

Nested loops.

---

Algorithm

```
for every i

      for every j

             if nums[i]+nums[j]==target

                    return
```

Nothing is remembered.

The same comparisons happen repeatedly.

---

# Brute Force Visualization

```
          2

      7   11   15

Compare

2+7

2+11

2+15

Then

7+11

7+15

Then

11+15
```

Every possible pair is checked.

---

# Brute Force Java Solution

```java
class Solution {

    public int[] twoSum(int[] nums, int target) {

        for(int i = 0; i < nums.length; i++) {

            for(int j = i + 1; j < nums.length; j++) {

                if(nums[i] + nums[j] == target) {

                    return new int[]{i, j};

                }

            }

        }

        return new int[]{};

    }

}
```

---

# Complexity Analysis

| Operation | Complexity |
|------------|-----------|
| Time | O(N²) |
| Space | O(1) |

---

Why?

Outer loop

```
N
```

Inner loop

```
N
```

Total

```
N × N

=

N²
```

Not acceptable for large inputs.

---

# Can We Improve?

Instead of asking:

```
Does another number exist?
```

Ask:

```
Have I already seen

target-currentNumber ?
```

This tiny observation completely changes the algorithm.

---

Suppose

```
Current Number = 7

Target = 9
```

Instead of searching every element

Compute

```
9-7

=

2
```

Now simply ask

```
Have I seen 2?
```

If yes,

Solution found.

---

# Introducing HashMap

HashMap stores

```
Key

↓

Value
```

For this problem

```
Key

↓

Number

Value

↓

Index
```

Example

```
2 → 0

7 → 1

11 → 2

15 → 3
```

Instead of searching,

Lookup happens instantly.

---

# Why HashMap Works

HashMap lookup is approximately

```
O(1)
```

instead of

```
O(N)
```

That means

Old algorithm

```
For every element

↓

Search remaining array

↓

O(N²)
```

New algorithm

```
For every element

↓

One HashMap lookup

↓

O(N)
```

This is the power of hashing.

---

# HashMap Internal View (Interview Perspective)

Imagine inserting

```
2
```

```
Hash(2)

↓

Bucket 4
```

Insert

```
7

↓

Bucket 8
```

Insert

```
11

↓

Bucket 2
```

Internally it resembles

```text
Bucket0

Bucket1

Bucket2
   |
 11

Bucket3

Bucket4
   |
   2

Bucket5

Bucket6

Bucket7

Bucket8
   |
   7
```

Interview point:

We do **not** scan every bucket.

We compute the bucket directly using the key's hash.

Average lookup:

```
O(1)
```

Worst case (many collisions):

```
O(N)
```

In Java 8+, excessive collisions are mitigated by converting long bucket chains into balanced trees, improving worst-case lookup to **O(log N)** for those buckets.

---

# Better Thought Process

Instead of this:

```
Current = 11

Search

2

7

15
```

Think:

```
Need =

Target

-

Current
```

Example

```
Current = 11

Target = 26

Need = 15
```

Now ask HashMap

```
Contains 15?
```

If yes,

Return answer immediately.

---

# Algorithm Idea

For every element

```
Need = target-current
```

If

```
Need
```

already exists in HashMap

Return both indices.

Otherwise

Store current number.

Continue.

---

# Dry Run (Conceptual)

Input

```text
nums = [2,7,11,15]
target = 9
```

Initial state:

```
HashMap = {}
```

### Iteration 1

Current number:

```
2
```

Need:

```
9 - 2 = 7
```

HashMap:

```
{}
```

7 not found.

Store:

```
2 → index 0
```

HashMap becomes:

```
{
2 → 0
}
```

---

### Iteration 2

Current:

```
7
```

Need:

```
2
```

HashMap:

```
{
2 → 0
}
```

2 exists.

Return

```
[0,1]
```

Only two iterations were required.

---

## Key Takeaways

- The transition from **nested search** to **constant-time lookup** is the central idea behind this problem.
- Store values you've already seen, then check whether the required complement already exists.
- This "complement lookup" pattern reappears in dozens of interview questions, making Two Sum the foundation of many hashing techniques.

---

**Next:** Part 2 covers the **optimal one-pass HashMap solution**, complete production-grade Java implementation, detailed execution trace, correctness proof, edge cases, common mistakes, interview follow-ups, and related LeetCode variations.

# Optimal One-Pass HashMap Solution

This is the solution expected in almost every interview.

Instead of:

1. Store every number first.
2. Search later.

We combine both operations into **one traversal**.

---

## Core Observation

For every element:

```text
Current = nums[i]

Need = target - nums[i]
```

If the required number has already been seen, then we have found the answer.

Otherwise, remember the current number for future iterations.

---

## Algorithm

```text
Create HashMap

for each element

    complement = target - current

    if complement exists

         return answer

    else

         store current number
```

Only one traversal of the array is required.

---

## Production-Grade Java Solution

```java
import java.util.HashMap;
import java.util.Map;

class Solution {

    public int[] twoSum(int[] nums, int target) {

        Map<Integer, Integer> map = new HashMap<>();

        for (int i = 0; i < nums.length; i++) {

            int complement = target - nums[i];

            if (map.containsKey(complement)) {
                return new int[]{map.get(complement), i};
            }

            map.put(nums[i], i);
        }

        return new int[]{};
    }
}
```

This is the standard interview solution.

---

# Line-by-Line Walkthrough

## Step 1

```java
Map<Integer, Integer> map = new HashMap<>();
```

Create a map.

```
Key

↓

Number

Value

↓

Index
```

Initially

```
{}
```

---

## Step 2

```java
for(int i=0;i<nums.length;i++)
```

Visit every element exactly once.

```
0

↓

1

↓

2

↓

...

↓

N-1
```

---

## Step 3

```java
int complement = target - nums[i];
```

Suppose

```
Current = 11

Target = 20
```

Then

```
Need

=

9
```

Instead of searching the array,

Ask HashMap

```
Have I already seen 9?
```

---

## Step 4

```java
if(map.containsKey(complement))
```

HashMap lookup

Average

```
O(1)
```

No scanning.

No nested loops.

---

## Step 5

```java
return new int[]{map.get(complement), i};
```

The previous index

comes from

```
map.get(complement)
```

Current index

is

```
i
```

Return both.

---

## Step 6

```java
map.put(nums[i], i);
```

Store current number for future iterations.

Example

```
Current

7

Index

1
```

Store

```
7 → 1
```

---

# Complete Dry Run

Input

```text
nums = [2,7,11,15]

target = 9
```

---

Initial

```
Map

{}
```

---

### i = 0

Current

```
2
```

Need

```
7
```

Lookup

```
Contains 7?

No
```

Insert

```
2 → 0
```

Map

```
{
2 → 0
}
```

---

### i = 1

Current

```
7
```

Need

```
2
```

Lookup

```
Contains 2?

Yes
```

Return

```
[0,1]
```

Done.

---

# Another Dry Run

Input

```text
nums=[3,2,4]

target=6
```

Initial

```
{}
```

---

Current

```
3
```

Need

```
3
```

Not found.

Store

```
3→0
```

---

Current

```
2
```

Need

```
4
```

Not found.

Store

```
2→1
```

---

Current

```
4
```

Need

```
2
```

Found.

Return

```
[1,2]
```

---

# Visualization

```text
Target = 9

Current = 2

Need = 7

Map

{}

↓

Insert

2→0

------------------------

Current = 7

Need = 2

Map

{
2→0
}

↓

Found

↓

Answer
```

---

# Why Do We Check Before Inserting?

Correct order:

```java
if(map.containsKey(complement))
    return ...

map.put(current,index);
```

Wrong order:

```java
map.put(current,index);

if(map.containsKey(complement))
```

This may incorrectly use the same element twice.

---

Example

```text
nums=[3,2,4]

target=6
```

If insertion happens first,

Current

```
3
```

Need

```
3
```

HashMap already contains

```
3
```

because we just inserted it.

Wrong answer.

Checking first prevents using the same index twice.

---

# Correctness Proof

### Invariant

Before processing index `i`, the HashMap contains every element from indices:

```text
0

↓

i-1
```

When processing

```
nums[i]
```

If

```
target-nums[i]
```

exists,

then

```
previous index

+

current index

=

target
```

Otherwise,

store current element.

Since every element is processed exactly once, every valid pair is eventually discovered.

---

# Complexity Analysis

| Operation | Complexity |
|-----------|------------|
| Array Traversal | O(N) |
| HashMap Lookup | O(1) Average |
| HashMap Insert | O(1) Average |

Overall

```
Time

O(N)
```

```
Space

O(N)
```

---

# Comparison

| Approach | Time | Space |
|-----------|------|-------|
| Brute Force | O(N²) | O(1) |
| Sorting + Two Pointers* | O(N log N) | O(1) / O(N) |
| One-Pass HashMap | O(N) | O(N) |

> *Sorting changes the original indices, so extra bookkeeping is required.

---

# Common Interview Mistakes

## 1. Using the Same Element Twice

Wrong

```java
map.put(nums[i], i);

if(map.containsKey(target-nums[i]))
```

Always check first.

---

## 2. Returning Values Instead of Indices

Wrong

```java
return new int[]{nums[i], complement};
```

Question asks for **indices**, not values.

---

## 3. Assuming Sorted Input

Two pointers only work naturally on sorted arrays.

For an unsorted array, HashMap is the preferred solution.

---

## 4. Ignoring Duplicate Values

Example

```text
nums=[3,3]

target=6
```

Correct answer

```
[0,1]
```

The HashMap approach handles duplicates correctly.

---

# Edge Cases

## Single Solution

```text
nums=[1,2]

target=3
```

Output

```text
[0,1]
```

---

## Duplicate Numbers

```text
nums=[3,3]

target=6
```

Output

```text
[0,1]
```

---

## Negative Numbers

```text
nums=[-3,4,3,90]

target=0
```

Output

```text
[0,2]
```

---

## Zero

```text
nums=[0,4,3,0]

target=0
```

Output

```text
[0,3]
```

---

## Large Values

```text
nums=[1000000,-1000000]

target=0
```

Still works correctly because `HashMap<Integer, Integer>` supports the full `int` range.

---

# Interview Follow-Up Questions

### Q1. Can you solve it without extra space?

Yes.

Sort the array and use two pointers.

However:

- Time becomes **O(N log N)**.
- Original indices must be preserved separately.

---

### Q2. Why is HashMap preferred?

Because it provides:

- Average **O(1)** insertion.
- Average **O(1)** lookup.
- Overall **O(N)** solution.

---

### Q3. What is the worst-case complexity of HashMap?

- Average lookup: **O(1)**.
- Worst case due to collisions: **O(N)**.
- In Java 8+, heavily-collided buckets become balanced trees, reducing worst-case bucket operations to **O(log N)**.

---

### Q4. Why not use a HashSet?

A `HashSet` stores only values.

We also need the **index**, so a `HashMap<Value, Index>` is required.

---

# Pattern Recognition

When you encounter phrases like:

- Find two numbers...
- Pair with given sum...
- Complement exists...
- Two indices...
- Pair difference...
- Pair product...

Think:

```text
HashMap

↓

Complement Lookup

↓

One Pass
```

This is one of the most frequently reused hashing patterns in coding interviews.

---

## What's Next

The next chapter covers **LeetCode #217 – Contains Duplicate**, introducing the `HashSet` pattern and explaining when a set is preferable to a map for constant-time membership testing.
