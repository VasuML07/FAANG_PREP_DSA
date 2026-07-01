# Stack Interview Preparation Guide (Java)

> A complete Stack-focused interview preparation guide for FAANG-level coding interviews using LeetCode.

---

# Table of Contents

1. Introduction
2. Easy Problems
   - 20. Valid Parentheses
   - 225. Implement Stack using Queues
   - 232. Implement Queue using Stacks
3. Medium Problems
   - 155. Min Stack
   - 150. Evaluate Reverse Polish Notation
   - 71. Simplify Path
   - 394. Decode String
   - 739. Daily Temperatures
   - 901. Online Stock Span
4. Hard Problems
   - 84. Largest Rectangle in Histogram
   - 85. Maximal Rectangle
   - 42. Trapping Rain Water
   - 32. Longest Valid Parentheses
   - 224. Basic Calculator
   - 726. Number of Atoms
5. Final Revision Notes
6. Interview Cheat Sheet

---

# Why Stack Matters in FAANG Interviews

Stack is one of the highest-frequency interview topics because it naturally solves problems involving:

- Nested structures
- Balanced symbols
- Expression evaluation
- Previous/Next Greater Element
- Monotonic data structures
- Parsing
- DFS simulation
- String decoding
- Histogram problems

Many difficult interview problems become linear-time solutions after recognizing that a stack stores unresolved information.

---

# Problem 1 — LeetCode 20. Valid Parentheses

**Difficulty:** Easy

**Companies**

Google • Amazon • Microsoft • Meta • Apple • Bloomberg • Adobe • Oracle • Uber • Salesforce

---

## Problem

Given a string containing only

```
()
{}
[]
```

Determine whether every opening bracket has

- a matching closing bracket
- correct order
- proper nesting

Example

```
Input

()[]{}

Output

true
```

Example

```
Input

([)]

Output

false
```

---

# Interview Pattern

This is the first problem almost everyone solves using a stack.

The stack always stores **opening brackets waiting to be matched.**

Whenever we encounter a closing bracket, it must match the latest opening bracket.

That "latest unmatched opening" property is exactly **LIFO**.

---

# Visualization

Input

```
({[]})
```

Process

```
(

Stack

(

-------------------

{

Stack

(
{

-------------------

[

Stack

(
{
[

-------------------

]

Pop [

Stack

(
{

-------------------

}

Pop {

Stack

(

-------------------

)

Pop (

Stack

Empty
```

Valid.

---

# Brute Force Idea

Repeatedly remove

```
()
{}
[]
```

until no change occurs.

If string becomes empty

Valid.

Otherwise

Invalid.

### Complexity

Time

```
O(N²)
```

Space

```
O(1)
```

Not suitable for interviews.

---

# Optimal Solution

Maintain a stack.

Rules

If character is

```
(
[
{
```

Push.

Otherwise

Check

- stack empty?
- top matches?

If not

Return false.

At the end

Stack must be empty.

---

# Why It Works

Every opening bracket waits for its partner.

The nearest unmatched opening must close first.

Exactly what a stack provides.

---

# Complexity

Time

```
O(N)
```

Space

```
O(N)
```

Worst case

```
(((((((((
```

---

# Java Solution

```java
import java.util.*;

class Solution {
    public boolean isValid(String s) {

        Stack<Character> stack = new Stack<>();

        for(char ch : s.toCharArray()){

            if(ch=='(' || ch=='[' || ch=='{'){
                stack.push(ch);
            }else{

                if(stack.isEmpty())
                    return false;

                char top = stack.pop();

                if(ch==')' && top!='(')
                    return false;

                if(ch==']' && top!='[')
                    return false;

                if(ch=='}' && top!='{')
                    return false;
            }
        }

        return stack.isEmpty();
    }
}
```

---

# Follow-up

Can you solve using

```
ArrayDeque
```

instead of

```
Stack
```

Expected answer

Yes.

```
ArrayDeque
```

is preferred because

- faster
- non-synchronized
- recommended by Java documentation

---

# Interview Variations

- Ignore non-bracket characters
- Multiple bracket types
- Return index of first mismatch
- Generate minimum removals

---

# Recognition Clue

If the problem says

- balanced
- nested
- matching
- latest opening

Think

**Stack immediately.**

---

---

# Problem 2 — LeetCode 225. Implement Stack using Queues

**Difficulty:** Easy

**Companies**

Amazon • Microsoft • Google • Apple • Meta

---

# Problem

Implement

```
push()
pop()
top()
empty()
```

using only queues.

---

# Interview Goal

Tests understanding of

- queue
- stack
- simulation
- data structure transformation

---

# Approach 1

Two queues.

Push

```
O(1)
```

Pop

```
O(N)
```

Easy but not optimal.

---

# Better Approach

Use one queue.

Whenever pushing

Rotate all previous elements behind the new element.

Example

Push

```
1
```

Queue

```
1
```

Push

```
2
```

Queue

```
2 1
```

Push

```
3
```

Queue

```
3 2 1
```

Now

Front becomes stack top.

---

# Complexity

Push

```
O(N)
```

Pop

```
O(1)
```

Top

```
O(1)
```

---

# Java

```java
import java.util.*;

class MyStack {

    Queue<Integer> q;

    public MyStack() {
        q = new LinkedList<>();
    }

    public void push(int x) {

        q.offer(x);

        int size = q.size();

        while(size > 1){
            q.offer(q.poll());
            size--;
        }
    }

    public int pop() {
        return q.poll();
    }

    public int top() {
        return q.peek();
    }

    public boolean empty() {
        return q.isEmpty();
    }
}
```

---

# Follow-up

Can push be

```
O(1)
```

and pop

```
O(N)
```

Yes.

Using another queue.

Interviewers often ask both implementations.

---

# Recognition

Simulation question.

Know both

- two queue solution
- one queue rotation

---

---

# Problem 3 — LeetCode 232. Implement Queue using Stacks

