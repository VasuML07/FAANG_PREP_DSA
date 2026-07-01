# Two Pointers Interview Guide (Part 1)

> **Coverage in this part**
>
> - Core Two Pointer Patterns
> - Problem 1 — Valid Palindrome
> - Problem 2 — Two Sum II
> - Problem 3 — Remove Duplicates from Sorted Array
> - Problem 4 — Move Zeroes
> - Problem 5 — Container With Most Water

---

# Important Two Pointer Patterns

Unlike Hashing, Dynamic Programming, or Graphs, **Two Pointers is not a standalone algorithm.** It is a technique that reduces unnecessary iterations by intelligently maintaining two indices.

Most FAANG interview questions using two pointers fall into one of these patterns.

---

# Pattern 1 — Opposite-End Pointers

One pointer starts from the beginning.

Another pointer starts from the end.

They move toward each other.

```
0   1   2   3   4   5
L               R

↓

0   1   2   3   4   5
    L       R

↓

0   1   2   3   4   5
        LR
```

Used when:

- Array is sorted
- Comparing opposite values
- Palindrome checking
- Sum problems

Typical Problems

- Two Sum II
- Valid Palindrome
- 3Sum
- 4Sum
- Container With Most Water
- Trapping Rain Water

---

# Pattern 2 — Same Direction Pointers

Both pointers move from left to right.

One pointer explores.

The other pointer maintains a valid region.

```
0 1 2 3 4 5 6

S
F

↓

0 1 2 3 4 5 6

S     F

↓

0 1 2 3 4 5 6

    S       F
```

Applications

- Removing duplicates
- Move Zeroes
- Merge arrays
- Stable partitioning

---

# Pattern 3 — Fast and Slow Pointer

Fast pointer moves faster.

Slow pointer moves slower.

```
0 → 1 → 2 → 3 → 4 → 5

S

F

↓

Fast moves twice

↓

Eventually

F reaches end
S reaches midpoint
```

Applications

- Linked List Cycle
- Middle of Linked List
- Happy Number
- Detect loops

---

# Pattern 4 — Sliding Window (Special Two Pointer Pattern)

Both pointers move only forward.

Window expands.

Window shrinks.

```
L
↓

1 2 3 4 5 6
^

↓

L
      R

↓

Window expands

↓

Need smaller window

↓

L moves
```

Applications

- Longest substring
- Minimum window
- Fruit Into Baskets
- Subarray problems

---

# Pattern 5 — Partitioning

Pointers rearrange elements.

```
Before

3 0 1 0 5 0 9

↓

After

3 1 5 9 0 0 0
```

Applications

- Move Zeroes
- Dutch National Flag
- Sort Colors
- Remove Element

---

# Pattern Selection Cheat Sheet

| Situation | Pattern |
|-----------|----------|
| Sorted array | Opposite pointers |
| Remove duplicates | Same direction |
| Stable rearrangement | Same direction |
| Linked list | Fast/Slow |
| Window constraints | Sliding window |
| Rearranging array | Partition |

---

# Problem 1 — Valid Palindrome

**LeetCode:** 125

**Difficulty:** Easy

**Companies**

- Meta
- Amazon
- Microsoft
- Google
- Apple
- Adobe

---

## Problem Statement

Given a string, determine whether it is a palindrome after ignoring non-alphanumeric characters and case differences.

---

## Why Two Pointers?

A palindrome compares matching characters from both ends.

Instead of creating another reversed string,

we compare directly.

```
A man, a plan, a canal: Panama

L                                   R

↓

Skip spaces

↓

Compare

a == a

↓

Move inward

↓

Repeat
```

Every character is processed at most once.

---

## Pointer Visualization

```
a b c c b a
^         ^

Match

  ^     ^

Match

    ^ ^

Done
```

---

## Approach

1. Left starts at beginning.
2. Right starts at end.
3. Skip non-alphanumeric.
4. Convert to lowercase.
5. Compare.
6. Mismatch → false.
7. Finish → true.

---

## Java Solution

