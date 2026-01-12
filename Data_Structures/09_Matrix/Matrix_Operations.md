# Matrix Operations - Comprehensive Notes

## Overview
A matrix is a 2D array organized in rows and columns. Essential for grid-based problems, images, graphs, and mathematical computations.

### Matrix Representation

```python
# Matrix representation in Python
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

# Access element
element = matrix[i][j]  # Row i, Column j

# Dimensions
rows = len(matrix)
cols = len(matrix[0])

# Create matrix
n, m = 3, 4
matrix = [[0] * m for _ in range(n)]  # 3x4 matrix of zeros
```

---

## Basic Matrix Operations

### 1. Matrix Traversal

```python
def traverse_matrix(matrix):
    """
    Row-wise traversal
    Time: O(m*n)
    Space: O(1)
    """
    rows, cols = len(matrix), len(matrix[0])
    
    # Row-wise
    for i in range(rows):
        for j in range(cols):
            print(matrix[i][j], end=" ")
        print()

def traverse_column_wise(matrix):
    """Column-wise traversal"""
    rows, cols = len(matrix), len(matrix[0])
    
    for j in range(cols):
        for i in range(rows):
            print(matrix[i][j], end=" ")
        print()

def traverse_diagonal(matrix):
    """Diagonal traversal"""
    n = len(matrix)
    
    # Main diagonal
    for i in range(n):
        print(matrix[i][i], end=" ")
    print()
    
    # Anti-diagonal
    for i in range(n):
        print(matrix[i][n-1-i], end=" ")
```

---

### 2. Spiral Matrix Traversal

```python
def spiral_order(matrix):
    """
    Traverse matrix in spiral order
    Time: O(m*n)
    Space: O(1)
    """
    if not matrix:
        return []
    
    result = []
    top, bottom = 0, len(matrix) - 1
    left, right = 0, len(matrix[0]) - 1
    
    while top <= bottom and left <= right:
        # Traverse right
        for col in range(left, right + 1):
            result.append(matrix[top][col])
        top += 1
        
        # Traverse down
        for row in range(top, bottom + 1):
            result.append(matrix[row][right])
        right -= 1
        
        # Traverse left (if still valid)
        if top <= bottom:
            for col in range(right, left - 1, -1):
                result.append(matrix[bottom][col])
            bottom -= 1
        
        # Traverse up (if still valid)
        if left <= right:
            for row in range(bottom, top - 1, -1):
                result.append(matrix[row][left])
            left += 1
    
    return result
```

#### Dry Run
```
Matrix:
1  2  3  4
5  6  7  8
9  10 11 12

Initial: top=0, bottom=2, left=0, right=3

Step 1: Traverse right (row 0)
        result = [1, 2, 3, 4]
        top = 1

Step 2: Traverse down (col 3)
        result = [1,2,3,4, 8,12]
        right = 2

Step 3: Traverse left (row 2)
        result = [1,2,3,4,8,12, 11,10,9]
        bottom = 1

Step 4: Traverse up (col 0)
        result = [1,2,3,4,8,12,11,10,9, 5]
        left = 1

Step 5: Traverse right (row 1)
        result = [1,2,3,4,8,12,11,10,9,5, 6,7]
        top = 2

top > bottom, stop

Result: [1,2,3,4,8,12,11,10,9,5,6,7]
```

---

### 3. Rotate Matrix 90° Clockwise

```python
def rotate(matrix):
    """
    Rotate matrix 90° clockwise in-place
    Time: O(n²)
    Space: O(1)
    
    Method: Transpose + Reverse rows
    """
    n = len(matrix)
    
    # Transpose
    for i in range(n):
        for j in range(i + 1, n):
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
    
    # Reverse each row
    for i in range(n):
        matrix[i].reverse()
    
    return matrix

# 90° counterclockwise
def rotate_counterclockwise(matrix):
    """
    Rotate 90° counterclockwise
    Method: Transpose + Reverse columns
    """
    n = len(matrix)
    
    # Transpose
    for i in range(n):
        for j in range(i + 1, n):
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
    
    # Reverse columns (reverse matrix)
    matrix.reverse()
    
    return matrix
```

