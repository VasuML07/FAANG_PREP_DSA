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
---

# Problem 4 — Intersection of Two Arrays

**LeetCode:** 349

**Difficulty:** Easy

**Companies**

- Google
- Amazon
- Microsoft
- Apple
- Meta

---

## Problem

Given two integer arrays `nums1` and `nums2`, return an array containing **only the unique elements** present in both arrays.

Example

```text
nums1 = [1,2,2,1]
nums2 = [2,2]

Output

[2]
```

---

# Interview Thought Process

The brute-force solution compares every element of one array with every element of the other.

```
for every x in nums1

    for every y in nums2

        if x==y
```

This works but repeatedly compares the same numbers.

Instead ask

> Can membership be checked instantly?

A HashSet provides average **O(1)** lookup.

---

# Naive Approach

Nested loops

```text
Time  : O(n × m)

Space : O(1)
```

---

# Better Observation

Store every element of the first array inside a HashSet.

Then scan the second array.

Whenever an element already exists inside the set,

it belongs to the intersection.

A second HashSet guarantees uniqueness.

---

# Visualization

```
nums1

1 2 2 1

↓

HashSet

{1,2}

nums2

2

↓

Found

Answer Set

{2}
```

---

# Algorithm

```
Create set1

Insert every number of nums1

Create answerSet

For every number in nums2

    if exists in set1

        add to answerSet

Convert answerSet into array
```

---

# Java Solution

```java
import java.util.HashSet;
import java.util.Set;

class Solution {

    public int[] intersection(int[] nums1, int[] nums2) {

        Set<Integer> first = new HashSet<>();

        for (int num : nums1) {
            first.add(num);
        }

        Set<Integer> answer = new HashSet<>();

        for (int num : nums2) {

            if (first.contains(num)) {
                answer.add(num);
            }
        }

        int[] result = new int[answer.size()];
        int index = 0;

        for (int num : answer) {
            result[index++] = num;
        }

        return result;
    }
}
```

---

# Execution Trace

```
nums1

[1,2,2,1]

nums2

[2,2]
```

|Step|Current|Set1|Answer|
|---|---|---|---|
|Insert|1|{1}|{}|
|Insert|2|{1,2}|{}|
|Scan|2|{1,2}|{2}|
|Scan|2|{1,2}|{2}|

Return

```
[2]
```

---

# Complexity

```
Time

O(n+m)

Space

O(n)
```

---

# Why Hashing?

Without hashing

```
Need membership test

↓

Linear search
```

With HashSet

```
Membership

↓

Constant average lookup
```

---

# HashSet Internal Insight

When inserting

```
2
```

Java computes

```
hashCode()

↓

bucket index
```

Conceptually

```
Bucket 0

Bucket 1

Bucket 2 ---> 2

Bucket 3
```

The second occurrence of

```
2
```

is ignored because sets do not allow duplicates.

---

# Common Pitfalls

### Forgetting uniqueness

Many candidates return

```
[2,2]
```

instead of

```
[2]
```

---

### Using ArrayList

Checking

```
contains()
```

inside an ArrayList takes

```
O(n)
```

HashSet provides

```
O(1)
```

average lookup.

---

### Sorting unnecessarily

Sorting both arrays gives

```
O(n log n)
```

which is slower than hashing.

---

# Interview Variations

- Intersection of Two Arrays II
- Union of Two Arrays
- Common Elements in K Arrays
- Distinct Common Characters

---

# Pattern Learned

```text
Membership Checking

↓

HashSet
```

---

---

# Problem 5 — Isomorphic Strings

**LeetCode:** 205

**Difficulty:** Easy

**Companies**

- Google
- Meta
- Microsoft
- Amazon
- Bloomberg

---

## Problem

Two strings are **isomorphic** if characters from one string can be replaced to obtain the other string.

Rules

- Every character maps to exactly one character.
- No two characters map to the same character.

Example

```text
s = "egg"

t = "add"

Output

true
```

Example

```text
s = "foo"

t = "bar"

Output

false
```

---

# Interview Thought Process

Many people only think about

```
a → x
```

But interviews often hide the real constraint.

The mapping must also be unique.

```
a → x

b → x
```

is illegal.

Therefore we must verify **both directions**.

---

# Naive Idea

Store only

```
s → t
```

mapping.

Fails for

```
ab

cc
```

because

```
a→c

b→c
```

looks valid from one direction.

---

# Better Observation

Maintain two HashMaps.

```
Forward

s → t

Backward

t → s
```

Every mapping must agree in both directions.

---

# Visualization

```
egg

↓

add


Map 1

e -> a

g -> d


Map 2

a -> e

d -> g
```

Everything matches.

Answer

```
true
```

---

# Algorithm

For every character position

```
Check forward mapping

Check backward mapping

If conflict

return false

Else

store mapping
```

---

# Java Solution

```java
import java.util.HashMap;
import java.util.Map;

class Solution {

    public boolean isIsomorphic(String s, String t) {

        if (s.length() != t.length()) {
            return false;
        }

        Map<Character, Character> forward = new HashMap<>();
        Map<Character, Character> backward = new HashMap<>();

        for (int i = 0; i < s.length(); i++) {

            char c1 = s.charAt(i);
            char c2 = t.charAt(i);

            if (forward.containsKey(c1) &&
                forward.get(c1) != c2) {
                return false;
            }

            if (backward.containsKey(c2) &&
                backward.get(c2) != c1) {
                return false;
            }

            forward.put(c1, c2);
            backward.put(c2, c1);
        }

        return true;
    }
}
```

---

# Execution Trace

```
s

egg

t

add
```

|Index|Character s|Character t|Forward|Backward|
|---|---|---|---|---|
|0|e|a|e→a|a→e|
|1|g|d|g→d|d→g|
|2|g|d|Already Valid|Already Valid|

Answer

```
true
```

---

# Complexity

```
Time

O(n)

Space

O(k)

k = distinct characters
```

---

# Why Hashing?

Each character mapping is checked in constant average time.

Without hashing,

searching previous mappings repeatedly would require linear scans.

---