```java
class Solution {
    public boolean isPalindrome(String s) {

        int left = 0;
        int right = s.length() - 1;

        while (left < right) {

            while (left < right &&
                    !Character.isLetterOrDigit(s.charAt(left))) {
                left++;
            }

            while (left < right &&
                    !Character.isLetterOrDigit(s.charAt(right))) {
                right--;
            }

            if (Character.toLowerCase(s.charAt(left)) !=
                Character.toLowerCase(s.charAt(right))) {
                return false;
            }

            left++;
            right--;
        }

        return true;
    }
}
```

---

## Complexity

Time

```
O(n)
```

Space

```
O(1)
```

---

## Key Interview Insights

Interviewers expect:

- No extra string creation
- Skip punctuation correctly
- Handle empty string
- Handle only symbols

Common mistake:

```
"A."
```

Answer is **true**, not false.

---

## Follow-ups

- Unicode characters
- Ignore accents?
- Ignore only spaces?

---

# Problem 2 — Two Sum II

**LeetCode:** 167

**Difficulty:** Easy

**Companies**

- Amazon
- Google
- Apple
- Bloomberg
- Meta

---

## Problem Statement

Array is sorted.

Return indices whose values sum to target.

---

## Why Two Pointers?

Sorted order gives monotonic behavior.

```
1 2 4 6 8 10

L          R

Target = 12

1+10=11

Need bigger

L++

↓

2+10=12

Done
```

HashMap unnecessary.

---

## Pointer Visualization

```
1 2 4 6 8 10

L         R

↓

Increase left

↓

    L     R

Found
```

---

## Approach

If sum is small

```
left++
```

If sum is large

```
right--
```

Repeat.

---

## Java Solution

```java
class Solution {

    public int[] twoSum(int[] numbers, int target) {

        int left = 0;
        int right = numbers.length - 1;

        while (left < right) {

            int sum = numbers[left] + numbers[right];

            if (sum == target) {
                return new int[]{left + 1, right + 1};
            }

            if (sum < target) {
                left++;
            } else {
                right--;
            }
        }

        return new int[]{};
    }
}
```

---

## Complexity

Time

```
O(n)
```

Space

```
O(1)
```

---

## Key Interview Insights

This works **only because array is sorted.**

Without sorting,

HashMap is better.

Interviewers often ask:

> Why not binary search?

Binary search becomes O(n log n).

Two pointers remain O(n).

---

## Follow-ups

- Return all pairs.
- Duplicates allowed.
- Closest sum.

---

# Problem 3 — Remove Duplicates from Sorted Array

**LeetCode:** 26

**Difficulty:** Easy

**Companies**

- Google
- Meta
- Microsoft
- Amazon

---

## Problem Statement

Remove duplicates in-place.

Return new length.

---

## Why Two Pointers?

One pointer scans.

Another writes.

```
1 1 2 2 3 4 4

R

↓

Unique

W

↓

Overwrite duplicates
```

---

## Visualization

```
Original

1 1 2 2 3 4 4

W
R

↓

1 2 2 2 3 4 4

    W
      R

↓

1 2 3 2 3 4 4

      W
```

---

## Approach

Read pointer

Finds new values.

Write pointer

Stores only unique numbers.

---

## Java Solution

```java
class Solution {

    public int removeDuplicates(int[] nums) {

        if (nums.length == 0)
            return 0;

        int write = 1;

        for (int read = 1; read < nums.length; read++) {

            if (nums[read] != nums[read - 1]) {

                nums[write] = nums[read];
                write++;
            }
        }

        return write;
    }
}
```

---

## Complexity

Time

```
O(n)
```

Space

```
O(1)
```

---

## Key Interview Insights

Never shift elements one by one.

That becomes

```
O(n²)
```

---

## Follow-ups

- Allow duplicates twice.
- Remove element.
- Compress sorted array.

---

# Problem 4 — Move Zeroes

**LeetCode:** 283

**Difficulty:** Easy

**Companies**

- Facebook
- Amazon
- Apple
- Google
- Microsoft

---

## Problem Statement

Move all zeros to the end while maintaining order.

---

## Why Two Pointers?

Write pointer tracks next non-zero position.

Read pointer scans.

```
0 1 0 3 12

R

↓

Write non-zero

1 _ _ _ _

↓

Fill remaining with zeros
```

---

## Visualization

```
0 1 0 3 12

W
R

↓

1 0 0 3 12

  W
      R

↓

1 3 12 0 0
```

