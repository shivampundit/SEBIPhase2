# Data Structures - Complete Revision Guide

## Quick Reference: All Key Concepts, Tables & Formulas

---

## 1. ARRAY

### Time Complexity Table

| Operation | Time Complexity | Notes |
|-----------|----------------|-------|
| **Access** | O(1) | Direct indexing |
| **Search** | O(n) | Linear search unsorted |
| **Search** | O(log n) | Binary search if sorted |
| **Insert at End** | O(1) | Amortized in dynamic arrays |
| **Insert at Position** | O(n) | Shift elements |
| **Delete** | O(n) | Shift elements |
| **Update** | O(1) | Direct access |

### Key Properties
```python
# Array size
length = len(arr)

# Memory location
address(arr[i]) = base_address + i × size_of_element

# 2D Array indexing
# Row-major: address = base + (i × cols + j) × size
# Col-major: address = base + (j × rows + i) × size
```

### Common Operations

```python
# Rotation by k positions
def rotate_array(arr, k):
    n = len(arr)
    k = k % n  # Handle k > n
    return arr[-k:] + arr[:-k]

# Two pointers pattern
def reverse_array(arr):
    left, right = 0, len(arr) - 1
    while left < right:
        arr[left], arr[right] = arr[right], arr[left]
        left += 1
        right -= 1
```

### Sliding Window Pattern
```python
# Fixed window size k
def sliding_window(arr, k):
    window_sum = sum(arr[:k])
    max_sum = window_sum
    
    for i in range(k, len(arr)):
        window_sum = window_sum - arr[i-k] + arr[i]
        max_sum = max(max_sum, window_sum)
    
    return max_sum
```

---

## 2. LINKED LIST

### Time Complexity Comparison

| Operation | Singly | Doubly | Circular | Array |
|-----------|--------|--------|----------|-------|
| **Access** | O(n) | O(n) | O(n) | O(1) |
| **Insert at Head** | O(1) | O(1) | O(1) | O(n) |
| **Insert at Tail** | O(n) | O(1) with tail | O(n) | O(1) |
| **Insert at Position** | O(n) | O(n) | O(n) | O(n) |
| **Delete at Head** | O(1) | O(1) | O(1) | O(n) |
| **Delete at Tail** | O(n) | O(1) with tail | O(n) | O(1) |
| **Delete Node** | O(n) | O(1) with ref | O(n) | O(n) |
| **Search** | O(n) | O(n) | O(n) | O(n) |
| **Space** | O(n) | O(n) 2× | O(n) | O(n) |

### Node Structures

```python
# Singly Linked List
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

# Doubly Linked List
class DNode:
    def __init__(self, data):
        self.data = data
        self.prev = None
        self.next = None
```

### Key Patterns & Algorithms

#### 1. Fast & Slow Pointers (Floyd's Cycle Detection)
```python
def has_cycle(head):
    """
    Detect cycle in linked list
    Time: O(n), Space: O(1)
    """
    slow = fast = head
    
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        
        if slow == fast:
            return True
    
    return False

# Find cycle start
def find_cycle_start(head):
    slow = fast = head
    
    # Find meeting point
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            break
    
    if not fast or not fast.next:
        return None
    
    # Move slow to head, advance both at same speed
    slow = head
    while slow != fast:
        slow = slow.next
        fast = fast.next
    
    return slow
```

#### 2. Reverse Linked List
```python
def reverse_list(head):
    """
    Reverse linked list iteratively
    Time: O(n), Space: O(1)
    """
    prev = None
    current = head
    
    while current:
        next_node = current.next
        current.next = prev
        prev = current
        current = next_node
    
    return prev
```

#### 3. Find Middle Element
```python
def find_middle(head):
    """
    Time: O(n), Space: O(1)
    """
    slow = fast = head
    
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    
    return slow  # For even length, returns 2nd middle
```

