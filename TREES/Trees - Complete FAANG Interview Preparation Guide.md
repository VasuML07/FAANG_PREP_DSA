# Trees - Complete FAANG Interview Preparation Guide (Java)

> Curated Tree interview guide covering Binary Trees, BSTs, Tries, N-ary Trees, Segment Trees, DP on Trees, and advanced traversal patterns through **15 carefully selected LeetCode problems**.

---

# Problem Roadmap

| # | Problem | Difficulty | Pattern | Companies |
|---|----------|------------|----------|-----------|
|1|Binary Tree Inorder Traversal|Easy|DFS, Stack|Google, Amazon, Microsoft|
|2|Binary Tree Level Order Traversal|Medium|BFS|Amazon, Meta, Apple|
|3|Maximum Depth of Binary Tree|Easy|DFS, BFS|Google, Microsoft|
|4|Binary Tree Right Side View|Medium|Level Order BFS|Amazon, Meta|
|5|Validate Binary Search Tree|Medium|BST Validation|Google, Microsoft|
|6|Lowest Common Ancestor of BST|Easy|BST Property|Amazon|
|7|Construct Binary Tree from Preorder and Inorder|Medium|Tree Construction|Meta, Google|
|8|Path Sum III|Medium|Prefix Sum + DFS|Google|
|9|Diameter of Binary Tree|Easy|Tree DP|Amazon|
|10|Binary Tree Maximum Path Sum|Hard|DP on Trees|Meta, Google|
|11|Serialize and Deserialize Binary Tree|Hard|DFS Encoding|Google, Amazon|
|12|Trie (Implement Trie)|Medium|Trie|Google, Microsoft|
|13|N-ary Tree Level Order Traversal|Medium|N-ary BFS|Amazon|
|14|Range Sum Query - Mutable|Medium|Segment Tree|Google|
|15|AVL Tree Design (Interview Variant)|Hard|Balanced BST|Google, Meta|

---

# 1. Binary Tree Inorder Traversal

**LeetCode #94**

**Difficulty**

Easy

**Companies**

Google • Amazon • Microsoft • Adobe

**Pattern**

- DFS
- Stack
- Morris Traversal (advanced)

---

## Problem

Return the inorder traversal of a binary tree.

```
Input

      1
       \
        2
       /
      3

Output

[1,3,2]
```

---

## Approach 1 — Recursive DFS

### Idea

Visit

```
Left
↓

Root
↓

Right
```

This naturally produces inorder sequence.

---

### Visualization

```
        4
      /   \
     2     6
    / \   / \
   1  3  5  7
```

Traversal

```
1
↓

2
↓

3
↓

4
↓

5
↓

6
↓

7
```

---

### Java

```java
class Solution {

    public List<Integer> inorderTraversal(TreeNode root) {

        List<Integer> ans = new ArrayList<>();

        dfs(root, ans);

        return ans;
    }

    private void dfs(TreeNode node, List<Integer> ans){

        if(node == null)
            return;

        dfs(node.left, ans);

        ans.add(node.val);

        dfs(node.right, ans);
    }
}
```

---

## Approach 2 — Iterative Stack

Instead of recursion, simulate function calls.

### Algorithm

```
Go left

Push nodes

Reach null

Pop

Visit

Move right
```

---

### Java

```java
class Solution {

    public List<Integer> inorderTraversal(TreeNode root) {

        List<Integer> ans = new ArrayList<>();

        Stack<TreeNode> stack = new Stack<>();

        TreeNode curr = root;

        while(curr != null || !stack.isEmpty()){

            while(curr != null){

                stack.push(curr);

                curr = curr.left;
            }

            curr = stack.pop();

            ans.add(curr.val);

            curr = curr.right;
        }

        return ans;
    }
}
```

---

## Optimal Complexity

|Method|Time|Space|
|------|----|------|
|Recursive|O(n)|O(h)|
|Iterative|O(n)|O(h)|

h = tree height

---

## FAANG Pattern

This question introduces:

- recursion
- stack simulation
- DFS order

Used later in

- BST validation
- Tree recovery
- Tree iterator

---

## Common Pitfalls

- Forgetting null check
- Visiting root before left
- Infinite loop in iterative solution

---

## LLM Relevance

Hierarchical parsing and syntax-tree traversal in compilers and LLM execution engines rely on inorder/DFS traversal concepts.

---

# 2. Binary Tree Level Order Traversal

**LeetCode #102**

**Difficulty**

Medium

**Companies**

Amazon • Apple • Meta • Google

**Pattern**

- BFS
- Queue

---

## Problem

Return nodes level by level.

Example

```
        3
      /   \
     9     20
          /  \
         15   7
```

Output

```
[
 [3],
 [9,20],
 [15,7]
]
```

---

## Key Idea

Process one level completely before moving to next.

