# Graphs in Python

## Table of Contents

1. Introduction to Graphs
2. Graph Terminology
3. Types of Graphs
4. Graph Representations in Python
5. Building Graphs from Scratch
6. Graph Traversal Algorithms
7. Shortest Path Algorithms
8. Minimum Spanning Trees
9. Topological Sorting
10. Strongly Connected Components
11. Cycle Detection
12. Graph Problems and Patterns
13. Weighted Graphs
14. Disjoint Set Union (Union-Find)
15. Advanced Graph Techniques
16. Graph Libraries in Python
17. Performance Optimization
18. Real-World Applications
19. Common Interview Problems
20. Practice Exercises
21. Conclusion

---

# 1. Introduction to Graphs

Graphs are among the most powerful and flexible data structures in computer science.

A graph consists of:

- **Vertices (Nodes)** → entities
- **Edges** → relationships between entities

Examples:

| Application | Nodes | Edges |
|---|---|---|
| Social network | People | Friendships |
| GPS navigation | Cities | Roads |
| Internet | Computers | Connections |
| Dependency management | Tasks | Dependencies |
| Recommendation systems | Users/Products | Interactions |

---

# 2. Graph Terminology

## Basic Terms

| Term | Meaning |
|---|---|
| Vertex (Node) | A graph element |
| Edge | Connection between nodes |
| Degree | Number of connected edges |
| Path | Sequence of vertices |
| Cycle | Path returning to origin |
| Connected Graph | All nodes reachable |
| Component | Independent subgraph |
| Weight | Cost/value on an edge |

---

## Directed vs Undirected

### Undirected Graph

```text
A --- B
|     |
C --- D
```

Edges work both ways.

---

### Directed Graph

```text
A → B → C
↑       ↓
└───────┘
```

Edges have direction.

---

# 3. Types of Graphs

## Unweighted Graph

All edges equal.

```python
graph = {
    'A': ['B', 'C'],
    'B': ['A', 'D'],
    'C': ['A'],
    'D': ['B']
}
```

---

## Weighted Graph

Edges contain costs.

```python
graph = {
    'A': [('B', 5), ('C', 2)],
    'B': [('D', 1)],
    'C': [('D', 7)],
    'D': []
}
```

---

## Cyclic vs Acyclic

### Cyclic

```text
A → B → C → A
```

### Acyclic

```text
A → B → C
```

---

## DAG (Directed Acyclic Graph)

Important for:

- task scheduling
- dependency resolution
- compilers
- CI/CD pipelines

---

# 4. Graph Representations in Python

There are multiple ways to represent graphs.

---

# 4.1 Adjacency List

Most common.

## Example

```python
graph = {
    0: [1, 2],
    1: [0, 3],
    2: [0],
    3: [1]
}
```

## Complexity

| Operation | Complexity |
|---|---|
| Add Edge | O(1) |
| Remove Edge | O(V) |
| Check Edge | O(V) |
| Space | O(V + E) |

---

# 4.2 Adjacency Matrix

```python
matrix = [
    [0, 1, 1, 0],
    [1, 0, 0, 1],
    [1, 0, 0, 0],
    [0, 1, 0, 0]
]
```

## Complexity

| Operation | Complexity |
|---|---|
| Add Edge | O(1) |
| Check Edge | O(1) |
| Space | O(V²) |

Useful for dense graphs.

---

# 4.3 Edge List

```python
edges = [
    (0, 1),
    (0, 2),
    (1, 3)
]
```

Useful for:

- Kruskal's algorithm
- graph serialization
- network transfers

---

# 5. Building Graphs from Scratch

## Object-Oriented Graph