#### 4. Merge Two Sorted Lists
```python
def merge_sorted(l1, l2):
    """
    Time: O(n+m), Space: O(1)
    """
    dummy = Node(0)
    current = dummy
    
    while l1 and l2:
        if l1.data <= l2.data:
            current.next = l1
            l1 = l1.next
        else:
            current.next = l2
            l2 = l2.next
        current = current.next
    
    current.next = l1 or l2
    return dummy.next
```

---

## 3. STACK

### Stack Properties
- **LIFO**: Last In First Out
- **Operations**: All O(1)
- **Applications**: Function calls, undo, expression evaluation

### Operation Complexities

| Operation | Time | Space |
|-----------|------|-------|
| **Push** | O(1) | O(1) |
| **Pop** | O(1) | O(1) |
| **Peek** | O(1) | O(1) |
| **IsEmpty** | O(1) | O(1) |
| **Size** | O(1) | O(1) |

### Implementation Comparison

| Method | Pros | Cons |
|--------|------|------|
| **Array** | Cache-friendly, simple | Fixed size (static) |
| **Dynamic Array** | Flexible size | Occasional O(n) resize |
| **Linked List** | No size limit | Extra memory for pointers |

### Classic Stack Problems

#### 1. Balanced Parentheses
```python
def is_balanced(s):
    """
    Time: O(n), Space: O(n)
    """
    stack = []
    pairs = {'(': ')', '[': ']', '{': '}'}
    
    for char in s:
        if char in pairs:
            stack.append(char)
        elif not stack or pairs[stack.pop()] != char:
            return False
    
    return len(stack) == 0
```

#### 2. Infix to Postfix Conversion
```python
def infix_to_postfix(exp):
    """
    Time: O(n), Space: O(n)
    """
    precedence = {'+': 1, '-': 1, '*': 2, '/': 2, '^': 3}
    stack = []
    output = []
    
    for char in exp:
        if char.isalnum():
            output.append(char)
        elif char == '(':
            stack.append(char)
        elif char == ')':
            while stack and stack[-1] != '(':
                output.append(stack.pop())
            stack.pop()  # Remove '('
        else:
            while (stack and stack[-1] != '(' and
                   precedence.get(stack[-1], 0) >= precedence[char]):
                output.append(stack.pop())
            stack.append(char)
    
    while stack:
        output.append(stack.pop())
    
    return ''.join(output)
```

#### 3. Stock Span Problem
```python
def calculate_span(prices):
    """
    Find span for each day (consecutive days with price ≤ current)
    Time: O(n), Space: O(n)
    """
    stack = []
    span = [0] * len(prices)
    
    for i in range(len(prices)):
        while stack and prices[stack[-1]] <= prices[i]:
            stack.pop()
        
        span[i] = i + 1 if not stack else i - stack[-1]
        stack.append(i)
    
    return span
```

#### 4. Next Greater Element
```python
def next_greater(arr):
    """
    Time: O(n), Space: O(n)
    """
    stack = []
    result = [-1] * len(arr)
    
    for i in range(len(arr) - 1, -1, -1):
        while stack and stack[-1] <= arr[i]:
            stack.pop()
        
        if stack:
            result[i] = stack[-1]
        
        stack.append(arr[i])
    
    return result
```

---

## 4. QUEUE

### Queue Properties
- **FIFO**: First In First Out
- **Operations**: All O(1)
- **Applications**: BFS, scheduling, buffering

### Types & Complexities

| Type | Enqueue | Dequeue | Front | Notes |
|------|---------|---------|-------|-------|
| **Simple Queue** | O(1) | O(1) | O(1) | Linear |
| **Circular Queue** | O(1) | O(1) | O(1) | Reuses space |
| **Priority Queue** | O(log n) | O(log n) | O(1) | Heap-based |
| **Deque** | O(1) both ends | O(1) both ends | O(1) | Double-ended |

### Circular Queue Formulas

```python
# Array size: capacity + 1 (to distinguish full/empty)

# Enqueue
rear = (rear + 1) % capacity

# Dequeue
front = (front + 1) % capacity

# Is Full
(rear + 1) % capacity == front

# Is Empty
front == rear

# Size
(rear - front + capacity) % capacity
```

