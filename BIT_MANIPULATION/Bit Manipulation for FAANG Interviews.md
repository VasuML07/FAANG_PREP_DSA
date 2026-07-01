# Bit Manipulation for FAANG Interviews
> Complete LeetCode Preparation Guide (Java)

Bit manipulation appears far more often than candidates expect. Strong interviewers use these problems to test whether you truly understand binary computation instead of memorizing patterns. Master these questions and many "hard-looking" problems become surprisingly small.

---

# Question 1 — Single Number (LeetCode 136)

**Difficulty:** Easy

**Asked By:** Google, Amazon, Microsoft, Meta, Apple

---

## Problem

Given a non-empty integer array, every element appears exactly twice except for one element.

Return that single element.

**Example**

```text
Input:
[4,1,2,1,2]

Output:
4
```

---

# Why This Problem Matters

This is usually the very first bit manipulation interview problem.

It teaches one of the most important interview properties:

> XOR cancels duplicates.

Understanding this property unlocks dozens of harder problems.

---

# Observation

Suppose we have

```text
4 1 2 1 2
```

Binary

```text
4 = 100
1 = 001
2 = 010
```

Now XOR every number.

```text
100
001
010
001
010
```

Notice

```text
001 XOR 001 = 000
010 XOR 010 = 000
```

Everything disappears except

```text
100
```

which equals **4**.

---

# XOR Properties

```text
a ^ a = 0

a ^ 0 = a

Associative

Commutative
```

Therefore

```text
a ^ b ^ a

=

(a ^ a) ^ b

=

0 ^ b

=

b
```

---

# Visualization

```text
Array

4 1 2 1 2

↓

4
↓

4 ^ 1

↓

4 ^ 1 ^ 2

↓

4 ^ 1 ^ 2 ^ 1

↓

4 ^ (1 ^ 1) ^ 2

↓

4 ^ 0 ^ 2

↓

4 ^ 2

↓

4 ^ 2 ^ 2

↓

4
```

---

# Algorithm

Initialize answer = 0

For every element

```text
answer ^= number
```

Return answer.

---

# Java Solution

```java
class Solution {

    public int singleNumber(int[] nums) {

        int xor = 0;

        // Duplicate numbers cancel out.
        for (int num : nums) {
            xor ^= num;
        }

        return xor;
    }
}
```

---

# Complexity

| Operation | Complexity |
|-----------|------------|
| Time | O(n) |
| Space | O(1) |

---

# Why It Works

Each duplicate contributes

```text
x ^ x = 0
```

Only the unique element survives.

---

# Interview Follow-up

If numbers appear **three times** instead of twice?

That becomes **Single Number II**, where XOR alone no longer works.

---

---

# Question 2 — Number of 1 Bits (LeetCode 191)

**Difficulty:** Easy

**Asked By:** Microsoft, Google, Apple, Amazon

---

## Problem

Return the number of set bits (1s) in an unsigned integer.

Example

```text
Input

11

Binary

1011

Output

3
```

---

# Observation

Instead of checking decimal digits, inspect binary bits.

Example

```text
13

Binary

1101
```

There are

```text
3
```

set bits.

---

# Brute Force

Repeatedly inspect the last bit.

```text
n & 1
```

If it is 1

increment answer.

Then

```text
n >>= 1
```

Repeat.

---

# Example

```text
1101

Last bit = 1

↓

110

Last bit = 0

↓

11

Last bit = 1

↓

1

Last bit = 1
```

Total

```text
3
```

---

# Better Trick — Brian Kernighan Algorithm

This is one of the most important interview tricks.

Observe

```text
n = 1011000
```

Subtract one

```text
1010111
```

AND them

```text
1011000

1010111

--------

1010000
```

The lowest set bit disappears.

Therefore

```text
n &= (n-1)
```

removes exactly **one set bit**.

Repeat until

```text
0
```

---

# Visualization

```text
11010000

↓

11000000

↓

10000000

↓

00000000
```

Three iterations

↓

Three set bits

---

# Java Solution

```java
class Solution {

    public int hammingWeight(int n) {

        int count = 0;

        while (n != 0) {

            n &= (n - 1);

            count++;
        }

        return count;
    }
}
```

---

# Complexity

| Operation | Complexity |
|-----------|------------|
| Time | O(number of set bits) |
| Space | O(1) |

Worst case

```text
32 iterations
```

for Java integers.

---

# Why It Works

Every iteration removes one set bit.

Therefore

Number of iterations

=

Number of ones.

---

# Interview Insight

Whenever someone asks

> Count set bits

Brian Kernighan should immediately come to mind.

---

---

# Question 3 — Counting Bits (LeetCode 338)

**Difficulty:** Easy

**Asked By:** Google, Meta, Microsoft