---

## Java Solution

```java
class Solution {

    public void moveZeroes(int[] nums) {

        int write = 0;

        for (int read = 0; read < nums.length; read++) {

            if (nums[read] != 0) {

                int temp = nums[write];
                nums[write] = nums[read];
                nums[read] = temp;

                write++;
            }
        }
    }
}
```

---

## Complexity

Time

```
O(n)
```

Space

```
O(1)
```

---

## Key Interview Insights

Stable order is preserved.

Do not create another array.

---

## Follow-ups

- Move negatives.
- Stable partition.
- Odd-even partition.

---

# Problem 5 — Container With Most Water

**LeetCode:** 11

**Difficulty:** Medium

**Companies**

- Amazon
- Google
- Apple
- Meta
- Bloomberg
- Uber

---

## Problem Statement

Find two lines that hold the maximum amount of water.

---

## Why Two Pointers?

Brute force checks every pair.

```
O(n²)
```

Observation:

Area depends on

```
min(height)

×

distance
```

Always move the **shorter wall**.

---

## Visualization

```
1 8 6 2 5 4 8 3 7

L               R

Area

=min(1,7)

×

8

↓

Move left

↓

8 6 2 5 4 8 3 7

  L           R

Higher chance of bigger area
```

---

## Why Move the Shorter Wall?

Suppose

```
Left = 4

Right = 10

Width = 8

Area = 32
```

Moving the taller wall

```
Width decreases

Minimum height still 4

Area cannot improve
```

Moving the shorter wall

```
Width decreases

But minimum height may increase

Area may improve
```

This greedy observation is the entire proof.

---

## Java Solution

```java
class Solution {

    public int maxArea(int[] height) {

        int left = 0;
        int right = height.length - 1;

        int maxArea = 0;

        while (left < right) {

            int width = right - left;
            int currentArea = Math.min(height[left], height[right]) * width;

            maxArea = Math.max(maxArea, currentArea);

            if (height[left] < height[right]) {
                left++;
            } else {
                right--;
            }
        }

        return maxArea;
    }
}
```

---

## Complexity

Time

```
O(n)
```

Space

```
O(1)
```

---

## Key Interview Insights

This is one of the most frequently asked greedy + two pointer interview questions.

Interviewers usually ask:

> Why is moving the shorter wall always correct?

Be prepared to justify it using the area formula.

---

## Common Pitfalls

- Moving the taller pointer first.
- Forgetting to update maximum before moving pointers.
- Thinking both pointers should move together.

---

## Follow-ups

- Maximum area after removing one wall.
- Circular container variant.
- Weighted heights variant.

---

# End of Part 1

**Covered Patterns**

- Opposite-End Pointers
- Same-Direction Pointers
- Stable Partition
- Greedy + Two Pointers

**Covered Problems**

1. Valid Palindrome
2. Two Sum II
3. Remove Duplicates from Sorted Array
4. Move Zeroes
5. Container With Most Water

Part 2 continues with five additional interview-standard Two Pointer problems, including **3Sum, Sort Colors, Squares of a Sorted Array, Boats to Save People, and Next Permutation**.

# Problem 6 — 3Sum

**LeetCode:** 15

**Difficulty:** Medium

**Companies**

- Meta
- Amazon
- Google
- Apple
- Microsoft
- Bloomberg
- Uber

---

## Problem Statement

Given an integer array `nums`, return **all unique triplets** `[a, b, c]` such that:

```
a + b + c = 0
```

The solution must not contain duplicate triplets.

---

## Why Two Pointers?

A brute-force solution checks every triplet.

```
O(n³)
```

Instead,

1. Sort the array.
2. Fix one element.
3. Solve the remaining Two Sum using opposite pointers.

Overall complexity becomes

```
O(n²)
```

---

## Pointer Visualization

```
Sorted

-4 -1 -1 0 1 2

Fix i

-4 -1 -1 0 1 2
 ^
 L             R

Need sum = 4

↓

Increase L

↓

Decrease R

↓

Found Triplet
```

---

## Approach

For every index `i`:

- Fix `nums[i]`
- Find two numbers whose sum equals `-nums[i]`
- Skip duplicates for:
  - Fixed element
  - Left pointer
  - Right pointer

