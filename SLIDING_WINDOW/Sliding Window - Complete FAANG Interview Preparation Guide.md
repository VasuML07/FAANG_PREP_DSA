# Sliding Window - Complete FAANG Interview Preparation Guide

> **Interview Focus:** Google • Amazon • Meta • Apple • Microsoft • Netflix • Adobe • Uber • Bloomberg

| Difficulty | Count |
|------------|------|
| Easy | 4 |
| Medium | 8 |
| Hard | 3 |

## Problems Covered

| # | Problem | Difficulty |
|---|----------|------------|
|1|Maximum Average Subarray I|Easy|
|2|Longest Substring Without Repeating Characters|Medium|
|3|Permutation in String|Medium|
|4|Minimum Size Subarray Sum|Medium|
|5|Longest Repeating Character Replacement|Medium|
|6|Max Consecutive Ones III|Medium|
|7|Fruit Into Baskets|Medium|
|8|Minimum Window Substring|Hard|
|9|Find All Anagrams in a String|Medium|
|10|Substring with Concatenation of All Words|Hard|
|11|Sliding Window Maximum|Hard|
|12|Longest Subarray of 1's After Deleting One Element|Medium|
|13|Binary Subarrays With Sum|Medium|
|14|Number of Nice Subarrays|Medium|
|15|Grumpy Bookstore Owner|Medium|

---

# 1. Maximum Average Subarray I

**LeetCode:** 643

**Difficulty:** Easy

**Companies:** Amazon, Google, Microsoft, Adobe

---

## Problem Statement

Given an integer array `nums` and an integer `k`, find the contiguous subarray of length exactly `k` having the maximum average value.

Return the maximum average.

---

## Interview Observation

Whenever you notice:

- Fixed window size
- Contiguous subarray
- Sum/average/max/min over every window

Think:

> **Fixed Sliding Window**

Instead of recalculating every window from scratch, remove one element and add one element.

---

## Approach

Suppose

```
Window Size = 4

2 5 1 8 3 9 7

Initial Window

2 5 1 8
```

Sum

```
16
```

Next window

```
5 1 8 3

New Sum

16
-2
+3

=17
```

Instead of computing

```
5+1+8+3
```

again.

Every movement costs O(1).

Overall complexity becomes O(n).

---

## Visualization

```
nums

1 12 -5 -6 50 3

k = 4

Window

[1 12 -5 -6]
 ^
 sum = 2

Move →

1 [12 -5 -6 50]
    ^
sum

2
-1
+50

=51
```

---

### Another Visualization

```
Before

L
↓

4 2 6 8 5 9
      ↑
      R

After Move

    L
    ↓

4 2 6 8 5 9
        ↑
        R

Window Sum

Old Sum
-remove nums[L]
+add nums[R]
```

---

## Java Solution

```java
class Solution {
    public double findMaxAverage(int[] nums, int k) {

        int windowSum = 0;

        for (int i = 0; i < k; i++) {
            windowSum += nums[i];
        }

        int maxSum = windowSum;

        for (int i = k; i < nums.length; i++) {

            windowSum += nums[i];
            windowSum -= nums[i - k];

            maxSum = Math.max(maxSum, windowSum);
        }

        return (double) maxSum / k;
    }
}
```

---

## Complexity

|Operation|Complexity|
|----------|----------|
|Time|O(n)|
|Space|O(1)|

---

## Edge Cases

- k = 1
- Entire array is window
- Negative numbers only
- Maximum window at end
- Maximum window at beginning

---

## LLM-Proof Follow-up Questions

### Q1

If window size changes dynamically, does this solution still work?

Why?

---

### Q2

Can this problem be solved using Prefix Sum?

Compare both approaches.

---

### Q3

How would you find the minimum average instead?

---

### Q4

Suppose k changes for every query.

Would Sliding Window still be optimal?

Explain.

---

# 2. Longest Substring Without Repeating Characters

**LeetCode:** 3

**Difficulty:** Medium

**Companies:** Google, Amazon, Meta, Bloomberg, Apple, Uber

---

## Problem Statement

Return the length of the longest substring without repeating characters.

---

## Interview Observation

Keywords

```
Substring

Longest

Distinct Characters
```

This almost always indicates

> Variable Sliding Window + HashMap/HashSet

---

## Why Sliding Window Works

Suppose

```
abcabcbb
```

Current window

```
abc
```

Next character

```
a
```

Window becomes

```
abca
```

Invalid.

Instead of restarting,

shrink left until duplicate disappears.

---

## Visualization

```
Input

a b c a b c b b
L
R

Window

abc

Length = 3

Next

abca

Duplicate

Move L

bca

Valid again
```

---

### Another Visualization

```
Valid Window

[a b c]

↓

Duplicate arrives

[a b c a]

↓

Shrink

[b c a]

↓

Expand Again
```

---

## Approach

Maintain

- left pointer
- right pointer
- HashMap of latest index

Whenever duplicate appears inside window

move left

```
left = max(left, lastIndex + 1)
```

Never move left backwards.

This guarantees O(n).

---

## Java Solution

```java
class Solution {

    public int lengthOfLongestSubstring(String s) {

        HashMap<Character, Integer> map = new HashMap<>();

        int left = 0;
        int answer = 0;

        for (int right = 0; right < s.length(); right++) {

            char ch = s.charAt(right);

            if (map.containsKey(ch)) {
                left = Math.max(left, map.get(ch) + 1);
            }

            map.put(ch, right);

            answer = Math.max(answer, right - left + 1);
        }

        return answer;
    }
}
```

---

## Complexity

|Operation|Complexity|
|----------|----------|
|Time|O(n)|
|Space|O(min(n, charset))|

---

## Edge Cases

- Empty string
- Single character
- All unique
- All duplicates
- Unicode characters

