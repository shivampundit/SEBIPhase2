# Backtracking - Comprehensive Notes

## Overview
Backtracking is an algorithmic technique for solving problems recursively by trying to build a solution incrementally, abandoning solutions ("backtracking") as soon as it determines that the solution cannot be completed.

### Key Characteristics
1. **Incremental approach**: Build solution step by step
2. **Abandons invalid paths**: Backtracks when constraint violated
3. **Explores all possibilities**: Systematic search
4. **Recursive nature**: Natural recursive implementation

### General Template

**Python:**
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

**C:**
```c
#include <stdio.h>
#include <stdbool.h>

void backtrack(int* candidate, int size) {
    if (is_solution(candidate, size)) {
        output(candidate, size);
        return;
    }
    
    int* candidates;
    int num_candidates = get_candidates(candidate, size, &candidates);
    
    for (int i = 0; i < num_candidates; i++) {
        if (is_valid(candidates[i])) {
            place(candidates[i]);
            backtrack(candidate, size + 1);
            remove_item(candidates[i]);  // Backtrack
        }
    }
    
    // Caller is responsible for freeing candidates if dynamically allocated
    free(candidates);
}
```

**C++:**
```cpp
#include <vector>
#include <iostream>

void backtrack(std::vector<int>& candidate) {
    if (is_solution(candidate)) {
        output(candidate);
        return;
    }
    
    std::vector<int> next_candidates = get_candidates(candidate);
    
    for (int next_candidate : next_candidates) {
        if (is_valid(next_candidate)) {
            candidate.push_back(next_candidate);
            backtrack(candidate);
            candidate.pop_back();  // Backtrack
        }
    }
}
```

**Java:**
```java
import java.util.*;

public class Backtracking {
    public static void backtrack(List<Integer> candidate) {
        if (isSolution(candidate)) {
            output(candidate);
            return;
        }
        
        List<Integer> nextCandidates = getCandidates(candidate);
        
        for (int nextCandidate : nextCandidates) {
            if (isValid(nextCandidate)) {
                candidate.add(nextCandidate);
                backtrack(candidate);
                // Remove last element (O(1) for ArrayList)
                candidate.remove(candidate.size() - 1);  // Backtrack
            }
        }
    }
}
```

---

## Classic Backtracking Problems

## 1. N-Queens Problem

### Problem
Place N queens on N×N chessboard so no two queens attack each other.

### Implementation

**Python:**
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

**C:**
```c
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

#define MAX_N 20

typedef struct {
    int board[MAX_N];
    int count;
} Solutions;

// Check if placing queen at (row, col) is safe
bool is_safe(int board[], int row, int col, int n) {
    int i, j;
    
    // Check column
    for (i = 0; i < row; i++) {
        if (board[i] == col) {
            return false;
        }
    }
    
    // Check diagonal (upper left)
    for (i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
        if (board[i] == j) {
            return false;
        }
    }
    
    // Check diagonal (upper right)
    for (i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {
        if (board[i] == j) {
            return false;
        }
    }
    
    return true;
}

// Backtracking function
void backtrack(int board[], int row, int n, int all_solutions[][MAX_N], int* solution_count) {
    if (row == n) {
        // Found a solution
        for (int i = 0; i < n; i++) {
            all_solutions[*solution_count][i] = board[i];
        }
        (*solution_count)++;
        return;
    }
    
    for (int col = 0; col < n; col++) {
        if (is_safe(board, row, col, n)) {
            board[row] = col;
            backtrack(board, row + 1, n, all_solutions, solution_count);
            board[row] = -1;  // Backtrack
        }
    }
}

// Print board
void print_board(int board[], int n) {
    for (int row = 0; row < n; row++) {
        for (int col = 0; col < n; col++) {
            if (board[row] == col) {
                printf("Q ");
            } else {
                printf(". ");
            }
        }
        printf("\n");
    }
    printf("\n");
}

// Solve N-Queens
int solve_n_queens(int n, int all_solutions[][MAX_N]) {
    int board[MAX_N];
    int solution_count = 0;
    
    // Initialize board
    for (int i = 0; i < n; i++) {
        board[i] = -1;
    }
    
    backtrack(board, 0, n, all_solutions, &solution_count);
    return solution_count;
}

// Example usage
int main() {
    int n = 4;
    int all_solutions[100][MAX_N];
    
    int count = solve_n_queens(n, all_solutions);
    printf("Number of solutions for %d-Queens: %d\n", n, count);
    
    if (count > 0) {
        printf("\nFirst solution:\n");
        print_board(all_solutions[0], n);
    }
    
    return 0;
}
```