---

## Java Solution

```java
class Solution {

    public List<List<Integer>> threeSum(int[] nums) {

        Arrays.sort(nums);

        List<List<Integer>> ans = new ArrayList<>();

        for (int i = 0; i < nums.length - 2; i++) {

            if (i > 0 && nums[i] == nums[i - 1])
                continue;

            int left = i + 1;
            int right = nums.length - 1;

            while (left < right) {

                int sum = nums[i] + nums[left] + nums[right];

                if (sum == 0) {

                    ans.add(Arrays.asList(
                            nums[i],
                            nums[left],
                            nums[right]
                    ));

                    left++;
                    right--;

                    while (left < right &&
                           nums[left] == nums[left - 1])
                        left++;

                    while (left < right &&
                           nums[right] == nums[right + 1])
                        right--;

                } else if (sum < 0) {

                    left++;

                } else {

                    right--;
                }
            }
        }

        return ans;
    }
}
```

---

## Complexity

Time

```
O(n²)
```

Space

```
O(1)

(excluding answer list)
```

---

## Key Interview Insights

The biggest source of bugs is duplicate handling.

Duplicates must be skipped:

- Before fixing `i`
- After moving left
- After moving right

---

## Common Pitfalls

❌ Forgetting to sort.

❌ Returning duplicate triplets.

❌ Using HashSet unnecessarily.

---

## Follow-ups

- 3Sum Closest
- 3Sum Smaller
- 3Sum With Multiplicity

---

# Problem 7 — Squares of a Sorted Array

**LeetCode:** 977

**Difficulty:** Easy

**Companies**

- Google
- Amazon
- Apple
- Microsoft

---

## Problem Statement

Return the squares of every element in sorted order.

Example

```
[-7,-3,2,3,11]

↓

[4,9,9,49,121]
```

---

## Why Two Pointers?

Negative numbers become positive after squaring.

Largest square always comes from one of the two ends.

---

## Visualization

```
-7 -3 2 3 11

L            R

Compare

49 vs 121

Place 121

↓

Move Right

Compare again
```

---

## Approach

Create answer array.

Fill from end.

Always choose larger square.

---

## Java Solution

```java
class Solution {

    public int[] sortedSquares(int[] nums) {

        int n = nums.length;

        int[] ans = new int[n];

        int left = 0;
        int right = n - 1;
        int index = n - 1;

        while (left <= right) {

            int leftSquare = nums[left] * nums[left];
            int rightSquare = nums[right] * nums[right];

            if (leftSquare > rightSquare) {

                ans[index--] = leftSquare;
                left++;

            } else {

                ans[index--] = rightSquare;
                right--;
            }
        }

        return ans;
    }
}
```

---

## Complexity

Time

```
O(n)
```

Space

```
O(n)
```

---

## Key Interview Insights

Sorting after squaring would take

```
O(n log n)
```

Two pointers reduce it to linear time.

---

## Follow-ups

- Cube instead of square.
- Absolute value sorting.

---

# Problem 8 — Sort Colors

**LeetCode:** 75

**Difficulty:** Medium

**Companies**

- Meta
- Google
- Amazon
- Microsoft
- Apple

---

## Problem Statement

Sort an array containing only

```
0
1
2
```

without using built-in sorting.

---

## Why Two Pointers?

Actually,

this is a **three-pointer** extension of partitioning.

Pointers:

```
low

mid

high
```

---

## Visualization

```
2 0 2 1 1 0

L
M
          H

↓

Swap with High

↓

Swap with Low

↓

Continue
```

---

## Dutch National Flag

```
0-zone

1-zone

Unknown

2-zone

------------------

0 0 |1 1| ? ? ? |2 2
```

---

## Java Solution

