# 02_Hash_Functions.md

> **Hashing Interview Guide — Chapter 2**
>
> This chapter covers hash functions in depth. Understanding how hash functions work is essential for explaining the performance of hash tables, Java's `HashMap`, and solving hashing-based interview questions.

---

# Table of Contents

- What is a Hash Function?
- Why Hash Functions Matter
- Properties of a Good Hash Function
- Deterministic Nature
- Uniform Distribution
- Fast Computation
- Avalanche Effect
- Collision Resistance
- Types of Hash Functions
- Integer Hashing
- String Hashing
- Polynomial Rolling Hash
- Division Method
- Multiplication Method
- Mid-Square Method
- Folding Method
- Universal Hashing
- Cryptographic Hash Functions
- Java `hashCode()`
- Why `equals()` and `hashCode()` Must Agree
- Negative Hash Codes
- Hash Compression
- Collision Probability
- Birthday Paradox
- Designing Good Custom Hash Functions
- Common Mistakes
- Interview Questions
- Summary

---

# What is a Hash Function?

A **hash function** converts a key into an integer that determines where the key should be stored inside a hash table.

General form:

```text
Key
 ↓
Hash Function
 ↓
Hash Code
 ↓
Bucket Index
```

Example:

```text
Key = 57

hash(57)

↓

57

↓

57 % 10

↓

Bucket 7
```

For strings:

```text
"apple"

↓

hash()

↓

93029210

↓

93029210 % 16

↓

Bucket 10
```

---

# Why Hash Functions Matter

The efficiency of a hash table depends almost entirely on the quality of its hash function.

A poor hash function leads to:

- Many collisions
- Long chains
- Slow lookups
- Poor scalability

A good hash function leads to:

- Even distribution
- Fewer collisions
- Faster operations
- Better cache utilization

---

# Properties of a Good Hash Function

A high-quality hash function should satisfy five major properties.

---

## 1. Deterministic

The same input must always produce the same output.

Example

```text
hash("apple")

↓

492381

Every time.
```

If this property fails, searching becomes impossible.

---

## 2. Uniform Distribution

Keys should spread evenly across all buckets.

Ideal distribution

```text
Bucket

0 ███

1 ███

2 ███

3 ███

4 ███

5 ███
```

Bad distribution

```text
Bucket

0 █████████████

1

2

3

4

5
```

Uniform distribution minimizes collisions.

---

## 3. Fast Computation

Computing the hash should take much less time than searching.

Typical complexity

| Key Type | Complexity |
|-----------|------------|
| Integer | O(1) |
| Long | O(1) |
| Character | O(1) |
| String | O(length) |
| Object | Depends on fields |

---

## 4. Avalanche Effect

Changing one bit of input should drastically change the output.

Example

```text
Input

apple

↓

Hash

193483748
```

Small change

```text
applf

↓

Hash

928347122
```

Notice how almost every digit changes.

---

## 5. Low Collision Rate

Different keys should rarely produce identical hashes.

Good

```text
A → 2

B → 5

C → 8
```

Poor

```text
A → 1

B → 1

C → 1
```

---

# Important Terminology

| Term | Meaning |
|------|----------|
| Hash Function | Converts key into integer |
| Hash Code | Integer returned |
| Compression Function | Converts hash code into bucket index |
| Bucket | Storage location |
| Collision | Two keys map to same bucket |

---

# Types of Hash Functions

Interview questions commonly involve these categories.

```text
Hash Functions

├── Integer
├── String
├── Rolling
├── Universal
├── Cryptographic
└── Custom Object
```

---

# Integer Hashing

Simplest form.

```java
int hash(int key) {
    return key % tableSize;
}
```

Example

```text
Table Size = 10

21 → 1

31 → 1

41 → 1
```

Many collisions occur if keys follow patterns.

---

# Division Method

Formula

```text
h(k) = k mod m
```

where

- k = key
- m = table size

Example

```text
m = 13

26 % 13 = 0

27 % 13 = 1

44 % 13 = 5
```