#### Dry Run
```
Original:
1 2 3
4 5 6
7 8 9

Step 1: Transpose
1 4 7
2 5 8
3 6 9

Step 2: Reverse each row
7 4 1
8 5 2
9 6 3

Result (90° clockwise):
7 4 1
8 5 2
9 6 3
```

---

### 4. Set Matrix Zeros

```python
def set_zeroes(matrix):
    """
    If element is 0, set entire row and column to 0
    Time: O(m*n)
    Space: O(1)
    
    Method: Use first row and column as markers
    """
    rows, cols = len(matrix), len(matrix[0])
    first_row_zero = False
    first_col_zero = False
    
    # Check if first row has zero
    for j in range(cols):
        if matrix[0][j] == 0:
            first_row_zero = True
            break
    
    # Check if first column has zero
    for i in range(rows):
        if matrix[i][0] == 0:
            first_col_zero = True
            break
    
    # Use first row and column as markers
    for i in range(1, rows):
        for j in range(1, cols):
            if matrix[i][j] == 0:
                matrix[i][0] = 0
                matrix[0][j] = 0
    
    # Set zeros based on markers
    for i in range(1, rows):
        for j in range(1, cols):
            if matrix[i][0] == 0 or matrix[0][j] == 0:
                matrix[i][j] = 0
    
    # Handle first row
    if first_row_zero:
        for j in range(cols):
            matrix[0][j] = 0
    
    # Handle first column
    if first_col_zero:
        for i in range(rows):
            matrix[i][0] = 0
```

---

### 5. Search in 2D Matrix

```python
def search_matrix(matrix, target):
    """
    Search in sorted matrix
    Each row sorted, first element of row > last of previous
    Time: O(log(m*n))
    Space: O(1)
    
    Method: Treat as 1D array, binary search
    """
    if not matrix or not matrix[0]:
        return False
    
    rows, cols = len(matrix), len(matrix[0])
    left, right = 0, rows * cols - 1
    
    while left <= right:
        mid = (left + right) // 2
        # Convert to 2D indices
        row = mid // cols
        col = mid % cols
        mid_value = matrix[row][col]
        
        if mid_value == target:
            return True
        elif mid_value < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return False

def search_matrix_ii(matrix, target):
    """
    Search in matrix where:
    - Each row sorted left to right
    - Each column sorted top to bottom
    Time: O(m + n)
    Space: O(1)
    
    Method: Start from top-right corner
    """
    if not matrix or not matrix[0]:
        return False
    
    rows, cols = len(matrix), len(matrix[0])
    row, col = 0, cols - 1
    
    while row < rows and col >= 0:
        if matrix[row][col] == target:
            return True
        elif matrix[row][col] > target:
            col -= 1  # Move left
        else:
            row += 1  # Move down
    
    return False
```

#### Dry Run: Search Matrix II
```
Matrix:
1   4   7   11
2   5   8   12
3   6   9   16
10  13  14  17

Target: 5

Start: row=0, col=3 (top-right)
matrix[0][3] = 11 > 5, move left
matrix[0][2] = 7 > 5, move left
matrix[0][1] = 4 < 5, move down
matrix[1][1] = 5 == 5, found! ✓
```

---

### 6. Diagonal Traverse

```python
def find_diagonal_order(matrix):
    """
    Traverse matrix diagonally
    Time: O(m*n)
    Space: O(1)
    """
    if not matrix or not matrix[0]:
        return []
    
    rows, cols = len(matrix), len(matrix[0])
    result = []
    row, col = 0, 0
    going_up = True
    
    for _ in range(rows * cols):
        result.append(matrix[row][col])
        
        if going_up:
            # Move up-right
            if col == cols - 1:
                row += 1
                going_up = False
            elif row == 0:
                col += 1
                going_up = False
            else:
                row -= 1
                col += 1
        else:
            # Move down-left
            if row == rows - 1:
                col += 1
                going_up = True
            elif col == 0:
                row += 1
                going_up = True
            else:
                row += 1
                col -= 1
    
    return result
```