# HashMap Concept Introduced

Unlike a HashSet,

a HashMap stores

```
Key

↓

Value
```

Example

```
e

↓

a
```

Internally

```
Bucket

↓

Entry

↓

Key

↓

Value
```

Multiple entries may share a bucket due to collisions.

Modern Java handles heavy collisions efficiently using balanced trees after a threshold is reached.

---

# Common Pitfalls

### Using one HashMap only

Fails for

```
ab

cc
```

---

### Comparing with == on String

Always use

```java
equals()
```

for Strings.

Character primitives are safe with `==`.

---

### Forgetting equal lengths

Different lengths can never be isomorphic.

---

# Edge Cases

```
s = ""

t = ""

true
```

---

```
s = "aa"

t = "ab"

false
```

---

```
s = "paper"

t = "title"

true
```

---

```
s = "badc"

t = "baba"

false
```

---

# Interview Variations

- Word Pattern
- Isomorphic Arrays
- Custom Character Encoding
- One-to-One Mapping Problems

---

# Pattern Learned

```text
Bidirectional Mapping

↓

Two HashMaps
```

---

# Easy Difficulty Summary

| Problem | Core Pattern | Primary Data Structure | Time | Space |
|---------|--------------|------------------------|------|-------|
| Two Sum | Complement Lookup | HashMap | O(n) | O(n) |
| Contains Duplicate | Duplicate Detection | HashSet | O(n) | O(n) |
| Valid Anagram | Frequency Counting | HashMap | O(n) | O(k) |
| Intersection of Two Arrays | Membership Testing | HashSet | O(n+m) | O(n) |
| Isomorphic Strings | Bidirectional Mapping | Two HashMaps | O(n) | O(k) |

---

# Easy-Level Interview Checklist

By this point you should recognize these five fundamental hashing patterns immediately:

| Pattern | Typical Interview Clue |
|----------|------------------------|
| Complement Hashing | "Find two numbers..." |
| Duplicate Detection | "Does any element repeat?" |
| Frequency Counting | "Count occurrences..." |
| Membership Testing | "Exists?" / "Common elements?" |
| Character Mapping | "Replace or transform characters..." |

These five patterns form the foundation for almost every medium-level hashing problem. The next section builds directly on them with grouping, prefix sums, sliding windows, and sequence detection.

# Medium Problems

Medium-level hashing problems combine one or more of the fundamental Easy patterns. The focus shifts from simple lookups to designing algorithms where hashing works together with prefix sums, sliding windows, sorting, or greedy techniques.

---

# Problem 6 — Group Anagrams

**LeetCode:** 49

**Difficulty:** Medium

**Companies**

- Google
- Amazon
- Meta
- Microsoft
- Apple
- Adobe

---

## Problem

Given an array of strings, group the anagrams together.

Example

```text
Input

["eat","tea","tan","ate","nat","bat"]

Output

[
 ["eat","tea","ate"],
 ["tan","nat"],
 ["bat"]
]
```

---

# Interview Thought Process

Anagrams contain exactly the same letters.

Instead of comparing every string with every other string,

create a **signature** that uniquely identifies every anagram group.

For example

```
eat

↓

aet
```

```
tea

↓

aet
```

```
ate

↓

aet
```

All three produce the same signature.

That signature becomes the HashMap key.

---

# Naive Approach

Compare every pair of strings.

```text
Time

O(n² × k)
```

where

```
k = average string length
```

Not scalable.

---

# Better Approach

For every word

```
Sort characters

↓

Use sorted string as HashMap key

↓

Append word to corresponding list
```

---

# Visualization

```text
eat

↓

aet

↓

HashMap

aet → [eat]

----------------

tea

↓

aet

↓

HashMap

aet → [eat, tea]

----------------

ate

↓

aet

↓

HashMap

aet → [eat, tea, ate]
```

---

# Java Solution

```java
import java.util.*;

class Solution {

    public List<List<String>> groupAnagrams(String[] strs) {

        Map<String, List<String>> map = new HashMap<>();

        for (String word : strs) {

            char[] chars = word.toCharArray();
            Arrays.sort(chars);

            String key = new String(chars);

            map.computeIfAbsent(key, k -> new ArrayList<>())
               .add(word);
        }

        return new ArrayList<>(map.values());
    }
}
```

---

# Execution Trace

|Word|Sorted Key|HashMap|
|---|---|---|
|eat|aet|aet→eat|
|tea|aet|aet→eat,tea|
|tan|ant|ant→tan|
|ate|aet|aet→eat,tea,ate|
|nat|ant|ant→tan,nat|
|bat|abt|abt→bat|

---

# Complexity

Sorting each word

```text
Time

O(n × k log k)
```

Space

```text
O(n × k)
```

---

# Why Hashing?

Without hashing

finding an existing group requires scanning previous groups.

HashMap provides direct access.

---

# Advanced Interview Follow-up

Can we avoid sorting?

Yes.

Use a frequency count of 26 letters.

Example

```
eat

↓

1#0#0#...1...
```

This reduces sorting overhead to

```
O(k)
```

per string.

Many Google interviews ask this optimization.

---

# Common Pitfalls

- Forgetting to create a new list for unseen keys.
- Returning the map instead of its values.
- Using mutable arrays directly as HashMap keys.

---

# Pattern Learned

```text
Canonical Representation

↓

HashMap Grouping
```

---

---

# Problem 7 — Top K Frequent Elements

**LeetCode:** 347

**Difficulty:** Medium

**Companies**

- Google
- Amazon
- Meta
- Microsoft
- Apple

---

## Problem

Return the k most frequent elements.

Example

```text
nums

[1,1,1,2,2,3]

k = 2

Output

[1,2]
```

---

# Interview Thought Process

The problem naturally breaks into two steps.

First

```
Count frequency
```

Then

```
Find largest frequencies
```

HashMap solves the first part.

---

# Step 1

Frequency table

```text
1 → 3

2 → 2

3 → 1
```

---

# Naive Approach

Sort frequencies.

```text
Time

O(n log n)
```

Works,

but interviews expect better.

---

# Better Approach

Bucket Sort.

