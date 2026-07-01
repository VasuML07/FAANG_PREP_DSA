# Math & Geometry LeetCode Interview Guide

---

# 1. Plus One

**LeetCode:** https://leetcode.com/problems/plus-one/

**Difficulty:** Easy

**Companies**

`Google` `Amazon` `Microsoft` `Adobe` `Apple`

---

## Problem

You are given a large integer represented as an integer array.

Increment the integer by one and return the resulting array.

Example

```
Input : [1,2,3]
Output: [1,2,4]
```

```
Input : [9,9,9]
Output: [1,0,0,0]
```

---

## Interview Insight

Although this appears to be an array problem, the real interview objective is understanding **carry propagation** exactly like elementary addition.

Most candidates overcomplicate it using string conversion or BigInteger.

FAANG interviewers expect an **O(n)** in-place solution.

---

## Visualization

```
Index

0 1 2
-------
1 2 9

Add 1

1 3 0


-------------------

9 9 9

↓

0 0 0
Carry remains

↓

1 0 0 0
```

---

## Solution Approach

Traverse from the last digit.

### Case 1

Current digit < 9

```
Increase it
Return immediately
```

Example

```
123

↓

124
```

---

### Case 2

Current digit = 9

```
Set it to 0

Carry continues
```

Example

```
129

↓

130
```

---

### Case 3

Every digit was 9

```
999

↓

000

Need new array

1000
```

---

## Java Solution

```java
class Solution {

    public int[] plusOne(int[] digits) {

        for (int i = digits.length - 1; i >= 0; i--) {

            if (digits[i] < 9) {
                digits[i]++;
                return digits;
            }

            digits[i] = 0;
        }

        int[] ans = new int[digits.length + 1];
        ans[0] = 1;

        return ans;
    }
}
```

---

## Edge Cases

✔ Single digit

```
7

↓

8
```

---

✔ Last digit not 9

```
451

↓

452
```

---

✔ Multiple carry

```
1999

↓

2000
```

---

✔ All 9s

```
99999

↓

100000
```

---

## Common Mistakes

- Using BigInteger
- Converting to String
- Forgetting all-9 case
- Returning original array after carry survives

---

## Complexity

| Metric | Value |
|---------|---------|
| Time | O(n) |
| Space | O(1) |

---

# 2. Palindrome Number

**LeetCode:** https://leetcode.com/problems/palindrome-number/

**Difficulty:** Easy

**Companies**

`Google` `Amazon` `Meta` `Microsoft` `Bloomberg`

---

## Problem

Determine whether an integer reads the same forward and backward.

Example

```
121

↓

true
```

```
123

↓

false
```

---

## Interview Insight

The trick isn't reversing the whole number.

Doing so may overflow.

Instead,

Reverse **only half**.

This elegant optimization is what interviewers look for.

---

## Visualization

```
12321

Left = 12

Right reversed = 12

Equal

Palindrome
```

Even digits

```
1221

12

12

Equal
```

---

## Solution Approach

Immediately reject

```
Negative numbers

or

Ending with zero
(except zero itself)
```

Now repeatedly

```
Take last digit

Append to reversedHalf
```

Stop when

```
Original <= reversedHalf
```

Finally compare

```
Even digits

x == rev

Odd digits

x == rev/10
```

---

## Java Solution

```java
class Solution {

    public boolean isPalindrome(int x) {

        if (x < 0 || (x % 10 == 0 && x != 0))
            return false;

        int reversed = 0;

        while (x > reversed) {

            reversed = reversed * 10 + x % 10;

            x /= 10;
        }

        return x == reversed || x == reversed / 10;
    }
}
```

---

## Trace

```
1221

x =1221

rev=1

x=122

rev=12

Stop

122/10=12

Equal
```

---

## Important Trick

Odd length

```
12321

Reverse

123

Ignore middle

12

Compare

12
```

---

## Complexity

| Metric | Value |
|---------|---------|
| Time | O(log₁₀N) |
| Space | O(1) |

---

## Common Mistakes

- Reversing entire integer
- Integer overflow
- Forgetting numbers ending in zero

---

# 3. Rotate Image ⭐ LLM-Proof

**LeetCode**

https://leetcode.com/problems/rotate-image/

**Difficulty**

Medium

**Companies**

`Google`

`Meta`

`Amazon`

`Apple`

`Microsoft`

`Uber`

`Airbnb`

---

## Why This is LLM-Proof

Many candidates memorize

```
Transpose

↓

Reverse rows
```

but fail when asked

- rotate anticlockwise
- rotate 180°
- rotate only outer layer
- rotate rectangular matrix

Interviewers frequently extend the problem.

Understanding the geometry is essential.

---

## Problem

Rotate an

```
n × n
```

matrix

```
90°
clockwise

In-place
```

---

