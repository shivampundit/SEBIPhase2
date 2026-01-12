# Backtracking - Comprehensive Notes

## Overview
Backtracking is an algorithmic technique for solving problems recursively by trying to build a solution incrementally, abandoning solutions ("backtracking") as soon as it determines that the solution cannot be completed.

### Key Characteristics
1. **Incremental approach**: Build solution step by step
2. **Abandons invalid paths**: Backtracks when constraint violated
3. **Explores all possibilities**: Systematic search
4. **Recursive nature**: Natural recursive implementation

### General Template
```python
def backtrack(candidate):
    if is_solution(candidate):
        output(candidate)
        return
    
    for next_candidate in get_candidates(candidate):
        if is_valid(next_candidate):
            place(next_candidate)
            backtrack(next_candidate)
            remove(next_candidate)  # Backtrack
```

---

## Classic Backtracking Problems

## 1. N-Queens Problem

### Problem
Place N queens on N×N chessboard so no two queens attack each other.

### Implementation
```python
def solve_n_queens(n):
    """
    Solve N-Queens problem
    Time: O(N!)
    Space: O(N)
    """
    def is_safe(board, row, col):
        # Check column
        for i in range(row):
            if board[i] == col:
                return False
        
        # Check diagonal (upper left)
        for i, j in zip(range(row-1, -1, -1), range(col-1, -1, -1)):
            if board[i] == j:
                return False
        
        # Check diagonal (upper right)
        for i, j in zip(range(row-1, -1, -1), range(col+1, n)):
            if board[i] == j:
                return False
        
        return True
    
    def backtrack(board, row):
        if row == n:
            solutions.append(board[:])
            return
        
        for col in range(n):
            if is_safe(board, row, col):
                board[row] = col
                backtrack(board, row + 1)
                board[row] = -1  # Backtrack
    
    solutions = []
    board = [-1] * n
    backtrack(board, 0)
    return solutions

# Example
solutions = solve_n_queens(4)
print(f"Number of solutions for 4-Queens: {len(solutions)}")

# Visualize first solution
def print_board(board):
    n = len(board)
    for row in range(n):
        line = ['.'] * n
        line[board[row]] = 'Q'
        print(' '.join(line))
    print()

if solutions:
    print("First solution:")
    print_board(solutions[0])
```

### Dry Run (N=4)
```
Initial: board = [-1, -1, -1, -1]

Row 0:
  Try col 0: Safe, board = [0, -1, -1, -1]
    Row 1:
      Try col 0: Unsafe (same column)
      Try col 1: Unsafe (diagonal)
      Try col 2: Safe, board = [0, 2, -1, -1]
        Row 2:
          Try col 0: Unsafe (same column)
          Try col 1: Unsafe (diagonal)
          Try col 2: Unsafe (same column)
          Try col 3: Unsafe (diagonal)
          Backtrack to Row 1
      Try col 3: Safe, board = [0, 3, -1, -1]
        Row 2:
          Try col 0: Unsafe
          Try col 1: Safe, board = [0, 3, 1, -1]
            Row 3:
              All unsafe
              Backtrack
          Try col 2: Unsafe
          Try col 3: Unsafe
          Backtrack to Row 1
      Backtrack to Row 0
  Try col 1: Safe, board = [1, -1, -1, -1]
    Row 1:
      Try col 0: Unsafe
      Try col 1: Unsafe
      Try col 2: Unsafe
      Try col 3: Safe, board = [1, 3, -1, -1]
        Row 2:
          Try col 0: Safe, board = [1, 3, 0, -1]
            Row 3:
              Try col 2: Safe, board = [1, 3, 0, 2]
              Solution found! ✓

Solutions for N=4:
Solution 1: [1, 3, 0, 2]
. Q . .
. . . Q
Q . . .
. . Q .

Solution 2: [2, 0, 3, 1]
. . Q .
Q . . .
. . . Q
. Q . .
```

---

## 2. Sudoku Solver

### Problem
Fill 9×9 grid following Sudoku rules.

