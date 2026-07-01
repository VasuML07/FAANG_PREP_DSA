# Linked Lists — Complete FAANG Interview Preparation Guide (Java)

> **Language:** Java  
> **Questions Covered:** 15  
> **Focus:** FAANG Interview Preparation  
> **Difficulty:** Easy → Medium → Hard

---

# Index

| # | Problem | Difficulty | Primary Pattern | Frequently Asked By |
|---|----------|------------|-----------------|--------------------|
|1|LeetCode 206 — Reverse Linked List|Easy|Pointer Manipulation|Google, Amazon, Microsoft, Apple, Meta|
|2|LeetCode 21 — Merge Two Sorted Lists|Easy|Merge|Amazon, Microsoft, Meta|
|3|LeetCode 141 — Linked List Cycle|Easy|Fast & Slow Pointer|Google, Amazon, Apple|
|4|LeetCode 83 — Remove Duplicates from Sorted List|Easy|Traversal|Microsoft, Meta|
|5|LeetCode 203 — Remove Linked List Elements|Easy|Dummy Node|Amazon|
|6|LeetCode 19 — Remove Nth Node From End|Medium|Two Pointers|Google, Microsoft, Meta|
|7|LeetCode 24 — Swap Nodes in Pairs|Medium|Pointer Manipulation|Amazon|
|8|LeetCode 143 — Reorder List|Medium|Reverse + Merge|Google, Apple|
|9|LeetCode 2 — Add Two Numbers|Medium|Simulation|Amazon, Meta|
|10|LeetCode 92 — Reverse Linked List II|Medium|Sub-list Reversal|Google|
|11|LeetCode 138 — Copy List with Random Pointer|Medium|HashMap / Node Cloning|Meta, Microsoft|
|12|LeetCode 146 — LRU Cache|Medium|DLL + HashMap|Google, Amazon|
|13|LeetCode 25 — Reverse Nodes in k-Group|Hard|Segment Reversal|Google, Apple|
|14|LeetCode 23 — Merge k Sorted Lists|Hard|Heap / Divide & Conquer|Google, Amazon|
|15|LeetCode 460 — LFU Cache|Hard|DLL + HashMap Design|Google, Meta|

---

# Essential Linked List Techniques

These techniques repeatedly appear in FAANG interviews.

---

## 1. Dummy Node

Instead of treating the head separately, create a fake node before it.

```
dummy -> head
```

Used in:

- LC 21
- LC 19
- LC 24
- LC 203
- LC 25

Benefits:

- Eliminates head edge cases
- Cleaner code
- Easier insertion/deletion

---

## 2. Fast & Slow Pointer

```
Fast: 2 steps
Slow: 1 step
```

Applications:

- Cycle detection
- Middle of list
- Palindrome
- Reordering

Used in:

- LC 141
- LC 143

---

## 3. In-place Reversal

```
prev
 |
NULL

curr -> next
```

Core operations:

```
next = curr.next
curr.next = prev
prev = curr
curr = next
```

Used in:

- LC 206
- LC 92
- LC 143
- LC 25

---

## 4. Merge Pattern

Maintain one tail pointer.

```
tail
 |
1→2→5

3→4
```

Move the smaller node each iteration.

Used in:

- LC 21
- LC 23

---

## 5. Two Pointer Gap Technique

Create distance of N.

```
Fast ---------->

Slow --->
```

When fast reaches end:

Slow is at required position.

Used in:

- LC 19

---

## 6. Node Cloning

Either

```
HashMap<Node, Node>
```

or

```
Original
Clone
Original
Clone
```

Used in:

- LC 138

---

## 7. Doubly Linked List + HashMap

Foundation of cache design.

```
Map
↓

Node <-> Node <-> Node
```

Used in:

- LC 146
- LC 460

---

# Problem 1 — Reverse Linked List

**LeetCode:** 206

**Difficulty:** Easy

**Asked By**

Google • Amazon • Microsoft • Apple • Meta

---

## Problem

Reverse a singly linked list.

Example

```
1 → 2 → 3 → 4 → 5

↓

5 → 4 → 3 → 2 → 1
```

---

# Approach 1 — Using Stack

Store nodes.

Pop one by one.

Reconnect.

### Complexity

Time

```
O(N)
```

Space

```
O(N)
```

Not preferred in interviews.

---

# Approach 2 — Iterative (Optimal)

Maintain three pointers.

```
prev
curr
next
```

Diagram

Initial

```
NULL <- 1 ->2->3->4
       ^
      curr
```

Iteration

```
next = curr.next

curr.next = prev

prev = curr

curr = next
```

Repeat until curr becomes NULL.

---

## Java