### Deque (Double-Ended Queue)
```python
from collections import deque

dq = deque()

# Operations - all O(1)
dq.append(x)        # Add to right
dq.appendleft(x)    # Add to left
dq.pop()            # Remove from right
dq.popleft()        # Remove from left
```

### Priority Queue
```python
import heapq

# Min heap (default)
pq = []
heapq.heappush(pq, (priority, item))
priority, item = heapq.heappop(pq)

# Max heap (negate priorities)
heapq.heappush(pq, (-priority, item))
```

---

## 5. BINARY TREE

### Tree Properties & Formulas

```python
# For tree with height h:
max_nodes = 2^(h+1) - 1

# For tree with n nodes:
min_height = ceil(log₂(n+1)) - 1
max_height = n - 1

# For full binary tree:
leaf_nodes = internal_nodes + 1

# For complete binary tree with n nodes:
leaf_nodes = ceil(n/2)
internal_nodes = floor(n/2)

# Array representation (0-indexed):
parent(i) = (i-1) // 2
left_child(i) = 2*i + 1
right_child(i) = 2*i + 2
```

### Tree Types Summary

| Type | Definition | Properties |
|------|------------|------------|
| **Full** | Each node has 0 or 2 children | Nodes = 2k+1 (k=internal) |
| **Complete** | All levels filled except last, filled left→right | Height = O(log n) |
| **Perfect** | All internal nodes have 2 children, leaves at same level | Nodes = 2^(h+1)-1 |
| **Balanced** | Height difference ≤ 1 for all nodes | Height = O(log n) |
| **Degenerate** | Each node has max 1 child | Height = O(n) |

### Traversal Complexities

| Traversal | Time | Space | Order |
|-----------|------|-------|-------|
| **Inorder** | O(n) | O(h) | Left-Root-Right |
| **Preorder** | O(n) | O(h) | Root-Left-Right |
| **Postorder** | O(n) | O(h) | Left-Right-Root |
| **Level Order** | O(n) | O(w) | Level by level |

**h** = height, **w** = max width

### Traversal Templates

```python
# Inorder (Left-Root-Right)
def inorder(root):
    if root:
        inorder(root.left)
        print(root.data)
        inorder(root.right)

# Preorder (Root-Left-Right)
def preorder(root):
    if root:
        print(root.data)
        preorder(root.left)
        preorder(root.right)

# Postorder (Left-Right-Root)
def postorder(root):
    if root:
        postorder(root.left)
        postorder(root.right)
        print(root.data)

# Level Order (BFS)
def level_order(root):
    from collections import deque
    if not root:
        return
    
    queue = deque([root])
    while queue:
        node = queue.popleft()
        print(node.data)
        
        if node.left:
            queue.append(node.left)
        if node.right:
            queue.append(node.right)
```

### Key Algorithms

#### 1. Tree Height
```python
def height(root):
    """Time: O(n), Space: O(h)"""
    if not root:
        return -1  # Or 0 depending on definition
    
    return 1 + max(height(root.left), height(root.right))
```

#### 2. Diameter (Longest Path)
```python
def diameter(root):
    """Time: O(n), Space: O(h)"""
    def helper(node):
        if not node:
            return 0, 0  # height, diameter
        
        lh, ld = helper(node.left)
        rh, rd = helper(node.right)
        
        height = 1 + max(lh, rh)
        diameter = max(lh + rh, ld, rd)
        
        return height, diameter
    
    return helper(root)[1]
```

#### 3. Lowest Common Ancestor (LCA)
```python
def lca(root, p, q):
    """Time: O(n), Space: O(h)"""
    if not root or root == p or root == q:
        return root
    
    left = lca(root.left, p, q)
    right = lca(root.right, p, q)
    
    if left and right:
        return root
    
    return left or right
```

#### 4. Check if Balanced
```python
def is_balanced(root):
    """Time: O(n), Space: O(h)"""
    def check(node):
        if not node:
            return 0, True
        
        lh, lb = check(node.left)
        rh, rb = check(node.right)
        
        balanced = lb and rb and abs(lh - rh) <= 1
        height = 1 + max(lh, rh)
        
        return height, balanced
    
    return check(root)[1]
```