Queue naturally achieves this.

---

### Visualization

```
Queue

[3]

↓

Process

↓

Add children

↓

[9,20]

↓

Process

↓

[15,7]
```

---

## Java

```java
class Solution {

    public List<List<Integer>> levelOrder(TreeNode root) {

        List<List<Integer>> ans = new ArrayList<>();

        if(root == null)
            return ans;

        Queue<TreeNode> queue = new LinkedList<>();

        queue.offer(root);

        while(!queue.isEmpty()){

            int size = queue.size();

            List<Integer> level = new ArrayList<>();

            for(int i=0;i<size;i++){

                TreeNode node = queue.poll();

                level.add(node.val);

                if(node.left!=null)
                    queue.offer(node.left);

                if(node.right!=null)
                    queue.offer(node.right);
            }

            ans.add(level);
        }

        return ans;
    }
}
```

---

## Complexity

|Time|Space|
|----|------|
|O(n)|O(width)|

Worst-case width

```
n/2
```

---

## Pattern Learned

Whenever question asks

- level
- minimum depth
- zigzag
- right side view

Think

```
QUEUE
```

---

## Common Pitfalls

- Using queue size after adding children
- Mixing multiple levels
- Forgetting empty tree

---

## FAANG Importance

Level-order traversal appears in

- serialization
- distributed systems
- filesystem traversal

---

## LLM Relevance

Breadth-first exploration is common when expanding reasoning graphs or traversing hierarchical knowledge structures.

---

# 3. Maximum Depth of Binary Tree

**LeetCode #104**

Difficulty

Easy

Companies

Google • Microsoft • Amazon

Pattern

DFS

Recursion

Tree Height

---

## Problem

Return maximum depth.

Example

```
      1
    /   \
   2     3
  /
 4
```

Depth

```
3
```

---

## Observation

Height

```
1 + max(left,right)
```

---

## Visualization

```
        1

      /   \

     2     3

    /

   4
```

Backtracking

```
4

↓

2

↓

1
```

---

## Java

```java
class Solution {

    public int maxDepth(TreeNode root) {

        if(root == null)
            return 0;

        return 1 + Math.max(
                maxDepth(root.left),
                maxDepth(root.right)
        );
    }
}
```

---

## BFS Alternative

Each level increases depth.

Useful when recursion stack is undesirable.

---

## Complexity

|Method|Time|Space|
|------|----|------|
|DFS|O(n)|O(h)|
|BFS|O(n)|O(width)|

---

## Interview Insight

This recurrence appears repeatedly.

```
height

diameter

balance

maximum path

subtree problems
```

Master this first.

---

## Common Mistakes

- Returning -1
- Confusing node count with edge count
- Ignoring null

---

## LLM Relevance

Tree height computation mirrors dependency depth calculations in reasoning and execution graphs.

---

# 4. Binary Tree Right Side View

**LeetCode #199**

Difficulty

Medium

Companies

Amazon • Meta • Microsoft

Pattern

Level Order

BFS

DFS

---

## Problem

Return the nodes visible from the right side.

Example

```
        1

      /   \

     2     3

      \      \

       5      4
```

Output

```
[1,3,4]
```

---

## BFS Idea

Each level

Take

```
Last node
```

---

### Visualization

```
Level 0

1

↓

Take

↓

1

----------------

Level 1

2 3

↓

Take

↓

3

----------------

Level 2

5 4

↓

Take

↓

4
```

---

## Java

```java
class Solution {

    public List<Integer> rightSideView(TreeNode root) {

        List<Integer> ans = new ArrayList<>();

        if(root == null)
            return ans;

        Queue<TreeNode> queue = new LinkedList<>();

        queue.offer(root);

        while(!queue.isEmpty()){

            int size = queue.size();

            for(int i=0;i<size;i++){

                TreeNode node = queue.poll();

                if(node.left!=null)
                    queue.offer(node.left);

                if(node.right!=null)
                    queue.offer(node.right);

                if(i == size-1)
                    ans.add(node.val);
            }
        }

        return ans;
    }
}
```

---

## DFS Alternative

Traverse

```
Right

↓

Left
```

Store first node at each depth.

Useful for interview follow-ups.

---

## Complexity

|Method|Time|Space|
|------|----|------|
|BFS|O(n)|O(width)|
|DFS|O(n)|O(h)|

---

## Common Pitfalls

- Taking first node instead of last
- Queue size changing during iteration
- Forgetting empty tree

---

## Why FAANG Asks This

Tests whether the candidate can adapt standard BFS to produce customized level-based outputs. This pattern extends naturally to:

- Left View
- Zigzag Traversal
- Average of Levels
- Largest Value in Each Row
- Vertical Order Traversal

---

## LLM Relevance

Selecting the rightmost node at each level is analogous to choosing representative nodes from hierarchical structures, a pattern used in document summarization and hierarchical attention mechanisms.

