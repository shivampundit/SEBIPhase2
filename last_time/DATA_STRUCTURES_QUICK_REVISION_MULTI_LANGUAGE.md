# Data Structures Quick Revision - Multi-Language
## **Last Minute Revision - All Key Concepts**

### 📚 Coverage: C | C++ | Java | Python
### ⚡ Focus: Concepts over Code | Tables | Quick Reference
### 🎯 Syllabus: **Array | Linked List | Stack | Queue | Binary Tree | BST | Heap | Hashing | Matrix | JSON**

---

## 1. ARRAY

### 1.1 Array Fundamentals

| Concept | Definition | Time Complexity | Space |
|---------|------------|----------------|-------|
| **Access** | Direct index access | O(1) | - |
| **Search** | Linear search | O(n) | O(1) |
| **Insert at end** | Add element | O(1) amortized | - |
| **Insert at position** | Shift elements | O(n) | - |
| **Delete** | Shift elements | O(n) | - |
| **Traverse** | Visit all elements | O(n) | - |

### 1.2 Array Types Comparison

| Type | Language | Declaration | Size | Resizable? |
|------|----------|-------------|------|-----------|
| **Static Array** | C/C++ | `int arr[10]` | Fixed | ❌ |
| **Dynamic Array** | C++ | `vector<int> v` | Dynamic | ✅ |
| **Array** | Java | `int[] arr = new int[10]` | Fixed | ❌ |
| **ArrayList** | Java | `ArrayList<Integer> list` | Dynamic | ✅ |
| **List** | Python | `arr = []` | Dynamic | ✅ |

### 1.3 Common Array Operations

| Operation | C/C++ | Java | Python | Time |
|-----------|-------|------|--------|------|
| **Initialize** | `int arr[5] = {0}` | `int[] arr = new int[5]` | `arr = [0]*5` | O(n) |
| **Access** | `arr[i]` | `arr[i]` | `arr[i]` | O(1) |
| **Length** | `sizeof(arr)/sizeof(arr[0])` | `arr.length` | `len(arr)` | O(1) |
| **Append** | Manual/vector.push_back() | list.add() | arr.append() | O(1)* |
| **Insert** | Manual shift | Manual shift | arr.insert(i, val) | O(n) |
| **Remove** | Manual shift | Manual shift | arr.remove(val) | O(n) |
| **Search** | Linear search | Linear search | `val in arr` | O(n) |
| **Sort** | `sort(arr, arr+n)` | `Arrays.sort(arr)` | `arr.sort()` | O(n log n) |

### 1.4 Array vs ArrayList vs Vector

| Feature | C Array | C++ Vector | Java ArrayList | Python List |
|---------|---------|------------|----------------|-------------|
| **Size** | Fixed | Dynamic | Dynamic | Dynamic |
| **Bounds Check** | ❌ | Optional | ✅ | ✅ |
| **Random Access** | O(1) | O(1) | O(1) | O(1) |
| **Insert/Delete** | O(n) | O(n) | O(n) | O(n) |
| **Memory** | Contiguous | Contiguous | Contiguous | Contiguous |
| **Type Safety** | No | Yes (C++) | Yes (Generics) | No (dynamic) |

### 1.5 Key Array Algorithms

| Algorithm | Purpose | Time | Space |
|-----------|---------|------|-------|
| **Kadane's Algorithm** | Max subarray sum | O(n) | O(1) |
| **Two Pointers** | Find pair with sum | O(n) | O(1) |
| **Sliding Window** | Max sum k elements | O(n) | O(1) |
| **Dutch National Flag** | Sort 0,1,2 | O(n) | O(1) |
| **Boyer-Moore Voting** | Majority element | O(n) | O(1) |

---

## 2. LINKED LIST

### 2.1 Linked List Types

| Type | Description | Memory | Access | Insert/Delete |
|------|-------------|--------|--------|---------------|
| **Singly Linked** | One direction pointer | Non-contiguous | O(n) | O(1)* |
| **Doubly Linked** | Two direction pointers | Non-contiguous | O(n) | O(1)* |
| **Circular Linked** | Last → First | Non-contiguous | O(n) | O(1)* |

*At known position

### 2.2 Node Structure

| Language | Singly Linked Node | Doubly Linked Node |
|----------|-------------------|-------------------|
| **C/C++** | `struct Node { int data; Node* next; };` | `struct Node { int data; Node *prev, *next; };` |
| **Java** | `class Node { int data; Node next; }` | `class Node { int data; Node prev, next; }` |
| **Python** | `class Node: data, next` | `class Node: data, prev, next` |

### 2.3 Operations Comparison

| Operation | Array | Singly LL | Doubly LL |
|-----------|-------|-----------|-----------|
| **Access i-th element** | O(1) | O(n) | O(n) |
| **Insert at beginning** | O(n) | O(1) | O(1) |
| **Insert at end** | O(1)* | O(n) or O(1)† | O(1)‡ |
| **Insert at position** | O(n) | O(n) | O(n) |
| **Delete at beginning** | O(n) | O(1) | O(1) |
| **Delete at end** | O(1)* | O(n) | O(1)‡ |
| **Search** | O(n) | O(n) | O(n) |
| **Memory** | Contiguous | Scattered | Scattered |
| **Extra Space per element** | 0 | 1 pointer | 2 pointers |