```java
class Solution {
    public ListNode reverseList(ListNode head) {

        ListNode prev = null;
        ListNode curr = head;

        while (curr != null) {

            ListNode next = curr.next;

            curr.next = prev;

            prev = curr;

            curr = next;
        }

        return prev;
    }
}
```

---

## Recursive Approach

Idea

Reverse remaining list first.

Then attach current node.

Java

```java
class Solution {

    public ListNode reverseList(ListNode head) {

        if (head == null || head.next == null)
            return head;

        ListNode newHead = reverseList(head.next);

        head.next.next = head;
        head.next = null;

        return newHead;
    }
}
```

---

## Complexity

|Metric|Value|
|------|-----|
|Time|O(N)|
|Space (Iterative)|O(1)|
|Space (Recursive)|O(N)|

---

## Interview Tricks

✓ Forgetting

```
next = curr.next
```

before changing pointer is the biggest bug.

✓ Last node becomes new head.

✓ Original head becomes tail.

---

## Why Interviewers Ask This

Tests

- Pointer manipulation
- Memory understanding
- Confidence with linked lists

This is arguably the most fundamental linked list problem.

---

# Problem 2 — Merge Two Sorted Lists

**LeetCode:** 21

**Difficulty:** Easy

**Asked By**

Amazon • Microsoft • Meta

---

## Problem

Merge

```
1→3→5

2→4→6
```

Output

```
1→2→3→4→5→6
```

---

# Approach 1 — Create New List

Allocate new nodes.

Simple.

Extra memory.

---

Complexity

Time

```
O(N)
```

Space

```
O(N)
```

---

# Approach 2 — Reuse Existing Nodes (Optimal)

Use dummy node.

```
dummy

tail
```

Always connect smaller node.

Diagram

```
L1

1→4→6

L2

2→3→5

↓

dummy→1→2→3→4→5→6
```

---

## Java

```java
class Solution {

    public ListNode mergeTwoLists(ListNode list1, ListNode list2) {

        ListNode dummy = new ListNode(-1);
        ListNode tail = dummy;

        while (list1 != null && list2 != null) {

            if (list1.val <= list2.val) {
                tail.next = list1;
                list1 = list1.next;
            } else {
                tail.next = list2;
                list2 = list2.next;
            }

            tail = tail.next;
        }

        if (list1 != null)
            tail.next = list1;

        if (list2 != null)
            tail.next = list2;

        return dummy.next;
    }
}
```

---

## Complexity

|Metric|Value|
|------|-----|
|Time|O(N+M)|
|Space|O(1)|

---

## Edge Cases

- One list empty
- Both empty
- Duplicate values
- Negative numbers

---

## Interview Insight

Dummy nodes simplify nearly every merge question.

This technique appears again in:

- Merge K Lists
- Reverse K Group
- Remove Nth Node

---

# Problem 3 — Linked List Cycle

**LeetCode:** 141

**Difficulty:** Easy

**Asked By**

Google • Amazon • Apple

---

## Problem

Determine whether a linked list contains a cycle.

Example

```
1→2→3→4
    ↑  ↓
    ←←←
```

Return

```
true
```

---

# Approach 1 — HashSet

Store visited nodes.

If node repeats

Cycle exists.

---

Complexity

Time

```
O(N)
```

Space

```
O(N)
```

---

# Approach 2 — Floyd's Cycle Detection (Optimal)

Also called

```
Tortoise and Hare
```

Diagram

```
Slow

1→2→3→4→5

Fast

1→3→5→...
```

If no cycle

Fast reaches NULL.

If cycle

Eventually

```
Fast == Slow
```

---

## Why It Works

Inside a cycle,

Fast gains one node every iteration.

Eventually catches slow.

Mathematical proof commonly asked in interviews.

---

## Java

```java
class Solution {

    public boolean hasCycle(ListNode head) {

        if (head == null)
            return false;

        ListNode slow = head;
        ListNode fast = head;

        while (fast != null && fast.next != null) {

            slow = slow.next;
            fast = fast.next.next;

            if (slow == fast)
                return true;
        }

        return false;
    }
}
```

---

## Complexity

|Metric|Value|
|------|-----|
|Time|O(N)|
|Space|O(1)|

---

## Edge Cases

✓ Single node

✓ Self-loop

```
1
↑↓
```

✓ Empty list

---

## Interview Insight

Floyd's algorithm is one of the highest-frequency interview topics.

Know:

- Detect cycle
- Find cycle start (LC 142 follow-up)
- Why it works mathematically

---

# Problem 4 — Remove Duplicates from Sorted List

**LeetCode:** 83

**Difficulty:** Easy

**Asked By**

Microsoft • Meta

---

## Problem

Input

```
1→1→2→3→3
```

Output

```
1→2→3
```

---

# Observation

Since list is sorted,

Duplicates are adjacent.