---

# Progress

- ✅ 1. Binary Tree Inorder Traversal
- ✅ 2. Binary Tree Level Order Traversal
- ✅ 3. Maximum Depth of Binary Tree
- ✅ 4. Binary Tree Right Side View

**Next Part (Part 2)** covers:

- **5. Validate Binary Search Tree**
- **6. Lowest Common Ancestor of a BST**
- **7. Construct Binary Tree from Preorder & Inorder Traversal**
- **8. Path Sum III**

---

# 5. Validate Binary Search Tree

**LeetCode #98**

**Difficulty**

Medium

**Companies**

Google • Microsoft • Amazon • Meta

**Pattern**

- BST
- DFS
- Inorder Traversal
- Bounds Validation

---

## Problem

Determine whether a binary tree is a valid Binary Search Tree (BST).

A BST satisfies:

- Left subtree values < current node
- Right subtree values > current node
- Every subtree must also satisfy BST rules

Example

```
      2
     / \
    1   3

Output:
true
```

Invalid Example

```
      5
     / \
    1   4
       / \
      3   6

Output:
false
```

---

## Approach 1 — Range Validation (Optimal)

Instead of comparing only parent-child nodes, keep track of the valid range.

Every node must satisfy

```
lower < node.val < upper
```

---

### Visualization

```
          8
        /   \
       4     10

Range

8 : (-∞,+∞)

4 : (-∞,8)

10 : (8,+∞)

2 : (-∞,4)

6 : (4,8)
```

---

## Java

```java
class Solution {

    public boolean isValidBST(TreeNode root) {
        return validate(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }

    private boolean validate(TreeNode node, long low, long high) {

        if (node == null)
            return true;

        if (node.val <= low || node.val >= high)
            return false;

        return validate(node.left, low, node.val)
                && validate(node.right, node.val, high);
    }
}
```

---

## Approach 2 — Inorder Traversal

Observation

```
BST inorder traversal

=

Strictly Increasing Sequence
```

Keep previous value.

If

```
current <= previous

↓

Invalid BST
```

---

## Complexity

|Method|Time|Space|
|------|----|------|
|Bounds DFS|O(n)|O(h)|
|Inorder|O(n)|O(h)|

---

## Common Pitfalls

- Comparing only parent and child
- Using Integer.MIN_VALUE instead of long
- Allowing duplicate values

---

## Why FAANG Asks This

Tests

- recursion
- invariants
- tree reasoning

Foundation for

- AVL
- Red-Black Trees
- Database Indexes

---

## LLM Relevance

Constraint propagation across hierarchical structures mirrors rule validation in symbolic reasoning systems.

---

# 6. Lowest Common Ancestor of a Binary Search Tree

**LeetCode #235**

**Difficulty**

Easy

**Companies**

Amazon • Google • Microsoft

**Pattern**

- BST Property
- Binary Search

---

## Problem

Find the lowest common ancestor of two nodes.

Example

```
          6
        /   \
       2     8
      / \   / \
     0  4  7  9
       / \
      3   5

p = 2

q = 8

Answer = 6
```

---

## Observation

If both nodes lie

Left

↓

Move Left

Both Right

↓

Move Right

Otherwise

↓

Current node is answer.

---

### Visualization

```
          6

      p←     →q

Different sides

↓

Answer = 6
```

---

## Java

```java
class Solution {

    public TreeNode lowestCommonAncestor(TreeNode root,
                                         TreeNode p,
                                         TreeNode q) {

        while (root != null) {

            if (p.val < root.val && q.val < root.val)
                root = root.left;

            else if (p.val > root.val && q.val > root.val)
                root = root.right;

            else
                return root;
        }

        return null;
    }
}
```

---

## Complexity

|Time|Space|
|----|------|
|O(h)|O(1)|

Balanced BST

```
O(log n)
```

Worst case

```
O(n)
```

---

## Pattern Learned

Binary Search on Trees.

Instead of

```
mid

↓

left/right
```

Use

```
root

↓

left/right
```

---

## Common Pitfalls

- Forgetting current node may be LCA
- Traversing entire tree unnecessarily
- Ignoring BST property

---

## Why FAANG Asks This

Checks whether candidates recognize structural properties instead of brute-force DFS.

---

## LLM Relevance

Finding the nearest shared ancestor resembles identifying common contexts within hierarchical knowledge graphs.

---

# 7. Construct Binary Tree from Preorder and Inorder Traversal

**LeetCode #105**

**Difficulty**

Medium

**Companies**

Google • Meta • Amazon

**Pattern**

- Divide & Conquer
- HashMap
- Tree Construction

---

## Problem

Given preorder and inorder traversal, reconstruct the original tree.

Example

```
Preorder

3 9 20 15 7

Inorder

9 3 15 20 7
```