---

## LLM-Proof Follow-up Questions

### Q1

Why is

```
left = max(left, map.get(ch)+1)
```

required?

Provide a counterexample.

---

### Q2

Can this be solved using only a HashSet?

Compare complexities.

---

### Q3

What changes if we need the actual substring instead of length?

---

### Q4

Suppose uppercase and lowercase are considered identical.

How would your solution change?

---

# 3. Permutation in String

**LeetCode:** 567

**Difficulty:** Medium

**Companies:** Amazon, Meta, Google, Microsoft

---

## Problem Statement

Given two strings

```
s1
s2
```

Return true if any permutation of s1 exists as a substring inside s2.

---

## Interview Observation

Keywords

```
Permutation

Substring

Fixed Length
```

Since every permutation has identical length,

window size never changes.

Perfect Fixed Sliding Window problem.

---

## Key Insight

If

```
s1

abc
```

Any valid window must have

```
Length = 3
```

We only compare

character frequencies.

Order is irrelevant.

---

## Visualization

```
s1

abc

Frequency

a=1
b=1
c=1
```

Sliding over

```
eidbacooo

Window

eid

↓

idb

↓

dba

↓

bac

Match ✓
```

---

### Frequency Visualization

```
Target

a:1
b:1
c:1

Current Window

a:1
b:1
c:1

Equal

Found
```

---

## Approach

Maintain two frequency arrays

```
need[26]

window[26]
```

1. Build frequency of s1.

2. Build first window.

3. Compare.

4. Slide

```
remove left

add right
```

5. Compare again.

Since alphabet size is fixed (26),

comparison is O(26) = O(1).

---

## Java Solution

```java
class Solution {

    public boolean checkInclusion(String s1, String s2) {

        if (s1.length() > s2.length()) {
            return false;
        }

        int[] need = new int[26];
        int[] window = new int[26];

        for (char c : s1.toCharArray()) {
            need[c - 'a']++;
        }

        int k = s1.length();

        for (int i = 0; i < k; i++) {
            window[s2.charAt(i) - 'a']++;
        }

        if (matches(need, window)) {
            return true;
        }

        for (int i = k; i < s2.length(); i++) {

            window[s2.charAt(i) - 'a']++;

            window[s2.charAt(i - k) - 'a']--;

            if (matches(need, window)) {
                return true;
            }
        }

        return false;
    }

    private boolean matches(int[] a, int[] b) {

        for (int i = 0; i < 26; i++) {

            if (a[i] != b[i]) {
                return false;
            }
        }

        return true;
    }
}
```

---

## Complexity

|Operation|Complexity|
|----------|----------|
|Time|O(n)|
|Space|O(1)|

---

## Edge Cases

- s1 longer than s2
- Identical strings
- Repeated characters in s1
- Match at beginning
- Match at end
- No valid permutation

---

## LLM-Proof Follow-up Questions

### Q1

Why is comparing two arrays considered O(1) here but not generally?

---

### Q2

Suppose characters include Unicode.

How would your approach change?

---

### Q3

Can this problem be solved using a single frequency array instead of two?

Explain the invariant.

---

### Q4

---

# 4. Minimum Size Subarray Sum

**LeetCode:** 209

**Difficulty:** Medium

**Companies:** Amazon, Google, Microsoft, Meta, Adobe

---

## Problem Statement

Given an array of **positive integers** `nums` and a positive integer `target`, return the **minimum length** of a contiguous subarray whose sum is greater than or equal to `target`.

Return `0` if no such subarray exists.

---

## Interview Observation

Keywords:

- Contiguous subarray
- Minimum length
- Sum ≥ target
- Positive integers

The phrase **positive integers** is the deciding clue.

Since every new element only increases the sum, once the window becomes valid, shrinking it can only decrease the sum. This monotonic behavior makes a **variable sliding window** possible.

---

## Approach

Maintain two pointers:

- `left`
- `right`

Expand the window until:

```
windowSum >= target
```

Now the window is valid.

Instead of stopping, keep shrinking from the left while the condition still holds.

Every valid window is a candidate answer.

---

## Visualization

```
Target = 7

Array

2 3 1 2 4 3
L
R

Sum = 2

↓

2 3
Sum = 5

↓

2 3 1 2
Sum = 8 ✓

Length = 4

Shrink

3 1 2
Sum = 6

Stop
```

---

### Another Visualization

```
Expand →

[2 3 1 2]

Sum = 8

↓

Valid

↓

Shrink

[3 1 2]

Sum = 6

Invalid

↓

Expand Again
```

---

## Java Solution

```java
class Solution {

    public int minSubArrayLen(int target, int[] nums) {

        int left = 0;
        int sum = 0;
        int answer = Integer.MAX_VALUE;

        for (int right = 0; right < nums.length; right++) {

            sum += nums[right];

            while (sum >= target) {

                answer = Math.min(answer, right - left + 1);

                sum -= nums[left];
                left++;
            }
        }

        return answer == Integer.MAX_VALUE ? 0 : answer;
    }
}
```

---

## Complexity

| Operation | Complexity |
|-----------|------------|
| Time | O(n) |
| Space | O(1) |

---

## Why O(n)?

Although there are nested loops, each pointer only moves forward.

```
Left  → n moves
Right → n moves

Total = 2n
```

---

## Edge Cases

- Target larger than total sum
- Single element equals target
- Entire array required
- Minimum window at beginning
- Minimum window at end

---

## Common Interview Mistake

Many candidates try Prefix Sum + Binary Search.

That works in `O(n log n)`.

Sliding Window achieves **O(n)** because all numbers are positive.

If negative numbers were allowed, this solution would break.

---

## LLM-Proof Follow-up Questions

### Q1

Why does this algorithm fail when negative numbers are present?

Give a counterexample.

