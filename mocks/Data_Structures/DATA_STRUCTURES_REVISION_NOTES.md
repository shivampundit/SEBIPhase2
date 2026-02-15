# DATA STRUCTURES REVISION NOTES
**Quick Reference for GATE-Level SEBI Exam | 1-Day Study Guide**

---

## 1. ARRAYS

### Time Complexity
| Operation | Array | Dynamic Array |
|-----------|-------|---------------|
| Access | O(1) | O(1) |
| Search (unsorted) | O(n) | O(n) |
| Search (sorted) | O(log n) | O(log n) |
| Insert (end) | O(1) | O(1) amortized |
| Insert (middle) | O(n) | O(n) |
| Delete | O(n) | O(n) |

### Key Algorithms
- **Kadane's**: Max subarray sum in O(n)
- **Two-Pointer**: Used for pair sum, palindrome check
- **Sliding Window**: Max of subarray, smallest subarray with sum
- **Dutch National Flag**: Sort 0,1,2 in O(n)
- **Moore's Voting**: Majority element in O(n), O(1) space

### Important Formulas
- **2D Array Address** (row-major): `Base + (i × cols + j) × size`
- **Column-major**: `Base + (j × rows + i) × size`
- **Rotate array right by k**: Reverse(0,n-1), Reverse(0,k-1), Reverse(k,n-1)

### Common Patterns
- **Prefix Sum**: For range queries
- **Difference Array**: Range updates
- **Binary Search variations**: Rotated array, peak element

---

## 2. LINKED LISTS

### Types & Operations
| Type | Access | Insert | Delete | Space |
|------|--------|--------|--------|-------|
| Singly | O(n) | O(1) | O(1) | n× 1 pointer |
| Doubly | O(n) | O(1) | O(1) | n × 2 pointers |
| Circular | O(n) | O(1) | O(1) | n × 1+ pointer |

### Key Techniques
- **Fast-Slow Pointer**: Find middle, detect cycle
- **Floyd's Cycle Detection**: Slow+Fast meet if cycle exists
- **Reverse**: Iterative (O(1) space) or Recursive (O(n) stack)
- **Merge Two Sorted**: O(m+n) time, O(1) space

### Common Operations
- **Remove nth from end**: Two pointers with n gap
- **LRU Cache**: Hash map + Doubly LL for O(1) get/put
- **Clone with random**: Interleaving or hash map
- **Palindrome check**: Reverse second half, compare

---

## 3. STACKS

### Applications
| Problem | Approach | Time |
|---------|----------|------|
| Balanced Parentheses | Push '(', pop on ')' | O(n) |
| Infix to Postfix | Operator precedence stack | O(n) |
| Evaluate Postfix | Operand stack | O(n) |
| Next Greater Element | Monotonic decreasing stack | O(n) |
| Largest Rectangle | Histogram with stack | O(n) |
| Min Stack | Auxiliary stack or formula | O(1) ops |

### Conversions
- **Infix**: A + B × C
- **Postfix**: A B C × +
- **Prefix**: + A × B C

### Key Points
- **LIFO**: Last In First Out
- **Implementation**: Array or Linked List
- **Stack using Queue**: Make push O(n) or pop O(n)
- **Queue using Stack**: Two stacks, amortized O(1)

---

## 4. QUEUES

### Types
| Type | Enqueue | Dequeue | Access |
|------|---------|---------|--------|
| Linear | O(1) | O(1) | O(1) front |
| Circular | O(1) | O(1) | O(1) |
| Priority | O(log n) | O(log n) | O(1) top |
| Deque | O(1) both ends | O(1) both | O(1) |

### Applications
- **BFS**: Graph traversal, shortest path (unweighted)
- **Level Order**: Tree traversal
- **Sliding Window Max**: Monotonic deque in O(n)
- **First Non-Repeating**: Queue + frequency map

### Circular Queue Full Condition
- **(rear + 1) % size == front**

---

## 5. TREES

### Binary Tree Properties
- **Max nodes at level L**: 2^L
- **Max nodes with height h**: 2^(h+1) - 1
- **Height with n nodes**: log₂(n) (min) to n-1 (max)
- **Leaf nodes in full BT**: (n+1)/2

### Tree Types
| Type | Property |
|------|----------|
| Full | Every node has 0 or 2 children |
| Complete | All levels full except last (filled left→right) |
| Perfect | All internal nodes have 2 children, all leaves at same level |
| Balanced | |height(left) - height(right)| ≤ 1 |

