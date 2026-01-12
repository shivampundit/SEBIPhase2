# Binary Search Tree (BST) - Comprehensive Notes

## Overview
A Binary Search Tree is a binary tree with the BST property: for each node, all values in left subtree are smaller and all values in right subtree are greater.

### BST Property
For every node:
- **Left subtree** < **Node value** < **Right subtree**
- This property must hold for all nodes

### Example BST
```
       50
      /  \
    30    70
   / \    / \
  20 40  60 80
```

Valid BST: Inorder traversal gives **sorted sequence**
Inorder: 20, 30, 40, 50, 60, 70, 80

---

## BST Node Structure

```python
class TreeNode:
    def __init__(self, data):
        self.data = data
        self.left = None
        self.right = None

class BST:
    def __init__(self):
        self.root = None
```

---

## BST Operations

### 1. Search in BST

```python
def search(root, key):
    """
    Search for key in BST
    Time: O(h) where h is height
    Best: O(log n) for balanced
    Worst: O(n) for skewed
    Space: O(h) for recursion
    """
    if not root or root.data == key:
        return root
    
    if key < root.data:
        return search(root.left, key)
    else:
        return search(root.right, key)

# Iterative
def search_iterative(root, key):
    """
    Iterative search
    Space: O(1)
    """
    current = root
    
    while current:
        if key == current.data:
            return current
        elif key < current.data:
            current = current.left
        else:
            current = current.right
    
    return None
```

#### Dry Run
```
BST:       50
          /  \
        30    70
       / \    / \
      20 40  60 80

Search for 40:
Step 1: current = 50, 40 < 50, go left
Step 2: current = 30, 40 > 30, go right
Step 3: current = 40, 40 == 40, found!

Search for 65:
Step 1: current = 50, 65 > 50, go right
Step 2: current = 70, 65 < 70, go left
Step 3: current = 60, 65 > 60, go right
Step 4: current = None, not found
```

---

### 2. Insert in BST

```python
def insert(root, key):
    """
    Insert key into BST
    Time: O(h)
    Space: O(h) for recursion
    """
    if not root:
        return TreeNode(key)
    
    if key < root.data:
        root.left = insert(root.left, key)
    elif key > root.data:
        root.right = insert(root.right, key)
    
    return root

# Iterative
def insert_iterative(root, key):
    """
    Iterative insertion
    Space: O(1)
    """
    new_node = TreeNode(key)
    
    if not root:
        return new_node
    
    current = root
    parent = None
    
    while current:
        parent = current
        if key < current.data:
            current = current.left
        else:
            current = current.right
    
    if key < parent.data:
        parent.left = new_node
    else:
        parent.right = new_node
    
    return root
```

#### Dry Run
```
Insert 45 into:
       50
      /  \
    30    70
   / \
  20 40

Step 1: 45 < 50, go left to 30
Step 2: 45 > 30, go right to 40
Step 3: 45 > 40, go right (null)
Step 4: Insert 45 as right child of 40

Result:
       50
      /  \
    30    70
   / \
  20 40
      \
      45
```

---

### 3. Delete from BST

```python
def delete_node(root, key):
    """
    Delete node with given key
    Time: O(h)
    Space: O(h)
    
    Three cases:
    1. Node has no children (leaf)
    2. Node has one child
    3. Node has two children
    """
    if not root:
        return None
    
    # Search for the node
    if key < root.data:
        root.left = delete_node(root.left, key)
    elif key > root.data:
        root.right = delete_node(root.right, key)
    else:
        # Node found
        
        # Case 1: Leaf node
        if not root.left and not root.right:
            return None
        
        # Case 2: One child
        if not root.left:
            return root.right
        if not root.right:
            return root.left
        
        # Case 3: Two children
        # Find inorder successor (min in right subtree)
        successor = find_min(root.right)
        root.data = successor.data
        root.right = delete_node(root.right, successor.data)
    
    return root

def find_min(root):
    """Find minimum value node"""
    while root.left:
        root = root.left
    return root

def find_max(root):
    """Find maximum value node"""
    while root.right:
        root = root.right
    return root
```

