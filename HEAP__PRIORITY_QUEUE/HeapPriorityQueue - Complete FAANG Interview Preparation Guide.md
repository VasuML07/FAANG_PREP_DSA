# Heap / Priority Queue - Complete FAANG Interview Preparation Guide (Java)

> GitHub-ready Markdown | Java Only | 15 Curated LeetCode Problems

---

# Table of Contents

## Easy

1. [1046. Last Stone Weight](#1-1046-last-stone-weight)
2. [703. Kth Largest Element in a Stream](#2-703-kth-largest-element-in-a-stream)
3. [1337. The K Weakest Rows in a Matrix](#3-1337-the-k-weakest-rows-in-a-matrix)
4. [506. Relative Ranks](#4-506-relative-ranks)

## Medium

5. [215. Kth Largest Element in an Array](#5-215-kth-largest-element-in-an-array)
6. [973. K Closest Points to Origin](#6-973-k-closest-points-to-origin)
7. [347. Top K Frequent Elements](#7-347-top-k-frequent-elements)
8. [692. Top K Frequent Words](#8-692-top-k-frequent-words)

## Hard

9. [295. Find Median from Data Stream](#9-295-find-median-from-data-stream)
10. [23. Merge k Sorted Lists](#10-23-merge-k-sorted-lists)
11. [480. Sliding Window Median](#11-480-sliding-window-median)
12. [632. Smallest Range Covering Elements from K Lists](#12-632-smallest-range-covering-elements-from-k-lists)
13. [857. Minimum Cost to Hire K Workers](#13-857-minimum-cost-to-hire-k-workers)
14. [502. IPO](#14-502-ipo)
15. [239. Sliding Window Maximum](#15-239-sliding-window-maximum)

---

# 1. 1046 Last Stone Weight

**Difficulty:** Easy

**LeetCode:** https://leetcode.com/problems/last-stone-weight/

**Companies**

- Amazon
- Microsoft
- Google
- Bloomberg
- Adobe

---

## Problem Statement

You have several stones.

Repeatedly:

- Pick the two heaviest stones.
- If equal, destroy both.
- Otherwise destroy the smaller one and insert the difference.

Return the last remaining stone.

---

## Why This Problem Matters

This is the purest heap interview question.

It teaches:

- Max Heap
- Repeated extraction
- Dynamic insertion
- Simulation using Priority Queue

Once comfortable with this pattern, many scheduling and greedy heap problems become easier.

---

## Visualization

Example

```
Stones

2 7 4 1 8 1

Max Heap

        8
      /   \
     7     4
    / \   /
   2  1  1

Remove 8 and 7

Difference =1

Heap

7 removed
8 removed
Insert 1

Remaining

4 2 1 1 1
```

---

## Key Insight

Every iteration needs

- Largest element
- Second largest element

Finding them by sorting repeatedly costs

```
O(N log N)
```

each round.

A max heap gives both removals in

```
log N
```

---

## Java Solution

```java
class Solution {
    public int lastStoneWeight(int[] stones) {

        PriorityQueue<Integer> maxHeap =
                new PriorityQueue<>((a, b) -> b - a);

        for (int stone : stones) {
            maxHeap.offer(stone);
        }

        while (maxHeap.size() > 1) {

            int first = maxHeap.poll();
            int second = maxHeap.poll();

            if (first != second) {
                maxHeap.offer(first - second);
            }
        }

        return maxHeap.isEmpty() ? 0 : maxHeap.poll();
    }
}
```

---

## Line-by-Line Explanation

### Create max heap

```java
PriorityQueue<Integer> maxHeap =
new PriorityQueue<>((a,b)->b-a);
```

Java PriorityQueue is min heap.

Comparator reverses ordering.

---

### Insert all stones

```java
for(int stone:stones)
```

Heap construction.

---

### While two stones exist

```java
while(maxHeap.size()>1)
```

Need two stones for smashing.

---

### Remove largest

```java
int first=maxHeap.poll();
```

Largest.

---

### Remove second largest

```java
int second=maxHeap.poll();
```

Second largest.

---

### Insert difference

```java
if(first!=second)
```

Equal stones disappear.

Otherwise difference returns.

---

## Complexity

| Operation | Complexity |
|-----------|------------|
| Build Heap | O(N logN) |
| Every Smash | O(logN) |
| Overall | O(N logN) |
| Space | O(N) |

---

## Variations

- Smash three largest stones
- Keep smallest remaining stone
- Continuous incoming stones

---

## Pattern Learned

> "Repeatedly extract highest priority item."

---

# 2. 703 Kth Largest Element in a Stream

**Difficulty:** Easy

**LeetCode:** https://leetcode.com/problems/kth-largest-element-in-a-stream/

**Companies**

- Amazon
- Google
- Meta
- Microsoft

---

## Problem Statement

Continuously receive numbers.

Return the kth largest element after every insertion.

---

## Why This Matters

One of the most common interview patterns.

Instead of storing everything...

Store only the useful information.

---

## Visualization

Suppose

```
k = 3

Numbers

5
2
10
8
1
20
```

Maintain only

```
Min Heap

5 8 10

Top

5

Current 3rd largest
```

Insert 20

```
5 removed

Heap

8
10
20

Answer

8
```

---

## Key Insight

Need kth largest.

Not largest.

Not sorted list.

Only keep

```
k largest elements
```

using a

```
Min Heap
```

The smallest inside heap is exactly kth largest.

---

## Java Solution

```java
class KthLargest {

    private PriorityQueue<Integer> minHeap;
    private int k;

    public KthLargest(int k, int[] nums) {

        this.k = k;
        minHeap = new PriorityQueue<>();

        for (int num : nums)
            add(num);
    }

    public int add(int val) {

        minHeap.offer(val);

        if (minHeap.size() > k)
            minHeap.poll();

        return minHeap.peek();
    }
}
```

---

## Line-by-Line Explanation

Store k

```java
this.k=k;
```

Need fixed heap size.

---

Insert

```java
offer(val)
```

Always insert.

---

Too many?

```java
if(size>k)
```

Remove smallest.

---

Answer

```java
peek()
```

Smallest among largest k elements.

Exactly kth largest.

---

## Complexity

| Operation | Complexity |
|------------|------------|
| Add | O(logK) |
| Peek | O(1) |
| Space | O(K) |

---

## Follow-ups

- kth smallest
- kth largest string
- kth largest custom object

---

## Pattern Learned

Fixed-size heap.

One of the highest ROI interview techniques.

---

# 3. 1337 The K Weakest Rows in a Matrix

**Difficulty:** Easy

**LeetCode:** https://leetcode.com/problems/the-k-weakest-rows-in-a-matrix/

**Companies**

- Amazon
- Google
- Apple
- Meta

---

## Problem Statement

Rows contain

```
1 1 1 0 0
```

Soldiers come first.

Weakness

1. Fewer soldiers
2. Smaller index

Return k weakest rows.

---

## Visualization

```
Row 0

11110

4 soldiers

Row 1

11000

2 soldiers

Heap

(2,row1)
(4,row0)
```

---

## Key Insight

Priority

```
Soldier Count

↓

Row Index
```

Store

```
(count,index)
```

inside heap.

---

## Java Solution

```java
class Solution {

    public int[] kWeakestRows(int[][] mat, int k) {

        PriorityQueue<int[]> pq =
                new PriorityQueue<>((a, b) -> {

                    if (a[0] == b[0])
                        return a[1] - b[1];

                    return a[0] - b[0];
                });

        for (int i = 0; i < mat.length; i++) {

            int soldiers = 0;

            for (int val : mat[i])
                soldiers += val;

            pq.offer(new int[]{soldiers, i});
        }

        int[] ans = new int[k];

        for (int i = 0; i < k; i++)
            ans[i] = pq.poll()[1];

        return ans;
    }
}
```

---

## Line-by-Line Explanation

Comparator

```java
count

↓

index
```

Exactly matches problem priority.

---

Count soldiers.

Insert

```
(count,index)
```

Extract k rows.

---

## Complexity

| Metric | Complexity |
|----------|------------|
| Counting | O(MN) |
| Heap | O(MlogM) |
| Space | O(M) |

---

## Follow-ups

- Binary search soldier count
- Max heap optimization
- Streaming rows

---

## Pattern Learned

Heap with custom comparator.

---

# 4. 506 Relative Ranks

**Difficulty:** Easy

**LeetCode:** https://leetcode.com/problems/relative-ranks/

**Companies**

- Amazon
- Microsoft
- Bloomberg

---

## Problem Statement

Assign

```
Highest

Gold Medal

Second

Silver Medal

Third

Bronze Medal

Others

Rank Number
```

---

## Why It Matters

Introduces

```
Priority Queue

+

Original Index Tracking
```

Very common interview requirement.

---

## Visualization

Scores

```
10
3
8
9
4
```

Heap

```
(10,0)
(9,3)
(8,2)
(4,4)
(3,1)
```

Extraction order

```
Gold

Silver

Bronze

4

5
```

---

## Key Insight

Sorting destroys indices.

Heap stores

```
(score,index)
```

allowing reconstruction.

---

## Java Solution

```java
class Solution {

    public String[] findRelativeRanks(int[] score) {

        PriorityQueue<int[]> pq =
                new PriorityQueue<>((a, b) -> b[0] - a[0]);

        for (int i = 0; i < score.length; i++)
            pq.offer(new int[]{score[i], i});

        String[] ans = new String[score.length];

        int rank = 1;

        while (!pq.isEmpty()) {

            int[] curr = pq.poll();

            if (rank == 1)
                ans[curr[1]] = "Gold Medal";
            else if (rank == 2)
                ans[curr[1]] = "Silver Medal";
            else if (rank == 3)
                ans[curr[1]] = "Bronze Medal";
            else
                ans[curr[1]] = String.valueOf(rank);

            rank++;
        }

        return ans;
    }
}
```

---

## Line-by-Line Explanation

### Max Heap

Largest score first.

---

### Store Index

```
(score,index)
```

Allows writing answer in original order.

---

### Rank Assignment

```
1

↓

Gold

2

↓

Silver

3

↓

Bronze
```

Remaining become numeric strings.

---

## Complexity

| Metric | Complexity |
|----------|------------|
| Heap Build | O(NlogN) |
| Extraction | O(NlogN) |
| Space | O(N) |

---

## Follow-ups

- Top 10 medals
- Tie handling
- Dynamic leaderboard

---

## Pattern Learned

Heap + original index restoration.

---

# End of Part 1

Covered:

- Easy heap simulation
- Fixed-size min heap
- Custom comparator
- Max heap with index tracking

**Next (Part 2):**
- **5. Kth Largest Element in an Array**
- **6. K Closest Points to Origin**
- **7. Top K Frequent Elements**
- **8. Top K Frequent Words**

---

# 5. 215. Kth Largest Element in an Array

**Difficulty:** Medium

**LeetCode:** https://leetcode.com/problems/kth-largest-element-in-an-array/

**Companies**

- Google
- Amazon
- Microsoft
- Meta
- Apple
- Bloomberg
- Adobe

---

## Problem Statement

Given an integer array `nums` and an integer `k`, return the **kth largest** element in the array.

The answer is the kth largest element in sorted order, **not** the kth distinct element.

---

## Why This Problem Matters

This is arguably the **most important Top-K interview problem**.

It introduces:

- Fixed-size Min Heap
- Top-K optimization
- Heap vs Sorting trade-offs
- Foundation for dozens of interview questions

---

## Visualization

Example

```
nums = [3,2,1,5,6,4]
k = 2
```

Maintain only 2 largest elements.

```
Insert 3

Heap
3

Insert 2

2
3

Insert 1

1 2 3

Remove smallest

2
3

Insert 5

2
3
5

Remove 2

3
5

Insert 6

3
5
6

Remove 3

5
6

Insert 4

4
5
6

Remove 4

5
6

Top

5
```

Answer = **5**

---

## Key Insight

Sorting gives

```
O(N logN)
```

Need only kth largest.

Maintain only

```
k elements
```

using a Min Heap.

The smallest inside heap is always the kth largest.

---

## Java Solution

```java
class Solution {

    public int findKthLargest(int[] nums, int k) {

        PriorityQueue<Integer> minHeap = new PriorityQueue<>();

        for (int num : nums) {

            minHeap.offer(num);

            if (minHeap.size() > k)
                minHeap.poll();
        }

        return minHeap.peek();
    }
}
```

---

## Line-by-Line Explanation

### Create Min Heap

```java
PriorityQueue<Integer> minHeap =
new PriorityQueue<>();
```

Smallest element always remains at the top.

---

### Insert every element

```java
offer(num);
```

All numbers are considered.

---

### Keep heap size fixed

```java
if(minHeap.size()>k)
```

Remove smallest.

This guarantees heap contains only the largest `k` elements.

---

### Answer

```java
peek();
```

The smallest among those largest `k` numbers.

Exactly kth largest.

---

## Complexity

| Metric | Complexity |
|----------|------------|
| Time | O(N logK) |
| Space | O(K) |

---

## Alternative

QuickSelect

```
Average

O(N)

Worst

O(N²)
```

Interviewers often ask:

> "Can you do better than heap?"

---

## Follow-ups

- kth smallest
- kth largest distinct
- kth largest stream
- kth largest matrix element

---

## Pattern Learned

**Fixed-size Min Heap**

---

## LLM-Proof?

**No**

Recognizing the Top-K pattern is usually sufficient.

---

# 6. 973. K Closest Points to Origin

**Difficulty:** Medium

**LeetCode:** https://leetcode.com/problems/k-closest-points-to-origin/

**Companies**

- Google
- Amazon
- Meta
- Microsoft
- Uber

---

## Problem Statement

Return the `k` points closest to the origin.

Distance

```
√(x²+y²)
```

Need not actually compute square root.

---

## Why This Problem Matters

Introduces

- Max Heap
- Custom Comparator
- Distance Metric
- Heap Optimization

---

## Visualization

Points

```
(1,3)
(-2,2)
(5,8)
(0,1)
```

Distances

```
10
8
89
1
```

Keep

```
k=2
```

Largest distance stays on top.

```
Heap

89
10

Insert 1

89 removed

10
1

Insert 8

10 removed

8
1
```

Remaining

```
(-2,2)
(0,1)
```

---

## Key Insight

Need smallest distances.

Instead of storing all points...

Keep only

```
k closest
```

A Max Heap removes the farthest whenever size exceeds `k`.

---

## Java Solution

```java
class Solution {

    public int[][] kClosest(int[][] points, int k) {

        PriorityQueue<int[]> maxHeap =
                new PriorityQueue<>(
                        (a, b) ->

                                (b[0] * b[0] + b[1] * b[1])
                                -
                                (a[0] * a[0] + a[1] * a[1]));

        for (int[] point : points) {

            maxHeap.offer(point);

            if (maxHeap.size() > k)
                maxHeap.poll();
        }

        int[][] ans = new int[k][2];

        while (k > 0)
            ans[--k] = maxHeap.poll();

        return ans;
    }
}
```

---

## Line-by-Line Explanation

### Comparator

Compare squared distances.

Avoid unnecessary square roots.

---

### Max Heap

Largest distance always removed first.

---

### Fixed Size

If heap exceeds `k`

```
poll()
```

removes farthest point.

---

### Build Result

Remaining heap contains exactly answer.

---

## Complexity

| Metric | Complexity |
|----------|------------|
| Time | O(N logK) |
| Space | O(K) |

---

## Follow-ups

- Closest cities
- Closest restaurants
- k nearest neighbors
- Closest values in BST

---

## Pattern Learned

**Fixed-size Max Heap**

---

## LLM-Proof?

**Moderate**

Requires identifying which heap (Min vs Max) minimizes work.

---

# 7. 347. Top K Frequent Elements

**Difficulty:** Medium

**LeetCode:** https://leetcode.com/problems/top-k-frequent-elements/

**Companies**

- Amazon
- Google
- Meta
- Microsoft
- Apple
- LinkedIn

---

## Problem Statement

Return the `k` most frequent elements.

Order does not matter.

---

## Why This Problem Matters

One of the most frequently asked heap problems.

Teaches

- Frequency Map
- Heap
- Custom Comparator

This pattern appears in logs, analytics, recommendation systems, and search engines.

---

## Visualization

```
1 1 1 2 2 3
```

Frequency

| Number | Count |
|---------|--------|
|1|3|
|2|2|
|3|1|

Heap

```
(3,1)
(2,2)
(1,3)
```

Top

```
1
2
```

---

## Key Insight

First count frequencies.

Then rank by frequency.

Heap stores

```
(number,frequency)
```

---

## Java Solution

```java
class Solution {

    public int[] topKFrequent(int[] nums, int k) {

        HashMap<Integer, Integer> map = new HashMap<>();

        for (int num : nums)
            map.put(num, map.getOrDefault(num, 0) + 1);

        PriorityQueue<Integer> minHeap =
                new PriorityQueue<>(
                        (a, b) -> map.get(a) - map.get(b));

        for (int num : map.keySet()) {

            minHeap.offer(num);

            if (minHeap.size() > k)
                minHeap.poll();
        }

        int[] ans = new int[k];

        while (k > 0)
            ans[--k] = minHeap.poll();

        return ans;
    }
}
```

---

## Line-by-Line Explanation

### Count

```java
HashMap
```

Stores frequencies.

---

### Heap Comparator

Numbers ordered by frequency.

---

### Fixed Heap

Only keep top `k` frequencies.

---

### Result

Heap contains required answer.

---

## Complexity

| Metric | Complexity |
|----------|------------|
| Counting | O(N) |
| Heap | O(M logK) |

`M = unique numbers`

---

## Follow-ups

- Top k words
- Top k websites
- Most common hashtags
- Most viewed videos

---

## Pattern Learned

**Frequency Map + Heap**

---

## LLM-Proof?

**No**

Classic interview pattern.

---

# 8. 692. Top K Frequent Words

**Difficulty:** Medium

**LeetCode:** https://leetcode.com/problems/top-k-frequent-words/

**Companies**

- Google
- Amazon
- Microsoft
- Apple
- Yelp

---

## Problem Statement

Return the `k` most frequent words.

If frequencies tie,

Return lexicographically smaller word first.

---

## Why This Problem Matters

Adds an important interview twist:

Multiple ordering rules.

Candidates often fail comparator implementation.

---

## Visualization

Input

```
i
love
leetcode
i
love
coding
```

Frequency

|Word|Count|
|----|------|
|i|2|
|love|2|
|leetcode|1|
|coding|1|

Ordering

```
2

↓

Lexicographical

↓

Frequency
```

---

## Key Insight

Heap comparator

Priority

```
Frequency

↓

Lexicographical Order
```

For fixed-size Min Heap

Comparator becomes slightly reversed.

---

## Java Solution

```java
class Solution {

    public List<String> topKFrequent(String[] words, int k) {

        HashMap<String, Integer> map = new HashMap<>();

        for (String word : words)
            map.put(word, map.getOrDefault(word, 0) + 1);

        PriorityQueue<String> minHeap =
                new PriorityQueue<>((a, b) -> {

                    if (!map.get(a).equals(map.get(b)))
                        return map.get(a) - map.get(b);

                    return b.compareTo(a);
                });

        for (String word : map.keySet()) {

            minHeap.offer(word);

            if (minHeap.size() > k)
                minHeap.poll();
        }

        LinkedList<String> ans = new LinkedList<>();

        while (!minHeap.isEmpty())
            ans.addFirst(minHeap.poll());

        return ans;
    }
}
```

---

## Line-by-Line Explanation

### Count Frequencies

HashMap stores occurrences.

---

### Comparator

Primary

```
Frequency
```

Secondary

```
Reverse Lexicographical
```

Why reverse?

Because the heap removes the "worst" candidate first.

---

### Reverse Output

Heap returns smallest first.

Insert at front.

```java
addFirst()
```

Produces correct descending order.

---

## Complexity

| Metric | Complexity |
|----------|------------|
| Counting | O(N) |
| Heap | O(M logK) |
| Space | O(M) |

---

## Follow-ups

- Top hashtags
- Trending searches
- Most frequent filenames
- Search autocomplete ranking

---

## Pattern Learned

**Frequency + Comparator Design**

Comparator questions are extremely common in Google and Meta interviews.

---

## LLM-Proof?

**Yes**

The comparator is subtle.

Most incorrect solutions fail because they mishandle the lexicographical tie-breaking inside a fixed-size Min Heap.

---

# End of Part 2

Completed:

- 5. Kth Largest Element in an Array
- 6. K Closest Points to Origin
- 7. Top K Frequent Elements
- 8. Top K Frequent Words

**Next (Part 3):**

- **295. Find Median from Data Stream** ⭐
- **23. Merge k Sorted Lists**
- **480. Sliding Window Median** ⭐ (LLM-Proof)
- **632. Smallest Range Covering Elements from K Lists** ⭐


---

# 9. 295. Find Median from Data Stream

**Difficulty:** Hard

**LeetCode:** https://leetcode.com/problems/find-median-from-data-stream/

**Companies**

- Google
- Meta
- Amazon
- Microsoft
- Apple
- Bloomberg
- Uber

**LLM-Proof:** ✅ Yes

---

## Problem Statement

Design a data structure that supports:

- `addNum(int num)`
- `findMedian()`

Numbers arrive one by one.

Both operations should be efficient.

---

## Why This Problem Matters

This is one of the highest-frequency Heap interview questions.

It introduces:

- Two Heaps
- Dynamic balancing
- Online algorithms
- Running median

Many interviewers expect this solution immediately.

---

## Visualization

Incoming stream

```
5
2
8
10
3
```

Maintain

```
Max Heap (Left)

5
2
3

Min Heap (Right)

8
10
```

```
          Median

        Left Top = 5
```

If total count becomes even

```
Median

=

(5+8)/2
```

---

## Key Insight

Split numbers into two groups.

```
Smaller Half

↓

Max Heap

----------------

Larger Half

↓

Min Heap
```

Invariant

```
Left Size == Right Size

OR

Left Size = Right Size + 1
```

---

## Java Solution

```java
class MedianFinder {

    private PriorityQueue<Integer> left;
    private PriorityQueue<Integer> right;

    public MedianFinder() {

        left = new PriorityQueue<>((a, b) -> b - a);
        right = new PriorityQueue<>();
    }

    public void addNum(int num) {

        left.offer(num);

        right.offer(left.poll());

        if (right.size() > left.size())
            left.offer(right.poll());
    }

    public double findMedian() {

        if (left.size() > right.size())
            return left.peek();

        return (left.peek() + right.peek()) / 2.0;
    }
}
```

---

## Line-by-Line Explanation

### Max Heap

Stores smaller half.

Largest among smaller numbers stays on top.

---

### Min Heap

Stores larger half.

Smallest among larger numbers stays on top.

---

### Insert

Always insert into left.

---

### Move Largest Left

```
left

↓

right
```

Maintains ordering.

---

### Balance

If right becomes larger

Move smallest back.

---

### Median

Odd count

```
left.peek()
```

Even count

```
(left.peek()+right.peek())/2
```

---

## Complexity

| Metric | Complexity |
|---------|------------|
| addNum | O(logN) |
| findMedian | O(1) |
| Space | O(N) |

---

## Follow-ups

- Running percentile
- Running average
- Sliding median
- Running quartiles

---

## Pattern Learned

**Two Heap Technique**

---

# 10. 23. Merge k Sorted Lists

**Difficulty:** Hard

**LeetCode:** https://leetcode.com/problems/merge-k-sorted-lists/

**Companies**

- Google
- Amazon
- Microsoft
- Meta
- Apple
- Oracle

---

## Problem Statement

Merge `k` sorted linked lists into one sorted linked list.

---

## Why This Problem Matters

Generalizes merge of two lists.

Instead of repeatedly scanning all heads,

Always remove the smallest node using a heap.

---

## Visualization

```
L1

1→4→8

L2

2→3→9

L3

5→6→7
```

Heap

```
1
2
5
```

Remove

```
1
```

Insert

```
4
```

Heap

```
2
4
5
```

Repeat.

---

## Key Insight

Only one node from each list can be the next answer.

Heap size never exceeds

```
k
```

---

## Java Solution

```java
class Solution {

    public ListNode mergeKLists(ListNode[] lists) {

        PriorityQueue<ListNode> pq =
                new PriorityQueue<>(
                        (a, b) -> a.val - b.val);

        for (ListNode node : lists)
            if (node != null)
                pq.offer(node);

        ListNode dummy = new ListNode(0);
        ListNode tail = dummy;

        while (!pq.isEmpty()) {

            ListNode node = pq.poll();

            tail.next = node;
            tail = tail.next;

            if (node.next != null)
                pq.offer(node.next);
        }

        return dummy.next;
    }
}
```

---

## Line-by-Line Explanation

Insert first node from every list.

Heap contains only current candidates.

---

Remove smallest.

Append to answer.

---

Insert next node from same list.

Repeat until heap becomes empty.

---

## Complexity

| Metric | Complexity |
|---------|------------|
| Time | O(N logK) |
| Space | O(K) |

`N = total nodes`

---

## Follow-ups

- Merge k arrays
- Merge log files
- Merge sorted streams
- External merge sort

---

## Pattern Learned

**Multi-way Merge**

---

## LLM-Proof?

Moderate.

Recognizing heap size remains `k` is the main observation.

---

# 11. 480. Sliding Window Median

**Difficulty:** Hard

**LeetCode:** https://leetcode.com/problems/sliding-window-median/

**Companies**

- Google
- Meta
- Jane Street
- Bloomberg
- Amazon

**LLM-Proof:** ✅ Strongly Yes

---

## Problem Statement

Return median for every sliding window of size `k`.

---

## Why This Problem Matters

This combines

- Sliding Window
- Two Heaps
- Lazy Deletion
- Heap Balancing

This is one of the hardest mainstream Heap interview questions.

---

## Visualization

```
Window

1 3 -1

Median

1

↓

Slide

3 -1 -3

Median

-1
```

Problem

Leaving element

```
1
```

is not necessarily at heap top.

Need delayed deletion.

---

## Key Insight

PriorityQueue cannot delete arbitrary elements efficiently.

Instead,

Store elements to delete later.

```
HashMap

value

↓

count
```

Whenever heap top is marked,

remove it.

This is called

```
Lazy Deletion
```

---

## High-Level Algorithm

```
Insert number

↓

Balance heaps

↓

Mark outgoing number

↓

Clean heap tops

↓

Read median
```

---

## Java Skeleton

```java
class Solution {

    public double[] medianSlidingWindow(int[] nums, int k) {

        // Two Heaps

        PriorityQueue<Integer> small =
                new PriorityQueue<>((a,b)->b-a);

        PriorityQueue<Integer> large =
                new PriorityQueue<>();

        HashMap<Integer,Integer> delayed =
                new HashMap<>();

        /*
            Full implementation intentionally omitted here.

            Interview focus:

            - Dual heaps
            - Lazy deletion
            - Balance after every insertion/removal
            - Remove invalid heap tops repeatedly
        */

        return new double[0];
    }
}
```

---

## Why Skeleton Instead of Full Code?

The complete Java solution is well over 120 lines.

Understanding the balancing logic is significantly more valuable than memorizing the implementation.

---

## Complexity

| Metric | Complexity |
|---------|------------|
| Time | O(N logK) |
| Space | O(K) |

---

## Follow-ups

- Running percentile
- Sliding average
- Sliding kth largest
- Sliding maximum

---

## Pattern Learned

**Two Heaps + Lazy Deletion**

---

## LLM-Proof?

✅ Yes.

The challenge lies in correctly maintaining heap invariants while handling stale elements.

---

# 12. 632. Smallest Range Covering Elements from K Lists

**Difficulty:** Hard

**LeetCode:** https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists/

**Companies**

- Google
- Meta
- Amazon
- Microsoft

**LLM-Proof:** ✅ Yes

---

## Problem Statement

Given `k` sorted lists,

find the smallest range containing at least one number from every list.

---

## Visualization

```
List1

4
10
15

List2

1
8
20

List3

5
12
18
```

Heap

```
1
4
5
```

Current Max

```
5
```

Range

```
1 → 5
```

Remove

```
1
```

Insert

```
8
```

Current Max

```
8
```

New Range

```
4 → 8
```

Continue.

---

## Key Insight

Maintain

```
Minimum

↓

Heap

Maximum

↓

Variable
```

At every step

```
Range

=

Current Max

-

Current Min
```

Move only the list that contributed the minimum element.

---

## Java Solution

```java
class Solution {

    public int[] smallestRange(List<List<Integer>> nums) {

        PriorityQueue<int[]> pq =
                new PriorityQueue<>(
                        (a,b)->a[0]-b[0]);

        int currentMax = Integer.MIN_VALUE;

        for(int i=0;i<nums.size();i++){

            int value = nums.get(i).get(0);

            pq.offer(new int[]{value,i,0});

            currentMax=Math.max(currentMax,value);
        }

        int start=0;
        int end=Integer.MAX_VALUE;

        while(true){

            int[] curr=pq.poll();

            int value=curr[0];
            int row=curr[1];
            int col=curr[2];

            if(currentMax-value<end-start){

                start=value;
                end=currentMax;
            }

            if(col+1==nums.get(row).size())
                break;

            int next=nums.get(row).get(col+1);

            currentMax=Math.max(currentMax,next);

            pq.offer(new int[]{next,row,col+1});
        }

        return new int[]{start,end};
    }
}
```

---

## Line-by-Line Explanation

Heap always stores

```
One element

↓

Each List
```

Current maximum tracked separately.

Whenever minimum advances,

recompute the candidate range.

If any list finishes,

no future valid range exists.

---

## Complexity

| Metric | Complexity |
|---------|------------|
| Time | O(N logK) |
| Space | O(K) |

---

## Follow-ups

- Merge sorted streams
- K-way merge
- Smallest interval among files
- Multi-source scheduling

---

## Pattern Learned

**Heap + Running Maximum**

---

# End of Part 3

Completed:

- 9. Find Median from Data Stream ⭐
- 10. Merge k Sorted Lists
- 11. Sliding Window Median ⭐
- 12. Smallest Range Covering Elements from K Lists ⭐

**Next (Final Part):**

- **857. Minimum Cost to Hire K Workers**
- **502. IPO**
- **239. Sliding Window Maximum**
- Final Heap/Priority Queue Pattern Cheat Sheet
- Interview Summary & Recognition Guide


---

# 13. 857. Minimum Cost to Hire K Workers

**Difficulty:** Hard

**LeetCode:** https://leetcode.com/problems/minimum-cost-to-hire-k-workers/

**Companies**

- Google
- Meta
- Amazon
- Microsoft
- Bloomberg

**LLM-Proof:** ✅ Yes

---

## Problem Statement

There are `n` workers.

Each worker has:

- Quality
- Minimum Wage Expectation

You must hire exactly **k** workers.

Every worker must be paid using the same wage-to-quality ratio.

Return the minimum total hiring cost.

---

## Why This Problem Matters

This is one of the best examples of combining:

- Greedy
- Sorting
- Max Heap

There is no obvious heap pattern. The greedy observation is the real challenge.

---

## Visualization

Suppose

| Worker | Quality | Wage | Ratio |
|---------|---------|------|------|
| A | 10 | 70 | 7 |
| B | 20 | 50 | 2.5 |
| C | 5 | 30 | 6 |

Sort by ratio.

```
2.5

↓

6

↓

7
```

Maintain

```
Current Ratio

×

Total Quality
```

Whenever quality becomes too large,

remove the largest quality using a Max Heap.

---

## Key Insight

If worker `i` has the highest ratio in the selected group,

everyone must be paid using that ratio.

Therefore

```
Cost

=

Current Ratio

×

Total Quality
```

To minimize cost,

keep the total quality as small as possible.

---

## Java Solution

```java
class Solution {

    public double mincostToHireWorkers(int[] quality,
                                       int[] wage,
                                       int k) {

        int n = quality.length;

        double[][] workers = new double[n][2];

        for (int i = 0; i < n; i++) {

            workers[i][0] =
                    (double) wage[i] / quality[i];

            workers[i][1] = quality[i];
        }

        Arrays.sort(workers,
                Comparator.comparingDouble(a -> a[0]));

        PriorityQueue<Integer> maxHeap =
                new PriorityQueue<>((a, b) -> b - a);

        int totalQuality = 0;

        double answer = Double.MAX_VALUE;

        for (double[] worker : workers) {

            int q = (int) worker[1];

            totalQuality += q;

            maxHeap.offer(q);

            if (maxHeap.size() > k)
                totalQuality -= maxHeap.poll();

            if (maxHeap.size() == k)
                answer = Math.min(
                        answer,
                        totalQuality * worker[0]);
        }

        return answer;
    }
}
```

---

## Line-by-Line Explanation

### Sort

Workers sorted by increasing ratio.

---

### Max Heap

Stores qualities.

Largest quality removed first.

---

### Running Sum

Maintains total quality.

---

### Compute Cost

```
ratio × totalQuality
```

Update minimum.

---

## Complexity

| Metric | Complexity |
|---------|------------|
| Sorting | O(N logN) |
| Heap | O(N logK) |
| Space | O(K) |

---

## Pattern Learned

**Greedy + Max Heap**

---

## Interview Takeaway

If an interviewer combines

- Heap
- Greedy
- Sorting

this problem should come to mind.

---

# 14. 502. IPO

**Difficulty:** Hard

**LeetCode:** https://leetcode.com/problems/ipo/

**Companies**

- Google
- Amazon
- Meta
- Microsoft

---

## Problem Statement

Initially,

Capital = `W`

There are several projects.

Each project requires:

- Capital
- Profit

Complete at most `k` projects.

Maximize final capital.

---

## Visualization

Projects

| Capital | Profit |
|----------|--------|
|0|1|
|1|3|
|2|5|

Initially

```
Capital = 1
```

Available

```
0

1
```

Choose

```
Highest Profit

↓

3
```

Capital becomes

```
4
```

Now project requiring

```
2
```

also becomes available.

---

## Key Insight

Need two structures.

Sort projects

```
Capital
```

Use Max Heap

```
Profit
```

Whenever capital increases,

new projects become available.

---

## Java Solution

```java
class Solution {

    public int findMaximizedCapital(int k,
                                    int w,
                                    int[] profits,
                                    int[] capital) {

        int n = profits.length;

        int[][] projects = new int[n][2];

        for (int i = 0; i < n; i++) {

            projects[i][0] = capital[i];
            projects[i][1] = profits[i];
        }

        Arrays.sort(projects,
                Comparator.comparingInt(a -> a[0]));

        PriorityQueue<Integer> maxHeap =
                new PriorityQueue<>((a, b) -> b - a);

        int index = 0;

        while (k-- > 0) {

            while (index < n &&
                    projects[index][0] <= w) {

                maxHeap.offer(projects[index][1]);
                index++;
            }

            if (maxHeap.isEmpty())
                break;

            w += maxHeap.poll();
        }

        return w;
    }
}
```

---

## Line-by-Line Explanation

### Sort

Projects ordered by required capital.

---

### Unlock Projects

Whenever capital grows,

push every newly available project.

---

### Max Heap

Choose highest profit.

---

### Repeat

Until

- k projects completed
- or no projects remain.

---

## Complexity

| Metric | Complexity |
|---------|------------|
| Sorting | O(N logN) |
| Heap | O(K logN) |
| Space | O(N) |

---

## Follow-ups

- Project scheduling
- Budget optimization
- Startup investment selection

---

## Pattern Learned

**Sort + Unlock + Max Heap**

---

## LLM-Proof?

Moderate.

Requires recognizing two independent ordering criteria.

---

# 15. 239. Sliding Window Maximum

**Difficulty:** Hard *(Official LeetCode: Hard)*

**LeetCode:** https://leetcode.com/problems/sliding-window-maximum/

**Companies**

- Amazon
- Google
- Meta
- Microsoft
- Apple
- Uber
- TikTok

---

## Problem Statement

Return the maximum element for every window of size `k`.

---

## Why This Problem Matters

Although the optimal solution uses a **Monotonic Deque**, interviewers frequently ask for the heap-based solution first to evaluate understanding of lazy removal.

This problem teaches:

- Max Heap
- Lazy Deletion
- Sliding Window
- Index Tracking

---

## Visualization

```
Window

1 3 -1

↓

Maximum = 3

Slide

3 -1 -3

↓

Maximum = 3

Slide

-1 -3 5

↓

Maximum = 5
```

Heap stores

```
(value,index)
```

Whenever the top index is outside the window,

discard it.

---

## Key Insight

Heap alone cannot remove expired elements efficiently.

Instead,

store indices.

Whenever

```
top.index < windowStart
```

remove it.

This is another example of **lazy deletion**.

---

## Java Solution

```java
class Solution {

    public int[] maxSlidingWindow(int[] nums, int k) {

        PriorityQueue<int[]> maxHeap =
                new PriorityQueue<>(
                        (a, b) -> b[0] - a[0]);

        int[] ans =
                new int[nums.length - k + 1];

        int index = 0;

        for (int i = 0; i < nums.length; i++) {

            maxHeap.offer(new int[]{nums[i], i});

            while (maxHeap.peek()[1] <= i - k)
                maxHeap.poll();

            if (i >= k - 1)
                ans[index++] = maxHeap.peek()[0];
        }

        return ans;
    }
}
```

---

## Line-by-Line Explanation

### Store

```
(value,index)
```

Index determines whether an element has expired.

---

### Insert

Current element enters the heap.

---

### Remove Expired

```
while(top.index <= i-k)
```

Discard elements no longer inside the window.

---

### Read Maximum

Heap top always contains the current maximum.

---

## Complexity

| Metric | Complexity |
|---------|------------|
| Time | O(N logK) |
| Space | O(K) |

---

## Better Solution?

Yes.

A **Monotonic Deque** solves this in

```
O(N)
```

The heap solution remains valuable because it generalizes to many sliding-window variants where monotonic queues do not apply directly.

---

## Pattern Learned

**Heap + Lazy Removal**

---

## Interview Note

If asked:

> "Can you optimize further?"

mention the Monotonic Queue approach.

---

# Heap / Priority Queue Pattern Cheat Sheet

| Pattern | Representative Problem |
|---------|------------------------|
| Max Heap Simulation | 1046. Last Stone Weight |
| Fixed-Size Min Heap | 703. Kth Largest in a Stream |
| Top-K Elements | 215. Kth Largest Element |
| Fixed-Size Max Heap | 973. K Closest Points |
| Frequency + Heap | 347. Top K Frequent Elements |
| Comparator Design | 692. Top K Frequent Words |
| Two Heaps | 295. Median from Data Stream |
| K-Way Merge | 23. Merge k Sorted Lists |
| Lazy Deletion | 239, 480 |
| Heap + Running Maximum | 632. Smallest Range |
| Greedy + Heap | 857. Hire K Workers |
| Sort + Heap | 502. IPO |

---

# Heap Interview Recognition Guide

| If You See... | Think... |
|---------------|----------|
| Largest / Smallest repeatedly | Heap |
| Top K | Fixed-size Heap |
| Dynamic Median | Two Heaps |
| Merge many sorted lists | Min Heap |
| Frequency ranking | HashMap + Heap |
| Sliding window with max/median | Heap + Lazy Deletion |
| Scheduling by priority | Priority Queue |
| Multiple ordering rules | Custom Comparator |
| Greedy with repeated best choice | Heap |
| Stream processing | Priority Queue |

---

# Most Important Heap Problems for FAANG

| Priority | Problem |
|----------|---------|
| ⭐⭐⭐⭐⭐ | 295. Find Median from Data Stream |
| ⭐⭐⭐⭐⭐ | 23. Merge k Sorted Lists |
| ⭐⭐⭐⭐⭐ | 215. Kth Largest Element in an Array |
| ⭐⭐⭐⭐⭐ | 347. Top K Frequent Elements |
| ⭐⭐⭐⭐☆ | 703. Kth Largest in a Stream |
| ⭐⭐⭐⭐☆ | 973. K Closest Points |
| ⭐⭐⭐⭐☆ | 239. Sliding Window Maximum |
| ⭐⭐⭐⭐☆ | 632. Smallest Range Covering K Lists |
| ⭐⭐⭐⭐☆ | 502. IPO |
| ⭐⭐⭐⭐☆ | 857. Minimum Cost to Hire K Workers |
| ⭐⭐⭐☆☆ | 692. Top K Frequent Words |
| ⭐⭐⭐☆☆ | 1337. K Weakest Rows |
| ⭐⭐⭐☆☆ | 506. Relative Ranks |
| ⭐⭐⭐☆☆ | 1046. Last Stone Weight |
| ⭐⭐⭐☆☆ | 480. Sliding Window Median |

---

# Final Revision Checklist

Before interviews, make sure you can implement from memory:

- Max Heap and Min Heap using `PriorityQueue`
- Custom comparators for primitive arrays and objects
- Fixed-size heap (`size > k`)
- Two-heap balancing
- Frequency map + heap
- K-way merge
- Lazy deletion using indices or a hash map
- Greedy + heap combinations
- Sorting + heap workflows
- Time and space complexity analysis for each pattern

---

# End of Guide