---

### Q2

Can you solve the negative-number version?

Which algorithm would you use?

---

### Q3

How would you find the **maximum** length subarray with sum at least `target`?

---

### Q4

Suppose the condition becomes:

```
sum > target
```

instead of

```
sum >= target
```

Does anything else change?

---

# 5. Longest Repeating Character Replacement

**LeetCode:** 424

**Difficulty:** Medium

**Companies:** Google, Meta, Amazon, Microsoft, Bloomberg

---

## Problem Statement

Given a string `s` and an integer `k`, you may replace at most `k` characters.

Return the length of the longest substring that can be transformed into a string containing only one repeating character.

---

## Interview Observation

The goal is **not** to find distinct characters.

Instead:

- Maintain a window
- Count character frequencies
- Ensure replacements needed ≤ `k`

This is one of the most important interview patterns involving **constraint tracking**.

---

## Key Insight

Suppose

```
AABABBA

Window

A A B A
```

Frequency

```
A = 3
B = 1
```

Most frequent character:

```
3
```

Window size:

```
4
```

Characters to replace

```
4 - 3 = 1
```

If replacements required ≤ k

the window is valid.

---

## Visualization

```
A A B A

Window Size = 4

Most Frequent = 3

Replace

B

↓

A A A A
```

---

### Invalid Window

```
A A B B C

Window Size = 5

Max Frequency = 2

Need

5-2=3 replacements

If k=2

Invalid

Shrink
```

---

## Java Solution

```java
class Solution {

    public int characterReplacement(String s, int k) {

        int[] freq = new int[26];

        int left = 0;
        int maxFreq = 0;
        int answer = 0;

        for (int right = 0; right < s.length(); right++) {

            maxFreq = Math.max(maxFreq,
                    ++freq[s.charAt(right) - 'A']);

            while ((right - left + 1) - maxFreq > k) {

                freq[s.charAt(left) - 'A']--;
                left++;
            }

            answer = Math.max(answer,
                    right - left + 1);
        }

        return answer;
    }
}
```

---

## Complexity

| Operation | Complexity |
|-----------|------------|
| Time | O(n) |
| Space | O(1) |

---

## Why Don't We Recompute maxFreq?

Many interviewers ask this.

Notice:

```
maxFreq

never decreases.
```

It can become "stale."

Surprisingly, the algorithm still works because a stale maximum may delay shrinking, but it never causes us to miss the optimal answer.

This is a classic interview optimization.

---

## Edge Cases

- k = 0
- All identical letters
- All different letters
- k larger than string length
- Single character string

---

## LLM-Proof Follow-up Questions

### Q1

Why is it still correct even if `maxFreq` becomes stale?

---

### Q2

How would you solve this if lowercase and uppercase were mixed?

---

### Q3

Can this algorithm work for Unicode?

---

### Q4

What if replacing characters had different costs instead of cost = 1?

---

# 6. Max Consecutive Ones III

**LeetCode:** 1004

**Difficulty:** Medium

**Companies:** Google, Amazon, Meta, Microsoft

---

## Problem Statement

Given a binary array `nums` and an integer `k`, return the maximum number of consecutive `1`s if you may flip at most `k` zeros.

---

## Interview Observation

The interviewer is effectively asking:

> Find the longest window containing at most `k` zeros.

Instead of tracking ones, track the **constraint**:

```
zeros <= k
```

---

## Approach

Expand the window.

Whenever zeros exceed `k`, shrink from the left until the window becomes valid again.

Answer = largest valid window.

---

## Visualization

```
k = 2

1 1 1 0 0 1 1

Window

[1 1 1 0 0]

Zeros = 2

Valid

↓

Add next

[1 1 1 0 0 1]

Still Valid

↓

Add next

[1 1 1 0 0 1 1]

Answer = 7
```

---

### Invalid Window

```
1 0 1 0 1 0

Window

[1 0 1 0 1 0]

Zeros = 3

k = 2

↓

Shrink

0 removed?

No

↓

Shrink Again

Now zeros = 2

Continue
```

---

## Java Solution

```java
class Solution {

    public int longestOnes(int[] nums, int k) {

        int left = 0;
        int zeros = 0;
        int answer = 0;

        for (int right = 0; right < nums.length; right++) {

            if (nums[right] == 0) {
                zeros++;
            }

            while (zeros > k) {

                if (nums[left] == 0) {
                    zeros--;
                }

                left++;
            }

            answer = Math.max(answer,
                    right - left + 1);
        }

        return answer;
    }
}
```

---

## Complexity

| Operation | Complexity |
|-----------|------------|
| Time | O(n) |
| Space | O(1) |

---

## Pattern Recognition

Notice the similarity to previous problems:

```
Longest window

subject to

Constraint
```

Constraint examples:

```
Duplicate characters <= 0

Zeros <= k

Replacements <= k

Distinct characters <= k
```

This is one of the most reusable sliding window templates in interviews.

---

## Edge Cases

- k = 0
- Entire array already ones
- Entire array zeros
- Empty array
- Single element

---

## LLM-Proof Follow-up Questions

### Q1

How would you modify the algorithm to return the actual subarray instead of its length?

---

### Q2

Suppose flipping each zero has a different cost.

How would the algorithm change?

---

### Q3

Can this be generalized to allow flipping any value, not just zero?

---

### Q4

If the array were streamed (infinite input), how would you maintain the answer online?

---


If we needed to return all matching starting indices instead of a boolean, what changes would you make?


---

# 7. Fruit Into Baskets

**LeetCode:** 904

**Difficulty:** Medium

**Companies:** Google, Amazon, Meta, Apple, Uber

---

## Problem Statement

You are given an integer array `fruits`, where each value represents a fruit type.

You have **two baskets**, and each basket can contain only **one type of fruit**.

