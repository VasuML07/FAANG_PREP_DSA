# Tries — FAANG Interview Preparation Guide

> **Topic:** Trie / Prefix Tree  
> **Language:** Java  
> **Questions Covered:** 15 (Part 1 covers Questions 1–5)

---

# Companies Frequently Asking Trie Problems

| Company | Frequency |
|----------|-----------|
| Google | ⭐⭐⭐⭐⭐ |
| Meta (Facebook) | ⭐⭐⭐⭐⭐ |
| Amazon | ⭐⭐⭐⭐⭐ |
| Microsoft | ⭐⭐⭐⭐ |
| Apple | ⭐⭐⭐⭐ |
| Uber | ⭐⭐⭐⭐ |
| Airbnb | ⭐⭐⭐ |
| Bloomberg | ⭐⭐⭐ |
| Adobe | ⭐⭐⭐ |
| Salesforce | ⭐⭐⭐ |
| LinkedIn | ⭐⭐⭐⭐ |
| ByteDance | ⭐⭐⭐⭐ |
| TikTok | ⭐⭐⭐ |
| Oracle | ⭐⭐⭐ |
| Walmart Global Tech | ⭐⭐⭐ |

---

# Problems Covered

| # | Problem | Difficulty |
|---|----------|------------|
|1|208. Implement Trie (Prefix Tree)|Medium|
|2|1804. Implement Trie II (Prefix Tree)|Medium|
|3|211. Design Add and Search Words Data Structure|Medium|
|4|648. Replace Words|Medium|
|5|720. Longest Word in Dictionary|Medium|

---

# 1. LeetCode 208 — Implement Trie (Prefix Tree)

**Difficulty:** Medium

**Companies**

Google, Amazon, Microsoft, Meta, Apple

**Problem**

Implement a Trie supporting:

- insert(word)
- search(word)
- startsWith(prefix)

---

## Interview Pattern

This is the base implementation used inside dozens of harder Trie questions.

Interviewers mainly check:

- clean Trie node design
- insertion
- prefix traversal
- search logic

---

## Visualization

```
Insert:
cat
car
can

(root)
   |
   c
   |
   a
 / | \
t  r  n
```

Searching `"car"`

```
root
 ↓
 c
 ↓
 a
 ↓
 r ✓
```

---

## Approach

Each node stores

- 26 children
- endOfWord flag

Insertion

- traverse characters
- create missing node
- mark last node

Search

- traverse
- verify last node is end

Prefix

- traverse only

---

## Java Solution

```java
class Trie {

    class TrieNode {
        TrieNode[] children = new TrieNode[26];
        boolean isEnd;
    }

    private TrieNode root;

    public Trie() {
        root = new TrieNode();
    }

    public void insert(String word) {

        TrieNode curr = root;

        for(char c : word.toCharArray()) {

            int idx = c - 'a';

            if(curr.children[idx] == null)
                curr.children[idx] = new TrieNode();

            curr = curr.children[idx];
        }

        curr.isEnd = true;
    }

    public boolean search(String word) {

        TrieNode curr = root;

        for(char c : word.toCharArray()) {

            int idx = c - 'a';

            if(curr.children[idx] == null)
                return false;

            curr = curr.children[idx];
        }

        return curr.isEnd;
    }

    public boolean startsWith(String prefix) {

        TrieNode curr = root;

        for(char c : prefix.toCharArray()) {

            int idx = c - 'a';

            if(curr.children[idx] == null)
                return false;

            curr = curr.children[idx];
        }

        return true;
    }
}
```

---

## Complexity

|Operation|Time|Space|
|----------|----|------|
|Insert|O(L)|O(L)|
|Search|O(L)|O(1)|
|Prefix|O(L)|O(1)|

L = word length

---

## Interview Follow-up

**Follow-up**

Support deletion efficiently.

Expected answer:

- Maintain pass count
- Remove unused nodes while backtracking.

---

# 2. LeetCode 1804 — Implement Trie II (Prefix Tree)

**Difficulty**

Medium

**Companies**

Google

Meta

Microsoft

Amazon

---

## Problem

Trie now supports

- insert
- countWordsEqualTo
- countWordsStartingWith
- erase

Duplicate words allowed.

---

## Visualization

```
Insert:

apple
apple
app

passCount

a(3)
 |
p(3)
 |
p(3)
 |
l(2)
 |
e(2)
```

---

## Key Idea

Each node stores

```
passCount
endCount
```

Insertion

```
increment passCount

last node

increment endCount
```

Deletion

```
decrement counts
```

---

## Java Solution

```java
class Trie {

    class TrieNode {

        TrieNode[] child = new TrieNode[26];

        int prefixCount;

        int wordCount;
    }

    TrieNode root;

    public Trie() {

        root = new TrieNode();
    }

    public void insert(String word) {

        TrieNode curr = root;

        for(char c : word.toCharArray()) {

            int idx = c - 'a';

            if(curr.child[idx] == null)
                curr.child[idx] = new TrieNode();

            curr = curr.child[idx];

            curr.prefixCount++;
        }

        curr.wordCount++;
    }

    public int countWordsEqualTo(String word) {

        TrieNode curr = root;

        for(char c : word.toCharArray()) {

            int idx = c - 'a';

            if(curr.child[idx] == null)
                return 0;

            curr = curr.child[idx];
        }

        return curr.wordCount;
    }

    public int countWordsStartingWith(String prefix) {

        TrieNode curr = root;

        for(char c : prefix.toCharArray()) {

            int idx = c - 'a';

            if(curr.child[idx] == null)
                return 0;

            curr = curr.child[idx];
        }

        return curr.prefixCount;
    }

    public void erase(String word) {

        TrieNode curr = root;

        for(char c : word.toCharArray()) {

            int idx = c - 'a';

            curr = curr.child[idx];

            curr.prefixCount--;
        }

        curr.wordCount--;
    }
}
```

---

## Complexity

|Operation|Complexity|
|----------|----------|
|Insert|O(L)|
|Erase|O(L)|
|Count Equal|O(L)|
|Count Prefix|O(L)|

---

## Follow-up