*Dynamic array | †With tail pointer | ‡With tail pointer

### 2.4 Common Linked List Algorithms

| Algorithm | Purpose | Technique | Time | Space |
|-----------|---------|-----------|------|-------|
| **Reverse** | Reverse list | Iterative/Recursive | O(n) | O(1)/O(n) |
| **Detect Cycle** | Find loop | Floyd's (Slow-Fast) | O(n) | O(1) |
| **Find Middle** | Get middle node | Slow-Fast pointers | O(n) | O(1) |
| **Merge Two Lists** | Combine sorted lists | Two pointers | O(n+m) | O(1) |
| **Remove N-th from End** | Delete node | Two pointers (gap) | O(n) | O(1) |
| **Palindrome Check** | Check palindrome | Reverse half + compare | O(n) | O(1) |

### 2.5 When to Use

| Use Case | Array | Linked List |
|----------|-------|-------------|
| **Random access needed** | ✅ | ❌ |
| **Frequent insert/delete at beginning** | ❌ | ✅ |
| **Frequent insert/delete in middle** | ❌ | ✅ |
| **Known size** | ✅ | Either |
| **Cache performance important** | ✅ | ❌ |
| **Memory fragmentation OK** | ❌ | ✅ |

---

## 3. STACK

### 3.1 Stack Fundamentals

| Property | Description |
|----------|-------------|
| **Principle** | LIFO (Last In First Out) |
| **Primary Operations** | push(), pop(), peek/top() |
| **Access** | Only top element |
| **Time Complexity** | O(1) for all operations |

### 3.2 Implementation Comparison

| Implementation | Language | Declaration | Pros | Cons |
|----------------|----------|-------------|------|------|
| **Array** | C/C++ | `int stack[MAX]; int top = -1;` | Simple, cache-friendly | Fixed size |
| **std::stack** | C++ | `stack<int> s;` | Dynamic, STL | Overhead |
| **Stack** | Java | `Stack<Integer> s = new Stack<>();` | Dynamic | Legacy, use Deque |
| **Deque** | Java | `Deque<Integer> s = new ArrayDeque<>();` | Efficient | - |
| **list** | Python | `stack = []` | Simple, dynamic | - |

### 3.3 Stack Operations

| Operation | Array-based | Linked List-based | Time | Space |
|-----------|-------------|-------------------|------|-------|
| **push(x)** | `stack[++top] = x` | Add at head | O(1) | O(1) |
| **pop()** | `return stack[top--]` | Remove from head | O(1) | O(1) |
| **peek()/top()** | `return stack[top]` | Return head data | O(1) | O(1) |
| **isEmpty()** | `top == -1` | `head == NULL` | O(1) | O(1) |
| **isFull()** | `top == MAX-1` | Never (dynamic) | O(1) | - |

### 3.4 Common Stack Applications

| Application | Purpose | Example |
|-------------|---------|---------|
| **Expression Evaluation** | Infix to Postfix | `(a+b)*c → ab+c*` |
| **Parentheses Matching** | Check balanced | `{[()]}` valid |
| **Function Calls** | Call stack | Recursion |
| **Undo/Redo** | Track operations | Editor |
| **DFS** | Graph traversal | Stack-based |
| **Backtracking** | State tracking | N-Queens |
| **Browser History** | Back button | Page stack |

### 3.5 Stack Problems & Techniques

| Problem Type | Technique | Time | Space |
|--------------|-----------|------|-------|
| **Next Greater Element** | Monotonic stack | O(n) | O(n) |
| **Stock Span** | Monotonic stack | O(n) | O(n) |
| **Largest Rectangle in Histogram** | Monotonic stack | O(n) | O(n) |
| **Valid Parentheses** | Stack matching | O(n) | O(n) |
| **Min Stack** | Auxiliary stack | O(1) ops | O(n) |

---

## 4. QUEUE

### 4.1 Queue Fundamentals

| Property | Description |
|----------|-------------|
| **Principle** | FIFO (First In First Out) |
| **Primary Operations** | enqueue(), dequeue(), front(), rear() |
| **Access** | Front and rear only |
| **Time Complexity** | O(1) for all operations |

### 4.2 Queue Types

| Type | Description | Use Case |
|------|-------------|----------|
| **Simple Queue** | Basic FIFO | General purpose |
| **Circular Queue** | Circular array, reuse space | Fixed-size buffer |
| **Priority Queue** | Element with priority served first | Task scheduling |
| **Deque** | Insert/delete both ends | Stack + Queue operations |

### 4.3 Implementation Comparison

