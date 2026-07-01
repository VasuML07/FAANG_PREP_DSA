# Binary Search - Complete FAANG Interview Preparation Guide (Java)

> **Language:** Java  
> **Problems Covered:** 15 (Part 1 contains Problems 1–4)  
> **Focus:** Interview-oriented Binary Search patterns used in FAANG interviews.

---

# Why Binary Search Matters

Binary Search is one of the highest ROI topics in coding interviews.

Interviewers rarely ask "implement binary search."

Instead they ask problems where you must recognize that:

- Search space is sorted.
- Answer itself can be binary searched.
- Rotated order still preserves monotonicity.
- Duplicates require boundary handling.
- Unknown search space must first be bounded.

Mastering these patterns allows solving dozens of seemingly unrelated interview problems.

---

# Problem 1 — Binary Search (LeetCode 704)

**Difficulty:** Easy

**Pattern**

Classic Binary Search

**Companies**

- Google
- Amazon
- Microsoft
- Apple
- Meta
- Adobe

---

## Problem Statement

Given a sorted integer array `nums` and an integer `target`, return the index of target.

Return `-1` if it doesn't exist.

Example

```
Input:

nums = [-1,0,3,5,9,12]
target = 9

Output:

4
```

---

# Interview Recognition

Whenever you hear

- sorted array
- O(log n)
- search element

Immediately think Binary Search.

---

# Important Tricks

### 1. Never compute mid like this

```java
int mid = (left + right) / 2;
```

Because it may overflow.

Always write

```java
int mid = left + (right - left) / 2;
```

---

### 2. Keep inclusive boundaries

```
left <= right
```

Not

```
left < right
```

Otherwise last element may never be checked.

---

### 3. Eliminate half every iteration

```
nums[mid] < target

Discard left half

left = mid + 1
```

```
nums[mid] > target

Discard right half

right = mid - 1
```

---

# Dry Run

```
nums

-1 0 3 5 9 12

left=0 right=5

mid=2

nums[2]=3

3<9

Move right

left=3

-------------------

left=3 right=5

mid=4

nums[4]=9

Found
```

---

# Java Solution

```java
class Solution {

    public int search(int[] nums, int target) {

        int left = 0;
        int right = nums.length - 1;

        while (left <= right) {

            int mid = left + (right - left) / 2;

            if (nums[mid] == target)
                return mid;

            if (nums[mid] < target)
                left = mid + 1;
            else
                right = mid - 1;
        }

        return -1;
    }
}
```

---

# Complexity

| Metric | Value |
|---------|-------|
| Time | O(log n) |
| Space | O(1) |

---

# Common Mistakes

### Infinite Loop

Wrong

```java
left = mid;
```

Correct

```java
left = mid + 1;
```

---

### Wrong Loop Condition

Wrong

```java
while(left < right)
```

Correct

```java
while(left <= right)
```

---

### Overflow

Wrong

```java
(left + right)/2
```

Correct

```java
left + (right-left)/2
```

---

# Why This Pattern Matters

Every advanced Binary Search question derives from this template.

Never memorize advanced problems before mastering this one.

---

# Problem 2 — Search Insert Position (LeetCode 35)

**Difficulty:** Easy

**Pattern**

Lower Bound Binary Search

**Companies**

- Google
- Amazon
- Apple
- Meta
- Bloomberg

---

## Problem Statement

Return the index where target exists.

If absent,

return where it should be inserted while keeping array sorted.

Example

```
nums

1 3 5 6

target=2

Answer=1
```

---

# Recognition

If interviewer says

> "return insertion position"

they're asking for

> First element ≥ target

This is called

```
Lower Bound
```

---

# Key Insight

Unlike normal Binary Search,

we don't stop after failing.

Instead we shrink toward the answer.

Eventually

```
left

becomes insertion point.
```

---

# Visualization

```
1 3 5 6

target=2

L        R

mid=1

3>=2

Move Left

-----------

L R

mid=0

1<2

Move Right

-----------

left=1

Finished

Answer=1
```

---

# Java Solution

```java
class Solution {

    public int searchInsert(int[] nums, int target) {

        int left = 0;
        int right = nums.length - 1;

        while (left <= right) {

            int mid = left + (right - left) / 2;

            if (nums[mid] < target)
                left = mid + 1;
            else
                right = mid - 1;
        }

        return left;
    }
}
```

---

# Complexity

| Metric | Value |
|---------|-------|
| Time | O(log n) |
| Space | O(1) |

---

# Interview Trick

At loop end

```
right < left
```

Always.

```
right

last smaller

left

first greater/equal
```

Therefore

```
left

is insertion point.
```

---

# Pattern Family

Lower Bound

Used in

- First occurrence
- Search Range
- LIS
- Patience Sorting
- Ordered Sets

---

# Problem 3 — First Bad Version (LeetCode 278)

**Difficulty:** Easy

**Pattern**