---

## Problem

Given an integer

```text
n
```

return an array

```text
ans
```

where

```text
ans[i]
```

contains the number of set bits in

```text
i
```

---

Example

```text
n = 5
```

Output

```text
0 1 1 2 1 2
```

---

# Observation

Using Question 2 for every number works.

Complexity

```text
O(n log n)
```

We can do better.

---

# Pattern

Take

```text
6

Binary

110
```

Remove last bit

```text
11

Binary

3
```

Relationship

```text
110

↓

11
```

The count differs only by the last bit.

Therefore

```text
count(i)

=

count(i >> 1)

+

(i & 1)
```

---

# Visualization

| Number | Binary | Formula | Answer |
|---------|---------|----------|---------|
|0|000|0|0|
|1|001|0+1|1|
|2|010|1+0|1|
|3|011|1+1|2|
|4|100|1+0|1|
|5|101|1+1|2|

---

# DP Relation

```text
dp[i]

=

dp[i >> 1]

+

(i & 1)
```

Every answer depends on a smaller number.

---

# Java Solution

```java
class Solution {

    public int[] countBits(int n) {

        int[] dp = new int[n + 1];

        for (int i = 1; i <= n; i++) {

            dp[i] = dp[i >> 1] + (i & 1);

        }

        return dp;
    }
}
```

---

# Complexity

| Operation | Complexity |
|-----------|------------|
| Time | O(n) |
| Space | O(n) |

---

# Why It Works

Right shifting removes the last bit.

The removed bit contributes either

```text
0
```

or

```text
1
```

Nothing else changes.

---

# Interview Extension

Alternative recurrence

```text
dp[i]

=

dp[i & (i-1)]

+

1
```

using Brian Kernighan.

Interviewers love both versions.

---

---

# Question 4 — Reverse Bits (LeetCode 190)

**Difficulty:** Easy

**Asked By:** Apple, Google, Amazon, Microsoft

---

## Problem

Reverse the bits of a 32-bit unsigned integer.

Example

```text
Input

00000010100101000001111010011100

Output

00111001011110000010100101000000
```

---

# Why This Problem Matters

This tests whether you understand

- shifts
- masking
- bit extraction
- bit construction

rather than simply counting bits.

---

# Observation

Read bits from right to left.

Build answer from left to right.

---

# Visualization

Suppose

```text
101100
```

Extract last bit.

```text
0
```

Append to answer.

```text
answer <<= 1

answer |= lastBit
```

Repeat.

---

Example

```text
Original

101100

↓

Take 0

Answer

0

↓

Take 0

Answer

00

↓

Take 1

Answer

001

↓

Take 1

Answer

0011

↓

Take 0

Answer

00110

↓

Take 1

Answer

001101
```

---

# Algorithm

Repeat exactly

```text
32
```

times.

Each iteration

Extract

```text
n & 1
```

Shift answer

```text
answer <<= 1
```

Insert bit

```text
answer |= bit
```

Shift input

```text
n >>>= 1
```

Notice the unsigned shift operator.

---

# Java Solution

```java
class Solution {

    public int reverseBits(int n) {

        int answer = 0;

        for (int i = 0; i < 32; i++) {

            // Make room for next bit
            answer <<= 1;

            // Copy last bit
            answer |= (n & 1);

            // Unsigned shift
            n >>>= 1;
        }

        return answer;
    }
}
```

---

# Complexity

| Operation | Complexity |
|-----------|------------|
| Time | O(32) = O(1) |
| Space | O(1) |

---

# Why It Works

Every iteration copies one bit from the input's least significant position to the answer's most significant remaining position.

After exactly 32 iterations, every bit has been reversed.

---

# Common Interview Mistakes

### Using signed shift

Wrong

```java
n >>= 1;
```

Correct

```java
n >>>= 1;
```

---

### Forgetting to shift answer first

Wrong order produces incorrect bit positions.

Always

```text
Shift

↓

Insert
```

---

### Stopping early

Reverse **all 32 bits**, even if the remaining bits are zero.

---

# Key Takeaways From Questions 1–4

| Problem | Core Trick |
|----------|------------|
|136. Single Number|XOR cancels duplicates|
|191. Number of 1 Bits|`n &= (n-1)` removes one set bit|
|338. Counting Bits|DP using right shift|
|190. Reverse Bits|Mask + Shift + Construct Answer|

---

**Next Part (Questions 5–8):**

5. Power of Two (231)  
6. Missing Number (268)  
7. Sum of Two Integers (371) **[LLM-Proof]**  
8. Single Number II (137) **[LLM-Proof]**

---

# Question 5 — Power of Two (LeetCode 231)

**Difficulty:** Easy

**Asked By:** Google, Amazon, Microsoft, Apple