### Traversals
- **Preorder**: Root → Left → Right (DFS)
- **Inorder**: Left → Root → Right (sorted for BST)
- **Postorder**: Left → Right → Root
- **Level Order**: BFS with queue

### Important Algorithms
- **Diameter**: Max(left_dia, right_dia, left_height + right_height)
- **LCA**: Lowest Common Ancestor
- **Morris Traversal**: O(1) space using threading
- **Serialize**: Preorder with NULL markers

---

## 6. BINARY SEARCH TREES (BST)

### Properties
- **Left subtree < Node < Right subtree**
- **Inorder traversal**: Sorted sequence
- Operations: Search, Insert, Delete in O(h), h = height

### Operations
- **Search**: O(h) best, O(n) worst (skewed)
- **Insert**: Similar to search, add at leaf
- **Delete**: 3 cases - leaf, one child, two children (use successor/predecessor)
- **Floor**: Largest ≤ x
- **Ceiling**: Smallest ≥ x

### Balanced BSTs
| Type | Height | Rotations |
|------|--------|-----------|
| AVL | log n | Balance factor ±1 |
| Red-Black | log n | Color properties |
| Splay | amortized log n | Self-adjusting |

---

## 7. HEAPS

### Properties
- **Max Heap**: Parent ≥ children
- **Min Heap**: Parent ≤ children
- **Complete Binary Tree** representation in array

### Array Representation (0-indexed)
- **Parent of i**: (i-1)/2
- **Left child of i**: 2i + 1
- **Right child of i**: 2i + 2

### Operations
| Operation | Time |
|-----------|------|
| Build Heap | O(n) |
| Insert | O(log n) |
| Extract Min/Max | O(log n) |
| Peek | O(1) |
| Heapify | O(log n) |

### Applications
- **Heap Sort**: O(n log n)
- **Priority Queue**: Scheduling
- **Kth Largest/Smallest**: Min/Max heap of size k
- **Median Finder**: Two heaps (max for left, min for right)
- **Merge K Lists**: Min heap of k elements

---

## 8. HASHING

### Hash Function Requirements
- **Deterministic**: Same input → same output
- **Uniform Distribution**: Minimize collisions
- **Efficient**: Fast computation

### Collision Resolution
| Method | Technique | Complexity |
|--------|-----------|------------|
| Chaining | Linked list at each bucket | O(1 + α) |
| Open Addressing | Linear probing: h(x,i) = (h(x)+i) % m | O(1/(1-α)) |
| | Quadratic: h(x,i) = (h(x)+i²) % m | Better clustering |
| | Double hash: h(x,i) = (h1(x)+i×h2(x)) % m | Best |

### Load Factor
- **α = n/m** (n elements, m slots)
- **Resize when**: α > 0.7-0.75
- **After resize**: Rehash all elements

### Applications
- **Two Sum**: O(n) with hash map
- **Group Anagrams**: Sorted string as key
- **Subarray Sum = k**: Prefix sum + hash map
- **LRU Cache**: Hash map + Doubly LL

---

## 9. GRAPHS

### Representations
| Type | Space | Check Edge | Get Neighbors |
|------|-------|------------|---------------|
| Adjacency Matrix | O(V²) | O(1) | O(V) |
| Adjacency List | O(V+E) | O(degree) | O(degree) |

### Traversals
- **DFS**: Stack (or recursion), O(V+E)
- **BFS**: Queue, O(V+E)

### Shortest Path
| Algorithm | Use Case | Time |
|-----------|----------|------|
| BFS | Unweighted | O(V+E) |
| Dijkstra | Non-negative weights | O((V+E) log V) with heap |
| Bellman-Ford | Negative weights | O(VE) |
| Floyd-Warshall | All-pairs | O(V³) |

### MST Algorithms
- **Prim's**: Priority queue, O(E log V)
- **Kruskal's**: Sort edges + Union-Find, O(E log E)

### Important Concepts
- **Cycle Detection**: DFS (back edge) or Union-Find
- **Topological Sort**: DFS or Kahn's (BFS), only for DAG
- **Strongly Connected**: Kosaraju or Tarjan
- **Bipartite**: 2-coloring with BFS/DFS
- **Articulation Point**: Removal increases components
- **Bridge**: Edge whose removal disconnects

### Graph Properties
- **Eulerian**: All even degree (circuit) or 0/2 odd degree (path)
- **Tree**: Connected + (n-1) edges

---

