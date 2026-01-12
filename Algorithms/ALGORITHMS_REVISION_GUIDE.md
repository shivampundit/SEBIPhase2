# Algorithms - Complete Revision Guide

## Quick Reference: All Key Concepts, Tables & Formulas

---

## 1. SORTING ALGORITHMS

### Time Complexity Table

| Algorithm | Best Case | Average Case | Worst Case | Space | Stable | Method |
|-----------|-----------|--------------|------------|-------|--------|--------|
| **Bubble Sort** | O(n) | O(n²) | O(n²) | O(1) | Yes | Comparison |
| **Selection Sort** | O(n²) | O(n²) | O(n²) | O(1) | No | Comparison |
| **Insertion Sort** | O(n) | O(n²) | O(n²) | O(1) | Yes | Comparison |
| **Merge Sort** | O(n log n) | O(n log n) | O(n log n) | O(n) | Yes | Divide & Conquer |
| **Quick Sort** | O(n log n) | O(n log n) | O(n²) | O(log n) | No | Divide & Conquer |
| **Heap Sort** | O(n log n) | O(n log n) | O(n log n) | O(1) | No | Heap |
| **Counting Sort** | O(n+k) | O(n+k) | O(n+k) | O(k) | Yes | Non-comparison |
| **Radix Sort** | O(d(n+k)) | O(d(n+k)) | O(d(n+k)) | O(n+k) | Yes | Non-comparison |

**Key Points:**
- **Stable**: Maintains relative order of equal elements
- **In-place**: Uses O(1) extra space
- **k**: Range of input, **d**: Number of digits

### Quick Sort - Partition Logic
```python
# Key formula: Pivot placement
left = [x for x in arr if x < pivot]
middle = [x for x in arr if x == pivot]
right = [x for x in arr if x > pivot]
```

### Merge Sort - Merge Process
```python
# Merging two sorted arrays
while i < len(left) and j < len(right):
    if left[i] <= right[j]:
        result.append(left[i])
        i += 1
    else:
        result.append(right[j])
        j += 1
```

### When to Use Which?

| Scenario | Best Choice | Reason |
|----------|-------------|--------|
| Small dataset (n < 50) | Insertion Sort | Simple, efficient for small n |
| Nearly sorted | Insertion Sort | O(n) best case |
| Guaranteed O(n log n) | Merge Sort | No worst case O(n²) |
| Limited space | Heap Sort | O(1) space, O(n log n) time |
| Average case performance | Quick Sort | Fastest average case |
| Stability required | Merge/Bubble | Preserve order |
| Integer range known | Counting/Radix | Linear time possible |

---

## 2. SEARCHING ALGORITHMS

### Time Complexity Comparison

| Algorithm | Time Complexity | Space | Prerequisites |
|-----------|----------------|-------|---------------|
| **Linear Search** | O(n) | O(1) | None |
| **Binary Search** | O(log n) | O(1) iterative, O(log n) recursive | Sorted array |
| **Jump Search** | O(√n) | O(1) | Sorted array |
| **Interpolation Search** | O(log log n) avg, O(n) worst | O(1) | Sorted, uniformly distributed |
| **Exponential Search** | O(log n) | O(1) | Sorted, unbounded |
| **Ternary Search** | O(log₃ n) | O(1) | Unimodal function |

### Binary Search Formula
```python
# Key formulas
mid = low + (high - low) // 2  # Avoid overflow

# Iterations needed
iterations = ceil(log₂(n))

# Example: n = 1024 → iterations = 10
```

### Binary Search Variations

| Variant | Formula | Use Case |
|---------|---------|----------|
| **First Occurrence** | Move right even on match | Find leftmost duplicate |
| **Last Occurrence** | Move left even on match | Find rightmost duplicate |
| **Floor** | Track last smaller element | Largest ≤ target |
| **Ceiling** | Track first larger element | Smallest ≥ target |

```python
# First occurrence pattern
if arr[mid] == target:
    result = mid
    high = mid - 1  # Continue searching left

# Last occurrence pattern
if arr[mid] == target:
    result = mid
    low = mid + 1   # Continue searching right
```

### Jump Search - Optimal Jump Size
```python
# Formula for optimal jump
jump = √n

# Why? Minimizes comparisons
# Jumps = n/√n = √n
# Linear checks = √n
# Total = 2√n = O(√n)
```