Advantages

- Very simple
- Fast
- Most widely used

Disadvantages

- Bad if table size chosen poorly

---

## Choosing Table Size

Interview rule:

Avoid

```text
16

32

64

128
```

Prefer

```text
17

31

61

127
```

Prime numbers reduce clustering.

---

# Multiplication Method

Formula

```text
h(k)= floor(m × (kA mod 1))
```

where

```text
0 < A < 1
```

Popular value

```text
A = 0.618033
```

Advantages

- Better distribution
- Less dependent on table size

---

# Mid-Square Method

Algorithm

```text
Square number

↓

Take middle digits

↓

Bucket
```

Example

```text
45²

↓

2025

↓

20

↓

Bucket 20
```

Rarely used today but common in interview theory.

---

# Folding Method

Large numbers are divided into groups.

Example

```text
123456789

↓

123

456

789

↓

123+456+789

↓

1368
```

Useful for very large numeric keys.

---

# String Hashing

Strings cannot be hashed using simple modulo.

Instead, characters are combined mathematically.

Example

```text
DOG

↓

D
O
G

↓

68
79
71

↓

Combined Hash
```

---

# Polynomial Rolling Hash

The most common interview string hash.

Formula

```text
Hash

=

Σ

(character × power)
```

More formally

```text
H(s)

=

(s₀ × p⁰)

+

(s₁ × p¹)

+

...
```

Common choices

```text
Base = 31

or

Base = 37

or

Base = 53
```

Modulus

```text
1,000,000,007

or

1,000,000,009
```

---

## Java Implementation

```java
public long computeHash(String s) {
    long hash = 0;
    long p = 31;
    long power = 1;
    long mod = 1_000_000_007;

    for (char c : s.toCharArray()) {
        hash = (hash + (c - 'a' + 1) * power) % mod;
        power = (power * p) % mod;
    }

    return hash;
}
```

Complexity

```
Time : O(n)

Space : O(1)
```

---

# Why Rolling Hash Matters

Rolling hash allows efficient substring hashing.

Without rolling hash

```
O(n²)
```

With rolling hash

```
O(n)
```

Applications

- Rabin-Karp
- Duplicate substring
- DNA sequence
- Longest duplicate substring

---

# Universal Hashing

Instead of fixed hash functions,

pick one randomly.

Purpose

Reduce adversarial collisions.

Mostly used in

- Databases
- Distributed systems
- Research

Rare in coding interviews.

---

# Cryptographic Hash Functions

Designed for security.

Examples

| Algorithm | Output |
|-----------|--------|
| MD5 | 128-bit |
| SHA-1 | 160-bit |
| SHA-256 | 256-bit |
| SHA-512 | 512-bit |

Properties

- One-way
- Collision resistant
- Avalanche effect
- Slow compared to HashMap hashing

Interview note

Cryptographic hashes are **not** used for Java `HashMap`.

---

# Java `hashCode()`

Every Java object has

```java
hashCode()
```

Example

```java
String s = "apple";

System.out.println(s.hashCode());
```

Output

```
93029210
```

The exact value is deterministic.

---

# Java String Hash Algorithm

Java computes

```text
s[0]×31^(n−1)

+

s[1]×31^(n−2)

+

...
```

Equivalent iterative implementation

```java
int hash = 0;

for (char c : chars)
    hash = 31 * hash + c;
```

Why 31?

- Prime number
- Efficient multiplication
- Good distribution
- Can optimize as

```text
31x

=

32x−x
```

making multiplication faster on many architectures.

---

# Why `equals()` and `hashCode()` Must Agree

Rule

If

```java
a.equals(b)
```

returns true,

then

```java
a.hashCode() == b.hashCode()
```

must also be true.

Otherwise,

`HashMap` breaks.

---

## Wrong Example

```java
class Student {

    int id;

    @Override
    public boolean equals(Object obj) {
        return true;
    }
}
```

No `hashCode()` override.