```python
class Graph:
    def __init__(self):
        self.graph = {}

    def add_vertex(self, vertex):
        if vertex not in self.graph:
            self.graph[vertex] = []

    def add_edge(self, u, v):
        self.add_vertex(u)
        self.add_vertex(v)

        self.graph[u].append(v)
        self.graph[v].append(u)

    def display(self):
        for vertex, neighbors in self.graph.items():
            print(vertex, '->', neighbors)


# Usage

g = Graph()
g.add_edge('A', 'B')
g.add_edge('A', 'C')
g.add_edge('B', 'D')

g.display()
```

---

# 6. Graph Traversal Algorithms

Traversal is fundamental.

Two primary algorithms:

1. Breadth-First Search (BFS)
2. Depth-First Search (DFS)

---

# 6.1 Breadth-First Search (BFS)

BFS explores level-by-level.

## Characteristics

- Uses a queue
- Finds shortest path in unweighted graphs
- Good for nearest-neighbor problems

---

## BFS Algorithm

```python
from collections import deque


def bfs(graph, start):
    visited = set()
    queue = deque([start])

    visited.add(start)

    while queue:
        node = queue.popleft()
        print(node)

        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
```

---

## Complexity

| Complexity | Value |
|---|---|
| Time | O(V + E) |
| Space | O(V) |

---

## BFS Shortest Path

```python
from collections import deque


def shortest_path(graph, start, end):
    queue = deque([(start, [start])])
    visited = set()

    while queue:
        node, path = queue.popleft()

        if node == end:
            return path

        visited.add(node)

        for neighbor in graph[node]:
            if neighbor not in visited:
                queue.append((neighbor, path + [neighbor]))
```

---

# 6.2 Depth-First Search (DFS)

DFS explores deeply before backtracking.

---

## Recursive DFS

```python

def dfs(graph, node, visited=None):
    if visited is None:
        visited = set()

    visited.add(node)
    print(node)

    for neighbor in graph[node]:
        if neighbor not in visited:
            dfs(graph, neighbor, visited)
```

---

## Iterative DFS

```python

def dfs_iterative(graph, start):
    stack = [start]
    visited = set()

    while stack:
        node = stack.pop()

        if node not in visited:
            visited.add(node)
            print(node)

            for neighbor in reversed(graph[node]):
                stack.append(neighbor)
```

---

## Complexity

| Complexity | Value |
|---|---|
| Time | O(V + E) |
| Space | O(V) |

---

# 7. Shortest Path Algorithms

---

# 7.1 Dijkstra's Algorithm

Finds shortest path in weighted graphs with non-negative weights.

---

## Key Concepts

- Greedy algorithm
- Uses priority queue
- Cannot handle negative weights

---

## Implementation

```python
import heapq


def dijkstra(graph, start):
    distances = {
        node: float('inf')
        for node in graph
    }

    distances[start] = 0

    pq = [(0, start)]

    while pq:
        current_distance, current_node = heapq.heappop(pq)

        if current_distance > distances[current_node]:
            continue

        for neighbor, weight in graph[current_node]:
            distance = current_distance + weight

            if distance < distances[neighbor]:
                distances[neighbor] = distance
                heapq.heappush(
                    pq,
                    (distance, neighbor)
                )

    return distances
```

---

## Complexity

| Complexity | Value |
|---|---|
| Time | O((V + E) log V) |
| Space | O(V) |

---

# 7.2 Bellman-Ford Algorithm

Handles negative weights.

---

## Features

- Detects negative cycles
- Slower than Dijkstra

---

## Implementation

```python

def bellman_ford(vertices, edges, source):
    distance = [float('inf')] * vertices
    distance[source] = 0

    for _ in range(vertices - 1):
        for u, v, w in edges:
            if distance[u] != float('inf'):
                if distance[u] + w < distance[v]:
                    distance[v] = distance[u] + w

    # Negative cycle detection
    for u, v, w in edges:
        if distance[u] + w < distance[v]:
            raise ValueError('Negative cycle detected')

    return distance
```

---

# 7.3 Floyd-Warshall Algorithm

All-pairs shortest path.

---

## Complexity

| Complexity | Value |
|---|---|
| Time | O(V³) |
| Space | O(V²) |

