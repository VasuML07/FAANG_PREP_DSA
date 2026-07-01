# LeetCode #217 — Contains Duplicate

> **Difficulty:** Easy  
> **Pattern:** HashSet • Duplicate Detection • Membership Lookup  
> **Companies:** Amazon, Google, Meta, Microsoft, Apple, Adobe, Oracle, Bloomberg  
> **Importance:** ⭐⭐⭐⭐⭐ (Fundamental Hashing Pattern)

---

# Table of Contents

- Problem Statement
- Interview Pattern
- Brute Force Solution
- Sorting Solution
- Optimal HashSet Solution
- Dry Run
- Why HashSet Instead of HashMap?
- Complexity Analysis
- Edge Cases
- Interview Follow-ups
- Similar Problems

---

# Problem Statement

Given an integer array `nums`, return `true` if any value appears **at least twice** in the array, and `false` if every element is distinct.

### Example 1

```text
Input:
nums = [1,2,3,1]

Output:
true
```

---

### Example 2

```text
Input:
nums = [1,2,3,4]

Output:
false
```

---

### Example 3

```text
Input:
nums = [1,1,1,3,3,4,3,2,4,2]

Output:
true
```

---

# Interview Pattern

This problem teaches one of the simplest but most useful hashing ideas:

```text
Have I seen this element before?
```

If the answer is **yes**, a duplicate exists.

This pattern appears in many interview questions:

- Contains Duplicate
- Happy Number
- Detect Cycle
- Longest Consecutive Sequence
- Sudoku Validation
- Find Duplicate Number
- Remove Duplicates
- Graph Traversal (Visited Set)

Whenever you need to remember previously visited values, think of a **HashSet**.

---

# Brute Force Solution

Compare every pair.

```text
1

↓

compare

↓

2

↓

compare

↓

3

↓

compare

↓

1
```

---

### Java Code

```java
class Solution {

    public boolean containsDuplicate(int[] nums) {

        for (int i = 0; i < nums.length; i++) {

            for (int j = i + 1; j < nums.length; j++) {

                if (nums[i] == nums[j]) {
                    return true;
                }

            }

        }

        return false;
    }
}
```

---

## Complexity

| Time | Space |
|------|-------|
| O(N²) | O(1) |

Not suitable for large inputs.

---

# Better Approach — Sorting

Sort the array first.

Duplicates become adjacent.

Example

Before sorting

```text
4 2 7 1 2
```

After sorting

```text
1 2 2 4 7
```

Now only compare neighboring elements.

---

### Java Code

```java
import java.util.Arrays;

class Solution {

    public boolean containsDuplicate(int[] nums) {

        Arrays.sort(nums);

        for (int i = 1; i < nums.length; i++) {

            if (nums[i] == nums[i - 1]) {
                return true;
            }

        }

        return false;
    }
}
```

---

## Complexity

| Time | Space |
|------|-------|
| O(N log N) | O(1)* |

> *Sorting primitive arrays in Java uses Dual-Pivot Quicksort with small stack usage.

Although much better than brute force, we can still improve.

---

# Optimal Solution — HashSet

Instead of sorting, remember every number already seen.

Algorithm

```text
Create HashSet

For each number

    Already present?

        Yes

            Duplicate found

        No

            Insert

Finish

No duplicates
```

---

## Production-Grade Java Solution

```java
import java.util.HashSet;
import java.util.Set;

class Solution {

    public boolean containsDuplicate(int[] nums) {

        Set<Integer> seen = new HashSet<>();

        for (int num : nums) {

            if (seen.contains(num)) {
                return true;
            }

            seen.add(num);
        }

        return false;
    }
}
```

---

# Dry Run

Input

```text
nums = [1,2,3,1]
```

Initially

```text
Set = {}
```

---

### Step 1

Current

```text
1
```

Already exists?

```text
No
```

Insert

```text
{1}
```

---

### Step 2

Current

```text
2
```

Exists?

```text
No
```

Insert

```text
{1,2}
```

---

### Step 3

Current

```text
3
```

Exists?

```text
No
```

Insert

```text
{1,2,3}
```

---

### Step 4

Current

```text
1
```

Exists?

```text
YES
```

Immediately return

```text
true
```

Notice that we stop as soon as a duplicate is found.

---

# Visualization

```text
Input

1 2 3 1

↓

Seen

{}

↓

{1}

↓

{1,2}

↓

{1,2,3}

↓

1 already exists

↓

Duplicate
```