Maximum possible frequency equals array size.

Create

```
bucket[i]

↓

numbers occurring i times
```

---

# Visualization

```text
Frequency

1 → 3

2 → 2

3 → 1


Buckets

0

1 → [3]

2 → [2]

3 → [1]
```

Now traverse buckets backward.

---

# Java Solution

```java
import java.util.*;

class Solution {

    public int[] topKFrequent(int[] nums, int k) {

        Map<Integer, Integer> frequency = new HashMap<>();

        for (int num : nums) {
            frequency.put(num,
                    frequency.getOrDefault(num, 0) + 1);
        }

        List<Integer>[] bucket = new ArrayList[nums.length + 1];

        for (int num : frequency.keySet()) {

            int freq = frequency.get(num);

            if (bucket[freq] == null) {
                bucket[freq] = new ArrayList<>();
            }

            bucket[freq].add(num);
        }

        int[] answer = new int[k];
        int index = 0;

        for (int i = bucket.length - 1;
             i >= 0 && index < k;
             i--) {

            if (bucket[i] == null) {
                continue;
            }

            for (int value : bucket[i]) {

                answer[index++] = value;

                if (index == k) {
                    break;
                }
            }
        }

        return answer;
    }
}
```

---

# Execution Trace

Frequency

```text
1 → 3

2 → 2

3 → 1
```

Buckets

```text
3

↓

1

2

↓

2

1

↓

3
```

Traverse

```
3

↓

2

↓

1
```

Answer

```
[1,2]
```

---

# Complexity

```text
Time

O(n)

Space

O(n)
```

---

# Why Hashing?

Frequency counting is the difficult part.

HashMap computes every frequency in one pass.

---

# Common Pitfalls

- Sorting frequencies unnecessarily.
- Forgetting multiple numbers may share one bucket.
- Returning more than k elements.

---

# Pattern Learned

```text
Frequency Counting

+

Bucket Sort
```

---

---

# Problem 8 — Longest Consecutive Sequence

**LeetCode:** 128

**Difficulty:** Medium

**Companies**

- Google
- Amazon
- Meta
- Microsoft

---

## Problem

Find the length of the longest consecutive sequence.

Example

```text
[100,4,200,1,3,2]

Output

4

Sequence

1 2 3 4
```

---

# Interview Thought Process

Sorting works.

But

```
O(n log n)
```

is not optimal.

HashSet allows us to discover sequences without sorting.

---

# Key Observation

Only start counting when

```
number-1

does not exist.
```

That guarantees we're at the beginning of a sequence.

---

# Visualization

```text
Set

{100,4,200,1,3,2}


1

No predecessor

↓

Start

1

↓

2

↓

3

↓

4

Length = 4


2

Has predecessor

↓

Skip
```

Every sequence is explored exactly once.

---

# Java Solution

```java
import java.util.HashSet;
import java.util.Set;

class Solution {

    public int longestConsecutive(int[] nums) {

        Set<Integer> set = new HashSet<>();

        for (int num : nums) {
            set.add(num);
        }

        int longest = 0;

        for (int num : set) {

            if (!set.contains(num - 1)) {

                int current = num;
                int length = 1;

                while (set.contains(current + 1)) {
                    current++;
                    length++;
                }

                longest = Math.max(longest, length);
            }
        }

        return longest;
    }
}
```

---

# Execution Trace

```text
Set

{100,4,200,1,3,2}
```

Check

```
100

↓

Start

Length = 1
```

Check

```
4

↓

Has predecessor

Skip
```

Check

```
1

↓

Start

↓

2

↓

3

↓

4

Length = 4
```

Maximum

```
4
```

---

# Complexity

```text
Time

O(n)

Space

O(n)
```

---

# Why Hashing?

HashSet converts

```
Does x exist?
```

into an average O(1) operation.

Without hashing,

every lookup becomes linear.

---

# Common Pitfalls

- Starting from every number.
- Sorting first.
- Forgetting duplicate values.

---

# Pattern Learned

```text
Sequence Detection

↓

HashSet
```

---

# Medium Problems Covered So Far

| Problem | Primary Pattern | Data Structure |
|----------|-----------------|----------------|
| Group Anagrams | Canonical Key | HashMap |
| Top K Frequent Elements | Frequency + Buckets | HashMap |
| Longest Consecutive Sequence | Sequence Expansion | HashSet |

The next section continues with two of the most important FAANG hashing patterns:

- **Problem 9:** Prefix Sum + HashMap (`Subarray Sum Equals K`)
- **Problem 10:** Sliding Window + HashMap (`Longest Substring Without Repeating Characters`)

- ---

# Problem 9 — Subarray Sum Equals K

**LeetCode:** 560

**Difficulty:** Medium

**Companies**

- Google
- Meta
- Amazon
- Microsoft
- Bloomberg
- Uber

---

## Problem

Given an integer array `nums` and an integer `k`, return the **total number of continuous subarrays** whose sum equals `k`.

Example

```text
nums = [1,1,1]

k = 2

Output

2
```

Subarrays

```text
[1,1]
   [1,1]
```

---

# Interview Thought Process

The brute-force solution is straightforward.

Generate every possible subarray.

Compute its sum.

Count those equal to `k`.

Works...

but becomes quadratic.

---

# Naive Approach

```text
for every start

    sum = 0

    for every end

        sum += nums[end]

        if sum == k

            answer++
```

Complexity

```text
Time

O(n²)

Space

O(1)
```

---

# Key Observation

Suppose

```
PrefixSum(i) = sum from 0...i
```

Then

```
SubarraySum(l,r)

=

Prefix[r]-Prefix[l-1]
```

If

```
Prefix[r]-Prefix[l-1]=k
```

then

```
Prefix[l-1]

=

Prefix[r]-k
```

So instead of searching all previous indices,

ask:

> **How many times has `(currentPrefix - k)` appeared before?**

That is exactly what a HashMap stores.

---

# Visualization

```
nums

1 1 1

Prefix Sums

1

2

3


Need

currentPrefix-k
```

Step-by-step