---

## Problem

Given an integer `n`, return `true` if it is a power of two. Otherwise, return `false`.

**Examples**

```text
Input: 16
Output: true

Binary:
10000
```

```text
Input: 18
Output: false

Binary:
10010
```

---

# Observation

Every power of two has **exactly one set bit**.

Examples:

```text
1   = 0001

2   = 0010

4   = 0100

8   = 1000

16  = 10000
```

Non-powers contain multiple set bits.

```text
10

1010
```

Two set bits.

---

# Bit Trick

We already learned

```text
n & (n - 1)
```

removes the lowest set bit.

Suppose

```text
16

10000
```

```text
15

01111
```

AND

```text
10000

01111

------

00000
```

Exactly zero.

Now consider

```text
18

10010
```

```text
17

10001
```

```text
10010

10001

------

10000
```

Not zero.

---

# Visualization

```text
Power of Two

100000

↓

011111

↓

000000
```

---

```text
Not Power of Two

101000

↓

100111

↓

100000
```

---

# Edge Case

Negative numbers and zero are **not** powers of two.

Therefore

```java
n > 0
```

must be checked first.

---

# Java Solution

```java
class Solution {

    public boolean isPowerOfTwo(int n) {

        if (n <= 0)
            return false;

        return (n & (n - 1)) == 0;
    }
}
```

---

# Complexity

| Operation | Complexity |
|------------|------------|
| Time | O(1) |
| Space | O(1) |

---

# Why It Works

A power of two has one set bit.

Removing that bit leaves zero.

Any other positive number still contains another set bit.

---

# Interview Follow-up

Power of Four?

Need

```text
Power of Two

AND

Bit located at even position
```

---

---

# Question 6 — Missing Number (LeetCode 268)

**Difficulty:** Easy

**Asked By:** Google, Meta, Amazon, Microsoft

---

## Problem

Given an array containing numbers

```text
0...n
```

with exactly one number missing,

return the missing number.

Example

```text
Input

3 0 1

Output

2
```

---

# Observation

Addition works.

XOR also works.

Why?

Duplicates cancel.

---

Suppose

```text
Numbers

0 1 2 3

Array

3 0 1
```

XOR both groups.

```text
0 ^ 1 ^ 2 ^ 3

^

3 ^ 0 ^ 1
```

Everything cancels except

```text
2
```

---

# Visualization

```text
Expected

0

1

2

3

↓

Actual

3

0

1

↓

Cancel

↓

2
```

---

# Algorithm

Initialize

```text
xor = n
```

Then

```text
xor ^= index

xor ^= value
```

for every position.

---

# Java Solution

```java
class Solution {

    public int missingNumber(int[] nums) {

        int xor = nums.length;

        for (int i = 0; i < nums.length; i++) {

            xor ^= i;
            xor ^= nums[i];

        }

        return xor;
    }
}
```

---

# Complexity

| Operation | Complexity |
|------------|------------|
| Time | O(n) |
| Space | O(1) |

---

# Why It Works

Every value appears twice except the missing one.

XOR removes all duplicates.

---

# Alternative

Mathematical formula

```text
n(n+1)/2
```

works too.

Interviewers often prefer XOR because it avoids overflow.

---

---

# Question 7 — Sum of Two Integers (LeetCode 371)

**Difficulty:** Medium

**[LLM-Proof]**

**Asked By:** Google, Apple, Microsoft

---

## Problem

Calculate

```text
a + b
```

without using

```text
+

or

-
```

operators.

---

# Why This Is LLM-Proof

This question cannot be solved by memorizing syntax.

You must understand **how CPUs perform binary addition**.

Interviewers often ask follow-up questions about carries, logic gates, and ALUs.

---

# Binary Addition

Decimal

```text
5 + 3
```

Binary

```text
101

011
```

How does hardware add them?

Two separate operations happen.

---

### Step 1 — Add without carry

XOR behaves exactly like addition **without carry**.

Example

```text
1 XOR 1 = 0
```

because

```text
1 + 1
```

creates a carry.

---

### Step 2 — Compute carry

Carry occurs only when

both bits are one.

That is exactly

```text
AND
```

---

Example

```text
101

011
```

XOR

```text
110
```

Carry

```text
001
```

Shift carry

```text
010
```

Repeat.

---

# Visualization

```text
a = 101

b = 011

----------------

Sum

110

Carry

001

↓

Shift Carry

010

----------------

110

010

↓

100

↓

Carry

010

↓

Shift

100

↓

1000

Final

1000

= 8
```

---

# Algorithm

Repeat until carry becomes zero.

```
carry = (a & b)

sum = a ^ b

carry <<= 1

a = sum

b = carry
```

---

# Java Solution