**Difficulty:** Easy

**Companies**

Google • Microsoft • Amazon • Meta • Apple • LinkedIn

---

# Problem

Implement queue using stacks.

Operations

```
push
pop
peek
empty
```

---

# Observation

Queue

FIFO

Stack

LIFO

Need two reversals.

---

# Key Trick

Maintain

```
input stack
output stack
```

Push

Always goes into input.

Pop

If output empty

Move everything

```
input

↓

output
```

Now oldest element reaches top.

---

# Visualization

Push

```
1
2
3
```

Input

```
3
2
1
```

Output

```
Empty
```

Need pop

Transfer

Output

```
1
2
3
```

Pop

```
1
```

Correct FIFO.

---

# Why This Works

Moving all elements reverses order.

Second stack restores FIFO.

Each element moves at most once.

---

# Complexity

Amortized

Push

```
O(1)
```

Pop

```
O(1)
```

Peek

```
O(1)
```

Worst transfer

```
O(N)
```

Amortized still

```
O(1)
```

This amortized analysis is frequently asked by interviewers.

---

# Java

```java
import java.util.*;

class MyQueue {

    Stack<Integer> in;
    Stack<Integer> out;

    public MyQueue() {
        in = new Stack<>();
        out = new Stack<>();
    }

    public void push(int x) {
        in.push(x);
    }

    private void transfer(){

        while(!in.isEmpty()){
            out.push(in.pop());
        }
    }

    public int pop() {

        if(out.isEmpty())
            transfer();

        return out.pop();
    }

    public int peek() {

        if(out.isEmpty())
            transfer();

        return out.peek();
    }

    public boolean empty() {
        return in.isEmpty() && out.isEmpty();
    }
}
```

---

# Common Interview Questions

### Why is this amortized O(1)?

Each element

- pushed once into input
- moved once to output
- popped once

Total work per element is constant over many operations.

---

### Can transfer happen every operation?

No.

Transfer happens only when output becomes empty.

---

### Why not transfer during push?

Doing so makes every push expensive.

The lazy transfer approach minimizes total work.

---

# Pattern Learned

This problem introduces an important interview concept:

> **Delayed processing (lazy evaluation)**

Instead of doing work immediately, postpone it until absolutely necessary.

The same idea appears later in:

- Min Stack
- Monotonic Stack
- Basic Calculator
- Histogram problems

---

**End of Part 1**

**Covered Problems**
- 20. Valid Parentheses
- 225. Implement Stack using Queues
- 232. Implement Queue using Stacks

**Next (Part 2):**
- 155. Min Stack
- 150. Evaluate Reverse Polish Notation
- 71. Simplify Path
 
---

# Problem 4 — LeetCode 155. Min Stack

**Difficulty:** Medium

**Companies**

Google • Amazon • Microsoft • Meta • Apple • Bloomberg • Adobe • Oracle • Uber

---

## Problem

Design a stack that supports the following operations in constant time:

- `push(x)`
- `pop()`
- `top()`
- `getMin()`

Example

```
Input

push(-2)
push(0)
push(-3)

getMin()

Output

-3

pop()

top()

0

getMin()

-2
```

---

# Why Interviewers Ask This

Many candidates immediately think:

> "Every time `getMin()` is called, scan the entire stack."

That works but completely misses the optimization opportunity.

This problem tests whether you can **augment a data structure** by storing additional information.

---

# Approach 1 — Linear Scan

Store everything in one stack.

Whenever `getMin()` is called:

```
Traverse entire stack
Find minimum
Return it
```

### Complexity

| Operation | Time |
|-----------|------|
| push | O(1) |
| pop | O(1) |
| top | O(1) |
| getMin | O(N) |

Fails interview expectations.

---

# Optimal Approach — Two Stacks

Maintain

```
Main Stack
```

Stores all elements.

```
Minimum Stack
```

Stores the minimum value seen so far.

Whenever

```
x <= current minimum
```

push it into both stacks.

Whenever popping

If popped value equals minimum stack top

Remove it from both stacks.

---

# Visualization

Push

```
5
```

```
Main : 5

Min : 5
```

---

Push

```
2
```

```
Main

5
2

Min

5
2
```

---

Push

```
7
```

```
Main

5
2
7

Min

5
2
```

Minimum remains

```
2
```

---

Push

```
1
```

```
Main

5
2
7
1

Min

5
2
1
```

Current minimum

```
1
```

---

Pop

```
1
```

Both stacks remove

```
1
```

Minimum automatically becomes

```
2
```

---

# Why It Works

The second stack stores every point where the minimum changes.

Whenever the current minimum disappears, the previous minimum is already waiting beneath it.

No searching required.

---

# Complexity

| Operation | Time | Space |
|-----------|------|-------|
| push | O(1) | O(N) |
| pop | O(1) | O(N) |
| top | O(1) | O(N) |
| getMin | O(1) | O(N) |

---

# Java Solution

```java
import java.util.Stack;

class MinStack {

    private Stack<Integer> stack;
    private Stack<Integer> minStack;

    public MinStack() {
        stack = new Stack<>();
        minStack = new Stack<>();
    }

    public void push(int val) {

        stack.push(val);

        if(minStack.isEmpty() || val <= minStack.peek())
            minStack.push(val);
    }

    public void pop() {

        if(stack.pop().equals(minStack.peek()))
            minStack.pop();
    }

    public int top() {
        return stack.peek();
    }

    public int getMin() {
        return minStack.peek();
    }
}
```

---

# Alternative Solution

Instead of storing only minimum values,

store

```
(value, currentMinimum)
```

inside every node.

Example

```
(5,5)

(2,2)

(7,2)

(1,1)
```

Each element remembers the minimum at its insertion time.

This removes the need for a second stack.

---

# Follow-Up Questions

- Can you reduce auxiliary space?
- Can this be implemented using a linked list?
- What happens with duplicate minimum values?

Notice the condition