**C++:**
```cpp
#include <iostream>
#include <vector>
#include <cmath>

using namespace std;

class NQueens {
private:
    vector<vector<int>> solutions;
    int n;
    
    // Check if placing queen at (row, col) is safe
    bool is_safe(const vector<int>& board, int row, int col) {
        // Check column
        for (int i = 0; i < row; i++) {
            if (board[i] == col) {
                return false;
            }
        }
        
        // Check diagonal (upper left)
        for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
            if (board[i] == j) {
                return false;
            }
        }
        
        // Check diagonal (upper right)
        for (int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {
            if (board[i] == j) {
                return false;
            }
        }
        
        return true;
    }
    
    // Backtracking function
    void backtrack(vector<int>& board, int row) {
        if (row == n) {
            solutions.push_back(board);
            return;
        }
        
        for (int col = 0; col < n; col++) {
            if (is_safe(board, row, col)) {
                board[row] = col;
                backtrack(board, row + 1);
                board[row] = -1;  // Backtrack
            }
        }
    }
    
public:
    // Solve N-Queens problem
    vector<vector<int>> solve_n_queens(int size) {
        n = size;
        solutions.clear();
        vector<int> board(n, -1);
        backtrack(board, 0);
        return solutions;
    }
    
    // Print board
    void print_board(const vector<int>& board) {
        int n = board.size();
        for (int row = 0; row < n; row++) {
            for (int col = 0; col < n; col++) {
                if (board[row] == col) {
                    cout << "Q ";
                } else {
                    cout << ". ";
                }
            }
            cout << endl;
        }
        cout << endl;
    }
};

// Example usage
int main() {
    NQueens nq;
    vector<vector<int>> solutions = nq.solve_n_queens(4);
    
    cout << "Number of solutions for 4-Queens: " << solutions.size() << endl;
    
    if (!solutions.empty()) {
        cout << "\nFirst solution:" << endl;
        nq.print_board(solutions[0]);
    }
    
    return 0;
}
```

**Java:**
```java
import java.util.*;

public class NQueens {
    private List<List<Integer>> solutions;
    private int n;
    
    // Check if placing queen at (row, col) is safe
    private boolean isSafe(List<Integer> board, int row, int col) {
        // Check column
        for (int i = 0; i < row; i++) {
            if (board.get(i) == col) {
                return false;
            }
        }
        
        // Check diagonal (upper left)
        for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
            if (board.get(i) == j) {
                return false;
            }
        }
        
        // Check diagonal (upper right)
        for (int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {
            if (board.get(i) == j) {
                return false;
            }
        }
        
        return true;
    }
    
    // Backtracking function
    private void backtrack(List<Integer> board, int row) {
        if (row == n) {
            solutions.add(new ArrayList<>(board));
            return;
        }
        
        for (int col = 0; col < n; col++) {
            if (isSafe(board, row, col)) {
                board.set(row, col);
                backtrack(board, row + 1);
                board.set(row, -1);  // Backtrack
            }
        }
    }
    
    // Solve N-Queens problem
    public List<List<Integer>> solveNQueens(int size) {
        n = size;
        solutions = new ArrayList<>();
        List<Integer> board = new ArrayList<>();
        
        // Initialize board
        for (int i = 0; i < n; i++) {
            board.add(-1);
        }
        
        backtrack(board, 0);
        return solutions;
    }
    
    // Print board
    public void printBoard(List<Integer> board) {
        int n = board.size();
        for (int row = 0; row < n; row++) {
            for (int col = 0; col < n; col++) {
                if (board.get(row) == col) {
                    System.out.print("Q ");
                } else {
                    System.out.print(". ");
                }
            }
            System.out.println();
        }
        System.out.println();
    }
    
    // Example usage
    public static void main(String[] args) {
        NQueens nq = new NQueens();
        List<List<Integer>> solutions = nq.solveNQueens(4);
        
        System.out.println("Number of solutions for 4-Queens: " + solutions.size());
        
        if (!solutions.isEmpty()) {
            System.out.println("\nFirst solution:");
            nq.printBoard(solutions.get(0));
        }
    }
}
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

**Python:**
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

**C:**
```c
#include <stdio.h>
#include <stdbool.h>

#define N 9

// Check if placing num at (row, col) is valid
bool is_valid(int board[N][N], int row, int col, int num) {
    // Check row
    for (int j = 0; j < N; j++) {
        if (board[row][j] == num) {
            return false;
        }
    }
    
    // Check column
    for (int i = 0; i < N; i++) {
        if (board[i][col] == num) {
            return false;
        }
    }
    
    // Check 3x3 box
    int box_row = 3 * (row / 3);
    int box_col = 3 * (col / 3);
    for (int i = box_row; i < box_row + 3; i++) {
        for (int j = box_col; j < box_col + 3; j++) {
            if (board[i][j] == num) {
                return false;
            }
        }
    }
    
    return true;
}

// Backtracking function to solve Sudoku
bool backtrack(int board[N][N]) {
    for (int row = 0; row < N; row++) {
        for (int col = 0; col < N; col++) {
            if (board[row][col] == 0) {
                for (int num = 1; num <= 9; num++) {
                    if (is_valid(board, row, col, num)) {
                        board[row][col] = num;
                        
                        if (backtrack(board)) {
                            return true;
                        }
                        
                        board[row][col] = 0;  // Backtrack
                    }
                }
                return false;
            }
        }
    }
    return true;
}

// Print board
void print_board(int board[N][N]) {
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            printf("%d ", board[i][j]);
        }
        printf("\n");
    }
}

// Example usage
int main() {
    int board[N][N] = {
        {5, 3, 0, 0, 7, 0, 0, 0, 0},
        {6, 0, 0, 1, 9, 5, 0, 0, 0},
        {0, 9, 8, 0, 0, 0, 0, 6, 0},
        {8, 0, 0, 0, 6, 0, 0, 0, 3},
        {4, 0, 0, 8, 0, 3, 0, 0, 1},
        {7, 0, 0, 0, 2, 0, 0, 0, 6},
        {0, 6, 0, 0, 0, 0, 2, 8, 0},
        {0, 0, 0, 4, 1, 9, 0, 0, 5},
        {0, 0, 0, 0, 8, 0, 0, 7, 9}
    };
    
    if (backtrack(board)) {
        printf("Solved Sudoku:\n");
        print_board(board);
    } else {
        printf("No solution exists\n");
    }
    
    return 0;
}
```

**C++:**
```cpp
#include <iostream>
#include <vector>