---

### 7. Word Search

```python
def exist(board, word):
    """
    Search for word in matrix
    Time: O(m*n * 4^L) where L = word length
    Space: O(L)
    
    Method: DFS with backtracking
    """
    rows, cols = len(board), len(board[0])
    
    def dfs(row, col, index):
        # Base case: found word
        if index == len(word):
            return True
        
        # Check bounds and character match
        if (row < 0 or row >= rows or 
            col < 0 or col >= cols or 
            board[row][col] != word[index]):
            return False
        
        # Mark as visited
        temp = board[row][col]
        board[row][col] = '#'
        
        # Explore all 4 directions
        found = (dfs(row + 1, col, index + 1) or
                 dfs(row - 1, col, index + 1) or
                 dfs(row, col + 1, index + 1) or
                 dfs(row, col - 1, index + 1))
        
        # Backtrack
        board[row][col] = temp
        
        return found
    
    # Try starting from each cell
    for i in range(rows):
        for j in range(cols):
            if dfs(i, j, 0):
                return True
    
    return False
```

---

### 8. Number of Islands

```python
def num_islands(grid):
    """
    Count number of islands (1s surrounded by 0s)
    Time: O(m*n)
    Space: O(m*n) for DFS stack
    """
    if not grid:
        return 0
    
    rows, cols = len(grid), len(grid[0])
    count = 0
    
    def dfs(row, col):
        # Check bounds
        if (row < 0 or row >= rows or 
            col < 0 or col >= cols or 
            grid[row][col] != '1'):
            return
        
        # Mark as visited
        grid[row][col] = '0'
        
        # Explore all 4 directions
        dfs(row + 1, col)
        dfs(row - 1, col)
        dfs(row, col + 1)
        dfs(row, col - 1)
    
    for i in range(rows):
        for j in range(cols):
            if grid[i][j] == '1':
                dfs(i, j)
                count += 1
    
    return count

# BFS approach
from collections import deque

def num_islands_bfs(grid):
    """Using BFS"""
    if not grid:
        return 0
    
    rows, cols = len(grid), len(grid[0])
    count = 0
    
    def bfs(start_row, start_col):
        queue = deque([(start_row, start_col)])
        grid[start_row][start_col] = '0'
        
        while queue:
            row, col = queue.popleft()
            
            # Check all 4 directions
            for dr, dc in [(1,0), (-1,0), (0,1), (0,-1)]:
                new_row, new_col = row + dr, col + dc
                
                if (0 <= new_row < rows and 
                    0 <= new_col < cols and 
                    grid[new_row][new_col] == '1'):
                    grid[new_row][new_col] = '0'
                    queue.append((new_row, new_col))
    
    for i in range(rows):
        for j in range(cols):
            if grid[i][j] == '1':
                bfs(i, j)
                count += 1
    
    return count
```

---

### 9. Valid Sudoku

```python
def is_valid_sudoku(board):
    """
    Check if 9x9 Sudoku board is valid
    Time: O(1) - fixed 9x9 board
    Space: O(1)
    """
    # Check rows
    for row in board:
        if not is_valid_unit(row):
            return False
    
    # Check columns
    for col in range(9):
        column = [board[row][col] for row in range(9)]
        if not is_valid_unit(column):
            return False
    
    # Check 3x3 boxes
    for box_row in range(0, 9, 3):
        for box_col in range(0, 9, 3):
            box = []
            for i in range(3):
                for j in range(3):
                    box.append(board[box_row + i][box_col + j])
            if not is_valid_unit(box):
                return False
    
    return True

def is_valid_unit(unit):
    """Check if unit (row/col/box) is valid"""
    unit = [x for x in unit if x != '.']
    return len(unit) == len(set(unit))

# Optimized single pass
def is_valid_sudoku_optimized(board):
    """
    Single pass validation
    """
    rows = [set() for _ in range(9)]
    cols = [set() for _ in range(9)]
    boxes = [set() for _ in range(9)]
    
    for i in range(9):
        for j in range(9):
            if board[i][j] == '.':
                continue
            
            num = board[i][j]
            box_index = (i // 3) * 3 + (j // 3)
            
            # Check duplicates
            if (num in rows[i] or 
                num in cols[j] or 
                num in boxes[box_index]):
                return False
            
            rows[i].add(num)
            cols[j].add(num)
            boxes[box_index].add(num)
    
    return True
```

