# Graph Algorithms in Python

## 1. Number of Islands (DFS/BFS)

### Question

You are given a 2D grid map of `'1'`s (land) and `'0'`s (water).  
Return the number of islands.

An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically.

#### Example

```python
Input:
grid = [
    ["1","1","0","0","0"],
    ["1","1","0","0","0"],
    ["0","0","1","0","0"],
    ["0","0","0","1","1"]
]

Output:
3
```

---

### Solution (DFS)

```python
class Solution:
    def numIslands(self, grid):
        if not grid:
            return 0

        rows, cols = len(grid), len(grid[0])
        islands = 0

        def dfs(r, c):
            if (
                r < 0 or c < 0 or
                r >= rows or c >= cols or
                grid[r][c] == "0"
            ):
                return

            grid[r][c] = "0"

            dfs(r + 1, c)
            dfs(r - 1, c)
            dfs(r, c + 1)
            dfs(r, c - 1)

        for r in range(rows):
            for c in range(cols):
                if grid[r][c] == "1":
                    islands += 1
                    dfs(r, c)

        return islands
```

---

### Solution (BFS)

```python
from collections import deque

class Solution:
    def numIslands(self, grid):
        if not grid:
            return 0

        rows, cols = len(grid), len(grid[0])
        islands = 0

        directions = [(1,0), (-1,0), (0,1), (0,-1)]

        for r in range(rows):
            for c in range(cols):
                if grid[r][c] == "1":
                    islands += 1

                    queue = deque([(r, c)])
                    grid[r][c] = "0"

                    while queue:
                        row, col = queue.popleft()

                        for dr, dc in directions:
                            nr, nc = row + dr, col + dc

                            if (
                                0 <= nr < rows and
                                0 <= nc < cols and
                                grid[nr][nc] == "1"
                            ):
                                grid[nr][nc] = "0"
                                queue.append((nr, nc))

        return islands
```

---

## 2. Clone Graph

### Question

Given a reference of a node in a connected undirected graph,  
return a deep copy (clone) of the graph.

Each node contains:
- `val`
- `neighbors`

---

### Solution (DFS)

```python
class Node:
    def __init__(self, val=0, neighbors=None):
        self.val = val
        self.neighbors = neighbors if neighbors else []

class Solution:
    def cloneGraph(self, node):
        if not node:
            return None

        old_to_new = {}

        def dfs(node):
            if node in old_to_new:
                return old_to_new[node]

            copy = Node(node.val)
            old_to_new[node] = copy

            for neighbor in node.neighbors:
                copy.neighbors.append(dfs(neighbor))

            return copy

        return dfs(node)
```

---

### Time Complexity

```text
O(V + E)
```

Where:
- `V` = number of vertices
- `E` = number of edges

---

## 3. Course Schedule (Topological Sort)

### Question

There are `numCourses` courses labeled from `0` to `numCourses - 1`.

You are given prerequisites where:

```python
[a, b]
```

means you must take course `b` before course `a`.

Return `True` if you can finish all courses, otherwise return `False`.

---

### Solution (Kahn’s Algorithm / BFS Topological Sort)

```python
from collections import defaultdict, deque

class Solution:
    def canFinish(self, numCourses, prerequisites):
        graph = defaultdict(list)
        indegree = [0] * numCourses

        for course, prereq in prerequisites:
            graph[prereq].append(course)
            indegree[course] += 1

        queue = deque()

        for i in range(numCourses):
            if indegree[i] == 0:
                queue.append(i)

        completed = 0

        while queue:
            node = queue.popleft()
            completed += 1

            for neighbor in graph[node]:
                indegree[neighbor] -= 1

                if indegree[neighbor] == 0:
                    queue.append(neighbor)

        return completed == numCourses
```

---

### Time Complexity

```text
O(V + E)
```

---

## 4. Pacific Atlantic Water Flow

### Question

You are given an `m x n` matrix of heights.

Water can flow from a cell to neighboring cells if the neighboring cell height is less than or equal to the current cell.

Return all coordinates where water can flow to both:
- Pacific Ocean
- Atlantic Ocean

---

### Solution (DFS)

```python
class Solution:
    def pacificAtlantic(self, heights):
        if not heights:
            return []

        rows, cols = len(heights), len(heights[0])

        pacific = set()
        atlantic = set()

        directions = [(1,0), (-1,0), (0,1), (0,-1)]

        def dfs(r, c, visited, prev_height):
            if (
                r < 0 or c < 0 or
                r >= rows or c >= cols or
                (r, c) in visited or
                heights[r][c] < prev_height
            ):
                return

            visited.add((r, c))

            for dr, dc in directions:
                dfs(r + dr, c + dc, visited, heights[r][c])

        for c in range(cols):
            dfs(0, c, pacific, heights[0][c])
            dfs(rows - 1, c, atlantic, heights[rows - 1][c])

        for r in range(rows):
            dfs(r, 0, pacific, heights[r][0])
            dfs(r, cols - 1, atlantic, heights[r][cols - 1])

        result = []

        for r in range(rows):
            for c in range(cols):
                if (r, c) in pacific and (r, c) in atlantic:
                    result.append([r, c])

        return result
```

---

### Time Complexity

```text
O(m * n)
```

---

## 5. Dijkstra’s Algorithm (Shortest Path)

### Question

Given a weighted graph and a starting node,  
find the shortest distance from the start node to all other nodes.

---

### Solution

```python
import heapq

def dijkstra(graph, start):
    distances = {node: float("inf") for node in graph}
    distances[start] = 0

    priority_queue = [(0, start)]

    while priority_queue:
        current_distance, current_node = heapq.heappop(priority_queue)

        if current_distance > distances[current_node]:
            continue

        for neighbor, weight in graph[current_node]:
            distance = current_distance + weight

            if distance < distances[neighbor]:
                distances[neighbor] = distance

                heapq.heappush(
                    priority_queue,
                    (distance, neighbor)
                )

    return distances
```

---

### Example

```python
graph = {
    "A": [("B", 1), ("C", 4)],
    "B": [("C", 2), ("D", 5)],
    "C": [("D", 1)],
    "D": []
}

print(dijkstra(graph, "A"))
```

### Output

```python
{
    'A': 0,
    'B': 1,
    'C': 3,
    'D': 4
}
```

---

### Time Complexity

```text
O((V + E) log V)
```

Where:
- `V` = number of vertices
- `E` = number of edges

---

# Summary

| Algorithm | Main Technique |
|---|---|
| Number of Islands | DFS / BFS |
| Clone Graph | DFS + HashMap |
| Course Schedule | Topological Sort |
| Pacific Atlantic Water Flow | DFS |
| Dijkstra’s Algorithm | Priority Queue / Heap |