This violates the contract.

---

## Correct Example

```java
class Student {

    int id;

    @Override
    public boolean equals(Object o) {
        if (!(o instanceof Student))
            return false;

        Student s = (Student) o;
        return id == s.id;
    }

    @Override
    public int hashCode() {
        return Integer.hashCode(id);
    }
}
```

---

# Negative Hash Codes

Java hash codes may be negative.

Example

```java
System.out.println("Aa".hashCode());
```

Possible output

```
-1234567
```

Therefore bucket index is computed as

```java
index = (hash & 0x7fffffff) % capacity;
```

or internally using bit masking when capacity is a power of two.

---

# Hash Compression

Hash code

↓

Bucket index

Formula

```java
index = hash % tableSize;
```

Java `HashMap`

uses

```java
hash & (capacity - 1)
```

because capacities are powers of two.

This is significantly faster than modulo.

---

# Collision Probability

Collisions become more likely as the table fills.

Higher

```
Load Factor
```

↓

Higher collisions

↓

Lower performance

---

# Birthday Paradox

Surprisingly,

collisions occur much earlier than intuition suggests.

Example

Only

```
23
```

people

↓

50%

chance

↓

Two share same birthday.

The same principle explains why collisions occur quickly in hash tables.

---

# Designing Good Custom Hash Functions

Guidelines

- Include all important fields.
- Use prime multipliers.
- Avoid constant values.
- Avoid using mutable fields.
- Keep computation fast.
- Ensure consistency with `equals()`.

---

## Good Example

```java
@Override
public int hashCode() {
    int result = Integer.hashCode(id);
    result = 31 * result + name.hashCode();
    result = 31 * result + age;
    return result;
}
```

---

# Common Mistakes

## Mistake 1

Returning constant hash code.

```java
return 1;
```

Every object lands in the same bucket.

Performance becomes

```
O(n)
```

---

## Mistake 2

Using mutable fields.

```java
map.put(student, value);

student.id = 100;
```

Now lookup may fail because the hash bucket changed.

---

## Mistake 3

Overriding `equals()` without `hashCode()`.

Violates the Java contract and causes incorrect `HashMap` behavior.

---

## Mistake 4

Ignoring collisions.

A hash function does **not** guarantee unique outputs.

Collision handling is mandatory.

---

# Interview Questions

## Q1. Why does Java use 31 in `String.hashCode()`?

**Answer**

- Prime number
- Good distribution
- Efficient arithmetic
- Historically proven effective

---

## Q2. Why can two unequal objects have the same hash code?

Because hash functions map a huge input space into a finite integer range.

Collisions are mathematically unavoidable.

---

## Q3. Can two equal objects have different hash codes?

No.

That violates the `equals()` / `hashCode()` contract.

---

## Q4. Why are prime numbers preferred?

They reduce repeating patterns and improve key distribution.

---

## Q5. Why doesn't `HashMap` use cryptographic hashes?

Because cryptographic hashes are much slower and provide guarantees that are unnecessary for hash table lookups.

---

# Summary

A hash function is the foundation of every hash-based data structure. Its quality directly determines the performance of `HashMap`, `HashSet`, and many interview algorithms.

Key points:

- Deterministic output
- Uniform distribution
- Fast computation
- Low collision rate
- Good avalanche effect
- Proper `equals()` / `hashCode()` implementation
- Efficient compression into bucket indices

Understanding these concepts is essential before exploring how Java's `HashMap` stores entries, handles collisions, resizes itself, and achieves near-constant-time performance.

---

# What's Next

The next chapter explores **Java HashMap Internals**, including:

- Internal `Node<K,V>` structure
- Bucket array layout
- Hash spreading (`h ^ (h >>> 16)`)
- Collision handling with linked lists
- Treeification into Red-Black Trees (Java 8+)
- Resize and rehash process
- Capacity and threshold calculations
- Load factor implementation
- Fail-fast iterators
- Interview questions based on HashMap internals
