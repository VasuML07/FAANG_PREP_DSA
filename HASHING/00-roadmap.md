# Hashing Interview Mastery Guide (Java)
> **00-roadmap.md**
>
> **FAANG • GATE • SDE Interviews • LeetCode Mastery**
>
> This roadmap is the entry point of the complete Hashing Interview Guide. It provides the learning order, topic hierarchy, LeetCode roadmap, interview progression, and study strategy before diving into individual problems.

---

# Table of Contents

- [1. About this Guide](#1-about-this-guide)
- [2. Who is this Guide For?](#2-who-is-this-guide-for)
- [3. Prerequisites](#3-prerequisites)
- [4. Why Hashing is One of the Most Important Interview Topics](#4-why-hashing-is-one-of-the-most-important-interview-topics)
- [5. Complete Learning Roadmap](#5-complete-learning-roadmap)
- [6. Problem Solving Progression](#6-problem-solving-progression)
- [7. Complete Pattern Roadmap](#7-complete-pattern-roadmap)
- [8. Difficulty-wise Learning Order](#8-difficulty-wise-learning-order)
- [9. Company-wise Focus Areas](#9-company-wise-focus-areas)
- [10. Recommended Study Schedule](#10-recommended-study-schedule)
- [11. Interview Readiness Checklist](#11-interview-readiness-checklist)
- [12. What Comes Next](#12-what-comes-next)

---

# 1. About this Guide

This repository is designed to make you **master Hashing for coding interviews**.

Unlike traditional DSA notes, this guide is:

- Problem-driven
- Interview-focused
- Java-specific
- Production-quality
- FAANG-oriented

Instead of learning theory first and problems later, every important concept is introduced naturally through actual LeetCode questions.

The philosophy is simple:

```
Problem
      ↓
Observation
      ↓
Hashing Pattern
      ↓
Optimized Solution
      ↓
Interview Tricks
      ↓
Related Problems
```

Every topic builds upon previously learned patterns.

---

# 2. Who is this Guide For?

This guide is suitable for:

- Beginners learning HashMap
- Intermediate Java developers
- LeetCode enthusiasts
- FAANG aspirants
- Amazon SDE Interviews
- Google SWE Interviews
- Meta Coding Interviews
- Microsoft Coding Interviews
- Apple Coding Interviews
- Uber
- Netflix
- Adobe
- Atlassian
- Walmart Global Tech
- Goldman Sachs
- JP Morgan
- Oracle
- Service-based company interviews

---

# 3. Prerequisites

Before starting this guide, you should know:

- Java syntax
- Arrays
- Strings
- Functions
- Classes
- Loops
- Time Complexity
- Big-O notation

Helpful but optional:

- Sliding Window
- Two Pointers
- Binary Search
- Recursion

---

# 4. Why Hashing is One of the Most Important Interview Topics

Hashing appears in almost every major company's interview process because it transforms many quadratic solutions into linear-time solutions.

Typical optimization:

```
Brute Force

O(N²)

↓

Store previous information inside a HashMap

↓

O(N)
```

Example:

```
Two Sum

Nested Loops

↓

HashMap

↓

O(N²)

↓

O(N)
```

---

Another common transformation:

```
Frequency Counting

Without HashMap

↓

Repeated Searching

↓

O(N²)

↓

HashMap Frequency

↓

O(N)
```

---

Substring problems:

```
Without Hashing

Every substring checked

↓

O(N²)

↓

Sliding Window + HashMap

↓

O(N)
```

---

Subarray problems:

```
Without Prefix Hashing

O(N²)

↓

Prefix Sum + HashMap

↓

O(N)
```

---

Graphs:

```
Adjacency Matrix

↓

Large Memory

↓

HashMap Graph

↓

Sparse Efficient Graph
```

---

Hashing is not just a data structure.

It is an optimization technique.

---

# 5. Complete Learning Roadmap

```text
                     HASHING
                         │
 ┌───────────────────────┼────────────────────────┐
 │                       │                        │
 │                       │                        │
Basic Operations     Frequency Count       Pair Problems
 │                       │                        │
 │                       │                        │
HashSet            Character Count         Two Sum
HashMap            Duplicate Count         Pair Difference
Contains           Anagram                 Pair Products
Remove             Grouping                Pair XOR
Insert
 │
 │
 ▼
Sliding Window
 │
Longest Substring
Minimum Window
Permutation
Character Replacement
Repeated DNA

 │
 ▼
Prefix Sum
 │
Subarray Sum
Binary Array
Modulo Problems
Equal 0 and 1
Continuous Sum

 │
 ▼
Advanced Hashing
 │
Rolling Hash
Rabin Karp
Polynomial Hash
Encoding
Hash Compression

 │
 ▼
Design Problems
 │
LRU Cache
LFU Cache
TimeMap
RandomizedSet

 │
 ▼
Graph Hashing
 │
Word Ladder
Alien Dictionary
Clone Graph
BFS State Compression

 │
 ▼
Hard Interview Questions
```

---

# 6. Problem Solving Progression

Always solve problems in this order.

```
Easy

↓

Medium

↓

Hard

↓

Mixed Revision

↓

Company Sheets

↓

Mock Interviews
```

Never jump directly to Hard questions.

The interview expects pattern recognition.

Patterns come from repetition.

---

## Stage 1

Goal:

Understand HashMap operations.

Problems include:

- Two Sum
- Contains Duplicate
- Valid Anagram
- Happy Number
- Ransom Note
- Jewels and Stones

Focus:

- put()
- get()
- containsKey()
- contains()
- remove()

---

## Stage 2

Goal:

Frequency counting.

Problems include:

- Top K Frequent
- Sort Characters by Frequency
- Group Anagrams
- Find Common Characters
- Isomorphic Strings

Focus:

```
Character

↓

Frequency

↓

Decision
```

---

## Stage 3

Goal:

Sliding Window + HashMap.

Problems include:

- Longest Substring
- Minimum Window
- Character Replacement
- Permutation in String
- Find All Anagrams

---

## Stage 4

Goal:

Prefix Sum + HashMap.

Problems:

- Subarray Sum Equals K
- Continuous Subarray Sum
- Binary Array
- Equal Zero and One

---

## Stage 5

Goal:

Design.

Problems:

- LRU Cache
- LFU Cache
- Time Based Key Value Store
- Randomized Set

---

## Stage 6

Goal:

Graph Hashing.

Problems:

- Word Ladder
- Clone Graph
- Alien Dictionary
- Evaluate Division

---

## Stage 7

Goal:

Advanced String Hashing.

Problems:

- Rabin Karp
- Rolling Hash
- Longest Duplicate Substring
- Repeated DNA Sequences

---

# 7. Complete Pattern Roadmap

| Pattern | Difficulty | Importance | Interview Frequency |
|----------|-----------|-------------|---------------------|
| HashSet Basics | Easy | ★★★★★ | Very High |
| HashMap Basics | Easy | ★★★★★ | Very High |
| Frequency Counting | Easy | ★★★★★ | Very High |
| Pair Finding | Easy | ★★★★★ | Very High |
| Character Mapping | Easy | ★★★★★ | High |
| Sliding Window + HashMap | Medium | ★★★★★ | Very High |
| Prefix Sum + HashMap | Medium | ★★★★★ | Very High |
| Grouping | Medium | ★★★★★ | High |
| Custom Ordering | Medium | ★★★★☆ | Medium |
| Design Problems | Medium | ★★★★★ | High |
| Rolling Hash | Hard | ★★★★☆ | Medium |
| Graph Hashing | Hard | ★★★★★ | High |
| String Encoding | Hard | ★★★★☆ | Medium |
| Polynomial Hash | Hard | ★★★★☆ | Medium |
| State Compression | Hard | ★★★★☆ | Medium |

---

# 8. Difficulty-wise Learning Order

## Easy

Learn:

- HashMap
- HashSet
- Counting
- Presence Checking
- Pair Search

Target:

**20–30 problems**

---

## Medium

Learn:

- Sliding Window
- Prefix Sum
- Grouping
- Cache Design
- String Mapping

Target:

**40–60 problems**

---

## Hard

Learn:

- Rolling Hash
- LFU Cache
- Word Ladder
- Advanced Graph Hashing
- Polynomial Hashing

Target:

**20–30 problems**

---

Overall target:

```
70–120 Hashing Problems
```

This is sufficient for nearly all software engineering interviews.

---

# 9. Company-wise Focus Areas

| Company | Most Asked Hashing Topics |
|----------|---------------------------|
| Google | Rolling Hash, String Hashing, Graph Hashing |
| Meta | Sliding Window, Frequency Maps |
| Amazon | Two Sum, Prefix Sum, Design Problems |
| Microsoft | HashMap + Strings |
| Apple | Cache Design, Character Mapping |
| Uber | Prefix Sum + HashMap |
| Netflix | Design + Graph Problems |
| Adobe | Frequency Counting |
| Atlassian | HashMap + Design |
| Goldman Sachs | Arrays + Hashing |

---

# 10. Recommended Study Schedule

| Week | Goal |
|------|------|
| Week 1 | HashMap Basics + Easy Problems |
| Week 2 | Frequency Counting |
| Week 3 | Sliding Window |
| Week 4 | Prefix Sum |
| Week 5 | Design Problems |
| Week 6 | Graph Hashing |
| Week 7 | Rolling Hash |
| Week 8 | Revision + Mock Interviews |

---

# 11. Interview Readiness Checklist

Before interviews, verify that you can:

- Solve Two Sum in under 2 minutes.
- Recognize when HashSet is sufficient.
- Choose HashMap over arrays appropriately.
- Identify frequency-counting problems immediately.
- Convert O(N²) solutions into O(N).
- Apply Prefix Sum + HashMap correctly.
- Use Sliding Window with HashMap confidently.
- Design an LRU Cache from scratch.
- Explain Java HashMap internals at a high level.
- Analyze time and space complexity without assistance.

---

# 12. What Comes Next

The remainder of this guide is organized as follows:

```text
01. Easy Problems
│
├── Two Sum
├── Contains Duplicate
├── Valid Anagram
├── Happy Number
├── Isomorphic Strings
├── Ransom Note
├── Intersection of Arrays
└── ...

02. Medium Problems
│
├── Group Anagrams
├── Top K Frequent Elements
├── Longest Substring Without Repeating Characters
├── Subarray Sum Equals K
├── LRU Cache
├── Word Pattern
└── ...

03. Hard Problems
│
├── LFU Cache
├── Word Ladder
├── Minimum Window Substring
├── Substring with Concatenation of All Words
├── Longest Duplicate Substring
└── ...

04. Company-wise Questions

05. Advanced Hashing

06. LLM-Proof Interview Questions

07. Revision Sheets

08. Complete Cheat Sheet
```

---

> **Next File:** `01-easy/01-two-sum.md` — the first problem-driven chapter begins with **LeetCode #1: Two Sum**, introducing `HashMap` through brute-force optimization, execution traces, interview variations, and production-grade Java implementations.