| Implementation | Language | Declaration | Space Utilization |
|----------------|----------|-------------|------------------|
| **Array** | C/C++ | `int queue[MAX]; int front=0, rear=-1;` | Wasted (linear) |
| **Circular Array** | C/C++ | `int queue[MAX]; int front=0, rear=0;` | Efficient |
| **Linked List** | All | Node-based | Dynamic |
| **queue** | C++ | `queue<int> q;` | Dynamic |
| **Queue** | Java | `Queue<Integer> q = new LinkedList<>();` | Dynamic |
| **collections.deque** | Python | `from collections import deque; q = deque()` | Dynamic |

### 4.4 Queue Operations

| Operation | Array (Linear) | Circular Array | Linked List | Time |
|-----------|---------------|----------------|-------------|------|
| **enqueue(x)** | `queue[++rear] = x` | `rear=(rear+1)%MAX; queue[rear]=x` | Add at tail | O(1) |
| **dequeue()** | `return queue[front++]` | `x=queue[front]; front=(front+1)%MAX` | Remove from head | O(1) |
| **front()** | `return queue[front]` | `return queue[front]` | Return head data | O(1) |
| **isEmpty()** | `front > rear` | `front == rear` | `head == NULL` | O(1) |
| **isFull()** | `rear == MAX-1` | `(rear+1)%MAX == front` | Never | O(1) |

### 4.5 Priority Queue

| Implementation | Language | Declaration | enqueue | dequeue | peek |
|----------------|----------|-------------|---------|---------|------|
| **Heap** | C++ | `priority_queue<int> pq;` | O(log n) | O(log n) | O(1) |
| **PriorityQueue** | Java | `PriorityQueue<Integer> pq = new PriorityQueue<>();` | O(log n) | O(log n) | O(1) |
| **heapq** | Python | `import heapq; pq = []` | O(log n) | O(log n) | O(1) |

### 4.6 Common Queue Applications

| Application | Queue Type | Example |
|-------------|-----------|---------|
| **BFS** | Simple Queue | Graph/Tree traversal |
| **Task Scheduling** | Priority Queue | CPU scheduling |
| **Buffering** | Circular Queue | IO buffers |
| **Printer Queue** | Simple Queue | Print jobs |
| **Call Center** | Queue | Customer service |
| **Undo Operations** | Deque | Editor (front=undo, rear=redo) |

---

## 5. BINARY TREE

### 5.1 Binary Tree Fundamentals

| Property | Description |
|----------|-------------|
| **Maximum children** | 2 (left, right) |
| **Height** | h = log₂(n) to n-1 |
| **Nodes at level i** | 2^i |
| **Max nodes (height h)** | 2^(h+1) - 1 |
| **Leaf nodes** | (n+1)/2 for full tree |

### 5.2 Binary Tree Types

| Type | Property | Example |
|------|----------|---------|
| **Full Binary Tree** | Every node has 0 or 2 children | All internals have 2 children |
| **Complete Binary Tree** | All levels filled except last, left-filled | Heap structure |
| **Perfect Binary Tree** | All internals have 2 children, all leaves same level | Height h, 2^(h+1)-1 nodes |
| **Balanced Binary Tree** | Height difference ≤ 1 for all nodes | AVL tree |
| **Degenerate/Skewed** | Each node has 1 child | Linked list |

### 5.3 Node Structure

| Language | Structure |
|----------|-----------|
| **C/C++** | `struct Node { int data; Node *left, *right; };` |
| **Java** | `class Node { int data; Node left, right; }` |
| **Python** | `class Node: __init__(self, data): self.data, self.left, self.right` |

### 5.4 Tree Traversals

| Traversal | Order | Use Case | Iterative? |
|-----------|-------|----------|-----------|
| **Inorder** | Left → Root → Right | BST (sorted order) | Stack |
| **Preorder** | Root → Left → Right | Copy tree, prefix expression | Stack |
| **Postorder** | Left → Right → Root | Delete tree, postfix expression | Stack |
| **Level Order** | Level by level | BFS, print by level | Queue |

### 5.5 Traversal Time/Space Complexity

| Traversal | Recursive Time | Recursive Space | Iterative Time | Iterative Space |
|-----------|---------------|-----------------|----------------|-----------------|
| **Inorder** | O(n) | O(h) | O(n) | O(h) |
| **Preorder** | O(n) | O(h) | O(n) | O(h) |
| **Postorder** | O(n) | O(h) | O(n) | O(h) |
| **Level Order** | O(n) | O(w)† | O(n) | O(w)† |

†w = max width of tree

### 5.6 Common Tree Operations

| Operation | Time (Balanced) | Time (Skewed) | Space |
|-----------|----------------|---------------|-------|
| **Search** | O(h) = O(log n) | O(n) | O(h) |
| **Insert** | O(h) | O(n) | O(h) |
| **Delete** | O(h) | O(n) | O(h) |
| **Height** | O(n) | O(n) | O(h) |
| **Count Nodes** | O(n) | O(n) | O(h) |
| **Diameter** | O(n) | O(n) | O(h) |

### 5.7 Important Tree Algorithms