Binary Search on Predicate

**Companies**

- Google
- Amazon
- Microsoft
- Uber

---

## Problem Statement

You have versions

```
1...

n
```

API

```java
boolean isBadVersion(int version)
```

Once a version becomes bad,

every version after it is also bad.

Return the first bad version.

---

# Recognition

Notice

```
False False False True True True True
```

This is a monotonic predicate.

Binary Search works on monotonic predicates.

---

# Visualization

```
1

2

3

4

5

6

7

Good Good Good Bad Bad Bad Bad

Need

first True
```

---

# Trick

When

```
mid

is bad
```

Don't discard it.

Because

it might itself be first bad.

So

```
right = mid
```

not

```
mid-1
```

---

# Java Solution

```java
public class Solution extends VersionControl {

    public int firstBadVersion(int n) {

        int left = 1;
        int right = n;

        while (left < right) {

            int mid = left + (right - left) / 2;

            if (isBadVersion(mid))
                right = mid;
            else
                left = mid + 1;
        }

        return left;
    }
}
```

---

# Complexity

| Metric | Value |
|---------|-------|
| Time | O(log n) |
| Space | O(1) |

---

# Why This Pattern Matters

Binary Search doesn't require sorted numbers.

It requires

```
False False False True True
```

or

```
True True False False
```

Any monotonic property can be binary searched.

This idea powers

- Minimum speed
- Capacity
- Koko Eating Bananas
- Aggressive Cows
- Allocate Books

---

# Problem 4 — Find First and Last Position of Element in Sorted Array (LeetCode 34)

**Difficulty:** Medium

**Pattern**

Lower Bound + Upper Bound

**Companies**

- Google
- Amazon
- Apple
- Meta
- LinkedIn

---

## Problem Statement

Return

```
[firstOccurrence,
 lastOccurrence]
```

If target absent

return

```
[-1,-1]
```

Example

```
nums

5 7 7 8 8 10

target=8

Output

[3,4]
```

---

# Recognition

Duplicates + Sorted Array

Almost always means

Boundary Binary Search.

Never use linear scan.

---

# Trick

Run Binary Search twice.

First search

```
first >= target
```

Second search

```
first > target
```

Then

```
last = upperBound - 1
```

---

# Visualization

```
5 7 7 8 8 10

Find Lower Bound

^

3

Find Upper Bound

^

5

Answer

3

5-1=4
```

---

# Helper Function

Lower Bound

```
first >= target
```

Upper Bound

```
first > target
```

Only comparison changes.

---

# Java Solution

```java
class Solution {

    public int[] searchRange(int[] nums, int target) {

        int first = lowerBound(nums, target);

        if (first == nums.length || nums[first] != target)
            return new int[]{-1, -1};

        int last = lowerBound(nums, target + 1) - 1;

        return new int[]{first, last};
    }

    private int lowerBound(int[] nums, int target) {

        int left = 0;
        int right = nums.length;

        while (left < right) {

            int mid = left + (right - left) / 2;

            if (nums[mid] < target)
                left = mid + 1;
            else
                right = mid;
        }

        return left;
    }
}
```

---

# Complexity

| Metric | Value |
|---------|-------|
| Time | O(log n) |
| Space | O(1) |

---

# Edge Cases

### Empty Array

```
[]
```

Return

```
[-1,-1]
```

---

### Single Element

```
[5]
```

Works automatically.

---

### Target Missing

```
1 2 4 5

target=3
```

Lower Bound correctly identifies absence.

---

### All Same

```
8 8 8 8 8
```

First Binary Search

returns 0

Second

returns 5

Answer

```
0

4
```

---

# Pattern Family

This boundary-search template appears in:

- Count occurrences in sorted array
- Lower Bound
- Upper Bound
- STL lower_bound()
- STL upper_bound()
- Database indexing
- B+ Tree leaf searches

---

# End of Part 1

Covered patterns:

- Classic Binary Search
- Lower Bound
- Binary Search on Predicate
- First & Last Occurrence using Boundary Search

**Next (Part 2):**

5. Search in Rotated Sorted Array (33)
6. Search in Rotated Sorted Array II (81)
7. Find Minimum in Rotated Sorted Array (153)
8. Find Peak Element (162)

---

# Problem 5 — Search in Rotated Sorted Array (LeetCode 33)

**Difficulty:** Medium

**Pattern**

Binary Search on Rotated Array

**Companies**

- Google
- Amazon
- Meta
- Microsoft
- Apple
- Uber

---

## Problem Statement

An array originally sorted in ascending order is rotated at an unknown pivot.

Search for the target in **O(log n)** time.

Example

```
Input

nums = [4,5,6,7,0,1,2]
target = 0

Output

4
```

---

# Interview Recognition

Keywords that should immediately trigger this pattern:

- Sorted but rotated
- Distinct elements
- O(log n)

A rotated array always contains **one sorted half**.