### Implementation
```python
def solve_sudoku(board):
    """
    Solve Sudoku puzzle
    Time: O(9^(n*n)) worst case
    Space: O(n*n)
    """
    def is_valid(board, row, col, num):
        # Check row
        if num in board[row]:
            return False
        
        # Check column
        if num in [board[i][col] for i in range(9)]:
            return False
        
        # Check 3x3 box
        box_row, box_col = 3 * (row // 3), 3 * (col // 3)
        for i in range(box_row, box_row + 3):
            for j in range(box_col, box_col + 3):
                if board[i][j] == num:
                    return False
        
        return True
    
    def backtrack():
        for row in range(9):
            for col in range(9):
                if board[row][col] == 0:
                    for num in range(1, 10):
                        if is_valid(board, row, col, num):
                            board[row][col] = num
                            
                            if backtrack():
                                return True
                            
                            board[row][col] = 0  # Backtrack
                    
                    return False
        return True
    
    backtrack()
    return board

# Example
board = [
    [5, 3, 0, 0, 7, 0, 0, 0, 0],
    [6, 0, 0, 1, 9, 5, 0, 0, 0],
    [0, 9, 8, 0, 0, 0, 0, 6, 0],
    [8, 0, 0, 0, 6, 0, 0, 0, 3],
    [4, 0, 0, 8, 0, 3, 0, 0, 1],
    [7, 0, 0, 0, 2, 0, 0, 0, 6],
    [0, 6, 0, 0, 0, 0, 2, 8, 0],
    [0, 0, 0, 4, 1, 9, 0, 0, 5],
    [0, 0, 0, 0, 8, 0, 0, 7, 9]
]

solve_sudoku(board)
for row in board:
    print(row)
```

---

## 3. Permutations

### Problem
Generate all permutations of a list.

### Implementation
```python
def permutations(nums):
    """
    Generate all permutations
    Time: O(n! * n)
    Space: O(n!)
    """
    def backtrack(current, remaining):
        if not remaining:
            result.append(current[:])
            return
        
        for i in range(len(remaining)):
            current.append(remaining[i])
            backtrack(current, remaining[:i] + remaining[i+1:])
            current.pop()  # Backtrack
    
    result = []
    backtrack([], nums)
    return result

# Example
nums = [1, 2, 3]
perms = permutations(nums)
print(f"Permutations: {perms}")
print(f"Total: {len(perms)}")
```

### Dry Run (nums=[1,2,3])
```
backtrack([], [1,2,3])
├─ Choose 1: backtrack([1], [2,3])
│  ├─ Choose 2: backtrack([1,2], [3])
│  │  └─ Choose 3: backtrack([1,2,3], [])
│  │     └─ Add [1,2,3] to result ✓
│  └─ Choose 3: backtrack([1,3], [2])
│     └─ Choose 2: backtrack([1,3,2], [])
│        └─ Add [1,3,2] to result ✓
├─ Choose 2: backtrack([2], [1,3])
│  ├─ Choose 1: backtrack([2,1], [3])
│  │  └─ Choose 3: backtrack([2,1,3], [])
│  │     └─ Add [2,1,3] to result ✓
│  └─ Choose 3: backtrack([2,3], [1])
│     └─ Choose 1: backtrack([2,3,1], [])
│        └─ Add [2,3,1] to result ✓
└─ Choose 3: backtrack([3], [1,2])
   ├─ Choose 1: backtrack([3,1], [2])
   │  └─ Choose 2: backtrack([3,1,2], [])
   │     └─ Add [3,1,2] to result ✓
   └─ Choose 2: backtrack([3,2], [1])
      └─ Choose 1: backtrack([3,2,1], [])
         └─ Add [3,2,1] to result ✓

Result: [[1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], [3,2,1]]
Total: 6 (3! = 6)
```

---

## 4. Combinations

### Problem
Generate all combinations of k numbers from 1 to n.

### Implementation
```python
def combinations(n, k):
    """
    Generate all combinations of k numbers from 1 to n
    Time: O(C(n,k) * k)
    Space: O(C(n,k))
    """
    def backtrack(start, current):
        if len(current) == k:
            result.append(current[:])
            return
        
        for i in range(start, n + 1):
            current.append(i)
            backtrack(i + 1, current)
            current.pop()  # Backtrack
    
    result = []
    backtrack(1, [])
    return result

# Example
combs = combinations(4, 2)
print(f"Combinations: {combs}")
print(f"Total: {len(combs)}")
```