```java
class Solution {

    public int getSum(int a, int b) {

        while (b != 0) {

            int carry = (a & b);

            a = a ^ b;

            b = carry << 1;

        }

        return a;
    }
}
```

---

# Complexity

| Operation | Complexity |
|------------|------------|
| Time | O(32) |
| Space | O(1) |

---

# Why It Works

Each iteration resolves one layer of carries.

Eventually no carries remain.

At that point

```text
XOR
```

already represents the complete sum.

---

# Interview Discussion

This question directly mirrors digital hardware.

Logic gates inside a processor perform exactly these operations.

---

# Common Mistake

People think XOR performs addition.

It does **addition without carry**.

Carry must be handled separately.

---

---

# Question 8 — Single Number II (LeetCode 137)

**Difficulty:** Medium

**[LLM-Proof]**

**Asked By:** Google, Meta, Apple

---

## Problem

Every number appears three times except one.

Return the unique number.

Example

```text
Input

2 2 3 2

Output

3
```

---

# Why XOR Fails

Question 1 relied on

```text
x ^ x = 0
```

Now

```text
x ^ x ^ x = x
```

Duplicates no longer disappear.

Need another idea.

---

# Observation

Think **bit by bit**.

Example

```text
Numbers

2

2

2

3
```

Binary

```text
010

010

010

011
```

Count every column.

| Bit Position | Count |
|--------------|-------|
|0|1|
|1|4|
|2|0|

Now take

```text
count % 3
```

Remaining bits belong to the unique number.

---

# Visualization

```text
010

010

010

011

-----------

Bit0

1

↓

1 % 3 = 1

Bit1

4

↓

4 % 3 = 1

Bit2

0

↓

0
```

Result

```text
011

=

3
```

---

# Algorithm

For every bit

```text
0...

31
```

Count how many numbers have that bit set.

If

```text
count % 3 != 0
```

set that bit in answer.

---

# Java Solution

```java
class Solution {

    public int singleNumber(int[] nums) {

        int answer = 0;

        for (int bit = 0; bit < 32; bit++) {

            int count = 0;

            for (int num : nums) {

                if (((num >> bit) & 1) == 1)
                    count++;

            }

            if (count % 3 != 0)
                answer |= (1 << bit);

        }

        return answer;
    }
}
```

---

# Complexity

| Operation | Complexity |
|------------|------------|
| Time | O(32 × n) = O(n) |
| Space | O(1) |

---

# Why It Works

Bits contributed by numbers appearing three times always sum to multiples of three.

Modulo operation removes them completely.

Only the unique number contributes leftover bits.

---

# Advanced Interview Follow-up

Many interviewers ask:

> Can you solve this without iterating over all 32 bits?

The expected answer uses a **finite-state machine** with two bitmasks (`ones` and `twos`), achieving the same complexity but with elegant state transitions. Explaining that solution demonstrates deep understanding of bitwise state encoding and is considered an advanced bit-manipulation technique.

---

# Key Takeaways From Questions 5–8

| Problem | Primary Technique |
|----------|-------------------|
|231. Power of Two|`n & (n-1)` leaves zero only for powers of two|
|268. Missing Number|XOR cancellation across indices and values|
|371. Sum of Two Integers|XOR for sum, AND + shift for carry|
|137. Single Number II|Count bits independently and apply modulo arithmetic|

---

**Next Part (Questions 9–12):**

9. Bitwise AND of Numbers Range (201) **[LLM-Proof]**  
10. Maximum XOR of Two Numbers in an Array (421) **[LLM-Proof]**  
11. UTF-8 Validation (393)  
12. Minimum Flips to Make `a OR b == c` (1318)

---

# Question 9 — Bitwise AND of Numbers Range (LeetCode 201)

**Difficulty:** Medium

**[LLM-Proof]**

**Asked By:** Google, Meta, Microsoft

---

## Problem

Given two integers `left` and `right`, return the bitwise AND of every number in the inclusive range `[left, right]`.

### Example

```text
Input:
left = 5
right = 7

Output:
4
```

Explanation

```text
5 = 101
6 = 110
7 = 111

101
110
111
---
100 = 4
```

---

# Why This Is LLM-Proof

Many candidates try to iterate through every number.

That works only for small ranges.

The real solution requires understanding how binary prefixes change.

---

# Key Observation

Whenever a bit changes from

```text
0

to

1
```

inside the range,

the AND for that position becomes

```text
0
```

Only the **common binary prefix** survives.

---

Example

```text
26

11010

30

11110
```

Common prefix

```text
11
```

Remaining bits vary.

Therefore answer

```text
11000
```

---

# Visualization

```text
11010

11011

11100

11101

11110

Common Prefix

11

Remaining Bits

xxxxx

↓

Answer

11000
```