Return the maximum number of fruits you can collect by choosing a contiguous subarray containing **at most two distinct fruit types**.

---

## Interview Observation

The problem never explicitly mentions Sliding Window.

Instead, look for these clues:

- Longest contiguous subarray
- At most K distinct elements
- Maximize window length

This is the canonical **"At Most K Distinct"** sliding window pattern.

---

## Key Insight

Maintain:

- Left pointer
- Right pointer
- Frequency map
- Number of distinct fruit types

Expand while the window contains at most two fruit types.

If it exceeds two, shrink until it becomes valid again.

---

## Visualization

```
fruits

1 2 1 2 3

Window

[1 2 1 2]

Distinct = 2 ✓

↓

Add 3

[1 2 1 2 3]

Distinct = 3 ✗

↓

Shrink

2 1 2 3

Distinct still 3

↓

Shrink

1 2 3

Distinct still 3

↓

Shrink

2 3

Distinct = 2 ✓
```

---

### Frequency Map Visualization

```
Window

2 1 2 2

Map

1 → 1

2 → 3

Distinct = 2

↓

Remove left

2 2 2

Map

2 → 3

Distinct = 1
```

---

## Java Solution

```java
class Solution {

    public int totalFruit(int[] fruits) {

        HashMap<Integer, Integer> map = new HashMap<>();

        int left = 0;
        int answer = 0;

        for (int right = 0; right < fruits.length; right++) {

            map.put(fruits[right],
                    map.getOrDefault(fruits[right], 0) + 1);

            while (map.size() > 2) {

                map.put(fruits[left],
                        map.get(fruits[left]) - 1);

                if (map.get(fruits[left]) == 0) {
                    map.remove(fruits[left]);
                }

                left++;
            }

            answer = Math.max(answer,
                    right - left + 1);
        }

        return answer;
    }
}
```

---

## Complexity

| Operation | Complexity |
|-----------|------------|
| Time | O(n) |
| Space | O(2) → O(1) *(maximum two keys after shrinking)* |

---

## Pattern Recognition

This is the generalized template for:

```
Longest Subarray

Subject To

Distinct Elements ≤ K
```

Many interview questions are direct variations of this pattern.

---

## Edge Cases

- One fruit type
- Two fruit types only
- All fruits identical
- All fruits distinct
- Empty array

---

## Common Interview Extension

Generalize to:

```
At Most K Distinct Numbers
```

Simply replace:

```
2

↓

K
```

The algorithm remains unchanged.

---

## LLM-Proof Follow-up Questions

### Q1

How would you modify the solution for **exactly** two distinct fruit types?

---

### Q2

Can this be solved without a HashMap?

Under what assumptions?

---

### Q3

Suppose baskets have capacities instead of unlimited storage.

How would the constraint change?

---

### Q4

Why is removing keys with frequency zero necessary?

---

# 8. Minimum Window Substring

**LeetCode:** 76

**Difficulty:** Hard

**Companies:** Google, Meta, Amazon, Microsoft, Apple, Bloomberg

---

## Problem Statement

Given two strings:

```
s
t
```

Return the **smallest substring** of `s` containing every character of `t`, including duplicates.

Return an empty string if no such substring exists.

---

## Interview Observation

This is arguably the most famous Sliding Window interview problem.

Unlike previous questions:

- The window does **not** have a fixed size.
- The validity depends on satisfying **all required character frequencies**.

The challenge is maintaining the window's validity while minimizing its size.

---

## Key Insight

Maintain two frequency maps:

```
Need

Current Window
```

Expand until every required character is present.

Once valid:

Shrink as much as possible while preserving validity.

The smallest valid window is the answer.

---

## Visualization

```
S

ADOBECODEBANC

T

ABC

Window

ADOBEC

Contains

A
B
C

Valid ✓

↓

Shrink

DOBEC

Missing A ✗

↓

Expand Again

...

BANC

Valid

Length = 4
```

---

### Window State

```
Need

A:1
B:1
C:1

Window

A:1
B:1
C:1

Matched = 3

Required = 3

Valid
```

---

## Java Solution

```java
class Solution {

    public String minWindow(String s, String t) {

        if (s.length() < t.length()) {
            return "";
        }

        HashMap<Character, Integer> need = new HashMap<>();

        for (char c : t.toCharArray()) {
            need.put(c, need.getOrDefault(c, 0) + 1);
        }

        HashMap<Character, Integer> window = new HashMap<>();

        int left = 0;
        int formed = 0;
        int required = need.size();

        int minLen = Integer.MAX_VALUE;
        int start = 0;

        for (int right = 0; right < s.length(); right++) {

            char c = s.charAt(right);

            window.put(c,
                    window.getOrDefault(c, 0) + 1);

            if (need.containsKey(c) &&
                    window.get(c).intValue() == need.get(c).intValue()) {

                formed++;
            }

            while (formed == required) {

                if (right - left + 1 < minLen) {

                    minLen = right - left + 1;
                    start = left;
                }

                char remove = s.charAt(left);

                window.put(remove,
                        window.get(remove) - 1);

                if (need.containsKey(remove) &&
                        window.get(remove) < need.get(remove)) {

                    formed--;
                }

                left++;
            }
        }

        return minLen == Integer.MAX_VALUE
                ? ""
                : s.substring(start, start + minLen);
    }
}
```

---

## Complexity

| Operation | Complexity |
|-----------|------------|
| Time | O(n) |
| Space | O(m) |

where `m` is the number of distinct characters.

---

## Why It Works

The window is always in one of two states:

```
Invalid

↓

Expand

↓

Valid

↓

Shrink

↓

Invalid

↓

Expand Again
```

Each pointer only moves forward.

---

## Edge Cases