Design erase that also frees memory.

---

# 3. LeetCode 211 — Design Add and Search Words Data Structure

**Difficulty**

Medium

**Companies**

Meta

Google

Amazon

LinkedIn

Apple

---

## Problem

Support

```
addWord()

search()
```

where

```
'.'
```

matches any letter.

---

## Visualization

Searching

```
b..

root
 ↓
 b
 ↓
(any)
 ↓
(any)
```

DFS explores every possible branch.

---

## Interview Pattern

This introduces

```
Trie + DFS
```

One of the most common interview combinations.

---

## Java Solution

```java
class WordDictionary {

    class TrieNode {

        TrieNode[] child = new TrieNode[26];

        boolean end;
    }

    TrieNode root;

    public WordDictionary() {

        root = new TrieNode();
    }

    public void addWord(String word) {

        TrieNode curr = root;

        for(char c : word.toCharArray()) {

            int idx = c - 'a';

            if(curr.child[idx] == null)
                curr.child[idx] = new TrieNode();

            curr = curr.child[idx];
        }

        curr.end = true;
    }

    public boolean search(String word) {

        return dfs(word,0,root);
    }

    private boolean dfs(String word,int pos,TrieNode node){

        if(node==null)
            return false;

        if(pos==word.length())
            return node.end;

        char c=word.charAt(pos);

        if(c=='.'){

            for(TrieNode next:node.child){

                if(next!=null && dfs(word,pos+1,next))
                    return true;
            }

            return false;
        }

        return dfs(word,pos+1,node.child[c-'a']);
    }
}
```

---

## Complexity

Worst case

```
O(26^L)
```

Typical

```
O(L)
```

---

## Follow-up

Support

```
*

```

instead of

```
.

```

where

```
*
```

matches zero or more characters.

This becomes a Trie + Backtracking problem.

---

# 4. LeetCode 648 — Replace Words

**Difficulty**

Medium

**Companies**

Google

Amazon

Microsoft

Meta

---

## Problem

Given dictionary roots

replace every word in sentence by its shortest root.

---

## Visualization

Dictionary

```
cat
bat
rat
```

Sentence

```
cattle was rattled by battery
```

Trie

```
c
|
a
|
t✓
```

Stop immediately at first terminal node.

---

## Approach

Insert all roots.

For every sentence word

walk Trie

first terminal node

replace immediately

---

## Java Solution

```java
class Solution {

    class TrieNode{

        TrieNode[] child=new TrieNode[26];

        boolean end;
    }

    TrieNode root=new TrieNode();

    private void insert(String word){

        TrieNode curr=root;

        for(char c:word.toCharArray()){

            int idx=c-'a';

            if(curr.child[idx]==null)
                curr.child[idx]=new TrieNode();

            curr=curr.child[idx];
        }

        curr.end=true;
    }

    private String search(String word){

        TrieNode curr=root;

        StringBuilder sb=new StringBuilder();

        for(char c:word.toCharArray()){

            int idx=c-'a';

            if(curr.child[idx]==null)
                return word;

            sb.append(c);

            curr=curr.child[idx];

            if(curr.end)
                return sb.toString();
        }

        return word;
    }

    public String replaceWords(List<String> dictionary,String sentence){

        for(String s:dictionary)
            insert(s);

        String[] words=sentence.split(" ");

        for(int i=0;i<words.length;i++)
            words[i]=search(words[i]);

        return String.join(" ",words);
    }
}
```

---

## Complexity

|Operation|Complexity|
|----------|-----------|
|Build Trie|O(NL)|
|Replace|O(S)|

---

## Follow-up

Instead of shortest root

return

```
longest root
```

Only search logic changes.

---

# 5. LeetCode 720 — Longest Word in Dictionary

**Difficulty**

Medium

**Companies**

Google

Amazon

Apple

Microsoft

---

## Problem

Return the longest word that can be built one character at a time.

Example

```
a

ap

app

appl

apple
```

Valid

```
apple
```

---

## Visualization

```
a✓
|
p✓
|
p✓
|
l✓
|
e✓
```

Every prefix must already exist.

---

## Key Observation

During DFS

visit only nodes whose

```
isEnd == true
```

---

## Java Solution

```java
class Solution {

    class TrieNode{

        TrieNode[] child=new TrieNode[26];

        boolean end;

        String word="";
    }

    TrieNode root=new TrieNode();

    public String longestWord(String[] words){

        for(String word:words){

            TrieNode curr=root;

            for(char c:word.toCharArray()){

                int idx=c-'a';

                if(curr.child[idx]==null)
                    curr.child[idx]=new TrieNode();

                curr=curr.child[idx];
            }

            curr.end=true;
            curr.word=word;
        }

        String ans="";

        Stack<TrieNode> stack=new Stack<>();

        stack.push(root);

        while(!stack.isEmpty()){

            TrieNode node=stack.pop();

            if(node!=root){

                if(node.word.length()>ans.length() ||
                        (node.word.length()==ans.length() &&
                         node.word.compareTo(ans)<0))
                    ans=node.word;
            }

            for(int i=25;i>=0;i--){

                TrieNode next=node.child[i];

                if(next!=null && next.end)
                    stack.push(next);
            }
        }

        return ans;
    }
}
```

---

## Complexity

|Metric|Complexity|
|------|-----------|
|Build Trie|O(NL)|
|DFS|O(NL)|
|Space|O(NL)|

---

## Follow-up

Return

- all longest words
- kth longest buildable word
- dynamic insert/delete version

These require storing additional metadata during traversal.

---

# End of Part 1

**Covered (5/15):**

- 208. Implement Trie
- 1804. Implement Trie II
- 211. Design Add and Search Words
- 648. Replace Words
- 720. Longest Word in Dictionary

**Part 2** will cover Questions **6–10**, including advanced Trie + DFS, Bitmask + Trie, and Prefix Search interview problems.

---

# 6. LeetCode 677 — Map Sum Pairs

**Difficulty:** Medium

**Companies**

- Google
- Amazon
- Microsoft
- Apple
- Meta

---

## Problem

Design a data structure that supports:

- `insert(String key, int val)`
- `sum(String prefix)`

`sum(prefix)` should return the total value of every key beginning with that prefix.

Example

```
insert("apple",3)

sum("ap") = 3

insert("app",2)

sum("ap") = 5

insert("apple",5)

sum("ap") = 7
```

Notice that inserting an existing key **updates** its value instead of adding a duplicate.

---

## Interview Pattern

This is a classic interview question that checks whether you understand how to augment Trie nodes with metadata.

Instead of recomputing sums every query:

- Maintain prefix sums while inserting.
- Adjust by the value difference (`delta`) when updating an existing key.

---

## Visualization

Initial:

```
apple -> 3

(root)
   |
   a(3)
   |
   p(3)
   |
   p(3)
   |
   l(3)
   |
   e(3)
```

Update

```
apple : 3 → 5

delta = +2

(root)
   |
   a(5)
   |
   p(5)
   |
   p(5)
   |
   l(5)
   |
   e(5)
```

Every node along the path increases by `delta`.

---

## Approach

Maintain:

- HashMap storing previous value of every key.
- Every Trie node stores:

```
prefixSum
```

Insertion:

```
delta = newValue - oldValue

Update every node along the path

prefixSum += delta
```

Query:

Traverse to prefix node.

Return stored sum.

---

## Java Solution

```java
class MapSum {

    class TrieNode {
        TrieNode[] child = new TrieNode[26];
        int sum;
    }

    private TrieNode root;
    private Map<String, Integer> map;

    public MapSum() {
        root = new TrieNode();
        map = new HashMap<>();
    }

    public void insert(String key, int val) {

        int delta = val - map.getOrDefault(key, 0);
        map.put(key, val);

        TrieNode curr = root;

        for (char c : key.toCharArray()) {

            int idx = c - 'a';

            if (curr.child[idx] == null)
                curr.child[idx] = new TrieNode();

            curr = curr.child[idx];

            curr.sum += delta;
        }
    }

    public int sum(String prefix) {

        TrieNode curr = root;

        for (char c : prefix.toCharArray()) {

            int idx = c - 'a';

            if (curr.child[idx] == null)
                return 0;

            curr = curr.child[idx];
        }

        return curr.sum;
    }
}
```

---

## Complexity

| Operation | Complexity |
|------------|------------|
| Insert | O(L) |
| Sum | O(L) |
| Space | O(N × L) |

---

## Common Pitfall

Wrong:

```
insert("apple",5)

sum += 5
```

Correct:

```
delta = 5 - previousValue
```

Otherwise repeated inserts produce incorrect prefix sums.

---

## Interview Follow-up

How would you support

```
delete(key)
```

Expected answer:

Subtract stored value along every prefix and remove key from HashMap.

---

# 7. LeetCode 421 — Maximum XOR of Two Numbers in an Array

**Difficulty:** Medium

**Companies**

- Google
- Amazon
- Microsoft
- Meta
- ByteDance
- Uber

---

## Problem

Given an integer array, return the maximum XOR obtainable from any pair.

Example

```
nums =

3
10
5
25
2
8

Answer = 28
```

Because

```
5 XOR 25 = 28
```

---

## Interview Pattern

This is the most famous **Bit Trie** problem.

Instead of characters:

Trie stores

```
0

1
```

for every bit.

Goal:

At every level choose the opposite bit because

```
1 XOR 0 = 1
```

which maximizes the result.

---

## Visualization

Numbers (5 bits)

```
5

00101

25

11001
```

Trie

```
          root
        /      \
       0        1
      /          \
     0            1
    /              \
...
```

Searching for

```
00101
```

tries to go

```
1

1

0

1

0
```

whenever possible.

---

## Approach

1. Insert every number bit-by-bit.

2. While searching:

For every bit

```
desired = oppositeBit
```

If available

```
answer |= (1 << bit)
```

Else

continue same direction.

---

## Java Solution

```java
class Solution {

    class TrieNode {

        TrieNode[] child = new TrieNode[2];
    }

    TrieNode root = new TrieNode();

    private void insert(int num) {

        TrieNode curr = root;

        for (int i = 31; i >= 0; i--) {

            int bit = (num >> i) & 1;

            if (curr.child[bit] == null)
                curr.child[bit] = new TrieNode();

            curr = curr.child[bit];
        }
    }

    private int search(int num) {

        TrieNode curr = root;

        int ans = 0;

        for (int i = 31; i >= 0; i--) {

            int bit = (num >> i) & 1;

            int opposite = 1 - bit;

            if (curr.child[opposite] != null) {

                ans |= (1 << i);
                curr = curr.child[opposite];

            } else {

                curr = curr.child[bit];
            }
        }

        return ans;
    }

    public int findMaximumXOR(int[] nums) {

        for (int num : nums)
            insert(num);

        int max = 0;

        for (int num : nums)
            max = Math.max(max, search(num));

        return max;
    }
}
```

---

## Complexity

| Metric | Complexity |
|---------|------------|
| Build Trie | O(32N) |
| Search | O(32N) |
| Space | O(32N) |

Simplifies to

```
O(N)
```

---

## Visualization

Suppose current bit

```
Current = 0

Want = 1
```

```
Trie

0
 \
 1   ← choose this

XOR becomes 1
```

---

## Common Pitfall

Many candidates greedily compare integers.

Correct solution compares

**bit-by-bit from MSB to LSB**.

---

## Interview Follow-up

Find

- Minimum XOR
- Maximum XOR in a range
- Streaming maximum XOR
- Dynamic insert/delete XOR Trie

These are common Google and ByteDance extensions.

---

# 8. LeetCode 1268 — Search Suggestions System

**Difficulty:** Medium

**Companies**

- Amazon
- Google
- Microsoft
- Apple
- Meta

---

## Problem

Given products and a search word,

after typing each character,

return at most **3 lexicographically smallest suggestions**.

Example

```
products

mobile
mouse
moneypot
monitor
mousepad

search

mouse
```

Output

```
m

mobile
moneypot
monitor

mo

mobile
moneypot
monitor

mou

mouse
mousepad

mous

mouse
mousepad

mouse

mouse
mousepad
```