Only compare with next node.

---

# Optimal Approach

Traverse once.

If

```
curr.val == curr.next.val
```

skip node.

Else

move forward.

Diagram

```
1→1→2

↓

1──→2
```

---

## Java

```java
class Solution {

    public ListNode deleteDuplicates(ListNode head) {

        ListNode curr = head;

        while (curr != null && curr.next != null) {

            if (curr.val == curr.next.val) {
                curr.next = curr.next.next;
            } else {
                curr = curr.next;
            }
        }

        return head;
    }
}
```

---

## Complexity

|Metric|Value|
|------|-----|
|Time|O(N)|
|Space|O(1)|

---

## Common Mistakes

❌ Moving current after deletion.

Correct:

```
Delete

Check again

Delete again

Move only when values differ.
```

---

## Edge Cases

- Empty list
- All duplicates

```
1→1→1→1
```

- No duplicates
- One node

---

## Why This Problem Matters

Interviewers evaluate whether you exploit the **sorted property** instead of introducing unnecessary data structures.

It reinforces efficient pointer updates while avoiding skipped duplicate chains.

---

# End of Part 1

**Covered so far**

- Techniques & Algorithms
- LC 206 — Reverse Linked List
- LC 21 — Merge Two Sorted Lists
- LC 141 — Linked List Cycle
- LC 83 — Remove Duplicates from Sorted List

**Next Part**

- LC 203 — Remove Linked List Elements
- LC 19 — Remove Nth Node From End
- LC 24 — Swap Nodes in Pairs
- LC 143 — Reorder List

---

# Problem 5 — Remove Linked List Elements

**LeetCode:** 203

**Difficulty:** Easy

**Asked By**

Amazon • Microsoft • Adobe

---

## Problem

Remove every node whose value equals `val`.

Example

```
Input

1 → 2 → 6 → 3 → 4 → 5 → 6

val = 6

Output

1 → 2 → 3 → 4 → 5
```

---

## Approach 1 — Special Handling for Head

Keep deleting head while it matches.

Then process remaining nodes.

Works, but introduces multiple edge cases.

---

### Complexity

```
Time  : O(N)
Space : O(1)
```

---

## Approach 2 — Dummy Node (Optimal)

A dummy node removes the need to treat the head separately.

```
dummy
  |
  v
0 → 1 → 2 → 6 → 3
```

Whenever the next node should be removed:

```
curr.next = curr.next.next;
```

Otherwise:

```
curr = curr.next;
```

---

### Dry Run

```
dummy → 1 → 2 → 6 → 3

              ^
            remove

↓

dummy → 1 → 2 ─────→ 3
```

---

## Java

```java
class Solution {

    public ListNode removeElements(ListNode head, int val) {

        ListNode dummy = new ListNode(-1);
        dummy.next = head;

        ListNode curr = dummy;

        while (curr.next != null) {

            if (curr.next.val == val) {
                curr.next = curr.next.next;
            } else {
                curr = curr.next;
            }
        }

        return dummy.next;
    }
}
```

---

## Complexity

| Metric | Value |
|--------|-------|
| Time | O(N) |
| Space | O(1) |

---

## Edge Cases

- Empty list
- All nodes removed
- Only head removed
- Only tail removed
- Consecutive nodes removed

---

## Interview Insight

Dummy nodes are arguably the most important linked-list trick.

Once mastered, questions like:

- Remove Nth Node
- Reverse K Group
- Swap Pairs
- Merge Lists

become much simpler.

---

# Problem 6 — Remove Nth Node From End

**LeetCode:** 19

**Difficulty:** Medium

**Asked By**

Google • Meta • Microsoft • Amazon

---

## Problem

Remove the Nth node from the end.

Example

```
1 → 2 → 3 → 4 → 5

n = 2

↓

1 → 2 → 3 → 5
```

---

# Approach 1 — Compute Length

Pass 1

Count length.

Pass 2

Delete

```
(length - n + 1)
```

node.

---

### Complexity

```
Time  : O(N)

Space : O(1)
```

Two traversals.

---

# Approach 2 — Two Pointer Gap (Optimal)

Maintain a gap of `n`.

```
Fast

↓

1→2→3→4→5

↑
Slow
```

Move fast first.

```
Gap = n
```

Then move both.

When fast reaches end,

Slow is before node to remove.

---

### Visualization

Initial

```
Dummy → 1 → 2 → 3 → 4 → 5
```

Gap

```
Fast ------------>

Slow ->
```

End

```
Slow before target
```

Delete

```
slow.next = slow.next.next
```

---

## Java