## Visualization

Original

```
1 2 3

4 5 6

7 8 9
```

Coordinates

```
(r,c)
```

become

```
(c,n-1-r)
```

Result

```
7 4 1

8 5 2

9 6 3
```

---

## Geometry Insight

Instead of moving every element individually,

decompose rotation into two operations.

```
Transpose

↓

Reverse every row
```

Why?

Transpose

```
(r,c)

↓

(c,r)
```

Row reverse

```
(c,r)

↓

(c,n-1-r)
```

Exactly the required transformation.

---

## Visualization

### Step 1

Transpose

```
1 4 7

2 5 8

3 6 9
```

---

### Step 2

Reverse Rows

```
7 4 1

8 5 2

9 6 3
```

Done.

---

## Java Solution

```java
class Solution {

    public void rotate(int[][] matrix) {

        int n = matrix.length;

        // transpose

        for (int i = 0; i < n; i++) {

            for (int j = i + 1; j < n; j++) {

                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }

        // reverse rows

        for (int[] row : matrix) {

            int left = 0;
            int right = n - 1;

            while (left < right) {

                int temp = row[left];
                row[left] = row[right];
                row[right] = temp;

                left++;
                right--;
            }
        }
    }
}
```

---

## Interview Extensions

### Rotate Anti-clockwise

```
Transpose

↓

Reverse columns
```

---

### Rotate 180°

```
Reverse rows

↓

Reverse columns
```

---

### Rotate k Times

```
k %= 4
```

---

## Edge Cases

```
1×1
```

```
Already rotated
```

---

```
2×2
```

```
Swap carefully
```

---

```
Large matrix

Avoid extra matrix
```

---

## Common Mistakes

- Using extra matrix despite in-place requirement
- Incorrect transpose loop (`j = i + 1`)
- Forgetting only upper triangle should be swapped
- Reversing columns instead of rows for clockwise rotation

---

## Complexity

| Metric | Value |
|---------|---------|
| Time | O(n²) |
| Space | O(1) |

---

**End of Part 1 (Problems 1–3).**

# 4. Spiral Matrix

**LeetCode:** https://leetcode.com/problems/spiral-matrix/

**Difficulty:** Medium

**Companies**

`Amazon` `Google` `Microsoft` `Meta` `Adobe`

---

## Problem

Given an `m × n` matrix, return all elements in spiral order.

Example

```
Input

1 2 3
4 5 6
7 8 9

Output

1 2 3 6 9 8 7 4 5
```

---

## Interview Insight

This problem is about **boundary simulation**.

Instead of marking visited cells, maintain four boundaries:

- Top
- Bottom
- Left
- Right

Shrink the boundaries after every traversal.

---

## Visualization

Initial boundaries

```
Top = 0
Bottom = 2
Left = 0
Right = 2

+---------+
|1 2 3|
|4 5 6|
|7 8 9|
+---------+
```

Traversal

```
→ Top Row

↓

↓ Right Column

↓

← Bottom Row

↓

↑ Left Column
```

Then shrink

```
Top++

Right--

Bottom--

Left++
```

Repeat until boundaries cross.

---

## Solution Approach

Maintain

```
top
bottom
left
right
```

Loop while

```
top <= bottom
&&
left <= right
```

Perform four traversals carefully.

Always check boundaries before traversing bottom row and left column.

---

## Java Solution

```java
class Solution {

    public List<Integer> spiralOrder(int[][] matrix) {

        List<Integer> ans = new ArrayList<>();

        int top = 0;
        int bottom = matrix.length - 1;
        int left = 0;
        int right = matrix[0].length - 1;

        while (top <= bottom && left <= right) {

            for (int j = left; j <= right; j++)
                ans.add(matrix[top][j]);
            top++;

            for (int i = top; i <= bottom; i++)
                ans.add(matrix[i][right]);
            right--;

            if (top <= bottom) {
                for (int j = right; j >= left; j--)
                    ans.add(matrix[bottom][j]);
                bottom--;
            }

            if (left <= right) {
                for (int i = bottom; i >= top; i--)
                    ans.add(matrix[i][left]);
                left++;
            }
        }

        return ans;
    }
}
```

---

## Edge Cases

✔ Single row

```
1 2 3
```

---

✔ Single column

```
1
2
3
```

---

✔ Rectangle

```
3 × 5
```

---

## Common Mistakes

- Traversing bottom row after boundaries cross
- Missing center element
- Forgetting boundary updates

---

## Complexity

| Metric | Value |
|---------|---------|
| Time | O(mn) |
| Space | O(1) (excluding output) |

---

# 5. Set Matrix Zeroes ⭐ LLM-Proof

**LeetCode:** https://leetcode.com/problems/set-matrix-zeroes/

**Difficulty:** Medium

**Companies**

