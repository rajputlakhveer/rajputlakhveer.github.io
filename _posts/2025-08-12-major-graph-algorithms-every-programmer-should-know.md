---
layout: home
title: "Major Graph Algorithms Every Programmer Should Know"
date: 2025-08-12
categories: "DSA"
tags: [DSA, Graphs, Algorithms, Programming, Problem, Solution]
image: 'https://github.com/user-attachments/assets/0b3dabf6-0f40-4021-b3e5-3a836288a307'
---

# 🌐 Major Graph Algorithms Every Programmer Should Know 🚀

Graphs are everywhere — from **social networks** and **Google Maps** to **recommendation engines** and **network routing**. If you know how to work with them, you can solve some of the most complex real-world problems!

In this blog, we’ll explore **major graph algorithms** 📊, their **logic**, **examples**, **Python code**, and **best use cases** — plus a **problem-to-algorithm cheat sheet** at the end! 💡

![68747470733a2f2f6d69726f2e6d656469756d2e636f6d2f6d61782f323536302f312a64746d7375544d715276597a6b5543533235744c44412e6a706567](https://github.com/user-attachments/assets/0b3dabf6-0f40-4021-b3e5-3a836288a307)

---

## 1️⃣ Breadth-First Search (BFS) 🔍

**📖 Concept:**
BFS explores a graph **level by level**. Perfect for finding the shortest path in an unweighted graph.

**🛠 Example:**
Find the shortest distance between two people in a social network.

**💻 Python Code:**

```python
from collections import deque

def bfs(graph, start):
    visited = set()
    queue = deque([start])

    while queue:
        node = queue.popleft()
        if node not in visited:
            print(node, end=" ")
            visited.add(node)
            queue.extend(graph[node] - visited)

graph = {
    'A': {'B', 'C'},
    'B': {'A', 'D', 'E'},
    'C': {'A', 'F'},
    'D': {'B'},
    'E': {'B', 'F'},
    'F': {'C', 'E'}
}

bfs(graph, 'A')
```

**✅ Best Use Cases:**

* Shortest path in unweighted graphs
* Social network friend recommendations
* Crawling websites

---

## 2️⃣ Depth-First Search (DFS) 🕵️‍♂️

**📖 Concept:**
DFS dives deep into the graph before backtracking — like exploring a maze fully before trying another path.

**🛠 Example:**
Check if there is a route between two cities in a road map.

**💻 Python Code:**

```python
def dfs(graph, start, visited=None):
    if visited is None:
        visited = set()
    visited.add(start)
    print(start, end=" ")
    for neighbor in graph[start] - visited:
        dfs(graph, neighbor, visited)

graph = {
    'A': {'B', 'C'},
    'B': {'A', 'D', 'E'},
    'C': {'A', 'F'},
    'D': {'B'},
    'E': {'B', 'F'},
    'F': {'C', 'E'}
}

dfs(graph, 'A')
```

**✅ Best Use Cases:**

* Detecting cycles in graphs
* Topological sorting (with a modified version)
* Solving puzzles like mazes or Sudoku

---

## 3️⃣ Dijkstra’s Algorithm 🛣

**📖 Concept:**
Finds the **shortest path** in a graph with **non-negative weights**.

**🛠 Example:**
Finding the fastest driving route between cities considering road distances.

**💻 Python Code:**

```python
import heapq

def dijkstra(graph, start):
    distances = {node: float('inf') for node in graph}
    distances[start] = 0
    pq = [(0, start)]

    while pq:
        current_distance, current_node = heapq.heappop(pq)

        if current_distance > distances[current_node]:
            continue

        for neighbor, weight in graph[current_node].items():
            distance = current_distance + weight
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                heapq.heappush(pq, (distance, neighbor))
    return distances

graph = {
    'A': {'B': 1, 'C': 4},
    'B': {'A': 1, 'C': 2, 'D': 5},
    'C': {'A': 4, 'B': 2, 'D': 1},
    'D': {'B': 5, 'C': 1}
}

print(dijkstra(graph, 'A'))
```

**✅ Best Use Cases:**

* GPS navigation systems
* Network routing algorithms
* Game AI pathfinding

---

## 4️⃣ Bellman-Ford Algorithm ⚡

**📖 Concept:**
Finds shortest paths **even with negative edge weights** (unlike Dijkstra).

**🛠 Example:**
Calculating currency conversion rates when some rates might cause a loss.

**💻 Python Code:**

```python
def bellman_ford(graph, vertices, start):
    distance = {v: float('inf') for v in vertices}
    distance[start] = 0

    for _ in range(len(vertices) - 1):
        for u, v, w in graph:
            if distance[u] + w < distance[v]:
                distance[v] = distance[u] + w

    return distance

edges = [
    ('A', 'B', 4), ('A', 'C', 5),
    ('B', 'C', -3), ('C', 'D', 4)
]
vertices = {'A', 'B', 'C', 'D'}

print(bellman_ford(edges, vertices, 'A'))
```

**✅ Best Use Cases:**

* Financial arbitrage detection
* Negative cost network routing

---

## 5️⃣ Floyd-Warshall Algorithm 🌍

**📖 Concept:**
Finds **shortest paths between all pairs** of vertices.

**🛠 Example:**
Finding the shortest travel time between all cities in a country.

**💻 Python Code:**

```python
def floyd_warshall(graph):
    dist = dict(graph)
    for k in graph:
        for i in graph:
            for j in graph:
                dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
    return dist

graph = {
    'A': {'A': 0, 'B': 3, 'C': float('inf')},
    'B': {'A': 2, 'B': 0, 'C': 1},
    'C': {'A': float('inf'), 'B': float('inf'), 'C': 0}
}

print(floyd_warshall(graph))
```

**✅ Best Use Cases:**

* Precomputing shortest distances in dense graphs
* Flight connection planning

---

## 6️⃣ Kruskal’s Algorithm 🌳

**📖 Concept:**
Builds a **Minimum Spanning Tree (MST)** by adding the smallest edges without forming cycles.

**🛠 Example:**
Connecting cities with the least road-building cost.

**💻 Python Code:**

```python
def kruskal(vertices, edges):
    parent = {v: v for v in vertices}

    def find(v):
        while parent[v] != v:
            v = parent[v]
        return v

    mst = []
    edges.sort(key=lambda x: x[2])

    for u, v, w in edges:
        if find(u) != find(v):
            mst.append((u, v, w))
            parent[find(u)] = find(v)
    return mst

vertices = ['A', 'B', 'C', 'D']
edges = [
    ('A', 'B', 1),
    ('B', 'C', 4),
    ('A', 'C', 3),
    ('C', 'D', 2)
]

print(kruskal(vertices, edges))
```

**✅ Best Use Cases:**

* Designing efficient road networks
* Network cabling with minimum cost

---

## 7️⃣ Prim’s Algorithm 🌲

**📖 Concept:**
Another MST algorithm — grows the tree from a starting vertex, adding the smallest edge each time.

**🛠 Example:**
Laying out electrical wiring in the cheapest way possible.

**💻 Python Code:**

```python
import heapq

def prim(graph, start):
    visited = set([start])
    edges = [
        (cost, start, to)
        for to, cost in graph[start].items()
    ]
    heapq.heapify(edges)
    mst = []

    while edges:
        cost, frm, to = heapq.heappop(edges)
        if to not in visited:
            visited.add(to)
            mst.append((frm, to, cost))
            for to_next, cost_next in graph[to].items():
                if to_next not in visited:
                    heapq.heappush(edges, (cost_next, to, to_next))
    return mst

graph = {
    'A': {'B': 1, 'C': 3},
    'B': {'A': 1, 'C': 4, 'D': 2},
    'C': {'A': 3, 'B': 4, 'D': 5},
    'D': {'B': 2, 'C': 5}
}

print(prim(graph, 'A'))
```

**✅ Best Use Cases:**

* Network design
* Electrical grid layout

---

## 🗂 Problem → Algorithm Cheat Sheet 📌

| Problem Type                            | Best Algorithm                  |
| --------------------------------------- | ------------------------------- |
| Shortest Path (Unweighted)              | **BFS**                         |
| Shortest Path (Weighted, No Negative)   | **Dijkstra**                    |
| Shortest Path (Weighted, With Negative) | **Bellman-Ford**                |
| All-Pairs Shortest Path                 | **Floyd-Warshall**              |
| Minimum Spanning Tree                   | **Kruskal / Prim**              |
| Detect Cycle in Graph                   | **DFS**                         |
| Connected Components                    | **DFS / BFS**                   |
| Pathfinding in Games                    | **A\*** (extension of Dijkstra) |

---

### 🎯 Final Thoughts

Mastering these algorithms will make you a **graph wizard 🧙‍♂️** capable of tackling anything from **network optimization** to **social media analytics**.

Next time you face a graph problem, check the **cheat sheet** — it’s your quick key to choosing the right tool 🔑.
