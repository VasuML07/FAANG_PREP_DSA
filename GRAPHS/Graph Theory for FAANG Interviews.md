# Graph Theory for FAANG Interviews
> Complete LeetCode Interview Preparation Guide (Java)

---

# Graph Problems Covered

| # | Problem | Difficulty | Primary Technique |
|---|----------|------------|-------------------|
|1|Number of Islands|Medium|DFS / BFS|
|2|Clone Graph|Medium|Graph Traversal + HashMap|
|3|Rotting Oranges|Medium|Multi-Source BFS|
|4|Course Schedule|Medium|Topological Sort|
|5|Course Schedule II|Medium|Topological Ordering|
|6|Graph Valid Tree|Medium|Union Find / DFS|
|7|Number of Connected Components|Medium|Union Find|
|8|Is Graph Bipartite?|Medium|Graph Coloring|
|9|Pacific Atlantic Water Flow|Medium|Reverse DFS/BFS|
|10|Network Delay Time|Medium|Dijkstra|
|11|Cheapest Flights Within K Stops|Medium|Modified Dijkstra/BFS|
|12|Path With Minimum Effort|Medium|Dijkstra + Binary Search|
|13|Redundant Connection|Medium|Union Find|
|14|Min Cost to Connect All Points|Medium|Minimum Spanning Tree|
|15|Word Ladder|Hard|BFS|

---

# Problem 1 — Number of Islands

**LeetCode:** https://leetcode.com/problems/number-of-islands/

**Difficulty**

Medium

**Companies**

Google • Amazon • Meta • Microsoft • Bloomberg • Uber • Apple

**Classification**

DFS • BFS • Connected Components • Flood Fill

---

## Problem

Given a 2D grid consisting of `'1'` (land) and `'0'` (water), return the number of islands.

An island is formed by connecting adjacent lands horizontally or vertically.

---

## Interview Goal

The interviewer wants to verify whether you understand that:

- Grid problems are graph problems.
- Every land cell is a node.
- Four directions become graph edges.
- Counting islands = counting connected components.

This is one of the most frequently asked graph questions.

---

## Visualization

Initial Grid

```
1 1 0 0 0
1 1 0 0 0
0 0 1 0 0
0 0 0 1 1
```

Graph View

```
Island 1

A──B
│  │
C──D


Island 2

E


Island 3

F──G
```

Answer = **3**

---

## Optimal Approach (DFS)

Instead of building an explicit graph:

- Treat every cell as a graph node.
- Start DFS whenever an unvisited land cell is found.
- DFS marks the entire connected component.
- Every DFS call corresponds to one island.

---

## DFS Walkthrough

```
Start

1 1 0
1 0 0
0 1 1
```

Visit

```
X X 0
X 0 0
0 1 1
```

Island Count = 1

Continue scanning...

```
X X 0
X 0 0
0 X X
```

Island Count = 2

Finished.

---

## Java Solution

```java
class Solution {

    private final int[][] DIR = {
        {1,0},
        {-1,0},
        {0,1},
        {0,-1}
    };

    public int numIslands(char[][] grid) {

        int islands = 0;

        for(int i=0;i<grid.length;i++){

            for(int j=0;j<grid[0].length;j++){

                if(grid[i][j]=='1'){
                    islands++;
                    dfs(grid,i,j);
                }
            }
        }

        return islands;
    }

    private void dfs(char[][] grid,int r,int c){

        if(r<0 || c<0 ||
           r>=grid.length ||
           c>=grid[0].length ||
           grid[r][c]=='0')
            return;

        grid[r][c]='0';

        for(int[] d:DIR){
            dfs(grid,r+d[0],c+d[1]);
        }
    }
}
```

---

## Complexity

|Operation|Complexity|
|----------|----------|
|Time|O(M × N)|
|Space|O(M × N) recursion worst case|

---

## Interview Tricks

### Trick 1

Do **not** maintain a visited array.

Simply convert visited land into water.

```
1 -> 0
```

Memory saved.

---

### Trick 2

Always check boundaries first.

Most interview bugs come from

```
grid[r][c]
```

before validating indices.

---

### Trick 3

Never revisit nodes.

Each cell is visited exactly once.

Hence

```
O(MN)
```

not

```
O((MN)^2)
```

---

## Common Edge Cases

✔ Empty grid

✔ All water

✔ Entire grid is one island

✔ Single cell

✔ Thin rectangular grids

---

## Follow-up Questions

- Count island sizes.
- Largest island.
- Number of distinct islands.
- Dynamic islands (Union Find).
- Diagonal adjacency.

---

# Problem 2 — Clone Graph

**LeetCode**

https://leetcode.com/problems/clone-graph/

**Difficulty**

Medium

**Companies**

Meta • Google • Amazon • Microsoft • Apple • LinkedIn

**Classification**

DFS • BFS • HashMap • Graph Copy

---

## Problem

Deep copy an undirected connected graph.

Each node contains

```
value
neighbors
```

Return the cloned graph.

---

## Why This Question Matters

Many candidates accidentally make a **shallow copy**.

Interviewer wants to verify understanding of:

- cycles
- revisits
- graph traversal
- node mapping

---

## Visualization

Original

```
1 -----2
|      |
|      |
4 -----3
```

Clone

```
1'----2'
|      |
|      |
4'----3'
```

Every node must be newly allocated.

---

## Key Insight

Need mapping

```
Original Node
      ↓
Cloned Node
```

Without this mapping,

cycles create infinite recursion.

---

## DFS Algorithm

```
clone(node)

already cloned?

YES

return clone

NO

create clone

store map

clone every neighbor

return clone
```

---

## Java Solution

```java
class Solution {

    private Map<Node, Node> map = new HashMap<>();

    public Node cloneGraph(Node node) {

        if(node==null)
            return null;

        if(map.containsKey(node))
            return map.get(node);

        Node clone = new Node(node.val);

        map.put(node, clone);

        for(Node nei : node.neighbors){
            clone.neighbors.add(cloneGraph(nei));
        }

        return clone;
    }
}
```