`Google` `Meta` `Amazon` `Apple` `Microsoft`

---

## Why This is LLM-Proof

Many candidates immediately use

```
O(m+n)
```

extra arrays.

Interviewers often insist on

```
Constant Space
```

Recognizing that the **first row and first column can store marker information** is the key insight.

---

## Problem

If an element is zero,

make its entire row and column zero.

Must be done **in-place**.

---

## Visualization

Original

```
1 1 1

1 0 1

1 1 1
```

Markers

```
First row

↓

Column markers

First column

↓

Row markers
```

After marking

```
1 0 1

0 0 1

1 1 1
```

Final

```
1 0 1

0 0 0

1 0 1
```

---

## Solution Approach

Maintain

```
firstRowZero

firstColZero
```

### Pass 1

Store markers

```
matrix[i][0]

matrix[0][j]
```

### Pass 2

Zero interior

### Pass 3

Zero first row

### Pass 4

Zero first column

---

## Java Solution

```java
class Solution {

    public void setZeroes(int[][] matrix) {

        int m = matrix.length;
        int n = matrix[0].length;

        boolean firstRow = false;
        boolean firstCol = false;

        for (int j = 0; j < n; j++)
            if (matrix[0][j] == 0)
                firstRow = true;

        for (int i = 0; i < m; i++)
            if (matrix[i][0] == 0)
                firstCol = true;

        for (int i = 1; i < m; i++) {

            for (int j = 1; j < n; j++) {

                if (matrix[i][j] == 0) {

                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }

        for (int i = 1; i < m; i++) {

            for (int j = 1; j < n; j++) {

                if (matrix[i][0] == 0 || matrix[0][j] == 0)
                    matrix[i][j] = 0;
            }
        }

        if (firstRow)
            Arrays.fill(matrix[0], 0);

        if (firstCol) {

            for (int i = 0; i < m; i++)
                matrix[i][0] = 0;
        }
    }
}
```

---

## Important Trick

Never overwrite the first row/column before using them as markers.

---

## Common Mistakes

- Destroying marker information
- Forgetting first row
- Forgetting first column

---

## Complexity

| Metric | Value |
|---------|---------|
| Time | O(mn) |
| Space | O(1) |

---

# 6. Max Points on a Line ⭐ LLM-Proof

**LeetCode:** https://leetcode.com/problems/max-points-on-a-line/

**Difficulty:** Hard

**Companies**

`Google` `Meta` `Amazon` `Apple` `Microsoft` `Uber`

---

## Why This is LLM-Proof

The obvious solution compares slopes using floating-point values.

That fails because of:

- Precision errors
- Vertical lines
- Duplicate points
- Negative slopes

The correct solution normalizes slopes using **GCD**, requiring careful mathematical reasoning.

---

## Problem

Given points on a 2D plane, return the maximum number of points that lie on the same straight line.

---

## Visualization

```
•

     •

          •

All three satisfy

(y₂-y₁)/(x₂-x₁)

Same normalized slope
```

---

## Geometry Insight

Instead of storing

```
dy/dx
```

as floating-point,

store

```
dy / gcd

dx / gcd
```

Example

```
6/8

↓

3/4
```

Now every equivalent slope has a unique representation.

---

## Solution Approach

For every point

```
Anchor
```

Compute slopes to all remaining points.

Normalize

```
dy

dx
```

using

```
gcd(dy,dx)
```

Store frequency in a HashMap.

Maximum frequency + anchor gives the answer.

---

## Java Solution

```java
class Solution {

    public int maxPoints(int[][] points) {

        if (points.length <= 2)
            return points.length;

        int answer = 0;

        for (int i = 0; i < points.length; i++) {

            Map<String, Integer> map = new HashMap<>();
            int localMax = 0;

            for (int j = i + 1; j < points.length; j++) {

                int dx = points[j][0] - points[i][0];
                int dy = points[j][1] - points[i][1];

                int g = gcd(dx, dy);

                dx /= g;
                dy /= g;

                String key = dx + "/" + dy;

                map.put(key, map.getOrDefault(key, 0) + 1);

                localMax = Math.max(localMax, map.get(key));
            }

            answer = Math.max(answer, localMax + 1);
        }

        return answer;
    }

    private int gcd(int a, int b) {

        if (b == 0)
            return Math.abs(a);

        return gcd(b, a % b);
    }
}
```

---

## Interview Trick

Instead of

```
Slope = 0.333333333333
```

store

```
1/3
```

Instead of

```
0.6666666666
```

store

```
2/3
```

After GCD normalization,

both become consistent representations, eliminating floating-point precision issues.

---

## Edge Cases

✔ Vertical line

```
dx = 0
```

---

✔ Horizontal line

```
dy = 0
```

---

✔ Duplicate points

---

✔ Negative slopes