Your task each iteration is to identify the sorted half and determine whether the target belongs there.

---

# Core Trick

At every iteration,

one of these must be true:

```
Left Half Sorted

or

Right Half Sorted
```

Determine which half is sorted.

Then decide whether the target lies inside that half.

---

# Visualization

```
4 5 6 7 0 1 2

L       M     R

mid = 7

Left Half

4 5 6 7

is sorted

Target = 0

Not inside

Discard left half
```

---

Another iteration

```
0 1 2

L M R

Right half sorted

Target found.
```

---

# Decision Table

| Condition | Action |
|-----------|--------|
| Left half sorted & target inside | Move right |
| Left half sorted & target outside | Move left |
| Right half sorted & target inside | Move left |
| Right half sorted & target outside | Move right |

---

# Java Solution

```java
class Solution {

    public int search(int[] nums, int target) {

        int left = 0;
        int right = nums.length - 1;

        while (left <= right) {

            int mid = left + (right - left) / 2;

            if (nums[mid] == target)
                return mid;

            // Left half sorted
            if (nums[left] <= nums[mid]) {

                if (target >= nums[left] && target < nums[mid])
                    right = mid - 1;
                else
                    left = mid + 1;
            }

            // Right half sorted
            else {

                if (target > nums[mid] && target <= nums[right])
                    left = mid + 1;
                else
                    right = mid - 1;
            }
        }

        return -1;
    }
}
```

---

# Complexity

| Metric | Value |
|--------|------|
| Time | O(log n) |
| Space | O(1) |

---

# Common Mistakes

### Comparing with right first

Always determine

```
Which side is sorted?
```

before checking target.

---

### Forgetting equality

Wrong

```java
nums[left] < nums[mid]
```

Correct

```java
nums[left] <= nums[mid]
```

Single-element partitions require equality.

---

# Why This Pattern Matters

This teaches an important interview principle:

> Binary Search works whenever you can eliminate half of the search space—even if the entire array isn't globally sorted.

---

# Problem 6 — Search in Rotated Sorted Array II (LeetCode 81)

**Difficulty:** Medium

**Pattern**

Rotated Binary Search with Duplicates

**Companies**

- Amazon
- Google
- Microsoft
- Bloomberg

---

## Problem Statement

Same as Problem 33, but duplicate values are allowed.

Return true if target exists.

---

Example

```
2 5 6 0 0 1 2

target=0

true
```

---

# What's Different?

Duplicates destroy the ability to determine which side is sorted.

Example

```
1 1 1 1 0 1

L M R

nums[left]
==
nums[mid]
```

Which half is sorted?

Impossible to know.

---

# Trick

Whenever

```
nums[left] == nums[mid]
```

simply shrink the search space.

```
left++
```

Likewise,

if

```
nums[right] == nums[mid]
```

```
right--
```

---

# Visualization

```
1 1 1 1 0 1

L M     R

Cannot decide.

Discard duplicate.

1 1 1 0 1
```

Eventually the ambiguity disappears.

---

# Java Solution

```java
class Solution {

    public boolean search(int[] nums, int target) {

        int left = 0;
        int right = nums.length - 1;

        while (left <= right) {

            int mid = left + (right - left) / 2;

            if (nums[mid] == target)
                return true;

            if (nums[left] == nums[mid] && nums[mid] == nums[right]) {
                left++;
                right--;
            }

            else if (nums[left] <= nums[mid]) {

                if (target >= nums[left] && target < nums[mid])
                    right = mid - 1;
                else
                    left = mid + 1;
            }

            else {

                if (target > nums[mid] && target <= nums[right])
                    left = mid + 1;
                else
                    right = mid - 1;
            }
        }

        return false;
    }
}
```

---

# Complexity

| Metric | Value |
|--------|------|
| Average | O(log n) |
| Worst Case | O(n) |
| Space | O(1) |

---

# Interview Follow-up

**Why O(n)?**

Consider

```
1 1 1 1 1 1 1
```

Every iteration removes only one element.

---

# Pattern Recognition

Whenever duplicates prevent identifying the sorted half,

expect to shrink boundaries until ordering becomes visible.

---

# Problem 7 — Find Minimum in Rotated Sorted Array (LeetCode 153)

**Difficulty:** Medium

**Pattern**

Binary Search on Rotation Pivot

**Companies**

- Google
- Amazon
- Apple
- Meta
- Adobe

---

## Problem Statement

Find the minimum element in a rotated sorted array.

Distinct elements only.

Example

```
Input

3 4 5 1 2

Output

1
```

---

# Recognition

We don't need the pivot index.

We only need the smallest element.

---

# Observation

Compare

```
nums[mid]

with

nums[right]
```

If

```
nums[mid] > nums[right]
```

minimum lies to the right.

Otherwise

minimum lies at mid or left.

---

# Visualization