#### Dry Run
```
Delete 30 from:
       50
      /  \
    30    70
   / \    / \
  20 40  60 80

Node 30 has two children:
1. Find inorder successor: min in right subtree
   Right subtree of 30 is 40
   Min of 40 is 40 itself
   
2. Replace 30 with 40
       50
      /  \
    40    70
   /     / \
  20    60 80

3. Delete original 40 (which had no children)

Final:
       50
      /  \
    40    70
   /     / \
  20    60 80
```

---

### 4. Validate BST

```python
def is_valid_bst(root):
    """
    Check if tree is valid BST
    Time: O(n)
    Space: O(h)
    """
    def validate(node, min_val, max_val):
        if not node:
            return True
        
        if node.data <= min_val or node.data >= max_val:
            return False
        
        return (validate(node.left, min_val, node.data) and
                validate(node.right, node.data, max_val))
    
    return validate(root, float('-inf'), float('inf'))

# Using inorder traversal
def is_valid_bst_inorder(root):
    """
    Validate using inorder traversal
    Inorder of BST should be sorted
    """
    def inorder(node, prev):
        if not node:
            return True
        
        if not inorder(node.left, prev):
            return False
        
        if prev[0] is not None and node.data <= prev[0]:
            return False
        
        prev[0] = node.data
        
        return inorder(node.right, prev)
    
    return inorder(root, [None])
```

#### Dry Run
```
Validate:
       10
      /  \
     5    15
         /  \
        6   20

Check node 10: -∞ < 10 < ∞ ✓
Check node 5: -∞ < 5 < 10 ✓
Check node 15: 10 < 15 < ∞ ✓
Check node 6: 10 < 6 < 15 ✗

NOT a valid BST (6 is in right subtree but < 10)
```

---

### 5. Kth Smallest Element

```python
def kth_smallest(root, k):
    """
    Find kth smallest element in BST
    Time: O(h + k)
    Space: O(h)
    
    Method: Inorder traversal gives sorted order
    """
    def inorder(node):
        if not node:
            return None
        
        # Traverse left
        result = inorder(node.left)
        if result is not None:
            return result
        
        # Process current
        self.count += 1
        if self.count == k:
            return node.data
        
        # Traverse right
        return inorder(node.right)
    
    self.count = 0
    return inorder(root)

# Iterative
def kth_smallest_iterative(root, k):
    """
    Iterative approach using stack
    """
    stack = []
    current = root
    count = 0
    
    while stack or current:
        while current:
            stack.append(current)
            current = current.left
        
        current = stack.pop()
        count += 1
        
        if count == k:
            return current.data
        
        current = current.right
    
    return None
```

---

### 6. Lowest Common Ancestor in BST

```python
def lowest_common_ancestor_bst(root, p, q):
    """
    Find LCA in BST
    Time: O(h)
    Space: O(1) iterative, O(h) recursive
    
    Uses BST property:
    - If both p, q < root, LCA in left
    - If both p, q > root, LCA in right
    - Otherwise, root is LCA
    """
    if not root:
        return None
    
    # Both in left subtree
    if p.data < root.data and q.data < root.data:
        return lowest_common_ancestor_bst(root.left, p, q)
    
    # Both in right subtree
    if p.data > root.data and q.data > root.data:
        return lowest_common_ancestor_bst(root.right, p, q)
    
    # Split point
    return root

# Iterative
def lca_bst_iterative(root, p, q):
    """
    Iterative LCA
    """
    while root:
        if p.data < root.data and q.data < root.data:
            root = root.left
        elif p.data > root.data and q.data > root.data:
            root = root.right
        else:
            return root
    
    return None
```

---

### 7. Convert Sorted Array to BST