Normalize sign consistently.

---

## Common Mistakes

- Using `double` for slope
- Ignoring duplicate points
- Forgetting GCD normalization
- Incorrect handling of sign (`-1/2` vs `1/-2`)

---

## Complexity

| Metric | Value |
|---------|---------|
| Time | O(n²) |
| Space | O(n) |

---

**End of Part 2 (Problems 4–6).**

# 7. Rotate Function

**LeetCode:** https://leetcode.com/problems/rotate-function/

**Difficulty:** Medium

**Companies**

`Amazon` `Google` `Microsoft`

---

## Problem

Given an integer array `nums`, define the rotation function:

```
F(k) = 0 * Bk[0] + 1 * Bk[1] + ... + (n-1) * Bk[n-1]
```

where `Bk` is the array rotated clockwise `k` positions.

Return the maximum value among all rotations.

---

## Interview Insight

The brute-force solution recalculates every rotation.

```
O(n²)
```

The optimal solution derives a mathematical recurrence.

---

## Visualization

```
nums

4 3 2 6

F(0)

0·4 + 1·3 + 2·2 + 3·6 = 25

↓

Rotate

6 4 3 2

F(1)

16
```

Instead of recomputing everything,

derive

```
F(k)

↓

F(k+1)
```

---

## Mathematical Derivation

Let

```
SUM = total array sum
```

Then

```
F(k)

↓

F(k+1)

= F(k)
+ SUM
- n * movedElement
```

The moved element is

```
nums[n-k-1]
```

This recurrence makes every next rotation constant time.

---

## Solution Approach

1. Compute total sum.
2. Compute `F(0)`.
3. Apply recurrence for every rotation.
4. Track maximum.

---

## Java Solution

```java
class Solution {

    public int maxRotateFunction(int[] nums) {

        int n = nums.length;

        long sum = 0;
        long value = 0;

        for (int i = 0; i < n; i++) {
            sum += nums[i];
            value += (long) i * nums[i];
        }

        long ans = value;

        for (int i = n - 1; i >= 1; i--) {

            value = value + sum - (long) n * nums[i];
            ans = Math.max(ans, value);
        }

        return (int) ans;
    }
}
```

---

## Edge Cases

✔ Single element

✔ Negative numbers

✔ Large values

---

## Common Mistakes

- Recomputing every rotation
- Integer overflow
- Wrong recurrence index

---

## Complexity

| Metric | Value |
|---------|---------|
| Time | O(n) |
| Space | O(1) |

---

# 8. Multiply Strings

**LeetCode:** https://leetcode.com/problems/multiply-strings/

**Difficulty:** Medium

**Companies**

`Google` `Amazon` `Apple` `Meta` `Microsoft`

---

## Problem

Multiply two non-negative integers represented as strings.

Do not use

```
BigInteger
```

or convert them directly into integers.

---

## Interview Insight

This problem simulates manual multiplication.

Understanding digit placement is more important than memorizing code.

---

## Visualization

```
   123
×   45
-------

   615

4920

-------

5535
```

Array representation

```
Length

m+n

because

999 × 999

↓

998001

3 digits × 3 digits

↓

6 digits
```

---

## Key Mathematical Observation

If

```
i

j
```

are digit positions,

their product contributes to

```
i+j+1
```

Carry contributes to

```
i+j
```

---

## Solution Approach

Create

```
answer[m+n]
```

Multiply every digit pair.

Store

```
sum

↓

carry

↓

remainder
```

Finally skip leading zeros.

---

## Java Solution

```java
class Solution {

    public String multiply(String num1, String num2) {

        if (num1.equals("0") || num2.equals("0"))
            return "0";

        int m = num1.length();
        int n = num2.length();

        int[] ans = new int[m + n];

        for (int i = m - 1; i >= 0; i--) {

            for (int j = n - 1; j >= 0; j--) {

                int mul = (num1.charAt(i) - '0')
                        * (num2.charAt(j) - '0');

                int sum = mul + ans[i + j + 1];

                ans[i + j + 1] = sum % 10;
                ans[i + j] += sum / 10;
            }
        }

        StringBuilder sb = new StringBuilder();

        for (int digit : ans) {

            if (!(sb.length() == 0 && digit == 0))
                sb.append(digit);
        }

        return sb.toString();
    }
}
```

---

## Execution Trace

```
12

×

34

↓

8

6

4

3

↓

408
```

Exactly as done manually.

---

## Common Mistakes

- Wrong carry position
- Incorrect output length
- Forgetting leading zeros
- Using integer conversion

---

## Complexity

| Metric | Value |
|---------|---------|
| Time | O(mn) |
| Space | O(m+n) |

---

# 9. Integer to English Words ⭐ LLM-Proof

**LeetCode:** https://leetcode.com/problems/integer-to-english-words/