```
3 4 5 1 2

      M   R

5 > 2

Minimum right

----------------

1 2

M R

1 < 2

Minimum left including mid
```

---

# Java Solution

```java
class Solution {

    public int findMin(int[] nums) {

        int left = 0;
        int right = nums.length - 1;

        while (left < right) {

            int mid = left + (right - left) / 2;

            if (nums[mid] > nums[right])
                left = mid + 1;
            else
                right = mid;
        }

        return nums[left];
    }
}
```

---

# Complexity

| Metric | Value |
|--------|------|
| Time | O(log n) |
| Space | O(1) |

---

# Trick

Notice

```
right = mid
```

not

```
mid-1
```

because

mid itself may be the minimum.

---

# Related Problems

- Rotation Count
- Search Rotated Array
- Rotated Array with Duplicates
- Find Pivot Index

---

# Problem 8 — Find Peak Element (LeetCode 162)

**Difficulty:** Medium

**Pattern**

Binary Search on Local Property

**Companies**

- Google
- Amazon
- Meta
- Microsoft
- Apple

---

## Problem Statement

A peak element is greater than its neighbors.

Return any peak index.

Example

```
Input

1 2 3 1

Output

2
```

---

# Recognition

The array isn't sorted.

Yet Binary Search still works.

Why?

Because the slope provides a monotonic direction.

---

# Observation

Compare

```
nums[mid]

and

nums[mid+1]
```

If

```
nums[mid] < nums[mid+1]
```

Peak must exist on the right.

Otherwise

Peak exists on the left including mid.

---

# Visualization

```
1 2 3 4 7 8 5 2

        M

Increasing

Peak →

--------------------

1 3 5 7 6

      M

Decreasing

Peak ←
```

---

# Why?

An increasing slope must eventually stop increasing.

Where it stops,

a peak exists.

---

# Java Solution

```java
class Solution {

    public int findPeakElement(int[] nums) {

        int left = 0;
        int right = nums.length - 1;

        while (left < right) {

            int mid = left + (right - left) / 2;

            if (nums[mid] < nums[mid + 1])
                left = mid + 1;
            else
                right = mid;
        }

        return left;
    }
}
```

---

# Complexity

| Metric | Value |
|--------|------|
| Time | O(log n) |
| Space | O(1) |

---

# Common Mistakes

### Comparing both neighbors

Unnecessary.

Only compare

```
mid

and

mid+1
```

---

### Using

```java
left <= right
```

This may access

```
mid+1
```

out of bounds.

Use

```java
left < right
```

instead.

---

# Pattern Family

This "search by slope" technique appears in

- Mountain Array
- Bitonic Search
- Peak Index in Mountain Array
- Local Minimum
- Local Maximum
- Optimization problems

---

# End of Part 2

## Binary Search Patterns Covered So Far

| Problem | Pattern |
|----------|---------|
| 704 | Classic Binary Search |
| 35 | Lower Bound |
| 278 | Predicate Binary Search |
| 34 | Lower + Upper Bound |
| 33 | Rotated Array Search |
| 81 | Rotated Array with Duplicates |
| 153 | Rotation Pivot |
| 162 | Peak Search (Slope Binary Search) |

**Next (Part 3):**

9. Peak Index in a Mountain Array (852)  
10. Search in Mountain Array (1095)  
11. Koko Eating Bananas (875)  
12. Capacity To Ship Packages Within D Days (1011)


---

# Problem 9 — Peak Index in a Mountain Array (LeetCode 852)

**Difficulty:** Medium

**Pattern**

Binary Search on Bitonic / Mountain Array

**Companies**

- Google
- Amazon
- Apple
- Meta
- Microsoft

---

## Problem Statement

A mountain array satisfies:

- Strictly increasing
- Followed by strictly decreasing

Return the **peak index**.

Example

```
Input

[0,2,5,7,6,4,1]

Output

3
```

---

# Interview Recognition

Keywords:

- Mountain Array
- Bitonic Array
- Peak Index

Unlike LeetCode 162, this problem guarantees exactly one peak.

---

# Core Observation

Compare

```
nums[mid]

with

nums[mid + 1]
```

If

```
nums[mid] < nums[mid+1]
```

you're climbing.

Peak lies on the right.

Otherwise,

you're descending.

Peak lies on the left (including mid).

---

# Visualization

```
0 2 5 7 6 4 1

      M

5 < 7

Move Right

-----------------

7 6 4

M

Descending

Move Left
```

Eventually,

```
left == right

↓

Peak
```

---

# Java Solution

```java
class Solution {

    public int peakIndexInMountainArray(int[] arr) {

        int left = 0;
        int right = arr.length - 1;

        while (left < right) {

            int mid = left + (right - left) / 2;

            if (arr[mid] < arr[mid + 1])
                left = mid + 1;
            else
                right = mid;
        }

        return left;
    }
}
```

---

# Complexity

| Metric | Value |
|--------|------|
| Time | O(log n) |
| Space | O(1) |