```java
val <= minStack.peek()
```

not

```java
val < minStack.peek()
```

Otherwise duplicate minimums break the solution.

---

# Pattern Learned

Whenever a question asks

```
Current Maximum
Current Minimum
Running Best
```

consider storing **extra state alongside each push**.

---

---

# Problem 5 — LeetCode 150. Evaluate Reverse Polish Notation

**Difficulty:** Medium

**Companies**

Google • Amazon • Microsoft • Meta • Apple • Bloomberg • VMware • Cisco

---

## Problem

Evaluate an arithmetic expression written in Reverse Polish Notation.

Operators

```
+
-
*
/
```

Example

```
Input

["2","1","+","3","*"]

Output

9
```

Explanation

```
(2+1)*3
```

---

# What is Reverse Polish Notation?

Instead of

```
2 + 3
```

we write

```
2 3 +
```

Operators always come **after** operands.

No parentheses are needed.

---

# Key Observation

Whenever we encounter

```
Number
```

store it.

Whenever we encounter

```
Operator
```

use the last two numbers.

That is exactly LIFO.

---

# Visualization

Expression

```
2 1 + 3 *
```

Process

```
2

Stack

2

---------------

1

Stack

2 1

---------------

+

Pop

1
2

Compute

2+1=3

Push

3

Stack

3

---------------

3

Stack

3 3

---------------

*

Pop

3
3

9

Stack

9
```

Answer

```
9
```

---

# Algorithm

For every token

If number

```
Push
```

Else

```
Pop second operand

Pop first operand

Compute

Push result
```

Order matters.

For subtraction

```
a-b
```

must be

```java
int b = stack.pop();
int a = stack.pop();
```

not the reverse.

---

# Complexity

Time

```
O(N)
```

Space

```
O(N)
```

---

# Java Solution

```java
import java.util.*;

class Solution {

    public int evalRPN(String[] tokens) {

        Stack<Integer> stack = new Stack<>();

        for(String token : tokens){

            switch(token){

                case "+":
                    stack.push(stack.pop() + stack.pop());
                    break;

                case "-":{
                    int b = stack.pop();
                    int a = stack.pop();
                    stack.push(a - b);
                    break;
                }

                case "*":
                    stack.push(stack.pop() * stack.pop());
                    break;

                case "/":{
                    int b = stack.pop();
                    int a = stack.pop();
                    stack.push(a / b);
                    break;
                }

                default:
                    stack.push(Integer.parseInt(token));
            }
        }

        return stack.pop();
    }
}
```

---

# Common Mistake

Many candidates write

```java
stack.pop() - stack.pop()
```

which changes operand order.

Always remember

```
Second popped value

operator

First popped value

×

Incorrect

First popped value

operator

Second popped value

✓ Correct
```

---

# Follow-Up

Convert

```
Infix

↓

Postfix

↓

Evaluate
```

Interviewers occasionally extend the question in this direction.

---

# Pattern Learned

Expression evaluation almost always maps to a stack.

---

---

# Problem 6 — LeetCode 71. Simplify Path

**Difficulty:** Medium

**Companies**

Google • Amazon • Microsoft • Meta • Apple • Dropbox • Bloomberg

---

## Problem

Given a Unix-style absolute path,

return its canonical path.

Rules

```
"."  -> current directory

".." -> parent directory

"//" -> single slash
```

Example

```
Input

/home//foo/

Output

/home/foo
```

---

# Observation

Each directory behaves like a stack.

Entering

```
home
```

means

```
Push(home)
```

Going back

```
..
```

means

```
Pop()
```

---

# Visualization

```
/a/./b/../../c/
```

Split

```
a

.

b

..

..

c
```

Processing

```
Stack

[]

Push a

[a]

Ignore .

[a]

Push b

[a,b]

..

[a]

..

[]

Push c

[c]
```

Final path

```
/c
```

---

# Algorithm

Split using

```
/
```

For each token

Ignore

```
""

"."
```

If

```
..
```

Pop if possible.

Otherwise

Push directory name.

Finally rebuild the path.

---

# Complexity

Time

```
O(N)
```

Space

```
O(N)
```

---

# Java Solution

```java
import java.util.*;

class Solution {

    public String simplifyPath(String path) {

        Deque<String> stack = new ArrayDeque<>();

        String[] parts = path.split("/");

        for(String part : parts){

            if(part.equals("") || part.equals("."))
                continue;

            if(part.equals("..")){

                if(!stack.isEmpty())
                    stack.removeLast();

            }else{

                stack.addLast(part);
            }
        }

        if(stack.isEmpty())
            return "/";

        StringBuilder sb = new StringBuilder();

        for(String dir : stack){
            sb.append("/").append(dir);
        }

        return sb.toString();
    }
}
```

---

# Why `Deque` Instead of `Stack`?

Modern Java recommends

```java
ArrayDeque
```

because it is

- faster
- non-synchronized
- lower memory overhead

Many interviewers expect this answer.

---

# Follow-Up Questions

- Handle relative paths.
- Support symbolic links.
- Normalize Windows paths.
- Prevent escaping beyond the root directory.

---

# Pattern Learned

Whenever the problem describes

- entering
- leaving
- undo
- rollback
- parent-child navigation

a stack is usually the appropriate data structure.

---

## End of Part 2

### Problems Covered

- **155. Min Stack**
- **150. Evaluate Reverse Polish Notation**
- **71. Simplify Path**

### Next (Part 3)

- **394. Decode String**
- **739. Daily Temperatures**
- **901. Online Stock Span**

---

# Problem 7 — LeetCode 394. Decode String

**Difficulty:** Medium

**Companies**

Google • Amazon • Meta • Microsoft • Apple • Bloomberg • Adobe • Uber

---

## Problem

Given an encoded string, decode it.

Encoding rule:

```
k[encoded_string]
```

means

```
encoded_string repeated k times
```

Examples

```
Input

3[a]2[bc]

Output

aaabcbc
```