Output

```
      3
     / \
    9  20
      /  \
     15   7
```

---

## Key Observation

Preorder

```
Root

↓

Left

↓

Right
```

Inorder

```
Left

↓

Root

↓

Right
```

Root splits inorder into

```
Left subtree

Right subtree
```

---

### Visualization

```
Preorder

3 | 9 | 20 15 7

Root = 3

Inorder

9 |3|15 20 7

Split

Left

9

Right

15 20 7
```

---

## Java

```java
class Solution {

    private int preIndex = 0;

    private Map<Integer, Integer> map = new HashMap<>();

    public TreeNode buildTree(int[] preorder, int[] inorder) {

        for (int i = 0; i < inorder.length; i++)
            map.put(inorder[i], i);

        return build(preorder, 0, inorder.length - 1);
    }

    private TreeNode build(int[] preorder, int left, int right) {

        if (left > right)
            return null;

        int rootValue = preorder[preIndex++];

        TreeNode root = new TreeNode(rootValue);

        int mid = map.get(rootValue);

        root.left = build(preorder, left, mid - 1);

        root.right = build(preorder, mid + 1, right);

        return root;
    }
}
```

---

## Complexity

|Time|Space|
|----|------|
|O(n)|O(n)|

HashMap removes repeated searching.

Without HashMap

```
O(n²)
```

---

## Common Pitfalls

- Forgetting preorder index
- Recomputing inorder search
- Wrong recursive boundaries

---

## Why FAANG Asks This

Excellent test of recursion, divide-and-conquer, and traversal understanding.

---

## LLM Relevance

Tree reconstruction resembles rebuilding parse trees and abstract syntax trees from serialized representations.

---

# 8. Path Sum III

**LeetCode #437**

**Difficulty**

Medium

**Companies**

Google • Amazon

**Pattern**

- DFS
- Prefix Sum
- HashMap

---

## Problem

Count paths whose sum equals target.

A path

- goes downward only
- may start anywhere
- may end anywhere

Example

```
        10
       /  \
      5   -3
     / \    \
    3   2   11
```

Target

```
8
```

Answer

```
3
```

---

## Naive Idea

Start DFS from every node.

Complexity

```
O(n²)
```

Too slow.

---

## Optimal Idea — Prefix Sum

Maintain

```
currentSum
```

Needed previous prefix

```
currentSum - target
```

If already seen

↓

A valid path exists.

---

### Visualization

```
Running Sum

10

↓

15

↓

18

Need

18-8=10

Already exists

↓

One valid path
```

---

## Java

```java
class Solution {

    public int pathSum(TreeNode root, int targetSum) {

        Map<Long, Integer> prefix = new HashMap<>();

        prefix.put(0L, 1);

        return dfs(root, 0L, targetSum, prefix);
    }

    private int dfs(TreeNode node,
                    long curr,
                    int target,
                    Map<Long, Integer> prefix) {

        if (node == null)
            return 0;

        curr += node.val;

        int count = prefix.getOrDefault(curr - target, 0);

        prefix.put(curr,
                prefix.getOrDefault(curr, 0) + 1);

        count += dfs(node.left, curr, target, prefix);

        count += dfs(node.right, curr, target, prefix);

        prefix.put(curr, prefix.get(curr) - 1);

        return count;
    }
}
```

---

## Complexity

|Time|Space|
|----|------|
|O(n)|O(n)|

---

## Key Insight

Prefix sums work on trees exactly like arrays, but the HashMap represents the current root-to-node path.

---

## Common Pitfalls

- Forgetting backtracking
- Using int instead of long
- Sharing prefix counts across sibling branches

---

## Why FAANG Asks This

Combines multiple advanced ideas:

- DFS
- Prefix Sum
- Backtracking
- HashMap optimization

Recognizing this pattern is a strong indicator of interview readiness.

---

## LLM Relevance

Maintaining cumulative context along a traversal path is conceptually similar to preserving execution state during recursive reasoning.

---

# Progress

- ✅ 1. Binary Tree Inorder Traversal
- ✅ 2. Binary Tree Level Order Traversal
- ✅ 3. Maximum Depth of Binary Tree
- ✅ 4. Binary Tree Right Side View
- ✅ 5. Validate Binary Search Tree
- ✅ 6. Lowest Common Ancestor of BST
- ✅ 7. Construct Binary Tree from Preorder & Inorder
- ✅ 8. Path Sum III

**Next Part (Part 3)** covers:

- **9. Diameter of Binary Tree**
- **10. Binary Tree Maximum Path Sum**
- **11. Serialize and Deserialize Binary Tree**
- **12. Implement Trie (Prefix Tree)**

  ---

# 9. Diameter of Binary Tree

**LeetCode #543**

**Difficulty**

Easy

**Companies**