---

# Trick

Never compare both neighbors.

Only

```
mid

vs

mid+1
```

is sufficient.

---

# Why This Pattern Matters

This pattern extends naturally into:

- Bitonic Search
- Mountain Array Search
- Peak Optimization
- Ternary-search style optimization discussions

---

# Problem 10 — Search in Mountain Array (LeetCode 1095)

**Difficulty:** Hard

**Pattern**

Peak Search + Two Binary Searches

**Companies**

- Google
- Meta
- Apple
- Amazon

---

## Problem Statement

Given a hidden Mountain Array API,

find the target.

You may only access elements through:

```java
MountainArray.get(index)
```

Return the smallest valid index.

---

# Recognition

The solution has three phases.

1. Find peak.
2. Binary search left half.
3. Binary search right half.

---

# Visualization

```
1 3 6 9 8 5 2

        Peak

Left

Ascending

Right

Descending
```

---

# Step 1

Find peak.

Same as Problem 852.

---

# Step 2

Binary Search

Ascending

```
1 3 6 9
```

---

# Step 3

If not found,

Binary Search

Descending

```
8 5 2
```

Comparison directions reverse.

---

# Java Solution

```java
class Solution {

    public int findInMountainArray(int target, MountainArray mountainArr) {

        int n = mountainArr.length();

        int left = 0;
        int right = n - 1;

        while (left < right) {

            int mid = left + (right - left) / 2;

            if (mountainArr.get(mid) < mountainArr.get(mid + 1))
                left = mid + 1;
            else
                right = mid;
        }

        int peak = left;

        int ans = ascendingSearch(mountainArr, target, 0, peak);

        if (ans != -1)
            return ans;

        return descendingSearch(mountainArr, target, peak + 1, n - 1);
    }

    private int ascendingSearch(MountainArray arr,
                                int target,
                                int left,
                                int right) {

        while (left <= right) {

            int mid = left + (right - left) / 2;

            int value = arr.get(mid);

            if (value == target)
                return mid;

            if (value < target)
                left = mid + 1;
            else
                right = mid - 1;
        }

        return -1;
    }

    private int descendingSearch(MountainArray arr,
                                 int target,
                                 int left,
                                 int right) {

        while (left <= right) {

            int mid = left + (right - left) / 2;

            int value = arr.get(mid);

            if (value == target)
                return mid;

            if (value > target)
                left = mid + 1;
            else
                right = mid - 1;
        }

        return -1;
    }
}
```

---

# Complexity

| Metric | Value |
|--------|------|
| Time | O(log n) |
| Space | O(1) |

---

# Interview Trick

Descending Binary Search simply reverses the comparison logic.

Many candidates unnecessarily reverse the array.

Never do that.

---

# Pattern Family

- Peak Search
- Two-phase Binary Search
- Hidden Array API
- Bitonic Array Search

---

# Problem 11 — Koko Eating Bananas (LeetCode 875)

**Difficulty:** Medium

**Pattern**

Binary Search on Answer

**Companies**

- Google
- Amazon
- Microsoft
- Meta
- Uber

---

## Problem Statement

Koko has several banana piles.

She eats

```
k
```

bananas per hour.

Return the **minimum eating speed** so she finishes within

```
h
```

hours.

---

Example

```
Piles

3 6 7 11

h=8

Answer=4
```

---

# Interview Recognition

You're not searching the array.

You're searching the answer.

---

# Search Space

Minimum speed

```
1
```

Maximum speed

```
max(piles)
```

Binary Search over this range.

---

# Predicate

```
Can finish within h hours?
```

If yes,

try smaller speed.

Otherwise,

increase speed.

---

# Visualization

```
Speed

1 ... maxPile

        M

Possible?

↓

Yes

Search Left

No

Search Right
```

---

# Feasibility Function

Hours needed

```
ceil(pile/speed)
```

Total

```
Σ ceil(...)
```

If

```
hours <= h

True
```

---

# Java Solution

```java
class Solution {

    public int minEatingSpeed(int[] piles, int h) {

        int left = 1;
        int right = 0;

        for (int pile : piles)
            right = Math.max(right, pile);

        while (left < right) {

            int mid = left + (right - left) / 2;

            if (canFinish(piles, h, mid))
                right = mid;
            else
                left = mid + 1;
        }

        return left;
    }

    private boolean canFinish(int[] piles,
                              int h,
                              int speed) {

        long hours = 0;

        for (int pile : piles)
            hours += (pile + speed - 1) / speed;

        return hours <= h;
    }
}
```

---

# Complexity

| Metric | Value |
|--------|------|
| Time | O(n log M) |
| Space | O(1) |

Where

```
M

=

Maximum pile
```

---

# Trick

Instead of

```java
Math.ceil((double)pile/speed)
```

use integer arithmetic

```java
(pile + speed - 1) / speed
```