| Algorithm | Purpose | Time | Space |
|-----------|---------|------|-------|
| **Level Order Traversal** | BFS | O(n) | O(w) |
| **Height/Depth** | Find height | O(n) | O(h) |
| **Diameter** | Longest path between leaves | O(n) | O(h) |
| **Lowest Common Ancestor (LCA)** | Find LCA of two nodes | O(n) | O(h) |
| **Check Balanced** | Verify height balance | O(n) | O(h) |
| **Serialize/Deserialize** | Convert to string | O(n) | O(n) |
| **Vertical Order** | Print columns | O(n log n) | O(n) |
| **Zigzag Traversal** | Alternate level order | O(n) | O(w) |

---

## 6. BINARY SEARCH TREE (BST)

### 6.1 BST Properties

| Property | Description |
|----------|-------------|
| **Left Subtree** | All nodes < root |
| **Right Subtree** | All nodes > root |
| **Inorder Traversal** | Gives sorted sequence |
| **Search/Insert/Delete** | O(h) average |
| **No Duplicates** | Typically (or handled specially) |

### 6.2 BST vs Binary Tree

| Feature | Binary Tree | BST |
|---------|-------------|-----|
| **Node Ordering** | No specific order | Left < Root < Right |
| **Search** | O(n) | O(h) average, O(log n) if balanced |
| **Insert** | Any position | Specific position (maintain order) |
| **Inorder** | Random order | Sorted order |
| **Use Case** | Generic hierarchy | Sorted data, searching |

### 6.3 BST Operations

| Operation | Best Case | Average Case | Worst Case | Notes |
|-----------|-----------|--------------|------------|-------|
| **Search** | O(log n) | O(log n) | O(n) | Worst = skewed |
| **Insert** | O(log n) | O(log n) | O(n) | Recursive or iterative |
| **Delete** | O(log n) | O(log n) | O(n) | 3 cases: leaf, 1 child, 2 children |
| **Find Min** | O(log n) | O(log n) | O(n) | Leftmost node |
| **Find Max** | O(log n) | O(log n) | O(n) | Rightmost node |
| **Successor** | O(h) | O(h) | O(n) | Next in inorder |
| **Predecessor** | O(h) | O(h) | O(n) | Previous in inorder |

### 6.4 BST Delete Cases

| Case | Description | Action |
|------|-------------|--------|
| **Leaf Node** | No children | Simply remove |
| **One Child** | One subtree | Replace with child |
| **Two Children** | Two subtrees | Replace with inorder successor/predecessor |

### 6.5 Self-Balancing BSTs

| Type | Balance Condition | Rotation | Height | Use Case |
|------|------------------|----------|--------|----------|
| **AVL Tree** | |height(left) - height(right)| ≤ 1 | Single/Double | O(log n) | Frequent searches |
| **Red-Black Tree** | Color properties | Recoloring + Rotation | 2*log(n+1) | Frequent insert/delete |
| **B-Tree** | Min/max children per node | Split/Merge | O(log n) | Databases |
| **Splay Tree** | Recently accessed at root | Splaying | Amortized O(log n) | Temporal locality |

### 6.6 Common BST Problems

| Problem | Technique | Time | Space |
|---------|-----------|------|-------|
| **Validate BST** | Inorder or range check | O(n) | O(h) |
| **kth Smallest** | Inorder traversal | O(k) to O(n) | O(h) |
| **Lowest Common Ancestor** | BST property | O(h) | O(1) iterative |
| **Convert Sorted Array to BST** | Divide and conquer | O(n) | O(log n) |
| **Inorder Successor** | BST property | O(h) | O(1) |
| **Range Sum** | Inorder with bounds | O(n) | O(h) |

---

## 7. HEAP

### 7.1 Heap Fundamentals

| Property | Description |
|----------|-------------|
| **Structure** | Complete binary tree |
| **Heap Property** | Parent ≥ children (max) or ≤ (min) |
| **Root** | Maximum (max-heap) or minimum (min-heap) |
| **Height** | O(log n) |
| **Array Representation** | Parent: i, Left: 2i+1, Right: 2i+2 |

### 7.2 Heap Types

| Type | Root Property | Use Case |
|------|---------------|----------|
| **Max Heap** | Root = Maximum | Priority queue (max priority) |
| **Min Heap** | Root = Minimum | Priority queue (min priority) |
| **Binary Heap** | Binary tree | General purpose |
| **Fibonacci Heap** | Better amortized complexity | Dijkstra's algorithm |
| **Binomial Heap** | Collection of binomial trees | Mergeable heaps |

### 7.3 Heap Operations

| Operation | Time Complexity | Description |
|-----------|----------------|-------------|
| **Insert** | O(log n) | Add at end, heapify up |
| **Extract Max/Min** | O(log n) | Remove root, heapify down |
| **Get Max/Min** | O(1) | Return root |
| **Delete** | O(log n) | Replace with last, heapify |
| **Heapify** | O(log n) | Maintain heap property at node |
| **Build Heap** | O(n) | Convert array to heap |
| **Heap Sort** | O(n log n) | Build heap + extract all |

### 7.4 Array Index Formulas