---

## 3. GREEDY ALGORITHMS

### Greedy Choice Property

**Key Concept**: Make locally optimal choice at each step, hoping to find global optimum.

### Classic Problems Summary

| Problem | Greedy Strategy | Time | Space |
|---------|----------------|------|-------|
| **Activity Selection** | Sort by finish time, select earliest | O(n log n) | O(1) |
| **Fractional Knapsack** | Sort by value/weight ratio, take highest | O(n log n) | O(1) |
| **Job Sequencing** | Sort by profit, schedule in free slot | O(n²) | O(n) |
| **Huffman Coding** | Build tree from min frequency nodes | O(n log n) | O(n) |
| **Coin Change (Greedy)** | Take largest coin first | O(n) | O(1) |
| **Prim's MST** | Add min edge to growing tree | O(E log V) | O(V) |
| **Kruskal's MST** | Add min edge if no cycle | O(E log E) | O(V) |

### Activity Selection - Key Formula
```python
# Greedy criterion
activities.sort(key=lambda x: x[1])  # Sort by finish time

# Selection logic
if start_i >= finish_prev:
    select(activity_i)
```

### Fractional Knapsack - Value/Weight Ratio
```python
# Formula
ratio = value / weight

# Total value
total_value = Σ(ratio_i × min(weight_i, remaining_capacity))
```

### Huffman Coding - Frequency Tree
```python
# Build tree bottom-up
while len(heap) > 1:
    left = heappop(heap)
    right = heappop(heap)
    merged = left.freq + right.freq
    heappush(heap, Node(merged, left, right))

# Code length formula
average_length = Σ(frequency_i × depth_i)
```

### Minimum Spanning Tree Formulas

**Prim's Algorithm:**
```
Start with any vertex
Repeat V-1 times:
    Add minimum edge connecting tree to non-tree vertex
```

**Kruskal's Algorithm:**
```
Sort all edges by weight
For each edge (u,v):
    If u and v in different components:
        Add edge to MST
        Union(u, v)
```

**MST Properties:**
- Edges in MST = V - 1
- Total weight = minimum possible
- Removing any edge disconnects tree

---

## 4. DYNAMIC PROGRAMMING

### DP Identification Checklist

✓ **Optimal Substructure**: Solution built from optimal solutions of subproblems  
✓ **Overlapping Subproblems**: Same subproblem solved multiple times  
✓ **Recurrence Relation**: Can express solution recursively

### DP Patterns & Recurrence Relations

#### 1. Fibonacci Numbers
```python
# Recurrence
F(n) = F(n-1) + F(n-2)
Base: F(0) = 0, F(1) = 1

# Time: O(n), Space: O(n) tabulation, O(1) optimized
```

#### 2. Longest Common Subsequence (LCS)
```python
# Recurrence
if s1[i] == s2[j]:
    LCS[i][j] = 1 + LCS[i-1][j-1]
else:
    LCS[i][j] = max(LCS[i-1][j], LCS[i][j-1])

# Time: O(m×n), Space: O(m×n)
```

#### 3. 0/1 Knapsack
```python
# Recurrence
if weight[i] <= capacity:
    DP[i][c] = max(
        value[i] + DP[i-1][c-weight[i]],  # Include
        DP[i-1][c]                         # Exclude
    )
else:
    DP[i][c] = DP[i-1][c]

# Time: O(n×W), Space: O(n×W)
```

#### 4. Coin Change (Min Coins)
```python
# Recurrence
DP[amount] = 1 + min(DP[amount - coin] for coin in coins)
Base: DP[0] = 0

# Time: O(n×amount), Space: O(amount)
```

#### 5. Longest Increasing Subsequence (LIS)
```python
# Recurrence (O(n²))
for i in range(n):
    for j in range(i):
        if arr[j] < arr[i]:
            DP[i] = max(DP[i], 1 + DP[j])

# Binary search approach: O(n log n)
```

#### 6. Edit Distance
```python
# Recurrence
if s1[i] == s2[j]:
    DP[i][j] = DP[i-1][j-1]
else:
    DP[i][j] = 1 + min(
        DP[i-1][j],      # Delete
        DP[i][j-1],      # Insert
        DP[i-1][j-1]     # Replace
    )

# Time: O(m×n), Space: O(m×n)
```