**Difficulty:** Hard

**Companies**

`Google` `Amazon` `Microsoft` `Meta` `Apple`

---

## Why This is LLM-Proof

The challenge is not coding.

The challenge is correctly decomposing numbers into linguistic groups.

Interviewers frequently modify this problem:

- Different language
- Indian numbering system
- Decimal support
- Currency formatting

Candidates who memorize code usually fail these extensions.

---

## Problem

Convert an integer into its English words representation.

Example

```
123

↓

One Hundred Twenty Three
```

```
1234567

↓

One Million Two Hundred Thirty Four Thousand Five Hundred Sixty Seven
```

---

## Visualization

Split into groups of three digits.

```
1

234

567

↓

Million

Thousand

Units
```

Each group is processed independently.

---

## Solution Approach

Create lookup arrays.

```
Below Twenty

Tens

Thousands
```

Recursively process

```
<20

<100

<1000
```

Then append

```
Thousand

Million

Billion
```

---

## Java Solution

```java
class Solution {

    private final String[] below20 = {
            "", "One", "Two", "Three", "Four", "Five", "Six",
            "Seven", "Eight", "Nine", "Ten", "Eleven",
            "Twelve", "Thirteen", "Fourteen", "Fifteen",
            "Sixteen", "Seventeen", "Eighteen", "Nineteen"
    };

    private final String[] tens = {
            "", "", "Twenty", "Thirty", "Forty",
            "Fifty", "Sixty", "Seventy", "Eighty", "Ninety"
    };

    private final String[] thousands = {
            "", "Thousand", "Million", "Billion"
    };

    public String numberToWords(int num) {

        if (num == 0)
            return "Zero";

        int index = 0;

        StringBuilder result = new StringBuilder();

        while (num > 0) {

            if (num % 1000 != 0) {

                String part = helper(num % 1000);

                result.insert(0,
                        part + thousands[index] + " ");
            }

            num /= 1000;
            index++;
        }

        return result.toString().trim();
    }

    private String helper(int num) {

        if (num == 0)
            return "";

        if (num < 20)
            return below20[num] + " ";

        if (num < 100)
            return tens[num / 10] + " " +
                    helper(num % 10);

        return below20[num / 100] +
                " Hundred " +
                helper(num % 100);
    }
}
```

---

## Important Insight

Every number is reduced into

```
XYZ

↓

Hundreds

Tens

Units
```

The recursion depth is extremely small because every recursive call processes at most three digits.

---

## Edge Cases

✔ Zero

✔ Exact hundreds

```
500

↓

Five Hundred
```

✔ Exact thousands

```
1000

↓

One Thousand
```

✔ Maximum input

```
2,147,483,647
```

---

## Common Mistakes

- Extra spaces
- Missing "Hundred"
- Wrong thousand grouping
- Incorrect recursion base case

---

## Complexity

| Metric | Value |
|---------|---------|
| Time | O(log₁₀N) |
| Space | O(log₁₀N) (recursion + output construction) |

---

**End of Part 3 (Problems 7–9).**

# 10. Happy Number

**LeetCode:** https://leetcode.com/problems/happy-number/

**Difficulty:** Easy

**Companies**

`Google` `Amazon` `Microsoft` `Adobe`

---

## Problem

A happy number is defined by repeatedly replacing the number with the sum of the squares of its digits until:

- It becomes **1** (happy), or
- It enters a cycle (not happy).

Return `true` if the number is happy.

Example

```
Input

19

Output

true
```

---

## Interview Insight

The hidden problem is **cycle detection**, not arithmetic.

Instead of storing every visited number in a HashSet, the optimal interview solution uses **Floyd's Cycle Detection (Tortoise & Hare)**.

---

## Visualization

```
19

↓

82

↓

68

↓

100

↓

1
```

Cycle example

```
2

↓

4

↓

16

↓

37

↓

58

↓

89

↓

145

↓

42

↓

20

↓

4

(repeats)
```

---

## Solution Approach

Use

```
slow

fast
```

where

```
slow

↓

next(number)

fast

↓

next(next(number))
```

If

```
fast == 1

Happy Number
```

If

```
slow == fast

Cycle detected
```

---

## Java Solution

```java
class Solution {

    public boolean isHappy(int n) {

        int slow = n;
        int fast = next(n);

        while (fast != 1 && slow != fast) {

            slow = next(slow);
            fast = next(next(fast));
        }

        return fast == 1;
    }

    private int next(int n) {

        int sum = 0;

        while (n > 0) {

            int digit = n % 10;
            sum += digit * digit;
            n /= 10;
        }

        return sum;
    }
}
```

---

## Edge Cases

✔ 1

✔ Large numbers

✔ Numbers entering long cycles

---

## Common Mistakes