---

# Trick

Keep shifting until both numbers become equal.

```text
left >>= 1

right >>= 1
```

Count shifts.

When equal,

shift back.

---

Example

```text
5

101

7

111

↓

10

11

↓

1

1

Common prefix found.

Shift back twice.

100
```

---

# Java Solution

```java
class Solution {

    public int rangeBitwiseAnd(int left, int right) {

        int shifts = 0;

        while (left != right) {

            left >>= 1;
            right >>= 1;
            shifts++;

        }

        return left << shifts;
    }
}
```

---

# Complexity

| Operation | Complexity |
|------------|------------|
| Time | O(32) |
| Space | O(1) |

---

# Why It Works

Every differing suffix must eventually become zero after repeated right shifts.

Only the identical prefix remains.

---

# Interview Follow-up

Alternative solution:

Repeatedly remove the lowest set bit of `right`.

```text
right &= right - 1
```

until

```text
right <= left
```

Both methods are accepted.

---

---

# Question 10 — Maximum XOR of Two Numbers in an Array (LeetCode 421)

**Difficulty:** Medium

**[LLM-Proof]**

**Asked By:** Google, Meta, Amazon

---

## Problem

Given an integer array,

return the maximum XOR obtainable from any two numbers.

Example

```text
Input

3 10 5 25 2 8

Output

28
```

Because

```text
5 XOR 25 = 28
```

---

# Why This Is LLM-Proof

This question tests whether you understand binary tries, greedy bit construction, and XOR optimization.

Simply checking every pair is not acceptable.

---

# Brute Force

Compare every pair.

```text
O(n²)
```

Too slow.

---

# Observation

Higher bits contribute more.

Example

```text
100000

>

011111
```

Therefore maximize the highest differing bit first.

---

# Binary Trie

Store every number bit-by-bit.

Example

```text
5

00101
```

Trie

```text
          root

        /      \

       0        1

      /

     0

    /

   1

  /

 0

/

1
```

Each level represents one bit.

---

# Greedy Search

Suppose current bit is

```text
1
```

To maximize XOR,

prefer

```text
0
```

If opposite exists,

take it.

Otherwise,

take same bit.

Repeat for every level.

---

# Visualization

```text
Current Number

10110

Trie

Prefer

0

when current bit

1

Prefer

1

when current bit

0
```

Each successful opposite increases XOR.

---

# Java Solution

```java
class Solution {

    static class TrieNode {

        TrieNode[] child = new TrieNode[2];

    }

    public int findMaximumXOR(int[] nums) {

        TrieNode root = new TrieNode();

        // Build trie
        for (int num : nums) {

            TrieNode node = root;

            for (int i = 31; i >= 0; i--) {

                int bit = (num >> i) & 1;

                if (node.child[bit] == null)
                    node.child[bit] = new TrieNode();

                node = node.child[bit];

            }
        }

        int max = 0;

        for (int num : nums) {

            TrieNode node = root;

            int xor = 0;

            for (int i = 31; i >= 0; i--) {

                int bit = (num >> i) & 1;

                int opposite = bit ^ 1;

                if (node.child[opposite] != null) {

                    xor |= (1 << i);
                    node = node.child[opposite];

                } else {

                    node = node.child[bit];

                }
            }

            max = Math.max(max, xor);

        }

        return max;
    }
}
```

---

# Complexity

| Operation | Complexity |
|------------|------------|
| Time | O(32 × n) |
| Space | O(32 × n) |

---

# Why It Works

Choosing the opposite bit whenever possible maximizes the most significant differing bit first.

Since higher bits dominate the value of an integer, this greedy strategy is optimal.

---

# Interview Discussion

Expected follow-up:

> Why does greedy work?

Answer:

Binary numbers are lexicographically ordered by higher bits.

Improving a higher bit always outweighs any improvements in lower bits.

---

---

# Question 11 — UTF-8 Validation (LeetCode 393)

**Difficulty:** Medium

**Asked By:** Apple, Microsoft

---

## Problem

Given an array representing bytes,

determine whether it forms valid UTF-8 encoding.

---

Example

```text
Input

197 130 1

Output

true
```

---

# Observation

UTF-8 has strict leading-bit patterns.

| Bytes | Prefix |
|--------|--------|
|1|0xxxxxxx|
|2|110xxxxx|
|3|1110xxxx|
|4|11110xxx|
|Continuation|10xxxxxx|

Everything depends on reading leading bits.

---

# Visualization

```text
110xxxxx

↓

Need exactly

1

continuation

↓

10xxxxxx
```

---

```text
1110xxxx

↓

Need

2

continuations

↓

10xxxxxx

10xxxxxx
```

---

# Algorithm

Maintain

```text
remainingBytes
```