- No solution
- `t` longer than `s`
- Duplicate characters in `t`
- Entire string is answer
- Multiple minimum windows

---

## Common Interview Mistake

Candidates often compare the entire frequency map after every move.

Instead:

Maintain

```
formed

required
```

This avoids repeated map comparisons.

---

## LLM-Proof Follow-up Questions

### Q1

Why do we compare **formed** with **required** instead of comparing the two maps directly?

---

### Q2

Suppose characters arrive as an infinite stream.

Can you maintain the minimum window online?

---

### Q3

How would you solve this if character comparison were case-insensitive?

---

### Q4

Can this algorithm work if deletions are allowed inside the window without moving the left pointer?

---

# 9. Find All Anagrams in a String

**LeetCode:** 438

**Difficulty:** Medium

**Companies:** Amazon, Google, Meta, Adobe

---

## Problem Statement

Given two strings:

```
s
p
```

Return all starting indices of substrings in `s` that are anagrams of `p`.

---

## Interview Observation

Unlike Problem 3 (**Permutation in String**), here we need:

- Every occurrence
- Not just whether one exists

Since every anagram has the same length as `p`, this becomes a **fixed-size sliding window** problem.

---

## Key Insight

Maintain two frequency arrays:

```
Target

Current Window
```

If they match, record the left index.

Slide one character at a time.

---

## Visualization

```
s

c b a e b a b a c d

p

a b c

Window

[c b a]

Match ✓

Index = 0

↓

[b a e]

No

↓

[a b c]

Match ✓
```

---

### Frequency Comparison

```
Target

a:1
b:1
c:1

Window

a:1
b:1
c:1

Equal

↓

Answer
```

---

## Java Solution

```java
class Solution {

    public List<Integer> findAnagrams(String s, String p) {

        List<Integer> answer = new ArrayList<>();

        if (p.length() > s.length()) {
            return answer;
        }

        int[] target = new int[26];
        int[] window = new int[26];

        for (char c : p.toCharArray()) {
            target[c - 'a']++;
        }

        int k = p.length();

        for (int i = 0; i < k; i++) {
            window[s.charAt(i) - 'a']++;
        }

        if (Arrays.equals(target, window)) {
            answer.add(0);
        }

        for (int i = k; i < s.length(); i++) {

            window[s.charAt(i) - 'a']++;

            window[s.charAt(i - k) - 'a']--;

            if (Arrays.equals(target, window)) {
                answer.add(i - k + 1);
            }
        }

        return answer;
    }
}
```

---

## Complexity

| Operation | Complexity |
|-----------|------------|
| Time | O(n) |
| Space | O(1) |

Array comparison is constant time because the alphabet size is fixed (26).

---

## Edge Cases

- `p` longer than `s`
- Every window is an anagram
- No anagrams
- Duplicate characters
- Single-character strings

---

## Relationship with Previous Problems

Notice the progression:

```
Problem 3

Return true if one permutation exists.

↓

Problem 9

Return every matching window.

↓

Same Sliding Window Pattern
Different Output
```

---

## LLM-Proof Follow-up Questions

### Q1

Can this be solved using a single frequency array instead of two?

---

### Q2

How would the solution change if the alphabet contained one million unique characters?

---

### Q3

Instead of returning indices, return the substrings themselves.

Would the complexity change?

---

### Q4

How would you optimize repeated frequency comparisons when the alphabet size is not constant?

---

---

# 10. Substring with Concatenation of All Words

**LeetCode:** 30

**Difficulty:** Hard

**Companies:** Google, Amazon, Meta, Microsoft, Apple, Bloomberg

---

## Problem Statement

You are given:

- A string `s`
- An array `words`, where every word has the same length

Return all starting indices where a substring is formed by concatenating **every word exactly once** (in any order) without extra characters.

---

## Interview Observation

This is not a character-based sliding window.

Instead, the window moves in **word-sized chunks**.

Interview clues:

- Every word has the same length
- Need exact frequency matching
- Contiguous substring
- Any permutation of words

This is a **fixed-size sliding window over tokens** rather than characters.

---

## Key Insight

Suppose

```
words

["foo","bar"]
```

Each word length

```
3
```

Window length

```
2 × 3 = 6
```

Instead of moving one character each time,

we process the string in **3 different offsets**:

```
Offset 0

barfoofoobar

bar foo foo bar

↓

Offset 1

a rf oof ...

↓

Offset 2

arf oo f...
```

Only windows aligned with word boundaries can ever be valid.

---

## Visualization

```
s

barfoofoobarthe

Words

foo
bar

Window

bar foo

✓

↓

foo foo

Extra foo

Shrink

↓

foo bar

✓
```

---

### Frequency Map

```
Need

foo → 1
bar → 1

Window

foo → 1
bar → 1

Match
```

---

## Approach

For each possible starting offset:

```
0

1

...

wordLength-1
```

Maintain:

- Left pointer
- Right pointer
- HashMap of current word counts

Whenever a word appears too many times:

```
Shrink
```

Whenever window contains all words:

```
Record index
```

---

## Java Solution

```java
class Solution {

    public List<Integer> findSubstring(String s, String[] words) {

        List<Integer> answer = new ArrayList<>();

        if (s.length() == 0 || words.length == 0)
            return answer;

        int wordLen = words[0].length();
        int totalWords = words.length;
        int totalLen = wordLen * totalWords;

        if (s.length() < totalLen)
            return answer;

        Map<String, Integer> need = new HashMap<>();

        for (String word : words)
            need.put(word, need.getOrDefault(word, 0) + 1);

        for (int offset = 0; offset < wordLen; offset++) {

            int left = offset;
            int count = 0;

            Map<String, Integer> window = new HashMap<>();

            for (int right = offset;
                 right + wordLen <= s.length();
                 right += wordLen) {

                String word = s.substring(right, right + wordLen);

                if (!need.containsKey(word)) {

                    window.clear();
                    count = 0;
                    left = right + wordLen;
                    continue;
                }

                window.put(word,
                        window.getOrDefault(word, 0) + 1);

                count++;

                while (window.get(word) > need.get(word)) {

                    String leftWord =
                            s.substring(left, left + wordLen);

                    window.put(leftWord,
                            window.get(leftWord) - 1);

                    left += wordLen;
                    count--;
                }

                if (count == totalWords) {

                    answer.add(left);

                    String leftWord =
                            s.substring(left, left + wordLen);

                    window.put(leftWord,
                            window.get(leftWord) - 1);

                    left += wordLen;
                    count--;
                }
            }
        }

        return answer;
    }
}
```