---

## Interview Pattern

Trie + Prefix Search

Every Trie node stores

```
Top 3 words
```

instead of searching the subtree every query.

---

## Visualization

```
root
 |
 m
 |
 o
 |
 u
 |
 s
 |
 e
```

Each node stores

```
["mobile",
 "moneypot",
 "monitor"]
```

or

```
["mouse",
 "mousepad"]
```

depending on the prefix.

---

## Approach

1. Sort products.

2. Insert in sorted order.

3. Every node keeps

```
maximum 3 strings
```

During query,

simply traverse the Trie and return stored list.

---

## Java Solution

```java
class Solution {

    class TrieNode {

        TrieNode[] child = new TrieNode[26];

        List<String> suggestions = new ArrayList<>();
    }

    TrieNode root = new TrieNode();

    private void insert(String word) {

        TrieNode curr = root;

        for (char c : word.toCharArray()) {

            int idx = c - 'a';

            if (curr.child[idx] == null)
                curr.child[idx] = new TrieNode();

            curr = curr.child[idx];

            if (curr.suggestions.size() < 3)
                curr.suggestions.add(word);
        }
    }

    public List<List<String>> suggestedProducts(String[] products,
                                                String searchWord) {

        Arrays.sort(products);

        for (String product : products)
            insert(product);

        List<List<String>> ans = new ArrayList<>();

        TrieNode curr = root;

        for (char c : searchWord.toCharArray()) {

            if (curr != null)
                curr = curr.child[c - 'a'];

            if (curr == null)
                ans.add(new ArrayList<>());

            else
                ans.add(curr.suggestions);
        }

        return ans;
    }
}
```

---

## Complexity

| Metric | Complexity |
|---------|------------|
| Sorting | O(N log N) |
| Build Trie | O(NL) |
| Query | O(L) |
| Space | O(NL) |

---

## Visualization

Node contents

```
Prefix "m"

↓

Suggestions

mobile
moneypot
monitor
```

Prefix

```
mou

↓

mouse
mousepad
```

No DFS required during queries because results are cached.

---

## Common Pitfall

Many candidates perform DFS from every prefix.

That leads to

```
O(N × L)
```

per query.

Interviewers expect

```
Store top 3 at insertion time.
```

---

## Interview Follow-up

Modify the system to support:

- Top **k** suggestions instead of 3
- Suggestions ranked by frequency instead of lexicographical order
- Dynamic insertion/removal of products
- Case-insensitive search
- Autocomplete for Unicode strings using `HashMap<Character, TrieNode>` instead of a fixed 26-element array

---

# End of Part 2A

**Covered (8/15):**

1. 208. Implement Trie (Prefix Tree)
2. 1804. Implement Trie II
3. 211. Design Add and Search Words Data Structure
4. 648. Replace Words
5. 720. Longest Word in Dictionary
6. 677. Map Sum Pairs
7. 421. Maximum XOR of Two Numbers in an Array
8. 1268. Search Suggestions System

**Next (Part 2B):**

- **212. Word Search II** (Trie + DFS + Backtracking)
- **820. Short Encoding of Words** (Reverse Trie)

---

# 9. LeetCode 212 — Word Search II

**Difficulty:** Hard

**Companies**

- Google
- Meta
- Amazon
- Microsoft
- Apple
- ByteDance
- Bloomberg

---

## Problem

Given an `m × n` board of characters and a list of words, return all words that can be formed by sequentially adjacent cells.

Rules:

- Horizontal or vertical movement only.
- A cell cannot be reused in the same word.

Example

```
Board

o a a n
e t a e
i h k r
i f l v

Words

oath
pea
eat
rain

Answer

[oath, eat]
```

---

## Interview Pattern

This is one of the most important Trie interview questions.

Instead of searching every word independently,

```
DFS × Number of Words
```

build one Trie containing every word.

Then perform DFS once from every board cell.

This reduces unnecessary searches dramatically.

---

## Visualization

Board

```
o  a  a  n
e  t  a  e
i  h  k  r
i  f  l  v
```

Trie

```
root
 |
 o
 |
 a
 |
 t
 |
 h✓

e
|
a
|
t✓
```

DFS walks both simultaneously.

---

## Approach

1. Insert every word into Trie.

2. DFS from every cell.

3. Stop immediately if

```
Trie child == null
```

4. Whenever Trie node marks a word,

add it to answer.

5. Mark visited cell using '#'.

6. Restore after DFS.

---

## Java Solution

```java
class Solution {

    class TrieNode {
        TrieNode[] child = new TrieNode[26];
        String word;
    }

    TrieNode root = new TrieNode();

    public List<String> findWords(char[][] board, String[] words) {

        for (String word : words)
            insert(word);

        List<String> ans = new ArrayList<>();

        for (int i = 0; i < board.length; i++) {

            for (int j = 0; j < board[0].length; j++) {

                dfs(board, i, j, root, ans);
            }
        }

        return ans;
    }

    private void insert(String word) {

        TrieNode curr = root;

        for (char c : word.toCharArray()) {

            int idx = c - 'a';

            if (curr.child[idx] == null)
                curr.child[idx] = new TrieNode();

            curr = curr.child[idx];
        }

        curr.word = word;
    }

    private void dfs(char[][] board,
                     int i,
                     int j,
                     TrieNode node,
                     List<String> ans) {

        if (i < 0 || j < 0 ||
            i == board.length ||
            j == board[0].length)
            return;

        char c = board[i][j];

        if (c == '#')
            return;

        TrieNode next = node.child[c - 'a'];

        if (next == null)
            return;

        if (next.word != null) {

            ans.add(next.word);

            next.word = null; // avoid duplicates
        }

        board[i][j] = '#';

        dfs(board, i + 1, j, next, ans);
        dfs(board, i - 1, j, next, ans);
        dfs(board, i, j + 1, next, ans);
        dfs(board, i, j - 1, next, ans);

        board[i][j] = c;
    }
}
```

---

## Complexity

| Metric | Complexity |
|---------|------------|
| Build Trie | O(W × L) |
| DFS | O(M × N × 4ᴸ) Worst Case |
| Space | O(W × L) |