---

## Complexity

|Operation|Complexity|
|----------|----------|
|Time|O(V+E)|
|Space|O(V)|

---

## ASCII Execution

```
Visit 1

Create 1'

↓

Visit 2

Create 2'

↓

Visit 3

Create 3'

↓

Visit 4

Create 4'

↓

Neighbor = 1

Already cloned

Return map[1]
```

---

## Interview Tricks

Never compare by value.

Different nodes may have identical values.

Always map by reference.

---

## Common Mistakes

❌ Forgetting HashMap

❌ Infinite recursion

❌ Copying neighbor references directly

❌ Shallow copy

---

## Follow-ups

- Clone directed graph
- Clone disconnected graph
- BFS implementation
- Serialize + Deserialize Graph

---

### LLM-Proof Question ⭐

**Why this is LLM-proof**

Unlike standard traversal questions, Clone Graph requires preserving **object identity**, handling **cycles**, and constructing a new graph incrementally. Small implementation mistakes (e.g., mapping by value instead of node reference) produce subtle bugs. Interviewers often introduce custom node structures or additional metadata to test reasoning rather than memorized patterns.

---

# Problem 3 — Rotting Oranges

**LeetCode**

https://leetcode.com/problems/rotting-oranges/

**Difficulty**

Medium

**Companies**

Amazon • Google • Microsoft • DoorDash • Meta

**Classification**

Multi-Source BFS • Grid Graph • Shortest Time Simulation

---

## Problem

A rotten orange rots all adjacent fresh oranges every minute.

Return the minimum minutes until no fresh orange remains, or `-1` if impossible.

---

## Key Insight

Instead of running BFS from each rotten orange independently:

- Treat **all rotten oranges as starting points**.
- Push every rotten orange into the queue initially.
- Process level by level.
- Each BFS level represents one minute.

This is the canonical **Multi-Source BFS** pattern.

---

## Visualization

Initial state:

```text
2 1 1
1 1 0
0 1 1
```

Minute 0:

```text
2 1 1
1 1 0
0 1 1
```

Minute 1:

```text
2 2 1
2 1 0
0 1 1
```

Minute 2:

```text
2 2 2
2 2 0
0 1 1
```

Minute 3:

```text
2 2 2
2 2 0
0 2 1
```

Minute 4:

```text
2 2 2
2 2 0
0 2 2
```

Answer = **4**

---

## Java Solution

```java
class Solution {

    private final int[][] DIR = {
        {1,0},{-1,0},{0,1},{0,-1}
    };

    public int orangesRotting(int[][] grid) {

        Queue<int[]> queue = new LinkedList<>();
        int fresh = 0;

        for(int i=0;i<grid.length;i++){
            for(int j=0;j<grid[0].length;j++){

                if(grid[i][j]==2)
                    queue.offer(new int[]{i,j});

                if(grid[i][j]==1)
                    fresh++;
            }
        }

        if(fresh==0)
            return 0;

        int minutes = 0;

        while(!queue.isEmpty()){

            int size = queue.size();

            while(size-- > 0){

                int[] cur = queue.poll();

                for(int[] d:DIR){

                    int nr = cur[0]+d[0];
                    int nc = cur[1]+d[1];

                    if(nr<0 || nc<0 ||
                       nr>=grid.length ||
                       nc>=grid[0].length)
                        continue;

                    if(grid[nr][nc]!=1)
                        continue;

                    grid[nr][nc]=2;
                    fresh--;

                    queue.offer(new int[]{nr,nc});
                }
            }

            minutes++;
        }

        return fresh==0 ? minutes-1 : -1;
    }
}
```

---

## Complexity

| Metric | Complexity |
|---------|------------|
| Time | O(M × N) |
| Space | O(M × N) |

---

## Interview Tips

- Each BFS level = one minute.
- Initialize the queue with **all** rotten oranges.
- Track the number of fresh oranges to avoid rescanning the grid.

---

## Common Edge Cases

- No fresh oranges.
- No rotten oranges.
- Fresh oranges isolated by empty cells.
- Single-cell grid.

---

## Follow-ups

- 8-direction infection.
- Weighted infection time.
- Different infection speeds.
- Obstacles that block spread.

---

# Problem 4 — Course Schedule

**LeetCode**

https://leetcode.com/problems/course-schedule/

**Difficulty**

Medium

**Companies**

Google • Meta • Amazon • Microsoft • Apple • Airbnb

**Classification**

Topological Sort • Directed Graph • Cycle Detection

**LLM-Proof Question ⭐**

---

## Problem

Given `numCourses` and prerequisite pairs, determine whether it is possible to finish all courses.

Example:

```
1 → 0
```

means:

To study **1**, you must first complete **0**.

Return `true` if all courses can be completed.

---

## Key Insight

A valid ordering exists **iff the directed graph contains no cycle**.

Rather than explicitly searching for cycles, perform **Kahn's Algorithm** (BFS Topological Sort):

- Compute indegree of every node.
- Push all nodes with indegree 0.
- Remove them one by one.
- Decrease indegrees of neighbors.
- If every node is processed, there is no cycle.

---

## Visualization

```text
0 → 1 → 3
 \
  \
   → 2 → 3
```

Initial indegree:

|Node|0|1|2|3|
|----|--|--|--|--|
|InDegree|0|1|1|2|

Queue:

```
[0]
```

Processing order:

```
0

↓

1 2

↓

3
```

Processed = 4 nodes

Answer = true

---

## Java Solution

```java
class Solution {

    public boolean canFinish(int numCourses, int[][] prerequisites) {

        List<List<Integer>> graph = new ArrayList<>();

        for(int i=0;i<numCourses;i++)
            graph.add(new ArrayList<>());

        int[] indegree = new int[numCourses];

        for(int[] edge : prerequisites){

            graph.get(edge[1]).add(edge[0]);
            indegree[edge[0]]++;
        }

        Queue<Integer> queue = new LinkedList<>();

        for(int i=0;i<numCourses;i++)
            if(indegree[i]==0)
                queue.offer(i);

        int visited = 0;

        while(!queue.isEmpty()){

            int node = queue.poll();
            visited++;

            for(int next : graph.get(node)){

                indegree[next]--;

                if(indegree[next]==0)
                    queue.offer(next);
            }
        }

        return visited==numCourses;
    }
}
```

