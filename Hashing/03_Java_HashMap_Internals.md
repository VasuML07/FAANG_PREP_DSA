# 03_Java_HashMap_Internals.md

> **Hashing Interview Guide — Chapter 3**
>
> This chapter explains the internal implementation of Java's `HashMap` in detail. Understanding these internals is one of the highest-value topics for FAANG interviews because it combines data structures, hashing, bit manipulation, object-oriented design, and performance optimization.

---

# Table of Contents

- Introduction
- Evolution of HashMap
- Internal Architecture
- Bucket Array
- Node Structure
- Computing Hash Codes
- Hash Spreading
- Bucket Index Calculation
- Insertion Process (`put()`)
- Search Process (`get()`)
- Deletion Process (`remove()`)
- Collision Resolution
- Separate Chaining
- Treeification (Java 8+)
- Untreeification
- Resize & Rehashing
- Capacity
- Threshold
- Load Factor
- Why Capacity Is a Power of Two
- Fail-Fast Iterators
- Time Complexity
- Memory Layout
- Java Source Code Walkthrough
- Common Interview Questions
- Common Mistakes
- Summary

---

# Introduction

`HashMap<K, V>` is the most commonly used implementation of the `Map` interface in Java.

It provides:

- Average **O(1)** insertion
- Average **O(1)** lookup
- Average **O(1)** deletion

while allowing:

- One `null` key
- Multiple `null` values
- Arbitrary object keys

---

# Evolution of HashMap

| Java Version | Collision Handling |
|--------------|--------------------|
| Java 7 | Linked List |
| Java 8+ | Linked List + Red-Black Tree |
| Java 11+ | Same as Java 8 with optimizations |
| Java 17+ | Minor implementation improvements |

Java 8 introduced **treeification**, significantly improving worst-case lookup performance.

---

# Internal Architecture

A simplified view:

```text
HashMap

│

├── Bucket Array

│      │

│      ├── Node

│      ├── Node

│      ├── TreeNode

│      └── null
│

├── Capacity

├── Size

├── Threshold

└── Load Factor
```

---

# Bucket Array

Internally, `HashMap` stores references inside an array.

```text
table[]

0

1

2

3

4

5

6

7
```

Each position is called a **bucket**.

Each bucket stores:

- nothing
- linked list
- red-black tree

---

# Node Structure

Each entry is represented by a `Node`.

Simplified implementation

```java
static class Node<K,V> {

    final int hash;

    final K key;

    V value;

    Node<K,V> next;

}
```

Fields

| Field | Purpose |
|--------|----------|
| hash | Cached hash value |
| key | Object key |
| value | Stored value |
| next | Next node in linked list |

---

# Example

Suppose

```
map.put("Apple",100)
```

Node

```text
Hash

↓

93281

↓

Key

↓

Apple

↓

Value

↓

100
```

---

# Default Values

```java
HashMap<Integer,String> map = new HashMap<>();
```

Internally

| Property | Default |
|-----------|----------|
| Capacity | 16 |
| Load Factor | 0.75 |
| Threshold | 12 |

Threshold

```
16 × 0.75

=

12
```

---

# Computing Hash Code

Step 1

```java
key.hashCode()
```

Example

```java
"CAT".hashCode()
```

Suppose

```
209381
```

---

# Why Hash Spreading?

Suppose

```text
Hash

↓

101100010101001111
```

If only low bits are used,

many keys may collide.

Java improves distribution using

```java
hash ^ (hash >>> 16)
```

---

# Hash Spreading

Implementation

```java
static final int hash(Object key) {

    int h;

    return (key == null)
            ? 0
            : (h = key.hashCode()) ^ (h >>> 16);
}
```

Purpose

Mix higher bits into lower bits.

Result

- Better bucket distribution
- Fewer collisions

---

# Bucket Index Calculation

Capacity is always

```
Power of Two
```

Instead of

```java
hash % capacity
```

Java computes

```java
index = hash & (capacity - 1);
```

Example

Capacity

```
16
```

Binary

```
10000
```

Mask

```
01111
```

Hash

```
11001010
```

Result

```
01010

=

10
```