---

# Why HashSet Instead of HashMap?

This is an important interview question.

HashMap stores:

```text
Key

↓

Value
```

HashSet stores:

```text
Only Keys
```

For this problem we only care about **existence**, not associated values.

Using a `HashMap<Integer, Integer>` would waste memory because every value would be unused.

---

# Internal View

Suppose we insert

```text
5
```

Internally

```text
Hash(5)

↓

Bucket

↓

Store
```

Checking

```text
contains(5)
```

computes the same bucket directly.

Average lookup

```text
O(1)
```

---

# Complexity Analysis

| Operation | Complexity |
|-----------|------------|
| Insert | O(1) Average |
| Lookup | O(1) Average |
| Traversal | O(N) |

Overall

```text
Time

O(N)
```

```text
Space

O(N)
```

---

# Common Mistakes

## Mistake 1

Using a list

```java
List<Integer> list = new ArrayList<>();
```

Checking

```java
list.contains(x);
```

takes

```text
O(N)
```

Overall complexity becomes

```text
O(N²)
```

---

## Mistake 2

Sorting when the array must remain unchanged.

Sorting modifies the original order.

If order matters later, use a `HashSet`.

---

## Mistake 3

Adding before checking?

Unlike **Two Sum**, either order works here:

```java
if (set.contains(num))
    return true;

set.add(num);
```

or

```java
if (!set.add(num))
    return true;
```

The second approach is more concise because `HashSet.add()` returns `false` if the element already exists.

---

# Cleaner Java Trick

```java
import java.util.HashSet;
import java.util.Set;

class Solution {

    public boolean containsDuplicate(int[] nums) {

        Set<Integer> set = new HashSet<>();

        for (int num : nums) {

            if (!set.add(num)) {
                return true;
            }

        }

        return false;
    }
}
```

This is a common interview idiom.

---

# Edge Cases

### Empty Array

```text
[]

Output

false
```

---

### Single Element

```text
[10]

Output

false
```

---

### All Duplicates

```text
[5,5,5,5]

Output

true
```

---

### Negative Numbers

```text
[-1,-2,-3,-1]

Output

true
```

---

### Large Integers

```text
[1000000000, -1000000000]

Output

false
```

---

# Follow-Up Questions

## Q1. Can this be solved without extra space?

Yes.

Sort the array first.

Time:

```text
O(N log N)
```

Space:

```text
O(1)
```

---

## Q2. Why isn't sorting always preferred?

Because:

- It changes the array order.
- It is slower than hashing.
- HashSet provides average **O(N)** time.

---

## Q3. What happens if many keys collide?

HashSet is backed by a `HashMap`.

Multiple keys may land in the same bucket.

In Java 8+, long collision chains are converted into balanced trees, improving worst-case bucket operations from **O(N)** to **O(log N)**.

---

## Q4. When should you choose HashMap instead?

Use a `HashMap` when you need extra information associated with each key, such as:

- Frequency counts
- Indices
- Last occurrence
- Prefix sums
- Mappings between values

---

# Pattern Recognition

When the problem asks:

- Have we seen this before?
- Detect duplicates.
- Track visited nodes.
- Prevent revisiting.
- Check uniqueness.

Think immediately:

```text
HashSet
```

When the problem asks:

- Count occurrences.
- Store indices.
- Map one value to another.

Think:

```text
HashMap
```

---

# Similar LeetCode Problems

| Problem | Pattern |
|---------|---------|
| 219. Contains Duplicate II | HashMap + Sliding Window |
| 220. Contains Duplicate III | Ordered Set / Buckets |
| 349. Intersection of Two Arrays | HashSet |
| 202. Happy Number | HashSet |
| 128. Longest Consecutive Sequence | HashSet |
| 36. Valid Sudoku | HashSet |
| 771. Jewels and Stones | HashSet |
| 217. Contains Duplicate | HashSet Basics |

---

# Key Takeaways

- Use a **HashSet** when only membership matters.
- Average insertion and lookup are **O(1)**.
- Stop immediately when a duplicate is found.
- Prefer `!set.add(value)` for concise Java code.
- This "visited set" pattern is reused extensively in graph traversal, cycle detection, and many hashing interview problems.

---

**Next:** **LeetCode #242 – Valid Anagram**, where the focus shifts from membership checking to **frequency counting**, introducing one of the most common HashMap/array-counting patterns in interviews.