#### 7. Matrix Chain Multiplication
```python
# Recurrence
DP[i][j] = min(
    DP[i][k] + DP[k+1][j] + cost[i][k][j]
    for k in range(i, j)
)

# Cost formula
cost = p[i-1] × p[k] × p[j]

# Time: O(n³), Space: O(n²)
```

#### 8. Rod Cutting
```python
# Recurrence
DP[n] = max(price[i] + DP[n-i-1] for i in range(n))

# Time: O(n²), Space: O(n)
```

### DP Problem Types

| Type | Example | Recurrence Pattern |
|------|---------|-------------------|
| **Linear** | Fibonacci, Climbing Stairs | F(n) depends on F(n-1), F(n-2) |
| **Grid** | Unique Paths, Min Path Sum | DP[i][j] from DP[i-1][j], DP[i][j-1] |
| **String** | LCS, Edit Distance | 2D table on strings |
| **Knapsack** | 0/1, Unbounded | Include/exclude decision |
| **Intervals** | Matrix Chain | Try all split points |
| **Subset** | Subset Sum, Partition | Include/exclude elements |

### Memoization vs Tabulation

| Aspect | Memoization (Top-Down) | Tabulation (Bottom-Up) |
|--------|----------------------|----------------------|
| **Approach** | Recursive + caching | Iterative |
| **Space** | O(n) + recursion stack | O(n) |
| **Ease** | Easier to write | Requires planning |
| **Performance** | Slower (function calls) | Faster |
| **Computes** | Only needed subproblems | All subproblems |

---

## 5. BACKTRACKING

### Backtracking Template
```python
def backtrack(state, choices):
    # Base case
    if is_solution(state):
        add_to_results(state)
        return
    
    # Try each choice
    for choice in choices:
        if is_valid(choice, state):
            # Make choice
            make_choice(state, choice)
            
            # Recurse
            backtrack(state, remaining_choices)
            
            # Undo choice (backtrack)
            undo_choice(state, choice)
```

### Classic Problems Summary

| Problem | Time Complexity | Space | Key Constraint |
|---------|----------------|-------|----------------|
| **N-Queens** | O(N!) | O(N²) | No two queens attack |
| **Sudoku Solver** | O(9^m) m=empty cells | O(1) | Row/col/box unique |
| **Permutations** | O(n! × n) | O(n) | All arrangements |
| **Combinations** | O(2^n × n) | O(n) | Select k from n |
| **Subset Sum** | O(2^n) | O(n) | Sum equals target |
| **Word Search** | O(m×n×4^L) | O(L) | Path exists in grid |
| **Generate Parentheses** | O(4^n / √n) | O(n) | Valid combinations |

### N-Queens - Attack Conditions
```python
# Queen attacks if:
# 1. Same row: row1 == row2
# 2. Same column: col1 == col2
# 3. Same diagonal: |row1-row2| == |col1-col2|

# Check diagonal
def not_attacking(r1, c1, r2, c2):
    return c1 != c2 and abs(r1-r2) != abs(c1-c2)
```

### Sudoku - Validation Formula
```python
# Box number for cell (i,j)
box = (i // 3) * 3 + (j // 3)

# Check valid placement
def is_valid(board, row, col, num):
    # Check row
    if num in board[row]: return False
    
    # Check column
    if num in [board[i][col] for i in range(9)]: return False
    
    # Check 3×3 box
    box_row, box_col = 3*(row//3), 3*(col//3)
    for i in range(box_row, box_row+3):
        for j in range(box_col, box_col+3):
            if board[i][j] == num: return False
    
    return True
```

### Combinations Formula
```python
# C(n, k) = n! / (k! × (n-k)!)

# Number of combinations
from math import comb
total = comb(n, k)

# Backtracking generates all
```

### Subset Sum - Pruning Optimization
```python
# Prune if:
# 1. Current sum > target → stop
# 2. Current sum + remaining < target → stop

def subset_sum(nums, target, index, current_sum):
    if current_sum == target:
        return True
    if current_sum > target or index == len(nums):
        return False
    
    # Include or exclude
    return (subset_sum(nums, target, index+1, current_sum + nums[index]) or
            subset_sum(nums, target, index+1, current_sum))
```

---

## 6. DIVIDE AND CONQUER

### Master Theorem