| Node | Index (0-based) | Index (1-based) |
|------|----------------|----------------|
| **Parent of i** | `(i-1)/2` | `i/2` |
| **Left child of i** | `2*i + 1` | `2*i` |
| **Right child of i** | `2*i + 2` | `2*i + 1` |

### 7.5 Language-Specific Heap

| Language | Implementation | Max Heap | Min Heap |
|----------|---------------|----------|----------|
| **C++** | `priority_queue` | `priority_queue<int>` (default) | `priority_queue<int, vector<int>, greater<int>>` |
| **Java** | `PriorityQueue` | `PriorityQueue<>(Collections.reverseOrder())` | `PriorityQueue<>()` (default) |
| **Python** | `heapq` | Use negative values | `heapq` (default) |

### 7.6 Heap Applications

| Application | Heap Type | Purpose |
|-------------|-----------|---------|
| **Priority Queue** | Min/Max heap | Task scheduling |
| **Heap Sort** | Max heap | O(n log n) sorting |
| **K Largest/Smallest** | Min/Max heap | Top K elements |
| **Median Maintenance** | Two heaps | Running median |
| **Merge K Sorted** | Min heap | Merge K arrays/lists |
| **Dijkstra's Algorithm** | Min heap | Shortest path |
| **Huffman Coding** | Min heap | Compression |

### 7.7 Common Heap Problems

| Problem | Technique | Time | Space |
|---------|-----------|------|-------|
| **Kth Largest Element** | Min heap of size k | O(n log k) | O(k) |
| **Top K Frequent** | Heap + frequency map | O(n log k) | O(n) |
| **Merge K Sorted Lists** | Min heap | O(n log k) | O(k) |
| **Running Median** | Two heaps (max+min) | O(log n) insert, O(1) median | O(n) |
| **Sort Nearly Sorted** | Min heap | O(n log k) | O(k) |

---

## 8. HASHING

### 8.1 Hashing Fundamentals

| Concept | Description |
|---------|-------------|
| **Hash Function** | Maps key to index: `h(key) → [0, m-1]` |
| **Hash Table** | Array with hash-based indexing |
| **Collision** | Two keys map to same index |
| **Load Factor** | α = n/m (n=elements, m=slots) |
| **Ideal Time** | O(1) average for all operations |

### 8.2 Hash Functions

| Type | Formula | Use Case |
|------|---------|----------|
| **Division Method** | `h(k) = k % m` | Simple, m should be prime |
| **Multiplication Method** | `h(k) = m * (k*A % 1)` | A ≈ 0.618 (golden ratio) |
| **Mid-Square** | Square key, extract middle digits | Universal |
| **Folding** | Split key, add parts | Long keys |

### 8.3 Collision Resolution

| Method | Technique | Pros | Cons |
|--------|-----------|------|------|
| **Chaining** | Linked list at each slot | Simple, handles high load | Extra space, cache miss |
| **Linear Probing** | `h(k, i) = (h(k) + i) % m` | Cache-friendly | Clustering |
| **Quadratic Probing** | `h(k, i) = (h(k) + c₁i + c₂i²) % m` | Reduces clustering | Secondary clustering |
| **Double Hashing** | `h(k, i) = (h₁(k) + i*h₂(k)) % m` | Less clustering | Two hash functions |

### 8.4 Time Complexity

| Operation | Average Case | Worst Case | Condition |
|-----------|-------------|------------|-----------|
| **Search** | O(1) | O(n) | Worst = all keys collide |
| **Insert** | O(1) | O(n) | Assuming space available |
| **Delete** | O(1) | O(n) | May need tombstone marker |

### 8.5 Language-Specific Hash Tables

| Language | Data Structure | Ordered? | Notes |
|----------|---------------|----------|-------|
| **C++** | `unordered_map<K, V>` | ❌ | Hash table |
| **C++** | `map<K, V>` | ✅ | Red-Black Tree |
| **Java** | `HashMap<K, V>` | ❌ | Hash table |
| **Java** | `LinkedHashMap<K, V>` | ✅ (insertion) | Hash + LinkedList |
| **Java** | `TreeMap<K, V>` | ✅ (sorted) | Red-Black Tree |
| **Python** | `dict` | ✅ (insertion, 3.7+) | Hash table |
| **Python** | `set` | ❌ | Hash set |

### 8.6 HashMap vs TreeMap

| Feature | HashMap | TreeMap |
|---------|---------|---------|
| **Implementation** | Hash Table | Red-Black Tree |
| **Ordering** | ❌ | ✅ (sorted keys) |
| **Search** | O(1) average | O(log n) |
| **Insert** | O(1) average | O(log n) |
| **Delete** | O(1) average | O(log n) |
| **Space** | O(n) | O(n) |
| **Null Keys** | Allowed (Java: 1) | Not allowed |

### 8.7 Common Hashing Applications

| Application | Data Structure | Use Case |
|-------------|---------------|----------|
| **Frequency Counting** | HashMap | Count occurrences |
| **Two Sum Problem** | HashMap | Find pair with sum |
| **Anagram Grouping** | HashMap | Group anagrams |
| **LRU Cache** | HashMap + Doubly LL | Cache implementation |
| **Substring Problems** | HashMap (sliding window) | Longest substring |
| **Duplicate Detection** | HashSet | Find duplicates |