```java
class Solution {

    public ListNode removeNthFromEnd(ListNode head, int n) {

        ListNode dummy = new ListNode(0);
        dummy.next = head;

        ListNode fast = dummy;
        ListNode slow = dummy;

        for (int i = 0; i <= n; i++) {
            fast = fast.next;
        }

        while (fast != null) {
            fast = fast.next;
            slow = slow.next;
        }

        slow.next = slow.next.next;

        return dummy.next;
    }
}
```

---

## Complexity

| Metric | Value |
|--------|-------|
| Time | O(N) |
| Space | O(1) |

---

## Common Mistakes

### Forgetting Dummy Node

Fails when deleting head.

---

### Wrong Gap

Most candidates use

```
n
```

instead of

```
n + 1
```

when starting from dummy.

---

## Why This Matters

Tests:

- Pointer reasoning
- Off-by-one handling
- Dummy node mastery

One of the highest-frequency medium questions.

---

# Problem 7 — Swap Nodes in Pairs

**LeetCode:** 24

**Difficulty:** Medium

**Asked By**

Amazon • Microsoft • Bloomberg

---

## Problem

Swap adjacent nodes.

Example

```
1 → 2 → 3 → 4

↓

2 → 1 → 4 → 3
```

Do **not** swap values.

Swap nodes.

---

# Approach 1 — Recursive

Swap first pair.

Recursively solve remaining list.

Elegant.

Extra recursion stack.

---

### Complexity

```
Time : O(N)

Space : O(N)
```

---

# Approach 2 — Iterative (Optimal)

Maintain

```
prev
first
second
```

Reconnect pointers.

---

### Visualization

Before

```
prev

↓

1 → 2 → 3 → 4
```

After

```
prev

↓

2 → 1 → 3 → 4
```

Reconnect.

Continue.

---

## Java

```java
class Solution {

    public ListNode swapPairs(ListNode head) {

        ListNode dummy = new ListNode(0);
        dummy.next = head;

        ListNode prev = dummy;

        while (prev.next != null && prev.next.next != null) {

            ListNode first = prev.next;
            ListNode second = first.next;

            first.next = second.next;
            second.next = first;
            prev.next = second;

            prev = first;
        }

        return dummy.next;
    }
}
```

---

## Complexity

| Metric | Value |
|--------|-------|
| Time | O(N) |
| Space | O(1) |

---

## Edge Cases

- Empty list
- One node
- Odd length
- Even length

---

## Interview Insight

Questions like

- Reverse K Group
- Reverse II

become easier after mastering this pointer rewiring.

---

# Problem 8 — Reorder List

**LeetCode:** 143

**Difficulty:** Medium

**Asked By**

Google • Apple • Meta • Amazon

---

## Problem

Given

```
1 → 2 → 3 → 4 → 5
```

Convert into

```
1 → 5 → 2 → 4 → 3
```

Node values cannot be modified.

---

# Approach 1 — Array

Store all nodes.

Use two pointers.

Reconnect.

---

### Complexity

```
Time : O(N)

Space : O(N)
```

---

# Approach 2 — Three-Step In-place Algorithm (Optimal)

This famous interview problem combines three patterns.

### Step 1

Find middle.

```
Fast

Slow
```

---

### Step 2

Reverse second half.

```
1→2→3

5→4
```

---

### Step 3

Merge alternately.

```
1

↓

5

↓

2

↓

4

↓

3
```

---

### Complete Visualization

Original

```
1 → 2 → 3 → 4 → 5
```

Middle

```
1 → 2 → 3

4 → 5
```

Reverse

```
1 → 2 → 3

5 → 4
```

Merge

```
1 → 5 → 2 → 4 → 3
```

---

## Java

```java
class Solution {

    public void reorderList(ListNode head) {

        if (head == null || head.next == null)
            return;

        // Find middle
        ListNode slow = head;
        ListNode fast = head;

        while (fast.next != null && fast.next.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }

        // Reverse second half
        ListNode prev = null;
        ListNode curr = slow.next;
        slow.next = null;

        while (curr != null) {
            ListNode next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }

        // Merge
        ListNode first = head;
        ListNode second = prev;

        while (second != null) {

            ListNode next1 = first.next;
            ListNode next2 = second.next;

            first.next = second;
            second.next = next1;

            first = next1;
            second = next2;
        }
    }
}
```

---

## Complexity

| Metric | Value |
|--------|-------|
| Time | O(N) |
| Space | O(1) |

---

## Why Interviewers Love This Problem

This single question combines multiple linked-list techniques:

- Fast & Slow Pointer
- Reverse Linked List
- Alternate Merge
- Careful Pointer Updates

It evaluates whether you can compose several simpler patterns into one optimal solution.

---

## Common Mistakes

- Forgetting to split the list before reversing.
- Losing the second-half pointer during reversal.
- Infinite loops caused by incorrect merge order.
- Mishandling odd-length lists.

---