---

## 6. BINARY SEARCH TREE (BST)

### BST Property
```
For every node:
- All nodes in left subtree < node
- All nodes in right subtree > node
- Both subtrees are BSTs
```

### Operation Complexities

| Operation | Average | Worst | Balanced (AVL/RB) |
|-----------|---------|-------|-------------------|
| **Search** | O(log n) | O(n) | O(log n) |
| **Insert** | O(log n) | O(n) | O(log n) |
| **Delete** | O(log n) | O(n) | O(log n) |
| **Min/Max** | O(log n) | O(n) | O(log n) |
| **Successor** | O(log n) | O(n) | O(log n) |
| **Space** | O(n) | O(n) | O(n) |

**Worst case**: Skewed tree (degenerates to linked list)

### Key Algorithms

#### 1. Search
```python
def search(root, key):
    """Time: O(h), Space: O(1) iterative"""
    while root:
        if key == root.data:
            return root
        elif key < root.data:
            root = root.left
        else:
            root = root.right
    return None
```

#### 2. Insert
```python
def insert(root, key):
    """Time: O(h), Space: O(h) recursive"""
    if not root:
        return TreeNode(key)
    
    if key < root.data:
        root.left = insert(root.left, key)
    elif key > root.data:
        root.right = insert(root.right, key)
    
    return root
```

#### 3. Delete (Three Cases)
```python
def delete(root, key):
    """Time: O(h), Space: O(h)"""
    if not root:
        return None
    
    if key < root.data:
        root.left = delete(root.left, key)
    elif key > root.data:
        root.right = delete(root.right, key)
    else:
        # Case 1: Leaf or Case 2: One child
        if not root.left:
            return root.right
        if not root.right:
            return root.left
        
        # Case 3: Two children
        # Find inorder successor (min in right subtree)
        successor = find_min(root.right)
        root.data = successor.data
        root.right = delete(root.right, successor.data)
    
    return root

def find_min(root):
    while root.left:
        root = root.left
    return root
```

#### 4. Validate BST
```python
def is_valid_bst(root, min_val=float('-inf'), max_val=float('inf')):
    """Time: O(n), Space: O(h)"""
    if not root:
        return True
    
    if not (min_val < root.data < max_val):
        return False
    
    return (is_valid_bst(root.left, min_val, root.data) and
            is_valid_bst(root.right, root.data, max_val))
```

#### 5. Inorder Successor
```python
def inorder_successor(root, node):
    """Time: O(h), Space: O(1)"""
    # Case 1: Node has right subtree
    if node.right:
        return find_min(node.right)
    
    # Case 2: No right subtree, find ancestor
    successor = None
    while root:
        if node.data < root.data:
            successor = root
            root = root.left
        elif node.data > root.data:
            root = root.right
        else:
            break
    
    return successor
```

### BST from Sorted Array
```python
def sorted_array_to_bst(arr):
    """
    Create balanced BST from sorted array
    Time: O(n), Space: O(log n)
    """
    def helper(left, right):
        if left > right:
            return None
        
        mid = (left + right) // 2
        root = TreeNode(arr[mid])
        
        root.left = helper(left, mid - 1)
        root.right = helper(mid + 1, right)
        
        return root
    
    return helper(0, len(arr) - 1)
```

---

## 7. HEAP

### Heap Properties

**Max Heap**: `parent ≥ children`  
**Min Heap**: `parent ≤ children`

### Array Index Formulas (0-indexed)

```python
parent(i) = (i - 1) // 2
left_child(i) = 2 * i + 1
right_child(i) = 2 * i + 2

# Check if index is valid
has_left = 2*i + 1 < heap_size
has_right = 2*i + 2 < heap_size
```

### Operation Complexities