### 8.8 Common Hash Problems

| Problem | Technique | Time | Space |
|---------|-----------|------|-------|
| **Two Sum** | HashMap (value → index) | O(n) | O(n) |
| **Subarray Sum = K** | HashMap (prefix sum) | O(n) | O(n) |
| **Longest Consecutive Sequence** | HashSet | O(n) | O(n) |
| **Group Anagrams** | HashMap (sorted → list) | O(n*k log k) | O(n*k) |
| **First Non-Repeating** | HashMap (frequency) | O(n) | O(n) |

---

## 9. MATRIX

### 9.1 Matrix Fundamentals

| Property | Description |
|----------|-------------|
| **Dimensions** | m × n (rows × columns) |
| **Access** | `matrix[i][j]` - O(1) |
| **Traverse** | Row-wise or Column-wise - O(m*n) |
| **Diagonal** | `matrix[i][i]` |

### 9.2 Matrix Declaration

| Language | Declaration | Example |
|----------|-------------|---------|
| **C** | `int matrix[ROWS][COLS]` | `int m[3][4]` |
| **C++** | `vector<vector<int>> matrix` | `vector<vector<int>> m(3, vector<int>(4))` |
| **Java** | `int[][] matrix = new int[rows][cols]` | `int[][] m = new int[3][4]` |
| **Python** | `matrix = [[]*cols for _ in range(rows)]` | `m = [[0]*4 for _ in range(3)]` |

### 9.3 Common Matrix Traversals

| Traversal | Pattern | Time | Example |
|-----------|---------|------|---------|
| **Row-wise** | `for i: for j: matrix[i][j]` | O(m*n) | Normal iteration |
| **Column-wise** | `for j: for i: matrix[i][j]` | O(m*n) | Cache unfriendly |
| **Diagonal** | `for i: matrix[i][i]` | O(min(m,n)) | Main diagonal |
| **Anti-diagonal** | `for i: matrix[i][n-1-i]` | O(min(m,n)) | Secondary diagonal |
| **Spiral** | Outside → Inside | O(m*n) | Layer by layer |
| **Zigzag** | Alternate direction | O(m*n) | Row-wise alternate |

### 9.4 Matrix Operations

| Operation | Time | Space | Description |
|-----------|------|-------|-------------|
| **Addition** | O(m*n) | O(m*n) | Add corresponding elements |
| **Multiplication** | O(m*n*p) | O(m*p) | C[i][j] = Σ A[i][k]*B[k][j] |
| **Transpose** | O(m*n) | O(m*n) or O(1) | Swap rows ↔ columns |
| **Rotate 90°** | O(m*n) | O(1) | Transpose + reverse rows |
| **Search (sorted)** | O(m+n) | O(1) | Start top-right/bottom-left |

### 9.5 Special Matrices

| Type | Property | Space Optimization |
|------|----------|-------------------|
| **Sparse Matrix** | Most elements = 0 | Store only non-zero |
| **Symmetric** | `A[i][j] = A[j][i]` | Store upper/lower triangle |
| **Diagonal** | Non-zero only on diagonal | Store 1D array |
| **Tridiagonal** | Non-zero on 3 diagonals | Store 3 arrays |
| **Upper Triangular** | 0 below diagonal | Store upper part |
| **Lower Triangular** | 0 above diagonal | Store lower part |

### 9.6 Common Matrix Algorithms

| Algorithm | Purpose | Time | Space |
|-----------|---------|------|-------|
| **Spiral Traversal** | Print spiral order | O(m*n) | O(1) |
| **Rotate 90° Clockwise** | Rotate matrix | O(m*n) | O(1) |
| **Set Matrix Zeroes** | Zero entire row/col | O(m*n) | O(1) |
| **Search 2D Matrix** | Binary search | O(log(m*n)) | O(1) |
| **Longest Increasing Path** | DFS + Memoization | O(m*n) | O(m*n) |
| **Number of Islands** | DFS/BFS | O(m*n) | O(m*n) |

### 9.7 Matrix Search Techniques

| Matrix Type | Technique | Time |
|-------------|-----------|------|
| **Unsorted** | Linear search | O(m*n) |
| **Row-wise sorted** | Binary search each row | O(m log n) |
| **Row & Column sorted** | Staircase search | O(m+n) |
| **Fully sorted** | Single binary search | O(log(m*n)) |

---

## 10. JSON OBJECTS

### 10.1 JSON Fundamentals

| Concept | Description |
|---------|-------------|
| **JSON** | JavaScript Object Notation |
| **Data Types** | String, Number, Boolean, Array, Object, null |
| **Key-Value** | `{"key": "value"}` |
| **Nested** | Objects/Arrays can contain objects/arrays |
| **Lightweight** | Text-based, human-readable |

### 10.2 JSON Structure