**For recurrences: T(n) = aT(n/b) + f(n)**

| Case | Condition | Solution |
|------|-----------|----------|
| **Case 1** | f(n) = O(n^c) where c < log_b(a) | T(n) = Θ(n^(log_b(a))) |
| **Case 2** | f(n) = Θ(n^c) where c = log_b(a) | T(n) = Θ(n^c × log n) |
| **Case 3** | f(n) = Ω(n^c) where c > log_b(a) | T(n) = Θ(f(n)) |

### Common Recurrences

| Algorithm | Recurrence | a | b | f(n) | Solution |
|-----------|------------|---|---|------|----------|
| **Binary Search** | T(n) = T(n/2) + O(1) | 1 | 2 | O(1) | O(log n) |
| **Merge Sort** | T(n) = 2T(n/2) + O(n) | 2 | 2 | O(n) | O(n log n) |
| **Quick Sort** | T(n) = 2T(n/2) + O(n) | 2 | 2 | O(n) | O(n log n) avg |
| **Binary Tree** | T(n) = 2T(n/2) + O(1) | 2 | 2 | O(1) | O(n) |
| **Karatsuba** | T(n) = 3T(n/2) + O(n) | 3 | 2 | O(n) | O(n^1.59) |
| **Strassen** | T(n) = 7T(n/2) + O(n²) | 7 | 2 | O(n²) | O(n^2.81) |

### Maximum Subarray (Kadane's Algorithm)
```python
# Divide and conquer: O(n log n)
def max_crossing_sum(arr, low, mid, high):
    left_sum = max(sum from mid to low)
    right_sum = max(sum from mid+1 to high)
    return left_sum + right_sum

# Kadane's (greedy): O(n)
max_ending_here = max(0, max_ending_here + arr[i])
max_so_far = max(max_so_far, max_ending_here)
```

### Closest Pair of Points
```python
# Formula: Euclidean distance
distance = √((x1-x2)² + (y1-y2)²)

# Divide and Conquer
# 1. Sort by x-coordinate: O(n log n)
# 2. Divide into two halves
# 3. Recursively find min in each half
# 4. Find min in strip around middle
# Total: O(n log n)
```

### Strassen's Matrix Multiplication
```python
# Standard: O(n³)
C[i][j] = Σ(A[i][k] × B[k][j])

# Strassen's: O(n^2.81)
# 7 multiplications instead of 8
M1 = (A11 + A22) × (B11 + B22)
M2 = (A21 + A22) × B11
M3 = A11 × (B12 - B22)
M4 = A22 × (B21 - B11)
M5 = (A11 + A12) × B22
M6 = (A21 - A11) × (B11 + B12)
M7 = (A12 - A22) × (B21 + B22)

# Result
C11 = M1 + M4 - M5 + M7
C12 = M3 + M5
C21 = M2 + M4
C22 = M1 - M2 + M3 + M6
```

### Power Function Optimization
```python
# Naive: O(n)
result = x × x × ... × x  (n times)

# Divide and Conquer: O(log n)
def power(x, n):
    if n == 0: return 1
    half = power(x, n // 2)
    if n % 2 == 0:
        return half × half
    else:
        return x × half × half

# Formula
x^n = (x^(n/2))² if n even
x^n = x × (x^(n//2))² if n odd
```

---

## 7. PATTERN SEARCHING

### Algorithm Comparison Table

| Algorithm | Preprocessing | Searching | Total | Space | Best For |
|-----------|--------------|-----------|-------|-------|----------|
| **Naive** | O(0) | O(m×n) | O(m×n) | O(1) | Short texts |
| **KMP** | O(m) | O(n) | O(m+n) | O(m) | Long patterns |
| **Rabin-Karp** | O(m) | O(n) avg | O(m+n) avg | O(1) | Multiple patterns |
| **Boyer-Moore** | O(m+σ) | O(n/m) best | O(m+n+σ) | O(σ) | Large alphabets |
| **Z-Algorithm** | O(0) | O(m+n) | O(m+n) | O(m+n) | Multiple searches |
| **Aho-Corasick** | O(Σm_i) | O(n+z) | O(Σm_i+n) | O(Σm_i) | Multiple patterns |

**where**: m = pattern length, n = text length, σ = alphabet size, z = matches