Faster and avoids floating-point precision issues.

---

# Why This Pattern Matters

Binary Search on Answer appears repeatedly in interviews.

Typical keywords:

- Minimum
- Maximum
- Smallest feasible
- Largest possible
- Minimize
- Maximize

---

# Problem 12 — Capacity To Ship Packages Within D Days (LeetCode 1011)

**Difficulty:** Medium

**Pattern**

Binary Search on Answer

**Companies**

- Amazon
- Google
- Meta
- Microsoft
- Bloomberg

---

## Problem Statement

Packages must be shipped in order.

Find the minimum ship capacity needed to deliver all packages within

```
D
```

days.

---

Example

```
weights

1 2 3 4 5 6 7 8 9 10

D=5

Answer=15
```

---

# Recognition

Again,

the array isn't searched.

The answer is.

---

# Search Space

Minimum capacity

```
Maximum weight
```

Maximum capacity

```
Sum of all weights
```

---

# Predicate

Can all packages be shipped within

```
D

days?
```

If yes,

capacity may be smaller.

Otherwise,

increase it.

---

# Visualization

```
Capacity

maxWeight -------- totalWeight

             M

Possible?

↓

Yes

Search Left

No

Search Right
```

---

# Feasibility Function

Simulate loading packages.

Whenever capacity exceeds,

start a new day.

Count days.

---

# Java Solution

```java
class Solution {

    public int shipWithinDays(int[] weights, int days) {

        int left = 0;
        int right = 0;

        for (int weight : weights) {
            left = Math.max(left, weight);
            right += weight;
        }

        while (left < right) {

            int mid = left + (right - left) / 2;

            if (canShip(weights, days, mid))
                right = mid;
            else
                left = mid + 1;
        }

        return left;
    }

    private boolean canShip(int[] weights,
                            int days,
                            int capacity) {

        int requiredDays = 1;
        int currentLoad = 0;

        for (int weight : weights) {

            if (currentLoad + weight > capacity) {
                requiredDays++;
                currentLoad = 0;
            }

            currentLoad += weight;
        }

        return requiredDays <= days;
    }
}
```

---

# Complexity

| Metric | Value |
|--------|------|
| Time | O(n log S) |
| Space | O(1) |

Where

```
S

=

Sum of weights
```

---

# Common Mistakes

### Binary searching the weights array

Wrong.

We're searching

```
capacity
```

not package positions.

---

### Choosing wrong lower bound

Lower bound is

```
max(weights)
```

because every package must fit.

---

# Pattern Recognition

This exact framework also solves:

- Allocate Books
- Painter's Partition
- Split Array Largest Sum
- Magnetic Force Between Balls
- Minimum Limit of Balls in a Bag

---

# End of Part 3

## Patterns Covered So Far

| Problem | Pattern |
|----------|---------|
| 704 | Classic Binary Search |
| 35 | Lower Bound |
| 278 | Predicate Binary Search |
| 34 | Lower + Upper Bound |
| 33 | Rotated Array Search |
| 81 | Rotated Array with Duplicates |
| 153 | Find Rotation Pivot |
| 162 | Peak Search |
| 852 | Mountain Peak |
| 1095 | Peak + Two Binary Searches |
| 875 | Binary Search on Answer (Minimum Feasible) |
| 1011 | Binary Search on Answer (Capacity Search) |

**Next (Final Part):**

13. Split Array Largest Sum (410)  
14. Median of Two Sorted Arrays (4)  
15. Find K-th Smallest Pair Distance (719)  

Followed by:

- LLM-Proof Follow-up Questions
- Complete Binary Search Pattern Summary
- Final Interview Cheat Sheet

  ---

# Problem 13 — Split Array Largest Sum (LeetCode 410)

**Difficulty:** Hard

**Pattern**

Binary Search on Answer (Minimize the Maximum)

**Companies**

- Google
- Amazon
- Meta
- Microsoft
- Apple

---

## Problem Statement

Given an integer array `nums` and an integer `k`, split the array into exactly `k` non-empty contiguous subarrays.

Minimize the **largest subarray sum**.

Return that minimum possible value.

Example

```
Input

nums = [7,2,5,10,8]
k = 2

Output

18

Explanation

[7,2,5] [10,8]

Largest sum = 18
```

---

# Interview Recognition

Keywords that immediately suggest this pattern:

- Minimize the maximum
- Largest value
- Partition into groups
- Contiguous partitions

You are **not** searching the array.

You are searching the answer.

---

# Search Space

Smallest possible answer

```
max(nums)
```

Largest possible answer

```
sum(nums)
```

Binary search this range.

---

# Predicate

```
Can we split into
<= k
subarrays
if maximum allowed sum = mid ?
```

If YES

Try smaller maximum.

If NO

Increase maximum.

---

# Visualization

```
Answer

maxSum ---------------- totalSum

            mid

Can split?

YES

↓

Search Left

NO

↓

Search Right
```