- Using infinite loop
- Forgetting cycle detection
- Incorrect digit extraction

---

## Complexity

| Metric | Value |
|---------|---------|
| Time | O(log N) (per iteration, bounded overall) |
| Space | O(1) |

---

# 11. Rectangle Area

**LeetCode:** https://leetcode.com/problems/rectangle-area/

**Difficulty:** Medium

**Companies**

`Google` `Amazon` `Meta` `Microsoft`

---

## Problem

Given coordinates of two axis-aligned rectangles, compute their total covered area.

---

## Geometry Visualization

Rectangle A

```
+---------+
|         |
|         |
+---------+
```

Rectangle B

```
      +---------+
      |         |
      |         |
      +---------+
```

Total Area

```
Area(A)

+

Area(B)

-

Overlap
```

---

## Geometry Formula

Rectangle Area

```
(width)

×

(height)
```

Overlap Width

```
min(right edges)

-

max(left edges)
```

Overlap Height

```
min(top edges)

-

max(bottom edges)
```

Negative overlap

↓

Treat as zero.

---

## Solution Approach

Compute

```
Area1

Area2

Overlap
```

Answer

```
Area1 + Area2 - Overlap
```

---

## Java Solution

```java
class Solution {

    public int computeArea(int ax1, int ay1,
                           int ax2, int ay2,
                           int bx1, int by1,
                           int bx2, int by2) {

        int area1 = (ax2 - ax1) * (ay2 - ay1);
        int area2 = (bx2 - bx1) * (by2 - by1);

        int overlapWidth =
                Math.max(0,
                        Math.min(ax2, bx2) -
                        Math.max(ax1, bx1));

        int overlapHeight =
                Math.max(0,
                        Math.min(ay2, by2) -
                        Math.max(ay1, by1));

        return area1 + area2 -
                overlapWidth * overlapHeight;
    }
}
```

---

## Important Formula

```
Overlap

=

max(0,

min(Right)

-

max(Left))
```

Same for height.

---

## Common Mistakes

- Forgetting non-overlapping rectangles
- Using absolute values
- Incorrect overlap calculation

---

## Complexity

| Metric | Value |
|---------|---------|
| Time | O(1) |
| Space | O(1) |

---

# 12. The Skyline Problem ⭐ LLM-Proof

**LeetCode:** https://leetcode.com/problems/the-skyline-problem/

**Difficulty:** Hard

**Companies**

`Google` `Amazon` `Meta` `Microsoft` `Apple`

---

## Why This is LLM-Proof

The difficulty isn't implementing a heap.

The challenge is recognizing that buildings generate **events** and that the skyline changes only at event boundaries.

Interviewers frequently ask follow-ups such as:

- Dynamic building insertion
- Removing buildings
- Skyline area computation
- Merging skylines

These require understanding the sweep-line paradigm rather than memorized code.

---

## Problem

Given a list of buildings,

```
[left, right, height]
```

return the skyline formed by these buildings.

---

## Visualization

Buildings

```
        _______

   _____|       |

__|              |____
```

Skyline

```
      ┌─────┐
      │     │
┌─────┘     └─────┐
│                 │
└─────────────────┘
```

---

## Core Idea

Convert every building into two events.

```
Start

(x, -height)

End

(x, +height)
```

Negative height ensures starts are processed before ends.

Sort all events.

Maintain active heights using a max-heap (PriorityQueue in reverse order or TreeMap).

Whenever the maximum height changes,

record a skyline point.

---

## Solution Approach

1. Convert buildings into start/end events.
2. Sort events by x-coordinate.
3. Maintain active heights.
4. Detect height changes.
5. Add key points.

---

## Java Solution

```java
class Solution {

    public List<List<Integer>> getSkyline(int[][] buildings) {

        List<int[]> events = new ArrayList<>();

        for (int[] b : buildings) {
            events.add(new int[]{b[0], -b[2]});
            events.add(new int[]{b[1], b[2]});
        }

        events.sort((a, b) -> {
            if (a[0] != b[0])
                return a[0] - b[0];
            return a[1] - b[1];
        });

        TreeMap<Integer, Integer> heights =
                new TreeMap<>(Collections.reverseOrder());

        heights.put(0, 1);

        int prev = 0;

        List<List<Integer>> ans = new ArrayList<>();

        for (int[] e : events) {

            int x = e[0];
            int h = e[1];

            if (h < 0) {

                heights.put(-h,
                        heights.getOrDefault(-h, 0) + 1);

            } else {

                heights.put(h,
                        heights.get(h) - 1);

                if (heights.get(h) == 0)
                    heights.remove(h);
            }

            int current = heights.firstKey();

            if (current != prev) {

                ans.add(Arrays.asList(x, current));
                prev = current;
            }
        }

        return ans;
    }
}
```

---

## Execution Trace

Buildings

