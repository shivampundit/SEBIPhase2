# ALGORITHMS - QUICK REVISION GUIDE (Multi-Language)
## For SEBI Phase 2 Exam Preparation

**Purpose**: Last 60-75 minutes revision before exam  
**Coverage**: Sorting, Searching, Greedy, DP, Backtracking, Divide & Conquer, Pattern Searching  
**Languages**: C, C++, Java, Python  
**Focus**: Algorithm patterns, complexity, when-to-use

---

## TABLE OF CONTENTS
1. [Sorting Algorithms Overview](#1-sorting-algorithms-overview)
2. [Searching Algorithms](#2-searching-algorithms)
3. [Greedy Algorithms](#3-greedy-algorithms)
4. [Dynamic Programming Patterns](#4-dynamic-programming-patterns)
5. [Backtracking](#5-backtracking)
6. [Divide and Conquer](#6-divide-and-conquer)
7. [Pattern Searching](#7-pattern-searching)
8. [Algorithm Selection Guide](#8-algorithm-selection-guide)
9. [Complexity Cheat Sheet](#9-complexity-cheat-sheet)
10. [Common Patterns & Tricks](#10-common-patterns--tricks)
11. [Multi-Language Syntax Quick Reference](#11-multi-language-syntax-quick-reference)
12. [Interview Quick Checks](#12-interview-quick-checks)
13. [Final 5-Minute Checklist](#13-final-5-minute-checklist)

---

## 1. SORTING ALGORITHMS OVERVIEW

### 1.1 Comparison of Sorting Algorithms

| Algorithm | Best | Average | Worst | Space | Stable | In-Place | When to Use |
|-----------|------|---------|-------|-------|--------|----------|-------------|
| **Bubble Sort** | O(n) | O(n²) | O(n²) | O(1) | ✅ | ✅ | Never in production, educational only |
| **Selection Sort** | O(n²) | O(n²) | O(n²) | O(1) | ❌ | ✅ | Small arrays, memory constrained |
| **Insertion Sort** | O(n) | O(n²) | O(n²) | O(1) | ✅ | ✅ | Nearly sorted data, small arrays |
| **Merge Sort** | O(n log n) | O(n log n) | O(n log n) | O(n) | ✅ | ❌ | Need stability + guaranteed O(n log n) |
| **Quick Sort** | O(n log n) | O(n log n) | O(n²) | O(log n) | ❌ | ✅ | General purpose, average-case performance |
| **Heap Sort** | O(n log n) | O(n log n) | O(n log n) | O(1) | ❌ | ✅ | Guaranteed time + space constrained |
| **Counting Sort** | O(n+k) | O(n+k) | O(n+k) | O(k) | ✅ | ❌ | Small integer range |
| **Radix Sort** | O(d(n+k)) | O(d(n+k)) | O(d(n+k)) | O(n+k) | ✅ | ❌ | Fixed-length integers/strings |
| **Tim Sort** | O(n) | O(n log n) | O(n log n) | O(n) | ✅ | ❌ | Python/Java default, real-world data |

**Key**:
- k = range of input values
- d = number of digits
- Stable = maintains relative order of equal elements
- In-Place = O(1) auxiliary space (excluding recursion stack)

### 1.2 Sorting Algorithm Implementations (Core Loops)

#### Bubble Sort
```
C/C++/Java:
for (int i = 0; i < n-1; i++)
    for (int j = 0; j < n-i-1; j++)
        if (arr[j] > arr[j+1])
            swap(arr[j], arr[j+1]);

Python:
for i in range(n-1):
    for j in range(n-i-1):
        if arr[j] > arr[j+1]:
            arr[j], arr[j+1] = arr[j+1], arr[j]
```

#### Selection Sort
```
C/C++/Java:
for (int i = 0; i < n-1; i++) {
    int min_idx = i;
    for (int j = i+1; j < n; j++)
        if (arr[j] < arr[min_idx])
            min_idx = j;
    swap(arr[i], arr[min_idx]);
}

Python:
for i in range(n-1):
    min_idx = min(range(i, n), key=arr.__getitem__)
    arr[i], arr[min_idx] = arr[min_idx], arr[i]
```

#### Quick Sort (Partition Logic)
```
C/C++/Java:
int partition(int arr[], int low, int high) {
    int pivot = arr[high];
    int i = low - 1;
    for (int j = low; j < high; j++) {
        if (arr[j] < pivot)
            swap(arr[++i], arr[j]);
    }
    swap(arr[i+1], arr[high]);
    return i + 1;
}

Python:
def partition(arr, low, high):
    pivot = arr[high]
    i = low - 1
    for j in range(low, high):
        if arr[j] < pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    arr[i+1], arr[high] = arr[high], arr[i+1]
    return i + 1
```

### 1.3 When to Use Which Sort

| Scenario | Best Choice | Reason |
|----------|-------------|--------|
| Nearly sorted data | **Insertion Sort** | O(n) for nearly sorted |
| Need stability | **Merge Sort** or Tim Sort | Maintains relative order |
| Large dataset, guaranteed time | **Merge Sort** or Heap Sort | O(n log n) worst-case |
| Space constrained | **Heap Sort** | O(1) space |
| General purpose | **Quick Sort** | Best average performance |
| Small integers (range k) | **Counting Sort** | O(n+k) linear time |
| Fixed-length int/strings | **Radix Sort** | O(d(n+k)) |
| Library sort (Python/Java) | **Tim Sort** | Hybrid, optimized for real data |
| Already using heap | **Heap Sort** | Reuse heap structure |

---

## 2. SEARCHING ALGORITHMS

### 2.1 Search Algorithm Comparison

| Algorithm | Time | Space | Requires | Use Case |
|-----------|------|-------|----------|----------|
| **Linear Search** | O(n) | O(1) | Nothing | Unsorted data, small arrays |
| **Binary Search** | O(log n) | O(1) iterative, O(log n) recursive | **Sorted array** | Sorted data, large arrays |
| **Jump Search** | O(√n) | O(1) | Sorted array | Better than linear, simpler than binary |
| **Interpolation Search** | O(log log n) avg, O(n) worst | O(1) | Sorted + **uniformly distributed** | Uniformly distributed data |
| **Exponential Search** | O(log n) | O(1) | Sorted array | Unbounded/infinite arrays |

### 2.2 Binary Search Patterns

#### Standard Binary Search
```
C/C++/Java:
int binarySearch(int arr[], int n, int target) {
    int left = 0, right = n - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;  // Avoid overflow
        if (arr[mid] == target) return mid;
        if (arr[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    return -1;
}

Python:
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

#### Find First Occurrence (Lower Bound)
```
while (left < right) {
    int mid = left + (right - left) / 2;
    if (arr[mid] < target)
        left = mid + 1;
    else
        right = mid;  // Don't exclude mid
}
return left;
```

#### Find Last Occurrence (Upper Bound)
```
while (left < right) {
    int mid = left + (right - left + 1) / 2;  // Bias toward right
    if (arr[mid] > target)
        right = mid - 1;
    else
        left = mid;  // Don't exclude mid
}
return left;
```

### 2.3 Search Patterns Summary

| Pattern | Condition | Update | Result |
|---------|-----------|--------|--------|
| **Standard** | `arr[mid] == target` | `left=mid+1` or `right=mid-1` | Exact match or -1 |
| **Lower Bound** | `arr[mid] < target` | `left=mid+1`, `right=mid` | First ≥ target |
| **Upper Bound** | `arr[mid] > target` | `right=mid-1`, `left=mid` | Last ≤ target |
| **Search Range** | Combine lower + upper | Both patterns | [first, last] |

---

## 3. GREEDY ALGORITHMS

### 3.1 Greedy Algorithm Characteristics

| Feature | Description |
|---------|-------------|
| **Strategy** | Make locally optimal choice at each step |
| **No Backtracking** | Once choice made, never reconsidered |
| **Requirements** | Optimal substructure + Greedy choice property |
| **Complexity** | Usually O(n log n) or better |
| **When Fails** | Problems requiring global optimization (use DP instead) |

### 3.2 Classic Greedy Problems

| Problem | Greedy Choice | Time | Key Insight |
|---------|---------------|------|-------------|
| **Activity Selection** | Select earliest ending activity | O(n log n) | Sort by end time, pick non-overlapping |
| **Fractional Knapsack** | Take highest value/weight ratio first | O(n log n) | Can take fractions, unlike 0/1 knapsack |
| **Huffman Coding** | Merge two minimum frequency nodes | O(n log n) | Use min-heap, build tree bottom-up |
| **Dijkstra's Shortest Path** | Visit nearest unvisited vertex | O(E log V) | Works for non-negative weights only |
| **Prim's MST** | Add minimum weight edge to tree | O(E log V) | Greedy on edge weights |
| **Kruskal's MST** | Add minimum weight edge (no cycle) | O(E log E) | Sort edges, use Union-Find |
| **Job Sequencing** | Sort by profit, schedule if possible | O(n log n) | Use deadline slots |
| **Coin Change (specific coins)** | Use largest denomination first | O(n) | Only works for canonical coin systems |

### 3.3 Activity Selection (Template)

```java
// Java
class Activity {
    int start, end;
}

int maxActivities(Activity[] activities) {
    // Sort by end time (CRITICAL for greedy)
    Arrays.sort(activities, (a, b) -> a.end - b.end);
    
    int count = 1;
    int lastEnd = activities[0].end;
    
    for (int i = 1; i < activities.length; i++) {
        if (activities[i].start >= lastEnd) {
            count++;
            lastEnd = activities[i].end;
        }
    }
    return count;
}
```

### 3.4 Greedy vs Dynamic Programming

| Feature | Greedy | Dynamic Programming |
|---------|--------|---------------------|
| **Choice** | Local optimal | Consider all choices |
| **Backtracking** | Never | Explores all paths |
| **Complexity** | Usually faster | Usually slower |
| **Examples** | Activity selection, MST | 0/1 Knapsack, LCS |
| **Correctness** | Needs proof | Always correct if formulated right |

---

## 4. DYNAMIC PROGRAMMING PATTERNS

### 4.1 DP Problem Identification

✅ **Use DP when you see**:
- Overlapping subproblems (same calculation repeated)
- Optimal substructure (optimal solution contains optimal subsolutions)
- **Keywords**: "maximum", "minimum", "count ways", "longest", "shortest"

❌ **Don't use DP when**:
- Greedy works (activity selection, MST)
- No overlapping subproblems (divide & conquer better)

### 4.2 Classic DP Problems

| Problem | Recurrence | Time | Space | Pattern |
|---------|------------|------|-------|---------|
| **Fibonacci** | `dp[i] = dp[i-1] + dp[i-2]` | O(n) | O(n) → O(1) | Linear DP |
| **0/1 Knapsack** | `dp[i][w] = max(dp[i-1][w], val[i] + dp[i-1][w-wt[i]])` | O(nW) | O(nW) → O(W) | 2D grid |
| **LCS** | `dp[i][j] = dp[i-1][j-1]+1` (if match) | O(mn) | O(mn) → O(min(m,n)) | 2D grid |
| **LIS** | `dp[i] = max(dp[j]+1)` where `arr[j]<arr[i]` | O(n²) or O(n log n) | O(n) | 1D array |
| **Edit Distance** | `min(insert, delete, replace)` | O(mn) | O(mn) | 2D grid |
| **Matrix Chain** | `dp[i][j] = min(dp[i][k] + dp[k+1][j] + cost)` | O(n³) | O(n²) | Interval DP |
| **Coin Change** | `dp[i] = min(dp[i-coin] + 1)` | O(n*amount) | O(amount) | Unbounded knapsack |
| **Subset Sum** | `dp[i][sum] = dp[i-1][sum] || dp[i-1][sum-arr[i]]` | O(n*sum) | O(sum) | 2D boolean |

### 4.3 DP Approaches

| Approach | Description | When to Use |
|----------|-------------|-------------|
| **Top-Down (Memoization)** | Recursion + cache | Easier to think, not all states computed |
| **Bottom-Up (Tabulation)** | Iterative, fill table | Better space optimization, no recursion overhead |

#### Fibonacci (Both Approaches)

**Top-Down** (Memoization):
```python
def fib(n, memo={}):
    if n <= 1:
        return n
    if n not in memo:
        memo[n] = fib(n-1, memo) + fib(n-2, memo)
    return memo[n]
```

**Bottom-Up** (Tabulation):
```python
def fib(n):
    if n <= 1:
        return n
    dp = [0] * (n + 1)
    dp[1] = 1
    for i in range(2, n + 1):
        dp[i] = dp[i-1] + dp[i-2]
    return dp[n]
```

**Space-Optimized** (O(1)):
```python
def fib(n):
    if n <= 1:
        return n
    prev, curr = 0, 1
    for _ in range(2, n + 1):
        prev, curr = curr, prev + curr
    return curr
```

### 4.4 0/1 Knapsack (Template)

```java
// Java - Bottom-Up DP
int knapsack(int[] weights, int[] values, int W) {
    int n = weights.length;
    int[][] dp = new int[n+1][W+1];
    
    for (int i = 1; i <= n; i++) {
        for (int w = 0; w <= W; w++) {
            if (weights[i-1] <= w) {
                dp[i][w] = Math.max(
                    dp[i-1][w],  // Don't take item i
                    values[i-1] + dp[i-1][w - weights[i-1]]  // Take item i
                );
            } else {
                dp[i][w] = dp[i-1][w];  // Can't take (too heavy)
            }
        }
    }
    return dp[n][W];
}
```

**Space-Optimized** (1D array):
```python
def knapsack(weights, values, W):
    dp = [0] * (W + 1)
    for i in range(len(weights)):
        for w in range(W, weights[i] - 1, -1):  # Reverse to avoid using updated values
            dp[w] = max(dp[w], values[i] + dp[w - weights[i]])
    return dp[W]
```

### 4.5 Longest Common Subsequence (LCS)

```cpp
// C++ - LCS
int lcs(string s1, string s2) {
    int m = s1.length(), n = s2.length();
    vector<vector<int>> dp(m+1, vector<int>(n+1, 0));
    
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (s1[i-1] == s2[j-1])
                dp[i][j] = dp[i-1][j-1] + 1;
            else
                dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
        }
    }
    return dp[m][n];
}
```

---

## 5. BACKTRACKING

### 5.1 Backtracking Template

```
void backtrack(state, choices) {
    if (is_solution(state)) {
        add_to_results(state);
        return;
    }
    
    for (choice in choices) {
        if (is_valid(choice)) {
            make_choice(choice);          // Modify state
            backtrack(state, new_choices); // Recurse
            undo_choice(choice);          // Backtrack (RESTORE state)
        }
    }
}
```

### 5.2 Classic Backtracking Problems

| Problem | Choices at Each Step | Solution Condition | Time Complexity |
|---------|---------------------|--------------------|--------------------|
| **N-Queens** | Column to place queen in current row | All N queens placed | O(N!) |
| **Sudoku** | Digit 1-9 for empty cell | All cells filled validly | O(9^(n*n)) worst |
| **Permutations** | Which unused element to add | All elements used | O(N * N!) |
| **Combinations** | Include or exclude element | Reached target size/sum | O(2^N) |
| **Subset Sum** | Include or exclude element | Sum equals target | O(2^N) |
| **Word Search** | Direction (up/down/left/right) | Entire word matched | O(N * 4^L) |
| **Rat in Maze** | Direction to move | Reached destination | O(4^(N*M)) |

### 5.3 N-Queens Solution (Template)

```python
def solveNQueens(n):
    def is_safe(row, col):
        # Check column
        for i in range(row):
            if board[i] == col:
                return False
        # Check diagonals
        for i in range(row):
            if abs(board[i] - col) == abs(i - row):
                return False
        return True
    
    def backtrack(row):
        if row == n:
            results.append(board[:])  # Found a solution
            return
        
        for col in range(n):
            if is_safe(row, col):
                board[row] = col       # Make choice
                backtrack(row + 1)      # Recurse
                # No explicit undo needed (overwritten in next iteration)
    
    results = []
    board = [-1] * n  # board[i] = column of queen in row i
    backtrack(0)
    return results
```

### 5.4 Backtracking vs Other Techniques

| Feature | Backtracking | Greedy | DP |
|---------|--------------|--------|-----|
| **Explores** | All possibilities | One path | All subproblems |
| **Prunes** | Invalid paths | Never looks back | No pruning |
| **Use Case** | Constraint satisfaction, enumeration | Optimization (when works) | Optimization (overlapping subproblems) |
| **Examples** | N-Queens, Sudoku | Activity selection | Knapsack, LCS |

---

## 6. DIVIDE AND CONQUER

### 6.1 Divide and Conquer Pattern

```
solve(problem):
    if problem is small enough:
        return base_case_solution(problem)
    
    # DIVIDE
    subproblems = divide(problem)
    
    # CONQUER
    subsolutions = [solve(subproblem) for subproblem in subproblems]
    
    # COMBINE
    return combine(subsolutions)
```

### 6.2 Classic D&C Algorithms

| Algorithm | Divide | Conquer | Combine | Time | Recurrence |
|-----------|--------|---------|---------|------|------------|
| **Merge Sort** | Split at middle | Sort halves | Merge sorted halves | O(n log n) | T(n) = 2T(n/2) + O(n) |
| **Quick Sort** | Partition around pivot | Sort partitions | None needed | O(n log n) avg | T(n) = 2T(n/2) + O(n) |
| **Binary Search** | Compare with middle | Search half | Return result | O(log n) | T(n) = T(n/2) + O(1) |
| **Strassen's Matrix** | Divide matrices | Multiply submatrices | Combine results | O(n^2.807) | T(n) = 7T(n/2) + O(n²) |
| **Closest Pair** | Divide by median x | Find in halves | Check boundary | O(n log n) | T(n) = 2T(n/2) + O(n) |
| **Karatsuba** | Split numbers | Multiply parts | Combine | O(n^1.58) | T(n) = 3T(n/2) + O(n) |

### 6.3 Merge Sort (Template)

```c
// C - Merge Sort
void merge(int arr[], int left, int mid, int right) {
    int n1 = mid - left + 1;
    int n2 = right - mid;
    int L[n1], R[n2];
    
    for (int i = 0; i < n1; i++) L[i] = arr[left + i];
    for (int j = 0; j < n2; j++) R[j] = arr[mid + 1 + j];
    
    int i = 0, j = 0, k = left;
    while (i < n1 && j < n2) {
        if (L[i] <= R[j])
            arr[k++] = L[i++];
        else
            arr[k++] = R[j++];
    }
    
    while (i < n1) arr[k++] = L[i++];
    while (j < n2) arr[k++] = R[j++];
}

void mergeSort(int arr[], int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        mergeSort(arr, left, mid);          // DIVIDE
        mergeSort(arr, mid + 1, right);     // DIVIDE
        merge(arr, left, mid, right);       // COMBINE
    }
}
```

### 6.4 Master Theorem (Recurrence Relations)

**Form**: T(n) = aT(n/b) + f(n)

| Case | Condition | Result |
|------|-----------|--------|
| **Case 1** | f(n) = O(n^(log_b(a) - ε)) for ε>0 | T(n) = Θ(n^(log_b(a))) |
| **Case 2** | f(n) = Θ(n^(log_b(a))) | T(n) = Θ(n^(log_b(a)) * log n) |
| **Case 3** | f(n) = Ω(n^(log_b(a) + ε)) and regularity | T(n) = Θ(f(n)) |

**Examples**:
- **Merge Sort**: T(n) = 2T(n/2) + O(n) → **Case 2** → T(n) = O(n log n)
- **Binary Search**: T(n) = T(n/2) + O(1) → **Case 2** → T(n) = O(log n)
- **Strassen's**: T(n) = 7T(n/2) + O(n²) → **Case 1** → T(n) = O(n^2.807)

---

## 7. PATTERN SEARCHING

### 7.1 String Matching Algorithms

| Algorithm | Preprocessing | Matching | Best For |
|-----------|--------------|----------|----------|
| **Naive** | O(1) | O(mn) | Short patterns, simple |
| **KMP** | O(m) | O(n) | Avoid backtracking in text |
| **Boyer-Moore** | O(m + σ) | O(n/m) best, O(mn) worst | Long patterns, large alphabet |
| **Rabin-Karp** | O(m) | O(n+m) avg, O(mn) worst | Multiple pattern search |
| **Aho-Corasick** | O(Σm) | O(n + z) | Multiple patterns (dictionary) |

**Where**:
- m = pattern length
- n = text length
- σ = alphabet size
- z = number of matches

### 7.2 KMP Algorithm (Knuth-Morris-Pratt)

**Key Idea**: Use failure function to avoid re-checking matched characters.

```python
def compute_lps(pattern):
    """Longest Prefix Suffix array"""
    m = len(pattern)
    lps = [0] * m
    length = 0
    i = 1
    
    while i < m:
        if pattern[i] == pattern[length]:
            length += 1
            lps[i] = length
            i += 1
        else:
            if length != 0:
                length = lps[length - 1]  # Don't restart from 0
            else:
                lps[i] = 0
                i += 1
    return lps

def kmp_search(text, pattern):
    n, m = len(text), len(pattern)
    lps = compute_lps(pattern)
    
    i = j = 0  # i for text, j for pattern
    matches = []
    
    while i < n:
        if text[i] == pattern[j]:
            i += 1
            j += 1
        
        if j == m:  # Full match
            matches.append(i - j)
            j = lps[j - 1]
        elif i < n and text[i] != pattern[j]:
            if j != 0:
                j = lps[j - 1]  # Use failure function
            else:
                i += 1
    
    return matches
```

### 7.3 Rabin-Karp (Rolling Hash)

```java
// Java - Rabin-Karp
final int PRIME = 101;

long hash(String s, int m) {
    long h = 0;
    for (int i = 0; i < m; i++)
        h = (h * 256 + s.charAt(i)) % PRIME;
    return h;
}

List<Integer> rabinKarp(String text, String pattern) {
    int n = text.length(), m = pattern.length();
    long patternHash = hash(pattern, m);
    long textHash = hash(text, m);
    long h = 1;  // 256^(m-1) % PRIME
    
    for (int i = 0; i < m - 1; i++)
        h = (h * 256) % PRIME;
    
    List<Integer> matches = new ArrayList<>();
    
    for (int i = 0; i <= n - m; i++) {
        if (patternHash == textHash) {
            // Hash match - verify character by character
            if (text.substring(i, i + m).equals(pattern))
                matches.add(i);
        }
        
        if (i < n - m) {
            // Roll hash: remove first char, add next char
            textHash = (256 * (textHash - text.charAt(i) * h) + text.charAt(i + m)) % PRIME;
            if (textHash < 0)
                textHash += PRIME;
        }
    }
    return matches;
}
```

### 7.4 Pattern Search Algorithm Selection

| Scenario | Best Algorithm |
|----------|---------------|
| Single pattern, multiple searches | **KMP** (preprocess once) |
| Long pattern, large alphabet | **Boyer-Moore** |
| Multiple different patterns | **Aho-Corasick** |
| Multiple occurrences of same pattern | **Rabin-Karp** |
| DNA sequences (small alphabet) | **KMP** or naive |
| Very short pattern | **Naive** (overhead not worth it) |

---

## 8. ALGORITHM SELECTION GUIDE

### 8.1 Problem-to-Algorithm Mapping

| Problem Feature | Algorithm to Consider |
|----------------|----------------------|
| **Sorted array** | Binary search, two pointers |
| **Unsorted array, find element** | Linear search, hash table |
| **Optimize with constraints** | Greedy (if works), DP |
| **All possible solutions** | Backtracking |
| **Overlapping subproblems** | Dynamic Programming |
| **Divide into independent subproblems** | Divide and Conquer |
| **Graph shortest path** | BFS (unweighted), Dijkstra (weighted), Bellman-Ford (negative weights) |
| **Graph minimum spanning tree** | Prim's, Kruskal's |
| **String matching** | KMP, Boyer-Moore, Rabin-Karp |
| **Sorting needed** | Quick sort (general), merge sort (stable), heap sort (space-constrained) |

### 8.2 Complexity Decision Tree

```
Need to sort?
├─ No
│  ├─ Search in sorted? → Binary Search O(log n)
│  ├─ Find all solutions? → Backtracking
│  ├─ Optimize? → Greedy or DP
│  └─ Search unsorted? → Linear O(n) or Hash O(1)
└─ Yes
   ├─ Need stable? → Merge Sort
   ├─ Space critical? → Heap Sort
   ├─ Small data? → Insertion Sort
   └─ General case? → Quick Sort
```

---

## 9. COMPLEXITY CHEAT SHEET

### 9.1 Sorting Complexities (Quick Reference)

| Algorithm | Average | Worst | Space |
|-----------|---------|-------|-------|
| Quick Sort | **O(n log n)** | O(n²) | O(log n) |
| Merge Sort | **O(n log n)** | **O(n log n)** | O(n) |
| Heap Sort | **O(n log n)** | **O(n log n)** | O(1) |
| Insertion | O(n²) | O(n²) | O(1) |
| Bubble | O(n²) | O(n²) | O(1) |
| Counting | **O(n+k)** | **O(n+k)** | O(k) |

### 9.2 Graph Algorithm Complexities

| Algorithm | Time | Space | Use Case |
|-----------|------|-------|----------|
| **BFS/DFS** | O(V+E) | O(V) | Traversal, connectivity |
| **Dijkstra** | O(E log V) with heap | O(V) | Shortest path, non-negative weights |
| **Bellman-Ford** | O(VE) | O(V) | Shortest path, negative weights |
| **Floyd-Warshall** | O(V³) | O(V²) | All-pairs shortest path |
| **Prim's MST** | O(E log V) | O(V) | Minimum spanning tree |
| **Kruskal's MST** | O(E log E) | O(V) | Minimum spanning tree |
| **Topological Sort** | O(V+E) | O(V) | DAG ordering |

---

## 10. COMMON PATTERNS & TRICKS

### 10.1 Two Pointers Pattern

**When to use**: Sorted array, find pair/triplet with condition

```python
# Two sum in sorted array
def two_sum_sorted(arr, target):
    left, right = 0, len(arr) - 1
    while left < right:
        current_sum = arr[left] + arr[right]
        if current_sum == target:
            return [left, right]
        elif current_sum < target:
            left += 1
        else:
            right -= 1
    return []
```

### 10.2 Sliding Window Pattern

**When to use**: Subarray/substring with constraint

```python
# Maximum sum subarray of size k
def max_sum_subarray(arr, k):
    window_sum = sum(arr[:k])
    max_sum = window_sum
    
    for i in range(k, len(arr)):
        window_sum = window_sum - arr[i-k] + arr[i]
        max_sum = max(max_sum, window_sum)
    
    return max_sum
```

### 10.3 Fast & Slow Pointers

**When to use**: Detect cycle, find middle

```python
# Detect cycle in linked list
def has_cycle(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow == fast:
            return True
    return False
```

### 10.4 Common Optimization Tricks

| Trick | Example |
|-------|---------|
| **Avoid overflow** | `mid = left + (right - left) / 2` instead of `(left + right) / 2` |
| **Space optimization in DP** | Use 1D array instead of 2D when only previous row needed |
| **Early termination** | Binary search returns immediately on match |
| **Pruning in backtracking** | Skip invalid choices early |
| **Memoization** | Cache results to avoid recalculation |

---

## 11. MULTI-LANGUAGE SYNTAX QUICK REFERENCE

### 11.1 Array/List Sorting

| Language | Syntax | Notes |
|----------|--------|-------|
| **C** | `qsort(arr, n, sizeof(int), compare_func)` | Need comparator function |
| **C++** | `sort(arr, arr+n)` or `sort(vec.begin(), vec.end())` | `<algorithm>` |
| **Java** | `Arrays.sort(arr)` or `Collections.sort(list)` | Auto-boxing for primitives |
| **Python** | `arr.sort()` or `sorted(arr)` | In-place vs new list |

### 11.2 Custom Comparators

**C++**:
```cpp
sort(arr, arr+n, [](int a, int b) { return a > b; });  // Descending
```

**Java**:
```java
Arrays.sort(arr, (a, b) -> b - a);  // Descending
```

**Python**:
```python
arr.sort(key=lambda x: -x)  # Descending
arr.sort(reverse=True)      # Descending (cleaner)
```

### 11.3 Recursion Stack Depth

| Language | Default Limit | How to Increase |
|----------|--------------|-----------------|
| **C/C++** | ~1-8 MB | Compiler flags |
| **Java** | -Xss (default ~1MB) | `-Xss2m` |
| **Python** | ~1000 | `sys.setrecursionlimit(10000)` |

---

## 12. INTERVIEW QUICK CHECKS

### Before Implementing Algorithm:

✅ **Checklist**:
- [ ] Understood problem constraints (n ≤ ?, time limit?)
- [ ] Considered edge cases (empty, single element, duplicates)
- [ ] Chosen appropriate algorithm (see selection guide)
- [ ] Analyzed time/space complexity
- [ ] Considered optimization (can we do better?)

### Common Interview Mistakes:

❌ **Avoid**:
- Not asking about input constraints
- Choosing inefficient algorithm (e.g., O(n²) when O(n log n) possible)
- Forgetting base cases in recursion
- Off-by-one errors in loops
- Not handling edge cases (null, empty, negative, overflow)

---

## 13. FINAL 5-MINUTE CHECKLIST

### Sorting:
- [ ] Quick sort: average O(n log n), worst O(n²), in-place
- [ ] Merge sort: O(n log n) guaranteed, stable, O(n) space
- [ ] Heap sort: O(n log n), in-place, not stable

### Searching:
- [ ] Binary search: O(log n), requires sorted array
- [ ] Lower/upper bound patterns for variants

### Greedy:
- [ ] Activity selection: sort by end time
- [ ] Only works when greedy choice property holds

### DP:
- [ ] 0/1 Knapsack: `dp[i][w] = max(exclude, include)`
- [ ] LCS: `dp[i][j] = dp[i-1][j-1]+1` if match, else `max(dp[i-1][j], dp[i][j-1])`
- [ ] Consider space optimization (2D → 1D)

### Backtracking:
- [ ] Make choice → Recurse → Undo choice
- [ ] Prune invalid branches early

### Pattern Matching:
- [ ] KMP: Build LPS array, O(n+m)
- [ ] Rabin-Karp: Rolling hash, good for multiple patterns

### Complexity:
- [ ] Master theorem for divide & conquer: T(n) = aT(n/b) + f(n)
- [ ] Always analyze both time and space

---

## EXAM SUCCESS TIPS

### Debugging Strategies:
1. **Trace with small example** (n=2 or n=3)
2. **Check boundary conditions** (0, 1, n-1, n)
3. **Verify loop invariants** (what's true before/after each iteration)
4. **Test base cases** in recursion

### Time Management:
- **Syntax questions** (1 min): Quick recall
- **Dry run questions** (2-3 min): Careful tracing
- **Algorithm selection** (1-2 min): Pattern recognition
- **Debugging** (2-3 min): Systematic checking

### Last-Minute Review:
Focus on:
1. Sorting algorithm complexities
2. Binary search variants (lower/upper bound)
3. DP recurrence relations (0/1 knapsack, LCS)
4. Greedy vs DP decision
5. Backtracking template

---

**GOOD LUCK WITH YOUR SEBI PHASE 2 EXAM!** 🚀

Total Review Time: **60-75 minutes**  
Topics Covered: **7 major algorithm categories**  
Languages: **C, C++, Java, Python**  
Remember: **Understand the pattern, not just memorize!**