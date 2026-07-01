# 01_Fundamentals.md

> **Hashing Interview Guide — Chapter 1**
>
> This chapter builds the theoretical foundation required for mastering hashing for coding interviews, competitive programming, system design, and Java development.

---

# Table of Contents

- What is Hashing?
- Why Hashing Matters
- Real-World Analogy
- Mathematical Definition
- Components of Hashing
- Hash Functions
- Characteristics of a Good Hash Function
- Terminology
- How Hash Tables Work
- Step-by-Step Example
- Insert Operation
- Search Operation
- Delete Operation
- Collision
- Why Collisions Occur
- Collision Resolution Overview
- Buckets
- Load Factor
- Rehashing
- Time Complexity Analysis
- Space Complexity Analysis
- Advantages
- Disadvantages
- Common Interview Misconceptions
- Java Perspective
- Applications of Hashing
- Summary
- Interview Takeaways

---

# What is Hashing?

Hashing is a technique that converts a piece of data (called a **key**) into a fixed-size integer called a **hash code** or **hash value**. This hash value determines where the data should be stored inside a hash table.

Instead of searching through every element one by one, hashing computes the storage location directly.

This makes searching, insertion, and deletion extremely fast.

---

## Example

Instead of searching through:

```
Apple
Banana
Orange
Mango
Grapes
```

Suppose

```
hash("Apple") = 4
```

Then we immediately jump to bucket **4**.

No linear search required.

---

# Why Hashing Matters

Hashing is one of the most important interview topics because many seemingly unrelated problems become simple once you recognize that they require constant-time lookup.

Examples:

- Two Sum
- Group Anagrams
- Valid Anagram
- Longest Consecutive Sequence
- Subarray Sum Equals K
- Word Pattern
- Isomorphic Strings
- Happy Number
- LRU Cache
- LFU Cache

Almost every FAANG interview contains at least one hashing problem.

---

# Real-World Analogy

Imagine a library.

Without hashing:

```
Need Book

↓

Walk shelf by shelf

↓

Eventually find it
```

Time:

```
O(n)
```

With hashing:

```
Need Book

↓

Computer generates shelf number

↓

Go directly

↓

Book found
```

Time:

```
Nearly O(1)
```

---

# Mathematical Definition

A hash function maps

```
Key

↓

Hash Function

↓

Integer
```

Formally

```
h(key) = index
```

Example

```
h(57)

↓

57 % 10

↓

7
```

Store inside bucket 7.

---

# Components of Hashing

```
Key
↓

Hash Function

↓

Hash Code

↓

Bucket Index

↓

Hash Table
```

Every hashing-based data structure follows this flow.

---

# Key

The value used to identify an object.

Examples

```
Username

Email

Roll Number

Product ID

Integer

Character

String
```

---

# Value

The associated information stored with the key.

Example

```
Key      Value

101      Alice

102      Bob

103      Charlie
```

---

# Bucket

A location inside the hash table.

```
Bucket 0

Bucket 1

Bucket 2

Bucket 3
```

Each bucket stores one or more entries.

---

# Hash Table

A hash table is an array of buckets.

Example

```
Index

0

1

2

3

4

5
```

Each bucket can contain

- one element
- multiple elements
- linked list
- balanced tree

depending on implementation.

---

# Hash Function

A hash function converts data into an integer.

Example

```
"CAT"

↓

hash()

↓

4829138

↓

4829138 % 16

↓

2
```

Store at bucket 2.

---

# Characteristics of a Good Hash Function

A good hash function should satisfy:

## 1. Deterministic

Same key

↓

Same output

Always.

---

## 2. Fast

Computing the hash should be very quick.

Usually

```
O(length of key)
```

For integers,

```
O(1)
```

---

## 3. Uniform Distribution

Keys should spread evenly.

Good

```
0 ███

1 ███

2 ███

3 ███

4 ███
```

Bad

```
0 ███████████████

1

2

3

4
```

---

## 4. Low Collision Rate

Different keys should rarely produce identical hashes.

---

## 5. Avalanche Effect

Tiny input change

↓

Huge output difference.

Example

```
apple

↓

123456
```

```
applf

↓

987324987
```

---

# Terminology

| Term | Meaning |
|------|----------|
| Key | Data to identify entry |
| Value | Stored information |
| Hash Function | Converts key into integer |
| Bucket | Storage location |
| Collision | Two keys share same bucket |
| Hash Code | Integer produced by hash function |
| Rehashing | Resize and rebuild table |
| Load Factor | Measure of fullness |

---

# How Hash Tables Work

Suppose table size

```
10
```

Hash function

```
key % 10
```

Insert

```
23

↓

23 % 10

↓

3

↓

Store at bucket 3
```

Insert

```
14

↓

4

↓

Store at bucket 4
```

Insert

```
33

↓

3
```

Collision occurs.

---

# Step-by-Step Example

Insert

```
10

↓

0
```

```
Bucket

0 → 10
```

Insert

```
20

↓

0
```

Collision.

Store

```
0

↓

10

↓

20
```

---

# Insert Operation

```
Receive key

↓

Compute hash

↓

Find bucket

↓

Insert
```

Average complexity

```
O(1)
```

Worst

```
O(n)
```

---

# Search Operation

```
Receive key

↓

Hash

↓

Bucket

↓

Compare elements

↓

Found
```

Average

```
O(1)
```

---

# Delete Operation

```
Hash

↓

Locate bucket

↓

Find element

↓

Remove
```