using namespace std;

class SudokuSolver {
private:
    static const int N = 9;
    
    // Check if placing num at (row, col) is valid
    bool is_valid(vector<vector<int>>& board, int row, int col, int num) {
        // Check row
        for (int j = 0; j < N; j++) {
            if (board[row][j] == num) {
                return false;
            }
        }
        
        // Check column
        for (int i = 0; i < N; i++) {
            if (board[i][col] == num) {
                return false;
            }
        }
        
        // Check 3x3 box
        int box_row = 3 * (row / 3);
        int box_col = 3 * (col / 3);
        for (int i = box_row; i < box_row + 3; i++) {
            for (int j = box_col; j < box_col + 3; j++) {
                if (board[i][j] == num) {
                    return false;
                }
            }
        }
        
        return true;
    }
    
    // Backtracking function
    bool backtrack(vector<vector<int>>& board) {
        for (int row = 0; row < N; row++) {
            for (int col = 0; col < N; col++) {
                if (board[row][col] == 0) {
                    for (int num = 1; num <= 9; num++) {
                        if (is_valid(board, row, col, num)) {
                            board[row][col] = num;
                            
                            if (backtrack(board)) {
                                return true;
                            }
                            
                            board[row][col] = 0;  // Backtrack
                        }
                    }
                    return false;
                }
            }
        }
        return true;
    }
    
public:
    // Solve Sudoku puzzle
    bool solve_sudoku(vector<vector<int>>& board) {
        return backtrack(board);
    }
    
    // Print board
    void print_board(const vector<vector<int>>& board) {
        for (const auto& row : board) {
            for (int val : row) {
                cout << val << " ";
            }
            cout << endl;
        }
    }
};

// Example usage
int main() {
    vector<vector<int>> board = {
        {5, 3, 0, 0, 7, 0, 0, 0, 0},
        {6, 0, 0, 1, 9, 5, 0, 0, 0},
        {0, 9, 8, 0, 0, 0, 0, 6, 0},
        {8, 0, 0, 0, 6, 0, 0, 0, 3},
        {4, 0, 0, 8, 0, 3, 0, 0, 1},
        {7, 0, 0, 0, 2, 0, 0, 0, 6},
        {0, 6, 0, 0, 0, 0, 2, 8, 0},
        {0, 0, 0, 4, 1, 9, 0, 0, 5},
        {0, 0, 0, 0, 8, 0, 0, 7, 9}
    };
    
    SudokuSolver solver;
    if (solver.solve_sudoku(board)) {
        cout << "Solved Sudoku:" << endl;
        solver.print_board(board);
    } else {
        cout << "No solution exists" << endl;
    }
    
    return 0;
}
```

**Java:**
```java
public class SudokuSolver {
    private static final int N = 9;
    
    // Check if placing num at (row, col) is valid
    private boolean isValid(int[][] board, int row, int col, int num) {
        // Check row
        for (int j = 0; j < N; j++) {
            if (board[row][j] == num) {
                return false;
            }
        }
        
        // Check column
        for (int i = 0; i < N; i++) {
            if (board[i][col] == num) {
                return false;
            }
        }
        
        // Check 3x3 box
        int boxRow = 3 * (row / 3);
        int boxCol = 3 * (col / 3);
        for (int i = boxRow; i < boxRow + 3; i++) {
            for (int j = boxCol; j < boxCol + 3; j++) {
                if (board[i][j] == num) {
                    return false;
                }
            }
        }
        
        return true;
    }
    
    // Backtracking function
    private boolean backtrack(int[][] board) {
        for (int row = 0; row < N; row++) {
            for (int col = 0; col < N; col++) {
                if (board[row][col] == 0) {
                    for (int num = 1; num <= 9; num++) {
                        if (isValid(board, row, col, num)) {
                            board[row][col] = num;
                            
                            if (backtrack(board)) {
                                return true;
                            }
                            
                            board[row][col] = 0;  // Backtrack
                        }
                    }
                    return false;
                }
            }
        }
        return true;
    }
    
    // Solve Sudoku puzzle
    public boolean solveSudoku(int[][] board) {
        return backtrack(board);
    }
    