```
[2,9,10]

[3,7,15]

[5,12,12]
```

Events

```
2  Start 10

3  Start 15

5  Start 12

7  End 15

9  End 10

12 End 12
```

Maximum height changes

↓

Skyline points recorded.

---

## Edge Cases

✔ Multiple buildings starting together

✔ Same ending coordinate

✔ Nested buildings

✔ Identical heights

---

## Common Mistakes

- Processing end events before start events
- Removing all copies of a duplicated height
- Using a simple PriorityQueue without lazy deletion
- Missing consecutive points with equal height

---

## Complexity

| Metric | Value |
|---------|---------|
| Time | O(n log n) |
| Space | O(n) |

---

**End of Part 4 (Problems 10–12).**

# 13. Detect Squares ⭐ LLM-Proof

**LeetCode:** https://leetcode.com/problems/detect-squares/

**Difficulty:** Medium

**Companies**

`Google` `Meta` `Amazon`

---

## Why This is LLM-Proof

This problem appears to be a geometry question, but the optimal solution combines:

- Coordinate Geometry
- Frequency Counting
- Hash Maps

Many candidates incorrectly try to enumerate every possible square.

Interviewers often extend this problem by asking:

- Detect rectangles instead of squares
- Count all unique squares
- Support point deletion
- Handle duplicate points efficiently

Understanding the geometric relationship between vertices is the real challenge.

---

## Problem

Design a data structure that supports:

- `add(point)`
- `count(point)`

Return the number of axis-aligned squares that can be formed using the query point.

---

## Geometry Insight

For an axis-aligned square,

```
(x1,y1)

+

(x2,y2)

↓

Diagonal
```

The remaining two points are

```
(x1,y2)

(x2,y1)
```

Side length

```
|x2-x1|

=

|y2-y1|
```

---

## Visualization

```
(x,y+d) ------- (x+d,y+d)

   |                 |

   |                 |

(x,y) -------- (x+d,y)
```

If all four points exist,

one square is formed.

---

## Solution Approach

Maintain

```
Map<x,
    Map<y,count>>
```

For every point sharing the same x-coordinate,

calculate side length.

Check both possible horizontal directions.

Multiply frequencies of all three required points.

---

## Java Solution

```java
class DetectSquares {

    private Map<Integer, Map<Integer, Integer>> points;

    public DetectSquares() {
        points = new HashMap<>();
    }

    public void add(int[] point) {

        int x = point[0];
        int y = point[1];

        points.putIfAbsent(x, new HashMap<>());

        Map<Integer, Integer> column = points.get(x);

        column.put(y, column.getOrDefault(y, 0) + 1);
    }

    public int count(int[] point) {

        int x = point[0];
        int y = point[1];

        if (!points.containsKey(x))
            return 0;

        int answer = 0;

        Map<Integer, Integer> sameColumn = points.get(x);

        for (int ny : sameColumn.keySet()) {

            if (ny == y)
                continue;

            int side = ny - y;

            answer += sameColumn.get(ny)
                    * get(x + side, y)
                    * get(x + side, ny);

            answer += sameColumn.get(ny)
                    * get(x - side, y)
                    * get(x - side, ny);
        }

        return answer;
    }

    private int get(int x, int y) {

        if (!points.containsKey(x))
            return 0;

        return points.get(x).getOrDefault(y, 0);
    }
}
```

---

## Edge Cases

✔ Duplicate points

✔ Side length = 0

✔ Empty data structure

✔ Negative coordinates

---

## Common Mistakes

- Ignoring duplicate frequencies
- Forgetting both left and right squares
- Counting degenerate squares

---

## Complexity

| Metric | Value |
|---------|---------|
| Add | O(1) |
| Count | O(n) |
| Space | O(n) |

---

# 14. Mirror Reflection

**LeetCode:** https://leetcode.com/problems/mirror-reflection/

**Difficulty:** Medium

**Companies**

`Google` `Microsoft`

---

## Problem

A laser starts from the southwest corner of a square room.

The laser hits the east wall at height `q`.

The room has mirrors on every wall.

Determine which receptor the laser reaches first.

---

## Visualization

```
Room

+-------------+

|            /|

|          /  |

|        /    |

|      /      |

+-------------+
```

Instead of simulating reflections,

unfold the room.

---

## Mathematical Insight

The beam reaches a receptor when

```
LCM(p,q)
```

is achieved.

Compute

```
lcm = p*q / gcd(p,q)
```

Then

```
m = lcm/p

n = lcm/q
```

Cases

```
m even

↓

0

m odd

↓

1 or 2
```

---

## Decision Table

| m | n | Receptor |
|---|---|----------|
| Even | Odd | 0 |
| Odd | Odd | 1 |
| Odd | Even | 2 |

---

## Java Solution