---

## Complexity

| Metric | Complexity |
|---------|------------|
| Time | O(V + E) |
| Space | O(V + E) |

---

## Interview Tricks

- A DAG always has at least one node with indegree 0.
- If the queue becomes empty before processing all nodes, a cycle exists.
- Kahn's Algorithm often yields cleaner code than DFS cycle detection.

---

## Common Edge Cases

- No prerequisites.
- Single course.
- Self-loop (e.g., `0 → 0`).
- Multiple disconnected dependency chains.

---

## Why This Is LLM-Proof

Interviewers frequently extend this problem beyond basic topological sorting, for example:

- Add or remove prerequisites dynamically.
- Return one valid schedule or all possible schedules.
- Prioritize courses with weights or deadlines.
- Detect which subset of courses forms a cycle.

These variations require adapting the algorithm rather than recognizing a memorized template.

---

## Follow-ups

- Return the actual ordering (**Course Schedule II**).
- Detect the cycle itself.
- Parallel course scheduling.
- Alien Dictionary (generalized topological sorting).



---

# Problem 5 — Course Schedule II

**LeetCode**

https://leetcode.com/problems/course-schedule-ii/

**Difficulty**

Medium

**Companies**

Google • Meta • Amazon • Microsoft • Apple • Airbnb

**Classification**

