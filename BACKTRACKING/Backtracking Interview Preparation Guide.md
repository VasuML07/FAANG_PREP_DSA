# Backtracking — Complete FAANG Interview Preparation Guide (Java)

> Production-ready GitHub markdown for interview preparation.
>
> **Language:** Java
>
> **Questions:** 15 LeetCode Problems
>
> This guide focuses on solving interview questions rather than teaching theory separately. Every problem introduces the required concepts naturally.

---

# Table of Contents

## Foundation

- [Backtracking Patterns Cheat Sheet](#backtracking-patterns-cheat-sheet)
- [FAANG Company Trends](#faang-company-trends)

## Problems

| # | Problem | Difficulty |
|---|----------|------------|
|1|46. Permutations|Medium|
|2|78. Subsets|Medium|
|3|77. Combinations|Medium|
|4|39. Combination Sum|Medium|
|5|40. Combination Sum II|Medium|
|6|17. Letter Combinations of a Phone Number|Medium|
|7|131. Palindrome Partitioning|Medium|
|8|79. Word Search|Medium|
|9|51. N-Queens|Hard|
|10|37. Sudoku Solver|Hard|
|11|93. Restore IP Addresses|Medium|
|12|90. Subsets II|Medium|
|13|47. Permutations II|Medium|
|14|698. Partition to K Equal Sum Subsets|Medium|
|15|282. Expression Add Operators|Hard|

---

# Backtracking Patterns Cheat Sheet

| Pattern | Recognition | State | Undo Step |
|----------|-------------|-------|-----------|
|Choose / Don't Choose|Subset questions|Current index|Remove chosen element|
|For-loop Expansion|Combination generation|Loop from current index|Remove last element|
|Visited Array|Permutation problems|Visited boolean[]|visited[i]=false|
|Constraint Checking|Sudoku / N-Queens|Board state|Restore board|
|Grid DFS + Backtracking|Word Search|Current cell|Restore character|
|Partition Building|Palindrome Partitioning|Current partition|Remove last partition|
|Target Reduction|Combination Sum|Remaining target|Remove current choice|
|Path Construction|Maze / Graph|Current path|Pop node|

---

## Common Template

```java
void backtrack(parameters){

    if(baseCase){
        answer.add(currentState);
        return;
    }

    for(each choice){

        choose();

        backtrack(nextState);

        undoChoice();
    }

}
```

---

## Important Interview Rules

### Rule 1

Always identify

```
Current State
↓

Available Choices
↓

Constraint

↓

Base Case
```

---

### Rule 2

Never forget the Undo Step.

```
Choose

↓

Explore

↓

Undo
```

Missing Undo is the #1 interview mistake.

---

### Rule 3

Never mutate shared state permanently.

Instead

```
path.add(x);

dfs(...);

path.remove(path.size()-1);
```

---

### Rule 4

Whenever duplicate numbers exist

Think:

```
Need Sorting?

Need Skip Duplicate?

Need Visited Array?
```

---

### Rule 5

Whenever board problems appear

Think

```
Rows

Columns

Diagonals

Visited Cells
```

---

# FAANG Company Trends

| Company | Favorite Backtracking Problems |
|----------|-------------------------------|
|Google|N Queens, Sudoku, Word Search, Expression Add Operators|
|Meta|Permutations, Subsets, Combination Sum|
|Amazon|Word Search, Partitioning, Phone Combinations|
|Apple|Palindrome Partitioning, Combination Sum|
|Netflix|Constraint Satisfaction, N-Queens|

---

# Problem 1 — LeetCode 46. Permutations

**Difficulty:** Medium

**Pattern**

Permutation Generation using Visited Array

**Asked By**

Google • Meta • Amazon

**LLM-Proof**

✅ Moderate

Requires understanding of state restoration.

---

## Problem Statement

Given an array of distinct integers, return all possible permutations.

Example

```
Input

[1,2,3]

Output

[
[1,2,3],
[1,3,2],
[2,1,3],
...
]
```

---

## Why This Tests Backtracking

Unlike subsets, every element must appear exactly once.

Interviewer wants to see if you understand

- visited array
- recursion state
- restoration

---

## Decision Tree

```text
            []
      /      |      \
     1       2       3
   /  \     / \     / \
 2    3    1   3   1   2
```

Each level chooses one unused number.

---

## Approach

Maintain

```
Current permutation

Visited array
```

For every unused number

```
Choose

↓

Visit

↓

Undo
```

---

## Java Solution

```java
class Solution {

    List<List<Integer>> ans = new ArrayList<>();

    public List<List<Integer>> permute(int[] nums) {

        boolean[] visited = new boolean[nums.length];

        backtrack(nums, visited, new ArrayList<>());

        return ans;
    }

    private void backtrack(int[] nums,
                           boolean[] visited,
                           List<Integer> current){

        if(current.size()==nums.length){

            ans.add(new ArrayList<>(current));
            return;
        }

        for(int i=0;i<nums.length;i++){

            if(visited[i])
                continue;

            visited[i]=true;
            current.add(nums[i]);

            backtrack(nums,visited,current);

            current.remove(current.size()-1);
            visited[i]=false;
        }
    }
}
```

---

## Complexity

Time

```
O(N × N!)
```

Reason

```
N! permutations

Copy each permutation

O(N)
```

Space

```
O(N)
```

Recursion stack + visited.

---

## Common Mistakes

❌ Forgetting

```
visited[i]=false;
```

❌ Reusing same list reference.

---

## Interview Insight

This is the canonical permutation problem.

Mastering it makes

- Permutations II
- N Queens
- Sudoku

significantly easier.

---

# Problem 2 — LeetCode 78. Subsets

**Difficulty**

Medium

**Pattern**

Choose / Don't Choose

**Asked By**

Meta • Amazon • Apple

**LLM-Proof**

No

Classic template.

---

## Problem Statement

Return every possible subset.

Example

```
Input

[1,2]

Output

[]

[1]

[2]

[1,2]
```

---

## Decision Tree

```text
          []
        /     \
      Take   Skip

Every number doubles possibilities.
```

---

## Observation

Unlike permutations

Order

does NOT matter.

No visited array.

---

## Java Solution

```java
class Solution {

    List<List<Integer>> ans = new ArrayList<>();

    public List<List<Integer>> subsets(int[] nums) {

        dfs(0, nums, new ArrayList<>());

        return ans;
    }

    private void dfs(int index,
                     int[] nums,
                     List<Integer> current){

        ans.add(new ArrayList<>(current));

        for(int i=index;i<nums.length;i++){

            current.add(nums[i]);

            dfs(i+1,nums,current);

            current.remove(current.size()-1);
        }
    }
}
```

---

## Complexity

Time

```
O(N×2^N)
```

Space

```
O(N)
```

---

## Common Mistakes

❌ Starting loop from zero

Instead

```
for(i=index...)
```

---

## Interview Insight

Subsets are the easiest gateway into backtracking.

Interviewers often expect candidates to derive this naturally.

---

# Problem 3 — LeetCode 77. Combinations

**Difficulty**

Medium

**Pattern**

For-loop Expansion

**Asked By**

Google • Meta

**LLM-Proof**

No

---

## Problem Statement

Return every combination of k numbers from

```
1...n
```

Example

```
n=4

k=2

Output

[1,2]

[1,3]

[1,4]

[2,3]

...
```

---

## Visualization

```text
Choose first

1

↓

Choose second

2

3

4

↓

Backtrack
```

---

## Key Observation

Unlike subsets

We stop when

```
size==k
```

---

## Java Solution

```java
class Solution {

    List<List<Integer>> ans = new ArrayList<>();

    public List<List<Integer>> combine(int n, int k) {

        dfs(1,n,k,new ArrayList<>());

        return ans;
    }

    private void dfs(int start,
                     int n,
                     int k,
                     List<Integer> current){

        if(current.size()==k){

            ans.add(new ArrayList<>(current));
            return;
        }

        for(int i=start;i<=n;i++){

            current.add(i);

            dfs(i+1,n,k,current);

            current.remove(current.size()-1);
        }
    }
}
```

---

## Complexity

```
O(C(n,k) × k)
```

Space

```
O(k)
```

---

## Common Mistake

Stopping only after loop ends.

Correct stopping condition

```
current.size()==k
```

---

## Interview Insight

This introduces

"generate fixed-length answers"

which appears frequently in Google interviews.

---

# Problem 4 — LeetCode 39. Combination Sum

**Difficulty**

Medium

**Pattern**

Target Reduction

**Asked By**

Amazon • Meta • Apple

**LLM-Proof**

✅ Yes

Requires identifying when reuse of elements is allowed.

---

## Problem Statement

Given distinct candidates and a target, return all unique combinations whose sum equals the target.

Each number may be chosen unlimited times.

Example

```
Input

[2,3,6,7]

Target=7

Output

[2,2,3]

[7]
```

---

## Decision Tree

```text
Target = 7

             7
         /    |    \
        2     3     6

Target

5

↓

3

↓

1

↓

Backtrack
```

---

## Why This Is Different

Unlike combinations,

numbers can be reused.

Notice

```java
dfs(i,...)
```

NOT

```java
dfs(i+1,...)
```

That single difference changes the entire recursion.

---

## Approach

At every level

```
Choose candidate

↓

Reduce target

↓

Continue from SAME index

↓

Undo
```

---

## Java Solution

```java
class Solution {

    List<List<Integer>> ans = new ArrayList<>();

    public List<List<Integer>> combinationSum(int[] candidates,
                                              int target) {

        dfs(0,candidates,target,new ArrayList<>());

        return ans;
    }

    private void dfs(int index,
                     int[] nums,
                     int target,
                     List<Integer> current){

        if(target==0){

            ans.add(new ArrayList<>(current));
            return;
        }

        if(target<0)
            return;

        for(int i=index;i<nums.length;i++){

            current.add(nums[i]);

            dfs(i,nums,target-nums[i],current);

            current.remove(current.size()-1);
        }
    }
}
```

---

## Complexity

Worst Case

```
O(N^(Target/minValue))
```

Actual complexity depends heavily on pruning and target size.

Space

```
O(Target)
```

---

## Common Mistakes

❌ Using

```java
dfs(i+1,...)
```

This incorrectly prevents reuse.

❌ Forgetting

```java
if(target<0)
    return;
```

leading to unnecessary recursion.

---

## Interview Insight

This problem evaluates whether you can distinguish between:

| Problem | Recursive Call |
|----------|----------------|
|Combinations|`dfs(i + 1, ...)`|
|Combination Sum|`dfs(i, ...)`|

Recognizing this distinction is a common interview expectation at Amazon and Meta.

---

# End of Part 1

**Covered Problems**

1. Permutations
2. Subsets
3. Combinations
4. Combination Sum

The next section (Part 2) continues with:

- Combination Sum II
- Letter Combinations of a Phone Number
- Palindrome Partitioning
- Word Search

---

# Problem 5 — LeetCode 40. Combination Sum II

**Difficulty**

Medium

**Pattern**

Combination Generation + Duplicate Elimination

**Asked By**

Google • Amazon • Apple

**LLM-Proof**

✅ Yes

Requires understanding duplicate pruning rather than memorizing the Combination Sum template.

---

## Problem Statement

Given a collection of candidate numbers (that may contain duplicates) and a target, return all **unique** combinations where the candidate numbers sum to the target.

Each number may be used **only once**.

Example

```text
Input

candidates = [10,1,2,7,6,1,5]
target = 8

Output

[
 [1,1,6],
 [1,2,5],
 [1,7],
 [2,6]
]
```

---

## Why This Tests Backtracking

Unlike **Combination Sum**, here:

- duplicate numbers exist
- each element can only be used once
- duplicate answers must not appear

This is primarily a **pruning** problem.

---

## Decision Process

```text
Sort Array

↓

Choose Current Number

↓

Skip Equal Numbers
(at same recursion level)

↓

Continue
```

---

## Duplicate Visualization

Sorted array

```text
[1,1,2,5,6,7,10]
   ^
```

Suppose recursion already explored

```
first 1
```

Choosing the second **1** immediately afterward at the same level produces identical combinations.

Therefore

```java
if(i > start && nums[i] == nums[i-1])
    continue;
```

---

## Java Solution

```java
class Solution {

    List<List<Integer>> ans = new ArrayList<>();

    public List<List<Integer>> combinationSum2(int[] candidates, int target) {

        Arrays.sort(candidates);

        dfs(candidates, target, 0, new ArrayList<>());

        return ans;
    }

    private void dfs(int[] nums,
                     int target,
                     int start,
                     List<Integer> current){

        if(target == 0){
            ans.add(new ArrayList<>(current));
            return;
        }

        if(target < 0)
            return;

        for(int i = start; i < nums.length; i++){

            if(i > start && nums[i] == nums[i-1])
                continue;

            current.add(nums[i]);

            dfs(nums,
                target - nums[i],
                i + 1,
                current);

            current.remove(current.size()-1);
        }
    }
}
```

---

## Complexity

Time

```text
O(2^N)
```

Worst-case search.

Sorting

```text
O(N log N)
```

Space

```text
O(N)
```

---

## Common Mistakes

❌ Forgetting to sort.

❌ Using

```java
dfs(i,...)
```

instead of

```java
dfs(i+1,...)
```

❌ Skipping duplicates incorrectly.

Wrong

```java
if(nums[i]==nums[i-1])
```

Correct

```java
if(i>start && nums[i]==nums[i-1])
```

---

## Interview Insight

This problem teaches one of the most important interview pruning rules:

> Skip duplicates **only within the same recursion level**, not across different recursion paths.

---

# Problem 6 — LeetCode 17. Letter Combinations of a Phone Number

**Difficulty**

Medium

**Pattern**

Decision Tree Expansion

**Asked By**

Amazon • Meta • Google

**LLM-Proof**

No

Classic recursive generation.

---

## Problem Statement

Given digits from **2-9**, return every possible letter combination.

Example

```text
Input

"23"

Output

ad
ae
af
bd
be
bf
cd
ce
cf
```

---

## Visualization

```text
           ""
        /   |   \
       a    b    c
     / | \ /|\ / | \
    d e f d...
```

Every digit expands into multiple choices.

---

## Mapping

```text
2 → abc

3 → def

4 → ghi

5 → jkl

6 → mno

7 → pqrs

8 → tuv

9 → wxyz
```

---

## Approach

State consists of

- current index
- current string

At each digit

```
Try every possible letter

↓

Move to next digit

↓

Remove character
```

---

## Java Solution

```java
class Solution {

    private final String[] map = {
            "", "", "abc", "def", "ghi",
            "jkl", "mno", "pqrs", "tuv", "wxyz"
    };

    List<String> ans = new ArrayList<>();

    public List<String> letterCombinations(String digits) {

        if(digits.length()==0)
            return ans;

        dfs(0, digits, new StringBuilder());

        return ans;
    }

    private void dfs(int index,
                     String digits,
                     StringBuilder sb){

        if(index==digits.length()){

            ans.add(sb.toString());
            return;
        }

        String letters = map[digits.charAt(index)-'0'];

        for(char ch : letters.toCharArray()){

            sb.append(ch);

            dfs(index+1,digits,sb);

            sb.deleteCharAt(sb.length()-1);
        }
    }
}
```

---

## Complexity

If every digit has four letters

```text
O(4^N)
```

Space

```text
O(N)
```

---

## Common Mistakes

❌ Using immutable String concatenation repeatedly.

Prefer

```java
StringBuilder
```

---

## Interview Insight

Interviewers want to see whether you naturally identify

```
State

↓

Choices

↓

Backtrack
```

rather than trying iterative solutions.

---

# Problem 7 — LeetCode 131. Palindrome Partitioning

**Difficulty**

Medium

**Pattern**

Partition Building

**Asked By**

Apple • Google • Amazon

**LLM-Proof**

✅ Yes

Requires combining recursion with dynamic constraint checking.

---

## Problem Statement

Partition a string so that every substring is a palindrome.

Return every possible partition.

Example

```text
Input

"aab"

Output

[
["a","a","b"],
["aa","b"]
]
```

---

## Visualization

```text
aab

↓

a | ab

↓

a | a | b

Valid

----------------

aa | b

Valid

----------------

aab

Not Palindrome
```

---

## Key Observation

Unlike subsets,

choices have **variable length**.

From one index

```
Take

1 character

2 characters

3 characters

...
```

Only continue if chosen substring is a palindrome.

---

## Java Solution

```java
class Solution {

    List<List<String>> ans = new ArrayList<>();

    public List<List<String>> partition(String s) {

        dfs(0, s, new ArrayList<>());

        return ans;
    }

    private void dfs(int start,
                     String s,
                     List<String> current){

        if(start == s.length()){

            ans.add(new ArrayList<>(current));
            return;
        }

        for(int end=start; end<s.length(); end++){

            if(!isPalindrome(s,start,end))
                continue;

            current.add(s.substring(start,end+1));

            dfs(end+1,s,current);

            current.remove(current.size()-1);
        }
    }

    private boolean isPalindrome(String s,
                                 int l,
                                 int r){

        while(l<r){

            if(s.charAt(l)!=s.charAt(r))
                return false;

            l++;
            r--;
        }

        return true;
    }
}
```

---

## Complexity

Worst Case

```text
O(N × 2^N)
```

Space

```text
O(N)
```

---

## Common Mistakes

❌ Checking palindrome after recursion.

Instead

```
Check

↓

Choose

↓

Recurse
```

---

## Interview Insight

This is often considered the transition from beginner to intermediate backtracking.

It introduces **constraint validation before recursion**.

---

# Problem 8 — LeetCode 79. Word Search

**Difficulty**

Medium

**Pattern**

Grid DFS + Backtracking

**Asked By**

Google • Amazon • Meta

**LLM-Proof**

✅ Yes

Requires reasoning about grids, visited cells, and path restoration.

---

## Problem Statement

Given a board and a word, determine whether the word exists.

Letters must be adjacent.

Each cell may be used only once.

Example

```text
A B C E

S F C S

A D E E

Word

ABCCED
```

---

## Visualization

```text
A → B → C

        ↓

        C

        ↓

E ← D
```

Every recursive call explores

```
Up

Down

Left

Right
```

---

## Key Idea

Instead of maintaining a visited array,

temporarily mark the board.

```
A

↓

#

↓

Restore A
```

This saves memory.

---

## Java Solution

```java
class Solution {

    public boolean exist(char[][] board,
                         String word) {

        int m = board.length;
        int n = board[0].length;

        for(int i=0;i<m;i++){

            for(int j=0;j<n;j++){

                if(dfs(board,word,i,j,0))
                    return true;
            }
        }

        return false;
    }

    private boolean dfs(char[][] board,
                        String word,
                        int row,
                        int col,
                        int index){

        if(index==word.length())
            return true;

        if(row<0 || col<0
                || row>=board.length
                || col>=board[0].length
                || board[row][col]!=word.charAt(index))
            return false;

        char temp = board[row][col];

        board[row][col]='#';

        boolean found =
                dfs(board,word,row+1,col,index+1)
             || dfs(board,word,row-1,col,index+1)
             || dfs(board,word,row,col+1,index+1)
             || dfs(board,word,row,col-1,index+1);

        board[row][col]=temp;

        return found;
    }
}
```

---

## Complexity

Let

```text
M = rows

N = columns

L = word length
```

Time

```text
O(M × N × 4^L)
```

Space

```text
O(L)
```

---

## Common Mistakes

❌ Forgetting to restore

```java
board[row][col]=temp;
```

❌ Revisiting the same cell.

❌ Creating a separate visited matrix unnecessarily.

---

## Interview Insight

This is one of the highest-frequency grid backtracking questions across FAANG.

It introduces three important interview ideas:

- in-place marking
- grid recursion
- path restoration

Once comfortable with this problem, questions like **Number of Islands**, **Surrounded Regions**, and advanced board-search problems become significantly easier.

---

# End of Part 2

**Covered Problems**

5. Combination Sum II
6. Letter Combinations of a Phone Number
7. Palindrome Partitioning
8. Word Search

**Next (Part 3)**

- 51. N-Queens
- 37. Sudoku Solver
- 93. Restore IP Addresses
- 90. Subsets II
 
---

# Problem 9 — LeetCode 51. N-Queens

**Difficulty**

Hard

**Pattern**

Constraint Satisfaction + Board Backtracking

**Asked By**

Google • Apple • Netflix • Meta

**LLM-Proof**

✅ Highly

Requires maintaining multiple constraints simultaneously and pruning aggressively.

---

## Problem Statement

Place **N** queens on an **N × N** chessboard so that no two queens attack each other.

Return all valid board configurations.

Example

```text
Input

n = 4

Output

[
[".Q..",
 "...Q",
 "Q...",
 "..Q."],

["..Q.",
 "Q...",
 "...Q",
 ".Q.."]
]
```

---

## Why This Tests Backtracking

Each placement affects all future choices.

Every queen introduces three constraints:

- Same column
- Main diagonal
- Anti-diagonal

Unlike previous problems, the search space is reduced almost entirely through **pruning**.

---

## Visualization

```text
Row 0

. Q . .

↓

Row 1

X X X .

↓

Only remaining safe cells continue recursion.
```

---

## Constraint Representation

Instead of scanning the board repeatedly, maintain three sets:

```text
Columns

0 1 2 3

--------------

Main Diagonal

row - col

--------------

Anti Diagonal

row + col
```

Checking safety becomes

```text
O(1)
```

---

## Java Solution

```java
class Solution {

    List<List<String>> ans = new ArrayList<>();

    Set<Integer> columns = new HashSet<>();
    Set<Integer> diag1 = new HashSet<>();
    Set<Integer> diag2 = new HashSet<>();

    public List<List<String>> solveNQueens(int n) {

        char[][] board = new char[n][n];

        for(char[] row : board)
            Arrays.fill(row,'.');

        dfs(0, board);

        return ans;
    }

    private void dfs(int row,
                     char[][] board){

        if(row==board.length){

            List<String> temp = new ArrayList<>();

            for(char[] r : board)
                temp.add(new String(r));

            ans.add(temp);

            return;
        }

        for(int col=0; col<board.length; col++){

            if(columns.contains(col)
                    || diag1.contains(row-col)
                    || diag2.contains(row+col))
                continue;

            board[row][col]='Q';

            columns.add(col);
            diag1.add(row-col);
            diag2.add(row+col);

            dfs(row+1,board);

            board[row][col]='.';

            columns.remove(col);
            diag1.remove(row-col);
            diag2.remove(row+col);
        }
    }
}
```

---

## Complexity

Worst Case

```text
O(N!)
```

Space

```text
O(N)
```

---

## Common Mistakes

❌ Checking every row and column repeatedly.

❌ Forgetting to remove constraints after recursion.

❌ Using nested loops instead of recursive row-by-row placement.

---

## Interview Insight

N-Queens is considered one of the classic backtracking interview questions because it demonstrates:

- pruning
- state restoration
- constraint propagation

Interviewers often evaluate code quality more than simply obtaining the correct answer.

---

# Problem 10 — LeetCode 37. Sudoku Solver

**Difficulty**

Hard

**Pattern**

Constraint Satisfaction + Recursive Search

**Asked By**

Google • Apple • Netflix

**LLM-Proof**

✅ Extremely

Requires reasoning about multiple simultaneous constraints and efficient pruning.

---

## Problem Statement

Fill the Sudoku board so every row, column, and 3×3 box contains digits **1–9** exactly once.

---

## Visualization

```text
Find Empty Cell

↓

Try 1

↓

Valid?

↓

Yes

↓

Continue

↓

Else

Try 2

↓

...

↓

Backtrack
```

---

## Constraints

Every placement must satisfy

```text
Row

Column

3×3 Box
```

---

## Java Solution

```java
class Solution {

    public void solveSudoku(char[][] board) {

        solve(board);
    }

    private boolean solve(char[][] board){

        for(int row=0; row<9; row++){

            for(int col=0; col<9; col++){

                if(board[row][col]!='.')
                    continue;

                for(char num='1'; num<='9'; num++){

                    if(valid(board,row,col,num)){

                        board[row][col]=num;

                        if(solve(board))
                            return true;

                        board[row][col]='.';
                    }
                }

                return false;
            }
        }

        return true;
    }

    private boolean valid(char[][] board,
                          int row,
                          int col,
                          char c){

        for(int i=0;i<9;i++){

            if(board[row][i]==c)
                return false;

            if(board[i][col]==c)
                return false;

            int r = 3*(row/3)+i/3;
            int cc = 3*(col/3)+i%3;

            if(board[r][cc]==c)
                return false;
        }

        return true;
    }
}
```

---

## Complexity

Worst theoretical complexity

```text
O(9^(Empty Cells))
```

Pruning makes practical runtime much smaller.

Space

```text
O(Empty Cells)
```

---

## Common Mistakes

❌ Forgetting the 3×3 subgrid check.

❌ Continuing after a successful solution instead of returning immediately.

---

## Interview Insight

Sudoku demonstrates that backtracking is not brute force.

Good pruning transforms an otherwise impossible search into a practical solution.

---

# Problem 11 — LeetCode 93. Restore IP Addresses

**Difficulty**

Medium

**Pattern**

String Partitioning + Constraint Validation

**Asked By**

Amazon • Apple • Meta

**LLM-Proof**

Moderate

---

## Problem Statement

Given a string containing only digits, restore every possible valid IP address.

Example

```text
Input

25525511135

Output

255.255.11.135

255.255.111.35
```

---

## Decision Tree

```text
25525511135

↓

255

↓

255

↓

11

↓

135
```

Each segment can have

```text
1 digit

2 digits

3 digits
```

---

## Valid Segment Rules

A segment must satisfy

```text
0 <= value <=255

No leading zero

Length <=3
```

---

## Java Solution

```java
class Solution {

    List<String> ans = new ArrayList<>();

    public List<String> restoreIpAddresses(String s) {

        dfs(s,0,new ArrayList<>());

        return ans;
    }

    private void dfs(String s,
                     int index,
                     List<String> current){

        if(current.size()==4){

            if(index==s.length())
                ans.add(String.join(".",current));

            return;
        }

        for(int len=1; len<=3 && index+len<=s.length(); len++){

            String part = s.substring(index,index+len);

            if((part.startsWith("0") && part.length()>1)
                    || Integer.parseInt(part)>255)
                continue;

            current.add(part);

            dfs(s,index+len,current);

            current.remove(current.size()-1);
        }
    }
}
```

---

## Complexity

Time

```text
O(1)
```

Maximum search space is bounded because only four segments exist.

Space

```text
O(1)
```

Ignoring output storage.

---

## Common Mistakes

❌ Allowing

```text
001
```

❌ Allowing values greater than

```text
255
```

---

## Interview Insight

This is an example of backtracking over **strings** rather than arrays or boards.

---

# Problem 12 — LeetCode 90. Subsets II

**Difficulty**

Medium

**Pattern**

Subset Generation + Duplicate Elimination

**Asked By**

Meta • Amazon • Google

**LLM-Proof**

Moderate

---

## Problem Statement

Return every possible subset.

Input may contain duplicate numbers.

Duplicate subsets are not allowed.

---

## Example

```text
Input

[1,2,2]

Output

[]

[1]

[2]

[1,2]

[2,2]

[1,2,2]
```

---

## Visualization

Sorted

```text
1

↓

2

↓

2

Skip duplicate branch
```

---

## Key Idea

Exactly like Combination Sum II

Sort first.

Then skip duplicates at the same recursion depth.

---

## Java Solution

```java
class Solution {

    List<List<Integer>> ans = new ArrayList<>();

    public List<List<Integer>> subsetsWithDup(int[] nums) {

        Arrays.sort(nums);

        dfs(nums,0,new ArrayList<>());

        return ans;
    }

    private void dfs(int[] nums,
                     int start,
                     List<Integer> current){

        ans.add(new ArrayList<>(current));

        for(int i=start;i<nums.length;i++){

            if(i>start && nums[i]==nums[i-1])
                continue;

            current.add(nums[i]);

            dfs(nums,i+1,current);

            current.remove(current.size()-1);
        }
    }
}
```

---

## Complexity

Time

```text
O(2^N)
```

Space

```text
O(N)
```

---

## Common Mistakes

❌ Forgetting to sort.

❌ Using duplicate skipping globally instead of per recursion level.

Correct

```java
if(i>start && nums[i]==nums[i-1])
```

---

## Interview Insight

This problem reinforces one of the most reusable interview patterns:

```text
Sort

↓

Skip duplicates

↓

Backtrack
```

The same strategy appears in multiple FAANG questions involving combinations, subsets, and permutations.

---

# End of Part 3

**Covered Problems**

9. N-Queens
10. Sudoku Solver
11. Restore IP Addresses
12. Subsets II

**Next (Final Part)**

- 47. Permutations II
- 698. Partition to K Equal Sum Subsets
- 282. Expression Add Operators
- Final Interview Revision Checklist
- Pattern Summary

---

# Problem 13 — LeetCode 47. Permutations II

**Difficulty**

Medium

**Pattern**

Permutation Generation + Duplicate Elimination

**Asked By**

Google • Meta • Amazon

**LLM-Proof**

✅ Yes

Requires combining the classic permutation template with duplicate pruning.

---

## Problem Statement

Given a collection of numbers that may contain duplicates, return all **unique** permutations.

Example

```text
Input

[1,1,2]

Output

[
 [1,1,2],
 [1,2,1],
 [2,1,1]
]
```

---

## Why This Tests Backtracking

Unlike **Permutations**, duplicate values can generate identical permutations.

The challenge is not generating permutations—it is **avoiding duplicate branches**.

---

## Visualization

Sorted Input

```text
[1,1,2]

          []
       /   |   \
      1    1    2
      ↑
 Skip duplicate
```

---

## Duplicate Rule

After sorting,

skip duplicates only when

```java
nums[i] == nums[i - 1]
```

and

```java
!visited[i - 1]
```

This ensures identical numbers are always chosen in a consistent order.

---

## Java Solution

```java
class Solution {

    List<List<Integer>> ans = new ArrayList<>();

    public List<List<Integer>> permuteUnique(int[] nums) {

        Arrays.sort(nums);

        boolean[] visited = new boolean[nums.length];

        dfs(nums, visited, new ArrayList<>());

        return ans;
    }

    private void dfs(int[] nums,
                     boolean[] visited,
                     List<Integer> current){

        if(current.size()==nums.length){

            ans.add(new ArrayList<>(current));
            return;
        }

        for(int i=0;i<nums.length;i++){

            if(visited[i])
                continue;

            if(i>0
                    && nums[i]==nums[i-1]
                    && !visited[i-1])
                continue;

            visited[i]=true;

            current.add(nums[i]);

            dfs(nums,visited,current);

            current.remove(current.size()-1);

            visited[i]=false;
        }
    }
}
```

---

## Complexity

Worst Case

```text
O(N × N!)
```

Duplicate pruning usually reduces actual runtime significantly.

Space

```text
O(N)
```

---

## Common Mistakes

❌ Forgetting to sort.

❌ Using

```java
nums[i]==nums[i-1]
```

without checking

```java
visited[i-1]
```

---

## Interview Insight

Compare duplicate handling across problems:

| Problem | Duplicate Strategy |
|----------|-------------------|
|Combination Sum II|`i > start`|
|Subsets II|`i > start`|
|Permutations II|`!visited[i-1]`|

Knowing **why** these conditions differ is a common interview discussion.

---

# Problem 14 — LeetCode 698. Partition to K Equal Sum Subsets

**Difficulty**

Medium

**Pattern**

Bucket Filling + Constraint Satisfaction

**Asked By**

Google • Amazon • Netflix

**LLM-Proof**

✅ Highly

Requires designing the recursive state rather than recognizing a standard pattern.

---

## Problem Statement

Given an integer array and an integer **k**, determine whether it is possible to partition the array into **k** subsets having equal sums.

Example

```text
Input

nums = [4,3,2,3,5,2,1]

k = 4

Output

true
```

Target subset sum

```text
5
```

---

## Visualization

```text
Bucket 1

5

-----------

Bucket 2

4 + 1

-----------

Bucket 3

3 + 2

-----------

Bucket 4

3 + 2
```

---

## Key Observation

Instead of building subsets directly,

build one bucket at a time.

Whenever

```text
Current Sum == Target
```

start constructing the next bucket.

---

## Java Solution

```java
class Solution {

    public boolean canPartitionKSubsets(int[] nums, int k) {

        int sum = 0;

        for(int x : nums)
            sum += x;

        if(sum % k != 0)
            return false;

        Arrays.sort(nums);

        boolean[] visited = new boolean[nums.length];

        return dfs(nums,
                visited,
                nums.length - 1,
                k,
                0,
                sum / k);
    }

    private boolean dfs(int[] nums,
                        boolean[] visited,
                        int index,
                        int k,
                        int currentSum,
                        int target){

        if(k==1)
            return true;

        if(currentSum==target)
            return dfs(nums,
                    visited,
                    nums.length-1,
                    k-1,
                    0,
                    target);

        for(int i=index;i>=0;i--){

            if(visited[i])
                continue;

            if(currentSum + nums[i] > target)
                continue;

            visited[i]=true;

            if(dfs(nums,
                    visited,
                    i-1,
                    k,
                    currentSum+nums[i],
                    target))
                return true;

            visited[i]=false;
        }

        return false;
    }
}
```

---

## Complexity

Worst Case

```text
O(k × 2^N)
```

Pruning reduces runtime substantially in practice.

Space

```text
O(N)
```

---

## Common Mistakes

❌ Forgetting to check

```text
sum % k
```

❌ Not pruning when

```text
currentSum > target
```

❌ Building all buckets simultaneously.

---

## Interview Insight

This is considered one of the strongest medium-level backtracking problems because the recursive state is not immediately obvious.

---

# Problem 15 — LeetCode 282. Expression Add Operators

**Difficulty**

Hard

**Pattern**

Expression Construction + State Tracking

**Asked By**

Google • Meta • Apple

**LLM-Proof**

✅ Extremely

Requires maintaining multiple pieces of state simultaneously.

---

## Problem Statement

Given a string containing only digits and a target value, insert

```text
+

-

*
```

between digits so the expression evaluates to the target.

Return every valid expression.

Example

```text
Input

num = "123"

target = 6

Output

1+2+3

1*2*3
```

---

## Why This Is Hard

The recursion state contains

- current position
- current expression
- evaluated value
- previous operand

Tracking multiplication precedence is the primary challenge.

---

## Visualization

```text
123

↓

1

↓

+

↓

2

↓

*

↓

3
```

Every recursive call decides

```text
Take

1 digit

2 digits

3 digits
```

and

```text
Choose Operator
```

---

## Java Solution

```java
class Solution {

    List<String> ans = new ArrayList<>();

    public List<String> addOperators(String num, int target) {

        dfs(num,
            target,
            0,
            0,
            0,
            new StringBuilder());

        return ans;
    }

    private void dfs(String num,
                     int target,
                     int index,
                     long value,
                     long previous,
                     StringBuilder path){

        if(index==num.length()){

            if(value==target)
                ans.add(path.toString());

            return;
        }

        for(int i=index;i<num.length();i++){

            if(i!=index && num.charAt(index)=='0')
                break;

            long current =
                    Long.parseLong(num.substring(index,i+1));

            int length = path.length();

            if(index==0){

                path.append(current);

                dfs(num,
                        target,
                        i+1,
                        current,
                        current,
                        path);

                path.setLength(length);

            }else{

                path.append("+").append(current);

                dfs(num,
                        target,
                        i+1,
                        value+current,
                        current,
                        path);

                path.setLength(length);

                path.append("-").append(current);

                dfs(num,
                        target,
                        i+1,
                        value-current,
                        -current,
                        path);

                path.setLength(length);

                path.append("*").append(current);

                dfs(num,
                        target,
                        i+1,
                        value-previous+previous*current,
                        previous*current,
                        path);

                path.setLength(length);
            }
        }
    }
}
```

---

## Complexity

Worst Case

```text
O(4^N)
```

Space

```text
O(N)
```

---

## Common Mistakes

❌ Ignoring multiplication precedence.

❌ Allowing numbers like

```text
05
```

❌ Using `int` instead of `long`.

---

## Interview Insight

This problem evaluates whether you can carry multiple pieces of recursive state correctly.

It is among the most challenging backtracking questions asked in Google interviews.

---

# Interview Revision Checklist

Before an interview, ensure you can identify the following within 30 seconds for any backtracking problem:

- State variables
- Available choices
- Base case
- Constraint checks
- Undo step
- Pruning opportunities
- Duplicate handling strategy
- Time and space complexity

---

# Pattern Summary

| Pattern | Representative Problems |
|----------|-------------------------|
|Permutation Generation|46, 47|
|Subset Generation|78, 90|
|Combination Generation|39, 40, 77|
|Constraint Satisfaction|51, 37, 698|
|Grid Backtracking|79|
|String Partitioning|131, 93|
|Expression Generation|282|
|Decision Tree Expansion|17|

---

# FAANG Pattern Frequency

| Company | Frequently Asked Backtracking Topics |
|----------|--------------------------------------|
|Google|Sudoku, N-Queens, Word Search, Expression Add Operators, Partition to K Equal Sum Subsets|
|Amazon|Combination Sum, Word Search, Restore IP Addresses, Letter Combinations|
|Meta|Permutations, Subsets, Combination Sum, Expression Add Operators|
|Apple|Palindrome Partitioning, Sudoku, N-Queens|
|Netflix|Constraint Satisfaction, N-Queens, Partition to K Equal Sum Subsets|

---

# Quick Recognition Guide

| If You See... | Think... |
|---------------|----------|
|Generate every arrangement|Permutations|
|Generate every selection|Subsets / Combinations|
|Find all valid partitions|Partition Backtracking|
|Fill a board under constraints|Constraint Satisfaction|
|Explore a grid path|Grid DFS + Backtracking|
|Insert operators or characters|Expression Construction|
|Duplicate values|Sort + Pruning|
|Target value decreases|Combination Sum|

---

# Final Notes

These 15 problems collectively cover nearly every major backtracking pattern encountered in FAANG interviews. Rather than memorizing solutions, focus on identifying:

1. **State** — What information defines the current recursive call?
2. **Choices** — What decisions are available at this step?
3. **Constraints** — What makes a choice invalid?
4. **Base Case** — When is a complete solution found?
5. **Undo Step** — How is the previous state restored?

Mastering these principles allows you to derive solutions for unfamiliar backtracking problems during interviews instead of relying on memorized templates.

---
**End of Guide**