## 10. ADVANCED STRUCTURES

### Trie (Prefix Tree)
- **Operations**: Insert, Search, StartsWith in O(L), L = word length
- **Space**: O(ALPHABET_SIZE × N × L)
- **Use**: Autocomplete, spell check, IP routing

### Segment Tree
- **Build**: O(n)
- **Query**: O(log n) (range min/max/sum)
- **Update**: O(log n)
- **Space**: O(4n) ≈ O(n)

### Fenwick Tree (BIT)
- **Build**: O(n log n) or O(n) optimized
- **Query**: O(log n) (prefix sum)
- **Update**: O(log n)
- **Space**: O(n)
- **Simpler than Segment Tree**

### Disjoint Set (Union-Find)
- **Find** with path compression: O(α(n)) ≈ O(1)
- **Union** by rank: O(α(n))
- **Applications**: Kruskal's MST, connected components

### Suffix Array
- **Build**: O(n log n)
- **Search pattern**: O(m log n), m = pattern length
- **Space**: O(n), less than suffix tree

---

## COMPLEXITY CHEAT SHEET

### Common Time Complexities (n = 10⁶)
- **O(1)**: Constant - any size ✓
- **O(log n)**: ~20 operations ✓
- **O(n)**: 10⁶ operations ✓
- **O(n log n)**: ~2×10⁷ operations ✓
- **O(n²)**: 10¹² operations ✗ (for n=10⁶)
- **O(2ⁿ)**: Exponential ✗

### Space Complexity
- **In-place**: O(1) extra space
- **Recursive**: O(h) stack space, h = recursion depth

---

## SYNTAX QUICK REFERENCE

### C/C++
```cpp
// Array
int arr[5] = {1,2,3};  // Rest = 0

// Vector
vector<int> v; v.push_back(1);

// Stack
stack<int> s; s.push(1); s.pop(); s.top();

// Queue
queue<int> q; q.push(1); q.pop(); q.front();

// Priority Queue (max heap)
priority_queue<int> pq;
// Min heap
priority_queue<int, vector<int>, greater<int>> minpq;

// Set/Map
set<int> s; map<int,int> m;
unordered_set<int> us; unordered_map<int,int> um;
```

### Java
```java
// ArrayList
ArrayList<Integer> list = new ArrayList<>();

// Stack
Stack<Integer> stack = new Stack<>();

// Queue
Queue<Integer> q = new LinkedList<>();

// Priority Queue (min heap)
PriorityQueue<Integer> pq = new PriorityQueue<>();

// HashSet/HashMap
HashSet<Integer> set = new HashSet<>();
HashMap<Integer, Integer> map = new HashMap<>();
```

### Python
```python
# List (dynamic array)
arr = []; arr.append(1)

# Stack
stack = []; stack.append(1); stack.pop()

# Queue
from collections import deque
q = deque(); q.append(1); q.popleft()

# Heap (min heap)
import heapq
heap = []; heapq.heappush(heap, 1); heapq.heappop(heap)

# Set/Dict
s = set(); d = {}
```

---

## COMMON PITFALLS

### Arrays
- **Index out of bounds**: Check `0 ≤ i < n`
- **Integer overflow**: Use `mid = low + (high-low)/2`
- **Uninitialized memory**: C++ arrays not initialized

### Linked Lists
- **NULL pointer access**: Always check `if(node != NULL)`
- **Lost references**: Store `next` before modifying pointers
- **Memory leaks**: Free nodes after deletion (C/C++)

### Trees
- **NULL checks**: Handle empty tree cases
- **Height definition**: Some use edges, others nodes
- **Recursion depth**: Stack overflow for skewed trees

### Graphs
- **Disconnected graphs**: May need multiple DFS/BFS calls
- **Visited array**: Essential to avoid infinite loops
- **Directed vs Undirected**: Different cycle detection

---

## EXAM STRATEGY

1. **Identify Data Structure**: Array, LL, Stack, Queue, Tree, Graph, Hash
2. **Check Constraints**: n ≤ 10³ → O(n²) OK, n ≤ 10⁶ → need O(n log n)
3. **Output Questions**: Dry run carefully with small example
4. **Syntax**: Know language-specific defaults (Java array/map, Python negative index)
5. **Edge Cases**: Empty, single element, duplicates, negative numbers
6. **Time Management**: Don't stuck on one question > 2 minutes

---

**MEMORIZE TIME COMPLEXITIES AND KEY FORMULAS BEFORE EXAM!**
