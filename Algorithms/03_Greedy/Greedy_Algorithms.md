# Greedy Algorithms - Comprehensive Notes

## Overview
Greedy algorithms make locally optimal choices at each step with the hope of finding a global optimum. They are characterized by:
- Making the best choice at each step
- Never reconsidering previous choices
- Fast and simple to implement
- Don't always guarantee optimal solution

---

## Key Characteristics

### 1. Greedy Choice Property
A global optimum can be arrived at by making locally optimal choices.

### 2. Optimal Substructure
An optimal solution contains optimal solutions to subproblems.

---

## Classic Greedy Problems

## 1. Activity Selection Problem

### Problem Statement
Given n activities with start and finish times, select maximum number of non-overlapping activities.

### Greedy Strategy
Select activities in order of their finishing times. Always pick the next activity that finishes first.

### Implementation (Python)
```python
def activity_selection(start, finish):
    """
    Select maximum number of non-overlapping activities
    Input: start times and finish times
    Output: list of selected activity indices
    """
    # Sort activities by finish time
    activities = [(finish[i], start[i], i) for i in range(len(start))]
    activities.sort()
    
    selected = []
    last_finish = 0
    
    for f, s, idx in activities:
        if s >= last_finish:  # Non-overlapping
            selected.append(idx)
            last_finish = f
    
    return selected

# Example
start = [1, 3, 0, 5, 8, 5]
finish = [2, 4, 6, 7, 9, 9]
result = activity_selection(start, finish)
print(f"Selected activities: {result}")
print(f"Maximum activities: {len(result)}")
```

### Dry Run Example
```
Activities: [(Start, Finish)]
A1: (1, 2)
A2: (3, 4)
A3: (0, 6)
A4: (5, 7)
A5: (8, 9)
A6: (5, 9)

After sorting by finish time:
A1: (1, 2) ✓ Selected (last_finish = 2)
A2: (3, 4) ✓ Selected (3 >= 2, last_finish = 4)
A3: (0, 6) ✗ Rejected (0 < 4)
A4: (5, 7) ✓ Selected (5 >= 4, last_finish = 7)
A5: (8, 9) ✓ Selected (8 >= 7, last_finish = 9)
A6: (5, 9) ✗ Rejected (5 < 7)

Selected: A1, A2, A4, A5
Maximum: 4 activities
```

### Complexity
- **Time**: O(n log n) - sorting
- **Space**: O(n)

---

## 2. Fractional Knapsack

### Problem Statement
Given weights and values of n items, and a knapsack with capacity W, maximize the value by taking fractions of items.

### Greedy Strategy
Sort items by value-to-weight ratio in descending order. Pick items with highest ratio first.

### Implementation (Python)
```python
def fractional_knapsack(values, weights, capacity):
    """
    Fractional knapsack using greedy approach
    """
    n = len(values)
    
    # Calculate value-to-weight ratio
    items = [(values[i]/weights[i], values[i], weights[i], i) 
             for i in range(n)]
    items.sort(reverse=True)  # Sort by ratio descending
    
    total_value = 0
    fractions = [0] * n
    
    for ratio, value, weight, idx in items:
        if capacity >= weight:
            # Take full item
            fractions[idx] = 1.0
            total_value += value
            capacity -= weight
        else:
            # Take fraction of item
            fractions[idx] = capacity / weight
            total_value += value * (capacity / weight)
            break
    
    return total_value, fractions

# Example
values = [60, 100, 120]
weights = [10, 20, 30]
capacity = 50

max_value, fractions = fractional_knapsack(values, weights, capacity)
print(f"Maximum value: {max_value}")
print(f"Fractions taken: {fractions}")
```

### Dry Run Example
```
Items: [(Value, Weight, Ratio)]
I1: (60, 10, 6.0)
I2: (100, 20, 5.0)
I3: (120, 30, 4.0)
Capacity: 50

After sorting by ratio:
I1: ratio=6.0, weight=10
    Take full: capacity=40, value=60

I2: ratio=5.0, weight=20
    Take full: capacity=20, value=160

I3: ratio=4.0, weight=30
    Take 20/30 fraction: capacity=0, value=160+80=240

Maximum Value: 240
Fractions: [1.0, 1.0, 0.667]
```

### Complexity
- **Time**: O(n log n)
- **Space**: O(n)

---

## 3. Job Sequencing Problem

### Problem Statement
Given jobs with deadlines and profits, schedule jobs to maximize profit (one job per unit time).

### Greedy Strategy
Sort jobs by profit in descending order. Assign each job to the latest available slot before its deadline.

### Implementation (Python)
```python
def job_sequencing(jobs, n_slots):
    """
    Job sequencing to maximize profit
    jobs: list of (id, deadline, profit)
    """
    # Sort jobs by profit (descending)
    jobs.sort(key=lambda x: x[2], reverse=True)
    
    # Track free slots
    slots = [-1] * n_slots
    total_profit = 0
    job_sequence = []
    
    for job_id, deadline, profit in jobs:
        # Find latest available slot before deadline
        for slot in range(min(n_slots, deadline) - 1, -1, -1):
            if slots[slot] == -1:
                slots[slot] = job_id
                total_profit += profit
                job_sequence.append(job_id)
                break
    
    return total_profit, job_sequence

# Example
jobs = [
    ('J1', 2, 100),
    ('J2', 1, 19),
    ('J3', 2, 27),
    ('J4', 1, 25),
    ('J5', 3, 15)
]

profit, sequence = job_sequencing(jobs, 3)
print(f"Maximum profit: {profit}")
print(f"Job sequence: {sequence}")
```