| Operation | Time | Description |
|-----------|------|-------------|
| **Insert** | O(log n) | Add at end, heapify up |
| **Extract Min/Max** | O(log n) | Remove root, heapify down |
| **Get Min/Max** | O(1) | Return root |
| **Heapify** | O(log n) | Fix single violation |
| **Build Heap** | O(n) | From array |
| **Heap Sort** | O(n log n) | Extract all elements |
| **Search** | O(n) | No ordering within levels |

### Key Algorithms

#### 1. Heapify Up (After Insert)
```python
def heapify_up(heap, index):
    """
    Restore heap property after insertion
    Time: O(log n)
    """
    while index > 0:
        parent = (index - 1) // 2
        
        if heap[index] < heap[parent]:  # Min heap
            heap[index], heap[parent] = heap[parent], heap[index]
            index = parent
        else:
            break
```

#### 2. Heapify Down (After Extract)
```python
def heapify_down(heap, index, heap_size):
    """
    Restore heap property after extraction
    Time: O(log n)
    """
    while True:
        smallest = index
        left = 2 * index + 1
        right = 2 * index + 2
        
        if left < heap_size and heap[left] < heap[smallest]:
            smallest = left
        
        if right < heap_size and heap[right] < heap[smallest]:
            smallest = right
        
        if smallest != index:
            heap[index], heap[smallest] = heap[smallest], heap[index]
            index = smallest
        else:
            break
```

#### 3. Build Heap from Array
```python
def build_heap(arr):
    """
    Convert array to heap
    Time: O(n) - NOT O(n log n)!
    """
    n = len(arr)
    
    # Start from last non-leaf node
    for i in range(n // 2 - 1, -1, -1):
        heapify_down(arr, i, n)
    
    return arr

# Why O(n)?
# Most nodes are leaves (n/2), cost = 0
# Next level (n/4), cost = 1
# Pattern: Σ(nodes_at_level × height) = O(n)
```

#### 4. Heap Sort
```python
def heap_sort(arr):
    """
    Sort using heap
    Time: O(n log n), Space: O(1)
    """
    n = len(arr)
    
    # Build max heap
    for i in range(n // 2 - 1, -1, -1):
        heapify_down(arr, i, n)
    
    # Extract elements one by one
    for i in range(n - 1, 0, -1):
        arr[0], arr[i] = arr[i], arr[0]
        heapify_down(arr, 0, i)
    
    return arr
```

### Heap Applications

| Problem | Heap Type | Time |
|---------|-----------|------|
| **Kth Largest Element** | Min heap of size k | O(n log k) |
| **Kth Smallest Element** | Max heap of size k | O(n log k) |
| **Median in Stream** | Two heaps (max + min) | O(log n) insert |
| **Merge K Sorted** | Min heap of k elements | O(n log k) |
| **Top K Frequent** | Min heap of size k | O(n log k) |

#### K Largest Elements
```python
import heapq

def k_largest(arr, k):
    """
    Time: O(n log k), Space: O(k)
    """
    # Use min heap of size k
    heap = arr[:k]
    heapq.heapify(heap)
    
    for num in arr[k:]:
        if num > heap[0]:
            heapq.heapreplace(heap, num)
    
    return heap
```

#### Median from Stream
```python
class MedianFinder:
    """
    Max heap (left): smaller half
    Min heap (right): larger half
    """
    def __init__(self):
        self.max_heap = []  # Left half (negated)
        self.min_heap = []  # Right half
    
    def add_num(self, num):
        """O(log n)"""
        if not self.max_heap or num <= -self.max_heap[0]:
            heapq.heappush(self.max_heap, -num)
        else:
            heapq.heappush(self.min_heap, num)
        
        # Balance heaps
        if len(self.max_heap) > len(self.min_heap) + 1:
            heapq.heappush(self.min_heap, -heapq.heappop(self.max_heap))
        elif len(self.min_heap) > len(self.max_heap):
            heapq.heappush(self.max_heap, -heapq.heappop(self.min_heap))
    
    def find_median(self):
        """O(1)"""
        if len(self.max_heap) > len(self.min_heap):
            return -self.max_heap[0]
        return (-self.max_heap[0] + self.min_heap[0]) / 2
```