    // Print board
    public void printBoard(int[][] board) {
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++) {
                System.out.print(board[i][j] + " ");
            }
            System.out.println();
        }
    }
    
    // Example usage
    public static void main(String[] args) {
        int[][] board = {
            {5, 3, 0, 0, 7, 0, 0, 0, 0},
            {6, 0, 0, 1, 9, 5, 0, 0, 0},
            {0, 9, 8, 0, 0, 0, 0, 6, 0},
            {8, 0, 0, 0, 6, 0, 0, 0, 3},
            {4, 0, 0, 8, 0, 3, 0, 0, 1},
            {7, 0, 0, 0, 2, 0, 0, 0, 6},
            {0, 6, 0, 0, 0, 0, 2, 8, 0},
            {0, 0, 0, 4, 1, 9, 0, 0, 5},
            {0, 0, 0, 0, 8, 0, 0, 7, 9}
        };
        
        SudokuSolver solver = new SudokuSolver();
        if (solver.solveSudoku(board)) {
            System.out.println("Solved Sudoku:");
            solver.printBoard(board);
        } else {
            System.out.println("No solution exists");
        }
    }
}
```

---

## 3. Permutations

### Problem
Generate all permutations of a list.

### Implementation

**Python:**
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

**C:**
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_SIZE 10
#define MAX_PERMS 3628800  // 10!

int result_count = 0;

// Swap two elements
void swap(int* a, int* b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

// Backtracking function to generate permutations
void backtrack(int nums[], int size, int start, int result[][MAX_SIZE]) {
    if (start == size - 1) {
        // Found a complete permutation
        for (int i = 0; i < size; i++) {
            result[result_count][i] = nums[i];
        }
        result_count++;
        return;
    }
    
    for (int i = start; i < size; i++) {
        swap(&nums[start], &nums[i]);
        backtrack(nums, size, start + 1, result);
        swap(&nums[start], &nums[i]);  // Backtrack
    }
}

// Generate all permutations
int permutations(int nums[], int size, int result[][MAX_SIZE]) {
    result_count = 0;
    int temp[MAX_SIZE];
    
    // Copy input array
    for (int i = 0; i < size; i++) {
        temp[i] = nums[i];
    }
    
    backtrack(temp, size, 0, result);
    return result_count;
}

// Print permutations
void print_permutations(int result[][MAX_SIZE], int count, int size) {
    printf("Permutations:\n");
    for (int i = 0; i < count; i++) {
        printf("[");
        for (int j = 0; j < size; j++) {
            printf("%d", result[i][j]);
            if (j < size - 1) printf(", ");
        }
        printf("]\n");
    }
}

// Example usage
int main() {
    int nums[] = {1, 2, 3};
    int size = 3;
    int result[MAX_PERMS][MAX_SIZE];
    
    int count = permutations(nums, size, result);
    print_permutations(result, count, size);
    printf("Total: %d\n", count);
    
    return 0;
}
```

**C++:**
```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

class Permutations {
private:
    vector<vector<int>> result;
    
    // Backtracking function
    void backtrack(vector<int>& nums, int start) {
        if (start == nums.size() - 1) {
            result.push_back(nums);
            return;
        }
        
        for (int i = start; i < nums.size(); i++) {
            swap(nums[start], nums[i]);
            backtrack(nums, start + 1);
            swap(nums[start], nums[i]);  // Backtrack
        }
    }
    
public:
    // Generate all permutations
    vector<vector<int>> permutations(vector<int> nums) {
        result.clear();
        backtrack(nums, 0);
        return result;
    }
    
    // Print permutations
    void print_permutations(const vector<vector<int>>& perms) {
        cout << "Permutations:" << endl;
        for (const auto& perm : perms) {
            cout << "[";
            for (size_t i = 0; i < perm.size(); i++) {
                cout << perm[i];
                if (i < perm.size() - 1) cout << ", ";
            }
            cout << "]" << endl;
        }
    }
};

// Example usage
int main() {
    Permutations p;
    vector<int> nums = {1, 2, 3};
    
    vector<vector<int>> perms = p.permutations(nums);
    p.print_permutations(perms);
    cout << "Total: " << perms.size() << endl;
    
    return 0;
}
```

**Java:**
```java
import java.util.*;

public class Permutations {
    private List<List<Integer>> result;
    
    // Backtracking function
    private void backtrack(List<Integer> nums, int start) {
        if (start == nums.size() - 1) {
            result.add(new ArrayList<>(nums));
            return;
        }
        
        for (int i = start; i < nums.size(); i++) {
            Collections.swap(nums, start, i);
            backtrack(nums, start + 1);
            Collections.swap(nums, start, i);  // Backtrack
        }
    }
    
    // Generate all permutations
    public List<List<Integer>> permutations(List<Integer> nums) {
        result = new ArrayList<>();
        backtrack(new ArrayList<>(nums), 0);
        return result;
    }
    
    // Print permutations
    public void printPermutations(List<List<Integer>> perms) {
        System.out.println("Permutations:");
        for (List<Integer> perm : perms) {
            System.out.println(perm);
        }
    }
    
    // Example usage
    public static void main(String[] args) {
        Permutations p = new Permutations();
        List<Integer> nums = Arrays.asList(1, 2, 3);
        
        List<List<Integer>> perms = p.permutations(nums);
        p.printPermutations(perms);
        System.out.println("Total: " + perms.size());
    }
}
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

**Python:**
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

**C:**
```c
#include <stdio.h>
#include <stdlib.h>

#define MAX_SIZE 20
#define MAX_COMBS 1000000

int result_count = 0;

// Backtracking function to generate combinations
void backtrack(int n, int k, int start, int current[], int current_size, int result[][MAX_SIZE]) {
    if (current_size == k) {
        // Found a complete combination
        for (int i = 0; i < k; i++) {
            result[result_count][i] = current[i];
        }
        result_count++;
        return;
    }
    
    for (int i = start; i <= n; i++) {
        current[current_size] = i;
        backtrack(n, k, i + 1, current, current_size + 1, result);
        // Backtrack (implicit as we don't modify current beyond current_size)
    }
}