```python
def sorted_array_to_bst(nums):
    """
    Convert sorted array to balanced BST
    Time: O(n)
    Space: O(log n) for recursion
    
    Method: Choose middle as root
    """
    if not nums:
        return None
    
    mid = len(nums) // 2
    root = TreeNode(nums[mid])
    
    root.left = sorted_array_to_bst(nums[:mid])
    root.right = sorted_array_to_bst(nums[mid+1:])
    
    return root
```

#### Dry Run
```
Array: [1, 2, 3, 4, 5, 6, 7]

Step 1: mid = 3, root = 4
        Array: [1,2,3] | 4 | [5,6,7]

Step 2: Left subtree from [1,2,3]
        mid = 1, node = 2
        [1] | 2 | [3]

Step 3: Right subtree from [5,6,7]
        mid = 1, node = 6
        [5] | 6 | [7]

Result:
       4
      / \
     2   6
    / \ / \
   1  3 5  7
```

---

### 8. BST Iterator

```python
class BSTIterator:
    """
    BST iterator with next() and hasNext()
    Time: O(1) average per call
    Space: O(h)
    """
    def __init__(self, root):
        self.stack = []
        self._push_left(root)
    
    def _push_left(self, node):
        while node:
            self.stack.append(node)
            node = node.left
    
    def next(self):
        """Return next smallest element"""
        node = self.stack.pop()
        self._push_left(node.right)
        return node.data
    
    def hasNext(self):
        """Check if next element exists"""
        return len(self.stack) > 0

# Usage
# iterator = BSTIterator(root)
# while iterator.hasNext():
#     print(iterator.next())
```

---

### 9. Range Sum Query

```python
def range_sum_bst(root, low, high):
    """
    Sum of values in range [low, high]
    Time: O(n)
    Space: O(h)
    """
    if not root:
        return 0
    
    if root.data < low:
        return range_sum_bst(root.right, low, high)
    
    if root.data > high:
        return range_sum_bst(root.left, low, high)
    
    return (root.data +
            range_sum_bst(root.left, low, high) +
            range_sum_bst(root.right, low, high))
```

---

### 10. Two Sum in BST

```python
def find_target(root, k):
    """
    Check if two elements sum to k
    Time: O(n)
    Space: O(n)
    
    Method 1: Inorder + Two Pointers
    """
    def inorder(node, nums):
        if not node:
            return
        inorder(node.left, nums)
        nums.append(node.data)
        inorder(node.right, nums)
    
    nums = []
    inorder(root, nums)
    
    left, right = 0, len(nums) - 1
    
    while left < right:
        current_sum = nums[left] + nums[right]
        
        if current_sum == k:
            return True
        elif current_sum < k:
            left += 1
        else:
            right -= 1
    
    return False

# Method 2: Using HashSet
def find_target_set(root, k):
    """
    Using HashSet
    Space: O(n)
    """
    def search(node, seen):
        if not node:
            return False
        
        complement = k - node.data
        if complement in seen:
            return True
        
        seen.add(node.data)
        
        return search(node.left, seen) or search(node.right, seen)
    
    return search(root, set())
```

---

### 11. Trim BST

```python
def trim_bst(root, low, high):
    """
    Remove nodes outside range [low, high]
    Time: O(n)
    Space: O(h)
    """
    if not root:
        return None
    
    if root.data < low:
        return trim_bst(root.right, low, high)
    
    if root.data > high:
        return trim_bst(root.left, low, high)
    
    root.left = trim_bst(root.left, low, high)
    root.right = trim_bst(root.right, low, high)
    
    return root
```

---

### 12. BST to Greater Sum Tree

```python
def bst_to_gst(root):
    """
    Convert BST to Greater Sum Tree
    Each node's value = original value + sum of all greater values
    Time: O(n)
    Space: O(h)
    
    Method: Reverse inorder (right-root-left)
    """
    def reverse_inorder(node, total):
        if not node:
            return total
        
        # Process right first
        total = reverse_inorder(node.right, total)
        
        # Update current node
        total += node.data
        node.data = total
        
        # Process left
        total = reverse_inorder(node.left, total)
        
        return total
    
    reverse_inorder(root, 0)
    return root
```