Amazon • Google • Microsoft • Apple

**Pattern**

- Tree DP
- Postorder DFS
- Height Computation

---

## Problem

Given the root of a binary tree, return the **diameter** of the tree.

The diameter is the **number of edges** in the longest path between any two nodes.

Example

```
        1
       / \
      2   3
     / \
    4   5

Output

3
```

Longest Path

```
4 → 2 → 1 → 3
```

---

## Key Observation

At every node

```
Longest path through node

=

Left Height

+

Right Height
```

The answer may or may not pass through the root.

---

### Visualization

```
          1

       /     \

      2       3

    /   \

   4     5
```

For node 2

```
Left Height = 1

Right Height = 1

Diameter = 2
```

For node 1

```
Left Height = 2

Right Height = 1

Diameter = 3
```

Maximum

```
3
```

---

## Java

```java
class Solution {

    private int diameter = 0;

    public int diameterOfBinaryTree(TreeNode root) {

        height(root);

        return diameter;
    }

    private int height(TreeNode node) {

        if (node == null)
            return 0;

        int left = height(node.left);

        int right = height(node.right);

        diameter = Math.max(diameter, left + right);

        return 1 + Math.max(left, right);
    }
}
```

---

## Complexity

|Time|Space|
|----|------|
|O(n)|O(h)|

---

## Pattern Learned

Whenever a question asks

- diameter
- balance
- longest path
- subtree answer

Think

```
Postorder DFS
```

because children must be solved first.

---

## Common Pitfalls

- Returning node count instead of edge count
- Updating diameter after return
- Forgetting global variable

---

## Why FAANG Asks This

Tests Tree Dynamic Programming.

Many difficult tree problems use this exact recurrence.

---

## LLM Relevance

Computing longest dependency chains is analogous to finding reasoning depth in hierarchical execution graphs.

---

# 10. Binary Tree Maximum Path Sum

**LeetCode #124**

**Difficulty**

Hard

**Companies**

Google • Meta • Amazon • Microsoft

**Pattern**

- Tree DP
- DFS
- Maximum Contribution

---

## Problem

Find the maximum possible path sum.

The path

- may start anywhere
- may end anywhere
- cannot revisit nodes

Example

```
       -10
       /  \
      9   20
         / \
        15  7

Output

42
```

Path

```
15 → 20 → 7
```

---

## Observation

Negative branches should never be extended.

Contribution

```
max(0, childContribution)
```

---

### Visualization

```
           20

        /      \

      15        7

Contribution

15

+

20

+

7

=

42
```

---

## Java

```java
class Solution {

    private int answer = Integer.MIN_VALUE;

    public int maxPathSum(TreeNode root) {

        dfs(root);

        return answer;
    }

    private int dfs(TreeNode node) {

        if (node == null)
            return 0;

        int left = Math.max(0, dfs(node.left));

        int right = Math.max(0, dfs(node.right));

        answer = Math.max(answer,
                left + right + node.val);

        return node.val + Math.max(left, right);
    }
}
```

---

## Complexity

|Time|Space|
|----|------|
|O(n)|O(h)|

---

## Important Insight

Each node returns

```
Best branch
```

but updates

```
Best complete path
```

Those are **different values**.

---

## Common Pitfalls

- Returning left + right + node
- Including negative paths
- Initializing answer as 0

---

## Why FAANG Asks This

Classic Tree DP.

Excellent test of recursive reasoning.

---

## LLM Relevance

Selecting optimal hierarchical reasoning paths while pruning poor branches resembles beam-search pruning strategies.

---

# 11. Serialize and Deserialize Binary Tree

**LeetCode #297**

**Difficulty**

Hard

**Companies**

Google • Amazon • Meta • Netflix

**Pattern**

- DFS
- Tree Encoding
- Recursion

---

## Problem

Convert a binary tree into a string and reconstruct it.

Example

```
        1
      /   \
     2     3
          / \
         4   5
```

Serialized

```
1,2,#,#,3,4,#,#,5,#,#
```

---

## Key Idea

Use preorder traversal.

Store

```
#

for

null nodes
```

Without null markers reconstruction is impossible.

---

### Serialization Visualization

```
        1

↓

1

↓

2

↓

#

↓

#

↓

3

↓

4

↓

#

↓

#
```

---

## Java

```java
public class Codec {

    public String serialize(TreeNode root) {

        StringBuilder sb = new StringBuilder();

        preorder(root, sb);

        return sb.toString();
    }

    private void preorder(TreeNode node,
                          StringBuilder sb) {

        if (node == null) {

            sb.append("#,");

            return;
        }

        sb.append(node.val).append(",");

        preorder(node.left, sb);

        preorder(node.right, sb);
    }

    public TreeNode deserialize(String data) {

        Queue<String> queue =
                new LinkedList<>(
                        Arrays.asList(data.split(",")));

        return build(queue);
    }

    private TreeNode build(Queue<String> queue) {

        String value = queue.poll();

        if (value.equals("#"))
            return null;

        TreeNode root =
                new TreeNode(Integer.parseInt(value));

        root.left = build(queue);

        root.right = build(queue);

        return root;
    }
}
```