### KMP - LPS Array Construction
```python
# Longest Proper Prefix which is also Suffix
# Example: "AABAAC"
# LPS:     [0,1,0,1,2,0]

def compute_lps(pattern):
    lps = [0] * len(pattern)
    length = 0
    i = 1
    
    while i < len(pattern):
        if pattern[i] == pattern[length]:
            length += 1
            lps[i] = length
            i += 1
        else:
            if length != 0:
                length = lps[length - 1]
            else:
                lps[i] = 0
                i += 1
    
    return lps

# Key insight: Skip comparisons using LPS
# No need to re-compare matched prefix
```

### Rabin-Karp - Rolling Hash Formula
```python
# Hash function
hash(s) = s[0]×d^(m-1) + s[1]×d^(m-2) + ... + s[m-1]×d^0
         mod q

# Rolling hash (remove old, add new)
hash_new = (d × (hash_old - s[old]×d^(m-1)) + s[new]) mod q

# Example: d=256 (ASCII), q=101 (prime)
# Pattern "ABC": hash = 65×256² + 66×256 + 67 mod 101
```

### Boyer-Moore - Bad Character Rule
```python
# Shift pattern based on mismatched character
# 1. If character not in pattern: shift by m
# 2. If character in pattern: align rightmost occurrence

# Bad character table
bad_char = {}
for i in range(len(pattern)):
    bad_char[pattern[i]] = i

# Shift calculation
shift = max(1, j - bad_char.get(text[i+j], -1))
```

### Z-Algorithm - Z-Array
```python
# Z[i] = length of longest substring starting from i
#        which is also prefix of string

# Example: "aabcaabxaaaz"
# Z-array: [12,1,0,0,3,1,0,0,2,2,1,0]

def z_algorithm(s):
    n = len(s)
    z = [0] * n
    l, r = 0, 0
    
    for i in range(1, n):
        if i > r:
            l, r = i, i
            while r < n and s[r] == s[r-l]:
                r += 1
            z[i] = r - l
            r -= 1
        else:
            k = i - l
            if z[k] < r - i + 1:
                z[i] = z[k]
            else:
                l = i
                while r < n and s[r] == s[r-l]:
                    r += 1
                z[i] = r - l
                r -= 1
    
    return z
```

### Pattern Matching - Key Formulas

**Probability of spurious hit (Rabin-Karp):**
```
P(false positive) = 1/q (where q is prime modulus)
```

**Expected comparisons (Boyer-Moore):**
```
Best case: O(n/m) - skip large portions
Worst case: O(m×n) - all characters match
```

**Aho-Corasick complexity:**
```
Build automaton: O(Σm_i)
Search: O(n + number of matches)
Space: O(ALPHABET_SIZE × Σm_i)
```

---

## COMPREHENSIVE FORMULA SHEET

### Big O Notations (Ascending Order)
```
O(1) < O(log log n) < O(log n) < O(√n) < O(n) < O(n log n) < 
O(n²) < O(n³) < O(2^n) < O(n!) < O(n^n)
```

### Logarithm Properties
```
log(a×b) = log(a) + log(b)
log(a/b) = log(a) - log(b)
log(a^b) = b × log(a)
log_a(b) = log(b) / log(a)
log₂(n) ≈ 3.32 × log₁₀(n)
```

### Combinatorics
```
Permutations: P(n,r) = n! / (n-r)!
Combinations: C(n,r) = n! / (r! × (n-r)!)
Subsets: 2^n
Binary strings of length n: 2^n
```

### Summation Formulas
```
Σ(i=1 to n) i = n(n+1)/2
Σ(i=1 to n) i² = n(n+1)(2n+1)/6
Σ(i=0 to n) 2^i = 2^(n+1) - 1
Σ(i=1 to n) 1/i ≈ ln(n)
```

### Recurrence Relations
```
Fibonacci: T(n) = T(n-1) + T(n-2) → O(φ^n) where φ = 1.618
Binary search: T(n) = T(n/2) + 1 → O(log n)
Merge sort: T(n) = 2T(n/2) + n → O(n log n)
```

---

## ALGORITHM SELECTION FLOWCHART

### For Sorting
```
Is data nearly sorted? → Yes → Insertion Sort O(n)
                       → No ↓
Is stability required? → Yes → Merge Sort O(n log n)
                       → No ↓
Is space limited? → Yes → Heap Sort O(n log n), O(1) space
                  → No ↓
General case → Quick Sort O(n log n) average
```