```
Prefix = 1

Need = -1

Not found

Store

1


Prefix = 2

Need = 0

Found once

Answer = 1


Prefix = 3

Need = 1

Found once

Answer = 2
```

---

# Why Initialize

```
0 → 1
```

Before Traversal?

This represents

```
Empty Prefix
```

Without it,

subarrays beginning at index 0 would never be counted.

Example

```
nums

[2]

k=2
```

Without

```
0→1
```

answer becomes

```
0
```

instead of

```
1
```

---

# Optimized Algorithm

```
HashMap

PrefixSum

↓

Frequency

Initialize

0→1

For every number

    prefix += number

    answer += map[prefix-k]

    store prefix
```

---

# Java Solution

```java
import java.util.HashMap;
import java.util.Map;

class Solution {

    public int subarraySum(int[] nums, int k) {

        Map<Integer, Integer> prefixCount = new HashMap<>();

        prefixCount.put(0, 1);

        int prefix = 0;
        int answer = 0;

        for (int num : nums) {

            prefix += num;

            answer += prefixCount.getOrDefault(prefix - k, 0);

            prefixCount.put(
                    prefix,
                    prefixCount.getOrDefault(prefix, 0) + 1
            );
        }

        return answer;
    }
}
```

---

# Execution Trace

Example

```text
nums

[1,2,3]

k=3
```

|Index|Number|Prefix|Need|Map Before|Answer|
|---|---|---|---|---|---|
|0|1|1|-2|{0=1}|0|
|1|2|3|0|{0=1,1=1}|1|
|2|3|6|3|{0=1,1=1,3=1}|2|

Subarrays

```text
[1,2]

[3]
```

---

# Complexity

```text
Time

O(n)

Space

O(n)
```

---

# Why Hashing?

Without hashing

finding previous prefix sums requires scanning all previous elements.

With HashMap

```
Lookup

↓

O(1)
```

average.

---

# HashMap Concept Introduced

Notice the HashMap stores

```
Prefix Sum

↓

Frequency
```

NOT

```
Prefix Sum

↓

Index
```

Why?

The same prefix can occur multiple times.

Example

```text
0 0 0
```

Prefix sums

```text
0

0

0
```

Each occurrence contributes additional valid subarrays.

---

# Common Pitfalls

### Forgetting

```
0 → 1
```

The most common interview mistake.

---

### Using HashSet

HashSet only answers

```
Exists?
```

This problem needs

```
How many times?
```

Therefore

```
HashMap
```

is required.

---

### Sliding Window

Sliding Window fails because negative numbers may exist.

Prefix Sum works for both positive and negative values.

---

# Edge Cases

```
nums = []

Answer

0
```

---

```
nums=[0,0,0]

k=0
```

Expected

```
6
```

---

```
Negative numbers

Supported
```

---

# Pattern Learned

```text
Prefix Sum

+

HashMap Frequency
```

---

---

# Problem 10 — Longest Substring Without Repeating Characters

**LeetCode:** 3

**Difficulty:** Medium

**Companies**

- Google
- Amazon
- Microsoft
- Meta
- Apple
- Adobe

---

## Problem

Find the length of the longest substring containing no repeated characters.

Example

```text
Input

"abcabcbb"

Output

3
```

Longest substring

```text
abc
```

---

# Interview Thought Process

The brute-force approach generates every substring.

For each substring,

check whether characters repeat.

Very expensive.

Instead,

maintain a **sliding window**.

---

# Naive Approach

```text
Generate every substring

↓

Check duplicates
```

Complexity

```text
Time

O(n²)
```

or worse.

---

# Better Observation

Maintain

```
Left

Right
```

Pointers.

Expand

```
Right
```

If a duplicate appears,

move

```
Left
```

until the duplicate disappears.

HashMap remembers the most recent position of every character.

---

# Visualization

```
abcabcbb


Window

[a]

↓

[ab]

↓

[abc]

↓

Duplicate a

Move left

↓

[bca]

↓

[cab]

↓

...
```

---

# Why HashMap Instead of HashSet?

HashSet works,

but repeated removals make the algorithm slightly less elegant.

HashMap stores

```
Character

↓

Last Index
```

allowing

```
Left

↓

Jump directly
```

instead of removing one character at a time.

---

# Algorithm

```
HashMap

Character

↓

Last Index

For every Right

If character already inside window

    Left = max(Left,lastIndex+1)

Update last index

Update answer
```

---

# Java Solution

```java
import java.util.HashMap;
import java.util.Map;

class Solution {

    public int lengthOfLongestSubstring(String s) {

        Map<Character, Integer> lastSeen = new HashMap<>();

        int left = 0;
        int answer = 0;

        for (int right = 0; right < s.length(); right++) {

            char current = s.charAt(right);

            if (lastSeen.containsKey(current)) {

                left = Math.max(
                        left,
                        lastSeen.get(current) + 1
                );
            }

            lastSeen.put(current, right);

            answer = Math.max(
                    answer,
                    right - left + 1
            );
        }

        return answer;
    }
}
```

---

# Execution Trace

Example

```text
abcabcbb
```

|Right|Character|Left|Window|Length|
|---|---|---|---|---|
|0|a|0|a|1|
|1|b|0|ab|2|
|2|c|0|abc|3|
|3|a|1|bca|3|
|4|b|2|cab|3|
|5|c|3|abc|3|
|6|b|5|cb|2|
|7|b|7|b|1|

Answer

```
3
```

---

# Complexity

```text
Time

O(n)

Space

O(min(n, charset))
```

---

# Why Hashing?

The difficult operation is

```
Where was this character last seen?
```

HashMap answers instantly.

Without hashing,

finding the previous occurrence requires scanning the window.

---

# HashMap Internal Insight

Each entry stores

```text
Character

↓

Last Position
```

Example

```text
a → 3

b → 4

c → 5
```

This allows the left pointer to jump instead of moving one step at a time.

Overall,

each character enters and leaves the window at most once,

leading to linear complexity.

---

# Common Pitfalls

### Using

```java
left = lastSeen.get(c) + 1;
```

Incorrect.

Must write