---

## Complexity

| Operation | Complexity |
|-----------|------------|
| Time | O(n × wordLength) |
| Space | O(number of unique words) |

---

## Edge Cases

- Empty string
- Duplicate words
- No valid concatenation
- One-word input
- Window found at end

---

## Interview Insight

Many candidates incorrectly move the window one character at a time.

The optimization is recognizing that every word has identical length.

---

## LLM-Proof Follow-up Questions

### Q1

Why must we iterate over every possible offset?

---

### Q2

How would the solution change if words had different lengths?

---

### Q3

Can Trie improve the complexity?

---

### Q4

How would you return the actual substrings instead of indices?

---

# 11. Sliding Window Maximum

**LeetCode:** 239

**Difficulty:** Hard

**Companies:** Google, Amazon, Microsoft, Meta, Apple, Bloomberg

---

## Problem Statement

Given an integer array `nums` and an integer `k`, return the maximum value in every contiguous subarray of size `k`.

---

## Interview Observation

A normal sliding window is **not enough**.

If the current maximum leaves the window, recomputing the maximum costs:

```
O(k)
```

Overall complexity becomes

```
O(nk)
```

The optimal solution requires a **Monotonic Deque**.

---

## Key Insight

Maintain indices inside a deque.

Invariant:

```
Deque values

Always Decreasing
```

Front always stores the maximum element of the current window.

---

## Visualization

```
nums

1 3 -1 -3 5

Deque

3 -1 -3

↓

Insert 5

Remove

-3

-1

3

Deque

5
```

---

### Window Movement

```
Window

[1 3 -1]

Maximum

3

↓

Slide

[3 -1 -3]

Maximum

3

↓

Slide

[-1 -3 5]

Maximum

5
```

---

## Approach

For each element:

Remove indices outside the window.

```
while front < left

remove front
```

Remove all smaller values.

```
while nums[last] < current

remove last
```

Insert current index.

Front always stores the answer.

---

## Java Solution

```java
class Solution {

    public int[] maxSlidingWindow(int[] nums, int k) {

        int n = nums.length;

        int[] answer = new int[n - k + 1];

        Deque<Integer> deque = new ArrayDeque<>();

        int index = 0;

        for (int i = 0; i < n; i++) {

            while (!deque.isEmpty()
                    && deque.peekFirst() <= i - k) {

                deque.pollFirst();
            }

            while (!deque.isEmpty()
                    && nums[deque.peekLast()] <= nums[i]) {

                deque.pollLast();
            }

            deque.offerLast(i);

            if (i >= k - 1) {

                answer[index++] = nums[deque.peekFirst()];
            }
        }

        return answer;
    }
}
```

---

## Complexity

| Operation | Complexity |
|-----------|------------|
| Time | O(n) |
| Space | O(k) |

---

## Why O(n)?

Every index:

```
Inserted once

Removed once
```

No index re-enters the deque.

Therefore:

```
2n operations
```

---

## Edge Cases

- k = 1
- Entire array
- Increasing array
- Decreasing array
- Duplicate maximum values

---

## Common Interview Mistake

Using a PriorityQueue.

Although correct,

removing expired elements is not constant time.

Deque provides the optimal O(n) solution.

---

## LLM-Proof Follow-up Questions

### Q1

Why does a monotonic deque guarantee O(n)?

---

### Q2

How would you compute the minimum of every window?

---

### Q3

Can a balanced BST replace the deque?

Compare complexities.

---

### Q4

Suppose the window size changes dynamically.

Can the deque technique still be used?

---

# 12. Longest Subarray of 1's After Deleting One Element

**LeetCode:** 1493

**Difficulty:** Medium

**Companies:** Amazon, Meta, Microsoft, Google

---

## Problem Statement

Given a binary array, delete exactly **one** element.

Return the maximum length of a subarray containing only `1`s after the deletion.

---

## Interview Observation

Deleting one element is equivalent to allowing

```
At Most One Zero
```

inside the window.

This is almost identical to **Max Consecutive Ones III**, except the final answer subtracts one deleted element.

---

## Key Insight

Maintain

```
zeros ≤ 1
```

Expand.

Whenever zeros exceed one,

shrink.

Window length minus one deletion gives the answer.

---

## Visualization

```
1 1 0 1 1 1

Window

[1 1 0 1 1 1]

Zeros = 1

Length = 6

Delete zero

↓

111111

Answer = 5
```

---

### Invalid Window

```
1 0 1 0 1

Zeros = 2

↓

Shrink

↓

Zeros = 1

Continue
```

---

## Java Solution

```java
class Solution {

    public int longestSubarray(int[] nums) {

        int left = 0;
        int zeros = 0;
        int answer = 0;

        for (int right = 0; right < nums.length; right++) {

            if (nums[right] == 0)
                zeros++;

            while (zeros > 1) {

                if (nums[left] == 0)
                    zeros--;

                left++;
            }

            answer = Math.max(answer,
                    right - left);
        }

        return answer;
    }
}
```

---

## Complexity