```java
class Solution {

    public void sortColors(int[] nums) {

        int low = 0;
        int mid = 0;
        int high = nums.length - 1;

        while (mid <= high) {

            if (nums[mid] == 0) {

                swap(nums, low, mid);
                low++;
                mid++;

            } else if (nums[mid] == 1) {

                mid++;

            } else {

                swap(nums, mid, high);
                high--;
            }
        }
    }

    private void swap(int[] nums, int i, int j) {

        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

---

## Complexity

Time

```
O(n)
```

Space

```
O(1)
```

---

## Key Interview Insights

Do **not** increment `mid` after swapping with `high`.

Reason:

The incoming value has not been processed yet.

This is the classic interview trap.

---

## Follow-ups

- Four colors.
- Rainbow sort.
- Generalized Dutch National Flag.

---

# Problem 9 — Boats to Save People

**LeetCode:** 881

**Difficulty:** Medium

**Companies**

- Amazon
- Google
- Meta
- Apple

---

## Problem Statement

Each boat carries at most

- two people
- total weight ≤ limit

Find minimum boats needed.

---

## Why Two Pointers?

After sorting,

Try pairing:

```
lightest

+

heaviest
```

If they fit,

great.

Otherwise,

heaviest must go alone.

---

## Visualization

```
1 2 2 3

L       R

Limit = 3

1+3>3

↓

Boat

3

↓

2+2>3

↓

Boat

2

↓

Boat

2

↓

Boat

1
```

---

## Greedy Proof

If the lightest person cannot fit with the heaviest,

no one can.

Therefore,

the heaviest must occupy a separate boat.

---

## Java Solution

```java
class Solution {

    public int numRescueBoats(int[] people, int limit) {

        Arrays.sort(people);

        int left = 0;
        int right = people.length - 1;

        int boats = 0;

        while (left <= right) {

            if (people[left] + people[right] <= limit) {
                left++;
            }

            right--;
            boats++;
        }

        return boats;
    }
}
```

---

## Complexity

Time

```
O(n log n)

(sort)
```

Space

```
O(1)
```

---

## Key Interview Insights

Sorting dominates complexity.

Greedy pairing is optimal.

---

## Follow-ups

- Boat capacity = 3.
- Different weight limits.
- Maximize remaining capacity.

---

# Problem 10 — Next Permutation

**LeetCode:** 31

**Difficulty:** Medium

**Companies**

- Google
- Meta
- Microsoft
- Amazon
- Apple

---

## Problem Statement

Rearrange numbers into the next lexicographically greater permutation.

If impossible,

return smallest permutation.

---

## Why Two Pointers?

After finding the pivot,

use pointers to

- locate successor
- reverse suffix

Both operations are pointer-based.

---

## Visualization

```
1 2 7 6 5 4

      Pivot

↓

Find successor

↓

Swap

↓

Reverse suffix

↓

1 4 2 5 6 7
```

---

## Algorithm

### Step 1

Find first decreasing index.

```
...3 5 4

Pivot = 3
```

---

### Step 2

Find smallest larger element.

---

### Step 3

Swap.

---

### Step 4

Reverse remaining suffix.

---

## Java Solution

```java
class Solution {

    public void nextPermutation(int[] nums) {

        int i = nums.length - 2;

        while (i >= 0 &&
               nums[i] >= nums[i + 1]) {
            i--;
        }

        if (i >= 0) {

            int j = nums.length - 1;

            while (nums[j] <= nums[i]) {
                j--;
            }

            swap(nums, i, j);
        }

        reverse(nums, i + 1, nums.length - 1);
    }

    private void reverse(int[] nums,
                         int left,
                         int right) {

        while (left < right) {
            swap(nums, left++, right--);
        }
    }