Where

- W = number of words
- L = maximum word length

Actual runtime is much better because Trie pruning eliminates most searches.

---

## Visualization

DFS

```
o

↓

a

↓

t

↓

h✓
```

Immediately stops when Trie edge doesn't exist.

```
Current Letter

↓

No Trie Child

↓

Return
```

This pruning is the entire optimization.

---

## Common Pitfalls

### ❌ Searching every word separately

```
for every word

DFS(board)
```

Too slow.

---

### ❌ Not removing duplicates

If

```
word = "eat"
```

appears multiple times,

output becomes

```
eat
eat
eat
```

Set

```java
node.word = null;
```

after finding it.

---

### ❌ Forgetting to restore board

Always restore

```java
board[i][j] = originalCharacter;
```

after DFS.

---

## Interview Follow-ups

1. Return coordinates instead of words.
2. Support diagonal movement.
3. Allow wildcard characters.
4. Dynamic insertion/removal of dictionary words.
5. Unicode Trie using `HashMap<Character, TrieNode>`.

---

# 10. LeetCode 820 — Short Encoding of Words

**Difficulty:** Medium

**Companies**

- Google
- Amazon
- Microsoft
- Apple
- Adobe

---

## Problem

Find the minimum length reference string encoding all words.

Example

```
words

time
me
bell

Encoding

time#bell#

Length = 10
```

Notice

```
me
```

does not need separate encoding because

```
time
```

already contains it.

---

## Interview Pattern

Instead of storing prefixes,

store **reversed words**.

Suffixes become prefixes.

---

## Visualization

Words

```
time

me

bell
```

Reverse

```
emit

em

lleb
```

Trie

```
root

├── e
│   └── m
│       └── i
│           └── t
└── l
    └── l
        └── e
            └── b
```

Notice

```
em
```

ends before

```
emit
```

so it contributes nothing.

---

## Approach

1. Reverse every word.
2. Insert into Trie.
3. Leaf nodes represent unique suffixes.
4. Only leaf words contribute.

Contribution

```
wordLength + 1
```

because of '#'.

---

## Java Solution

```java
class Solution {

    class TrieNode {

        TrieNode[] child = new TrieNode[26];
    }

    TrieNode root = new TrieNode();

    Map<TrieNode, Integer> map = new HashMap<>();

    public int minimumLengthEncoding(String[] words) {

        Arrays.sort(words,
                (a, b) -> b.length() - a.length());

        for (String word : words)
            insert(word);

        int ans = 0;

        for (Map.Entry<TrieNode, Integer> e : map.entrySet()) {

            TrieNode node = e.getKey();

            boolean leaf = true;

            for (TrieNode next : node.child) {

                if (next != null) {

                    leaf = false;
                    break;
                }
            }

            if (leaf)
                ans += e.getValue() + 1;
        }

        return ans;
    }

    private void insert(String word) {

        TrieNode curr = root;

        for (int i = word.length() - 1; i >= 0; i--) {

            int idx = word.charAt(i) - 'a';

            if (curr.child[idx] == null)
                curr.child[idx] = new TrieNode();

            curr = curr.child[idx];
        }

        map.put(curr, word.length());
    }
}
```

---

## Complexity

| Metric | Complexity |
|---------|------------|
| Sorting | O(N log N) |
| Build Trie | O(NL) |
| Leaf Scan | O(NL) |
| Space | O(NL) |

---

## Visualization

Encoding

```
time#

bell#
```

No separate

```
me#
```

because

```
time
```

already contains it.

Trie

```
emit

↓

em

(shared path)
```

Only the deepest leaf contributes.

---

## Common Pitfalls

### ❌ Building a normal Trie

This solves prefix problems.

This question is about suffixes.

Reverse every word first.

---

### ❌ Counting every word

Only leaf nodes add to the answer.

---

### ❌ Forgetting '#'

Contribution is

```
length + 1
```

not

```
length
```

---

## Interview Follow-ups

1. Return the encoding string instead of only its length.
2. Compress dynamically as new words arrive.
3. Support deletion.
4. Extend to Unicode alphabets.
5. Handle millions of words with compressed Trie (Radix Trie).

---

# End of Part 2B

## Covered So Far (10/15)

| # | Problem | Difficulty |
|---|----------|------------|
|208|Implement Trie (Prefix Tree)|Medium|
|1804|Implement Trie II|Medium|
|211|Design Add and Search Words Data Structure|Medium|
|648|Replace Words|Medium|
|720|Longest Word in Dictionary|Medium|
|677|Map Sum Pairs|Medium|
|421|Maximum XOR of Two Numbers in an Array|Medium|
|1268|Search Suggestions System|Medium|
|212|Word Search II|Hard|
|820|Short Encoding of Words|Medium|

---

## Next (Part 3A)

Questions **11–13**

- **1707. Maximum XOR With an Element From Array** (Offline Queries + Bit Trie)
- **1938. Maximum Genetic Difference Query** (Tree DFS + Bit Trie)
- **1032. Stream of Characters** (Reverse Trie + Streaming)

---

# 11. LeetCode 1707 — Maximum XOR With an Element From Array

**Difficulty:** Hard

**Companies**

- Google
- Meta
- Amazon
- Microsoft
- ByteDance

---

## Problem

Given

```
nums[]
queries[i] = [x, m]
```

Find

```
max(x XOR num)
```

where

```
num <= m
```

If no such number exists, return

```
-1
```

---

## Interview Pattern

This combines

```
Sorting
+
Offline Queries
+
Bit Trie
```

Instead of rebuilding the Trie for every query,

sort both

- nums
- queries

and insert numbers only once.

---

## Visualization

```
nums

0 1 2 3 4

↓

Sorted

0 1 2 3 4
```

Query

```
x = 7
m = 2
```

Trie contains only

```
0
1
2
```

Maximum XOR is computed using those numbers only.

---

## Approach

1. Sort nums.

2. Store query index.

```
[x,m,index]
```

3. Sort queries by

```
m
```

4. Insert every eligible number into Trie.