```java
left = Math.max(left,
                lastSeen.get(c)+1);
```

Otherwise,

the left pointer may move backwards.

---

### Forgetting to update the map

Always overwrite the previous index.

---

### Confusing substring with subsequence

Substring

```
Continuous
```

Subsequence

```
Can skip characters
```

---

# Edge Cases

```
""

Answer

0
```

---

```
"bbbb"

Answer

1
```

---

```
"abcdef"

Answer

6
```

---

# Pattern Learned

```text
Sliding Window

+

HashMap
```

---

# Medium Difficulty Summary

| Problem | Primary Pattern | Data Structure | Time | Space |
|----------|-----------------|----------------|------|-------|
| Group Anagrams | Canonical Representation | HashMap | O(n·k log k) | O(n·k) |
| Top K Frequent Elements | Frequency Counting + Buckets | HashMap | O(n) | O(n) |
| Longest Consecutive Sequence | Sequence Detection | HashSet | O(n) | O(n) |
| Subarray Sum Equals K | Prefix Sum | HashMap | O(n) | O(n) |
| Longest Substring Without Repeating Characters | Sliding Window | HashMap | O(n) | O(min(n, charset)) |

---

# Medium-Level Pattern Recognition

| Interview Clue | Think Immediately |
|---------------|-------------------|
| "Group similar strings" | Canonical Key + HashMap |
| "Most frequent..." | Frequency Map |
| "Longest consecutive..." | HashSet |
| "Subarray sum..." | Prefix Sum + HashMap |
| "Longest substring..." | Sliding Window + HashMap |

These five patterns account for a significant portion of medium-level hashing questions asked in FAANG interviews. The Hard section combines these ideas with graphs, rolling hashes, and advanced window techniques.

# Hard Problems

Hard hashing questions rarely introduce completely new data structures. Instead, they combine the patterns you've already learned:

- HashMap + Sliding Window
- HashMap + Graph
- HashMap + Prefix Structures
- HashMap + Rolling Hash
- Frequency Maps + Window Optimization

Master these five problems and you'll recognize most hard hashing interview patterns.

---

# Problem 11 — Minimum Window Substring

**LeetCode:** 76

**Difficulty:** Hard

**Companies**

- Google
- Meta
- Amazon
- Microsoft
- Apple
- Bloomberg

---

## Problem

Given two strings `s` and `t`, return the **minimum window substring** of `s` that contains every character of `t`.

Example

```text
s = "ADOBECODEBANC"

t = "ABC"

Output

"BANC"
```

---

# Interview Thought Process

A brute-force approach generates every substring and checks whether it contains all required characters.

```text
O(n²)
```

or worse.

Instead,

maintain a sliding window.

Expand until all required characters are present.

Then shrink the window as much as possible.

---

# Key Observation

Maintain two frequency maps.

```
Required

↓

Characters of t
```

```
Window

↓

Current substring
```

When

```
Window == Required
```

try shrinking from the left.

---

# Visualization

```text
A D O B E C O D E B A N C

Expand →

A D O B E C

Valid

↓

Shrink ←

Still valid?

↓

No

Expand again
```

Eventually

```
BANC
```

becomes the smallest valid window.

---

# Algorithm

```
Count characters of t

Expand right pointer

Update current window

When window satisfies all requirements

    Update answer

    Shrink left

Repeat
```

---

# Java Solution

```java
import java.util.HashMap;
import java.util.Map;

class Solution {

    public String minWindow(String s, String t) {

        if (s.length() < t.length()) {
            return "";
        }

        Map<Character, Integer> need = new HashMap<>();

        for (char c : t.toCharArray()) {
            need.put(c, need.getOrDefault(c, 0) + 1);
        }

        Map<Character, Integer> window = new HashMap<>();

        int formed = 0;
        int required = need.size();

        int left = 0;

        int minLength = Integer.MAX_VALUE;
        int start = 0;

        for (int right = 0; right < s.length(); right++) {

            char c = s.charAt(right);

            window.put(c,
                    window.getOrDefault(c, 0) + 1);

            if (need.containsKey(c) &&
                    window.get(c).intValue() ==
                            need.get(c).intValue()) {

                formed++;
            }

            while (formed == required) {

                if (right - left + 1 < minLength) {

                    minLength = right - left + 1;
                    start = left;
                }

                char remove = s.charAt(left);

                window.put(remove,
                        window.get(remove) - 1);

                if (need.containsKey(remove) &&
                        window.get(remove) <
                                need.get(remove)) {

                    formed--;
                }

                left++;
            }
        }

        return minLength == Integer.MAX_VALUE
                ? ""
                : s.substring(start,
                start + minLength);
    }
}
```

---

# Complexity

```text
Time

O(n)

Space

O(k)
```

---

# Why Hashing?

Both frequency maps require constant-time updates.

Without hashing,

checking every window would require rescanning characters repeatedly.

---

# Common Pitfalls

- Comparing total frequencies instead of distinct satisfied characters.
- Forgetting duplicate letters in `t`.
- Shrinking the window too early.

---

# Pattern Learned

```text
Sliding Window

+

Dual HashMaps
```

---

---

# Problem 12 — Substring with Concatenation of All Words

**LeetCode:** 30

**Difficulty:** Hard

**Companies**

- Google
- Amazon
- Microsoft
- Meta

---

## Problem

Given a string and a list of equal-length words,

find all starting indices where every word appears exactly once.

Example

```text
s

"barfoothefoobarman"

words

["foo","bar"]

Output

[0,9]
```

---

# Interview Thought Process

This is a sliding-window problem,

but instead of characters,

the window moves one **word** at a time.

HashMap stores the required frequencies of words.

---

# Visualization

```text
bar foo the foo bar man

↓

bar foo

✓

Index 0


foo the

✗


foo bar

✓

Index 9
```

---

# Approach

```
Store required frequencies

Move window in word-sized jumps

Maintain current frequency map

Compare maps
```

---

# Complexity

```text
Time

O(n × wordLength)

Space

O(number of words)
```

---

# Why Hashing?

Words can repeat.

HashMap efficiently tracks frequencies inside the current window.