---

# Java Solution

```java
class Solution {

    public int splitArray(int[] nums, int k) {

        int left = 0;
        int right = 0;

        for (int num : nums) {
            left = Math.max(left, num);
            right += num;
        }

        while (left < right) {

            int mid = left + (right - left) / 2;

            if (canSplit(nums, k, mid))
                right = mid;
            else
                left = mid + 1;
        }

        return left;
    }

    private boolean canSplit(int[] nums,
                             int k,
                             int limit) {

        int parts = 1;
        int sum = 0;

        for (int num : nums) {

            if (sum + num > limit) {
                parts++;
                sum = 0;
            }

            sum += num;
        }

        return parts <= k;
    }
}
```

---

# Complexity

| Metric | Value |
|--------|------|
| Time | O(n log(sum)) |
| Space | O(1) |

---

# Pattern Family

Exactly the same framework as

- Allocate Books
- Painter Partition
- Ship Packages
- Magnetic Force
- Aggressive Cows

---

# Problem 14 — Median of Two Sorted Arrays (LeetCode 4)

**Difficulty:** Hard

**Pattern**

Binary Search on Partition

**Companies**

- Google
- Meta
- Apple
- Microsoft
- Amazon

---

## Problem Statement

Given two sorted arrays,

find the median in

```
O(log(min(n,m)))
```

---

Example

```
nums1

1 3

nums2

2

Median = 2
```

---

# Interview Recognition

One of the most famous FAANG Binary Search problems.

You are **not** searching values.

You are searching the **correct partition**.

---

# Key Idea

Partition both arrays such that

```
Left Half

contains exactly half
the elements.

AND

Every left element

<=

Every right element.
```

---

# Visualization

```
nums1

1 3 8 9

      |

nums2

2 4 5 7 10

      |

Left

1 3 2 4

Right

8 9 5 7 10
```

Adjust partition until

```
maxLeft

<=

minRight
```

---

# Java Solution

```java
class Solution {

    public double findMedianSortedArrays(int[] nums1,
                                         int[] nums2) {

        if (nums1.length > nums2.length)
            return findMedianSortedArrays(nums2, nums1);

        int x = nums1.length;
        int y = nums2.length;

        int left = 0;
        int right = x;

        while (left <= right) {

            int partitionX = left + (right - left) / 2;
            int partitionY = (x + y + 1) / 2 - partitionX;

            int maxLeftX =
                    partitionX == 0 ? Integer.MIN_VALUE
                            : nums1[partitionX - 1];

            int minRightX =
                    partitionX == x ? Integer.MAX_VALUE
                            : nums1[partitionX];

            int maxLeftY =
                    partitionY == 0 ? Integer.MIN_VALUE
                            : nums2[partitionY - 1];

            int minRightY =
                    partitionY == y ? Integer.MAX_VALUE
                            : nums2[partitionY];

            if (maxLeftX <= minRightY &&
                    maxLeftY <= minRightX) {

                if ((x + y) % 2 == 0) {

                    return (Math.max(maxLeftX, maxLeftY)
                            + Math.min(minRightX, minRightY))
                            / 2.0;
                }

                return Math.max(maxLeftX, maxLeftY);
            }

            if (maxLeftX > minRightY)
                right = partitionX - 1;
            else
                left = partitionX + 1;
        }

        return 0;
    }
}
```

---

# Complexity

| Metric | Value |
|--------|------|
| Time | O(log(min(n,m))) |
| Space | O(1) |

---

# Why This Pattern Matters

This problem teaches that Binary Search can operate on

- indices
- partitions
- feasibility
- mathematical invariants

—not just array values.

---

# Problem 15 — Find K-th Smallest Pair Distance (LeetCode 719)

**Difficulty:** Hard

**Pattern**

Binary Search on Answer + Sliding Window

**Companies**

- Google
- Amazon
- Meta

---

## Problem Statement

Given an integer array,

find the

```
k-th

smallest absolute difference
between any pair.
```

---

Example

```
nums

1 3 1

Pairs

(1,1)=0

(1,3)=2

(1,3)=2

Sorted

0 2 2

k=2

Answer=2
```

---

# Recognition

Search space

is

```
distance

NOT

array index.
```

---

# Search Space

Minimum

```
0
```

Maximum

```
max-min
```

---

# Predicate

Count

how many pairs have

```
distance <= mid
```

If count ≥ k

search left.

Otherwise

search right.

---

# Sliding Window Count

Array is sorted first.

Maintain

```
left

right
```

Expand

```
right
```

Shrink

```
left
```

until

```
nums[right]-nums[left]
<=mid
```

Every valid window contributes

```
right-left
```

pairs.

---

# Java Solution