    private void swap(int[] nums,
                      int i,
                      int j) {

        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```

---

## Complexity

Time

```
O(n)
```

Space

```
O(1)
```

---

## Key Interview Insights

Most candidates memorize this algorithm but cannot explain **why reversing the suffix works**.

Reason:

The suffix is already in decreasing order.

Reversing produces the smallest possible suffix, giving the immediate next permutation.

---

## Common Pitfalls

- Reversing before swapping.
- Choosing the wrong successor.
- Sorting the suffix instead of reversing.

---

## Follow-ups

- Previous permutation.
- kth permutation.
- Next greater integer.

---

# End of Part 2

## Problems Covered

6. 3Sum
7. Squares of a Sorted Array
8. Sort Colors
9. Boats to Save People
10. Next Permutation

Part 3 covers:

- Trapping Rain Water
- 4Sum
- Valid Triangle Number
- Merge Sorted Array
- Backspace String Compare
- LLM-Proof Interview Questions
- Complete Two Pointer Cheat Sheet
- FAANG Quick Reference Table


# Problem 11 — Trapping Rain Water

**LeetCode:** 42

**Difficulty:** Hard

**Companies**

- Google
- Amazon
- Meta
- Microsoft
- Apple
- Bloomberg
- Uber

---

## Problem Statement

Given an elevation map represented by an integer array, compute how much water can be trapped after raining.

Example

```
Input

0 1 0 2 1 0 1 3 2 1 2 1

Output

6
```

---

## Why Two Pointers?

A brute-force solution computes the tallest bar on the left and right for every index.

```
O(n²)
```

A better solution precomputes prefix and suffix maximum arrays.

```
O(n)
Space: O(n)
```

The optimal solution uses **two pointers** and maintains only:

- Left maximum
- Right maximum

```
Time : O(n)
Space: O(1)
```

---

## Visualization

```
0 1 0 2 1 0 1 3 2 1 2 1

L                     R

LeftMax = 0
RightMax = 1

↓

Move smaller side

↓

Update max

↓

Calculate trapped water
```

---

## Key Observation

Water trapped at an index is

```
min(leftMax, rightMax) - height[i]
```

If

```
leftMax < rightMax
```

then the left side completely determines the answer.

We do not need to know the exact future right maximum.

---

## Algorithm

```
left = 0

right = n-1

leftMax = 0

rightMax = 0

while(left < right)

    if(height[left] < height[right])

        process left

    else

        process right
```

---

## Java Solution

```java
class Solution {

    public int trap(int[] height) {

        int left = 0;
        int right = height.length - 1;

        int leftMax = 0;
        int rightMax = 0;

        int water = 0;

        while (left < right) {

            if (height[left] < height[right]) {

                if (height[left] >= leftMax) {
                    leftMax = height[left];
                } else {
                    water += leftMax - height[left];
                }

                left++;

            } else {

                if (height[right] >= rightMax) {
                    rightMax = height[right];
                } else {
                    water += rightMax - height[right];
                }

                right--;
            }
        }

        return water;
    }
}
```

---

## Complexity

Time

```
O(n)
```

Space

```
O(1)
```

---

## Interview Insights

Interviewers frequently ask

> Why is moving the smaller pointer always safe?

Answer:

The smaller maximum is the limiting wall.

Even if the opposite side increases later,

the smaller maximum still determines trapped water.

---

## Common Mistakes

- Using current height instead of maximum height.
- Updating water before updating max.
- Forgetting equal-height case.

---

## Follow-ups

- Trapping Rain Water II
- Maximum Water Between Buildings

---

# Problem 12 — 4Sum

**LeetCode:** 18

**Difficulty:** Medium

**Companies**

- Amazon
- Google
- Meta
- Microsoft
- Apple

---

## Problem Statement

Return all unique quadruplets whose sum equals the target.

---

## Why Two Pointers?

Exactly like 3Sum.

Instead of fixing one number,

fix two numbers,

then solve the remaining Two Sum.

```
O(n⁴)

↓

O(n³)
```

---

## Visualization

```
Sorted Array

-2 -1 0 0 1 2

^

Fix i

    ^

Fix j

        L     R

↓

Move pointers
```

---

## Java Solution

```java
class Solution {

    public List<List<Integer>> fourSum(int[] nums, int target) {

        Arrays.sort(nums);

        List<List<Integer>> ans = new ArrayList<>();

        int n = nums.length;

        for (int i = 0; i < n - 3; i++) {

            if (i > 0 && nums[i] == nums[i - 1])
                continue;

            for (int j = i + 1; j < n - 2; j++) {

                if (j > i + 1 && nums[j] == nums[j - 1])
                    continue;

                int left = j + 1;
                int right = n - 1;

                while (left < right) {

                    long sum = (long) nums[i]
                             + nums[j]
                             + nums[left]
                             + nums[right];

                    if (sum == target) {

                        ans.add(Arrays.asList(
                                nums[i],
                                nums[j],
                                nums[left],
                                nums[right]
                        ));

                        left++;
                        right--;

                        while (left < right &&
                               nums[left] == nums[left - 1])
                            left++;

                        while (left < right &&
                               nums[right] == nums[right + 1])
                            right--;

                    } else if (sum < target) {

                        left++;

                    } else {

                        right--;
                    }
                }
            }
        }

        return ans;
    }
}
```

---

## Complexity

Time

```
O(n³)
```

Space

```
O(1)

(excluding answer)
```

---

## Key Interview Insights

Use

```java
long
```

Integer overflow is a common interview trap.

---

## Follow-ups

- k-Sum
- Generic recursive solution

---

# Problem 13 — Valid Triangle Number

**LeetCode:** 611

**Difficulty:** Medium

**Companies**

- Amazon
- Google
- Meta

---

## Problem Statement

Count the number of triplets that can form a valid triangle.

Triangle condition

```
a + b > c
```

---

## Why Two Pointers?

Sort the array.

Fix the largest side.

Find valid pairs using opposite pointers.

---

## Visualization

```
2 2 3 4

        Fix

2 2 3 4
L   R

↓

If

L+R>largest

Everything between

L...R

also works.
```

---

## Java Solution

```java
class Solution {

    public int triangleNumber(int[] nums) {

        Arrays.sort(nums);

        int count = 0;

        for (int i = nums.length - 1; i >= 2; i--) {

            int left = 0;
            int right = i - 1;

            while (left < right) {

                if (nums[left] + nums[right] > nums[i]) {

                    count += right - left;
                    right--;

                } else {

                    left++;
                }
            }
        }

        return count;
    }
}
```

---

## Complexity

Time

```
O(n²)
```

Space

```
O(1)
```

---

## Interview Insights

The trick is

```
count += right-left
```

not just

```
count++
```

Many candidates miss this optimization.

---

## Follow-ups

- Count acute triangles.
- Largest triangle perimeter.

---

# Problem 14 — Merge Sorted Array

**LeetCode:** 88

**Difficulty:** Easy

**Companies**

- Microsoft
- Amazon
- Google
- Apple
- Meta

---

## Problem Statement

Merge two sorted arrays into the first array in-place.

---

## Why Two Pointers?

Start filling from the back.

Otherwise,

existing values would be overwritten.

---

## Visualization

```
nums1

1 2 3 _ _ _

nums2

2 5 6

↓

Compare

3

6

↓

Write 6

↓

Continue backwards
```

---

## Java Solution

```java
class Solution {

    public void merge(int[] nums1,
                      int m,
                      int[] nums2,
                      int n) {

        int i = m - 1;
        int j = n - 1;
        int k = m + n - 1;

        while (i >= 0 && j >= 0) {

            if (nums1[i] > nums2[j]) {

                nums1[k--] = nums1[i--];

            } else {

                nums1[k--] = nums2[j--];
            }
        }

        while (j >= 0) {

            nums1[k--] = nums2[j--];
        }
    }
}
```

---

## Complexity

Time

```
O(m+n)
```

Space

```
O(1)
```

---

## Key Interview Insights

Always merge from the end.

Forward merging destroys unprocessed values.

---

## Follow-ups

- Merge k arrays.
- External merge.

---

# Problem 15 — Backspace String Compare

**LeetCode:** 844

**Difficulty:** Easy

**Companies**

- Google
- Amazon
- Meta
- Apple

---

## Problem Statement

`#` means backspace.

Determine whether two strings become equal.

---

## Why Two Pointers?

Instead of constructing final strings,

scan backwards.

Skip deleted characters.

---

## Visualization

```
ab#c

^

↓

Skip b

↓

Compare

a

c
```

---

## Algorithm

Maintain

```
skipS

skipT
```

Move backwards.

Ignore deleted characters.

Compare remaining characters.

---

## Java Solution

```java
class Solution {

    public boolean backspaceCompare(String s, String t) {

        int i = s.length() - 1;
        int j = t.length() - 1;

        int skipS = 0;
        int skipT = 0;

        while (i >= 0 || j >= 0) {

            while (i >= 0) {

                if (s.charAt(i) == '#') {
                    skipS++;
                    i--;
                } else if (skipS > 0) {
                    skipS--;
                    i--;
                } else {
                    break;
                }
            }

            while (j >= 0) {

                if (t.charAt(j) == '#') {
                    skipT++;
                    j--;
                } else if (skipT > 0) {
                    skipT--;
                    j--;
                } else {
                    break;
                }
            }

            if (i >= 0 && j >= 0 &&
                s.charAt(i) != t.charAt(j))
                return false;

            if ((i >= 0) != (j >= 0))
                return false;

            i--;
            j--;
        }

        return true;
    }
}
```

---

## Complexity

Time

```
O(n)
```

Space

```
O(1)
```

---

## Key Interview Insights

Avoid building new strings.

Reverse traversal is significantly cleaner.

---

# LLM-Proof Interview Questions

These questions test reasoning rather than memorization.

---

## 1. Prove Why Moving the Shorter Pointer Works

Applicable to:

- Container With Most Water
- Trapping Rain Water

Interviewers expect a mathematical proof using

```
Area = min(height1,height2) × width
```

rather than intuition.

---

## 2. Design a Generic k-Sum Algorithm

Instead of separate

- 2Sum
- 3Sum
- 4Sum

write a recursive

```
kSum()

↓

Base Case

Two Pointers

↓

Recursive Reduction
```

Expected Complexity

```
O(n^(k-1))
```

---

## 3. Streaming Two-Pointer Problem

Given

```
10⁹

numbers
```

stored on disk,

design a two-pointer algorithm when the entire array cannot fit into memory.

Discussion points

- External memory
- Buffered reading
- Sequential scans
- Merge strategy

---

# Two Pointers Cheat Sheet

| Pattern | When to Use | Example Problems |
|----------|-------------|------------------|
| Opposite Ends | Sorted array, palindrome | Two Sum II, 3Sum, 4Sum |
| Same Direction | Remove/compact elements | Move Zeroes, Remove Duplicates |
| Fast & Slow | Linked lists, cycle detection | Linked List Cycle, Middle Node |
| Sliding Window | Contiguous subarrays | Minimum Window, Longest Substring |
| Partition | Rearrangement | Sort Colors, Dutch National Flag |

---

# FAANG Quick Reference Table

| Problem | Difficulty | Companies | Technique |
|----------|------------|-----------|-----------|
| Valid Palindrome | Easy | Meta, Amazon, Google | Opposite Pointers |
| Two Sum II | Easy | Amazon, Google | Opposite Pointers |
| Remove Duplicates from Sorted Array | Easy | Google, Meta | Same Direction |
| Move Zeroes | Easy | Meta, Amazon | Stable Partition |
| Container With Most Water | Medium | Amazon, Google, Meta | Opposite + Greedy |
| 3Sum | Medium | Meta, Amazon | Sorting + Opposite Pointers |
| Squares of a Sorted Array | Easy | Google, Amazon | Opposite Pointers |
| Sort Colors | Medium | Meta, Google | Three-Pointer Partition |
| Boats to Save People | Medium | Amazon, Google | Greedy + Opposite Pointers |
| Next Permutation | Medium | Google, Meta | Pivot + Reverse |
| Trapping Rain Water | Hard | Google, Amazon | Opposite Pointers |
| 4Sum | Medium | Amazon, Google | Nested + Two Pointers |
| Valid Triangle Number | Medium | Amazon | Opposite Pointers |
| Merge Sorted Array | Easy | Microsoft, Amazon | Backward Two Pointers |
| Backspace String Compare | Easy | Google, Meta | Reverse Two Pointers |

---

# Final Takeaways

## Recognize the Pattern

Ask yourself:

- Is the array sorted?
- Am I comparing two ends?
- Am I compacting elements?
- Am I maintaining a valid window?
- Can I eliminate unnecessary comparisons using pointer movement?

## Common Interview Mistakes

- Forgetting to sort before applying opposite pointers.
- Mishandling duplicate values in 3Sum/4Sum.
- Moving the wrong pointer after comparison.
- Updating pointers before processing the current state.
- Using extra space when an in-place solution is expected.
- Missing integer overflow (`long`) in sum problems.

## Interview Strategy

When you identify a potential two-pointer problem:

1. Determine whether the data is sorted (or can be sorted).
2. Choose the appropriate pointer pattern.
3. Clearly justify **why each pointer moves**.
4. State the time and space complexity before writing code.
5. Walk through one example to validate edge cases such as duplicates, empty arrays, or all identical values.

---

# End of Two Pointers Interview Guide