```
Input

3[a2[c]]

Output

accaccacc
```

---

# Why This Problem Appears in Interviews

This problem combines multiple ideas:

- Nested structures
- String parsing
- Stack simulation
- Expression decoding

It tests whether you can correctly manage state while parsing characters from left to right.

---

# Key Observation

Whenever we encounter a

```
[
```

we start a completely new nested string.

Before entering it, we must remember:

- Current decoded string
- Current repetition count

A stack is ideal for storing this suspended state.

---

# Visualization

Input

```
3[a2[c]]
```

Processing

```
Read 3

count = 3

----------------

Read [

Push

("",3)

current = ""

----------------

Read a

current = "a"

----------------

Read 2

count = 2

----------------

Read [

Push

("a",2)

current=""

----------------

Read c

current="c"

----------------

Read ]

Pop

("a",2)

Repeat

cc

Current

acc

----------------

Read ]

Pop

("",3)

Repeat

accaccacc
```

---

# Brute Force Idea

Repeatedly locate the innermost brackets and expand them.

This requires repeated scanning and string rebuilding.

### Complexity

Worst case

```
O(N²)
```

Not interview friendly.

---

# Optimal Stack Solution

Maintain

```
Stack<Integer> counts
Stack<StringBuilder> strings
```

Rules

Digit

```
Build current number
```

Letter

```
Append to current string
```

Opening bracket

```
Push current string

Push count

Reset both
```

Closing bracket

```
Pop previous string

Repeat current string

Append
```

---

# Complexity

Time

```
O(N)
```

Space

```
O(N)
```

---

# Java Solution

```java
import java.util.*;

class Solution {

    public String decodeString(String s) {

        Stack<Integer> countStack = new Stack<>();
        Stack<StringBuilder> stringStack = new Stack<>();

        StringBuilder current = new StringBuilder();

        int number = 0;

        for(char ch : s.toCharArray()){

            if(Character.isDigit(ch)){

                number = number * 10 + (ch - '0');

            }else if(ch == '['){

                countStack.push(number);
                stringStack.push(current);

                number = 0;
                current = new StringBuilder();

            }else if(ch == ']'){

                int repeat = countStack.pop();

                StringBuilder previous = stringStack.pop();

                while(repeat-- > 0)
                    previous.append(current);

                current = previous;

            }else{

                current.append(ch);
            }
        }

        return current.toString();
    }
}
```

---

# Common Mistakes

### Forgetting multi-digit numbers

Wrong

```
12[a]
```

becomes

```
1 then 2
```

Correct

```java
number = number * 10 + digit;
```

---

### Forgetting nested strings

Current string must be pushed before resetting.

---

# Follow-Up Questions

- Decode recursively.
- Support additional operators.
- Encode a string instead of decoding.

---

# Pattern Learned

Whenever parsing nested expressions,

think

```
Save state

↓

Process nested block

↓

Restore state
```

This is one of the strongest indicators for a stack.

---

---

# Problem 8 — LeetCode 739. Daily Temperatures

**Difficulty:** Medium

**Companies**

Amazon • Google • Meta • Microsoft • Apple • Bloomberg • ByteDance • Uber

---

## Problem

For every day,

return how many days must pass until a warmer temperature.

If none exists,

return

```
0
```

Example

```
Input

[73,74,75,71,69,72,76,73]

Output

[1,1,4,2,1,1,0,0]
```

---

# Why This Problem Matters

This is the classic introduction to

> **Monotonic Stack**

The technique appears repeatedly in FAANG interviews.

Later problems such as

- Largest Rectangle
- Stock Span
- Next Greater Element
- Trapping Rain Water

all rely on the same idea.

---

# Brute Force

For every index,

scan right until a warmer day is found.

### Complexity

Time

```
O(N²)
```

Space

```
O(1)
```

---

# Key Observation

Instead of asking

> "When is the next warmer day?"

ask

> "Which previous days are still waiting for a warmer day?"

Those unresolved indices belong inside a stack.

---

# Monotonic Stack

Maintain a stack of indices whose temperatures are in

```
decreasing order
```

Whenever a warmer temperature appears,

resolve every colder day.

---

# Visualization

```
73 74 75 71 69 72
```

Stack stores indices

```
73

[0]

------------

74

74 > 73

Answer[0]=1

Stack

[1]

------------

75

75 >74

Answer[1]=1

Stack

[2]

------------

71

Stack

[2,3]

------------

69

Stack

[2,3,4]

------------

72

72>69

Answer[4]=1

72>71

Answer[3]=2

Stack

[2,5]
```

---

# Why It Works

Every index enters the stack once.

Every index leaves once.

No repeated searching.

---

# Complexity

Time

```
O(N)
```

Space

```
O(N)
```

---

# Java Solution

```java
import java.util.*;

class Solution {

    public int[] dailyTemperatures(int[] temperatures) {

        int n = temperatures.length;

        int[] answer = new int[n];

        Stack<Integer> stack = new Stack<>();

        for(int i = 0; i < n; i++){

            while(!stack.isEmpty() &&
                    temperatures[i] > temperatures[stack.peek()]){

                int index = stack.pop();

                answer[index] = i - index;
            }

            stack.push(i);
        }

        return answer;
    }
}
```

---

# Interview Insight

Notice the stack stores

```
indices
```

not temperatures.

Why?

Because we need

```
distance

i-index
```

---

# Follow-Up

Interviewers may immediately ask

- Next Greater Element
- Previous Greater Element
- Next Smaller Element

All are solved using the same monotonic stack template.

---

# Pattern Learned

Whenever the problem asks

```
Nearest Greater

Nearest Smaller

Next Greater

Previous Smaller
```

immediately consider a monotonic stack.

---

---

# Problem 9 — LeetCode 901. Online Stock Span

**Difficulty:** Medium

**Companies**

Amazon • Google • Microsoft • Meta • Bloomberg • Apple

---