---

# Common Pitfalls

- Sliding character-by-character instead of word-by-word.
- Ignoring duplicate words.
- Not resetting the window correctly after an invalid word.

---

# Pattern Learned

```text
Sliding Window

+

Word Frequency Map
```

---

---

# Problem 13 — Alien Dictionary

**LeetCode:** 269 (Premium)

**Difficulty:** Hard

**Companies**

- Google
- Meta
- Amazon
- Microsoft

---

## Problem

Given a sorted dictionary of an unknown language,

return one valid ordering of its characters.

---

# Interview Thought Process

Hashing alone isn't enough.

We must build

```
Character

↓

Neighbors
```

This naturally forms a graph.

HashMap stores the adjacency list.

---

# Visualization

```text
wrt

wrf

↓

t → f


wrf

er

↓

w → e


Graph

w

↓

e

↓

r

↓

t

↓

f
```

Topological Sort produces

```
wertf
```

---

# Java Idea

```text
HashMap<Character, Set<Character>>

↓

Adjacency List

+

Indegree Map

↓

BFS (Kahn's Algorithm)
```

---

# Complexity

```text
Time

O(V+E)

Space

O(V+E)
```

---

# Why Hashing?

Character lookup must be constant time.

HashMap stores

```
Character

↓

Neighbors
```

efficiently.

---

# Common Pitfalls

- Ignoring prefix conflicts.
- Adding duplicate edges.
- Forgetting isolated characters.

---

# Pattern Learned

```text
Graph

+

HashMap
```

---

---

# Problem 14 — Find All People With Secret

**LeetCode:** 2092

**Difficulty:** Hard

**Companies**

- Google
- Meta
- Amazon

---

## Problem

People meet at different times.

A secret spreads only among connected people during the same timestamp.

Find everyone who eventually learns the secret.

---

# Interview Thought Process

Meetings naturally create a graph.

But each timestamp represents a different temporary graph.

HashMap groups meetings by time.

---

# Visualization

```text
Time

1

↓

0—2

↓

2—5


Time

3

↓

5—8

↓

8—9
```

Each timestamp

↓

Build graph

↓

DFS/BFS

↓

Discard graph

---

# Approach

```
Group meetings by time

↓

HashMap

Time

↓

Meeting List

↓

Process timestamps in order

↓

Temporary graph

↓

DFS/BFS
```

---

# Complexity

```text
Time

O(n log n)

Space

O(n)
```

---

# Why Hashing?

Grouping meetings by timestamp allows efficient reconstruction of temporary graphs.

---

# Common Pitfalls

- Mixing meetings from different timestamps.
- Not rebuilding graphs for each time.
- Forgetting disconnected components.

---

# Pattern Learned

```text
Temporal Graph

+

HashMap Grouping
```

---

---

# Problem 15 — Longest Duplicate Substring

**LeetCode:** 1044

**Difficulty:** Hard

**Companies**

- Google
- Amazon
- Meta

---

## Problem

Find the longest substring that appears at least twice.

---

# Interview Thought Process

Comparing every pair of substrings is impossible.

Instead,

combine

```
Binary Search

+

Rolling Hash
```

Hashing converts each substring into a numeric fingerprint.

---

# Rolling Hash Concept

Instead of storing

```
banana
```

store

```
Hash(banana)
```

Adjacent substrings reuse previous computations.

```
banana

↓

anana

↓

nana

↓

Rolling Hash
```

---

# Visualization

```text
Substring Length = 5

banana

↓

Hash1

anana

↓

Hash2

nana

↓

Hash3
```

Repeated hashes

↓

Possible duplicate

↓

Verify

---

# Algorithm

```
Binary Search

↓

Guess length

↓

Rolling Hash

↓

HashSet

↓

Duplicate?

↓

Increase length

Else

Decrease length
```

---

# Complexity

```text
Time

O(n log n)

Space

O(n)
```

---

# Why Hashing?

Without hashing,

comparing every substring requires

```text
O(n²)
```

or worse.

Rolling hash reduces substring comparison to nearly constant time.

---

# Common Pitfalls

- Assuming equal hashes always mean equal strings (hash collisions).
- Forgetting collision verification.
- Integer overflow in hash computation.

---

# Collision Handling

Two different strings can produce the same hash.

Example

```text
Hash(A)

=

Hash(B)
```

This is called a **collision**.

To avoid incorrect answers,

verify actual substrings after matching hashes.

Some implementations also use

```
Double Hashing
```

to make collisions extremely unlikely.

---

# Pattern Learned

```text
Binary Search

+

Rolling Hash

+

HashSet
```

---

# Hard Difficulty Summary

| Problem | Main Technique | Data Structure | Time |
|----------|----------------|----------------|------|
| Minimum Window Substring | Sliding Window | HashMap | O(n) |
| Substring with Concatenation of All Words | Word Window | HashMap | O(n × wordLength) |
| Alien Dictionary | Graph | HashMap | O(V+E) |
| Find All People With Secret | Temporal Graph | HashMap | O(n log n) |
| Longest Duplicate Substring | Rolling Hash | HashSet | O(n log n) |

---

# Complete Pattern Cheat Sheet

| Pattern | Representative Problem |
|----------|------------------------|
| Complement Lookup | Two Sum |
| Duplicate Detection | Contains Duplicate |
| Frequency Counting | Valid Anagram |
| Membership Testing | Intersection of Two Arrays |
| Character Mapping | Isomorphic Strings |
| Canonical Representation | Group Anagrams |
| Frequency + Buckets | Top K Frequent Elements |
| Sequence Detection | Longest Consecutive Sequence |
| Prefix Sum + HashMap | Subarray Sum Equals K |
| Sliding Window | Longest Substring Without Repeating Characters |
| Advanced Sliding Window | Minimum Window Substring |
| Word-Based Window | Substring with Concatenation of All Words |
| Graph Representation | Alien Dictionary |
| Time-Based Graph | Find All People With Secret |
| Rolling Hash | Longest Duplicate Substring |

---

At this point, all **15 representative LeetCode problems** have been covered. The remaining final section includes:

- Company-wise hashing patterns
- Java `HashMap` internals (bucket array, resizing, treeification)
- Collision handling (chaining vs linear probing)
- `HashMap` vs `HashSet` vs `Hashtable` vs `ConcurrentHashMap`
- Memory management and GC considerations
- Consistent Hashing & Distributed Hashing
- Thread safety
- Hash function visualizations
- LLM-proof interview questions
- Final FAANG revision cheat sheet

  # Advanced Topics, Java Internals, Company Patterns & Final Revision

---

# FAANG Company-Wise Hashing Patterns

Different companies tend to emphasize different hashing patterns.

| Company | Frequently Asked Hashing Patterns | Representative Problems |
|----------|-----------------------------------|-------------------------|
| Google | Rolling Hash, Sliding Window, Graph Hashing | Longest Duplicate Substring, Minimum Window Substring, Alien Dictionary |
| Meta | Prefix Sum, Frequency Maps, String Hashing | Subarray Sum Equals K, Group Anagrams |
| Amazon | HashMap + Arrays, Frequency Counting | Two Sum, Top K Frequent Elements |
| Microsoft | Character Mapping, Prefix Sum, Windows | Isomorphic Strings, Longest Substring |
| Apple | Sliding Window, String Processing | Minimum Window Substring |
| Uber | Prefix Sum, Graph Hashing | Subarray Sum Equals K |
| Bloomberg | Frequency Counting, Maps | Valid Anagram, Top K Frequent |
| Adobe | String Mapping | Isomorphic Strings |

---

# Company Interview Focus

## Google

Usually asks

- Sliding Window
- Rolling Hash
- Graph + HashMap
- Hybrid algorithms

Difficulty

```
Medium → Hard
```

---

## Amazon

Frequently tests

- HashMap basics
- Frequency counting
- Arrays
- Optimized lookups

Difficulty

```
Easy → Medium
```

---

## Meta

Focus

- Prefix Sum
- String problems
- Character frequency
- Sliding windows

---

## Microsoft

Emphasizes

- Clean implementation
- Edge cases
- Character mapping
- Window problems

---

# Java HashMap Internal Working

Suppose we insert

```java
map.put("CAT", 5);
```

Internally

```
Key

↓

hashCode()

↓

Hash Mixing

↓

Bucket Index

↓

Store Entry
```

---

## Internal Structure

```text
Bucket Array

+--------------------------------------------------+
|0|1|2|3|4|5|6|7|8|9|10|11|12|13|14|15|
+--------------------------------------------------+
          |
          ▼

      Node
      +----------------------+
      | Key   = CAT          |
      | Value = 5            |
      | Next  ------------+  |
      +-------------------|--+
                          |
                          ▼
                     Next Node
```

Each bucket stores

```
Key

Value

Hash

Next Pointer
```

---

# Collision Handling

Two different keys may produce the same bucket.

Example

```
CAT

↓

Bucket 5


DOG

↓

Bucket 5
```

---

## Chaining

Java stores both nodes inside the same bucket.

```text
Bucket

↓

CAT

↓

DOG

↓

FOX
```

Average lookup

```
O(1)
```

---

## Treeification (Java 8+)

If one bucket becomes too long

```
Linked List

↓

Red Black Tree
```

```text
CAT

   DOG

FOX

    RAT

OWL
```

Worst-case lookup improves from

```
O(n)

↓

O(log n)
```

---

# HashMap Resize

Default capacity

```
16
```

Default load factor

```
0.75
```

Resize happens when

```
size >

capacity × loadFactor
```

Example

```
16 × 0.75

=

12
```

After inserting the 13th element

```
Capacity

16

↓

32
```

---

## Resize Visualization

Before

```text
Capacity = 16

Buckets

0 1 2 3 ... 15
```

After

```text
Capacity = 32

Buckets

0 1 2 3 ... 31
```

All entries are rehashed into the new bucket array.

---

# Load Factor

A higher load factor means

- Less memory
- More collisions

A lower load factor means

- Faster lookup
- More memory

Default

```java
0.75
```

provides a good balance.

---

# HashMap vs HashSet vs Hashtable vs ConcurrentHashMap

| Feature | HashMap | HashSet | Hashtable | ConcurrentHashMap |
|----------|---------|----------|------------|-------------------|
| Stores | Key → Value | Keys Only | Key → Value | Key → Value |
| Thread Safe | No | No | Yes | Yes |
| Allows null key | Yes | Yes | No | No |
| Allows null value | Yes | N/A | No | No |
| Performance | Fast | Fast | Slow | Very Fast Concurrently |
| Synchronization | None | None | Entire table | Fine-grained |

---

# When to Use What?

## HashMap

Use when

```
Need

Key

↓

Value
```

Examples

- Two Sum
- Prefix Sum
- Frequency Counting

---

## HashSet

Use when

```
Only membership matters.
```

Examples

- Contains Duplicate
- Longest Consecutive Sequence
- Intersection

---

## Hashtable

Legacy class.

Usually avoid in interviews unless specifically asked.

---

## ConcurrentHashMap

Preferred in multithreaded applications.

Example

```
Multiple threads

↓

Read

Write

Safely
```

Unlike `Hashtable`, it does not lock the entire map for every operation, allowing better scalability.

---

# Memory & Garbage Collection Considerations

Each `HashMap` entry stores more than just the key and value.

Conceptually

```text
Node

↓

Hash

Key

Value

Next Pointer
```

Therefore

Large HashMaps consume significant heap memory.

---

## Frequent Resizing

Repeated expansion

```
16

↓

32

↓

64

↓

128
```

creates temporary objects that increase GC activity.

If the expected size is known,

initialize capacity.

Example

```java
Map<Integer, Integer> map = new HashMap<>(100000);
```

This reduces unnecessary rehashing.

---

# Thread Safety

`HashMap`

```text
✓ Fast

✗ Not Thread Safe
```

Concurrent writes may produce incorrect results.

Use

```java
ConcurrentHashMap
```

for concurrent applications.

---

# Consistent Hashing

Used in