Topological Sort • BFS (Kahn's Algorithm) • DAG

---

## Problem

Return **one valid ordering** of courses that allows completion of all courses.

If impossible, return an empty array.

Unlike Course Schedule I, we now need the **actual topological ordering** rather than simply detecting whether one exists.

---

## Interview Goal

The interviewer wants to test whether you can extend cycle detection into **constructing the ordering**.

The algorithm remains almost identical:

- Build graph
- Compute indegree
- Process nodes with indegree 0
- Store processing order
- Verify every node is processed

---

## Visualization

Prerequisites

```
1 <- 0
2 <- 0
3 <- 1
3 <- 2
```

Graph

```
      0
     / \
    1   2
     \ /
      3
```

Topological Order

```
0

↓

1 2

↓

3
```

Possible answers

```
[0,1,2,3]

or

[0,2,1,3]
```

Both are valid.

---

## Algorithm

1. Construct adjacency list.
2. Compute indegree.
3. Push all zero-indegree nodes.
4. Remove nodes one by one.
5. Append each removed node into answer.
6. If answer size != number of courses → cycle exists.

---

## Java Solution

```java
class Solution {

    public int[] findOrder(int numCourses, int[][] prerequisites) {

        List<List<Integer>> graph = new ArrayList<>();

        for(int i=0;i<numCourses;i++)
            graph.add(new ArrayList<>());

        int[] indegree = new int[numCourses];

        for(int[] edge : prerequisites){

            graph.get(edge[1]).add(edge[0]);
            indegree[edge[0]]++;
        }

        Queue<Integer> queue = new LinkedList<>();

        for(int i=0;i<numCourses;i++){

            if(indegree[i]==0)
                queue.offer(i);
        }

        int[] order = new int[numCourses];
        int index = 0;

        while(!queue.isEmpty()){

            int node = queue.poll();

            order[index++] = node;

            for(int next : graph.get(node)){

                indegree[next]--;

                if(indegree[next]==0)
                    queue.offer(next);
            }
        }

        if(index!=numCourses)
            return new int[0];

        return order;
    }
}
```

---

## Complexity

|Metric|Complexity|
|------|----------|
|Time|O(V+E)|
|Space|O(V+E)|

---

## Interview Tricks

### Multiple Correct Answers

Topological sort is **not unique**.

Example

```
A

↓

B   C

↓

D
```

Both

```
A B C D

and

A C B D
```

are valid.

---

### Queue Size

If multiple nodes have indegree zero,

```
choose any.
```

The interviewer knows multiple answers exist.

---

### Common Bug

Don't forget

```java
order[index++] = node;
```

Many candidates only count visited nodes.

---

## Edge Cases

✔ No prerequisites

✔ One course

✔ Cycle

✔ Multiple disconnected DAGs

---

## Interview Follow-ups

- Return lexicographically smallest ordering.
- Count all possible topological orderings.
- Detect unique ordering.
- Parallel execution scheduling.

---

# Problem 6 — Graph Valid Tree

**LeetCode**

https://leetcode.com/problems/graph-valid-tree/

*(Premium — frequently asked in interviews)*

**Difficulty**

Medium

**Companies**

Google • Meta • Amazon • Microsoft • Apple

**Classification**

Union Find • DFS • Connected Components

---

## Problem

Given

- n nodes
- undirected edges

Determine whether the graph forms a valid tree.

---

## Tree Properties

A graph is a tree **iff**

```
No cycles

AND

Exactly one connected component
```

Equivalent property

```
Edges = Nodes - 1

AND

Connected
```

---

## Visualization

Valid Tree

```
0

|

1

/ \

2  3

    |

    4
```

Invalid

Cycle

```
0
| \
1--2
```

Disconnected

```
0—1

2—3
```

---

## Interview Insight

Union Find naturally detects cycles while building the graph.

If

```
find(u)==find(v)
```

then

```
cycle detected.
```

---

## Algorithm

Step 1

Check

```
edges == n-1
```

If false

Immediately return false.

---

Step 2

Union every edge.

If

```
parent(u)==parent(v)
```

Cycle exists.

---

Step 3

Return true.

---

## Java Solution

```java
class Solution {

    class DSU{

        int[] parent;
        int[] rank;

        DSU(int n){

            parent=new int[n];
            rank=new int[n];

            for(int i=0;i<n;i++)
                parent[i]=i;
        }

        int find(int x){

            if(parent[x]!=x)
                parent[x]=find(parent[x]);

            return parent[x];
        }

        boolean union(int a,int b){

            int pa=find(a);
            int pb=find(b);

            if(pa==pb)
                return false;

            if(rank[pa]<rank[pb]){

                parent[pa]=pb;

            }else if(rank[pb]<rank[pa]){

                parent[pb]=pa;

            }else{

                parent[pb]=pa;
                rank[pa]++;
            }

            return true;
        }
    }

    public boolean validTree(int n,int[][] edges){

        if(edges.length!=n-1)
            return false;

        DSU dsu=new DSU(n);

        for(int[] edge:edges){

            if(!dsu.union(edge[0],edge[1]))
                return false;
        }

        return true;
    }
}
```

---

## Complexity

|Metric|Complexity|
|------|----------|
|Time|O(E α(N))|
|Space|O(N)|

---

## Why Union Find?

Without Union Find

DFS

```
O(V+E)
```

works.

Union Find becomes especially valuable when

- edges arrive dynamically
- graph changes continuously
- online connectivity queries exist

---

## Common Mistakes

Checking only

```
edges==nodes-1
```

is insufficient.

Example

```
0—1

2—3
```

Edges = Nodes − 1

Yet disconnected.

---

## Follow-ups

- Return connected components.
- Remove one edge to make tree.
- Dynamic graph validation.
- Count spanning trees.

---

# Problem 7 — Number of Connected Components in an Undirected Graph

**LeetCode**

https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/

*(Premium)*

**Difficulty**

Medium

**Companies**

Google • Meta • Amazon • Bloomberg • Uber

**Classification**

DFS • BFS • Union Find

---

## Problem

Return the number of connected components.

---

Example

```
0—1—2

3—4
```

Answer

```
2
```

---

## Insight

Exactly the same idea as

**Number of Islands**

except

Grid

↓

General Graph

Each DFS discovers one component.

---

## Visualization

```
0 -----1

|

2


3-----4


5
```

Connected Components

```
{0,1,2}

{3,4}

{5}
```

Answer = 3

---

## DFS Algorithm

```
visit node

↓

mark visited

↓

visit neighbors

↓

finished component
```

Repeat for every unvisited node.

---

## Java Solution

```java
class Solution {

    public int countComponents(int n, int[][] edges) {

        List<List<Integer>> graph = new ArrayList<>();

        for(int i=0;i<n;i++)
            graph.add(new ArrayList<>());

        for(int[] edge:edges){

            graph.get(edge[0]).add(edge[1]);
            graph.get(edge[1]).add(edge[0]);
        }

        boolean[] visited=new boolean[n];

        int count=0;

        for(int i=0;i<n;i++){

            if(!visited[i]){

                dfs(graph,visited,i);
                count++;
            }
        }

        return count;
    }

    private void dfs(List<List<Integer>> graph,
                     boolean[] visited,
                     int node){

        visited[node]=true;

        for(int next:graph.get(node)){

            if(!visited[next])
                dfs(graph,visited,next);
        }
    }
}
```

---

## Complexity

|Metric|Complexity|
|------|----------|
|Time|O(V+E)|
|Space|O(V)|

---

## Interview Tricks

If graph is extremely large and edges stream continuously,

replace DFS with

```
Union Find
```

because repeated DFS becomes expensive.

---

## Edge Cases

✔ Isolated vertices

✔ No edges

✔ Fully connected graph

✔ Single node

---

## Follow-ups

- Component sizes.
- Largest component.
- Dynamic connectivity.
- Provinces (LeetCode 547).

---

### LLM-Proof Question ⭐

This problem becomes significantly harder when interviewers introduce:

- Streaming edge updates.
- Millions of vertices that cannot fit into memory.
- Distributed graph processing.
- Online connectivity queries.

These extensions require selecting appropriate data structures (e.g., Union-Find or distributed processing) rather than simply applying DFS from memory.

---

# Problem 8 — Is Graph Bipartite?

**LeetCode**

https://leetcode.com/problems/is-graph-bipartite/

**Difficulty**

Medium

**Companies**

Google • Meta • Amazon • Microsoft • TikTok

**Classification**

Graph Coloring • BFS • DFS

---

## Problem

Determine whether every edge connects nodes of **different colors**.

Equivalent question:

Can the graph be divided into two sets where no adjacent nodes belong to the same set?

---

## Key Insight

Only two colors are required.

```
Color A

↓

Neighbors

↓

Color B

↓

Neighbors

↓

Color A
```

If a conflict occurs,

graph is **not bipartite**.

---

## Visualization

Valid

```
Red

0

/ \

1   2

Blue
```

Invalid

```
0

/ \

1---2
```

Odd cycle

Impossible to color.

---

## BFS Coloring

Initially

```
Color[0]=0
```

Neighbors

```
Color=1
```

Their neighbors

```
Color=0
```

Continue.

Conflict?

Return false.

---

## Java Solution

```java
class Solution {

    public boolean isBipartite(int[][] graph) {

        int n = graph.length;

        int[] color = new int[n];

        Arrays.fill(color,-1);

        Queue<Integer> queue = new LinkedList<>();

        for(int i=0;i<n;i++){

            if(color[i]!=-1)
                continue;

            color[i]=0;
            queue.offer(i);

            while(!queue.isEmpty()){

                int node = queue.poll();

                for(int next : graph[node]){

                    if(color[next]==-1){

                        color[next]=1-color[node];
                        queue.offer(next);

                    }else if(color[next]==color[node]){

                        return false;
                    }
                }
            }
        }

        return true;
    }
}
```

---

## Complexity

|Metric|Complexity|
|------|----------|
|Time|O(V+E)|
|Space|O(V)|

---

## Interview Tricks

Odd-length cycles

↓

Not Bipartite

Even-length cycles

↓

Always Bipartite

This observation alone solves many theoretical interview questions.

---

## Common Edge Cases

✔ Disconnected graph

✔ Single node

✔ No edges

✔ Self-loop (immediately false)

---

## Interview Follow-ups

- Possible Bipartition (LeetCode 886)
- Graph Coloring with K colors
- Detect odd cycle explicitly
- Maximum Bipartite Matching (Hopcroft–Karp)

---

# Problem 9 — Pacific Atlantic Water Flow

**LeetCode**

https://leetcode.com/problems/pacific-atlantic-water-flow/

**Difficulty**

Medium

**Companies**

Google • Amazon • Meta • Microsoft • Apple

**Classification**

Reverse DFS • Reverse BFS • Reachability • Matrix Graph

**LLM-Proof Question ⭐**

---

## Problem

Given an `m × n` matrix of heights:

- Water can flow from a cell to neighboring cells of **equal or lower height**.
- The **Pacific Ocean** touches the left and top borders.
- The **Atlantic Ocean** touches the right and bottom borders.

Return every cell that can reach **both oceans**.

---

## Interview Insight

A brute-force solution would start DFS/BFS from **every cell**, resulting in:

```
O((MN)^2)
```

Instead, reverse the thinking.

Rather than asking:

> Can this cell reach the ocean?

Ask:

> Which cells can the ocean reach if water flowed backwards?

Reverse traversal changes the condition:

Instead of

```
Current >= Next
```

we move only if

```
Next >= Current
```

This converts many repeated searches into only **two traversals**.

---

## Visualization

```
Pacific

~~~~~~~~~~~~~

1 2 2 3

3 2 3 4

2 4 5 3

Atlantic

~~~~~~~~~~~~~
```

DFS from Pacific borders

↓

Reachable Set A

DFS from Atlantic borders

↓

Reachable Set B

Final Answer

```
A ∩ B
```

---

## Algorithm

1. DFS/BFS from Pacific borders.
2. Mark reachable cells.
3. DFS/BFS from Atlantic borders.
4. Mark reachable cells.
5. Return cells visited by both traversals.

---

## Java Solution

```java
class Solution {

    private final int[][] DIR = {
        {1,0},{-1,0},{0,1},{0,-1}
    };

    public List<List<Integer>> pacificAtlantic(int[][] heights) {

        int m = heights.length;
        int n = heights[0].length;

        boolean[][] pacific = new boolean[m][n];
        boolean[][] atlantic = new boolean[m][n];

        for(int i=0;i<m;i++){

            dfs(heights,pacific,i,0);
            dfs(heights,atlantic,i,n-1);
        }

        for(int j=0;j<n;j++){

            dfs(heights,pacific,0,j);
            dfs(heights,atlantic,m-1,j);
        }

        List<List<Integer>> ans = new ArrayList<>();

        for(int i=0;i<m;i++){

            for(int j=0;j<n;j++){

                if(pacific[i][j] && atlantic[i][j]){

                    ans.add(Arrays.asList(i,j));
                }
            }
        }

        return ans;
    }

    private void dfs(int[][] h,
                     boolean[][] vis,
                     int r,
                     int c){

        if(vis[r][c])
            return;

        vis[r][c]=true;

        for(int[] d:DIR){

            int nr=r+d[0];
            int nc=c+d[1];

            if(nr<0 || nc<0 ||
               nr>=h.length ||
               nc>=h[0].length)
                continue;

            if(vis[nr][nc])
                continue;

            if(h[nr][nc] < h[r][c])
                continue;

            dfs(h,vis,nr,nc);
        }
    }
}
```

---

## Complexity

|Metric|Complexity|
|------|----------|
|Time|O(M×N)|
|Space|O(M×N)|

---

## Key Interview Tricks

- Reverse the search.
- Think in terms of **reachability**, not simulation.
- Running DFS from boundaries is dramatically cheaper than from every node.

---

## Edge Cases

- Single row.
- Single column.
- Equal heights everywhere.
- Strictly increasing matrix.
- Strictly decreasing matrix.

---

## Why This Is LLM-Proof

The difficult part is recognizing the **reverse traversal transformation**.

Many candidates (and code generators) default to DFS from every cell. Interviewers often modify the flow rules, add diagonal movement, or introduce multiple destination regions, requiring abstraction rather than memorized solutions.

---

## Follow-ups

- Multiple oceans.
- Diagonal movement.
- Return path instead of coordinates.
- Dynamic height updates.

---

# Problem 10 — Network Delay Time

**LeetCode**

https://leetcode.com/problems/network-delay-time/

**Difficulty**

Medium

**Companies**

Google • Amazon • Meta • Microsoft • Bloomberg • Uber

**Classification**

Dijkstra • Shortest Path • Weighted Graph

---

## Problem

You are given a directed weighted graph.

Signal starts from node `k`.

Return the minimum time required for **all nodes** to receive the signal.

Return `-1` if impossible.

---

## Interview Insight

BFS works only for **equal-weight edges**.

Once edge weights differ,

```
Use Dijkstra.
```

Dijkstra always expands the node with the smallest currently known distance.

---

## Visualization

```
        2

1 ------------> 2

 \              |

4 \             |1

   \            |

    > 3 --------

        2
```

Distance Evolution

```
Start

dist[1]=0

↓

Visit 1

↓

dist[2]=2

dist[3]=4

↓

Visit 2

↓

dist[3]=3
```

Shortest paths found.

---

## Algorithm

- Build adjacency list.
- Maintain distance array.
- Use min-heap.
- Ignore outdated heap entries.
- Maximum distance among all nodes is the answer.

---

## Java Solution

```java
class Solution {

    public int networkDelayTime(int[][] times,
                                int n,
                                int k) {

        List<List<int[]>> graph = new ArrayList<>();

        for(int i=0;i<=n;i++)
            graph.add(new ArrayList<>());

        for(int[] edge:times){

            graph.get(edge[0]).add(
                new int[]{edge[1],edge[2]}
            );
        }

        int[] dist=new int[n+1];

        Arrays.fill(dist,Integer.MAX_VALUE);

        dist[k]=0;

        PriorityQueue<int[]> pq =
            new PriorityQueue<>(
                (a,b)->a[1]-b[1]
            );

        pq.offer(new int[]{k,0});

        while(!pq.isEmpty()){

            int[] cur=pq.poll();

            int node=cur[0];
            int cost=cur[1];

            if(cost>dist[node])
                continue;

            for(int[] next:graph.get(node)){

                int nei=next[0];
                int wt=next[1];

                if(cost+wt<dist[nei]){

                    dist[nei]=cost+wt;

                    pq.offer(
                        new int[]{nei,dist[nei]}
                    );
                }
            }
        }

        int ans=0;

        for(int i=1;i<=n;i++){

            if(dist[i]==Integer.MAX_VALUE)
                return -1;

            ans=Math.max(ans,dist[i]);
        }

        return ans;
    }
}
```

---

## Complexity

|Metric|Complexity|
|------|----------|
|Time|O((V+E) log V)|
|Space|O(V+E)|

---

## Interview Tricks

Priority Queue always stores

```
(node,distance)
```

Never

```
(node,edgeWeight)
```

---

Ignore stale entries

```java
if(cost > dist[node])
    continue;
```

This avoids an explicit visited array.

---

## Common Edge Cases

- Disconnected graph.
- Single node.
- Duplicate edges.
- Multiple shortest paths.

---

## Follow-ups

- Bellman-Ford.
- Negative edges.
- Floyd-Warshall.
- A* Search.

---

# Problem 11 — Cheapest Flights Within K Stops

**LeetCode**

https://leetcode.com/problems/cheapest-flights-within-k-stops/

**Difficulty**

Medium

**Companies**

Google • Amazon • Meta • Expedia • Uber

**Classification**

Modified Dijkstra • BFS • Dynamic Programming

---

## Problem

Find the cheapest flight from `src` to `dst` using **at most K stops**.

---

## Interview Insight

Normal Dijkstra is insufficient.

Why?

Because

```
Cheapest path

≠

Fewest stops
```

A state must contain:

```
(city,
cost,
stops)
```

instead of just

```
(city,
cost)
```

---

## Visualization

```
0

|100

↓

1

|100

↓

2

Direct

0 ----500---->2
```

With

```
K=0
```

Answer

```
500
```

With

```
K=1
```

Answer

```
200
```

---

## State Expansion

Priority Queue

```
(city,cost,stops)
```

Distance depends on

```
city

AND

stops used.
```

---

## Java Solution

```java
class Solution {

    public int findCheapestPrice(int n,
                                 int[][] flights,
                                 int src,
                                 int dst,
                                 int k) {

        List<List<int[]>> graph = new ArrayList<>();

        for(int i=0;i<n;i++)
            graph.add(new ArrayList<>());

        for(int[] f:flights){

            graph.get(f[0]).add(
                new int[]{f[1],f[2]}
            );
        }

        PriorityQueue<int[]> pq =
            new PriorityQueue<>(
                (a,b)->a[1]-b[1]
            );

        pq.offer(new int[]{src,0,0});

        int[] stops=new int[n];

        Arrays.fill(stops,Integer.MAX_VALUE);

        while(!pq.isEmpty()){

            int[] cur=pq.poll();

            int city=cur[0];
            int cost=cur[1];
            int used=cur[2];

            if(city==dst)
                return cost;

            if(used>k || used>stops[city])
                continue;

            stops[city]=used;

            for(int[] next:graph.get(city)){

                pq.offer(
                    new int[]{
                        next[0],
                        cost+next[1],
                        used+1
                    }
                );
            }
        }

        return -1;
    }
}
```

---

## Complexity

|Metric|Complexity|
|------|----------|
|Time|O(E log V) (average)|
|Space|O(V+E)|

---

## Interview Tricks

Unlike classic Dijkstra,

state contains

```
(city,
cost,
stops)
```

Ignoring stops leads to incorrect pruning.

---

## Common Edge Cases

- Destination unreachable.
- K = 0.
- Multiple equal-cost routes.
- Graph containing cycles.

---

## Follow-ups

- Exact K stops.
- Maximum budget.
- Return actual itinerary.
- Multiple destinations.

---

# Problem 12 — Path With Minimum Effort

**LeetCode**

https://leetcode.com/problems/path-with-minimum-effort/

**Difficulty**

Medium

**Companies**

Google • Amazon • Microsoft • Apple

**Classification**

Dijkstra • Binary Search + BFS • Grid Graph

---

## Problem

Moving between adjacent cells incurs effort:

```
abs(heightDifference)
```

The effort of a path equals the **maximum edge cost** on that path.

Return the minimum possible effort.

---

## Interview Insight

This is **not** the shortest path.

Instead of minimizing

```
sum(weights)
```

we minimize

```
maximum edge weight
```

State transition becomes

```
newEffort = max(currentEffort, edgeWeight)
```

---

## Visualization

```
1 2 2

3 8 2

5 3 5
```

Possible path

```
1

↓

3

↓

5

↓

3

↓

5
```

Maximum edge difference

```
2
```

---

## Java Solution

```java
class Solution {

    private final int[][] DIR = {
        {1,0},{-1,0},{0,1},{0,-1}
    };

    public int minimumEffortPath(int[][] heights) {

        int m = heights.length;
        int n = heights[0].length;

        int[][] effort = new int[m][n];

        for(int[] row:effort)
            Arrays.fill(row,Integer.MAX_VALUE);

        PriorityQueue<int[]> pq =
            new PriorityQueue<>(
                (a,b)->a[2]-b[2]
            );

        effort[0][0]=0;

        pq.offer(new int[]{0,0,0});

        while(!pq.isEmpty()){

            int[] cur=pq.poll();

            int r=cur[0];
            int c=cur[1];
            int e=cur[2];

            if(r==m-1 && c==n-1)
                return e;

            if(e>effort[r][c])
                continue;

            for(int[] d:DIR){

                int nr=r+d[0];
                int nc=c+d[1];

                if(nr<0 || nc<0 ||
                   nr>=m || nc>=n)
                    continue;

                int next=Math.max(
                    e,
                    Math.abs(
                        heights[r][c]-heights[nr][nc]
                    )
                );

                if(next<effort[nr][nc]){

                    effort[nr][nc]=next;

                    pq.offer(
                        new int[]{nr,nc,next}
                    );
                }
            }
        }

        return 0;
    }
}
```

---

## Complexity

|Metric|Complexity|
|------|----------|
|Time|O(MN log(MN))|
|Space|O(MN)|

---

## Interview Tricks

Replace

```
distance += weight
```

with

```
distance = max(distance, weight)
```

This tiny modification transforms standard Dijkstra into a minimax path algorithm.

---

## Common Edge Cases

- Single cell.
- Equal heights.
- Large height differences.
- Multiple optimal paths.

---

## Follow-ups

- Binary Search + BFS solution.
- Return the actual path.
- Diagonal movement.
- Multiple destination queries.

---

# Problem 13 — Redundant Connection

**LeetCode**

https://leetcode.com/problems/redundant-connection/

**Difficulty**

Medium

**Companies**

Google • Meta • Amazon • Microsoft • Bloomberg

**Classification**

Union-Find (Disjoint Set Union) • Cycle Detection

---

## Problem

You are given a connected undirected graph that started as a tree with one extra edge added.

Return the edge that can be removed so the graph becomes a tree again.

---

## Interview Insight

A tree never contains cycles.

As we process edges:

- If two nodes belong to different components → merge them.
- If two nodes already belong to the same component → this edge creates the cycle.

That edge is the answer.

---

## Visualization

```
Input

1 -----2
 \     |
  \    |
   \   |
     3

Process

(1,2) ✓
(1,3) ✓
(2,3) ✗

Cycle Found

Answer = (2,3)
```

---

## Algorithm

```
For every edge

↓

find(u)

find(v)

↓

Same parent?

YES

Return edge

NO

Union them
```

---

## Java Solution

```java
class Solution {

    class DSU {

        int[] parent;
        int[] rank;

        DSU(int n) {
            parent = new int[n + 1];
            rank = new int[n + 1];

            for (int i = 0; i <= n; i++)
                parent[i] = i;
        }

        int find(int x) {
            if (parent[x] != x)
                parent[x] = find(parent[x]);

            return parent[x];
        }

        boolean union(int a, int b) {

            int pa = find(a);
            int pb = find(b);

            if (pa == pb)
                return false;

            if (rank[pa] < rank[pb]) {
                parent[pa] = pb;
            } else if (rank[pb] < rank[pa]) {
                parent[pb] = pa;
            } else {
                parent[pb] = pa;
                rank[pa]++;
            }

            return true;
        }
    }

    public int[] findRedundantConnection(int[][] edges) {

        DSU dsu = new DSU(edges.length);

        for (int[] edge : edges) {

            if (!dsu.union(edge[0], edge[1]))
                return edge;
        }

        return new int[0];
    }
}
```

---

## Complexity

|Metric|Complexity|
|------|----------|
|Time|O(E α(N))|
|Space|O(N)|

---

## Interview Tricks

- Union-Find is almost always the intended solution.
- DFS works but is slower if performed after every edge insertion.
- Path Compression + Union by Rank gives near O(1) operations.

---

## Edge Cases

- Cycle near the beginning.
- Cycle at the end.
- Large chain plus one extra edge.

---

## Follow-ups

- Directed graph (**Redundant Connection II**)
- Return every redundant edge.
- Dynamic edge insertions.

---

# Problem 14 — Min Cost to Connect All Points

**LeetCode**

https://leetcode.com/problems/min-cost-to-connect-all-points/

**Difficulty**

Medium

**Companies**

Amazon • Google • Meta • Microsoft • Apple

**Classification**

Minimum Spanning Tree • Prim's Algorithm • Kruskal's Algorithm

---

## Problem

Given points on a 2D plane,

```
Cost = Manhattan Distance
```

Connect every point with minimum total cost.

---

## Interview Insight

This is a textbook **Minimum Spanning Tree (MST)** problem.

Important observation:

We do **not** want shortest paths.

We want

```
Minimum total cost
```

while connecting **all** vertices.

---

## Visualization

```
Points

A

      B

          C

Possible MST

A ----- B
        |
        |
        C
```

---

## Prim's Algorithm

```
Start anywhere

↓

Add cheapest edge leaving MST

↓

Repeat

↓

Until all nodes included
```

---

## Java Solution

```java
class Solution {

    public int minCostConnectPoints(int[][] points) {

        int n = points.length;

        boolean[] visited = new boolean[n];

        PriorityQueue<int[]> pq =
                new PriorityQueue<>(
                        (a, b) -> a[1] - b[1]);

        pq.offer(new int[]{0, 0});

        int answer = 0;
        int edgesUsed = 0;

        while (!pq.isEmpty()) {

            int[] cur = pq.poll();

            int node = cur[0];
            int cost = cur[1];

            if (visited[node])
                continue;

            visited[node] = true;

            answer += cost;

            edgesUsed++;

            if (edgesUsed == n)
                break;

            for (int next = 0; next < n; next++) {

                if (visited[next])
                    continue;

                int dist =
                        Math.abs(points[node][0] - points[next][0])
                                + Math.abs(points[node][1] - points[next][1]);

                pq.offer(new int[]{next, dist});
            }
        }

        return answer;
    }
}
```

---

## Complexity

|Metric|Complexity|
|------|----------|
|Time|O(N² log N)|
|Space|O(N)|

---

## Prim vs Kruskal

|Prim|Kruskal|
|----|--------|
|Grows one tree|Sorts edges|
|Uses Priority Queue|Uses Union-Find|
|Better for dense graphs|Better for sparse graphs|

---

## Interview Tricks

Shortest Path

≠

Minimum Spanning Tree

Many candidates mistakenly attempt Dijkstra.

---

## Edge Cases

- Single point.
- Duplicate coordinates.
- Collinear points.

---

## Follow-ups

- Kruskal implementation.
- Euclidean distance.
- Dynamic point insertion.
- Second-best MST.

---

# Problem 15 — Word Ladder

**LeetCode**

https://leetcode.com/problems/word-ladder/

**Difficulty**

Hard

**Companies**

Google • Amazon • Meta • Microsoft • Apple • LinkedIn

**Classification**

Graph Construction • BFS • Shortest Path

**LLM-Proof Question ⭐**

---

## Problem

Transform

```
beginWord

↓

endWord
```

changing **one character at a time**.

Each intermediate word must exist in the dictionary.

Return the minimum number of transformations.

---

## Interview Insight

Words are graph nodes.

Two words are connected if they differ by exactly one character.

Instead of explicitly building every edge,

generate neighbors by replacing each character with

```
a...z
```

This avoids O(N²) comparisons.

---

## Visualization

```
hit

↓

hot

↓

dot

↓

dog

↓

cog
```

Shortest Path Length

```
5
```

---

## Algorithm

```
Queue

↓

Current word

↓

Generate neighbors

↓

Visit unseen words

↓

Stop when destination reached
```

---

## Java Solution

```java
class Solution {

    public int ladderLength(String beginWord,
                            String endWord,
                            List<String> wordList) {

        Set<String> dict = new HashSet<>(wordList);

        if (!dict.contains(endWord))
            return 0;

        Queue<String> queue = new LinkedList<>();
        queue.offer(beginWord);

        Set<String> visited = new HashSet<>();
        visited.add(beginWord);

        int level = 1;

        while (!queue.isEmpty()) {

            int size = queue.size();

            while (size-- > 0) {

                String word = queue.poll();

                if (word.equals(endWord))
                    return level;

                char[] chars = word.toCharArray();

                for (int i = 0; i < chars.length; i++) {

                    char old = chars[i];

                    for (char c = 'a'; c <= 'z'; c++) {

                        chars[i] = c;

                        String next = new String(chars);

                        if (!visited.contains(next)
                                && dict.contains(next)) {

                            visited.add(next);
                            queue.offer(next);
                        }
                    }

                    chars[i] = old;
                }
            }

            level++;
        }

        return 0;
    }
}
```

---

## Complexity

Let

```
N = number of words

L = word length
```

|Metric|Complexity|
|------|----------|
|Time|O(N × L × 26)|
|Space|O(N)|

---

## Interview Tricks

Never compare every pair of words.

```
O(N²)
```

is too slow.

Generate neighbors on demand.

---

## Common Edge Cases

- End word absent.
- Begin equals end.
- Multiple shortest paths.
- Duplicate words.

---

## Why This Is LLM-Proof

Interviewers frequently extend the problem in ways that require deeper reasoning:

- Return **all** shortest transformation sequences (Word Ladder II).
- Allow weighted transformations.
- Support wildcard characters or variable-length words.
- Process a continuously changing dictionary.

These changes require redesigning the graph representation or traversal strategy instead of applying a memorized BFS template.

---

## Follow-ups

- Word Ladder II.
- Bidirectional BFS.
- A* Search.
- Dynamic dictionary updates.

---

# Graph Interview Cheat Sheet

|Problem Type|Primary Algorithm|
|-------------|-----------------|
|Connected Components|DFS / BFS / Union-Find|
|Flood Fill|DFS / BFS|
|Shortest Path (Unweighted)|BFS|
|Shortest Path (Weighted)|Dijkstra|
|Negative Edge Weights|Bellman-Ford|
|All-Pairs Shortest Path|Floyd-Warshall|
|Minimum Spanning Tree|Prim / Kruskal|
|Cycle Detection (Undirected)|Union-Find / DFS|
|Cycle Detection (Directed)|DFS / Topological Sort|
|Topological Ordering|Kahn's Algorithm / DFS|
|Bipartite Graph|BFS / DFS Coloring|
|Strongly Connected Components|Kosaraju / Tarjan|
|Bridge Detection|Tarjan|
|Articulation Points|Tarjan|

---

# Complexity Summary

|Algorithm|Time Complexity|Space Complexity|
|-----------|---------------|----------------|
|DFS|O(V+E)|O(V)|
|BFS|O(V+E)|O(V)|
|Topological Sort|O(V+E)|O(V)|
|Union-Find|O(E α(V))|O(V)|
|Dijkstra|O((V+E) log V)|O(V+E)|
|Bellman-Ford|O(VE)|O(V)|
|Floyd-Warshall|O(V³)|O(V²)|
|Prim|O(E log V)|O(V)|
|Kruskal|O(E log E)|O(V)|

---

# Common Interview Mistakes

1. Using BFS on weighted graphs.
2. Forgetting to mark nodes as visited before recursion/queue insertion.
3. Mixing directed and undirected graph logic.
4. Building an explicit graph when implicit traversal is sufficient (e.g., grids).
5. Missing disconnected components by starting DFS/BFS from only one node.
6. Forgetting stale-entry checks in Dijkstra's priority queue.
7. Using shortest-path algorithms for MST problems.
8. Ignoring recursion depth limits on very large graphs.

---

# 7-Day Graph Revision Plan

|Day|Topics|
|---|------|
|1|DFS, BFS, Number of Islands, Clone Graph|
|2|Multi-Source BFS, Connected Components, Graph Valid Tree|
|3|Topological Sort, Course Schedule I & II|
|4|Union-Find, Redundant Connection, Bipartite Graph|
|5|Dijkstra, Network Delay Time, Cheapest Flights|
|6|MST (Prim/Kruskal), Min Cost to Connect Points, Path With Minimum Effort|
|7|Word Ladder, Pacific Atlantic, Mixed Graph Interview Practice|

---

# Final Interview Strategy

1. Identify whether the graph is **directed or undirected**.
2. Determine whether the graph is **weighted or unweighted**.
3. Recognize whether the task is:
   - Traversal
   - Connectivity
   - Cycle detection
   - Ordering
   - Shortest path
   - Minimum spanning tree
4. Select the matching algorithm before writing code.
5. Clearly explain why alternative algorithms are unsuitable.

Mastering these 15 problems provides coverage of the graph patterns most frequently encountered in FAANG-style interviews and establishes a strong foundation for solving unseen graph variants.