### Dry Run (n=4, k=2)
```
backtrack(1, [])
├─ Choose 1: backtrack(2, [1])
│  ├─ Choose 2: backtrack(3, [1,2])
│  │  └─ Add [1,2] ✓
│  ├─ Choose 3: backtrack(4, [1,3])
│  │  └─ Add [1,3] ✓
│  └─ Choose 4: backtrack(5, [1,4])
│     └─ Add [1,4] ✓
├─ Choose 2: backtrack(3, [2])
│  ├─ Choose 3: backtrack(4, [2,3])
│  │  └─ Add [2,3] ✓
│  └─ Choose 4: backtrack(5, [2,4])
│     └─ Add [2,4] ✓
└─ Choose 3: backtrack(4, [3])
   └─ Choose 4: backtrack(5, [3,4])
      └─ Add [3,4] ✓

Result: [[1,2], [1,3], [1,4], [2,3], [2,4], [3,4]]
Total: 6 (C(4,2) = 6)
```

---

## 5. Subset Sum Problem

### Problem
Find all subsets that sum to a target value.

### Implementation
```python
def subset_sum(nums, target):
    """
    Find all subsets that sum to target
    Time: O(2^n)
    Space: O(2^n)
    """
    def backtrack(start, current, current_sum):
        if current_sum == target:
            result.append(current[:])
            return
        
        if current_sum > target:
            return
        
        for i in range(start, len(nums)):
            current.append(nums[i])
            backtrack(i + 1, current, current_sum + nums[i])
            current.pop()  # Backtrack
    
    result = []
    backtrack(0, [], 0)
    return result

# Example
nums = [2, 3, 5, 7]
target = 7
subsets = subset_sum(nums, target)
print(f"Subsets that sum to {target}: {subsets}")
```

### Dry Run (nums=[2,3,5,7], target=7)
```
backtrack(0, [], 0)
├─ Choose 2 (sum=2): backtrack(1, [2], 2)
│  ├─ Choose 3 (sum=5): backtrack(2, [2,3], 5)
│  │  ├─ Choose 5 (sum=10): Exceeds target, return
│  │  └─ Choose 7 (sum=12): Exceeds target, return
│  └─ Choose 5 (sum=7): backtrack(3, [2,5], 7)
│     └─ Add [2,5] ✓
├─ Choose 3 (sum=3): backtrack(2, [3], 3)
│  ├─ Choose 5 (sum=8): Exceeds target
│  └─ Choose 7 (sum=10): Exceeds target
├─ Choose 5 (sum=5): backtrack(3, [5], 5)
│  └─ Choose 7 (sum=12): Exceeds target
└─ Choose 7 (sum=7): backtrack(4, [7], 7)
   └─ Add [7] ✓

Result: [[2,5], [7]]
```

---

## 6. Word Search

### Problem
Given a 2D board and a word, check if word exists in the grid (horizontally or vertically).

### Implementation
```python
def word_search(board, word):
    """
    Search for word in 2D board
    Time: O(m*n*4^L) where L is word length
    Space: O(L)
    """
    def backtrack(row, col, index):
        if index == len(word):
            return True
        
        if (row < 0 or row >= len(board) or 
            col < 0 or col >= len(board[0]) or
            board[row][col] != word[index]):
            return False
        
        # Mark as visited
        temp = board[row][col]
        board[row][col] = '#'
        
        # Explore 4 directions
        found = (backtrack(row + 1, col, index + 1) or
                backtrack(row - 1, col, index + 1) or
                backtrack(row, col + 1, index + 1) or
                backtrack(row, col - 1, index + 1))
        
        # Backtrack
        board[row][col] = temp
        
        return found
    
    for i in range(len(board)):
        for j in range(len(board[0])):
            if backtrack(i, j, 0):
                return True
    
    return False

# Example
board = [
    ['A', 'B', 'C', 'E'],
    ['S', 'F', 'C', 'S'],
    ['A', 'D', 'E', 'E']
]

print(word_search(board, "ABCCED"))  # True
print(word_search(board, "SEE"))     # True
print(word_search(board, "ABCB"))    # False
```