- Distributed Databases
- Caching Systems
- Load Balancers
- Distributed Storage

Instead of

```
Server = hash(key)%N
```

which causes massive redistribution when servers change,

consistent hashing places servers on a ring.

---

## Consistent Hash Ring

```text
                Server A
                  ●

        ●                     ●
     Server D             Server B


             Hash Ring


                 ●
             Server C
```

Each key moves only to the next server clockwise.

Adding one server affects only nearby keys.

---

# Why Companies Use It

Examples

- Cassandra
- DynamoDB
- Redis Cluster
- Memcached
- Apache Kafka partitioning (related concepts)

---

# Distributed Hashing

Large systems split data across many machines.

```text
Client

↓

Hash(Key)

↓

Machine 4

↓

Retrieve Data
```

Benefits

- Scalability
- Parallelism
- Fault Isolation

---

# Hash Function Distribution

Good hash function

```text
Buckets

0  ███

1  ███

2  ███

3  ███

4  ███

5  ███
```

Even distribution minimizes collisions.

---

Bad hash function

```text
Buckets

0  █████████████████

1

2

3

4

5
```

Many collisions

↓

Poor performance

---

# Collision Resolution

## Separate Chaining (Java)

```text
Bucket

↓

A

↓

B

↓

C
```

Pros

- Simple
- Flexible
- Used by Java HashMap

---

## Linear Probing

```text
Bucket

5

Occupied

↓

6

Occupied

↓

7

Empty

↓

Insert
```

Pros

- Cache friendly

Cons

- Primary clustering

---

# Complexity Summary

| Operation | Average | Worst |
|------------|---------|--------|
| Insert | O(1) | O(log n)* |
| Search | O(1) | O(log n)* |
| Delete | O(1) | O(log n)* |

\*Assuming Java 8+ treeified buckets under heavy collisions.

---

# LLM-Proof Interview Questions

These questions are designed to test genuine understanding rather than memorized patterns.

---

## Question 1

Why does **Subarray Sum Equals K** require a `HashMap` instead of a `HashSet`?

### Expected Answer

A `HashSet` only stores whether a prefix sum exists.

The algorithm needs **how many times** each prefix sum has occurred because multiple identical prefix sums correspond to multiple valid subarrays.

---

## Question 2

Suppose Java's `HashMap` never resized.

What happens?

### Expected Answer

- Buckets become longer.
- More collisions occur.
- Lookup degrades toward linear time.
- Memory usage stays lower, but performance deteriorates significantly.

---

## Question 3

Why can **Sliding Window** solve **Longest Substring Without Repeating Characters** but not **Subarray Sum Equals K** with negative numbers?

### Expected Answer

Sliding Window relies on a monotonic property (expanding/shrinking changes the sum predictably). Negative numbers break that property, so Prefix Sum + HashMap is required.

---

## Question 4

Two different strings produce the same rolling hash.

Can the algorithm return them as equal?

### Expected Answer

No.

Equal hashes indicate only a candidate match. The actual substrings must be compared (or double hashing used) to eliminate collisions.

---

## Question 5

When would `HashSet` be strictly better than `HashMap`?

### Expected Answer

When only membership is required and no associated value needs to be stored. Examples include duplicate detection and set intersection.

---

# Failure Modes That Reveal Weak Understanding

A candidate likely lacks a deep understanding if they:

- Use a `HashSet` where frequencies are required.
- Forget to initialize `0 → 1` in prefix-sum problems.
- Move the sliding-window left pointer backwards.
- Ignore hash collisions in rolling-hash algorithms.
- Use only one map for isomorphic-string problems.
- Claim all `HashMap` operations are always `O(1)` without mentioning collisions or treeification.

---

# Final FAANG Revision Cheat Sheet

| Pattern | Representative Problem | Complexity |
|---------|------------------------|------------|
| Complement Lookup | Two Sum | O(n) |
| Duplicate Detection | Contains Duplicate | O(n) |
| Frequency Counting | Valid Anagram | O(n) |
| Membership Testing | Intersection of Two Arrays | O(n+m) |
| Bidirectional Mapping | Isomorphic Strings | O(n) |
| Canonical Representation | Group Anagrams | O(n·k log k) |
| Frequency + Buckets | Top K Frequent Elements | O(n) |
| Sequence Detection | Longest Consecutive Sequence | O(n) |
| Prefix Sum + Frequency Map | Subarray Sum Equals K | O(n) |
| Sliding Window | Longest Substring Without Repeating Characters | O(n) |
| Dual Frequency Maps | Minimum Window Substring | O(n) |
| Word-Based Sliding Window | Substring with Concatenation of All Words | O(n × wordLength) |
| Graph Representation | Alien Dictionary | O(V+E) |
| Temporal Graph | Find All People With Secret | O(n log n) |
| Rolling Hash | Longest Duplicate Substring | O(n log n) |

---

# 30-Second Interview Decision Tree

```text
Problem mentions...

            │
            ▼

Find Pair?
        │
        └──► Complement HashMap

Duplicates?
        │
        └──► HashSet

Count Frequency?
        │
        └──► HashMap<Key, Count>

Group Similar Items?
        │
        └──► Canonical Key + HashMap

Longest Consecutive?
        │
        └──► HashSet

Subarray Sum?
        │
        └──► Prefix Sum + HashMap

Substring?
        │
        └──► Sliding Window + HashMap

Graph Relations?
        │
        └──► HashMap<Node, List<Node>>

Repeated Substrings?
        │
        └──► Rolling Hash
```

---

# Final Takeaways

1. **Recognize the pattern before choosing the data structure.**
2. **HashSet answers "Have I seen this?"**
3. **HashMap answers "What information is associated with this key?"**
4. **Frequency maps, prefix sums, and sliding windows account for the majority of hashing interview questions.**
5. **Understand `HashMap` internals—collisions, resizing, load factor, and treeification—to answer follow-up questions confidently.**
6. **Be aware of edge cases: duplicate keys, negative numbers, hash collisions, and concurrency.**
7. **In interviews, explain *why* hashing reduces time complexity, not just *that* it does.**

---

**End of Guide**


