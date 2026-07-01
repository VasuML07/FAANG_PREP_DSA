# Hashing Interview Mastery Guide (Java)

> **FAANG Interview Preparation • Java • LeetCode • Beginner → Advanced**
>
> This guide teaches hashing almost entirely through carefully selected LeetCode problems. Every important interview concept naturally appears while solving real interview questions rather than through isolated theory.

---

# Table of Contents

## Easy

| # | Problem | Difficulty | Technique |
|---|----------|------------|-----------|
|1|Two Sum|Easy|Complement HashMap|
|2|Contains Duplicate|Easy|HashSet|
|3|Valid Anagram|Easy|Frequency Counting|
|4|Intersection of Two Arrays|Easy|HashSet Operations|
|5|Isomorphic Strings|Easy|Bidirectional Character Mapping|

---

## Medium

|#|Problem|Technique|
|---|---|---|
|6|Group Anagrams|Grouping|
|7|Top K Frequent Elements|Frequency + Bucket Sort|
|8|Longest Consecutive Sequence|HashSet Expansion|
|9|Subarray Sum Equals K|Prefix Sum + HashMap|
|10|Longest Substring Without Repeating Characters|Sliding Window|

---

## Hard

|#|Problem|Technique|
|---|---|---|
|11|Minimum Window Substring|Advanced Sliding Window|
|12|Substring with Concatenation of All Words|HashMap + Window|
|13|Alien Dictionary|HashMap Graph|
|14|Find All People With Secret|Hash Graph|
|15|Longest Duplicate Substring|Rolling Hash|

---

# Difficulty Progression

```text
Easy
│
├── HashSet
├── HashMap
├── Frequency Counting
├── Character Mapping
└── Set Operations
        │
        ▼
Medium
│
├── Prefix Sum
├── Sliding Window
├── Grouping
├── Frequency Buckets
└── Sequence Detection
        │
        ▼
Hard
│
├── Complex Windows
├── Rolling Hash
├── Graph Hashing
├── Encoding
└── Hybrid Algorithms
```

---

# Problem 1 — Two Sum

**LeetCode:** 1

**Difficulty:** Easy

**Companies**

- Google
- Amazon
- Microsoft
- Apple
- Meta
- Adobe
- Uber

---

## Problem

Given an integer array and a target, return indices of two numbers whose sum equals the target.

```
nums = [2,7,11,15]

target = 9

Answer

[0,1]
```

---

# Interview Thought Process

Most beginners immediately think:

> Compare every pair.

That works...

But is it optimal?

Suppose

```
100000 numbers
```

Nested loops become very expensive.

Instead ask

> While looking at one number, can I instantly know whether its partner already exists?

That is exactly what hashing provides.

---

# Naive Approach

For every element

Search every remaining element.

```
for i

    for j

        if nums[i]+nums[j]==target

            return
```

Complexity

```
Time : O(n²)

Space : O(1)
```

Far too slow for interviews.

---

# Better Observation

Suppose current number is

```
7
```

Target

```
9
```

Instead of searching every number,

calculate

```
needed = 9-7 = 2
```

Now only one question matters

> Have we already seen 2?

HashMap answers this in nearly constant time.

---

# HashMap Visualization

```
Target = 9

Array

2   7   11   15

Step 1

Store

2 -> index 0

HashMap

+-------+-------+
| Key   | Value |
+-------+-------+
| 2     | 0     |
+-------+-------+

Step 2

Current = 7

Need = 2

Lookup

Found!

Answer

[0,1]
```

---

# Optimized Algorithm

For each number

```
need = target-current

if need exists

    return answer

store current
```

---

# Java Solution

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

        return new int[0];
    }
}
```

---

# Execution Trace

```
nums

[2,7,11,15]

target

9
```

|Step|Current|Need|HashMap|Action|
|---|---|---|---|---|
|1|2|7|{}|Store|
|2|7|2|{2}|Found|
|Done||||Return [0,1]|

---

# Complexity

```
Time

O(n)

Space

O(n)
```

---

# Why Hashing Wins

Without hashing

```
Need

search entire array
```

With hashing

```
Need

direct lookup
```

Interview trick:

Transform

```
Find pair
```

into

```
Find complement
```

---

# HashMap Internal Insight

When storing

```
map.put(2,0)
```

Java computes

```
hash(2)
```

then chooses a bucket.

```
Bucket

0

1

2 ---> (2,0)

3

4
```

Lookup repeats the same computation.

Average lookup

```
O(1)
```

Worst case

```
O(n)
```

Modern Java converts long collision chains into Red-Black Trees, making worst-case lookups approximately **O(log n)** when many keys collide.

---

# Common Pitfalls

### Storing before checking

Wrong

```
put()

containsKey()
```

Fails when

```
target = 6

nums=[3,3]
```

Always

```
Check

↓