Average

```
O(1)
```

---

# Collision

Collision occurs when multiple keys map to the same bucket.

Example

```
Table Size = 10

15 % 10 = 5

25 % 10 = 5

35 % 10 = 5
```

All collide.

---

# Why Collisions Occur

Reasons include:

- Finite table size
- Infinite possible keys
- Imperfect hash functions
- Similar input values

Collisions are **inevitable** in practical systems.

---

# Collision Resolution Overview

Major strategies:

| Strategy | Idea |
|-----------|------|
| Separate Chaining | Store multiple entries in bucket |
| Linear Probing | Search next empty slot |
| Quadratic Probing | Jump quadratically |
| Double Hashing | Use second hash function |

Each is discussed in detail in later chapters.

---

# Buckets

Visual representation

```
Bucket 0

↓

10

↓

20

↓

30
```

Each bucket may hold multiple entries depending on implementation.

---

# Load Factor

The load factor measures how full a hash table is.

Formula

```
Load Factor = Number of Elements / Number of Buckets
```

Example

```
Buckets = 16

Elements = 12

Load Factor = 12 / 16

= 0.75
```

---

## Why It Matters

Low load factor:

- Faster lookups
- More memory usage

High load factor:

- More collisions
- Slower operations
- Better memory utilization

Java's `HashMap` uses a default load factor of **0.75**, balancing performance and memory.

---

# Rehashing

When the load factor exceeds a threshold, the hash table is resized.

Typical process:

```text
Capacity = 16

↓

Load Factor > 0.75

↓

Resize to 32

↓

Recompute bucket index for every key

↓

Insert into new table
```

Rehashing is expensive (`O(n)`), but happens infrequently, so average operation cost remains `O(1)`.

---

# Time Complexity Analysis

| Operation | Average | Worst |
|-----------|---------|--------|
| Insert | O(1) | O(n) |
| Search | O(1) | O(n) |
| Delete | O(1) | O(n) |
| Resize | — | O(n) |

---

## Why Average Is O(1)

Assuming:

- Good hash function
- Uniform distribution
- Controlled load factor

Each bucket contains only a few elements.

---

## Why Worst Case Is O(n)

If every key hashes to the same bucket:

```text
Bucket

↓

A

↓

B

↓

C

↓

D

↓

E
```

Searching becomes a linear scan.

---

# Space Complexity Analysis

For `n` elements:

| Structure | Space |
|-----------|-------|
| Hash Table | O(n) |
| Buckets | O(n) |
| Total | O(n) |

Additional overhead comes from:

- Bucket array
- Node objects
- References
- Resizing

---

# Advantages

- Very fast average lookup
- Efficient insertion and deletion
- Supports arbitrary keys
- Scales well
- Foundation for many algorithms
- Widely used in databases, compilers, caches, and distributed systems

---

# Disadvantages

- Extra memory overhead
- Collisions are unavoidable
- Performance depends on hash quality
- Worst-case lookup is linear
- Rehashing can be expensive

---

# Common Interview Misconceptions

### "HashMap is always O(1)"

Incorrect.

Worst-case performance can be `O(n)` (or `O(log n)` in modern Java when buckets become balanced trees).

---

### "No collisions means perfect hashing"

Perfect hashing is only practical in specialized scenarios with known key sets.

General-purpose hash tables always plan for collisions.

---

### "Hashing keeps data sorted"

False.

Hash tables do **not** maintain ordering.

Use structures like `TreeMap` when sorted order is required.

---

### "Hash functions must be unique"

False.

Different keys can produce the same hash code.

Correct implementations handle collisions safely.

---

# Java Perspective

In Java:

- `HashMap`
- `HashSet`
- `LinkedHashMap`
- `LinkedHashSet`
- `Hashtable`
- `WeakHashMap`
- `IdentityHashMap`
- `ConcurrentHashMap`

all rely on hashing.

The next chapters explore their internal implementations, performance characteristics, and interview implications.

---

# Applications of Hashing

Hashing appears in many domains beyond interview problems.

## Data Structures

- HashMap
- HashSet
- Dictionary
- Symbol Table

## Databases

- Hash indexes
- Hash joins

## Networking

- Routing tables
- Packet deduplication

## Security

- Password hashing
- Digital signatures
- Checksums

## Distributed Systems

- Consistent hashing
- Distributed caches

## Compilers

- Symbol lookup
- Identifier tables

## Machine Learning

- Feature hashing
- Sparse vector encoding

---

# Summary

Hashing provides near constant-time lookup by mapping keys to buckets using a hash function.

Its efficiency depends on:

- A well-designed hash function
- Low collision rates
- Appropriate load factor
- Efficient collision resolution
- Periodic rehashing

Mastering these fundamentals is essential before studying Java's `HashMap` internals and advanced hashing patterns.

---

# Interview Takeaways

- Explain hashing without using implementation details first.
- Always distinguish **average-case** from **worst-case** complexity.
- Understand why collisions are inevitable.
- Know the purpose of buckets, load factor, and rehashing.
- Recognize that `HashMap` performance depends on hash distribution.
- Be able to explain when hashing is preferable to arrays, linked lists, trees, or sorting.

---

# What's Next

The next chapter covers **Hash Functions** in depth, including:

- Division method
- Multiplication method
- Mid-square hashing
- Folding method
- Universal hashing
- Cryptographic hash functions
- Java `hashCode()`
- Designing good hash functions
- Collision probability
- Practical interview questions
- Java implementation details
```