5. Answer XOR.

---

## Java Solution

```java
class Solution {

    class TrieNode {
        TrieNode[] child = new TrieNode[2];
    }

    TrieNode root = new TrieNode();

    private void insert(int num) {

        TrieNode curr = root;

        for (int i = 31; i >= 0; i--) {

            int bit = (num >> i) & 1;

            if (curr.child[bit] == null)
                curr.child[bit] = new TrieNode();

            curr = curr.child[bit];
        }
    }

    private int query(int num) {

        TrieNode curr = root;

        if (curr.child[0] == null && curr.child[1] == null)
            return -1;

        int ans = 0;

        for (int i = 31; i >= 0; i--) {

            int bit = (num >> i) & 1;

            int opposite = bit ^ 1;

            if (curr.child[opposite] != null) {

                ans |= (1 << i);

                curr = curr.child[opposite];

            } else {

                curr = curr.child[bit];
            }
        }

        return ans;
    }

    public int[] maximizeXor(int[] nums, int[][] queries) {

        Arrays.sort(nums);

        int[][] q = new int[queries.length][3];

        for (int i = 0; i < queries.length; i++) {

            q[i][0] = queries[i][0];
            q[i][1] = queries[i][1];
            q[i][2] = i;
        }

        Arrays.sort(q, Comparator.comparingInt(a -> a[1]));

        int[] ans = new int[q.length];

        int index = 0;

        for (int[] query : q) {

            while (index < nums.length &&
                    nums[index] <= query[1]) {

                insert(nums[index]);
                index++;
            }

            ans[query[2]] = query(query[0]);
        }

        return ans;
    }
}
```

---

## Complexity

| Metric | Complexity |
|---------|------------|
| Sorting | O(N log N + Q log Q) |
| Trie Operations | O(32(N+Q)) |
| Overall | O(N log N + Q log Q) |

---

## Visualization

```
nums

1
2
4
7
10

Query

x=5

m=4

Trie

1
2
4

↓

Maximum XOR
```

---

## Common Pitfall

Many candidates rebuild the Trie for every query.

That becomes

```
O(NQ)
```

Offline sorting avoids this completely.

---

## Interview Follow-up

Support

- online queries
- insertion
- deletion
- range XOR queries

---

# 12. LeetCode 1938 — Maximum Genetic Difference Query

**Difficulty:** Hard

**Companies**

- Google
- Meta

---

## Problem

A rooted tree is given.

Each query

```
[node, value]
```

asks for

```
maximum(value XOR ancestor)
```

where

```
ancestor
```

is any node on the current root-to-node path.

---

## Interview Pattern

This is one of the hardest Trie interview problems.

Technique

```
Tree DFS

+

Dynamic Bit Trie

+

Backtracking
```

---

## Visualization

```
          0
        /   \
       1     2
     /  \
    3    4
```

DFS

```
0

↓

1

↓

3
```

Trie contains only

```
0

1

3
```

When DFS returns,

```
3
```

is removed.

---

## Approach

DFS

```
Insert current node value

↓

Answer queries

↓

DFS children

↓

Remove current node
```

Trie always represents

```
Current Ancestor Path
```

---

## Java Skeleton

```java
class TrieNode {

    TrieNode[] child = new TrieNode[2];

    int count;
}
```

Insertion

```java
void insert(int num)
```

Removal

```java
void remove(int num)
```

Maximum XOR

```java
int query(int value)
```

DFS

```java
void dfs(int node){

    insert(node);

    answerQueries(node);

    for(child)

        dfs(child);

    remove(node);
}
```

---

## Complexity

| Metric | Complexity |
|---------|------------|
| DFS | O(N) |
| Trie Operations | O(32) |
| Total | O((N+Q)×32) |

---

## Visualization

Trie changes dynamically.

```
DFS

0

Trie

0

↓

DFS

1

Trie

0

1

↓

DFS

4

Trie

0

1

4

↓

Backtrack

Trie

0

1
```

---

## Common Pitfall

Global Trie.

Wrong.

Trie should contain only

```
Current DFS Path
```

Therefore

```
Remove()
```

during backtracking is mandatory.

---

## Interview Follow-up

Modify for

- subtree queries
- dynamic trees
- weighted XOR
- binary lifting + Trie

---

# 13. LeetCode 1032 — Stream of Characters

**Difficulty:** Hard

**Companies**

- Google
- Microsoft
- Amazon
- Meta

---

## Problem

Characters arrive one at a time.

After every character,

determine whether

**any suffix**

matches a dictionary word.

---

## Example

Dictionary

```
cd
f
kl
```

Stream

```
a

b

c

d

↓

true
```

because

```
cd
```

appears as suffix.

---

## Interview Pattern

Instead of storing words normally,

store

```
Reversed Words
```

Maintain stream.

Search backward.

---

## Visualization

Dictionary

```
cd

↓

dc
```

Trie

```
root

↓

d

↓

c✓
```

Incoming stream

```
a b c d
```

Traverse backward

```
d

↓

c

✓
```

---

## Approach

1. Reverse every dictionary word.

2. Insert into Trie.

3. Maintain stream.

4. Search from newest character backwards.

---

## Java Solution

```java
class StreamChecker {

    class TrieNode {

        TrieNode[] child = new TrieNode[26];

        boolean end;
    }

    TrieNode root = new TrieNode();

    StringBuilder stream = new StringBuilder();

    public StreamChecker(String[] words) {

        for (String word : words)
            insert(word);
    }

    private void insert(String word) {

        TrieNode curr = root;

        for (int i = word.length() - 1; i >= 0; i--) {

            int idx = word.charAt(i) - 'a';

            if (curr.child[idx] == null)
                curr.child[idx] = new TrieNode();

            curr = curr.child[idx];
        }

        curr.end = true;
    }

    public boolean query(char letter) {

        stream.append(letter);

        TrieNode curr = root;

        for (int i = stream.length() - 1;
             i >= 0 && curr != null;
             i--) {

            int idx = stream.charAt(i) - 'a';

            curr = curr.child[idx];

            if (curr != null && curr.end)
                return true;
        }

        return false;
    }
}
```