| Element | Syntax | Example |
|---------|--------|---------|
| **Object** | `{ }` | `{"name": "John", "age": 30}` |
| **Array** | `[ ]` | `[1, 2, 3, 4]` |
| **String** | `"text"` | `"Hello World"` |
| **Number** | `123` or `12.34` | `42`, `3.14` |
| **Boolean** | `true` / `false` | `true` |
| **Null** | `null` | `null` |

### 10.3 JSON Operations by Language

| Language | Parse JSON | Create JSON | Access | Library |
|----------|-----------|------------|--------|---------|
| **C++** | Parse string | Build object | `obj["key"]` | `nlohmann/json`, `RapidJSON` |
| **Java** | `JSONObject obj = new JSONObject(str)` | `JSONObject obj = new JSONObject()` | `obj.get("key")` | `org.json`, `Gson`, `Jackson` |
| **Python** | `json.loads(str)` | `json.dumps(dict)` | `obj["key"]` | `json` (built-in) |
| **JavaScript** | `JSON.parse(str)` | `JSON.stringify(obj)` | `obj.key` or `obj["key"]` | Built-in |

### 10.4 Python JSON

| Operation | Method | Example |
|-----------|--------|---------|
| **Parse (str → dict)** | `json.loads(json_string)` | `data = json.loads('{"name": "John"}')` |
| **Serialize (dict → str)** | `json.dumps(dict)` | `json_str = json.dumps({"name": "John"})` |
| **Read from file** | `json.load(file)` | `with open('f.json') as f: data = json.load(f)` |
| **Write to file** | `json.dump(data, file)` | `with open('f.json','w') as f: json.dump(data, f)` |
| **Pretty print** | `json.dumps(data, indent=4)` | Formatted with indentation |

### 10.5 Java JSON (using org.json)

| Operation | Method | Example |
|-----------|--------|---------|
| **Parse** | `new JSONObject(string)` | `JSONObject obj = new JSONObject(jsonStr);` |
| **Get value** | `obj.getString("key")` | `String name = obj.getString("name");` |
| **Put value** | `obj.put("key", value)` | `obj.put("age", 30);` |
| **Parse array** | `new JSONArray(string)` | `JSONArray arr = new JSONArray(arrayStr);` |
| **Array element** | `arr.get(index)` | `Object item = arr.get(0);` |

### 10.6 Common JSON Operations

| Operation | Time | Description |
|-----------|------|-------------|
| **Parse** | O(n) | Convert string to object |
| **Serialize** | O(n) | Convert object to string |
| **Access key** | O(1) average | Hash-based lookup |
| **Add/Update** | O(1) average | Modify object |
| **Delete key** | O(1) average | Remove key-value |
| **Iterate** | O(n) | Loop through keys/values |

### 10.7 JSON vs XML

| Feature | JSON | XML |
|---------|------|-----|
| **Readability** | ✅ More readable | Less readable |
| **Data Types** | ✅ Native types | Everything is string |
| **Size** | ✅ Smaller | Larger (tags) |
| **Parsing Speed** | ✅ Faster | Slower |
| **Attributes** | ❌ No attributes | ✅ Has attributes |
| **Comments** | ❌ Not allowed | ✅ Allowed |
| **Use Case** | APIs, Web apps | Config files, SOAP |

### 10.8 Common JSON Patterns

| Pattern | Purpose | Example |
|---------|---------|---------|
| **Nested Objects** | Hierarchical data | `{"user": {"name": "John", "address": {...}}}` |
| **Array of Objects** | List of items | `{"users": [{"name": "John"}, {"name": "Jane"}]}` |
| **Optional Fields** | Nullable data | Check if key exists before access |
| **Flat Structure** | Simple key-value | `{"id": 1, "name": "John"}` |

---

## 11. COMPREHENSIVE COMPARISON

### 11.1 Linear vs Non-Linear

| Type | Examples | Traversal | Memory |
|------|----------|-----------|--------|
| **Linear** | Array, Linked List, Stack, Queue | Sequential | Contiguous (array) / Scattered (LL) |
| **Non-Linear** | Tree, Graph, Heap | Multiple paths | Scattered |

### 11.2 Time Complexity Comparison

| Data Structure | Access | Search | Insert | Delete | Space |
|----------------|--------|--------|--------|--------|-------|
| **Array** | O(1) | O(n) | O(n) | O(n) | O(n) |
| **Linked List** | O(n) | O(n) | O(1)* | O(1)* | O(n) |
| **Stack** | O(n) | O(n) | O(1) | O(1) | O(n) |
| **Queue** | O(n) | O(n) | O(1) | O(1) | O(n) |
| **Hash Table** | - | O(1)† | O(1)† | O(1)† | O(n) |
| **BST** | O(h) | O(h) | O(h) | O(h) | O(n) |
| **Heap** | O(1)‡ | O(n) | O(log n) | O(log n) | O(n) |

*At known position | †Average case | ‡Root only

### 11.3 When to Use What