## Problem

Design a data structure that returns

the stock span of today's price.

Stock span =

Number of consecutive previous days

whose prices are

```
<= today's price
```

Example

```
Input

100

80

60

70

60

75

85

Output

1

1

1

2

1

4

6
```

---

# Key Observation

Smaller prices become useless once a larger price arrives.

Instead of storing every previous value,

discard values that can never affect future answers.

---

# Monotonic Stack

Maintain a stack in

```
strictly decreasing order
```

Each element stores

```
(price, span)
```

When a higher price arrives,

merge spans.

---

# Visualization

```
100

Stack

(100,1)

----------------

80

(100,1)

(80,1)

----------------

60

(100,1)

(80,1)

(60,1)

----------------

70

Pop

60

span=2

Stack

100

80

70
```

---

# Algorithm

Current span starts at

```
1
```

While stack top

```
<= current price
```

Pop

Add its span

Push merged span.

---

# Complexity

Time

```
O(1) amortized
```

Worst single call

```
O(N)
```

Space

```
O(N)
```

---

# Java Solution

```java
import java.util.*;

class StockSpanner {

    private Stack<int[]> stack;

    public StockSpanner() {
        stack = new Stack<>();
    }

    public int next(int price) {

        int span = 1;

        while(!stack.isEmpty() &&
                stack.peek()[0] <= price){

            span += stack.pop()[1];
        }

        stack.push(new int[]{price, span});

        return span;
    }
}
```

---

# Why Store Span?

Without storing spans,

every query could revisit the same elements repeatedly.

By merging spans,

each element contributes exactly once.

---

# Common Mistake

Many candidates store only prices.

Then,

after popping,

they lose information about how many days those prices already represented.

Always store

```
(price, span)
```

---

# Follow-Up Questions

- What if prices arrive in descending order?
- Can this be implemented using arrays?
- Return maximum span so far.
- Support deleting the latest price.

---

# Pattern Learned

Daily Temperatures and Stock Span are the two foundational monotonic stack problems.

Mastering them makes it significantly easier to solve

- Largest Rectangle in Histogram
- Maximal Rectangle
- Trapping Rain Water
- Next Greater Element series

---

## End of Part 3

### Problems Covered

- **394. Decode String**
- **739. Daily Temperatures**
- **901. Online Stock Span**

### Next (Part 4)

- **84. Largest Rectangle in Histogram**
- **85. Maximal Rectangle**
- **42. Trapping Rain Water**

---

# Problem 10 — LeetCode 84. Largest Rectangle in Histogram

**Difficulty:** Hard

**Companies**

Google • Amazon • Microsoft • Meta • Apple • Bloomberg • Uber • Adobe • LinkedIn

---

## Problem

Given an array representing bar heights of a histogram,

find the area of the largest rectangle.

Example

```
Input

[2,1,5,6,2,3]

Output

10
```

Explanation

```
Height = 5

Width = 2

Area = 10
```

---

# Why This Problem Matters

This is one of the most important monotonic stack problems.

It introduces a reusable interview technique:

> Find the Previous Smaller Element (PSE) and Next Smaller Element (NSE).

This exact technique appears in several FAANG interview questions.

---

# Brute Force

Choose every bar as the rectangle height.

Expand left while bars are taller.

Expand right while bars are taller.

Compute area.

### Complexity

```
Time : O(N²)

Space : O(1)
```

---

# Better Observation

Instead of expanding from every bar,

find

- first smaller element on the left
- first smaller element on the right

Those two boundaries determine the maximum width.

---

# Visualization

```
Histogram

2 1 5 6 2 3

Index

0 1 2 3 4 5
```

For height

```
5
```

Previous smaller

```
1
```

Next smaller

```
2
```

Width

```
4 - 1 - 1 = 2
```

Area

```
5 × 2 = 10
```

---

# Monotonic Increasing Stack

Maintain indices in increasing order.

Whenever a smaller height arrives,

all taller bars have found their

```
Next Smaller Element
```

Pop them immediately and compute their area.

---

# Stack Visualization

```
2

Stack

[0]

--------------

1

1 < 2

Pop

Area = 2

Stack

[]

Push 1

--------------

5

Stack

1 5

--------------

6

Stack

1 5 6

--------------

2

2 < 6

Pop

Area

6 × 1

2 < 5

Pop

Area

5 × 2
```

---

# Complexity

```
Time : O(N)

Space : O(N)
```

Each index enters and leaves the stack once.

---

# Java Solution

```java
import java.util.*;

class Solution {

    public int largestRectangleArea(int[] heights) {

        Stack<Integer> stack = new Stack<>();

        int maxArea = 0;

        for(int i = 0; i <= heights.length; i++){

            int currentHeight = (i == heights.length) ? 0 : heights[i];

            while(!stack.isEmpty() &&
                  currentHeight < heights[stack.peek()]){

                int height = heights[stack.pop()];

                int leftBoundary = stack.isEmpty() ? -1 : stack.peek();

                int width = i - leftBoundary - 1;

                maxArea = Math.max(maxArea, height * width);
            }

            stack.push(i);
        }

        return maxArea;
    }
}
```

---

# Why Add an Extra Zero?

Notice

```java
for(i <= n)
```

instead of

```java
i < n
```

The artificial

```
0
```

forces every remaining bar to be processed.

Without it,

bars remaining in the stack would never compute their rectangle.

---

# Common Mistakes

### Forgetting Width Formula

Correct

```
width = right - left - 1
```

---

### Using Heights Instead of Indices

The stack must store

```
indices
```

because width depends on positions.

---

# Follow-Up

Interviewers often ask

- Return rectangle coordinates.
- Largest square.
- Circular histogram.

---

# Pattern Learned

Whenever width depends on the nearest smaller element,

a monotonic stack is often the optimal solution.

---

---

# Problem 11 — LeetCode 85. Maximal Rectangle

**Difficulty:** Hard

**Companies**

