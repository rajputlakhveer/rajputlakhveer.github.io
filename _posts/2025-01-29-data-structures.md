---
layout: home
title: "Data Structures"
date: 2025-01-29
categories: "DSA"
tags: [Data Structures, DSA, Algorithms, Tree, Graph]
image: 'https://github.com/user-attachments/assets/4905de18-116c-4246-8ef3-87217b477d16'
---

# 📚 The Ultimate Guide to Data Structures with Examples 🚀

Data structures are the backbone of programming and software development. They help in organizing and managing data efficiently, enabling faster and optimized performance. In this guide, we will explore different data structures, their types, and real-world examples to understand their practical applications. 

![What-is-Data-Structure](https://github.com/user-attachments/assets/4905de18-116c-4246-8ef3-87217b477d16)

---

## 1️⃣ Arrays 📊
### What is an Array?
An array is a collection of elements stored in contiguous memory locations. It allows storing multiple values of the same type under a single variable name.

### Types of Arrays:
- **One-dimensional Array (1D)**: A linear list of elements stored in a single row or column.
- **Multi-dimensional Array (2D, 3D, etc.)**: Arrays within arrays, where elements are arranged in a grid-like structure, commonly used for matrices.
- **Jagged Array**: An array where sub-arrays have different lengths, useful in situations where each dataset varies in size.

### Example:
```ruby
# Ruby Example: One-dimensional Array
arr = [10, 20, 30, 40, 50]
puts arr[2]  # Output: 30
```

**Use Case:** Arrays are used for storing and accessing data sequentially, such as in databases and search operations.

---

## 2️⃣ Linked Lists 🔗
### What is a Linked List?
A linked list is a linear data structure where each element (node) points to the next node in the sequence. Unlike arrays, linked lists do not have contiguous memory allocation.

### Types of Linked Lists:
- **Singly Linked List**: Each node has a reference to the next node only, making it efficient for forward traversal.
- **Doubly Linked List**: Each node has references to both the previous and the next node, allowing bidirectional traversal.
- **Circular Linked List**: The last node points back to the first node, forming a circular structure, often used in buffer management.

### Example:
```ruby
class Node
  attr_accessor :data, :next
  def initialize(data)
    @data = data
    @next = nil
  end
end

head = Node.new(1)
head.next = Node.new(2)
head.next.next = Node.new(3)
puts head.next.data  # Output: 2
```

**Use Case:** Linked lists are used in dynamic memory allocation, implementing stacks and queues.

---

## 3️⃣ Stacks 📥📤
### What is a Stack?
A stack follows the **LIFO (Last In, First Out)** principle, where the last inserted element is accessed first.

### Types of Stacks:
- **Static Stack**: Implemented using arrays with a fixed size.
- **Dynamic Stack**: Implemented using linked lists to allow flexible memory allocation.

### Operations:
- **Push** (Insert element)
- **Pop** (Remove element)
- **Peek** (View top element)

### Example:
```ruby
stack = []
stack.push(10)
stack.push(20)
puts stack.pop  # Output: 20
```

**Use Case:** Stacks are used in function calls, undo operations, and expression evaluation.

---

## 4️⃣ Queues 🚦
### What is a Queue?
A queue follows the **FIFO (First In, First Out)** principle, where the first inserted element is accessed first.

### Types of Queues:
- **Simple Queue**: Elements are added at the rear and removed from the front.
- **Circular Queue**: The last position connects back to the first position, avoiding memory wastage.
- **Priority Queue**: Elements are dequeued based on priority rather than order.
- **Deque (Double-Ended Queue)**: Elements can be inserted and removed from both ends.

### Example:
```ruby
queue = []
queue.push(1)
queue.push(2)
puts queue.shift  # Output: 1
```

**Use Case:** Used in scheduling algorithms, task processing, and network buffers.

---

## 5️⃣ Hash Tables 🔑
### What is a Hash Table?
A hash table (or dictionary) is a data structure that stores key-value pairs, allowing fast data retrieval.

### Types of Hash Tables:
- **Chaining Hash Table**: Uses linked lists to handle collisions.
- **Open Addressing Hash Table**: Uses probing to resolve collisions within the table.

### Example:
```ruby
hash = {"name" => "Alice", "age" => 25}
puts hash["name"]  # Output: Alice
```

**Use Case:** Used in caching, database indexing, and fast lookups.

---

## 6️⃣ Trees 🌳
### What is a Tree?
A tree is a non-linear hierarchical data structure consisting of nodes. The topmost node is called the **root**, and each node can have child nodes.

### Types of Trees:
- **Binary Tree**: Each node has at most two children.
- **Binary Search Tree (BST)**: A binary tree with ordered node placement for quick searching.
- **AVL Tree**: A self-balancing binary tree.
- **Heap**: A specialized tree used for priority-based operations.
- **Trie**: Used for string searching and prefix-matching.

### Example:
```ruby
class TreeNode
  attr_accessor :value, :left, :right
  def initialize(value)
    @value = value
    @left = nil
    @right = nil
  end
end
root = TreeNode.new(10)
root.left = TreeNode.new(5)
root.right = TreeNode.new(15)
puts root.left.value  # Output: 5
```

**Use Case:** Used in file systems, databases, and AI algorithms.

---

## 7️⃣ Graphs 🖇️
### What is a Graph?
A graph is a data structure consisting of **nodes (vertices)** connected by **edges**.

### Types of Graphs:
- **Directed Graph**: Edges have a direction.
- **Undirected Graph**: Edges do not have a direction.
- **Weighted Graph**: Edges have weights, representing costs.
- **Unweighted Graph**: Edges do not have weights.

### Example:
```ruby
graph = {
  "A" => ["B", "C"],
  "B" => ["A", "D", "E"],
  "C" => ["A", "F"],
  "D" => ["B"],
  "E" => ["B", "F"],
  "F" => ["C", "E"]
}
puts graph["A"]  # Output: ["B", "C"]
```

**Use Case:** Used in social networks, recommendation systems, and pathfinding algorithms.

---

## Conclusion 🎯
Data structures are fundamental to efficient programming. Choosing the right one depends on the use case and required operations. Whether you're building a high-performance application or working with large datasets, mastering these structures will boost your coding efficiency. 🚀

Did you find this guide helpful? Let me know in the comments! 😊