| Operation | Complexity |
|-----------|------------|
| Time | O(n) |
| Space | O(1) |

---

## Why `right - left`?

Notice:

```
Window Size

=

right-left+1
```

But one element **must** be deleted.

Therefore

```
(right-left+1)-1

↓

right-left
```

This subtle detail is a common interview trap.

---

## Edge Cases

- All ones
- All zeros
- Single element
- Zero at beginning
- Zero at end

---

## Pattern Recognition

This problem is another instance of

```
Longest Window

Subject To

Bad Elements ≤ K
```

Where

```
Bad Element

=

Zero
```

---

## LLM-Proof Follow-up Questions

### Q1

Why do we compute `right - left` instead of `right - left + 1`?

---

### Q2

How would you modify the algorithm if exactly **two** deletions were required?

---

### Q3

Suppose deleting a `1` is also allowed.

Would the algorithm change?

---

### Q4

How would you return the indices of the deleted element instead of only the length?

---

---

# 13. Binary Subarrays With Sum

**LeetCode:** 930

**Difficulty:** Medium

**Companies:** Google, Meta, Amazon, Microsoft

---

## Problem Statement

Given a binary array `nums` and an integer `goal`, return the number of non-empty subarrays whose sum equals `goal`.

---

## Interview Observation

At first glance, this looks like a Prefix Sum problem.

However, because the array is **binary (0 and 1)**, we can derive an elegant Sliding Window solution.

The key identity is:

```
Exactly(goal)

=

AtMost(goal)

-

AtMost(goal - 1)
```

Instead of counting subarrays with exactly `goal`, count:

- Subarrays with sum ≤ goal
- Subarrays with sum ≤ goal−1

Their difference gives the required answer.

---

## Why This Works

Suppose

```
goal = 2
```

```
AtMost(2)

contains

sum = 0
sum = 1
sum = 2
```

```
AtMost(1)

contains

sum = 0
sum = 1
```

Subtracting removes every subarray except those whose sum is exactly 2.

---

## Visualization

```
nums

1 0 1 0 1

goal = 2

Window

[1 0 1]

Sum = 2

↓

Expand

[1 0 1 0]

Still ≤ 2

↓

Expand

[1 0 1 0 1]

Sum = 3

Shrink
```

---

### Identity Visualization

```
All Windows

───────────────

AtMost(2)

█████████████

AtMost(1)

███████

Difference

■■■■
```

---

## Java Solution

```java
class Solution {

    public int numSubarraysWithSum(int[] nums, int goal) {
        return atMost(nums, goal) - atMost(nums, goal - 1);
    }

    private int atMost(int[] nums, int goal) {

        if (goal < 0) {
            return 0;
        }

        int left = 0;
        int sum = 0;
        int answer = 0;

        for (int right = 0; right < nums.length; right++) {

            sum += nums[right];

            while (sum > goal) {

                sum -= nums[left];
                left++;
            }

            answer += right - left + 1;
        }

        return answer;
    }
}
```

---

## Complexity

| Operation | Complexity |
|-----------|------------|
| Time | O(n) |
| Space | O(1) |

---

## Why `answer += right - left + 1`?

Suppose

```
Window

L           R

1 0 1 0
```

Every subarray ending at `R` and starting between `L` and `R` is valid.

Count:

```
R-L+1
```

No enumeration is needed.

---

## Edge Cases

- goal = 0
- Entire array zeros
- Entire array ones
- Single element
- Empty answer

---

## Common Interview Mistake

Many candidates immediately use Prefix Sum + HashMap.

While correct (`O(n)`), interviewers often expect recognition of the **AtMost trick** because the input is binary.

---

## LLM-Proof Follow-up Questions

### Q1

Why does the AtMost trick fail for arrays containing negative numbers?

---

### Q2

Why is the binary property essential?

---

### Q3

How would you solve the general integer version?

---

### Q4

Can this be extended to count subarrays whose sum lies in a range `[L, R]`?

---

# 14. Number of Nice Subarrays

**LeetCode:** 1248

**Difficulty:** Medium

**Companies:** Google, Amazon, Meta

---

## Problem Statement

Given an integer array `nums` and an integer `k`, return the number of subarrays containing exactly `k` odd numbers.

---

## Interview Observation

Ignore the actual values.

Only care whether each number is:

```
Odd

or

Even
```

Transform mentally into

```
Odd → 1

Even → 0
```

The problem becomes nearly identical to **Binary Subarrays With Sum**.

---

## Key Insight

Again,

```
Exactly(k)

=

AtMost(k)

-

AtMost(k-1)
```

Instead of summing values, count odd numbers.

---

## Visualization

```
nums

2 1 2 1 2

Odd Count

0 1 0 1 0

Goal = 2

Window

[2 1 2 1]

Odd = 2

Valid
```

---

### Sliding Window

```
Expand

↓

Odd Count

0

1

2

↓

Too many?

Shrink

↓

Continue
```

---

## Java Solution

```java
class Solution {

    public int numberOfSubarrays(int[] nums, int k) {
        return atMost(nums, k) - atMost(nums, k - 1);
    }

    private int atMost(int[] nums, int k) {

        if (k < 0) {
            return 0;
        }

        int left = 0;
        int odd = 0;
        int answer = 0;

        for (int right = 0; right < nums.length; right++) {

            if ((nums[right] & 1) == 1) {
                odd++;
            }

            while (odd > k) {

                if ((nums[left] & 1) == 1) {
                    odd--;
                }

                left++;
            }

            answer += right - left + 1;
        }

        return answer;
    }
}
```

---

## Complexity

| Operation | Complexity |
|-----------|------------|
| Time | O(n) |
| Space | O(1) |

---

## Pattern Recognition

Observe the reusable template:

```
Exactly K

↓

AtMost(K)

-

AtMost(K-1)
```

