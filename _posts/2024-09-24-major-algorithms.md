---
layout: home
title: "Major Algorithms"
date: 2024-09-24
categories: "Algorithms"
tags: [Algorithms, BDS, Sorting, Graph]
image: 'https://github.com/user-attachments/assets/ac8200ca-85bb-4d7a-a5ee-b8515c3c1395'
---

# Major Algorithms Every Programmer Should Know 🤖💻

As programmers, understanding and implementing key algorithms is a crucial step toward problem-solving and writing efficient code. Whether you're coding in Ruby, Python, or JavaScript, algorithms are foundational building blocks. Here's a list of **major algorithms every programmer should know** and an explanation of each, along with examples. Let's dive in! 🔍✨

---

## 1. **Sorting Algorithms** 🧮

Sorting is the process of arranging data in a particular order, such as ascending or descending. There are several sorting algorithms, and each has its own use case depending on the data size and complexity.

### Common Sorting Algorithms:
- **Bubble Sort**
- **Merge Sort**
- **Quick Sort**

![0_HGUFcgpnYdX2EABv](https://github.com/user-attachments/assets/1a01153c-64c7-4592-9aeb-d015fecdf656)

### Example: Merge Sort (Ruby)
Merge Sort is a **divide-and-conquer** algorithm that splits the array into two halves, recursively sorts each half, and then merges them back together.

```ruby
def merge_sort(arr)
  return arr if arr.length <= 1

  mid = arr.length / 2
  left = merge_sort(arr[0...mid])
  right = merge_sort(arr[mid..])

  merge(left, right)
end

def merge(left, right)
  result = []
  while left.any? && right.any?
    if left.first <= right.first
      result << left.shift
    else
      result << right.shift
    end
  end
  result + left + right
end

arr = [4, 3, 2, 1]
puts merge_sort(arr)  # Output: [1, 2, 3, 4]
```

---

## 2. **Search Algorithms** 🔎

Search algorithms help find an element in a data structure. The most commonly used algorithms are **Linear Search** and **Binary Search**.

![types-of-search-algorithms](https://github.com/user-attachments/assets/ac8200ca-85bb-4d7a-a5ee-b8515c3c1395)

### Example: Binary Search (Python)
Binary Search works efficiently on sorted arrays by dividing the search space in half.

```python
def binary_search(arr, target):
    low, high = 0, len(arr) - 1
    
    while low <= high:
        mid = (low + high) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            low = mid + 1
        else:
            high = mid - 1
    return -1

arr = [1, 2, 3, 4, 5]
print(binary_search(arr, 3))  # Output: 2
```

---

## 3. **Graph Algorithms** 🌐

Graphs represent connections between nodes. Many problems, such as network routing and social connections, are modeled as graphs. Essential graph algorithms include:

- **Breadth-First Search (BFS)**
- **Depth-First Search (DFS)**
- **Dijkstra’s Algorithm** (for shortest path)

![1_0DEK_ouEZZz4_MMdhKE_Wg](https://github.com/user-attachments/assets/a2ced0b0-4665-4170-b986-f02b1be68109)

### Example: Depth-First Search (JavaScript)
DFS explores nodes by going as deep as possible down one path before backtracking.

```javascript
function dfs(graph, node, visited = new Set()) {
  if (visited.has(node)) return;
  
  visited.add(node);
  console.log(node);

  graph[node].forEach(neighbor => dfs(graph, neighbor, visited));
}

const graph = {
  A: ['B', 'C'],
  B: ['D'],
  C: ['E'],
  D: [],
  E: ['F'],
  F: []
};

dfs(graph, 'A'); // Output: A, B, D, C, E, F
```

---

## 4. **Dynamic Programming (DP)** 📊

Dynamic Programming is an optimization technique that solves complex problems by breaking them down into simpler subproblems. It’s particularly effective for problems involving optimal decisions, like **Knapsack Problem**, **Fibonacci Sequence**, or **Longest Common Subsequence**.

![1702106715913](https://github.com/user-attachments/assets/dbebfc26-ca12-4558-9ec6-c3cfdd8cd6fc)

### Example: Fibonacci with Dynamic Programming (Python)
DP stores the results of subproblems to avoid redundant calculations.

```python
def fibonacci(n, memo={}):
    if n in memo:
        return memo[n]
    if n <= 2:
        return 1
    memo[n] = fibonacci(n - 1, memo) + fibonacci(n - 2, memo)
    return memo[n]

print(fibonacci(10))  # Output: 55
```

---

## 5. **Greedy Algorithms** 💡

Greedy algorithms make the locally optimal choice at each step with the hope of finding the global optimum. Classic problems include **Coin Change**, **Huffman Encoding**, and **Activity Selection**.

![1_RISNojLWzlJsrmip2Qm9NA](https://github.com/user-attachments/assets/513ca6bd-4a2b-47a7-9389-8ddebb3f357b)

### Example: Coin Change Problem (Ruby)
The objective is to make change for a given amount with the fewest coins.

```ruby
def coin_change(coins, amount)
  count = 0
  coins.sort.reverse.each do |coin|
    count += amount / coin
    amount %= coin
  end
  count
end

puts coin_change([1, 5, 10, 25], 63)  # Output: 6 (2 x 25, 1 x 10, 3 x 1)
```

---

## 6. **Divide and Conquer** ⚔️

Divide and Conquer algorithms break the problem into smaller subproblems, solve them individually, and then combine the results. **Merge Sort** and **Quick Sort** are examples of this approach.

![divide_and_conquer_approach](https://github.com/user-attachments/assets/7f4dc65e-a25a-4ea9-a6ed-8bb5afdabbf2)

### Example: Quick Sort (Python)
Quick Sort selects a pivot element and partitions the array such that elements less than the pivot are on one side and greater elements on the other.

```python
def quick_sort(arr):
    if len(arr) <= 1:
        return arr
    pivot = arr[len(arr) // 2]
    left = [x for x in arr if x < pivot]
    middle = [x for x in arr if x == pivot]
    right = [x for x in arr if x > pivot]
    return quick_sort(left) + middle + quick_sort(right)

arr = [3, 6, 8, 10, 1, 2, 1]
print(quick_sort(arr))  # Output: [1, 1, 2, 3, 6, 8, 10]
```

---

## 7. **Backtracking** 🔙

Backtracking is used to solve constraint-satisfaction problems. It explores possible solutions by trying partial solutions and abandoning them if they are not feasible. It's commonly used in problems like **Sudoku**, **N-Queens**, or **Maze Solving**.

![backtracking](https://github.com/user-attachments/assets/e9e25417-2ca6-4582-b68a-030165cd42ef)

### Example: N-Queens Problem (Python)
The goal is to place N queens on an NxN chessboard such that no two queens threaten each other.

```python
def solve_n_queens(n):
    def is_safe(board, row, col):
        for i in range(row):
            if board[i] == col or board[i] - i == col - row or board[i] + i == col + row:
                return False
        return True

    def solve(board, row):
        if row == n:
            result.append(board[:])
            return
        for col in range(n):
            if is_safe(board, row, col):
                board[row] = col
                solve(board, row + 1)

    result = []
    solve([-1] * n, 0)
    return result

print(solve_n_queens(4))  # Output: Possible positions for 4 queens
```

---

## 8. **Hashing Algorithms** 🗂️

Hashing algorithms map data of arbitrary size to fixed-size values. They are widely used in **Hash Tables**, **Databases**, and **Cryptography**. The focus is on minimizing collisions while ensuring fast lookup times.

![HashingAlgorithm](https://github.com/user-attachments/assets/f9a291f1-b347-4f5e-b96d-ffbf76148aad)

### Example: Simple Hash Table (JavaScript)
```javascript
class HashTable {
  constructor(size) {
    this.table = new Array(size);
  }

  hash(key) {
    let hash = 0;
    for (let char of key) {
      hash += char.charCodeAt(0);
    }
    return hash % this.table.length;
  }

  set(key, value) {
    const index = this.hash(key);
    this.table[index] = value;
  }

  get(key) {
    const index = this.hash(key);
    return this.table[index];
  }
}

const ht = new HashTable(50);
ht.set("name", "Lakhveer");
console.log(ht.get("name"));  // Output: Lakhveer
```

---

## Conclusion 🎯

Understanding these algorithms not only makes you a better programmer but also helps you write optimized, scalable, and efficient code. Whether you're preparing for an interview or building a new feature, these algorithms will guide your problem-solving approach. Happy coding! 😊

--- 

Hope this helps you in leveling up your programming skills! Feel free to add these algorithms to your toolbox and share them with fellow coders. 🚀
