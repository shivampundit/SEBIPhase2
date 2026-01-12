# Binary Tree - Comprehensive Notes

## Overview
A Binary Tree is a hierarchical data structure where each node has at most two children, referred to as left child and right child.

### Key Terminology
- **Root**: Top node with no parent
- **Parent**: Node with children
- **Child**: Node descended from another node
- **Leaf**: Node with no children
- **Internal Node**: Node with at least one child
- **Depth**: Distance from root to node
- **Height**: Distance from node to deepest leaf
- **Level**: All nodes at same depth

### Properties
1. Maximum nodes at level i: 2^i
2. Maximum nodes in tree of height h: 2^(h+1) - 1
3. Minimum height for n nodes: log₂(n+1) - 1

---

## Types of Binary Trees

### 1. Full Binary Tree
Every node has 0 or 2 children (no node has 1 child)

```
       1
      / \
     2   3
    / \
   4   5
```

### 2. Complete Binary Tree
All levels filled except possibly last, filled from left to right

```
       1
      / \
     2   3
    / \  /
   4  5 6
```

### 3. Perfect Binary Tree
All internal nodes have 2 children, all leaves at same level

```
       1
      / \
     2   3
    / \ / \
   4  5 6  7
```

### 4. Balanced Binary Tree
Height difference between left and right subtrees ≤ 1 for all nodes

### 5. Degenerate/Skewed Tree
Each parent has only one child (like linked list)

```
1
 \
  2
   \
    3
```

---

## Node Structure

```python
class TreeNode:
    def __init__(self, data):
        self.data = data
        self.left = None
        self.right = None

class BinaryTree:
    def __init__(self):
        self.root = None
```

---

## Tree Traversals

### 1. Inorder Traversal (Left-Root-Right)

```python
def inorder(root):
    """
    Inorder: Left → Root → Right
    Time: O(n)
    Space: O(h) for recursion stack
    """
    if root:
        inorder(root.left)
        print(root.data, end=" ")
        inorder(root.right)

# Iterative using stack
def inorder_iterative(root):
    """
    Iterative inorder traversal
    """
    stack = []
    current = root
    
    while stack or current:
        if current:
            stack.append(current)
            current = current.left
        else:
            current = stack.pop()
            print(current.data, end=" ")
            current = current.right
```

#### Dry Run
```
Tree:
       1
      / \
     2   3
    / \
   4   5

Inorder: 4 2 5 1 3

Recursive calls:
inorder(1)
  inorder(2)
    inorder(4)
      inorder(None) - return
      print 4
      inorder(None) - return
    print 2
    inorder(5)
      inorder(None) - return
      print 5
      inorder(None) - return
  print 1
  inorder(3)
    inorder(None) - return
    print 3
    inorder(None) - return

Output: 4 2 5 1 3
```

---

### 2. Preorder Traversal (Root-Left-Right)

```python
def preorder(root):
    """
    Preorder: Root → Left → Right
    Time: O(n)
    Space: O(h)
    """
    if root:
        print(root.data, end=" ")
        preorder(root.left)
        preorder(root.right)

# Iterative
def preorder_iterative(root):
    """
    Iterative preorder traversal
    """
    if not root:
        return
    
    stack = [root]
    
    while stack:
        node = stack.pop()
        print(node.data, end=" ")
        
        if node.right:
            stack.append(node.right)
        if node.left:
            stack.append(node.left)
```

Output for same tree: `1 2 4 5 3`

---

### 3. Postorder Traversal (Left-Right-Root)

```python
def postorder(root):
    """
    Postorder: Left → Right → Root
    Time: O(n)
    Space: O(h)
    """
    if root:
        postorder(root.left)
        postorder(root.right)
        print(root.data, end=" ")

# Iterative
def postorder_iterative(root):
    """
    Iterative postorder using two stacks
    """
    if not root:
        return
    
    stack1 = [root]
    stack2 = []
    
    while stack1:
        node = stack1.pop()
        stack2.append(node)
        
        if node.left:
            stack1.append(node.left)
        if node.right:
            stack1.append(node.right)
    
    while stack2:
        print(stack2.pop().data, end=" ")
```

Output for same tree: `4 5 2 3 1`

---

### 4. Level Order Traversal (BFS)

```python
from collections import deque

def level_order(root):
    """
    Level order traversal (BFS)
    Time: O(n)
    Space: O(w) where w is max width
    """
    if not root:
        return
    
    queue = deque([root])
    
    while queue:
        node = queue.popleft()
        print(node.data, end=" ")
        
        if node.left:
            queue.append(node.left)
        if node.right:
            queue.append(node.right)

# Level by level
def level_order_by_level(root):
    """
    Print each level on separate line
    """
    if not root:
        return
    
    queue = deque([root])
    
    while queue:
        level_size = len(queue)
        
        for _ in range(level_size):
            node = queue.popleft()
            print(node.data, end=" ")
            
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        print()  # New line after each level
```

Output: `1 2 3 4 5`