---

## Implementation

```python

def floyd_warshall(graph):
    n = len(graph)

    dist = [row[:] for row in graph]

    for k in range(n):
        for i in range(n):
            for j in range(n):
                dist[i][j] = min(
                    dist[i][j],
                    dist[i][k] + dist[k][j]
                )

    return dist
```

---

# 8. Minimum Spanning Trees

A spanning tree connects all vertices with minimum total edge weight.

Two major algorithms:

1. Prim's Algorithm
2. Kruskal's Algorithm

---

# 8.1 Prim's Algorithm

Greedy algorithm using a priority queue.

---

## Implementation

```python
import heapq


def prim(graph, start):
    visited = set()
    min_heap = [(0, start)]

    total_cost = 0

    while min_heap:
        cost, node = heapq.heappop(min_heap)

        if node in visited:
            continue

        visited.add(node)
        total_cost += cost

        for neighbor, weight in graph[node]:
            if neighbor not in visited:
                heapq.heappush(
                    min_heap,
                    (weight, neighbor)
                )

    return total_cost
```

---

# 8.2 Kruskal's Algorithm

Sorts edges by weight.

Uses Union-Find.

---

## Implementation

```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])

        return self.parent[x]

    def union(self, x, y):
        root_x = self.find(x)
        root_y = self.find(y)

        if root_x == root_y:
            return False

        if self.rank[root_x] < self.rank[root_y]:
            self.parent[root_x] = root_y
        elif self.rank[root_x] > self.rank[root_y]:
            self.parent[root_y] = root_x
        else:
            self.parent[root_y] = root_x
            self.rank[root_x] += 1

        return True


def kruskal(n, edges):
    uf = UnionFind(n)

    edges.sort(key=lambda x: x[2])

    mst_cost = 0

    for u, v, weight in edges:
        if uf.union(u, v):
            mst_cost += weight

    return mst_cost
```

---

# 9. Topological Sorting

Used on Directed Acyclic Graphs.

Applications:

- task scheduling
- package managers
- build systems
- prerequisite systems

---

## Kahn's Algorithm

```python
from collections import deque


def topological_sort(graph):
    in_degree = {
        node: 0
        for node in graph
    }

    for node in graph:
        for neighbor in graph[node]:
            in_degree[neighbor] += 1

    queue = deque(
        [node for node in in_degree if in_degree[node] == 0]
    )

    order = []

    while queue:
        node = queue.popleft()
        order.append(node)

        for neighbor in graph[node]:
            in_degree[neighbor] -= 1

            if in_degree[neighbor] == 0:
                queue.append(neighbor)

    return order
```

---

# 10. Strongly Connected Components

A strongly connected component (SCC) is a subset where every node can reach every other node.

---

## Kosaraju's Algorithm

```python
from collections import defaultdict


def kosaraju(graph):
    visited = set()
    stack = []

    def dfs(node):
        visited.add(node)

        for neighbor in graph[node]:
            if neighbor not in visited:
                dfs(neighbor)

        stack.append(node)

    for node in graph:
        if node not in visited:
            dfs(node)

    reversed_graph = defaultdict(list)

    for node in graph:
        for neighbor in graph[node]:
            reversed_graph[neighbor].append(node)

    visited.clear()
    sccs = []

    def reverse_dfs(node, component):
        visited.add(node)
        component.append(node)

        for neighbor in reversed_graph[node]:
            if neighbor not in visited:
                reverse_dfs(neighbor, component)

    while stack:
        node = stack.pop()

        if node not in visited:
            component = []
            reverse_dfs(node, component)
            sccs.append(component)

    return sccs
```

---

# 11. Cycle Detection

---

# 11.1 Undirected Graph Cycle Detection

Using Union-Find.

```python
class DSU:
    def __init__(self, n):
        self.parent = list(range(n))

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])

        return self.parent[x]

    def union(self, x, y):
        px = self.find(x)
        py = self.find(y)

        if px == py:
            return True

        self.parent[px] = py
        return False
```