---

## 8. HASHING

### Hash Table Concepts

```python
# Load Factor
α = n / m
# n = number of entries
# m = table size

# Ideal load factor: 0.7 - 0.75
# Too high → more collisions
# Too low → wasted space
```

### Hash Function Methods

| Method | Formula | Example | Use Case |
|--------|---------|---------|----------|
| **Division** | h(k) = k % m | 42 % 10 = 2 | Simple, fast |
| **Multiplication** | h(k) = ⌊m(kA mod 1)⌋ | A ≈ 0.618 | Better distribution |
| **Mid-Square** | h(k) = middle digits of k² | 45² = 2025 → 02 | Numeric keys |
| **Folding** | h(k) = sum of parts | 12345 → 12+34+5 | Large keys |

### Collision Resolution Comparison

| Method | Average | Worst | Space | Pros | Cons |
|--------|---------|-------|-------|------|------|
| **Chaining** | O(1+α) | O(n) | O(n) | Simple, no clustering | Extra space |
| **Linear Probing** | O(1/(1-α)) | O(n) | O(m) | Cache-friendly | Primary clustering |
| **Quadratic Probing** | O(1/(1-α)) | O(n) | O(m) | Less clustering | Secondary clustering |
| **Double Hashing** | O(1/(1-α)) | O(n) | O(m) | Uniform distribution | Two hash functions |

### Collision Resolution Formulas

#### 1. Chaining
```python
# Each slot has linked list
table[hash(key)].append((key, value))

# Expected chain length
E[chain_length] = α = n/m
```

#### 2. Linear Probing
```python
# Probe sequence
h(k, i) = (h(k) + i) % m
# i = 0, 1, 2, 3, ...

# Example: h(k)=5, m=10
# Try: 5, 6, 7, 8, 9, 0, 1, 2, 3, 4
```

#### 3. Quadratic Probing
```python
# Probe sequence
h(k, i) = (h(k) + c₁*i + c₂*i²) % m
# Common: c₁=1, c₂=1

# Example: h(k)=5, m=10
# Try: 5, 6, 9, 4, 1, 0, 1, 4, 9, 6
```

#### 4. Double Hashing
```python
# Probe sequence
h(k, i) = (h₁(k) + i*h₂(k)) % m

# h₂(k) must never be 0
# Common: h₂(k) = R - (k % R), R < m and prime

# Example: h₁(k)=5, h₂(k)=3, m=10
# Try: 5, 8, 1, 4, 7, 0, 3, 6, 9, 2
```

### Perfect Hashing
```python
# Two-level hashing for static sets
# Level 1: n slots, O(n) space
# Level 2: At most c collisions, c² space

# Total space: O(n)
# Lookup: O(1) worst case
# No collisions at level 2
```

### String Hashing - Polynomial Rolling Hash

```python
def polynomial_hash(s, prime=31, mod=10**9+7):
    """
    h(s) = Σ(s[i] × prime^i) mod m
    Time: O(len(s))
    """
    hash_value = 0
    prime_pow = 1
    
    for char in s:
        hash_value = (hash_value + ord(char) * prime_pow) % mod
        prime_pow = (prime_pow * prime) % mod
    
    return hash_value

# Why polynomial hash?
# 1. Different strings → different hashes (low collision)
# 2. Substring hashes computable in O(1)
# 3. Good for pattern matching
```

### Common Hash Table Operations

```python
# Python dict (hash table)
table = {}

# O(1) average operations
table[key] = value      # Insert/Update
value = table[key]      # Access
del table[key]          # Delete
key in table           # Search
table.get(key, default) # Safe access

# Counter - frequency map
from collections import Counter
freq = Counter([1, 2, 2, 3, 3, 3])
# {1: 1, 2: 2, 3: 3}

# DefaultDict - auto-initialize
from collections import defaultdict
graph = defaultdict(list)
graph[1].append(2)  # No KeyError
```

### Hash Table Patterns