```java
import java.util.Arrays;

class Solution {

    public int smallestDistancePair(int[] nums, int k) {

        Arrays.sort(nums);

        int left = 0;
        int right = nums[nums.length - 1] - nums[0];

        while (left < right) {

            int mid = left + (right - left) / 2;

            if (countPairs(nums, mid) >= k)
                right = mid;
            else
                left = mid + 1;
        }

        return left;
    }

    private int countPairs(int[] nums,
                           int limit) {

        int count = 0;
        int left = 0;

        for (int right = 0;
             right < nums.length;
             right++) {

            while (nums[right] - nums[left] > limit)
                left++;

            count += right - left;
        }

        return count;
    }
}
```

---

# Complexity

| Metric | Value |
|--------|------|
| Sorting | O(n log n) |
| Binary Search | O(log W) |
| Window Count | O(n) |
| Total | O(n log n + n log W) |
| Space | O(1) (excluding sort) |

---

# Pattern Family

This combines two interview favorites:

- Binary Search on Answer
- Sliding Window

A common FAANG follow-up is asking whether the counting step can be improved without brute force.

---

# LLM-Proof Follow-up Questions

For each original problem, here are deeper variations interviewers commonly ask.

| Original Problem | Follow-up Variations |
|------------------|----------------------|
| 704 | Search in an infinite sorted stream; implement recursive binary search; return insertion point instead of -1. |
| 35 | Implement `lower_bound` and `upper_bound`; count occurrences using only binary search; find predecessor/successor. |
| 278 | API calls are expensive—minimize them; first good version instead; predicate changes dynamically. |
| 34 | Find first element ≥ x and last element ≤ y; count frequency in O(log n); search multiple targets efficiently. |
| 33 | Find rotation count; return pivot first, then search; adapt for descending rotated arrays. |
| 81 | Analyze worst-case degradation; prove correctness with duplicates; optimize duplicate skipping. |
| 153 | Handle duplicates (LeetCode 154); return pivot index instead of value; compute rotation count. |
| 162 | Return all peaks; find local minimum; solve for a 2D matrix peak. |
| 852 | Validate whether an array is a valid mountain; search for valleys instead of peaks; support duplicate values. |
| 1095 | Minimize expensive API calls; cache queried values; extend to multiple peaks. |
| 875 | Variable eating speeds each hour; minimize cost instead of speed; prove binary-search monotonicity. |
| 1011 | Packages may be reordered; variable ship capacities per day; minimize average load instead of maximum. |
| 410 | Partition into at most `k` groups; reconstruct the partitions after finding the answer; optimize for weighted costs. |
| 4 | Find the k-th smallest instead of median; extend to three sorted arrays; median of two sorted linked lists. |
| 719 | Return the actual pair instead of distance; support online insertions; solve using heaps instead of binary search. |

---

# Binary Search Techniques & Algorithms Summary

| Technique | Representative Problems | Recognition Clue |
|-----------|-------------------------|------------------|
| Classic Binary Search | 704 | Sorted array + O(log n) |
| Lower Bound | 35, 34 | First element ≥ target |
| Upper Bound | 34 | First element > target |
| Predicate Binary Search | 278 | False...False...True...True |
| Boundary Search | 34 | First/last occurrence |
| Rotated Array Search | 33 | Sorted but rotated |
| Rotated Search with Duplicates | 81 | Rotation + duplicates |
| Find Rotation Pivot | 153 | Minimum in rotated array |
| Peak Search | 162, 852 | Increasing/decreasing slope |
| Bitonic Search | 1095 | Mountain array |
| Binary Search on Answer | 875, 1011, 410 | Minimize/Maximize feasible value |
| Binary Search on Partition | 4 | Two sorted arrays + median |
| Binary Search + Sliding Window | 719 | Count valid answers efficiently |

---

# Binary Search Interview Cheat Sheet

| Interview Hint | Likely Pattern |
|----------------|----------------|
| Sorted array | Classic Binary Search |
| First occurrence | Lower Bound |
| Last occurrence | Upper Bound |
| Rotated sorted array | Rotation Search |
| Find minimum in rotated array | Pivot Search |
| Peak / Mountain | Slope Binary Search |
| Smallest feasible answer | Binary Search on Answer |
| Largest feasible answer | Binary Search on Answer |
| Partition arrays | Binary Search on Partition |
| Count answers ≤ x | Binary Search + Counting |
| Hidden monotonic predicate | Predicate Binary Search |

---

# Final Takeaways

1. Binary Search is not limited to searching arrays—it searches **monotonic search spaces**.
2. Learn to identify the **invariant** rather than memorizing templates.
3. Many Hard interview problems reduce to one of four core ideas:
   - Boundary Search
   - Rotated Search
   - Binary Search on Answer
   - Binary Search on Partition
4. If you can define a monotonic predicate (`true/false` transition), there is a strong chance Binary Search applies.
5. These 15 problems cover the majority of Binary Search patterns encountered in FAANG-style interviews and provide a reusable toolkit for solving many unseen variants.

---
**End of Binary Search Guide**