// Generate all combinations of k numbers from 1 to n
int combinations(int n, int k, int result[][MAX_SIZE]) {
    result_count = 0;
    int current[MAX_SIZE];
    backtrack(n, k, 1, current, 0, result);
    return result_count;
}

// Print combinations
void print_combinations(int result[][MAX_SIZE], int count, int k) {
    printf("Combinations:\n");
    for (int i = 0; i < count; i++) {
        printf("[");
        for (int j = 0; j < k; j++) {
            printf("%d", result[i][j]);
            if (j < k - 1) printf(", ");
        }
        printf("]\n");
    }
}

// Example usage
int main() {
    int n = 4, k = 2;
    int result[MAX_COMBS][MAX_SIZE];
    
    int count = combinations(n, k, result);
    print_combinations(result, count, k);
    printf("Total: %d\n", count);
    
    return 0;
}
```

**C++:**
```cpp
#include <iostream>
#include <vector>

using namespace std;

class Combinations {
private:
    vector<vector<int>> result;
    int n, k;
    
    // Backtracking function
    void backtrack(int start, vector<int>& current) {
        if (current.size() == k) {
            result.push_back(current);
            return;
        }
        
        for (int i = start; i <= n; i++) {
            current.push_back(i);
            backtrack(i + 1, current);
            current.pop_back();  // Backtrack
        }
    }
    
public:
    // Generate all combinations of k numbers from 1 to n
    vector<vector<int>> combinations(int n_val, int k_val) {
        n = n_val;
        k = k_val;
        result.clear();
        vector<int> current;
        backtrack(1, current);
        return result;
    }
    
    // Print combinations
    void print_combinations(const vector<vector<int>>& combs) {
        cout << "Combinations:" << endl;
        for (const auto& comb : combs) {
            cout << "[";
            for (size_t i = 0; i < comb.size(); i++) {
                cout << comb[i];
                if (i < comb.size() - 1) cout << ", ";
            }
            cout << "]" << endl;
        }
    }
};

// Example usage
int main() {
    Combinations c;
    vector<vector<int>> combs = c.combinations(4, 2);
    
    c.print_combinations(combs);
    cout << "Total: " << combs.size() << endl;
    
    return 0;
}
```

**Java:**
```java
import java.util.*;

public class Combinations {
    private List<List<Integer>> result;
    private int n, k;
    
    // Backtracking function
    private void backtrack(int start, List<Integer> current) {
        if (current.size() == k) {
            result.add(new ArrayList<>(current));
            return;
        }
        
        for (int i = start; i <= n; i++) {
            current.add(i);
            backtrack(i + 1, current);
            current.remove(current.size() - 1);  // Backtrack
        }
    }
    
    // Generate all combinations of k numbers from 1 to n
    public List<List<Integer>> combinations(int n, int k) {
        this.n = n;
        this.k = k;
        result = new ArrayList<>();
        backtrack(1, new ArrayList<>());
        return result;
    }
    
    // Print combinations
    public void printCombinations(List<List<Integer>> combs) {
        System.out.println("Combinations:");
        for (List<Integer> comb : combs) {
            System.out.println(comb);
        }
    }
    
    // Example usage
    public static void main(String[] args) {
        Combinations c = new Combinations();
        List<List<Integer>> combs = c.combinations(4, 2);
        
        c.printCombinations(combs);
        System.out.println("Total: " + combs.size());
    }
}
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

**Python:**
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

**C:**
```c
#include <stdio.h>
#include <stdlib.h>

#define MAX_SIZE 20
#define MAX_SUBSETS 10000

int result_count = 0;

// Backtracking function to find subsets that sum to target
void backtrack(int nums[], int size, int start, int current[], int current_size, 
               int current_sum, int target, int result[][MAX_SIZE], int result_sizes[]) {
    if (current_sum == target) {
        // Found a valid subset
        for (int i = 0; i < current_size; i++) {
            result[result_count][i] = current[i];
        }
        result_sizes[result_count] = current_size;
        result_count++;
        return;
    }
    
    if (current_sum > target) {
        return;  // Prune
    }
    
    for (int i = start; i < size; i++) {
        current[current_size] = nums[i];
        backtrack(nums, size, i + 1, current, current_size + 1, 
                 current_sum + nums[i], target, result, result_sizes);
        // Backtrack (implicit)
    }
}

// Find all subsets that sum to target
int subset_sum(int nums[], int size, int target, int result[][MAX_SIZE], int result_sizes[]) {
    result_count = 0;
    int current[MAX_SIZE];
    backtrack(nums, size, 0, current, 0, 0, target, result, result_sizes);
    return result_count;
}

// Print subsets
void print_subsets(int result[][MAX_SIZE], int result_sizes[], int count) {
    printf("Subsets:\n");
    for (int i = 0; i < count; i++) {
        printf("[");
        for (int j = 0; j < result_sizes[i]; j++) {
            printf("%d", result[i][j]);
            if (j < result_sizes[i] - 1) printf(", ");
        }
        printf("]\n");
    }
}

// Example usage
int main() {
    int nums[] = {2, 3, 5, 7};
    int size = 4;
    int target = 7;
    int result[MAX_SUBSETS][MAX_SIZE];
    int result_sizes[MAX_SUBSETS];
    
    int count = subset_sum(nums, size, target, result, result_sizes);
    printf("Subsets that sum to %d:\n", target);
    print_subsets(result, result_sizes, count);
    
    return 0;
}
```