---

## Complexity

| Metric | Complexity |
|---------|------------|
| Insert | O(WL) |
| Query | O(Lmax) |
| Space | O(WL) |

Where

```
Lmax
```

is maximum word length.

---

## Visualization

```
Stream

a

↓

ab

↓

abc

↓

abcd
```

Search

```
d

↓

c

✓
```

---

## Common Pitfall

Searching from

```
left → right
```

is incorrect.

Need

```
right → left
```

because the problem asks for suffixes.

---

## Interview Follow-up

Support

- deletion
- wildcard characters
- Unicode
- multiple streams
- streaming with millions of characters (truncate stream to maximum dictionary length)

---

# End of Part 3A

## Covered (13/15)

| # | Problem |
|---|----------|
|208|Implement Trie|
|1804|Implement Trie II|
|211|Design Add and Search Words|
|648|Replace Words|
|720|Longest Word in Dictionary|
|677|Map Sum Pairs|
|421|Maximum XOR of Two Numbers|
|1268|Search Suggestions System|
|212|Word Search II|
|820|Short Encoding of Words|
|1707|Maximum XOR With an Element From Array|
|1938|Maximum Genetic Difference Query|
|1032|Stream of Characters|

---

## Next (Final Part)

Questions **14–15**

- **745. Prefix and Suffix Search**
- **472. Concatenated Words**

followed by:

- Trie Interview Patterns
- Memory Optimizations
- XOR Trie Cheat Sheet
- Prefix vs Suffix Trie
- Radix Trie
- Compressed Trie
- Unicode Trie
- LLM-proof Follow-ups
- FAANG Interview Tricks
- Complexity Cheat Sheet
- Common Mistakes
- One-page Revision Notes

---

# 14. LeetCode 745 — Prefix and Suffix Search

**Difficulty:** Hard

**Companies**

- Google
- Meta
- Amazon
- Apple
- Microsoft

---

## Problem

Design a data structure that supports

```
f(prefix, suffix)
```

and returns the **largest index** of a word having both the given prefix and suffix.

Example

```
words

apple

Queries

f("a","e") = 0

f("ap","le") = 0

f("b","e") = -1
```

---

## Interview Pattern

This is one of the most famous Trie design questions.

A naive solution would build

- Prefix Trie
- Suffix Trie

Then intersect index lists.

Interviewers expect a more elegant solution.

---

## Key Trick

Insert every word in the form

```
suffix + '{' + word
```

where

```
'{' ASCII = 123

'a' = 97
'z' = 122
```

Therefore

```
{

```

acts like the 27th character.

Example

```
apple

↓

{apple
e{apple
le{apple
ple{apple
pple{apple
apple{apple
```

Now query

```
prefix

suffix
```

becomes

```
suffix + '{' + prefix
```

---

## Visualization

Word

```
apple
```

Inserted

```
apple{apple

pple{apple

ple{apple

le{apple

e{apple

{apple
```

Trie

```
root
 |
 l
 |
 e
 |
 {
 |
 a
 |
 p
 |
 p
 |
 l
 |
 e
```

---

## Approach

For every word

Generate every suffix

```
suffix + '{' + word
```

Insert into Trie.

Each node stores

```
largest index
```

During query

Search

```
suffix + '{' + prefix
```

Answer stored index.

---

## Java Solution

```java
class WordFilter {

    class TrieNode {

        TrieNode[] child = new TrieNode[27];

        int weight = -1;
    }

    TrieNode root = new TrieNode();

    public WordFilter(String[] words) {

        for (int weight = 0; weight < words.length; weight++) {

            String word = words[weight];

            String key = word + "{";

            for (int i = 0; i <= word.length(); i++) {

                TrieNode curr = root;

                curr.weight = weight;

                for (int j = i; j < key.length() + word.length(); j++) {

                    char c = (j < key.length())
                            ? key.charAt(j)
                            : word.charAt(j - key.length());

                    int idx = (c == '{') ? 26 : c - 'a';

                    if (curr.child[idx] == null)
                        curr.child[idx] = new TrieNode();

                    curr = curr.child[idx];

                    curr.weight = weight;
                }
            }
        }
    }

    public int f(String prefix, String suffix) {

        TrieNode curr = root;

        String search = suffix + "{" + prefix;

        for (char c : search.toCharArray()) {

            int idx = (c == '{') ? 26 : c - 'a';

            if (curr.child[idx] == null)
                return -1;

            curr = curr.child[idx];
        }

        return curr.weight;
    }
}
```

---

## Complexity

| Metric | Complexity |
|---------|------------|
| Build | O(N × L²) |
| Query | O(P + S) |
| Space | O(N × L²) |

---

## Visualization

Searching

```
Prefix = ap

Suffix = le
```

Trie Search

```
le{ap
```

↓

```
Stored Index

↓

Answer
```

---

## Common Pitfalls

### Wrong

Maintain

```
Prefix Trie

Suffix Trie
```

Then intersect lists.

Complex.

Large memory.

Slow.

---

### Correct

Single combined Trie.

---

## Interview Follow-up

Support

- dynamic insertion
- deletion
- top-k matching indices
- lexicographically smallest word instead of largest index

---

# 15. LeetCode 472 — Concatenated Words

**Difficulty:** Hard

**Companies**

- Google
- Meta
- Amazon
- Microsoft
- Apple

---

## Problem

Find every word that can be formed using at least two smaller words from the dictionary.

Example

```
cat
cats
dog
catsdog
```

Answer

```
catsdog
```

because

```
cats + dog
```

---

## Interview Pattern

Trie

+

DFS

+

Memoization

Very common Google interview problem.

---

## Visualization

Dictionary

```
cat

cats

dog
```

Trie

```
root

↓

c

↓

a

↓

t✓

↓

s✓
```

Searching

```
catsdog
```

```
cat✓

↓

Continue

↓

cats✓

↓

dog✓

↓

Success
```

---

## Approach

1. Insert every word.

2. DFS from index.

3. Whenever Trie reaches terminal node,

start DFS from next position.

4. Use memoization.

---

## Java Solution

