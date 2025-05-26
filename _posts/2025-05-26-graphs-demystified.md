---
layout: home
title: "Graphs Demystified"
date: 2025-05-26
categories: "DSA"
tags: [Data Structure, Algorithms, DSA, Graphs, Problems]
image: 'https://github.com/user-attachments/assets/8ae8bfe1-3bf6-45ac-854b-16c9c51aeb56'
---

# 📊 **Graphs Demystified: A Comprehensive Guide with Examples & Problems!** 🚀  

Graphs are one of the most versatile and powerful data structures in computer science. They model relationships between objects, making them essential for solving problems in social networks, maps, recommendation systems, and more. In this blog, we’ll dive deep into **graphs**, their types, representations, and real-world problem-solving with code examples! 🎯  

![Types-of-graphs](https://github.com/user-attachments/assets/8ae8bfe1-3bf6-45ac-854b-16c9c51aeb56)

---

## **🔹 What is a Graph?**  
A **graph** is a non-linear data structure consisting of:  
- **Vertices (Nodes)**: Represent entities (e.g., users in a social network).  
- **Edges**: Represent relationships between nodes (e.g., friendships).  

Formally, a graph **G = (V, E)**, where:  
- **V** = Set of vertices  
- **E** = Set of edges  

---

## **🔹 Types of Graphs**  

### **1️⃣ Undirected Graph**  
- Edges have **no direction**.  
- If (A, B) is an edge, then A is connected to B and vice versa.  
- **Example**: Facebook friendships (if A is friends with B, then B is also friends with A).  

### **2️⃣ Directed Graph (Digraph)**  
- Edges have **direction**.  
- (A → B) means A points to B, but not necessarily the reverse.  
- **Example**: Twitter followings (A follows B doesn’t mean B follows A).  

### **3️⃣ Weighted Graph**  
- Edges have **weights (costs)** associated with them.  
- **Example**: Road networks (weights represent distances).  

### **4️⃣ Unweighted Graph**  
- Edges have **no weights**.  
- **Example**: Social network connections.  

### **5️⃣ Cyclic Graph**  
- Contains **at least one cycle** (a path that starts and ends at the same node).  
- **Example**: Circular dependencies in tasks.  

### **6️⃣ Acyclic Graph**  
- **No cycles** present.  
- **Example**: Family tree (no one can be their own ancestor).  

### **7️⃣ Connected Graph**  
- **All nodes are reachable** from any other node.  
- **Example**: Fully linked computer network.  

### **8️⃣ Disconnected Graph**  
- Some nodes are **unreachable** from others.  
- **Example**: Isolated computers in a network.  

### **9️⃣ Tree**  
- A **connected acyclic undirected graph**.  
- **Example**: File system hierarchy.  

### **🔟 Bipartite Graph**  
- Vertices can be divided into **two disjoint sets** where no two graph vertices in the same set are adjacent.  
- **Example**: Job applicants (set A) and job positions (set B).  

---

## **🔹 Graph Representations**  
### **1️⃣ Adjacency Matrix**  
- A **2D array** where `matrix[i][j] = 1` if there’s an edge between `i` and `j`, else `0`.  
- **Pros**:  
  - Fast edge lookup (O(1)).  
- **Cons**:  
  - Consumes more space (O(V²)).  

#### **Example Code (Undirected Graph):**  
```python
# Adjacency Matrix for an undirected graph
V = 4
graph = [[0] * V for _ in range(V)]

def add_edge(u, v):
    graph[u][v] = 1
    graph[v][u] = 1  # Since it's undirected

add_edge(0, 1)
add_edge(1, 2)
add_edge(2, 3)
add_edge(3, 0)

print(graph)
```
**Output:**  
```
[
 [0, 1, 0, 1],
 [1, 0, 1, 0],
 [0, 1, 0, 1],
 [1, 0, 1, 0]
]
```

### **2️⃣ Adjacency List**  
- A **list of lists** where each node stores its neighbors.  
- **Pros**:  
  - Space-efficient (O(V + E)).  
- **Cons**:  
  - Slower edge lookup (O(V) in worst case).  

#### **Example Code (Directed Graph):**  
```python
# Adjacency List for a directed graph
graph = {
    0: [1, 2],
    1: [2],
    2: [0, 3],
    3: [3]
}

print(graph)
```
**Output:**  
```
{0: [1, 2], 1: [2], 2: [0, 3], 3: [3]}
```

---

## **🔹 Graph Traversals**  
### **1️⃣ Breadth-First Search (BFS)**  
- Explores **level by level** (uses a **queue**).  
- **Use Case**: Shortest path in an unweighted graph.  

#### **Example Code:**  
```python
from collections import deque

def bfs(graph, start):
    visited = set()
    queue = deque([start])
    visited.add(start)
    
    while queue:
        node = queue.popleft()
        print(node, end=" ")
        
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)

# Example usage
graph = {0: [1, 2], 1: [2], 2: [0, 3], 3: [3]}
bfs(graph, 2)
```
**Output:**  
```
2 0 3 1
```

### **2️⃣ Depth-First Search (DFS)**  
- Explores as **deep as possible** (uses a **stack**).  
- **Use Case**: Topological sorting, cycle detection.  

#### **Example Code (Recursive):**  
```python
def dfs(graph, node, visited=None):
    if visited is None:
        visited = set()
    visited.add(node)
    print(node, end=" ")
    
    for neighbor in graph[node]:
        if neighbor not in visited:
            dfs(graph, neighbor, visited)

# Example usage
graph = {0: [1, 2], 1: [2], 2: [0, 3], 3: [3]}
dfs(graph, 2)
```
**Output:**  
```
2 0 1 3
```

---

## **🔹 Common Graph Problems & Solutions**  

### **1️⃣ Shortest Path (Unweighted Graph) – BFS**  
**Problem:** Find the shortest path from **A to B** in an unweighted graph.  

#### **Solution:**  
```python
from collections import deque

def shortest_path_bfs(graph, start, end):
    queue = deque([(start, [start])])
    visited = set([start])
    
    while queue:
        node, path = queue.popleft()
        if node == end:
            return path
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append((neighbor, path + [neighbor]))
    return None

# Example usage
graph = {0: [1, 3], 1: [0, 2], 2: [1, 3], 3: [0, 2]}
print(shortest_path_bfs(graph, 0, 2))
```
**Output:**  
```
[0, 1, 2]  # Shortest path from 0 → 2
```

### **2️⃣ Detect Cycle in a Directed Graph – DFS**  
**Problem:** Check if a **directed graph** contains a cycle.  

#### **Solution:**  
```python
def is_cyclic_directed(graph):
    visited = set()
    recursion_stack = set()
    
    def dfs(node):
        visited.add(node)
        recursion_stack.add(node)
        for neighbor in graph.get(node, []):
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

# Example usage
graph = {0: [1], 1: [2], 2: [0]}
print(is_cyclic_directed(graph))  # Output: True
```

### **3️⃣ Topological Sorting (DAG) – DFS**  
**Problem:** Linear ordering of vertices such that for every directed edge **u → v**, **u comes before v**.  

#### **Solution:**  
```python
def topological_sort(graph):
    visited = set()
    stack = []
    
    def dfs(node):
        visited.add(node)
        for neighbor in graph.get(node, []):
            if neighbor not in visited:
                dfs(neighbor)
        stack.append(node)
    
    for node in graph:
        if node not in visited:
            dfs(node)
    return stack[::-1]

# Example usage
graph = {5: [2, 0], 4: [0, 1], 2: [3], 3: [1]}
print(topological_sort(graph))
```
**Output:**  
```
[5, 4, 2, 3, 1, 0]  # Valid topological order
```

---

## **🔹 Conclusion**  
Graphs are **fundamental** in solving real-world problems, from social networks to GPS navigation. Understanding their **types, traversals, and problem-solving techniques** is crucial for any programmer. 🚀  

### **💡 Key Takeaways:**  
✔ Graphs model **relationships** between entities.  
✔ **BFS & DFS** are essential traversal techniques.  
✔ **Adjacency List vs Matrix** depends on use case.  
✔ **Cycle detection, shortest path, and topological sorting** are classic problems.  

Now, go solve some graph problems on [LeetCode](https://leetcode.com/tag/graph/) or [Codeforces](https://codeforces.com/)! Happy coding! 😃💻  

---

**📢 Did you find this helpful? Drop a comment below! Let’s discuss more graph algorithms!** 🚀👇