#### 1. Two Sum
```python
def two_sum(nums, target):
    """
    Time: O(n), Space: O(n)
    """
    seen = {}
    for i, num in enumerate(nums):
        complement = target - num
        if complement in seen:
            return [seen[complement], i]
        seen[num] = i
```

#### 2. Longest Substring Without Repeating
```python
def length_longest_substring(s):
    """
    Time: O(n), Space: O(min(n, m))
    m = alphabet size
    """
    seen = {}
    max_len = start = 0
    
    for i, char in enumerate(s):
        if char in seen and seen[char] >= start:
            start = seen[char] + 1
        
        seen[char] = i
        max_len = max(max_len, i - start + 1)
    
    return max_len
```

#### 3. Group Anagrams
```python
def group_anagrams(words):
    """
    Time: O(n × k log k), Space: O(n × k)
    n = words, k = avg length
    """
    from collections import defaultdict
    
    groups = defaultdict(list)
    for word in words:
        key = ''.join(sorted(word))
        groups[key].append(word)
    
    return list(groups.values())
```

### Hash vs Balanced BST

| Feature | Hash Table | BST (AVL/RB) |
|---------|-----------|--------------|
| **Search** | O(1) avg | O(log n) |
| **Insert** | O(1) avg | O(log n) |
| **Delete** | O(1) avg | O(log n) |
| **Ordered** | No | Yes |
| **Min/Max** | O(n) | O(log n) |
| **Range Query** | O(n) | O(log n + k) |
| **Space** | O(n) | O(n) |

**Use Hash when**: Need fast lookup, no ordering required  
**Use BST when**: Need ordering, range queries, successor/predecessor

---

## 9. GRAPH REPRESENTATIONS

### Adjacency Matrix

```python
# n × n matrix
# mat[i][j] = 1 if edge i→j exists

# Space: O(n²)
# Add edge: O(1)
# Remove edge: O(1)
# Check edge: O(1)
# Get neighbors: O(n)

# Good for: Dense graphs
```

### Adjacency List

```python
# Array/dict of lists
# graph[u] = [v1, v2, ...] (neighbors of u)

# Space: O(n + e)
# Add edge: O(1)
# Remove edge: O(degree)
# Check edge: O(degree)
# Get neighbors: O(1)

# Good for: Sparse graphs
```

### Edge List

```python
# List of edges: [(u, v, weight), ...]

# Space: O(e)
# Add edge: O(1)
# Sort edges: O(e log e)

# Good for: Kruskal's MST, sorting edges
```

---

## 10. COMMON TIME COMPLEXITIES - QUICK REFERENCE

### Data Structure Operations

| DS | Access | Search | Insert | Delete | Space |
|----|--------|--------|--------|--------|-------|
| **Array** | O(1) | O(n) | O(n) | O(n) | O(n) |
| **Sorted Array** | O(1) | O(log n) | O(n) | O(n) | O(n) |
| **Linked List** | O(n) | O(n) | O(1)* | O(1)* | O(n) |
| **Stack** | O(n) | O(n) | O(1) | O(1) | O(n) |
| **Queue** | O(n) | O(n) | O(1) | O(1) | O(n) |
| **Hash Table** | - | O(1)† | O(1)† | O(1)† | O(n) |
| **BST** | O(log n)† | O(log n)† | O(log n)† | O(log n)† | O(n) |
| **Heap** | O(1)‡ | O(n) | O(log n) | O(log n) | O(n) |
| **Trie** | O(m) | O(m) | O(m) | O(m) | O(alphabet×m×n) |

\* = with node reference  
† = average/balanced case  
‡ = min/max only  
m = key length, n = number of keys

---

## DECISION TREE: CHOOSING THE RIGHT DATA STRUCTURE

### Need O(1) Access by Index?
- ✅ **Array**

### Need Frequent Insertions/Deletions at Ends?
- At one end: **Stack** or **Queue**
- At both ends: **Deque**

### Need Fast Search/Insert/Delete?
- No ordering needed: **Hash Table** (O(1) avg)
- Ordering needed: **BST** (O(log n))
- Range queries: **BST** or **Segment Tree**