Google • Amazon • Microsoft • Meta • Apple • Bloomberg

---

## Problem

Given a binary matrix,

find the area of the largest rectangle containing only

```
1
```

Example

```
Input

1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0

Output

6
```

---

# Key Insight

This problem is **not** a completely new algorithm.

Instead,

every row becomes a histogram.

Then solve

```
Largest Rectangle in Histogram
```

for each row.

---

# Histogram Transformation

Original

```
1 0 1 0

1 0 1 1

1 1 1 1
```

Running heights

```
1 0 1 0

2 0 2 1

3 1 3 2
```

Each row is now a histogram.

---

# Algorithm

For every row

```
Update histogram heights

↓

Run Largest Rectangle

↓

Update answer
```

---

# Visualization

```
Row 1

1 0 1

↓

Histogram

1 0 1

Area

1

----------------

Row 2

1 0 1

↓

Histogram

2 0 2

Area

2

----------------

Row 3

1 1 1

↓

Histogram

3 1 3

Area

3
```

---

# Complexity

Suppose

```
Rows = M

Columns = N
```

Time

```
O(M × N)
```

Space

```
O(N)
```

---

# Java Solution

```java
import java.util.*;

class Solution {

    public int maximalRectangle(char[][] matrix) {

        if(matrix.length == 0)
            return 0;

        int cols = matrix[0].length;

        int[] heights = new int[cols];

        int maxArea = 0;

        for(char[] row : matrix){

            for(int i = 0; i < cols; i++){

                if(row[i] == '1')
                    heights[i]++;
                else
                    heights[i] = 0;
            }

            maxArea = Math.max(maxArea,
                    largestRectangleArea(heights));
        }

        return maxArea;
    }

    private int largestRectangleArea(int[] heights){

        Stack<Integer> stack = new Stack<>();

        int max = 0;

        for(int i = 0; i <= heights.length; i++){

            int current = (i == heights.length) ? 0 : heights[i];

            while(!stack.isEmpty() &&
                    current < heights[stack.peek()]){

                int height = heights[stack.pop()];

                int left = stack.isEmpty() ? -1 : stack.peek();

                int width = i - left - 1;

                max = Math.max(max, height * width);
            }

            stack.push(i);
        }

        return max;
    }
}
```

---

# Interview Insight

Most candidates try complicated 2D algorithms.

The intended solution is simply

```
Matrix

↓

Histogram

↓

Problem 84
```

Recognizing this reduction is the real challenge.

---

# Pattern Learned

Many hard interview questions are solved by transforming them into an already-known problem.

---

---

# Problem 12 — LeetCode 42. Trapping Rain Water

**Difficulty:** Hard

**Companies**

Google • Amazon • Microsoft • Meta • Apple • Bloomberg • Uber • Adobe

---

## Problem

Given elevation heights,

find how much rainwater can be trapped.

Example

```
Input

[0,1,0,2,1,0,1,3,2,1,2,1]

Output

6
```

---

# Multiple Solutions

This question has several interview-worthy approaches.

| Approach | Time | Space |
|----------|------|--------|
| Brute Force | O(N²) | O(1) |
| Prefix/Suffix Arrays | O(N) | O(N) |
| Two Pointers | O(N) | O(1) |
| Monotonic Stack | O(N) | O(N) |

Although the two-pointer solution is the most space-efficient, the stack solution demonstrates another powerful stack pattern.

---

# Monotonic Stack Idea

Maintain indices of bars in decreasing order.

Whenever a taller bar appears,

it forms the right boundary.

The stack top after popping becomes the left boundary.

The popped bar becomes the valley.

---

# Visualization

```
Height

3 0 2

Stack

3

↓

0

↓

2
```

Water trapped

```
Left Boundary = 3

Right Boundary = 2

Valley = 0

Water Height

min(3,2)-0

=2
```

Width

```
1
```

Area

```
2
```

---

# Algorithm

For every index

While

```
Current height >

Stack top
```

Pop valley.

Compute

```
Width

×

Bounded Height
```

Add to answer.

---

# Complexity

```
Time : O(N)

Space : O(N)
```

---

# Java Solution

```java
import java.util.*;

class Solution {

    public int trap(int[] height) {

        Stack<Integer> stack = new Stack<>();

        int water = 0;

        for(int i = 0; i < height.length; i++){

            while(!stack.isEmpty() &&
                    height[i] > height[stack.peek()]){

                int bottom = stack.pop();

                if(stack.isEmpty())
                    break;

                int left = stack.peek();

                int width = i - left - 1;

                int boundedHeight =
                        Math.min(height[left], height[i]) - height[bottom];

                water += width * boundedHeight;
            }

            stack.push(i);
        }

        return water;
    }
}
```

---

# Common Mistakes

### Forgetting Empty Stack Check

After popping,

there may be no left boundary.

Always check

```java
if(stack.isEmpty())
    break;
```

---

### Incorrect Width

Correct

```
width = right - left - 1
```

---

# Follow-Up

Interviewers may ask

- Solve using two pointers.
- Explain why two pointers use O(1) space.
- Which approach would you choose in production?

Expected answer:

Two pointers are generally preferred because they achieve

```
O(N) time

O(1) space
```

---

# Pattern Learned

A monotonic stack can identify

- left boundary
- valley
- right boundary

without rescanning the array.

---

## End of Part 4

### Problems Covered

- **84. Largest Rectangle in Histogram**
- **85. Maximal Rectangle**
- **42. Trapping Rain Water**

### Next (Final Part)

- **32. Longest Valid Parentheses**
- **224. Basic Calculator**
- **726. Number of Atoms**
- **Final Stack Interview Cheat Sheet**
- **FAANG Revision Checklist**


---

# Problem 13 — LeetCode 32. Longest Valid Parentheses

**Difficulty:** Hard

**Companies**

Google • Amazon • Microsoft • Meta • Apple • Bloomberg • Uber • Adobe

---

## Problem