If zero,

identify a new character.

Otherwise,

expect

```text
10xxxxxx
```

---

# Java Solution

```java
class Solution {

    public boolean validUtf8(int[] data) {

        int remaining = 0;

        for (int num : data) {

            if (remaining == 0) {

                if ((num >> 7) == 0) {

                    continue;

                } else if ((num >> 5) == 0b110) {

                    remaining = 1;

                } else if ((num >> 4) == 0b1110) {

                    remaining = 2;

                } else if ((num >> 3) == 0b11110) {

                    remaining = 3;

                } else {

                    return false;

                }

            } else {

                if ((num >> 6) != 0b10)
                    return false;

                remaining--;

            }
        }

        return remaining == 0;
    }
}
```

---

# Complexity

| Operation | Complexity |
|------------|------------|
| Time | O(n) |
| Space | O(1) |

---

# Why It Works

UTF-8 is defined entirely by bit prefixes.

Bit shifting isolates those prefixes in constant time.

---

# Interview Insight

This question frequently appears in systems interviews because UTF-8 parsing is common in compilers, databases, and network protocols.

---

---

# Question 12 — Minimum Flips to Make `a OR b == c` (LeetCode 1318)

**Difficulty:** Medium

**Asked By:** Google, Amazon

---

## Problem

Given integers `a`, `b`, and `c`,

return the minimum number of bit flips required so that

```text
(a OR b) == c
```

---

Example

```text
a = 2

b = 6

c = 5
```

Output

```text
3
```

---

# Observation

Each bit position is independent.

Process one bit at a time.

---

Suppose

```text
a

1

b

1

c

0
```

Need

```text
2
```

flips.

---

Suppose

```text
a

0

b

1

c

1
```

Already correct.

Need

```text
0
```

flips.

---

# Decision Table

| a | b | c | Flips |
|---|---|---|-------|
|0|0|0|0|
|0|0|1|1|
|0|1|0|1|
|1|0|0|1|
|1|1|0|2|
|0|1|1|0|
|1|0|1|0|
|1|1|1|0|

---

# Visualization

```text
Current

a = 1

b = 1

↓

OR

1

Need

0

↓

Flip both bits

↓

Total = 2
```

---

# Algorithm

For all 32 bits:

Extract

```text
bitA

bitB

bitC
```

Evaluate according to the decision table.

---

# Java Solution

```java
class Solution {

    public int minFlips(int a, int b, int c) {

        int flips = 0;

        for (int i = 0; i < 32; i++) {

            int bitA = (a >> i) & 1;
            int bitB = (b >> i) & 1;
            int bitC = (c >> i) & 1;

            if (bitC == 0) {

                flips += bitA + bitB;

            } else {

                if ((bitA | bitB) == 0)
                    flips++;

            }
        }

        return flips;
    }
}
```

---

# Complexity

| Operation | Complexity |
|------------|------------|
| Time | O(32) = O(1) |
| Space | O(1) |

---

# Why It Works

Since OR operates independently on each bit,

the minimum flips can also be computed independently for every bit position.

Adding those local optimums gives the global optimum.

---

# Key Takeaways From Questions 9–12

| Problem | Core Technique |
|----------|----------------|
|201. Bitwise AND of Range|Common binary prefix via repeated right shifts|
|421. Maximum XOR|Binary Trie + Greedy bit selection|
|393. UTF-8 Validation|Bit masks and prefix matching|
|1318. Minimum Flips|Per-bit state analysis and case evaluation|

---

**Next Part (Questions 13–15 + Final Summary):**

13. Total Hamming Distance (477)  
14. Maximum Product of Word Lengths (318) **[LLM-Proof]**  
15. Maximum XOR With an Element From Array (1707) **[Hard][LLM-Proof]**  
**Patterns and Mental Models**


---

# Question 13 — Total Hamming Distance (LeetCode 477)

**Difficulty:** Medium

**Asked By:** Google, Meta, Amazon

---

## Problem

The **Hamming Distance** between two integers is the number of bit positions where they differ.

Given an integer array, return the **sum of Hamming distances for every pair**.

### Example

```text
Input

[4, 14, 2]

Output

6
```

---

# Observation

Brute force compares every pair.

```text
O(n² × 32)
```

This becomes too slow for large inputs.

Instead of comparing **numbers**, compare **bit positions**.

---

## Key Insight

For one bit position:

- Every **0** paired with a **1** contributes exactly **1** to the answer.
- Two zeros contribute **0**.
- Two ones contribute **0**.

Therefore,

```text
Contribution

=

countOnes × countZeros
```

---

## Visualization

Bit position

```text
Numbers

4  = 0100

14 = 1110

2  = 0010
```

Consider bit 1:

```text
0

1

1
```