### Dry Run Example
```
Jobs: (ID, Deadline, Profit)
J1: (2, 100)
J2: (1, 19)
J3: (2, 27)
J4: (1, 25)
J5: (3, 15)

After sorting by profit:
J1: (2, 100) - Assign to slot 1 → [_, J1, _]
J3: (2, 27)  - Assign to slot 0 → [J3, J1, _]
J4: (1, 25)  - No slot available before deadline 1
J2: (1, 19)  - No slot available before deadline 1
J5: (3, 15)  - Assign to slot 2 → [J3, J1, J5]

Total Profit: 100 + 27 + 15 = 142
Sequence: [J1, J3, J5]
```

### Complexity
- **Time**: O(n²)
- **Space**: O(n)

---

## 4. Huffman Coding

### Problem Statement
Given characters and their frequencies, generate optimal prefix-free binary codes to minimize total encoding length.

### Greedy Strategy
Build a binary tree by repeatedly combining two nodes with smallest frequencies.

### Implementation (Python)
```python
import heapq
from collections import defaultdict

class Node:
    def __init__(self, char, freq):
        self.char = char
        self.freq = freq
        self.left = None
        self.right = None
    
    def __lt__(self, other):
        return self.freq < other.freq

def huffman_coding(char_freq):
    """
    Generate Huffman codes for given character frequencies
    """
    # Create min heap
    heap = [Node(char, freq) for char, freq in char_freq.items()]
    heapq.heapify(heap)
    
    # Build Huffman tree
    while len(heap) > 1:
        left = heapq.heappop(heap)
        right = heapq.heappop(heap)
        
        merged = Node(None, left.freq + right.freq)
        merged.left = left
        merged.right = right
        
        heapq.heappush(heap, merged)
    
    # Generate codes
    root = heap[0]
    codes = {}
    
    def generate_codes(node, code):
        if node.char is not None:
            codes[node.char] = code
            return
        
        if node.left:
            generate_codes(node.left, code + '0')
        if node.right:
            generate_codes(node.right, code + '1')
    
    generate_codes(root, '')
    return codes

# Example
char_freq = {'a': 5, 'b': 9, 'c': 12, 'd': 13, 'e': 16, 'f': 45}
codes = huffman_coding(char_freq)

for char, code in sorted(codes.items()):
    print(f"{char}: {code}")

# Calculate average length
total_freq = sum(char_freq.values())
avg_length = sum(len(codes[char]) * char_freq[char] for char in codes) / total_freq
print(f"Average code length: {avg_length:.2f}")
```

### Dry Run Example
```
Characters: {a:5, b:9, c:12, d:13, e:16, f:45}

Step 1: Combine a(5) and b(9) → ab(14)
Heap: [c:12, d:13, ab:14, e:16, f:45]

Step 2: Combine c(12) and d(13) → cd(25)
Heap: [ab:14, e:16, cd:25, f:45]

Step 3: Combine ab(14) and e(16) → abe(30)
Heap: [cd:25, abe:30, f:45]

Step 4: Combine cd(25) and abe(30) → cdabe(55)
Heap: [f:45, cdabe:55]

Step 5: Combine f(45) and cdabe(55) → root(100)

Final Tree:
         100
        /    \
      45(f)   55
             /   \
           25     30
          / \    / \
        12  13  14  16
        c   d   /\   e
              5  9
              a  b

Codes:
f: 0
c: 100
d: 101
a: 1100
b: 1101
e: 111
```

### Complexity
- **Time**: O(n log n)
- **Space**: O(n)

---

## 5. Coin Change (Greedy)

### Problem Statement
Given coin denominations and amount, find minimum number of coins needed.

### Note
Greedy works for canonical coin systems (like US: 1, 5, 10, 25), but not for all systems.

### Implementation (Python)
```python
def coin_change_greedy(coins, amount):
    """
    Greedy coin change (may not always give optimal solution)
    """
    coins.sort(reverse=True)  # Sort descending
    
    count = 0
    result = []
    
    for coin in coins:
        while amount >= coin:
            amount -= coin
            count += 1
            result.append(coin)
    
    if amount == 0:
        return count, result
    else:
        return -1, []  # Cannot make exact change

# Example - Works for canonical system
coins = [1, 5, 10, 25]
amount = 63
count, result = coin_change_greedy(coins, amount)
print(f"Minimum coins: {count}")
print(f"Coins used: {result}")

# Example - Fails for non-canonical system
coins = [1, 3, 4]
amount = 6
count, result = coin_change_greedy(coins, amount)
print(f"Greedy result: {result}")  # [4, 1, 1] = 3 coins
print(f"Optimal: [3, 3]")  # 2 coins (greedy fails here)
```