### Need to Track Min/Max?
- Static data: **Sorted Array** + binary search
- Dynamic data: **Heap** (O(1) peek, O(log n) insert/delete)
- Both min and max: **Two heaps** or **BST**

### Need to Check Membership?
- Small set: **Array**
- Large set: **Hash Set** (O(1)) or **Bloom Filter** (probabilistic)

### Need LIFO?
- **Stack**

### Need FIFO?
- **Queue** or **Deque**

### Need Priority?
- **Priority Queue** (heap-based)

### Need Hierarchical Data?
- **Tree** (Binary, N-ary, Trie)

### Need Graph Representation?
- Dense: **Adjacency Matrix**
- Sparse: **Adjacency List**

---

## MEMORY & CACHE CONSIDERATIONS

### Array vs Linked List

**Arrays**:
- ✅ Cache-friendly (contiguous memory)
- ✅ Better for sequential access
- ❌ Fixed size (static) or resize overhead (dynamic)

**Linked Lists**:
- ✅ Dynamic size, no waste
- ✅ O(1) insert/delete with pointer
- ❌ Poor cache locality
- ❌ Extra memory for pointers

### Space Complexity Extras

```python
# Node overhead
class Node:
    data      # 8 bytes (64-bit pointer/int)
    next      # 8 bytes
    # Total: 16 bytes per node

# Array
arr = [0] * n  # n × element_size

# Linked List
# n × (element_size + pointer_size)
# Overhead: ~2× compared to array
```

---

## QUICK TIPS & TRICKS

### 1. Fast Exponentiation
```python
# x^n in O(log n)
def power(x, n):
    if n == 0:
        return 1
    half = power(x, n // 2)
    if n % 2 == 0:
        return half * half
    return x * half * half
```

### 2. XOR Properties
```python
# a ^ a = 0
# a ^ 0 = a
# XOR is commutative and associative

# Find single number (all others appear twice)
def single_number(nums):
    result = 0
    for num in nums:
        result ^= num
    return result
```

### 3. Bit Manipulation
```python
# Check if power of 2
(n & (n-1)) == 0 and n != 0

# Count set bits
bin(n).count('1')

# Get ith bit
(n >> i) & 1

# Set ith bit
n | (1 << i)

# Clear ith bit
n & ~(1 << i)
```

### 4. Math Formulas
```python
# Sum of first n numbers
sum = n * (n + 1) // 2

# Sum of squares
sum = n * (n + 1) * (2*n + 1) // 6

# GCD
from math import gcd
g = gcd(a, b)

# LCM
lcm = (a * b) // gcd(a, b)
```

---

## FINAL CHECKLIST FOR INTERVIEWS

### Before Solving:
1. ☐ Understand the problem completely
2. ☐ Ask clarifying questions
3. ☐ Identify input/output format
4. ☐ Consider edge cases
5. ☐ Think about constraints (size, time, space)

### During Solving:
1. ☐ Start with brute force (if needed)
2. ☐ Think out loud
3. ☐ Optimize step by step
4. ☐ Choose appropriate data structure
5. ☐ Consider time/space tradeoffs
6. ☐ Write clean, readable code
7. ☐ Use meaningful variable names

### After Solving:
1. ☐ Trace through example
2. ☐ Test edge cases
3. ☐ Analyze time complexity
4. ☐ Analyze space complexity
5. ☐ Discuss alternative approaches
6. ☐ Identify potential optimizations

### Edge Cases to Consider:
- Empty input
- Single element
- Duplicates
- Negative numbers
- Very large numbers (overflow)
- Special values (0, null, empty string)

---

## END OF REVISION GUIDE

**Remember**: The best data structure depends on your use case. Consider:
- **Access patterns**: Random vs Sequential
- **Operation frequency**: Read-heavy vs Write-heavy
- **Size constraints**: Memory limits
- **Performance requirements**: Time vs Space tradeoffs

**Master the fundamentals, practice patterns, ace the interviews! 🚀**