Bit masking is much faster than modulo.

---

# Why Capacity Is Power of Two

Suppose

Capacity

```
16
```

Mask

```
15

↓

1111
```

Now

```java
hash & 15
```

works perfectly.

If capacity were

```
15
```

masking would not distribute values correctly.

---

# Insertion Process (`put()`)

Algorithm

```text
Receive Key

↓

Compute hash

↓

Spread bits

↓

Calculate bucket index

↓

Bucket Empty?

↓

Yes

↓

Insert

↓

No

↓

Traverse

↓

Duplicate Key?

↓

Replace Value

↓

Otherwise

↓

Append Node

↓

Check Treeify

↓

Check Resize
```

---

# Example

Insert

```java
map.put(12,"A");
```

Suppose

```
Hash

↓

1248
```

Bucket

```
8
```

Bucket empty

↓

Insert.

---

Second insertion

```java
map.put(28,"B");
```

Suppose same bucket.

Linked list

```text
Bucket 8

↓

12

↓

28
```

---

# Search Process (`get()`)

Algorithm

```text
Receive Key

↓

Hash

↓

Bucket

↓

First Node

↓

Equal?

↓

Yes

↓

Return

↓

Else

↓

Traverse List / Tree
```

Average

```
O(1)
```

---

# Example

```java
map.get(28);
```

Bucket

```
8
```

Compare

```
12

↓

No
```

Next

```
28

↓

Yes
```

Return value.

---

# Deletion Process (`remove()`)

Algorithm

```text
Hash

↓

Bucket

↓

Traverse

↓

Found?

↓

Reconnect Links

↓

Delete
```

Average

```
O(1)
```

---

# Collision Resolution

Java uses

```
Separate Chaining
```

Every bucket stores

```text
Node

↓

Node

↓

Node
```

instead of open addressing.

Advantages

- Simple
- Flexible
- Easy resizing

---

# Example

Three keys

```
15

25

35
```

All hash to

```
Bucket 5
```

Stored as

```text
Bucket 5

↓

15

↓

25

↓

35
```

---

# Treeification (Java 8+)

Problem

Very long linked lists become slow.

Solution

Convert linked list into

```
Red-Black Tree
```

Lookup improves

```
O(n)

↓

O(log n)
```

---

## Treeification Conditions

Tree conversion happens only if

```
Bucket Size >= 8
```

AND

```
Capacity >= 64
```

Otherwise,

Java resizes instead.

---

# Why Not Treeify Immediately?

Suppose capacity

```
16
```

Long chain usually indicates

```
Table Too Small
```

Resizing often removes collisions.

Treeification is expensive.

---

# Untreeification

If entries become small again

```
< 6 Nodes
```

Java converts tree back into linked list.

Reason

Linked lists consume less memory.

---

# Resize Operation

When

```
size > threshold
```

Resize occurs.

Example

Capacity

```
16
```

↓

```
32
```

↓

```
64
```

↓

```
128
```

Always doubles.

---

# Rehashing Process

```text
Old Table

↓

Allocate New Table

↓

Double Capacity

↓

Move Every Node

↓

Recompute Bucket

↓

Done
```

Time

```
O(n)
```

---

# Capacity

Capacity

=

Number of buckets.

Default

```
16
```

Growth

```
16

↓

32

↓

64

↓

128

↓

256
```

---

# Load Factor

Formula

```
Elements / Capacity
```

Default

```
0.75
```

Meaning

75% full before resize.

---

# Threshold

Formula

```
Capacity × Load Factor
```

Example

| Capacity | Threshold |
|-----------|------------|
| 16 | 12 |
| 32 | 24 |
| 64 | 48 |
| 128 | 96 |

---

# Why Load Factor Is 0.75

Trade-off

Low load factor

- Faster lookup
- More memory

High load factor

- Less memory
- More collisions

0.75 gives the best balance for most workloads.

---

# Memory Layout

Each node stores

- hash
- key reference
- value reference
- next reference

Approximate layout

```text
Node

┌────────────┐

│ hash       │

├────────────┤

│ key ref    │

├────────────┤

│ value ref  │

├────────────┤

│ next ref   │

└────────────┘
```