# End of Part 2

Completed Problems:

- LC 203 — Remove Linked List Elements
- LC 19 — Remove Nth Node From End
- LC 24 — Swap Nodes in Pairs
- LC 143 — Reorder List

**Next Part (Part 3)**

- LC 2 — Add Two Numbers
- LC 92 — Reverse Linked List II
- LC 138 — Copy List with Random Pointer
- LC 146 — LRU Cache

---

# Problem 9 — Add Two Numbers

**LeetCode:** 2

**Difficulty:** Medium

**Asked By**

Amazon • Meta • Google • Microsoft • Apple

---

## Problem

Two non-empty linked lists represent two non-negative integers in reverse order.

Each node contains one digit.

Return the sum as a linked list.

Example

```
2 → 4 → 3
5 → 6 → 4

342 + 465 = 807

Output

7 → 0 → 8
```

---

## Approach 1 — Convert to Integer

Build integers.

Add them.

Create a new list.

### Why It Fails

Large inputs exceed integer limits.

Not acceptable in interviews.

---

### Complexity

```
Time  : O(N)

Space : O(1)
```

But not scalable.

---

## Approach 2 — Digit-by-Digit Simulation (Optimal)

Exactly mimic manual addition.

Maintain:

- carry
- current digit
- result tail

---

### Visualization

```
Carry = 0

  2 → 4 → 3
+ 5 → 6 → 4
----------------
  7 → 0 → 8
```

Another example

```
9 → 9 → 9

1

↓

0 → 0 → 0 → 1
```

Carry propagates until the end.

---

## Java

```java
class Solution {

    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {

        ListNode dummy = new ListNode(0);
        ListNode tail = dummy;

        int carry = 0;

        while (l1 != null || l2 != null || carry != 0) {

            int sum = carry;

            if (l1 != null) {
                sum += l1.val;
                l1 = l1.next;
            }

            if (l2 != null) {
                sum += l2.val;
                l2 = l2.next;
            }

            tail.next = new ListNode(sum % 10);
            tail = tail.next;

            carry = sum / 10;
        }

        return dummy.next;
    }
}
```

---

## Complexity

| Metric | Value |
|--------|-------|
| Time | O(max(N, M)) |
| Space | O(max(N, M)) *(output list excluded in many interview analyses)* |

---

## Edge Cases

- Different lengths
- Final carry
- One list empty
- Many consecutive carries

---

## Interview Insight

This question checks whether you can:

- Simulate arithmetic
- Handle carries correctly
- Build linked lists dynamically

---

# Problem 10 — Reverse Linked List II

**LeetCode:** 92

**Difficulty:** Medium

**Asked By**

Google • Microsoft • Amazon

---

## Problem

Reverse only the nodes from position `left` to `right`.

Example

```
1 → 2 → 3 → 4 → 5

left = 2

right = 4

↓

1 → 4 → 3 → 2 → 5
```

---

## Approach 1 — Store Nodes

Copy nodes into an array.

Reverse.

Reconnect.

---

### Complexity

```
Time  : O(N)

Space : O(N)
```

---

## Approach 2 — In-place Head Insertion (Optimal)

Move to the node before `left`.

Repeatedly move the next node to the front of the reversed section.

---

### Visualization

Initial

```
1 → 2 → 3 → 4 → 5
    L         R
```

After first iteration

```
1 → 3 → 2 → 4 → 5
```

After second iteration

```
1 → 4 → 3 → 2 → 5
```

No extra memory required.

---

## Java

```java
class Solution {

    public ListNode reverseBetween(ListNode head, int left, int right) {

        if (head == null || left == right)
            return head;

        ListNode dummy = new ListNode(0);
        dummy.next = head;

        ListNode prev = dummy;

        for (int i = 1; i < left; i++) {
            prev = prev.next;
        }

        ListNode curr = prev.next;

        for (int i = 0; i < right - left; i++) {

            ListNode next = curr.next;

            curr.next = next.next;

            next.next = prev.next;

            prev.next = next;
        }

        return dummy.next;
    }
}
```

---

## Complexity

| Metric | Value |
|--------|-------|
| Time | O(N) |
| Space | O(1) |

---

## Common Mistakes

- Forgetting the dummy node when `left = 1`
- Losing the remaining list
- Incorrect loop count (`right - left`)

---

## Why This Matters

Tests:

- Partial reversal
- Pointer manipulation
- Dummy node usage

Frequently appears as a follow-up to LC 206.

---

# Problem 11 — Copy List with Random Pointer

**LeetCode:** 138

**Difficulty:** Medium

**Asked By**

Meta • Microsoft • Google • Amazon

---

## Problem

Each node contains:

```
next
random
```

Create a deep copy.

Example