```java
class Solution {

    public int mirrorReflection(int p, int q) {

        int g = gcd(p, q);

        int m = (p / g);
        int n = (q / g);

        if (m % 2 == 0)
            return 2;

        if (n % 2 == 0)
            return 0;

        return 1;
    }

    private int gcd(int a, int b) {

        if (b == 0)
            return a;

        return gcd(b, a % b);
    }
}
```

---

## Important Trick

Instead of simulating reflections,

convert the problem into

```
LCM

+

Parity
```

This reduces an infinite simulation to simple number theory.

---

## Common Mistakes

- Simulating reflections
- Wrong parity interpretation
- Incorrect LCM calculation

---

## Complexity

| Metric | Value |
|---------|---------|
| Time | O(log(min(p,q))) |
| Space | O(1) |

---

# 15. Robot Bounded In Circle

**LeetCode:** https://leetcode.com/problems/robot-bounded-in-circle/

**Difficulty:** Medium

**Companies**

`Google` `Amazon`

---

## Problem

Given a sequence of robot instructions:

```
G

Go

L

Turn Left

R

Turn Right
```

Determine whether repeating the instructions forever keeps the robot within a bounded circle.

---

## Visualization

Initial Direction

```
      N

W          E

      S
```

Example

```
GLGLGLG
```

Robot continually rotates

↓

Returns near origin

↓

Bounded

---

## Mathematical Insight

Only one execution of the instruction string is needed.

After one pass:

### Case 1

Robot returns to origin

```
Bounded
```

### Case 2

Robot changes direction

```
Also bounded

Future repetitions eventually loop.
```

### Case 3

Robot ends elsewhere facing North

```
Unbounded
```

---

## Solution Approach

Track

```
x

y

direction
```

Directions

```
0

North

1

East

2

South

3

West
```

Process instructions.

Finally check

```
Origin

OR

Direction != North
```

---

## Java Solution

```java
class Solution {

    public boolean isRobotBounded(String instructions) {

        int[][] dir = {
                {0,1},
                {1,0},
                {0,-1},
                {-1,0}
        };

        int x = 0;
        int y = 0;
        int d = 0;

        for (char c : instructions.toCharArray()) {

            if (c == 'G') {

                x += dir[d][0];
                y += dir[d][1];

            } else if (c == 'L') {

                d = (d + 3) % 4;

            } else {

                d = (d + 1) % 4;
            }
        }

        return (x == 0 && y == 0) || d != 0;
    }
}
```

---

## Execution Trace

Input

```
GGLLGG
```

Robot

```
North

↓

South

↓

Origin
```

Answer

```
true
```

---

## Edge Cases

✔ Empty movement

✔ Only turns

✔ Straight line

✔ Large instruction string

---

## Common Mistakes

- Simulating multiple repetitions
- Incorrect direction update
- Mixing x and y coordinates

---

## Complexity

| Metric | Value |
|---------|---------|
| Time | O(n) |
| Space | O(1) |

---

# FAANG Interview Revision Checklist

| Pattern | Representative Problem |
|----------|------------------------|
| Carry Propagation | Plus One |
| Half Reversal | Palindrome Number |
| Matrix Transformation | Rotate Image |
| Boundary Simulation | Spiral Matrix |
| In-place Matrix Marking | Set Matrix Zeroes |
| Geometry + GCD | Max Points on a Line |
| Mathematical Recurrence | Rotate Function |
| Manual Arithmetic | Multiply Strings |
| Recursive Number Decomposition | Integer to English Words |
| Floyd Cycle Detection | Happy Number |
| Coordinate Geometry | Rectangle Area |
| Sweep Line + Heap | Skyline Problem |
| Hash Map + Geometry | Detect Squares |
| GCD + LCM + Parity | Mirror Reflection |
| Simulation + State Machine | Robot Bounded in Circle |

---

# LLM-Proof Problems Summary

These problems require deeper reasoning than straightforward pattern matching:

| Problem | Why It's LLM-Proof |
|----------|--------------------|
| Rotate Image | Requires understanding coordinate transformations and matrix operations, enabling follow-up variants (anticlockwise, 180°, layer-wise). |
| Set Matrix Zeroes | The constant-space marker technique using the first row and column is a non-obvious optimization. |
| Max Points on a Line | Correct slope normalization with GCD avoids floating-point precision issues and handles vertical/duplicate points. |
| Integer to English Words | Requires recursive decomposition into linguistic groups rather than a standard algorithmic pattern. |
| Detect Squares | Combines coordinate geometry with frequency maps; interviewers often extend it to dynamic geometric queries. |
| Skyline Problem | Introduces the sweep-line paradigm with event processing and active height maintenance, a common advanced interview technique. |

---
**End of Math & Geometry LeetCode FAANG Interview Guide (15/15 Problems).**