---

## Complexity

|Time|Space|
|----|------|
|O(n)|O(n)|

---

## Why Null Markers Matter

Without

```
#
```

these two trees serialize identically.

```
1
 \
 2
```

and

```
  1

 /

2
```

---

## Common Pitfalls

- Omitting null markers
- Wrong traversal order
- Using inorder alone

---

## Why FAANG Asks This

Frequently appears in

- distributed systems
- storage engines
- networking
- cache synchronization

---

## LLM Relevance

Tree serialization is directly related to structured data exchange, AST storage, and hierarchical knowledge representation.

---

# 12. Implement Trie (Prefix Tree)

**LeetCode #208**

**Difficulty**

Medium

**Companies**

Google • Microsoft • Amazon • Meta

**Pattern**

- Trie
- Prefix Search
- Character Trees

---

## Problem

Implement

```
insert()

search()

startsWith()
```

---

### Visualization

Insert

```
apple
```

Trie

```
root

 |

 a

 |

 p

 |

 p

 |

 l

 |

 e*
```

---

## Java

```java
class Trie {

    class TrieNode {

        TrieNode[] children = new TrieNode[26];

        boolean isWord;
    }

    private TrieNode root;

    public Trie() {

        root = new TrieNode();
    }

    public void insert(String word) {

        TrieNode node = root;

        for(char c : word.toCharArray()){

            int index = c - 'a';

            if(node.children[index] == null)
                node.children[index] = new TrieNode();

            node = node.children[index];
        }

        node.isWord = true;
    }

    public boolean search(String word) {

        TrieNode node = traverse(word);

        return node != null && node.isWord;
    }

    public boolean startsWith(String prefix) {

        return traverse(prefix) != null;
    }

    private TrieNode traverse(String word){

        TrieNode node = root;

        for(char c : word.toCharArray()){

            int index = c - 'a';

            if(node.children[index] == null)
                return null;

            node = node.children[index];
        }

        return node;
    }
}
```

---

## Complexity

|Operation|Time|
|---------|----|
|Insert|O(L)|
|Search|O(L)|
|Prefix|O(L)|

L = word length.

---

## Applications

- Search engines
- IDE autocomplete
- Spell checking
- IP routing
- Dictionary lookup

---

## Common Pitfalls

- Forgetting end-of-word marker
- Confusing prefix with complete word
- Creating duplicate nodes

---

## Why FAANG Asks This

Trie is one of Google's favorite data structures because of its importance in search systems.

---

## LLM Relevance

Token dictionaries, vocabulary lookup, autocomplete, and prefix-based decoding are natural Trie applications.

---

# Progress

- ✅ 1. Binary Tree Inorder Traversal
- ✅ 2. Binary Tree Level Order Traversal
- ✅ 3. Maximum Depth of Binary Tree
- ✅ 4. Binary Tree Right Side View
- ✅ 5. Validate Binary Search Tree
- ✅ 6. Lowest Common Ancestor of BST
- ✅ 7. Construct Binary Tree from Preorder & Inorder
- ✅ 8. Path Sum III
- ✅ 9. Diameter of Binary Tree
- ✅ 10. Binary Tree Maximum Path Sum
- ✅ 11. Serialize and Deserialize Binary Tree
- ✅ 12. Implement Trie

**Next Part (Final Part)** covers:

- **13. N-ary Tree Level Order Traversal**
- **14. Range Sum Query – Mutable (Segment Tree)**
- **15. AVL Tree Design (Interview Variant)**
- **Final Complexity Cheat Sheet**
- **FAANG Revision Notes**

  ---

# 13. N-ary Tree Level Order Traversal

**LeetCode #429**

**Difficulty**

Medium

**Companies**

Amazon • Google • Microsoft

**Pattern**

- BFS
- Queue
- N-ary Tree Traversal

---

## Problem

Given the root of an N-ary tree, return its level-order traversal.

Unlike a binary tree, every node may have **0 to N children**.

Example

```
            1
        /   |   \
       3    2    4
      / \
     5   6
```

Output

```
[
 [1],
 [3,2,4],
 [5,6]
]
```

---

## Observation

Binary tree BFS still works.

The only difference is that instead of

```
left

right
```

we iterate through

```
children list
```

---

### Visualization

```
Queue

[1]

↓

Process

↓

Add

3

2

4

↓

Queue

[3,2,4]

↓

Process

↓

5

6
```

---

## Java