```
A ----> B ----> C
|       |       |
v       v       v
C       A       B
```

Every copied node must point only to copied nodes.

---

## Approach 1 — HashMap (Interview Friendly)

Create copies.

Store mapping.

```
Original

↓

Clone
```

Second pass:

Assign

```
next

random
```

using the map.

---

### Visualization

```
Original Node

↓

HashMap

↓

Copied Node
```

---

## Java

```java
class Solution {

    public Node copyRandomList(Node head) {

        if (head == null)
            return null;

        HashMap<Node, Node> map = new HashMap<>();

        Node curr = head;

        while (curr != null) {
            map.put(curr, new Node(curr.val));
            curr = curr.next;
        }

        curr = head;

        while (curr != null) {

            map.get(curr).next = map.get(curr.next);
            map.get(curr).random = map.get(curr.random);

            curr = curr.next;
        }

        return map.get(head);
    }
}
```

---

## Approach 2 — O(1) Space Node Interleaving (Optimal)

Three passes.

### Pass 1

Insert clone after every original node.

```
A → A'

↓

B → B'

↓

C → C'
```

---

### Pass 2

Assign random pointers.

```
A.random.next

↓

A'.random
```

---

### Pass 3

Separate the two lists.

---

## Complexity

| Approach | Time | Space |
|-----------|------|--------|
| HashMap | O(N) | O(N) |
| Interleaving | O(N) | O(1) |

---

## Interview Insight

Many interviewers first expect the HashMap solution, then ask:

> Can you do it without extra memory?

Be prepared with the interleaving technique.

---

# Problem 12 — LRU Cache

**LeetCode:** 146

**Difficulty:** Medium

**Asked By**

Google • Amazon • Meta • Microsoft • Apple

---

## Problem

Design an LRU (Least Recently Used) cache supporting:

```
get(key)

put(key, value)
```

Both operations must be:

```
O(1)
```

---

## Key Idea

Two data structures are required.

```
HashMap

+

Doubly Linked List
```

---

### Why Not Only HashMap?

HashMap gives

```
O(1)

lookup
```

But cannot determine the least recently used key.

---

### Why Doubly Linked List?

Keeps usage order.

Most recently used

```
Head
```

Least recently used

```
Tail
```

---

### Structure

```
Head

↓

[A] ⇄ [B] ⇄ [C]

↓

Tail
```

Most recently used

```
Head.next
```

Least recently used

```
Tail.prev
```

---

## Core Operations

### get(key)

- Exists → move node to front
- Return value

### put(key, value)

If exists

- Update value
- Move to front

Else

- Insert at front
- Remove tail if capacity exceeded

---

## Java

```java
class LRUCache {

    class Node {

        int key;
        int value;

        Node prev;
        Node next;

        Node(int k, int v) {
            key = k;
            value = v;
        }
    }

    private final int capacity;

    private final HashMap<Integer, Node> map;

    private final Node head;

    private final Node tail;

    public LRUCache(int capacity) {

        this.capacity = capacity;

        map = new HashMap<>();

        head = new Node(0, 0);
        tail = new Node(0, 0);

        head.next = tail;
        tail.prev = head;
    }

    private void remove(Node node) {

        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    private void insert(Node node) {

        node.next = head.next;
        node.prev = head;

        head.next.prev = node;
        head.next = node;
    }

    public int get(int key) {

        if (!map.containsKey(key))
            return -1;

        Node node = map.get(key);

        remove(node);
        insert(node);

        return node.value;
    }

    public void put(int key, int value) {

        if (map.containsKey(key)) {

            Node node = map.get(key);

            remove(node);

            node.value = value;

            insert(node);

            return;
        }

        if (map.size() == capacity) {

            Node lru = tail.prev;

            remove(lru);

            map.remove(lru.key);
        }

        Node node = new Node(key, value);

        insert(node);

        map.put(key, node);
    }
}
```

---

## Complexity

| Operation | Time |
|-----------|------|
| get | O(1) |
| put | O(1) |

Space

```
O(capacity)
```

---

## Common Mistakes

- Forgetting to move accessed nodes to the front.
- Removing the wrong node during eviction.
- Incorrect DLL pointer updates.
- Using a singly linked list (cannot remove in O(1)).

---

## Interview Insight

This is one of the most frequently asked system-design-style coding problems.

The interviewer is testing whether you can combine:

- HashMap
- Doubly Linked List
- Object-oriented design
- Constant-time operations

---

# End of Part 3

Completed Problems:

- LC 2 — Add Two Numbers
- LC 92 — Reverse Linked List II
- LC 138 — Copy List with Random Pointer
- LC 146 — LRU Cache

**Next Part (Final)**