Store
```

---

### Returning values instead of indices

Question asks

```
indices
```

not

```
numbers
```

---

### Forgetting duplicates

HashMap naturally handles duplicates correctly when implemented in the proper order.

---

# Interview Variations

- Two Sum II (sorted array)
- Two Sum BST
- Two Sum Data Structure
- 3Sum
- 4Sum

---

# Pattern Learned

```
Complement Hashing
```

Whenever interview asks

```
Find two values
```

think

```
Need = Target - Current
```

---

---

# Problem 2 — Contains Duplicate

**LeetCode:** 217

**Difficulty:** Easy

**Companies**

- Amazon
- Microsoft
- Meta
- Apple

---

# Problem

Return true if any value appears at least twice.

Example

```
[1,2,3,1]

True
```

---

# Observation

Question is equivalent to

> Have I already seen this number?

Perfect HashSet problem.

---

# Naive

Compare every pair.

```
O(n²)
```

---

# Better Approach

Maintain a HashSet.

If current value already exists,

duplicate found.

---

# Visualization

```
Array

1 2 3 1

Set

{}

↓

{1}

↓

{1,2}

↓

{1,2,3}

↓

1 already exists

Return true
```

---

# Java Solution

```java
import java.util.HashSet;
import java.util.Set;

class Solution {

    public boolean containsDuplicate(int[] nums) {

        Set<Integer> seen = new HashSet<>();

        for (int num : nums) {

            if (!seen.add(num)) {
                return true;
            }
        }

        return false;
    }
}
```

---

# Complexity

```
Time

O(n)

Space

O(n)
```

---

# Why HashSet Instead of HashMap?

We only care

```
Exists?

Yes

No
```

No associated value needed.

HashSet is simpler and slightly more memory efficient.

---

# Execution Trace

|Current|Set|Result|
|---|---|---|
|1|{1}|Continue|
|2|{1,2}|Continue|
|3|{1,2,3}|Continue|
|1|Already Exists|True|

---

# HashSet Internal Detail

Internally,

Java HashSet is backed by a HashMap.

Conceptually

```
HashSet

↓

HashMap

Key = number

Value = dummy object
```

Therefore

```
contains()

add()

remove()
```

are all average

```
O(1)
```

---

# Pitfalls

- Sorting first gives O(n log n), which is unnecessary.
- Using a List results in O(n²) due to linear search.

---

# Pattern Learned

```
Duplicate Detection

↓

HashSet
```

---

# Problem 3 — Valid Anagram

**LeetCode:** 242

**Difficulty:** Easy

**Companies**

- Google
- Amazon
- Microsoft
- Meta

---

## Problem

Given two strings `s` and `t`, determine whether `t` is an anagram of `s`.

Example:

```text
s = "anagram"
t = "nagaram"

Output: true
```

---

## Observation

Two strings are anagrams if:

- They contain exactly the same characters.
- Each character appears the same number of times.

Instead of comparing positions, compare **frequencies**.

This introduces one of the most important hashing patterns:

> Frequency Counting

---

## Naive Approach

Sort both strings.

```text
"anagram"

↓

"aaagmnr"

"nagaram"

↓

"aaagmnr"
```

Compare.

### Complexity

```text
Time : O(n log n)

Space : O(1) / O(n)
```

---

## Optimized Approach

Use a HashMap (or array for lowercase English letters).

Count characters in `s`, then subtract counts using `t`.

If every count becomes zero, they are anagrams.

---

## Java Solution (HashMap)

```java
import java.util.HashMap;
import java.util.Map;

class Solution {

    public boolean isAnagram(String s, String t) {

        if (s.length() != t.length()) {
            return false;
        }

        Map<Character, Integer> freq = new HashMap<>();

        for (char c : s.toCharArray()) {
            freq.put(c, freq.getOrDefault(c, 0) + 1);
        }

        for (char c : t.toCharArray()) {

            if (!freq.containsKey(c)) {
                return false;
            }

            freq.put(c, freq.get(c) - 1);

            if (freq.get(c) == 0) {
                freq.remove(c);
            }
        }

        return freq.isEmpty();
    }
}
```

---

## Execution Trace

|Step|Character|HashMap|
|---|---|---|
|Read s|a|{a=1}|
|Read s|n|{a=1,n=1}|
|...|...|...|
|Read t|n|Decrease|
|Read t|a|Decrease|
|End|—|Empty Map|

---

## Complexity

```text
Time : O(n)

Space : O(k)

k = unique characters
```

---

## Why Hashing?

Hashing lets us track frequencies in constant average time per character, avoiding sorting.

---

## Common Pitfalls

- Forgetting to compare lengths first.
- Allowing negative counts.
- Not removing zero-frequency entries when using a HashMap.

---

## Pattern Learned

```text
Frequency Counting

↓

HashMap / HashSet / Array
```

---

*Continue with Problems 4–15 in the subsequent parts.*