```text
Zeros = 1

Ones = 2
```

Contribution

```text
1 × 2 = 2
```

Repeat for every bit.

---

# Algorithm

For every bit from

```text
0 → 31
```

Count

```text
ones
```

Then

```text
zeros = n - ones
```

Contribution

```text
ones × zeros
```

---

# Java Solution

```java
class Solution {

    public int totalHammingDistance(int[] nums) {

        int answer = 0;
        int n = nums.length;

        for (int bit = 0; bit < 32; bit++) {

            int ones = 0;

            for (int num : nums) {

                if (((num >> bit) & 1) == 1)
                    ones++;

            }

            answer += ones * (n - ones);

        }

        return answer;
    }
}
```

---

# Complexity

| Operation | Complexity |
|------------|------------|
| Time | O(32 × n) = O(n) |
| Space | O(1) |

---

# Why It Works

Each differing pair is counted exactly once for every differing bit.

No pair is missed.

No pair is counted twice.

---

# Interview Follow-up

Suppose numbers are

```text
64-bit
```

Nothing changes except

```text
64 iterations
```

instead of

```text
32
```

---

---

# Question 14 — Maximum Product of Word Lengths (LeetCode 318)

**Difficulty:** Medium

**[LLM-Proof]**

**Asked By:** Google, Meta, Amazon

---

## Problem

Given a list of lowercase words,

find two words with **no common characters**

such that

```text
length(word1) × length(word2)
```

is maximum.

Return the maximum product.

---

Example

```text
Input

["abcw","baz","foo","bar","xtfn","abcdef"]

Output

16
```

because

```text
abcw

×

xtfn

=

16
```

---

# Why This Is LLM-Proof

Many candidates use

```text
HashSet
```

for every comparison.

That works but is inefficient.

Interviewers expect you to encode character sets into a single integer using bit masks.

---

# Observation

Lowercase English letters

```text
26
```

fit inside one integer.

Each bit represents one character.

---

Example

```text
Word

abc
```

Mask

```text
a

000...001

b

000...010

c

000...100

----------------

111
```

---

Word

```text
bd
```

Mask

```text
1010
```

---

Common letters?

Simply compute

```text
mask1 & mask2
```

---

If result equals

```text
0
```

they share no characters.

---

# Visualization

```text
abc

111

xyz

11100000000000000000000000

AND

↓

00000000000000000000000000

↓

Valid Pair
```

---

# Algorithm

Build one bitmask per word.

Then compare every pair.

---

# Java Solution

```java
class Solution {

    public int maxProduct(String[] words) {

        int n = words.length;

        int[] masks = new int[n];

        for (int i = 0; i < n; i++) {

            int mask = 0;

            for (char ch : words[i].toCharArray()) {

                mask |= (1 << (ch - 'a'));

            }

            masks[i] = mask;

        }

        int answer = 0;

        for (int i = 0; i < n; i++) {

            for (int j = i + 1; j < n; j++) {

                if ((masks[i] & masks[j]) == 0) {

                    answer = Math.max(
                        answer,
                        words[i].length() * words[j].length()
                    );

                }
            }
        }

        return answer;
    }
}
```

---

# Complexity

| Operation | Complexity |
|------------|------------|
| Time | O(n²) |
| Space | O(n) |

---

# Why It Works

Every character corresponds to one bit.

Bitwise AND instantly tells whether two words share a character.

Instead of comparing letters individually,

we compare entire character sets in one operation.

---

# Interview Insight

This pattern appears frequently in

- search engines
- compiler symbol tables
- permission systems
- feature flags
- indexing systems

---

---

# Question 15 — Maximum XOR With an Element From Array (LeetCode 1707)

**Difficulty:** Hard

**[LLM-Proof]**

**Asked By:** Google, Meta

---

## Problem

Given

- an array
- multiple queries

Each query contains

```text
(x, m)
```

Find the maximum

```text
x XOR value
```

where

```text
value ≤ m
```

If no valid number exists,

return

```text
-1
```

---

# Why This Is LLM-Proof

This combines several interview topics:

- Bit Manipulation
- Binary Trie
- Offline Query Processing
- Sorting
- Greedy Optimization

Memorizing one pattern is not enough.

---

# Naive Solution

For every query

scan the entire array.

```text
O(nq)
```

Too slow.

---

# Observation

Queries have limits.

```text
value ≤ m
```

Sort

- array
- queries

Insert only valid numbers into the trie.

---

# Visualization

```text
Array

1

2

4

8

16

Queries

m = 4

↓

Insert

1

2

4

Trie

↓

Answer Query

↓

Next Query

m = 16

↓

Insert

8

16

↓

Answer Again
```

The trie grows only once.

---

# High-Level Algorithm