#### Dry Run
```
Tree:
       1
      / \
     2   3
    / \
   4   5

Initial: queue = [1]

Step 1: Dequeue 1, print 1, enqueue 2, 3
        queue = [2, 3]
        Output: 1

Step 2: Dequeue 2, print 2, enqueue 4, 5
        queue = [3, 4, 5]
        Output: 1 2

Step 3: Dequeue 3, print 3, no children
        queue = [4, 5]
        Output: 1 2 3

Step 4: Dequeue 4, print 4, no children
        queue = [5]
        Output: 1 2 3 4

Step 5: Dequeue 5, print 5, no children
        queue = []
        Output: 1 2 3 4 5
```

---

## Common Binary Tree Operations

### 1. Insert Node (Level Order)

```python
def insert(root, data):
    """
    Insert node in level order fashion
    Time: O(n)
    Space: O(w)
    """
    new_node = TreeNode(data)
    
    if not root:
        return new_node
    
    queue = deque([root])
    
    while queue:
        node = queue.popleft()
        
        if not node.left:
            node.left = new_node
            return root
        else:
            queue.append(node.left)
        
        if not node.right:
            node.right = new_node
            return root
        else:
            queue.append(node.right)
    
    return root
```

---

### 2. Delete Node

```python
def delete_deepest(root, d_node):
    """Delete deepest rightmost node"""
    queue = deque([root])
    
    while queue:
        node = queue.popleft()
        
        if node is d_node:
            node = None
            return
        
        if node.right:
            if node.right is d_node:
                node.right = None
                return
            else:
                queue.append(node.right)
        
        if node.left:
            if node.left is d_node:
                node.left = None
                return
            else:
                queue.append(node.left)

def delete_node(root, key):
    """
    Delete node with given key
    Time: O(n)
    """
    if not root:
        return None
    
    if not root.left and not root.right:
        if root.data == key:
            return None
        return root
    
    key_node = None
    queue = deque([root])
    temp = None
    
    while queue:
        temp = queue.popleft()
        
        if temp.data == key:
            key_node = temp
        
        if temp.left:
            queue.append(temp.left)
        
        if temp.right:
            queue.append(temp.right)
    
    if key_node:
        key_node.data = temp.data
        delete_deepest(root, temp)
    
    return root
```

---

### 3. Height of Tree

```python
def height(root):
    """
    Calculate height of tree
    Time: O(n)
    Space: O(h)
    """
    if not root:
        return -1
    
    left_height = height(root.left)
    right_height = height(root.right)
    
    return max(left_height, right_height) + 1

# Iterative using level order
def height_iterative(root):
    """
    Calculate height iteratively
    """
    if not root:
        return -1
    
    queue = deque([root])
    height = -1
    
    while queue:
        level_size = len(queue)
        height += 1
        
        for _ in range(level_size):
            node = queue.popleft()
            
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
    
    return height
```

---

### 4. Count Nodes

```python
def count_nodes(root):
    """
    Count total nodes
    Time: O(n)
    Space: O(h)
    """
    if not root:
        return 0
    
    return 1 + count_nodes(root.left) + count_nodes(root.right)

# Count leaf nodes
def count_leaf_nodes(root):
    """
    Count leaf nodes
    """
    if not root:
        return 0
    
    if not root.left and not root.right:
        return 1
    
    return count_leaf_nodes(root.left) + count_leaf_nodes(root.right)
```

---

### 5. Maximum Element

```python
def find_max(root):
    """
    Find maximum element in tree
    Time: O(n)
    Space: O(h)
    """
    if not root:
        return float('-inf')
    
    left_max = find_max(root.left)
    right_max = find_max(root.right)
    
    return max(root.data, left_max, right_max)
```

---

### 6. Mirror/Invert Tree

```python
def mirror_tree(root):
    """
    Mirror the binary tree
    Time: O(n)
    Space: O(h)
    """
    if not root:
        return None
    
    # Swap left and right
    root.left, root.right = root.right, root.left
    
    # Recursively mirror subtrees
    mirror_tree(root.left)
    mirror_tree(root.right)
    
    return root
```

---

### 7. Diameter of Tree

```python
def diameter(root):
    """
    Find diameter (longest path between any two nodes)
    Time: O(n)
    Space: O(h)
    """
    def helper(node):
        if not node:
            return 0, 0  # height, diameter
        
        left_height, left_diameter = helper(node.left)
        right_height, right_diameter = helper(node.right)
        
        current_height = max(left_height, right_height) + 1
        current_diameter = max(
            left_height + right_height,
            left_diameter,
            right_diameter
        )
        
        return current_height, current_diameter
    
    return helper(root)[1]
```

---

### 8. Lowest Common Ancestor (LCA)

```python
def lowest_common_ancestor(root, p, q):
    """
    Find LCA of two nodes
    Time: O(n)
    Space: O(h)
    """
    if not root or root == p or root == q:
        return root
    
    left = lowest_common_ancestor(root.left, p, q)
    right = lowest_common_ancestor(root.right, p, q)
    
    if left and right:
        return root
    
    return left if left else right
```

---

### 9. Check if Balanced