**C++:**
```cpp
#include <iostream>
#include <vector>

using namespace std;

class SubsetSum {
private:
    vector<vector<int>> result;
    
    // Backtracking function
    void backtrack(const vector<int>& nums, int start, vector<int>& current, 
                   int current_sum, int target) {
        if (current_sum == target) {
            result.push_back(current);
            return;
        }
        
        if (current_sum > target) {
            return;  // Prune
        }
        
        for (int i = start; i < nums.size(); i++) {
            current.push_back(nums[i]);
            backtrack(nums, i + 1, current, current_sum + nums[i], target);
            current.pop_back();  // Backtrack
        }
    }
    
public:
    // Find all subsets that sum to target
    vector<vector<int>> subset_sum(const vector<int>& nums, int target) {
        result.clear();
        vector<int> current;
        backtrack(nums, 0, current, 0, target);
        return result;
    }
    
    // Print subsets
    void print_subsets(const vector<vector<int>>& subsets, int target) {
        cout << "Subsets that sum to " << target << ":" << endl;
        for (const auto& subset : subsets) {
            cout << "[";
            for (size_t i = 0; i < subset.size(); i++) {
                cout << subset[i];
                if (i < subset.size() - 1) cout << ", ";
            }
            cout << "]" << endl;
        }
    }
};

// Example usage
int main() {
    SubsetSum ss;
    vector<int> nums = {2, 3, 5, 7};
    int target = 7;
    
    vector<vector<int>> subsets = ss.subset_sum(nums, target);
    ss.print_subsets(subsets, target);
    
    return 0;
}
```

**Java:**
```java
import java.util.*;

public class SubsetSum {
    private List<List<Integer>> result;
    
    // Backtracking function
    private void backtrack(int[] nums, int start, List<Integer> current, 
                          int currentSum, int target) {
        if (currentSum == target) {
            result.add(new ArrayList<>(current));
            return;
        }
        
        if (currentSum > target) {
            return;  // Prune
        }
        
        for (int i = start; i < nums.length; i++) {
            current.add(nums[i]);
            backtrack(nums, i + 1, current, currentSum + nums[i], target);
            current.remove(current.size() - 1);  // Backtrack
        }
    }
    
    // Find all subsets that sum to target
    public List<List<Integer>> subsetSum(int[] nums, int target) {
        result = new ArrayList<>();
        backtrack(nums, 0, new ArrayList<>(), 0, target);
        return result;
    }
    
    // Print subsets
    public void printSubsets(List<List<Integer>> subsets, int target) {
        System.out.println("Subsets that sum to " + target + ":");
        for (List<Integer> subset : subsets) {
            System.out.println(subset);
        }
    }
    
    // Example usage
    public static void main(String[] args) {
        SubsetSum ss = new SubsetSum();
        int[] nums = {2, 3, 5, 7};
        int target = 7;
        
        List<List<Integer>> subsets = ss.subsetSum(nums, target);
        ss.printSubsets(subsets, target);
    }
}
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

**Python:**
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

**C:**
```c
#include <stdio.h>
#include <stdbool.h>
#include <string.h>

// Backtracking function
bool backtrack(char board[][100], int rows, int cols, int row, int col, 
               const char* word, int index) {
    if (index == strlen(word)) {
        return true;
    }
    
    if (row < 0 || row >= rows || col < 0 || col >= cols || 
        board[row][col] != word[index]) {
        return false;
    }
    
    // Mark as visited
    char temp = board[row][col];
    board[row][col] = '#';
    
    // Explore 4 directions
    bool found = backtrack(board, rows, cols, row + 1, col, word, index + 1) ||
                 backtrack(board, rows, cols, row - 1, col, word, index + 1) ||
                 backtrack(board, rows, cols, row, col + 1, word, index + 1) ||
                 backtrack(board, rows, cols, row, col - 1, word, index + 1);
    
    // Backtrack
    board[row][col] = temp;
    
    return found;
}

// Search for word in 2D board
bool word_search(char board[][100], int rows, int cols, const char* word) {
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            if (backtrack(board, rows, cols, i, j, word, 0)) {
                return true;
            }
        }
    }
    return false;
}

// Example usage
int main() {
    char board[3][100] = {
        {'A', 'B', 'C', 'E'},
        {'S', 'F', 'C', 'S'},
        {'A', 'D', 'E', 'E'}
    };
    int rows = 3, cols = 4;
    
    printf("ABCCED: %s\n", word_search(board, rows, cols, "ABCCED") ? "True" : "False");
    printf("SEE: %s\n", word_search(board, rows, cols, "SEE") ? "True" : "False");
    printf("ABCB: %s\n", word_search(board, rows, cols, "ABCB") ? "True" : "False");
    
    return 0;
}
```

**C++:**
```cpp
#include <iostream>
#include <vector>
#include <string>

using namespace std;