---

### 10. Maximal Rectangle

```python
def maximal_rectangle(matrix):
    """
    Find largest rectangle containing only 1s
    Time: O(m*n)
    Space: O(n)
    
    Method: Convert to histogram problem
    """
    if not matrix or not matrix[0]:
        return 0
    
    rows, cols = len(matrix), len(matrix[0])
    heights = [0] * cols
    max_area = 0
    
    for i in range(rows):
        for j in range(cols):
            # Update heights
            if matrix[i][j] == '1':
                heights[j] += 1
            else:
                heights[j] = 0
        
        # Find max area in histogram
        max_area = max(max_area, largest_rectangle_histogram(heights))
    
    return max_area

def largest_rectangle_histogram(heights):
    """
    Largest rectangle in histogram
    Time: O(n)
    """
    stack = []
    max_area = 0
    heights.append(0)
    
    for i, h in enumerate(heights):
        while stack and heights[stack[-1]] > h:
            height = heights[stack.pop()]
            width = i if not stack else i - stack[-1] - 1
            max_area = max(max_area, height * width)
        
        stack.append(i)
    
    heights.pop()
    return max_area
```

---

## Matrix Patterns

### 1. Four Directions (Up, Down, Left, Right)
```python
directions = [(0,1), (0,-1), (1,0), (-1,0)]

for dr, dc in directions:
    new_row = row + dr
    new_col = col + dc
```

### 2. Eight Directions (Including Diagonals)
```python
directions = [
    (-1,-1), (-1,0), (-1,1),
    (0,-1),          (0,1),
    (1,-1),  (1,0),  (1,1)
]
```

### 3. Bounds Checking
```python
def is_valid(row, col, rows, cols):
    return 0 <= row < rows and 0 <= col < cols
```

---

## Time Complexity Summary

| Operation | Time | Space |
|-----------|------|-------|
| Traverse | O(m*n) | O(1) |
| Spiral | O(m*n) | O(1) |
| Rotate | O(n²) | O(1) |
| Search (sorted) | O(log(m*n)) | O(1) |
| DFS/BFS | O(m*n) | O(m*n) |
| Transpose | O(m*n) | O(1) |

---

## Exam Tips

### 1. MCQ Questions
**Q**: Time to traverse m×n matrix?
**A**: O(m*n)

**Q**: Space to rotate matrix in-place?
**A**: O(1)

**Q**: To rotate 90° clockwise?
**A**: Transpose + Reverse rows

**Q**: Search in row & column sorted matrix?
**A**: Start from top-right, O(m+n)

### 2. Common Patterns
- **Spiral**: Use boundaries (top, bottom, left, right)
- **Rotation**: Transpose + Reverse
- **DFS/BFS**: For connectivity problems
- **DP**: For path problems
- **Binary Search**: For sorted matrices

### Key Points
1. **2D to 1D**: index = row * cols + col
2. **1D to 2D**: row = index // cols, col = index % cols
3. **Directions**: Use direction arrays
4. **In-place**: Use matrix itself for markers
5. **Spiral**: Four-direction traversal with boundaries
6. **Rotation**: Transpose then reverse
7. **Search**: Binary search or start from corner
8. **Islands**: DFS/BFS for connected components