---

# 11.2 Directed Graph Cycle Detection

Using DFS.

```python

def has_cycle(graph):
    visited = set()
    recursion_stack = set()

    def dfs(node):
        visited.add(node)
        recursion_stack.add(node)

        for neighbor in graph[node]:
            if neighbor not in visited:
                if dfs(neighbor):
                    return True
            elif neighbor in recursion_stack:
                return True

        recursion_stack.remove(node)
        return False

    for node in graph:
        if node not in visited:
            if dfs(node):
                return True

    return False
```

---

# 12. Graph Problems and Patterns

---

# 12.1 Connected Components

```python

def connected_components(graph):
    visited = set()
    count = 0

    def dfs(node):
        visited.add(node)

        for neighbor in graph[node]:
            if neighbor not in visited:
                dfs(neighbor)

    for node in graph:
        if node not in visited:
            dfs(node)
            count += 1

    return count
```

---

# 12.2 Bipartite Graph

```python
from collections import deque


def is_bipartite(graph):
    color = {}

    for start in graph:
        if start not in color:
            queue = deque([start])
            color[start] = 0

            while queue:
                node = queue.popleft()

                for neighbor in graph[node]:
                    if neighbor not in color:
                        color[neighbor] = 1 - color[node]
                        queue.append(neighbor)
                    elif color[neighbor] == color[node]:
                        return False

    return True
```

---

# 12.3 Number of Islands

Classic matrix graph problem.

```python

def num_islands(grid):
    if not grid:
        return 0

    rows = len(grid)
    cols = len(grid[0])

    visited = set()

    def dfs(r, c):
        if (
            r < 0 or
            c < 0 or
            r >= rows or
            c >= cols or
            grid[r][c] == '0' or
            (r, c) in visited
        ):
            return

        visited.add((r, c))

        directions = [
            (1, 0),
            (-1, 0),
            (0, 1),
            (0, -1)
        ]

        for dr, dc in directions:
            dfs(r + dr, c + dc)

    islands = 0

    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == '1' and (r, c) not in visited:
                dfs(r, c)
                islands += 1

    return islands
```

---

# 13. Weighted Graphs

Weighted graphs store edge costs.

---

## Representation

```python
weighted_graph = {
    'A': [('B', 4), ('C', 2)],
    'B': [('D', 3)],
    'C': [('D', 1)],
    'D': []
}
```

---

## Real-World Uses

| Problem | Weight Meaning |
|---|---|
| GPS | Distance |
| Networking | Latency |
| Finance | Cost |
| Games | Movement cost |

---

# 14. Disjoint Set Union (Union-Find)

Powerful structure for connectivity.

---

## Optimizations

1. Path compression
2. Union by rank

---

## Efficient Implementation

```python
class UnionFind:
    def __init__(self, size):
        self.parent = list(range(size))
        self.rank = [0] * size

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])

        return self.parent[x]

    def union(self, x, y):
        root_x = self.find(x)
        root_y = self.find(y)

        if root_x == root_y:
            return

        if self.rank[root_x] < self.rank[root_y]:
            self.parent[root_x] = root_y
        elif self.rank[root_x] > self.rank[root_y]:
            self.parent[root_y] = root_x
        else:
            self.parent[root_y] = root_x
            self.rank[root_x] += 1
```

---

# 15. Advanced Graph Techniques

---

# 15.1 A* Search

Heuristic shortest-path algorithm.

Used in:

- robotics
- games
- navigation systems

---

## Skeleton

```python
import heapq


def a_star(start, goal, heuristic):
    pq = [(0, start)]

    while pq:
        _, node = heapq.heappop(pq)

        if node == goal:
            return
```

---

# 15.2 Tarjan's Algorithm

Finds strongly connected components efficiently.

Complexity:

```text
O(V + E)
```

---

# 15.3 Eulerian Path