```java
class Solution {

    public List<List<Integer>> levelOrder(Node root) {

        List<List<Integer>> answer = new ArrayList<>();

        if (root == null)
            return answer;

        Queue<Node> queue = new LinkedList<>();

        queue.offer(root);

        while (!queue.isEmpty()) {

            int size = queue.size();

            List<Integer> level = new ArrayList<>();

            for (int i = 0; i < size; i++) {

                Node node = queue.poll();

                level.add(node.val);

                for (Node child : node.children)
                    queue.offer(child);
            }

            answer.add(level);
        }

        return answer;
    }
}
```

---

## Complexity

|Time|Space|
|----|------|
|O(n)|O(width)|

---

## Pattern Extension

This exact BFS works for

- Folder structures
- Organization charts
- XML
- JSON trees
- DOM trees

---

## Common Pitfalls

- Forgetting null root
- Forgetting to iterate all children
- Mixing multiple levels

---

## Why FAANG Asks This

Generalizes binary tree algorithms to arbitrary hierarchical structures frequently used in production systems.

---

## LLM Relevance

Knowledge graphs, document outlines, HTML DOMs, and agent planning trees are naturally modeled as N-ary trees.

---

# 14. Range Sum Query – Mutable (Segment Tree)

**LeetCode #307**

**Difficulty**

Medium

**Companies**

Google • Amazon • Meta

**Pattern**

- Segment Tree
- Divide & Conquer
- Range Query
- Point Update

---

## Problem

Support two operations efficiently.

```
update(index, value)

sumRange(left, right)
```

Naive approach

```
Update

O(1)

Query

O(n)
```

Too slow.

---

## Segment Tree Idea

Store sums for ranges.

```
                 [0..7]

              /          \

         [0..3]         [4..7]

        /     \         /     \

    [0..1] [2..3] [4..5] [6..7]
```

Each node stores

```
Sum of interval
```

---

## Operations

### Update

```
Leaf

↓

Parent

↓

Root
```

### Query

Visit only overlapping intervals.

---

## Java

```java
class NumArray {

    private int[] tree;
    private int[] nums;
    private int n;

    public NumArray(int[] nums) {

        this.nums = nums;

        n = nums.length;

        tree = new int[4 * n];

        build(1, 0, n - 1);
    }

    private void build(int node, int left, int right) {

        if (left == right) {

            tree[node] = nums[left];

            return;
        }

        int mid = (left + right) / 2;

        build(node * 2, left, mid);

        build(node * 2 + 1, mid + 1, right);

        tree[node] = tree[node * 2] + tree[node * 2 + 1];
    }

    public void update(int index, int value) {

        update(1, 0, n - 1, index, value);
    }

    private void update(int node,
                        int left,
                        int right,
                        int index,
                        int value) {

        if (left == right) {

            tree[node] = value;

            return;
        }

        int mid = (left + right) / 2;

        if (index <= mid)
            update(node * 2, left, mid, index, value);
        else
            update(node * 2 + 1, mid + 1, right, index, value);

        tree[node] = tree[node * 2] + tree[node * 2 + 1];
    }

    public int sumRange(int left, int right) {

        return query(1, 0, n - 1, left, right);
    }

    private int query(int node,
                      int left,
                      int right,
                      int ql,
                      int qr) {

        if (ql <= left && right <= qr)
            return tree[node];

        if (right < ql || left > qr)
            return 0;

        int mid = (left + right) / 2;

        return query(node * 2, left, mid, ql, qr)
                +
               query(node * 2 + 1, mid + 1, right, ql, qr);
    }
}
```

---

## Complexity

|Operation|Time|
|----------|----|
|Build|O(n)|
|Update|O(log n)|
|Range Query|O(log n)|

---

## Pattern Learned

Whenever you see

- multiple updates
- multiple range queries

Think

```
Segment Tree

Fenwick Tree
```

---

## Common Pitfalls

- Wrong midpoint
- Incorrect overlap conditions
- Allocating tree too small

---

## Why FAANG Asks This

Appears in

- databases
- analytics
- search indexing
- gaming
- financial systems

---

## LLM Relevance

Hierarchical aggregation over ranges is analogous to efficient aggregation of token statistics and distributed computation.

---

# 15. AVL Tree Design (Interview Variant)

> Although AVL Trees are not a dedicated LeetCode problem, balanced BST implementation and rotations are common interview follow-ups after BST questions.

**Difficulty**

Hard

**Companies**

Google • Meta • Microsoft

**Pattern**

- Self-Balancing BST
- Rotations
- Height Maintenance

---

## Problem

Maintain a Binary Search Tree such that

```
Height Difference

≤ 1
```

for every node.

---

## Balance Factor

```
Balance

=

Height(Left)

-

Height(Right)
```

Allowed

```
-1

0

1
```

---

## Rotations

### Left Rotation