#### Dry Run
```
Original BST:
       4
      / \
     1   6
    / \ / \
   0  2 5  7
        \   \
         3   8

Reverse inorder: 8, 7, 6, 5, 4, 3, 2, 1, 0
Running sum:
8
8+7=15
15+6=21
21+5=26
26+4=30
30+3=33
33+2=35
35+1=36
36+0=36

Result:
       30
      /  \
    36    21
    / \   / \
  36 35 26 15
       \    \
       33    8
```

---

## BST Properties

### 1. Inorder Traversal
```python
# Inorder of BST is always SORTED
def inorder(root):
    if root:
        inorder(root.left)
        print(root.data, end=" ")
        inorder(root.right)
```

### 2. Predecessor and Successor

```python
def inorder_predecessor(root, key):
    """
    Find inorder predecessor (largest value < key)
    """
    predecessor = None
    current = root
    
    while current:
        if key > current.data:
            predecessor = current
            current = current.right
        else:
            current = current.left
    
    return predecessor

def inorder_successor(root, key):
    """
    Find inorder successor (smallest value > key)
    """
    successor = None
    current = root
    
    while current:
        if key < current.data:
            successor = current
            current = current.left
        else:
            current = current.right
    
    return successor
```

---

## Time Complexity Comparison

| Operation | Average | Worst (Skewed) | Best (Balanced) |
|-----------|---------|----------------|-----------------|
| Search | O(h) | O(n) | O(log n) |
| Insert | O(h) | O(n) | O(log n) |
| Delete | O(h) | O(n) | O(log n) |
| Find Min/Max | O(h) | O(n) | O(log n) |
| Inorder | O(n) | O(n) | O(n) |

where h = height of tree

### Balanced vs Skewed

**Balanced BST** (h = log n):
```
       50
      /  \
    30    70
   / \    / \
  20 40  60 80
```

**Skewed BST** (h = n):
```
10
 \
  20
   \
    30
     \
      40
       \
        50
```

---

## BST vs Binary Tree

| Feature | Binary Tree | BST |
|---------|-------------|-----|
| Order | No specific order | Left < Root < Right |
| Search | O(n) | O(h) average |
| Inorder | Any order | Sorted |
| Use Case | Hierarchical data | Ordered data, lookups |

---

## Exam Tips

### 1. Quick Identification
```
Is this a valid BST?

       10
      /  \
     5    15
         /  \
        12   20

Inorder: 5, 10, 12, 15, 20
Sorted? YES → Valid BST ✓
```

### 2. Common Mistakes
```
NOT a valid BST:
       10
      /  \
     5    15
         /  \
        6   20

Why? 6 < 10 but in right subtree
```

### 3. MCQ Patterns

**Q**: Inorder traversal of BST gives?
**A**: Sorted sequence in ascending order

**Q**: Time to search in balanced BST?
**A**: O(log n)

**Q**: Worst case search time in BST?
**A**: O(n) when tree is skewed

**Q**: Which operation uses BST property most efficiently?
**A**: Search (can eliminate half the tree)

**Q**: To delete a node with two children?
**A**: Replace with inorder successor or predecessor

### 4. Debugging Tips
- Always check: Left < Root < Right for ALL nodes
- Validate range: Use min/max boundaries
- Inorder test: Should produce sorted sequence
- Height matters: O(h) operations

### Key Points
1. **BST Property**: Left < Root < Right
2. **Inorder**: Always sorted in BST
3. **Search**: O(log n) balanced, O(n) skewed
4. **Deletion**: Three cases (0, 1, 2 children)
5. **Validation**: Check range constraints
6. **LCA**: Uses BST property for efficient search
7. **Balance**: Affects all operation complexities
8. **Applications**: Databases, file systems, symbol tables