Path visiting every edge exactly once.

Conditions:

- 0 or 2 odd-degree vertices

Applications:

- route optimization
- DNA sequencing

---

# 16. Graph Libraries in Python

---

# 16.1 NetworkX

Most popular Python graph library.

## Installation

```bash
pip install networkx
```

---

## Basic Example

```python
import networkx as nx

G = nx.Graph()

G.add_edge('A', 'B')
G.add_edge('B', 'C')

print(G.nodes())
print(G.edges())
```

---

## Shortest Path

```python
path = nx.shortest_path(G, 'A', 'C')
print(path)
```

---

# 16.2 igraph

Faster for large graphs.

---

# 16.3 graph-tool

Very high performance.

Written in C++.

---

# 17. Performance Optimization

---

# Use Appropriate Structures

| Scenario | Best Structure |
|---|---|
| Sparse graph | Adjacency list |
| Dense graph | Matrix |
| Fast edge lookup | Matrix |
| Memory efficiency | List |

---

# Optimize Traversals

- Use iterative DFS for large recursion depth
- Use deque instead of list queues
- Avoid repeated node visits
- Use heaps for weighted shortest paths

---

# Complexity Cheat Sheet

| Algorithm | Complexity |
|---|---|
| BFS | O(V + E) |
| DFS | O(V + E) |
| Dijkstra | O((V + E) log V) |
| Bellman-Ford | O(VE) |
| Floyd-Warshall | O(V³) |
| Kruskal | O(E log E) |
| Prim | O((V + E) log V) |
| Topological Sort | O(V + E) |

---

# 18. Real-World Applications

---

## Social Networks

- friend recommendations
- community detection
- influence analysis

---

## Navigation Systems

- shortest route
- traffic optimization
- logistics

---

## Compilers

- dependency graphs
- optimization passes

---

## Cybersecurity

- attack path analysis
- network topology

---

## AI and Machine Learning

- graph neural networks
- knowledge graphs
- recommendation systems

---

# 19. Common Interview Problems

---

## Easy

- BFS traversal
- DFS traversal
- number of islands
- clone graph

---

## Medium

- course schedule
- word ladder
- network delay time
- redundant connection

---

## Hard

- traveling salesman
- minimum cost to connect points
- alien dictionary
- critical connections

---

# 20. Practice Exercises

---

# Beginner

1. Implement BFS
2. Implement DFS
3. Detect cycles
4. Count connected components

---

# Intermediate

1. Implement Dijkstra
2. Topological sorting
3. Bipartite graph detection
4. Union-Find problems

---

# Advanced

1. Implement A*
2. Tarjan's SCC
3. Maximum flow algorithms
4. Graph coloring
5. Strong bridge detection

---

# 21. Conclusion

Graphs are among the most important and versatile data structures in software engineering and computer science.

To master graphs:

1. Understand graph representations
2. Practice BFS and DFS extensively
3. Learn shortest path algorithms deeply
4. Solve many traversal-based problems
5. Study weighted and directed graph techniques
6. Learn Union-Find and SCC algorithms
7. Build real-world projects using graph concepts

---

# Recommended Learning Path

## Stage 1

- Graph representations
- BFS
- DFS
- Connected components

---

## Stage 2

- Dijkstra
- Topological sorting
- Cycle detection
- Bipartite graphs

---

## Stage 3

- Minimum spanning trees
- SCC algorithms
- Bellman-Ford
- Floyd-Warshall

---

## Stage 4

- Advanced graph theory
- Network flow
- Graph optimization
- Competitive programming problems

---

# Final Advice

Graphs become intuitive only through repetition.

The best way to learn:

- draw graphs visually
- trace algorithms manually
- implement algorithms repeatedly
- solve real problems
- optimize your solutions

Mastering graphs will significantly improve your problem-solving skills in:

- backend engineering
- distributed systems
- AI
- networking
- cybersecurity
- competitive programming
- technical interviews