Given a string containing only

```
'('
')'
```

find the length of the longest valid (well-formed) parentheses substring.

### Example 1

```
Input

"(()"

Output

2
```

### Example 2

```
Input

")()())"

Output

4
```

---

# Interview Pattern

Unlike **Valid Parentheses**, we are **not** checking whether the whole string is valid.

Instead, we need the **longest valid contiguous substring**.

The key challenge is remembering the last unmatched position.

---

# Brute Force

Generate every substring.

Check whether each substring is valid.

### Complexity

```
Time : O(N³)

Space : O(1)
```

Not practical.

---

# Optimal Stack Solution

Instead of storing parentheses,

store **indices**.

Initialize the stack with

```
-1
```

This acts as a sentinel representing the position before the string starts.

Rules

- `'('` → Push its index.
- `')'` → Pop one index.
- If the stack becomes empty, push the current index.
- Otherwise,

```
Current Length

=

Current Index - Stack Top
```

---

# Visualization

Input

```
)()())
```

```
Stack

[-1]

----------------

)

Pop

[]

Empty

Push 0

[0]

----------------

(

Push 1

[0,1]

----------------

)

Pop

[0]

Length

2-0 = 2

----------------

(

Push 3

[0,3]

----------------

)

Pop

[0]

Length

4-0 = 4
```

Maximum

```
4
```

---

# Why Sentinel (-1)?

Without it,

the first valid substring would compute an incorrect length.

Example

```
()
```

Length

```
1 - (-1) = 2
```

Without

```
-1
```

the calculation fails.

---

# Complexity

```
Time : O(N)

Space : O(N)
```

---

# Java Solution

```java
import java.util.*;

class Solution {

    public int longestValidParentheses(String s) {

        Stack<Integer> stack = new Stack<>();

        stack.push(-1);

        int maxLength = 0;

        for(int i = 0; i < s.length(); i++){

            if(s.charAt(i) == '('){

                stack.push(i);

            }else{

                stack.pop();

                if(stack.isEmpty()){

                    stack.push(i);

                }else{

                    maxLength = Math.max(maxLength,
                            i - stack.peek());
                }
            }
        }

        return maxLength;
    }
}
```

---

# Follow-Up Questions

- Solve using Dynamic Programming.
- Return the actual substring.
- Count all valid substrings.

---

# Pattern Learned

Sometimes a stack stores **positions instead of values** because distances matter more than the elements themselves.

---

---

# Problem 14 — LeetCode 224. Basic Calculator

**Difficulty:** Hard

**Companies**

Google • Amazon • Microsoft • Meta • Apple • Bloomberg • Uber • Oracle

---

## Problem

Evaluate an arithmetic expression containing

```
+
-
(
)
Spaces
```

Example

```
Input

"(1+(4+5+2)-3)+(6+8)"

Output

23
```

---

# Interview Pattern

This problem extends the ideas from

- Valid Parentheses
- Reverse Polish Notation

Instead of only matching parentheses,

we must also preserve

- Previous result
- Previous sign

before entering a new expression.

---

# Key Observation

Whenever

```
(
```

appears,

save

```
Current Result

Current Sign
```

Process the inner expression.

When

```
)
```

appears,

restore them.

---

# Visualization

Expression

```
2-(3+4)
```

Before

```
(

Result = 2

Sign = -

Push

2

-1
```

Solve

```
3+4 = 7
```

Restore

```
2 - 7
```

Final

```
-5
```

---

# Algorithm

Maintain

```
result

number

sign

stack
```

Rules

Digit

```
Build current number
```

Operator

```
Apply previous number

Update sign
```

Opening bracket

Push

```
result

sign
```

Reset current state.

Closing bracket

Finish current expression.

Restore

```
Previous Sign

Previous Result
```

---

# Complexity

```
Time : O(N)

Space : O(N)
```

---

# Java Solution

```java
import java.util.*;

class Solution {

    public int calculate(String s) {

        int result = 0;
        int sign = 1;
        int number = 0;

        Stack<Integer> stack = new Stack<>();

        for(int i = 0; i < s.length(); i++){

            char ch = s.charAt(i);

            if(Character.isDigit(ch)){

                number = number * 10 + (ch - '0');

            }else if(ch == '+'){

                result += sign * number;
                number = 0;
                sign = 1;

            }else if(ch == '-'){

                result += sign * number;
                number = 0;
                sign = -1;

            }else if(ch == '('){

                stack.push(result);
                stack.push(sign);

                result = 0;
                sign = 1;

            }else if(ch == ')'){

                result += sign * number;
                number = 0;

                result *= stack.pop();
                result += stack.pop();
            }
        }

        return result + sign * number;
    }
}
```

---

# Common Mistakes

### Forgetting the last number

Expressions ending with digits require

```java
return result + sign * number;
```

Otherwise,

the final operand is ignored.

---

### Ignoring spaces

Skip spaces during parsing.

---

# Follow-Up

Interviewers often extend this problem to

- Multiplication and division
- Operator precedence
- Decimal values
- Variables

This leads to **Basic Calculator II** and **Basic Calculator III**.

---

# Pattern Learned

Parsing nested expressions almost always involves

```
Current State

↓

Push

↓

Process Nested Block

↓

Pop

↓

Continue
```

---

---

# Problem 15 — LeetCode 726. Number of Atoms

**Difficulty:** Hard

**Companies**

Google • Meta • Microsoft • Bloomberg • Amazon

---

## Problem

Given a chemical formula,

return the count of every atom in sorted order.

Example

```
Input

K4(ON(SO3)2)2

Output

K4N2O14S4
```

---

# Why This Problem Is Difficult

This combines

- Stack
- Parsing
- HashMap
- Nested multipliers

It is one of the hardest parsing questions on LeetCode.

---

# Key Observation

Every pair of parentheses creates a new scope.

Each scope has its own atom counts.

When

```
)
```

appears,