- LC 25 — Reverse Nodes in k-Group
- LC 23 — Merge k Sorted Lists
- LC 460 — LFU Cache
- LLM-Proof / Tricky Follow-up Questions
- Pattern Summary
- Last-Minute FAANG Interview Cheat Sheet

---

# Problem 13 — Reverse Nodes in k-Group

**LeetCode:** 25

**Difficulty:** Hard

**Asked By**

Google • Apple • Amazon • Microsoft

---

## Problem

Given a linked list, reverse every group of `k` nodes.

If the remaining nodes are fewer than `k`, leave them unchanged.

Example

```
Input

1 → 2 → 3 → 4 → 5

k = 2

Output

2 → 1 → 4 → 3 → 5
```

Another Example

```
Input

1 → 2 → 3 → 4 → 5 → 6

k = 3

Output

3 → 2 → 1 → 6 → 5 → 4
```

---

## Approach 1 — Store in Array

- Copy nodes into an array.
- Reverse every block.
- Reconnect.

### Complexity

```
Time  : O(N)
Space : O(N)
```

Not preferred because the interview expects an in-place solution.

---

## Approach 2 — In-place Group Reversal (Optimal)

Use four pointers:

- dummy
- groupPrev
- kth
- groupNext

---

### Step 1

Locate the kth node.

```
dummy

↓

1 → 2 → 3 → 4 → 5
        ^
       kth
```

---

### Step 2

Reverse the current group.

```
1 → 2 → 3

↓

3 → 2 → 1
```

---

### Step 3

Reconnect.

```
Previous Part

↓

Reversed Group

↓

Remaining List
```

Repeat.

---

## Java

```java
class Solution {

    public ListNode reverseKGroup(ListNode head, int k) {

        ListNode dummy = new ListNode(0);
        dummy.next = head;

        ListNode groupPrev = dummy;

        while (true) {

            ListNode kth = getKth(groupPrev, k);

            if (kth == null)
                break;

            ListNode groupNext = kth.next;

            ListNode prev = groupNext;
            ListNode curr = groupPrev.next;

            while (curr != groupNext) {

                ListNode temp = curr.next;

                curr.next = prev;

                prev = curr;

                curr = temp;
            }

            ListNode temp = groupPrev.next;

            groupPrev.next = kth;

            groupPrev = temp;
        }

        return dummy.next;
    }

    private ListNode getKth(ListNode curr, int k) {

        while (curr != null && k > 0) {
            curr = curr.next;
            k--;
        }

        return curr;
    }
}
```

---

## Complexity

| Metric | Value |
|---------|-------|
| Time | O(N) |
| Space | O(1) |

---

## Common Mistakes

- Reversing incomplete groups.
- Losing `groupNext`.
- Forgetting to reconnect the previous group.

---

## Why Interviewers Love This

This question combines:

- Dummy node
- Group processing
- Pointer reversal
- Careful bookkeeping

If you solve this cleanly, your linked-list fundamentals are generally considered strong.

---

# Problem 14 — Merge k Sorted Lists

**LeetCode:** 23

**Difficulty:** Hard

**Asked By**

Google • Amazon • Meta • Microsoft • Apple

---

## Problem

Merge `k` sorted linked lists into one sorted list.

Example

```
1 → 4 → 5

1 → 3 → 4

2 → 6

↓

1 → 1 → 2 → 3 → 4 → 4 → 5 → 6
```

---

## Approach 1 — Sequential Merge

Merge one list at a time.

```
Merge L1 & L2

↓

Merge Result & L3

↓

Merge Result & L4
```

### Complexity

```
Time

O(NK)
```

where

```
N = total nodes
```

Not optimal.

---

## Approach 2 — Min Heap (Optimal)

Insert the first node of every list into a priority queue.

Always remove the smallest node.

Insert its next node.

---

### Visualization

```
Heap

1
1
2

↓

Remove 1

↓

Insert next node
```

Continue until the heap is empty.

---

## Java

```java
class Solution {

    public ListNode mergeKLists(ListNode[] lists) {

        PriorityQueue<ListNode> pq =
                new PriorityQueue<>((a, b) -> a.val - b.val);

        for (ListNode node : lists) {
            if (node != null)
                pq.offer(node);
        }

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

## Approach 3 — Divide & Conquer

Exactly like Merge Sort.

```
8 Lists

↓

4

↓

2

↓

1
```

Repeatedly merge pairs.

---

### Complexity

| Approach | Time | Space |
|-----------|------|--------|
| Sequential | O(NK) | O(1) |
| Heap | O(N log K) | O(K) |
| Divide & Conquer | O(N log K) | O(log K) recursion |

---

## Interview Insight

A common follow-up is:

> Can you do better than merging one list at a time?

Expected answer:

```
Priority Queue

or