```java
class Solution {

    class TrieNode {

        TrieNode[] child = new TrieNode[26];

        boolean end;
    }

    TrieNode root = new TrieNode();

    public List<String> findAllConcatenatedWordsInADict(String[] words) {

        Arrays.sort(words, Comparator.comparingInt(String::length));

        List<String> ans = new ArrayList<>();

        for (String word : words) {

            if (word.length() == 0)
                continue;

            if (search(word, 0))
                ans.add(word);

            insert(word);
        }

        return ans;
    }

    private void insert(String word) {

        TrieNode curr = root;

        for (char c : word.toCharArray()) {

            int idx = c - 'a';

            if (curr.child[idx] == null)
                curr.child[idx] = new TrieNode();

            curr = curr.child[idx];
        }

        curr.end = true;
    }

    private boolean search(String word, int index) {

        if (index == word.length())
            return true;

        TrieNode curr = root;

        for (int i = index; i < word.length(); i++) {

            int idx = word.charAt(i) - 'a';

            if (curr.child[idx] == null)
                return false;

            curr = curr.child[idx];

            if (curr.end && search(word, i + 1))
                return true;
        }

        return false;
    }
}
```

---

## Complexity

| Metric | Complexity |
|---------|------------|
| Build | O(NL) |
| DFS | O(NL²) Worst Case |
| Space | O(NL) |

Memoization improves repeated searches significantly.

---

## Visualization

```
catsdog

↓

cats✓

↓

dog✓

↓

Accepted
```

---

## Common Pitfalls

### Wrong

Checking every split

```
for i

left

right
```

without Trie.

Leads to repeated substring creation.

---

### Wrong

Inserting every word first.

A word could incorrectly match itself.

Correct approach

```
Sort by length

↓

Search

↓

Insert
```

---

## Interview Follow-up

Support

- minimum number of concatenated words
- return actual decomposition
- weighted dictionary
- dynamic updates
- longest concatenated word only

---

# Trie Techniques & Tricks

| Pattern | Used In |
|----------|----------|
| Basic Trie | 208, 1804 |
| Trie + DFS | 211, 212 |
| Trie + Backtracking | 212 |
| Prefix Sum Trie | 677 |
| Reverse Trie | 820, 1032 |
| Bit Trie | 421, 1707, 1938 |
| Offline Queries | 1707 |
| Tree + Trie | 1938 |
| Combined Prefix/Suffix Trie | 745 |
| Trie + Memoization | 472 |

---

# Memory Optimizations

Instead of

```java
TrieNode[] child = new TrieNode[26];
```

use

```java
HashMap<Character, TrieNode>
```

Useful for

- Unicode
- Sparse Trie
- Large alphabets

Trade-off

| Array | HashMap |
|--------|----------|
| Faster | Lower Memory |
| Fixed Alphabet | Dynamic Alphabet |

---

# Common Trie Pitfalls

| Mistake | Fix |
|----------|-----|
| Forgetting end marker | Store `isEnd` |
| DFS explores entire Trie | Prune immediately |
| Duplicate answers | Mark visited words |
| Wrong XOR direction | Prefer opposite bit |
| Normal Trie for suffix problem | Reverse Trie |
| Rebuilding Trie repeatedly | Offline processing |
| Missing deletion counts | Maintain frequency/pass counts |

---

# LLM-Proof Interview Follow-ups

Interviewers frequently extend Trie problems with variations such as:

### Design

- Delete words efficiently
- Count duplicate words
- Dynamic insertion/removal
- Persistent Trie
- Compressed (Radix) Trie

### Search

- Wildcard (`.`)
- Kleene star (`*`)
- Regular-expression style matching
- Approximate matching (edit distance ≤ 1)
- Case-insensitive search

### Autocomplete

- Top-K suggestions
- Most frequently searched words
- Recently searched ranking
- Personalized autocomplete
- Search history weighting

### XOR Trie

- Minimum XOR
- Maximum XOR
- XOR in range
- Online XOR queries
- Dynamic updates

### Prefix/Suffix

- Longest common prefix
- Shortest unique prefix
- Prefix counting
- Suffix matching
- Word frequency by prefix

---

# Complexity Cheat Sheet

| Operation | Time |
|------------|------|
| Insert | O(L) |
| Search | O(L) |
| Prefix Search | O(L) |
| Delete | O(L) |
| Wildcard Search | O(26ᴸ) Worst Case |
| Bit Trie Query | O(32) |
| Reverse Trie Query | O(L) |
| DFS + Trie | O(Board × Branching) |

---

# One-Page Revision Notes

## Recognize the Pattern

| Problem Statement | Technique |
|-------------------|-----------|
| Prefix lookup | Trie |
| Suffix lookup | Reverse Trie |
| Binary XOR | Bit Trie |
| Multiple dictionary words on grid | Trie + DFS |
| Streaming suffix queries | Reverse Trie |
| Prefix + Suffix together | Combined Trie |
| Dictionary decomposition | Trie + DFS + Memoization |
| Online autocomplete | Trie + Cached Suggestions |

---

## FAANG Interview Checklist

Before coding, identify whether the problem involves:

- Prefix matching
- Suffix matching
- Binary bits
- Multiple string searches
- Dynamic dictionary updates
- Streaming input
- Backtracking with pruning
- Offline query optimization

If yes, a Trie-based solution is often expected.

---

# End of Trie Interview Guide

**Total Problems Covered: 15/15**

1. 208. Implement Trie (Prefix Tree)
2. 1804. Implement Trie II
3. 211. Design Add and Search Words Data Structure
4. 648. Replace Words
5. 720. Longest Word in Dictionary
6. 677. Map Sum Pairs
7. 421. Maximum XOR of Two Numbers in an Array
8. 1268. Search Suggestions System
9. 212. Word Search II
10. 820. Short Encoding of Words
11. 1707. Maximum XOR With an Element From Array
12. 1938. Maximum Genetic Difference Query
13. 1032. Stream of Characters
14. 745. Prefix and Suffix Search
15. 472. Concatenated Words

This completes the GitHub-ready Trie interview preparation guide.