multiply the entire scope and merge it into the previous one.

---

# Visualization

```
K4

(

O

N

(

S

O3

)

2

)

2
```

Processing

```
Push empty map

↓

Build inner map

↓

Multiply

↓

Merge

↓

Repeat
```

---

# Algorithm

Maintain

```
Stack<HashMap<String,Integer>>
```

Rules

```
(

Push empty map
```

```
Element

Store count
```

```
)

Pop map

Multiply

Merge
```

Finally,

sort keys alphabetically.

---

# Complexity

Suppose

```
N = formula length
```

```
Time : O(N log K)

K = distinct atom types
```

```
Space : O(N)
```

---

# Java Solution

```java
import java.util.*;

class Solution {

    public String countOfAtoms(String formula) {

        Stack<Map<String, Integer>> stack = new Stack<>();
        stack.push(new HashMap<>());

        int i = 0;

        while(i < formula.length()){

            char ch = formula.charAt(i);

            if(ch == '('){

                stack.push(new HashMap<>());
                i++;

            }else if(ch == ')'){

                i++;

                int multiplier = 0;

                while(i < formula.length() &&
                        Character.isDigit(formula.charAt(i))){

                    multiplier = multiplier * 10 +
                            formula.charAt(i) - '0';

                    i++;
                }

                if(multiplier == 0)
                    multiplier = 1;

                Map<String,Integer> top = stack.pop();

                Map<String,Integer> current = stack.peek();

                for(String atom : top.keySet()){

                    current.put(atom,
                            current.getOrDefault(atom,0) +
                            top.get(atom) * multiplier);
                }

            }else{

                int start = i++;

                while(i < formula.length() &&
                        Character.isLowerCase(formula.charAt(i)))
                    i++;

                String atom = formula.substring(start,i);

                int count = 0;

                while(i < formula.length() &&
                        Character.isDigit(formula.charAt(i))){

                    count = count * 10 +
                            formula.charAt(i)-'0';

                    i++;
                }

                if(count == 0)
                    count = 1;

                stack.peek().put(atom,
                        stack.peek().getOrDefault(atom,0)+count);
            }
        }

        TreeMap<String,Integer> sorted =
                new TreeMap<>(stack.pop());

        StringBuilder answer = new StringBuilder();

        for(String atom : sorted.keySet()){

            answer.append(atom);

            if(sorted.get(atom) > 1)
                answer.append(sorted.get(atom));
        }

        return answer.toString();
    }
}
```

---

# Follow-Up Questions

- Support square brackets.
- Preserve original ordering.
- Handle invalid formulas.
- Return a map instead of a string.

---

# Pattern Learned

Nested scopes combined with aggregation frequently indicate

```
Stack + HashMap
```

---

# FAANG Stack Pattern Cheat Sheet

| Pattern | Representative Problems |
|----------|-------------------------|
| Balanced Symbols | 20. Valid Parentheses |
| Stack Simulation | 225, 232 |
| Auxiliary Stack | 155. Min Stack |
| Expression Evaluation | 150. Reverse Polish Notation |
| Directory Navigation | 71. Simplify Path |
| Nested Parsing | 394. Decode String |
| Monotonic Increasing Stack | 739, 84, 85 |
| Monotonic Decreasing Stack | 901 |
| Boundary Detection | 42. Trapping Rain Water |
| Index-Based Stack | 32. Longest Valid Parentheses |
| Expression Parsing | 224. Basic Calculator |
| Nested Scope Aggregation | 726. Number of Atoms |

---

# Common Interview Mistakes

1. Storing values when indices are required.
2. Forgetting to check whether the stack is empty before popping.
3. Incorrect width calculation:

```
width = right - left - 1
```

4. Using `Stack` when `ArrayDeque` is preferred in Java.
5. Mishandling duplicate minimum values in **Min Stack**.
6. Reversing operand order in Reverse Polish Notation.
7. Forgetting sentinel values such as `-1` in Longest Valid Parentheses.
8. Missing the final pending number in parsing problems.
9. Forgetting multi-digit numbers while parsing.
10. Not recognizing monotonic stack patterns.

---

# Stack Pattern Recognition Guide

| Problem Clue | Likely Technique |
|--------------|------------------|
| Balanced / Nested | Stack |
| Undo / Rollback | Stack |
| Expression Evaluation | Stack |
| Previous Greater | Monotonic Stack |
| Next Greater | Monotonic Stack |
| Previous Smaller | Monotonic Stack |
| Next Smaller | Monotonic Stack |
| Running Minimum | Auxiliary Stack |
| Nested Parsing | Stack + State |
| Histogram | Monotonic Increasing Stack |

---

# Final FAANG Revision Checklist

- Understand **LIFO** intuitively.
- Know when to store **values** vs **indices**.
- Be comfortable with `ArrayDeque` as a stack replacement.
- Master **Monotonic Stack** templates:
  - Next Greater Element
  - Previous Greater Element
  - Next Smaller Element
  - Previous Smaller Element
- Recognize parsing problems requiring saved state.
- Practice sentinel techniques (`-1`, dummy height `0`).
- Memorize width calculations for histogram-based questions.
- Be able to explain amortized analysis (e.g., Stock Span, Queue using Stacks).
- Implement all 15 problems without referring to notes.
- Explain why each stack operation is necessary, not just how it works.

---

# Covered Problems

### Easy
- 20. Valid Parentheses
- 225. Implement Stack using Queues
- 232. Implement Queue using Stacks

### Medium
- 155. Min Stack
- 150. Evaluate Reverse Polish Notation
- 71. Simplify Path
- 394. Decode String
- 739. Daily Temperatures
- 901. Online Stock Span

### Hard
- 84. Largest Rectangle in Histogram
- 85. Maximal Rectangle
- 42. Trapping Rain Water
- 32. Longest Valid Parentheses
- 224. Basic Calculator
- 726. Number of Atoms

---

**End of Stack Interview Preparation Guide**