Divide & Conquer
```

---

# Problem 15 — LFU Cache

**LeetCode:** 460

**Difficulty:** Hard

**Asked By**

Google • Meta • Uber

---

## Problem

Design an LFU (Least Frequently Used) cache.

Operations

```
get()

put()
```

must both be

```
O(1)
```

Evict

- lowest frequency
- if tied, least recently used

---

## Core Data Structures

Unlike LRU, one doubly linked list is insufficient.

Need:

```
Key → Node

Frequency → Doubly Linked List

Minimum Frequency
```

---

### Structure

```
Map

↓

Node

↓

Frequency

↓

DLL
```

Example

```
Freq 1

A ⇄ B

Freq 2

C ⇄ D

Freq 5

E
```

---

## High-Level Algorithm

### get(key)

- Lookup node
- Remove from current frequency list
- Increase frequency
- Insert into higher-frequency list

---

### put(key, value)

If key exists

- Update value
- Increase frequency

Else

If full

- Remove LFU node
- Insert new node with frequency 1

Update

```
minFrequency
```

---

## Why It Is Hard

You must maintain

- frequency ordering
- recency ordering
- O(1) lookup
- O(1) updates

simultaneously.

---

## Complexity

| Operation | Time |
|-----------|------|
| get | O(1) |
| put | O(1) |

Space

```
O(capacity)
```

---

## Interview Insight

Interviewers rarely expect a complete implementation from memory.

They evaluate whether you can correctly design:

- Node structure
- Frequency buckets
- HashMap relationships
- Update logic

Being able to explain the design clearly often matters more than writing every line perfectly.

---

# LLM-Proof / Tricky Follow-up Questions

These questions frequently expose weak reasoning or incomplete implementations.

---

## 1. Reverse Nodes in k-Group (LC 25)

### Common Failure

Many solutions reverse the last group even when it contains fewer than `k` nodes.

Example

```
1 → 2 → 3 → 4 → 5

k = 3
```

Correct

```
3 → 2 → 1 → 4 → 5
```

Incorrect

```
3 → 2 → 1 → 5 → 4
```

---

## 2. Copy List with Random Pointer (LC 138)

### Follow-up

Implement without a `HashMap`.

Expected answer:

- Interleave cloned nodes
- Assign random pointers
- Separate the lists

Many candidates know the HashMap solution but cannot derive the O(1) space approach.

---

## 3. LRU Cache

### Follow-up

Why can't a singly linked list be used?

Expected explanation:

Removing an arbitrary node requires its predecessor.

Without backward links, removal becomes O(N), violating the required O(1) complexity.

---

# Pattern Summary

| Pattern | Problems |
|----------|----------|
| Dummy Node | 21, 19, 24, 25, 203 |
| Fast & Slow Pointer | 141, 143 |
| In-place Reversal | 206, 92, 143, 25 |
| Merge | 21, 23 |
| Two-Pointer Gap | 19 |
| Simulation | 2 |
| HashMap | 138, 146, 460 |
| Priority Queue | 23 |
| Doubly Linked List | 146, 460 |

---

# Last-Minute FAANG Interview Cheat Sheet

## Always Consider

- Can a dummy node eliminate edge cases?
- Can two pointers reduce extra space?
- Can the list be reversed in-place?
- Is a HashMap necessary, or can pointers alone solve it?
- Is a doubly linked list required for O(1) deletion?

---

## Pointer Update Rule

Before changing any pointer:

```java
ListNode next = curr.next;
```

Never overwrite `curr.next` before saving it.

---

## High-Frequency Questions

| Priority | Problems |
|----------|----------|
| ⭐⭐⭐⭐⭐ | 206, 21, 141, 19, 143 |
| ⭐⭐⭐⭐ | 2, 92, 24, 138 |
| ⭐⭐⭐ | 23, 25, 146 |
| ⭐⭐ | 203, 83, 460 |

---

## Interview Progression

A common interview sequence is:

```
Reverse Linked List
        ↓
Merge Two Lists
        ↓
Cycle Detection
        ↓
Remove Nth Node
        ↓
Reorder List
        ↓
Reverse k-Group
        ↓
LRU Cache
```

Mastering the earlier problems builds the foundation for the later, more complex ones.

---

# Final Takeaways

1. Master **dummy nodes**—they simplify insertion, deletion, swapping, and reversal.
2. Be comfortable with **fast/slow pointers** for cycle detection and middle-node problems.
3. Practice **pointer rewiring** until you can reverse lists without drawing diagrams.
4. Recognize recurring patterns rather than memorizing solutions.
5. For design questions like **LRU** and **LFU Cache**, focus on choosing the correct data structures before writing code.
6. In interviews, explain your approach, edge cases, and complexity before coding.

---

**End of Linked Lists — Complete FAANG Interview Preparation Guide**