class WordSearch {
private:
    // Backtracking function
    bool backtrack(vector<vector<char>>& board, int row, int col, 
                   const string& word, int index) {
        if (index == word.length()) {
            return true;
        }
        
        if (row < 0 || row >= board.size() || 
            col < 0 || col >= board[0].size() ||
            board[row][col] != word[index]) {
            return false;
        }
        
        // Mark as visited
        char temp = board[row][col];
        board[row][col] = '#';
        
        // Explore 4 directions
        bool found = backtrack(board, row + 1, col, word, index + 1) ||
                     backtrack(board, row - 1, col, word, index + 1) ||
                     backtrack(board, row, col + 1, word, index + 1) ||
                     backtrack(board, row, col - 1, word, index + 1);
        
        // Backtrack
        board[row][col] = temp;
        
        return found;
    }
    
public:
    // Search for word in 2D board
    bool word_search(vector<vector<char>>& board, const string& word) {
        for (int i = 0; i < board.size(); i++) {
            for (int j = 0; j < board[0].size(); j++) {
                if (backtrack(board, i, j, word, 0)) {
                    return true;
                }
            }
        }
        return false;
    }
};

// Example usage
int main() {
    WordSearch ws;
    vector<vector<char>> board = {
        {'A', 'B', 'C', 'E'},
        {'S', 'F', 'C', 'S'},
        {'A', 'D', 'E', 'E'}
    };
    
    cout << "ABCCED: " << (ws.word_search(board, "ABCCED") ? "True" : "False") << endl;
    cout << "SEE: " << (ws.word_search(board, "SEE") ? "True" : "False") << endl;
    cout << "ABCB: " << (ws.word_search(board, "ABCB") ? "True" : "False") << endl;
    
    return 0;
}
```

**Java:**
```java
public class WordSearch {
    
    // Backtracking function
    private boolean backtrack(char[][] board, int row, int col, String word, int index) {
        if (index == word.length()) {
            return true;
        }
        
        if (row < 0 || row >= board.length || 
            col < 0 || col >= board[0].length ||
            board[row][col] != word.charAt(index)) {
            return false;
        }
        
        // Mark as visited
        char temp = board[row][col];
        board[row][col] = '#';
        
        // Explore 4 directions
        boolean found = backtrack(board, row + 1, col, word, index + 1) ||
                        backtrack(board, row - 1, col, word, index + 1) ||
                        backtrack(board, row, col + 1, word, index + 1) ||
                        backtrack(board, row, col - 1, word, index + 1);
        
        // Backtrack
        board[row][col] = temp;
        
        return found;
    }
    
    // Search for word in 2D board
    public boolean wordSearch(char[][] board, String word) {
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                if (backtrack(board, i, j, word, 0)) {
                    return true;
                }
            }
        }
        return false;
    }
    
    // Example usage
    public static void main(String[] args) {
        WordSearch ws = new WordSearch();
        char[][] board = {
            {'A', 'B', 'C', 'E'},
            {'S', 'F', 'C', 'S'},
            {'A', 'D', 'E', 'E'}
        };
        
        System.out.println("ABCCED: " + ws.wordSearch(board, "ABCCED"));
        System.out.println("SEE: " + ws.wordSearch(board, "SEE"));
        System.out.println("ABCB: " + ws.wordSearch(board, "ABCB"));
    }
}
```

---

## 7. Generate Parentheses

### Problem
Generate all combinations of n pairs of valid parentheses.

### Implementation

**Python:**
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

**C:**
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_RESULT 10000
#define MAX_LEN 100

int result_count = 0;

// Backtracking function to generate parentheses
void backtrack(int n, char* current, int pos, int open_count, int close_count, 
               char result[][MAX_LEN]) {
    if (pos == 2 * n) {
        current[pos] = '\0';
        strcpy(result[result_count], current);
        result_count++;
        return;
    }
    
    if (open_count < n) {
        current[pos] = '(';
        backtrack(n, current, pos + 1, open_count + 1, close_count, result);
    }
    
    if (close_count < open_count) {
        current[pos] = ')';
        backtrack(n, current, pos + 1, open_count, close_count + 1, result);
    }
}

// Generate all valid parentheses combinations
int generate_parentheses(int n, char result[][MAX_LEN]) {
    result_count = 0;
    char current[MAX_LEN];
    backtrack(n, current, 0, 0, 0, result);
    return result_count;
}

// Example usage
int main() {
    int n = 3;
    char result[MAX_RESULT][MAX_LEN];
    
    int count = generate_parentheses(n, result);
    printf("Valid parentheses for n=%d:\n", n);
    for (int i = 0; i < count; i++) {
        printf("%s\n", result[i]);
    }
    printf("Total: %d\n", count);
    
    return 0;
}
```

**C++:**
```cpp
#include <iostream>
#include <vector>
#include <string>

using namespace std;

class GenerateParentheses {
private:
    vector<string> result;
    int n;
    
    // Backtracking function
    void backtrack(string current, int open_count, int close_count) {
        if (current.length() == 2 * n) {
            result.push_back(current);
            return;
        }
        
        if (open_count < n) {
            backtrack(current + '(', open_count + 1, close_count);
        }
        
        if (close_count < open_count) {
            backtrack(current + ')', open_count, close_count + 1);
        }
    }
    
public:
    // Generate all valid parentheses combinations
    vector<string> generate_parentheses(int n_val) {
        n = n_val;
        result.clear();
        backtrack("", 0, 0);
        return result;
    }
};

// Example usage
int main() {
    GenerateParentheses gp;
    int n = 3;
    vector<string> parens = gp.generate_parentheses(n);
    
    cout << "Valid parentheses for n=" << n << ":" << endl;
    for (const string& p : parens) {
        cout << p << endl;
    }
    cout << "Total: " << parens.size() << endl;
    
    return 0;
}
```