### For Searching
```
Is data sorted? → No → Linear Search O(n)
                → Yes ↓
Is data uniformly distributed? → Yes → Interpolation O(log log n)
                                → No ↓
Is data large? → Yes → Binary Search O(log n)
               → No → Linear/Jump Search
```

### For Optimization
```
Overlapping subproblems? → Yes → Dynamic Programming
                         → No ↓
Greedy choice property? → Yes → Greedy Algorithm
                        → No ↓
Try all possibilities → Backtracking
```

---

## QUICK REVISION CHECKLIST

### ✅ Must Remember

**Sorting:**
- [ ] Merge Sort: Divide, merge with O(n log n)
- [ ] Quick Sort: Partition around pivot
- [ ] Heap Sort: Build heap, extract max
- [ ] Stability: Merge, Insertion, Bubble are stable

**Searching:**
- [ ] Binary Search: mid = low + (high-low)//2
- [ ] O(log n) requires sorted array
- [ ] Jump Search: Jump by √n

**Greedy:**
- [ ] Activity Selection: Sort by finish time
- [ ] Fractional Knapsack: Sort by value/weight
- [ ] MST: Prim's (vertex-based), Kruskal's (edge-based)

**Dynamic Programming:**
- [ ] LCS: 2D table, match or max(left, top)
- [ ] 0/1 Knapsack: Include/exclude decision
- [ ] Coin Change: 1 + min(DP[amount-coin])
- [ ] LIS: O(n²) DP or O(n log n) binary search

**Backtracking:**
- [ ] N-Queens: Check row, column, diagonal
- [ ] Sudoku: Validate row, column, 3×3 box
- [ ] Always undo choice after recursion

**Divide & Conquer:**
- [ ] Master Theorem: T(n) = aT(n/b) + f(n)
- [ ] Merge Sort, Quick Sort, Binary Search
- [ ] Maximum Subarray: O(n log n) or Kadane's O(n)

**Pattern Matching:**
- [ ] KMP: Build LPS array O(m), search O(n)
- [ ] Rabin-Karp: Rolling hash O(n) average
- [ ] Boyer-Moore: Bad character rule, best O(n/m)

---

## EXAM TIPS

### Time Complexity Analysis
1. **Single loop**: O(n)
2. **Nested loops**: O(n²), O(n³), etc.
3. **Halving each time**: O(log n)
4. **Divide and conquer**: Apply Master Theorem
5. **Recursion with memoization**: Number of unique states

### Space Complexity Analysis
1. **Fixed variables**: O(1)
2. **Single array**: O(n)
3. **2D array**: O(n²)
4. **Recursion depth**: O(depth)
5. **Hash table/set**: O(unique elements)

### Common Mistakes to Avoid
- ❌ Using 0-indexed array in 1-indexed problem
- ❌ Not handling empty array/string
- ❌ Integer overflow in calculations
- ❌ Forgetting to check bounds
- ❌ Not considering duplicate elements
- ❌ Incorrect loop termination condition

### Debugging Strategy
1. **Trace with small input**: n=3 or n=5
2. **Check base cases**: Empty, single element
3. **Verify loop bounds**: Off-by-one errors
4. **Print intermediate values**: Step-by-step
5. **Test edge cases**: Max/min values, negatives

---

## FINAL MEMORIZATION

### 🔥 Top 10 Must-Know Complexities
1. Binary Search: **O(log n)**
2. Merge Sort: **O(n log n)**
3. Quick Sort: **O(n log n) average, O(n²) worst**
4. Hash Table: **O(1) average**
5. DFS/BFS: **O(V + E)**
6. Dijkstra: **O((V+E) log V)**
7. Fibonacci (DP): **O(n)**
8. LCS: **O(m×n)**
9. 0/1 Knapsack: **O(n×W)**
10. KMP: **O(m+n)**

### 🎯 Pattern Recognition
- **Two pointers**: Sorted array problems
- **Sliding window**: Subarray/substring problems
- **Hash map**: Frequency counting, lookup
- **Stack**: Balanced parentheses, next greater
- **Queue**: Level-order, BFS
- **Recursion + Memo**: DP problems
- **Sorting first**: Often simplifies problem

---

**🚀 Master these concepts and you're ready for the exam!**