---

## 7. Generate Parentheses

### Problem
Generate all combinations of n pairs of valid parentheses.

### Implementation
```python
def generate_parentheses(n):
    """
    Generate all valid parentheses combinations
    Time: O(4^n / sqrt(n)) - Catalan number
    Space: O(4^n / sqrt(n))
    """
    def backtrack(current, open_count, close_count):
        if len(current) == 2 * n:
            result.append(current)
            return
        
        if open_count < n:
            backtrack(current + '(', open_count + 1, close_count)
        
        if close_count < open_count:
            backtrack(current + ')', open_count, close_count + 1)
    
    result = []
    backtrack('', 0, 0)
    return result

# Example
n = 3
parens = generate_parentheses(n)
print(f"Valid parentheses for n={n}:")
for p in parens:
    print(p)
print(f"Total: {len(parens)}")
```

### Dry Run (n=2)
```
backtrack('', 0, 0)
├─ Add '(': backtrack('(', 1, 0)
│  ├─ Add '(': backtrack('((', 2, 0)
│  │  └─ Add ')': backtrack('(()', 2, 1)
│  │     └─ Add ')': backtrack('(())', 2, 2)
│  │        └─ Add '(())' ✓
│  └─ Add ')': backtrack('()', 1, 1)
│     └─ Add '(': backtrack('()(', 2, 1)
│        └─ Add ')': backtrack('()()', 2, 2)
│           └─ Add '()()' ✓

Result: ['(())', '()()']
```

---

## Backtracking Patterns

### Pattern 1: Choice + Explore + Unchoose
```python
def backtrack(state):
    if is_goal(state):
        save(state)
        return
    
    for choice in get_choices(state):
        make_choice(choice)
        backtrack(new_state)
        undo_choice(choice)  # Backtrack
```

### Pattern 2: With Pruning
```python
def backtrack(state):
    if is_invalid(state):
        return  # Prune
    
    if is_goal(state):
        save(state)
        return
    
    for choice in get_choices(state):
        backtrack(make_choice(choice))
```

---

## Backtracking vs Other Techniques

| Feature | Backtracking | Dynamic Programming | Greedy |
|---------|--------------|---------------------|--------|
| Approach | Try all possibilities | Store subproblem results | Make local optimal choice |
| When stuck | Backtrack | Use stored result | Continue forward |
| Complexity | Often exponential | Polynomial | Usually linear/log |
| Guarantee | Can find all solutions | Optimal solution | Not always optimal |
| Examples | N-Queens, Sudoku | Knapsack, LCS | Activity Selection |

---

## Exam Tips

### 1. Recognizing Backtracking Problems
Look for:
- "Find all solutions"
- "Generate all combinations/permutations"
- Constraint satisfaction
- Puzzle solving
- Path finding with constraints

### 2. Implementation Checklist
```python
def backtrack(state):
    # 1. Base case (goal reached)
    if is_goal(state):
        record_solution()
        return
    
    # 2. Pruning (invalid state)
    if is_invalid(state):
        return
    
    # 3. Explore choices
    for choice in choices:
        # 4. Make choice
        state.add(choice)
        
        # 5. Recurse
        backtrack(state)
        
        # 6. Undo choice (backtrack)
        state.remove(choice)
```

### 3. Common Bugs
- Forgetting to backtrack (undo choice)
- Not checking validity before recursing
- Modifying passed list/object without copying
- Incorrect base case
- Missing pruning conditions

### 4. Optimization Techniques
1. **Pruning**: Stop early if constraints violated
2. **Sorting**: Order choices to find solutions faster
3. **Visited tracking**: Avoid revisiting states
4. **Heuristics**: Use problem-specific knowledge

### Key Points
1. **Template**: Choose → Explore → Unchoose
2. **Recursion**: Natural fit for backtracking
3. **State management**: Track current state carefully
4. **Pruning**: Essential for performance
5. **All solutions**: Backtracking finds all valid solutions
6. **Time complexity**: Often exponential O(2^n) or O(n!)