**Java:**
```java
import java.util.*;

public class GenerateParentheses {
    private List<String> result;
    private int n;
    
    // Backtracking function
    private void backtrack(String current, int openCount, int closeCount) {
        if (current.length() == 2 * n) {
            result.add(current);
            return;
        }
        
        if (openCount < n) {
            backtrack(current + '(', openCount + 1, closeCount);
        }
        
        if (closeCount < openCount) {
            backtrack(current + ')', openCount, closeCount + 1);
        }
    }
    
    // Generate all valid parentheses combinations
    public List<String> generateParentheses(int n) {
        this.n = n;
        result = new ArrayList<>();
        backtrack("", 0, 0);
        return result;
    }
    
    // Example usage
    public static void main(String[] args) {
        GenerateParentheses gp = new GenerateParentheses();
        int n = 3;
        List<String> parens = gp.generateParentheses(n);
        
        System.out.println("Valid parentheses for n=" + n + ":");
        for (String p : parens) {
            System.out.println(p);
        }
        System.out.println("Total: " + parens.size());
    }
}
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

**Python:**
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

**C:**
```c
void backtrack(State* state) {
    if (is_goal(state)) {
        save(state);
        return;
    }
    
    Choice* choices;
    int num_choices = get_choices(state, &choices);
    
    for (int i = 0; i < num_choices; i++) {
        make_choice(state, choices[i]);
        backtrack(state);
        undo_choice(state, choices[i]);  // Backtrack
    }
}
```

**C++:**
```cpp
void backtrack(State& state) {
    if (is_goal(state)) {
        save(state);
        return;
    }
    
    std::vector<Choice> choices = get_choices(state);
    
    for (const Choice& choice : choices) {
        make_choice(state, choice);
        backtrack(state);
        undo_choice(state, choice);  // Backtrack
    }
}
```

**Java:**
```java
public void backtrack(State state) {
    if (isGoal(state)) {
        save(state);
        return;
    }
    
    List<Choice> choices = getChoices(state);
    
    for (Choice choice : choices) {
        makeChoice(state, choice);
        backtrack(state);
        undoChoice(state, choice);  // Backtrack
    }
}
```

### Pattern 2: With Pruning

**Python:**
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

**C:**
```c
void backtrack(State* state) {
    if (is_invalid(state)) {
        return;  // Prune
    }
    
    if (is_goal(state)) {
        save(state);
        return;
    }
    
    Choice* choices;
    int num_choices = get_choices(state, &choices);
    
    for (int i = 0; i < num_choices; i++) {
        State* new_state = make_choice(state, choices[i]);
        backtrack(new_state);
    }
}
```

**C++:**
```cpp
void backtrack(const State& state) {
    if (is_invalid(state)) {
        return;  // Prune
    }
    
    if (is_goal(state)) {
        save(state);
        return;
    }
    
    std::vector<Choice> choices = get_choices(state);
    
    for (const Choice& choice : choices) {
        backtrack(make_choice(state, choice));
    }
}
```

**Java:**
```java
public void backtrack(State state) {
    if (isInvalid(state)) {
        return;  // Prune
    }
    
    if (isGoal(state)) {
        save(state);
        return;
    }
    
    List<Choice> choices = getChoices(state);
    
    for (Choice choice : choices) {
        backtrack(makeChoice(state, choice));
    }
}
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

**Python:**
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

**C:**
```c
void backtrack(State* state) {
    // 1. Base case (goal reached)
    if (is_goal(state)) {
        record_solution(state);
        return;
    }
    
    // 2. Pruning (invalid state)
    if (is_invalid(state)) {
        return;
    }
    
    // 3. Explore choices
    Choice* choices;
    int num_choices = get_choices(state, &choices);
    
    for (int i = 0; i < num_choices; i++) {
        // 4. Make choice
        add_choice(state, choices[i]);
        
        // 5. Recurse
        backtrack(state);
        
        // 6. Undo choice (backtrack)
        remove_choice(state, choices[i]);
    }
}
```

**C++:**
```cpp
void backtrack(State& state) {
    // 1. Base case (goal reached)
    if (is_goal(state)) {
        record_solution(state);
        return;
    }
    
    // 2. Pruning (invalid state)
    if (is_invalid(state)) {
        return;
    }
    
    // 3. Explore choices
    std::vector<Choice> choices = get_choices(state);
    
    for (const Choice& choice : choices) {
        // 4. Make choice
        state.add(choice);
        
        // 5. Recurse
        backtrack(state);
        
        // 6. Undo choice (backtrack)
        state.remove(choice);
    }
}
```

**Java:**
```java
public void backtrack(State state) {
    // 1. Base case (goal reached)
    if (isGoal(state)) {
        recordSolution(state);
        return;
    }
    
    // 2. Pruning (invalid state)
    if (isInvalid(state)) {
        return;
    }
    
    // 3. Explore choices
    List<Choice> choices = getChoices(state);
    
    for (Choice choice : choices) {
        // 4. Make choice
        state.add(choice);
        
        // 5. Recurse
        backtrack(state);
        
        // 6. Undo choice (backtrack)
        state.remove(choice);
    }
}
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