Applicable whenever the constraint is monotonic.

---

## Edge Cases

- k = 0
- All even numbers
- All odd numbers
- Single element
- Large arrays

---

## LLM-Proof Follow-up Questions

### Q1

Can this problem be solved using Prefix Sum?

Compare both methods.

---

### Q2

Suppose we count prime numbers instead of odd numbers.

Does the same algorithm work?

---

### Q3

Why does `answer += right-left+1` remain valid?

---

### Q4

Can you solve this online if numbers arrive continuously?

---

# 15. Grumpy Bookstore Owner

**LeetCode:** 1052

**Difficulty:** Medium

**Companies:** Amazon, Google, Microsoft

---

## Problem Statement

A bookstore owner is sometimes grumpy.

You are given:

- `customers[i]`
- `grumpy[i]`

The owner can suppress grumpiness for exactly `minutes` consecutive minutes.

Return the maximum number of satisfied customers.

---

## Interview Observation

This problem combines:

- Fixed Sliding Window
- Baseline computation

Instead of maximizing total customers directly,

compute:

```
Already Satisfied

+

Extra Customers
```

Only the "extra" part depends on the chosen window.

---

## Key Insight

Customers already satisfied:

```
grumpy = 0
```

Customers that can additionally become satisfied:

```
grumpy = 1
```

Find the window with the maximum additional customers.

---

## Visualization

```
customers

1 2 3 4 5

grumpy

0 1 1 0 1

Already Happy

1 + 4 = 5

Window Size = 2

Extra

2+3 = 5

Answer

5 + 5 = 10
```

---

### Window Movement

```
Extra Gain

2+3

↓

3+0

↓

0+5

Maximum Gain
```

---

## Java Solution

```java
class Solution {

    public int maxSatisfied(int[] customers,
                            int[] grumpy,
                            int minutes) {

        int satisfied = 0;

        for (int i = 0; i < customers.length; i++) {

            if (grumpy[i] == 0) {
                satisfied += customers[i];
            }
        }

        int extra = 0;

        for (int i = 0; i < minutes; i++) {

            if (grumpy[i] == 1) {
                extra += customers[i];
            }
        }

        int maxExtra = extra;

        for (int i = minutes; i < customers.length; i++) {

            if (grumpy[i] == 1) {
                extra += customers[i];
            }

            if (grumpy[i - minutes] == 1) {
                extra -= customers[i - minutes];
            }

            maxExtra = Math.max(maxExtra, extra);
        }

        return satisfied + maxExtra;
    }
}
```

---

## Complexity

| Operation | Complexity |
|-----------|------------|
| Time | O(n) |
| Space | O(1) |

---

## Edge Cases

- minutes = 1
- minutes = array length
- Owner never grumpy
- Owner always grumpy
- Single customer

---

## Interview Insight

A common mistake is recalculating the total satisfied customers for every window.

Instead:

```
Baseline

+

Maximum Increment
```

This separates the problem into two independent computations.

---

## LLM-Proof Follow-up Questions

### Q1

How would you solve the problem if the owner could suppress grumpiness twice?

---

### Q2

Suppose each suppression interval had a different length.

How would the algorithm change?

---

### Q3

Can Prefix Sum solve this problem? Compare it with Sliding Window.

---

### Q4

How would you return the optimal interval in addition to the maximum satisfied customers?

---

# Interview Revision Cheat Sheet

## Pattern Recognition

| Interview Clue | Typical Pattern |
|----------------|-----------------|
| Fixed window size | Fixed Sliding Window |
| Longest substring/subarray | Variable Sliding Window |
| At most K distinct | HashMap + Variable Window |
| Frequency matching | HashMap / Frequency Array |
| Maximum in every window | Monotonic Deque |
| Exactly K | `AtMost(K) - AtMost(K-1)` |
| Positive numbers + target sum | Expand/Shrink Window |
| Character replacement budget | Constraint Tracking |
| Token-based window | Word-Length Sliding Window |

---

# Complete Problem List

| # | Problem | Core Pattern |
|---|---------|--------------|
|1|Maximum Average Subarray I|Fixed Window|
|2|Longest Substring Without Repeating Characters|HashMap + Variable Window|
|3|Permutation in String|Frequency Array|
|4|Minimum Size Subarray Sum|Positive Numbers Window|
|5|Longest Repeating Character Replacement|Constraint Tracking|
|6|Max Consecutive Ones III|At Most K Bad Elements|
|7|Fruit Into Baskets|At Most K Distinct|
|8|Minimum Window Substring|Need vs Window Frequencies|
|9|Find All Anagrams in a String|Fixed Frequency Window|
|10|Substring with Concatenation of All Words|Token Sliding Window|
|11|Sliding Window Maximum|Monotonic Deque|
|12|Longest Subarray of 1's After Deleting One Element|At Most One Zero|
|13|Binary Subarrays With Sum|Exactly K = AtMost(K) − AtMost(K−1)|
|14|Number of Nice Subarrays|Exactly K Odds|
|15|Grumpy Bookstore Owner|Fixed Window Gain Maximization|

---

# Final Interview Checklist

Before implementing a solution, ask:

- Is the problem asking for a **contiguous** subarray or substring?
- Is the window size **fixed** or **dynamic**?
- Can I maintain the answer by **adding one element and removing one element**?
- Is the constraint **monotonic** (e.g., positives only, at most K)?
- Should I track:
  - Frequency?
  - Distinct count?
  - Running sum?
  - Maximum frequency?
  - Number of invalid elements?
- Does the problem require:
  - A **HashMap**
  - A **frequency array**
  - A **Deque**
  - The **AtMost(K) − AtMost(K−1)** trick?

If you can answer these questions during an interview, you'll usually identify the correct sliding window template within a minute.