### Complexity
- **Time**: O(n log n + amount/min_coin)
- **Space**: O(k) where k is number of coins used

---

## 6. Minimum Spanning Tree - Prim's Algorithm

### Problem Statement
Find minimum cost spanning tree in a weighted undirected graph.

### Greedy Strategy
Start with any vertex. Always add the minimum weight edge that connects a vertex in MST to a vertex outside.

### Implementation (Python)
```python
import heapq

def prims_mst(graph, start=0):
    """
    Prim's algorithm for Minimum Spanning Tree
    graph: adjacency list with weights
    """
    n = len(graph)
    visited = [False] * n
    min_heap = [(0, start, -1)]  # (weight, vertex, parent)
    
    mst_edges = []
    total_cost = 0
    
    while min_heap:
        weight, u, parent = heapq.heappop(min_heap)
        
        if visited[u]:
            continue
        
        visited[u] = True
        if parent != -1:
            mst_edges.append((parent, u, weight))
            total_cost += weight
        
        # Add edges to unvisited neighbors
        for v, w in graph[u]:
            if not visited[v]:
                heapq.heappush(min_heap, (w, v, u))
    
    return mst_edges, total_cost

# Example
graph = [
    [(1, 2), (3, 6)],           # 0: edges to 1(weight 2), 3(weight 6)
    [(0, 2), (2, 3), (3, 8), (4, 5)],  # 1
    [(1, 3), (4, 7)],           # 2
    [(0, 6), (1, 8)],           # 3
    [(1, 5), (2, 7)]            # 4
]

mst, cost = prims_mst(graph)
print(f"MST edges: {mst}")
print(f"Total cost: {cost}")
```

---

## 7. Minimum Spanning Tree - Kruskal's Algorithm

### Greedy Strategy
Sort all edges by weight. Add edges in increasing order if they don't form a cycle (use Union-Find).

### Implementation (Python)
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
        px, py = self.find(x), self.find(y)
        if px == py:
            return False
        
        if self.rank[px] < self.rank[py]:
            px, py = py, px
        
        self.parent[py] = px
        if self.rank[px] == self.rank[py]:
            self.rank[px] += 1
        
        return True

def kruskals_mst(n, edges):
    """
    Kruskal's algorithm for MST
    n: number of vertices
    edges: list of (u, v, weight)
    """
    edges.sort(key=lambda x: x[2])  # Sort by weight
    uf = UnionFind(n)
    
    mst_edges = []
    total_cost = 0
    
    for u, v, weight in edges:
        if uf.union(u, v):
            mst_edges.append((u, v, weight))
            total_cost += weight
            
            if len(mst_edges) == n - 1:
                break
    
    return mst_edges, total_cost

# Example
n = 5
edges = [
    (0, 1, 2), (0, 3, 6), (1, 2, 3),
    (1, 3, 8), (1, 4, 5), (2, 4, 7)
]

mst, cost = kruskals_mst(n, edges)
print(f"MST edges: {mst}")
print(f"Total cost: {cost}")
```

---

## When Greedy Works vs Fails

### ✅ Greedy Works (Optimal)
1. Activity Selection
2. Fractional Knapsack
3. Huffman Coding
4. Minimum Spanning Tree (Prim's, Kruskal's)
5. Dijkstra's Shortest Path (non-negative weights)

### ❌ Greedy Fails (Not Optimal)
1. **0/1 Knapsack** - Need Dynamic Programming
2. **Longest Path** - NP-hard
3. **Traveling Salesman** - NP-hard
4. **Graph Coloring** - Need backtracking
5. **Coin Change** (non-canonical systems) - Need DP

---

## Greedy vs Dynamic Programming

| Aspect | Greedy | Dynamic Programming |
|--------|--------|---------------------|
| Choice | Makes one choice at each step | Considers all choices |
| Reconsider | Never | Can reconsider |
| Complexity | Usually faster | Usually slower |
| Optimality | Not always optimal | Always optimal |
| Examples | Activity Selection | 0/1 Knapsack |

---

## Exam Tips

### 1. Identifying Greedy Problems
- Look for "maximum" or "minimum" in problem
- Check if greedy choice property holds
- Verify optimal substructure

### 2. Common Patterns
```python
# Pattern 1: Sort and select
items.sort(key=lambda x: some_criteria)
for item in items:
    if can_select(item):
        select(item)

# Pattern 2: Min heap approach
heap = []
while heap:
    current = heappop(heap)
    process(current)
    add_neighbors_to_heap()
```

### 3. Dry Run Tips
- Always sort first if needed
- Track the greedy choice at each step
- Verify no cycles (for graph problems)
- Calculate running total

### 4. Complexity Analysis
- Sorting: O(n log n)
- Heap operations: O(log n)
- Union-Find: O(α(n)) ≈ O(1)

### Key Points
1. Greedy makes locally optimal choice
2. Not guaranteed to find global optimum
3. Must prove greedy choice property
4. Usually faster than DP
5. Common in optimization problems