1. Sort array.
2. Sort queries by `m`.
3. Maintain pointer into array.
4. Insert eligible numbers into trie.
5. Answer each query greedily.
6. Restore original order.

---

# Trie Structure

```text
Root

├──0

│   ├──0

│   └──1

└──1

    ├──0

    └──1
```

Each level stores one bit.

---

# Java Solution

```java
import java.util.*;

class Solution {

    static class TrieNode {
        TrieNode[] child = new TrieNode[2];
    }

    TrieNode root = new TrieNode();

    private void insert(int num) {

        TrieNode node = root;

        for (int i = 31; i >= 0; i--) {

            int bit = (num >> i) & 1;

            if (node.child[bit] == null)
                node.child[bit] = new TrieNode();

            node = node.child[bit];
        }
    }

    private int getMaxXor(int num) {

        TrieNode node = root;

        if (node.child[0] == null && node.child[1] == null)
            return -1;

        int xor = 0;

        for (int i = 31; i >= 0; i--) {

            int bit = (num >> i) & 1;

            int opposite = bit ^ 1;

            if (node.child[opposite] != null) {

                xor |= (1 << i);
                node = node.child[opposite];

            } else {

                node = node.child[bit];

            }
        }

        return xor;
    }

    public int[] maximizeXor(int[] nums, int[][] queries) {

        Arrays.sort(nums);

        int q = queries.length;

        int[][] offline = new int[q][3];

        for (int i = 0; i < q; i++) {

            offline[i][0] = queries[i][0];
            offline[i][1] = queries[i][1];
            offline[i][2] = i;

        }

        Arrays.sort(offline, Comparator.comparingInt(a -> a[1]));

        int[] answer = new int[q];

        int index = 0;

        for (int[] query : offline) {

            while (index < nums.length &&
                    nums[index] <= query[1]) {

                insert(nums[index]);
                index++;

            }

            answer[query[2]] = getMaxXor(query[0]);

        }

        return answer;
    }
}
```

---

# Complexity

| Operation | Complexity |
|------------|------------|
| Sorting | O(n log n + q log q) |
| Trie Insert | O(32n) |
| Query | O(32q) |
| Overall | O((n + q) log n) approximately |
| Space | O(32n) |

---

# Why It Works

Sorting guarantees every inserted number satisfies

```text
value ≤ m
```

The trie always contains exactly the valid candidates.

Greedy traversal maximizes XOR one bit at a time.

---

# Common Interview Mistakes

❌ Build one trie for every query.

```text
O(nq)
```

---

❌ Ignore query ordering.

Sorting is the optimization.

---

❌ Forget original query indices.

Answers must be returned in original order.

---

# Patterns and Mental Models

The 15 problems in this guide reduce to a handful of reusable interview patterns.

| Pattern | Questions |
|----------|-----------|
| XOR cancels duplicates | 136, 268 |
| Remove lowest set bit (`n & (n-1)`) | 191, 231 |
| Dynamic Programming on bits | 338 |
| Bit extraction and reconstruction | 190 |
| Simulating hardware logic | 371 |
| Count bits column-wise | 137, 477 |
| Common binary prefix | 201 |
| Binary Trie | 421, 1707 |
| Character-set bitmasking | 318 |
| Bit-prefix validation | 393 |
| Independent per-bit decisions | 1318 |

---

# Interview Mental Checklist

When you see a new bit manipulation problem, ask these questions in order:

1. Can duplicates be eliminated with **XOR**?
2. Does `n & (n - 1)` simplify the number?
3. Can the answer be computed **bit by bit** independently?
4. Is there a **binary prefix** that never changes?
5. Should each value be represented as a **bitmask**?
6. Does a **Binary Trie** maximize or minimize XOR?
7. Is the problem really simulating **digital hardware** (carry, masks, shifts)?
8. Can preprocessing convert repeated bit operations into constant-time queries?

If you can identify which of these patterns applies, most bit manipulation interview questions become variations of a familiar idea rather than entirely new problems.

---

## Final Revision Order (Highest ROI)

1. **136** – Single Number
2. **191** – Number of 1 Bits
3. **231** – Power of Two
4. **268** – Missing Number
5. **338** – Counting Bits
6. **190** – Reverse Bits
7. **371** – Sum of Two Integers ⭐
8. **137** – Single Number II ⭐
9. **201** – Bitwise AND of Range ⭐
10. **421** – Maximum XOR ⭐
11. **393** – UTF-8 Validation
12. **1318** – Minimum Flips
13. **477** – Total Hamming Distance
14. **318** – Maximum Product of Word Lengths ⭐
15. **1707** – Maximum XOR With Constraints ⭐

**⭐ = Highest-value conceptual questions for FAANG and LLM-style interviews.**