```python
def is_balanced(root):
    """
    Check if tree is balanced
    Time: O(n)
    Space: O(h)
    """
    def check_height(node):
        if not node:
            return 0
        
        left_height = check_height(node.left)
        if left_height == -1:
            return -1
        
        right_height = check_height(node.right)
        if right_height == -1:
            return -1
        
        if abs(left_height - right_height) > 1:
            return -1
        
        return max(left_height, right_height) + 1
    
    return check_height(root) != -1
```

---

### 10. Path Sum

```python
def has_path_sum(root, target_sum):
    """
    Check if path with given sum exists
    Time: O(n)
    Space: O(h)
    """
    if not root:
        return False
    
    if not root.left and not root.right:
        return root.data == target_sum
    
    return (has_path_sum(root.left, target_sum - root.data) or
            has_path_sum(root.right, target_sum - root.data))

# Find all paths with sum
def find_all_paths(root, target_sum):
    """
    Find all root-to-leaf paths with target sum
    """
    def helper(node, remaining, path, result):
        if not node:
            return
        
        path.append(node.data)
        
        if not node.left and not node.right and remaining == node.data:
            result.append(path[:])
        
        helper(node.left, remaining - node.data, path, result)
        helper(node.right, remaining - node.data, path, result)
        
        path.pop()
    
    result = []
    helper(root, target_sum, [], result)
    return result
```

---

### 11. Vertical Order Traversal

```python
from collections import defaultdict, deque

def vertical_order(root):
    """
    Vertical order traversal
    Time: O(n log n)
    Space: O(n)
    """
    if not root:
        return []
    
    column_table = defaultdict(list)
    queue = deque([(root, 0)])
    
    while queue:
        node, column = queue.popleft()
        column_table[column].append(node.data)
        
        if node.left:
            queue.append((node.left, column - 1))
        if node.right:
            queue.append((node.right, column + 1))
    
    return [column_table[x] for x in sorted(column_table.keys())]
```

---

### 12. Serialize and Deserialize

```python
def serialize(root):
    """
    Serialize tree to string
    Time: O(n)
    """
    def helper(node):
        if not node:
            return "null,"
        
        return str(node.data) + "," + helper(node.left) + helper(node.right)
    
    return helper(root)

def deserialize(data):
    """
    Deserialize string to tree
    Time: O(n)
    """
    def helper(nodes):
        val = next(nodes)
        
        if val == "null":
            return None
        
        node = TreeNode(int(val))
        node.left = helper(nodes)
        node.right = helper(nodes)
        
        return node
    
    nodes = iter(data.split(","))
    return helper(nodes)
```

---

## Tree Construction

### 1. From Inorder and Preorder

```python
def build_tree_inorder_preorder(inorder, preorder):
    """
    Construct tree from inorder and preorder
    Time: O(n)
    Space: O(n)
    """
    if not inorder or not preorder:
        return None
    
    root_val = preorder[0]
    root = TreeNode(root_val)
    
    mid = inorder.index(root_val)
    
    root.left = build_tree_inorder_preorder(
        inorder[:mid],
        preorder[1:mid+1]
    )
    root.right = build_tree_inorder_preorder(
        inorder[mid+1:],
        preorder[mid+1:]
    )
    
    return root
```

### 2. From Inorder and Postorder

```python
def build_tree_inorder_postorder(inorder, postorder):
    """
    Construct tree from inorder and postorder
    """
    if not inorder or not postorder:
        return None
    
    root_val = postorder[-1]
    root = TreeNode(root_val)
    
    mid = inorder.index(root_val)
    
    root.left = build_tree_inorder_postorder(
        inorder[:mid],
        postorder[:mid]
    )
    root.right = build_tree_inorder_postorder(
        inorder[mid+1:],
        postorder[mid:-1]
    )
    
    return root
```

---

## Time Complexity Summary

| Operation | Time | Space |
|-----------|------|-------|
| Insertion | O(n) | O(w) |
| Deletion | O(n) | O(w) |
| Search | O(n) | O(h) |
| Traversal | O(n) | O(h) |
| Height | O(n) | O(h) |

where w = max width, h = height

---

## Exam Tips

### 1. Traversal Outputs
```
Tree:    1
        / \
       2   3
      / \
     4   5

Inorder: 4 2 5 1 3
Preorder: 1 2 4 5 3
Postorder: 4 5 2 3 1
Level Order: 1 2 3 4 5
```

### 2. MCQ Questions
**Q**: Inorder traversal of BST gives?
**A**: Sorted sequence

**Q**: Which traversal uses queue?
**A**: Level Order (BFS)

**Q**: Which traversal uses stack?
**A**: Preorder, Inorder, Postorder (DFS)

### 3. Common Patterns
- **Recursion**: Natural for trees
- **Level Order**: Use queue
- **DFS**: Use stack or recursion
- **Two pointers**: For LCA, paths

### Key Points
1. **Traversals**: Inorder, Preorder, Postorder, Level Order
2. **Height**: Longest path from root to leaf
3. **Balanced**: |left_height - right_height| ≤ 1
4. **Complete**: All levels filled except possibly last
5. **Full**: Every node has 0 or 2 children
6. **Perfect**: All leaves at same level
7. **BST Inorder**: Gives sorted sequence