Large `HashMap`s can consume significant memory because every entry is an object.

---

# Fail-Fast Iterators

Example

```java
for(Integer x : map.keySet()){

    map.put(10,"A");

}
```

Runtime

```
ConcurrentModificationException
```

Reason

Iterator detects structural modification.

---

# Safe Removal

Correct

```java
Iterator<Integer> it = map.keySet().iterator();

while(it.hasNext()){

    Integer x = it.next();

    if(x % 2 == 0)
        it.remove();

}
```

---

# Time Complexity

| Operation | Average | Worst |
|-----------|---------|--------|
| put() | O(1) | O(n) |
| get() | O(1) | O(n) |
| remove() | O(1) | O(n) |
| Tree Lookup | O(log n) | O(log n) |
| Resize | O(n) | O(n) |

---

# Internal Workflow

```text
put(key,value)

↓

hashCode()

↓

Spread Hash

↓

Bucket Index

↓

Bucket Empty?

├── Yes → Insert
│
└── No
      │
      ├── Same Key → Replace
      │
      └── Collision
              │
              ├── Linked List
              │
              └── Tree (if ≥8 nodes)
```

---

# Simplified `put()` Pseudocode

```java
put(key, value):

hash = spread(key.hashCode())

index = hash & (capacity - 1)

if bucket empty
    insert

else

    search bucket

    if key exists
        replace value

    else
        append node

if size > threshold
    resize()
```

---

# Common Interview Questions

## Q1. Why does Java cache the hash value inside each node?

To avoid recomputing `hashCode()` repeatedly during lookups, collision traversal, and resizing.

---

## Q2. Why does `HashMap` use linked lists instead of arrays inside buckets?

Linked lists allow efficient insertion without shifting elements and support dynamic bucket sizes.

---

## Q3. Why convert to a Red-Black Tree after 8 nodes?

Long linked lists degrade lookup to `O(n)`. A balanced tree improves it to `O(log n)`.

---

## Q4. Why is the minimum capacity for treeification 64?

If the table is still small, resizing usually distributes entries better and avoids the overhead of maintaining a tree.

---

## Q5. Why use `hash ^ (hash >>> 16)`?

To mix high-order bits into low-order bits, improving bucket distribution when only the lower bits determine the index.

---

## Q6. Does `HashMap` preserve insertion order?

No.

Use:

- `LinkedHashMap` → insertion/access order
- `TreeMap` → sorted order

---

## Q7. Is `HashMap` thread-safe?

No.

Concurrent modifications without synchronization can lead to data races and inconsistent state.

Use `ConcurrentHashMap` for concurrent access.

---

# Common Mistakes

- Assuming `HashMap` is always `O(1)`.
- Forgetting that mutable keys can break lookups.
- Overriding `equals()` without `hashCode()`.
- Ignoring resize costs in performance-critical code.
- Using `HashMap` where ordering is required.

---

# Key Interview Facts

| Topic | Value |
|--------|-------|
| Default Capacity | 16 |
| Default Load Factor | 0.75 |
| Default Threshold | 12 |
| Treeify Threshold | 8 |
| Untreeify Threshold | 6 |
| Minimum Capacity for Treeify | 64 |
| Collision Handling | Separate Chaining |
| Java 8 Improvement | Red-Black Tree |
| Null Keys Allowed | 1 |
| Null Values Allowed | Multiple |

---

# Summary

`HashMap` achieves near-constant-time performance through a combination of:

- Efficient hash functions
- Bit spreading
- Power-of-two bucket arrays
- Separate chaining
- Red-Black tree conversion
- Dynamic resizing

Understanding these implementation details is frequently tested in Java interviews and provides the foundation for reasoning about performance, correctness, and advanced hashing problems.

---

# What's Next

The next chapter covers **HashSet Internals**, including:

- Relationship with `HashMap`
- Internal implementation
- Duplicate detection
- Equality semantics
- Performance characteristics
- Memory usage
- Iteration behavior
- Practical interview scenarios
- Common pitfalls
- Comparison with `TreeSet` and `LinkedHashSet`