```
    x
     \
      y
     / \
    T2  T3

↓

      y
     / \
    x  T3
     \
     T2
```

---

### Right Rotation

```
       y
      /
     x
    / \
   T1 T2

↓

      x
     / \
    T1  y
       /
      T2
```

---

## Four Cases

|Case|Rotation|
|----|---------|
|LL|Right Rotation|
|RR|Left Rotation|
|LR|Left then Right|
|RL|Right then Left|

---

## Simplified Rotation Code

```java
class Node {

    int val;

    int height;

    Node left;

    Node right;
}

private Node rightRotate(Node y){

    Node x = y.left;

    Node t2 = x.right;

    x.right = y;

    y.left = t2;

    y.height = Math.max(height(y.left),
                        height(y.right)) + 1;

    x.height = Math.max(height(x.left),
                        height(x.right)) + 1;

    return x;
}

private Node leftRotate(Node x){

    Node y = x.right;

    Node t2 = y.left;

    y.left = x;

    x.right = t2;

    x.height = Math.max(height(x.left),
                        height(x.right)) + 1;

    y.height = Math.max(height(y.left),
                        height(y.right)) + 1;

    return y;
}
```

---

## Complexity

|Operation|Time|
|----------|----|
|Search|O(log n)|
|Insert|O(log n)|
|Delete|O(log n)|

---

## Why AVL Matters

Without balancing

```
BST

↓

Linked List

↓

O(n)
```

AVL guarantees

```
O(log n)
```

---

## Common Pitfalls

- Updating height after rotation
- Missing LR and RL cases
- Incorrect balance factor calculation

---

## Why FAANG Asks This

Tests understanding beyond standard BSTs and evaluates knowledge of balanced search trees used in real systems.

---

## LLM Relevance

Balanced hierarchical structures improve lookup efficiency, similar to optimized indexing strategies used in large-scale AI infrastructure.

---

# Final Complexity Cheat Sheet

|Problem|Pattern|Time|Space|
|--------|-------|----|------|
|Inorder Traversal|DFS|O(n)|O(h)|
|Level Order Traversal|BFS|O(n)|O(width)|
|Maximum Depth|DFS|O(n)|O(h)|
|Right Side View|BFS / DFS|O(n)|O(width)|
|Validate BST|DFS + Bounds|O(n)|O(h)|
|LCA of BST|BST Search|O(h)|O(1)|
|Construct Tree|Divide & Conquer|O(n)|O(n)|
|Path Sum III|DFS + Prefix Sum|O(n)|O(n)|
|Diameter|Tree DP|O(n)|O(h)|
|Maximum Path Sum|Tree DP|O(n)|O(h)|
|Serialize Tree|DFS|O(n)|O(n)|
|Trie|Prefix Tree|O(L)|O(L)|
|N-ary Level Order|BFS|O(n)|O(width)|
|Segment Tree|Range Query|O(log n)|O(n)|
|AVL Tree|Balanced BST|O(log n)|O(log n)|

---

# FAANG Last-Minute Revision

## Core Traversal Patterns

|Question asks...|Think...|
|----------------|---------|
|Visit every node|DFS|
|Level-wise output|BFS|
|Height / Balance|Postorder DFS|
|BST validation|Bounds or Inorder|
|Tree reconstruction|Preorder + Inorder|
|Longest path|Tree DP|
|Prefix-based words|Trie|
|Range queries with updates|Segment Tree|
|Self-balancing search|AVL Tree|
|Organization charts / DOM|N-ary Tree|

---

# Frequently Reused Interview Patterns

- Recursive DFS
- Iterative DFS using Stack
- BFS using Queue
- Postorder Dynamic Programming
- Prefix Sum on Trees
- Divide and Conquer
- HashMap + DFS
- Binary Search Tree properties
- Serialization / Deserialization
- Self-balancing Trees
- Range Query Data Structures

---

# Recommended Solving Order

1. Binary Tree Inorder Traversal
2. Maximum Depth of Binary Tree
3. Binary Tree Level Order Traversal
4. Right Side View
5. Validate BST
6. Lowest Common Ancestor of BST
7. Construct Binary Tree
8. Path Sum III
9. Diameter of Binary Tree
10. Binary Tree Maximum Path Sum
11. Serialize & Deserialize Binary Tree
12. Implement Trie
13. N-ary Tree Level Order Traversal
14. Range Sum Query – Mutable
15. AVL Tree Design

---

# Completion Checklist

- Binary Tree Traversals
- Binary Search Trees
- Recursive DFS
- Iterative DFS
- Breadth-First Search
- Dynamic Programming on Trees
- Divide & Conquer
- Prefix Sum on Trees
- Serialization
- Trie
- N-ary Trees
- Segment Tree
- AVL Tree
- FAANG Company Tags
- LLM Relevance Notes
- Production-ready Java Solutions

**End of Guide**