| Scenario | Best Choice | Reason |
|----------|-------------|--------|
| **Random access needed** | Array | O(1) access |
| **Frequent insert/delete at ends** | Linked List / Deque | O(1) operations |
| **LIFO operations** | Stack | Natural LIFO |
| **FIFO operations** | Queue | Natural FIFO |
| **Priority-based** | Heap / Priority Queue | O(1) max/min |
| **Fast search/insert/delete** | Hash Table | O(1) average |
| **Sorted data** | BST / TreeMap | Maintains order |
| **Hierarchical data** | Tree | Natural hierarchy |
| **2D data** | Matrix | Grid structure |
| **Key-value pairs** | HashMap / Dict | Fast lookup |

---

## 12. CRITICAL EXAM POINTS

### Must Remember - Array

1. ✅ Access: O(1), Insert/Delete: O(n)
2. ✅ Dynamic arrays resize when capacity reached
3. ✅ Cache-friendly (contiguous memory)
4. ✅ Binary search requires sorted array
5. ✅ Two pointers technique for pairs

### Must Remember - Linked List

1. ✅ No random access (O(n) to reach i-th)
2. ✅ Insert/Delete O(1) at known position
3. ✅ Floyd's cycle detection (slow-fast pointers)
4. ✅ Reverse: iterative O(1) space, recursive O(n)
5. ✅ Doubly LL: O(1) delete at end with tail pointer

### Must Remember - Stack

1. ✅ LIFO principle
2. ✅ All operations O(1)
3. ✅ Function call stack uses stack
4. ✅ Monotonic stack for next greater element
5. ✅ Used in DFS, backtracking

### Must Remember - Queue

1. ✅ FIFO principle
2. ✅ Circular queue reuses space
3. ✅ Priority queue: O(log n) operations
4. ✅ Deque: insert/delete both ends
5. ✅ Used in BFS, level order

### Must Remember - Tree

1. ✅ Height h = O(log n) to O(n)
2. ✅ Inorder of BST = sorted
3. ✅ Level order uses queue
4. ✅ Pre/In/Post use stack (iterative)
5. ✅ Complete tree → heap structure

### Must Remember - BST

1. ✅ Left < Root < Right
2. ✅ Operations O(h) - can be O(n) if skewed
3. ✅ Delete with 2 children: replace with successor
4. ✅ AVL/Red-Black maintain O(log n)
5. ✅ Inorder gives sorted sequence

### Must Remember - Heap

1. ✅ Complete binary tree
2. ✅ Parent ≥ children (max) or ≤ (min)
3. ✅ Root = max/min (O(1) access)
4. ✅ Insert/Delete: O(log n)
5. ✅ Build heap: O(n), Heap sort: O(n log n)

### Must Remember - Hashing

1. ✅ Average O(1) for search/insert/delete
2. ✅ Collision resolution: chaining vs open addressing
3. ✅ Load factor α = n/m
4. ✅ HashMap unordered, TreeMap sorted
5. ✅ Hash + Linked List = LRU cache

### Must Remember - Matrix

1. ✅ Access: O(1), Traverse: O(m*n)
2. ✅ Row & col sorted: staircase search O(m+n)
3. ✅ Rotate 90°: transpose + reverse
4. ✅ Sparse matrix: store non-zero only
5. ✅ DFS/BFS for islands, paths

### Must Remember - JSON

1. ✅ Text-based, human-readable
2. ✅ Data types: object, array, string, number, boolean, null
3. ✅ Parse: O(n), Access: O(1) average
4. ✅ Python: json.loads/dumps
5. ✅ Nested objects/arrays allowed

---

## 13. INTERVIEW QUICK CHECKS

### Questions to Ask Yourself

1. **Need fast random access?** → Array
2. **Frequent insert/delete at beginning?** → Linked List
3. **Last-in-first-out?** → Stack
4. **First-in-first-out?** → Queue
5. **Always need max/min?** → Heap
6. **Fast search in unsorted data?** → Hash Table
7. **Need sorted order?** → BST / TreeMap
8. **Hierarchical relationships?** → Tree
9. **Grid/2D data?** → Matrix
10. **Key-value pairs?** → HashMap / Dictionary

---

## 14. FINAL REVISION CHECKLIST

### Before Exam - Confirm You Know:

- [ ] Array: O(1) access, O(n) insert/delete
- [ ] Linked List: Floyd's cycle detection
- [ ] Stack: LIFO, monotonic stack pattern
- [ ] Queue: FIFO, circular queue concept
- [ ] Tree: All traversals (in/pre/post/level)
- [ ] BST: Inorder = sorted, delete 3 cases
- [ ] Heap: Parent-child relationship, O(log n) ops
- [ ] Hashing: O(1) average, collision resolution
- [ ] Matrix: Row-col sorted search technique
- [ ] JSON: Parse/serialize operations
- [ ] Time complexities for all operations
- [ ] When to use which data structure

---

## END OF QUICK REVISION

### 🎯 You're now ready for Data Structures exam!
### ✅ All concepts covered
### 🔥 Good luck!

---

**Last Updated**: Perfect for SEBI Phase 2
**Coverage**: Array to JSON - All 10 Topics
**Time to Review**: 75-90 minutes
