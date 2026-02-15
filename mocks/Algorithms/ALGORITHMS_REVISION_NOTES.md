# ALGORITHMS REVISION NOTES
**Quick Reference for GATE-Level SEBI Exam | 1-Day Study Guide**

---

## 1. SORTING ALGORITHMS

### Time Complexity Cheat Sheet
| Algorithm | Best | Average | Worst | Space | Stable |
|-----------|------|---------|-------|-------|--------|
| Bubble | O(n) | O(n²) | O(n²) | O(1) | ✓ |
| Selection | O(n²) | O(n²) | O(n²) | O(1) | ✗ |
| Insertion | O(n) | O(n²) | O(n²) | O(1) | ✓ |
| Merge | O(n log n) | O(n log n) | O(n log n) | O(n) | ✓ |
| Quick | O(n log n) | O(n log n) | O(n²) | O(log n) | ✗ |
| Heap | O(n log n) | O(n log n) | O(n log n) | O(1) | ✗ |
| Counting | O(n+k) | O(n+k) | O(n+k) | O(k) | ✓ |
| Radix | O(d(n+k)) | O(d(n+k)) | O(d(n+k)) | O(n+k) | ✓ |

### Key Points
- **Bubble**: Swaps adjacent elements, multiple passes
- **Selection**: Finds minimum, places at beginning
- **Insertion**: Builds sorted array one item at a time, best for nearly sorted
- **Merge**: Divide & Conquer, always O(n log n), needs extra space
- **Quick**: Pivot-based partitioning, worst case O(n²) on sorted array
- **Heap**: Convert to max-heap, extract max repeatedly
- **Counting**: Non-comparison, works when range k is small
- **Radix**: Sort by digits (LSD or MSD)
- **STL sort()**: Intro Sort (Quick+Heap+Insertion hybrid)
- **Tim Sort**: Python default (Merge+Insertion hybrid)

### Stability
- **Stable**: Bubble, Insertion, Merge, Counting, Radix, Tim
- **Unstable**: Selection, Quick, Heap

---

## 2. SEARCHING ALGORITHMS

### Comparison
| Algorithm | Condition | Time | Space |
|-----------|-----------|------|-------|
| Linear | Any array | O(n) | O(1) |
| Binary | Sorted | O(log n) | O(1) |
| Jump | Sorted | O(√n) | O(1) |
| Interpolation | Uniformly sorted | O(log log n) avg | O(1) |
| Exponential | Unbounded sorted | O(log i) | O(1) |
| Ternary | Sorted | O(log₃ n) | O(1) |

### Key Points
- **Binary Search**: `mid = low + (high-low)/2` avoids overflow
- **Jump Search**: Optimal jump = √n
- **Interpolation**: Position = `low + ((x - arr[low]) * (high-low)) / (arr[high] - arr[low])`
- **Exponential**: Find range [2^i-1, 2^i], then binary search
- **Ternary**: Two midpoints, divide into 3 parts (usually slower than binary)
- **Rotated Array**: Check which half is sorted, then decide
- **First/Last Occurrence**: Modified binary search continues after finding

### Important
- Binary search condition: `while(low <= high)` (= for single element)
- Java `Arrays.binarySearch()`: returns `-(insertion_point) - 1` if not found
- Comparison-based search lower bound: Ω(log n)

---

## 3. GREEDY ALGORITHMS

### When Greedy Works
1. **Greedy Choice Property**: Local optimum → Global optimum
2. **Optimal Substructure**: Problem has optimal solution to subproblems

### Classic Problems
| Problem | Strategy | Time |
|---------|----------|------|
| Activity Selection | Sort by end time, select non-overlapping | O(n log n) |
| Fractional Knapsack | Sort by value/weight ratio, take highest | O(n log n) |
| Huffman Coding | Build tree from min frequencies | O(n log n) |
| Job Sequencing | Sort by profit, schedule by deadline | O(n²) or O(n log n) |
| Coin Change (canonical) | Greedy from largest denomination | O(k) |
| Prim's MST | Min edge from visited set | O(E log V) |
| Kruskal's MST | Sort edges, Union-Find | O(E log E) |
| Dijkstra's Shortest Path | Min distance vertex | O((V+E) log V) |

### When Greedy Fails
- **0/1 Knapsack**: Need DP
- **Coin Change** (non-canonical systems): Need DP
- **Longest Path**: NP-hard
- **Graph Coloring**: Heuristic only

### MST Notes
- **Prim**: Grows one tree (Priority Queue)
- **Kruskal**: Merges forests (Union-Find)
- Both find MST, same weight

### Dijkstra Limitation
- **Fails** with negative weights (use Bellman-Ford instead)

---

## 4. DYNAMIC PROGRAMMING

### DP Identification
- **Overlapping Subproblems**: Same calculation repeated
- **Optimal Substructure**: Optimal solution contains optimal sub-solutions
- **Approach**: Recursion → Memoization → Tabulation

### Classic DP Problems

#### Linear DP
| Problem | Recurrence | Time | Space |
|---------|------------|------|-------|
| Fibonacci | F(n) = F(n-1) + F(n-2) | O(n) | O(1) optimized |
| Climbing Stairs | ways(n) = ways(n-1) + ways(n-2) | O(n) | O(1) |
| House Robber | rob(n) = max(rob(n-1), rob(n-2)+arr[n]) | O(n) | O(1) |

#### Knapsack Variants
- **0/1 Knapsack**: `dp[i][w] = max(dp[i-1][w], dp[i-1][w-wt[i]] + val[i])`
- **Unbounded**: Can reuse items
- **Subset Sum**: Check if sum possible
- **Partition**: Equal sum subsets

#### String DP
- **LCS**: `dp[i][j] = 1 + dp[i-1][j-1]` if match, else `max(dp[i-1][j], dp[i][j-1])`
- **LIS**: Longest Increasing Subsequence O(n log n) with binary search
- **Edit Distance**: Insert/Delete/Replace operations
- **Palindrome**: LPS, Palindromic Substrings

#### Grid DP
- **Min Path Sum**: `dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1])`
- **Unique Paths**: `dp[i][j] = dp[i-1][j] + dp[i][j-1]`

#### Interval DP
- **Matrix Chain**: `dp[i][j] = min(dp[i][k] + dp[k+1][j] + cost)` for all k
- **Burst Balloons**: Consider last balloon burst

#### DP on Trees
- **Tree DP**: Process bottom-up or top-down

### Catalan Numbers
- Formula: `C(n) = C(0)*C(n-1) + C(1)*C(n-2) + ... + C(n-1)*C(0)`
- Applications: BST count, Parentheses, Polygon triangulation
- Values: C(0)=1, C(1)=1, C(2)=2, C(3)=5, C(4)=14, C(5)=42

### Optimization Techniques
- **Space Optimization**: Usually O(n) → O(1) using rolling array
- **Print Solution**: Store choices/path during table fill

---

## 5. BACKTRACKING

### Template
```
solve(state):
    if (goal reached): process solution
    for each choice:
        if (valid choice):
            make choice
            solve(new state)
            undo choice (backtrack)
```

### Classic Problems
| Problem | Approach | Complexity |
|---------|----------|------------|
| N-Queens | Place queen, check row/col/diagonal | O(n!) |
| Sudoku | Try 1-9, check row/col/box | O(9^(n²)) |
| Permutations | Swap and recurse | O(n!) |
| Combinations | Include/exclude | O(2ⁿ) |
| Subset Sum | Include/exclude elements | O(2ⁿ) |
| Graph Coloring | Try each color | O(m^V) |
| Hamiltonian Cycle | DFS with visited track | O(n!) |
| Knight's Tour | Move L-shaped, backtrack | O(8^(n²)) |
| Rat in Maze | Try 4 directions | O(4^(n²)) |
| Word Break | Try all positions | O(2ⁿ) |

### Key Points
- **Pruning**: Stop exploring when solution impossible
- **vs Branch & Bound**: B&B adds bounding function for optimization
- **vs DP**: Backtracking explores all, DP stores subproblem solutions
- **Time**: Usually exponential

### N-Queens Solutions
- n=4: 2 solutions
- n=8: 92 solutions

---

## 6. DIVIDE AND CONQUER

### Master Theorem
**T(n) = aT(n/b) + f(n)**

1. **Case 1**: If f(n) = O(n^c) where c < log_b(a) → **T(n) = Θ(n^log_b(a))**
2. **Case 2**: If f(n) = Θ(n^log_b(a)) → **T(n) = Θ(n^log_b(a) × log n)**
3. **Case 3**: If f(n) = Ω(n^c) where c > log_b(a) → **T(n) = Θ(f(n))**

### Common Recurrences
- **T(n) = 2T(n/2) + O(n)** → O(n log n) [Merge Sort]
- **T(n) = T(n/2) + O(1)** → O(log n) [Binary Search]
- **T(n) = 2T(n/2) + O(1)** → O(n) [Tree Traversal]
- **T(n) = 4T(n/2) + O(n)** → O(n²) [Strassen loses here]

### Classic Algorithms
| Algorithm | Problem | Time |
|-----------|---------|------|
| Merge Sort | Sorting | O(n log n) |
| Quick Sort | Sorting | O(n log n) avg |
| Binary Search | Search | O(log n) |
| Strassen | Matrix Mult | O(n^2.807) |
| Karatsuba | Integer Mult | O(n^1.59) |
| Closest Pair | 2D Points | O(n log n) |
| FFT | Polynomial Mult | O(n log n) |
| Quick Select | kth Element | O(n) avg |
| Count Inversions | Inversions | O(n log n) |

### Key Points
- **Max Subarray**: Can use D&C or Kadane's (DP) O(n)
- **Peak Element**: Binary search approach O(log n)
- **Median of 2 Sorted**: Binary search O(log n)
- **Tiling Defective Board**: Use L-shaped tiles recursively

---

## 7. PATTERN MATCHING

### Algorithm Comparison
| Algorithm | Preprocessing | Matching | Best For |
|-----------|---------------|----------|----------|
| Naive | O(1) | O(nm) | None |
| KMP | O(m) | O(n) | Repeated patterns |
| Boyer-Moore | O(m+σ) | O(n/m) best | Large alphabets |
| Rabin-Karp | O(m) | O(n+m) avg | Multiple patterns |
| Finite Automata | O(mσ) | O(n) | Fixed pattern |
| Aho-Corasick | O(Σm) | O(n+z) | Multiple patterns |
| Z-Algorithm | O(n+m) | O(n+m) | Linear time |

### KMP Algorithm
- **LPS Array**: Longest Proper Prefix which is Suffix
- **Build LPS**: O(m)
- **Pattern Match**: O(n)
- **Total**: O(n+m)

### Rabin-Karp
- **Rolling Hash**: `hash = (d * (hash - text[i]*h) + text[i+m]) % q`
- **Advantage**: O(1) hash update
- **Issue**: Hash collisions (verify match)

### Boyer-Moore
- **Bad Character Rule**: Skip based on mismatch
- **Good Suffix Rule**: Additional skip
- **Best Case**: O(n/m)

### Z-Algorithm
- **Z[i]**: Length of longest substring starting from i matching prefix
- **Applications**: Pattern matching, palindrome

### Suffix Structures
- **Suffix Array**: Sorted suffixes, O(n log n) build
- **Suffix Tree**: O(n) build (Ukkonen), O(m) search
- **Applications**: Longest repeated substring, Pattern matching

### Important
- **Manacher's**: Longest palindromic substring O(n)
- **Trie**: Prefix matching, O(m) insert/search
- **Aho-Corasick**: Multiple pattern matching using Trie + Failure links

---

## QUICK FORMULAS & FACTS

### Logarithms
- log₂(n) = number of times to divide n by 2 to reach 1
- log(n!) ≈ n log n

### Summations
- 1+2+...+n = n(n+1)/2 = O(n²)
- 1+2+4+...+2^k = 2^(k+1) - 1 = O(2^k)

### Combinatorics
- C(n,k) = n! / (k!(n-k)!)
- Grid paths (m×n): C(m+n, m)
- Catalan: C(n) = (2n)! / ((n+1)! × n!)

### Tree Height
- Complete binary tree with n nodes: height = log₂(n)
- Heap height: ⌊log₂(n)⌋

### Common Mistakes
- **Integer overflow**: Use `mid = low + (high-low)/2`
- **Off-by-one**: Check boundary conditions
- **Modulo negatives**: `((a % m) + m) % m`
- **Floating point**: Use epsilon for comparison

---

## EXAM TIPS
1. **Identify problem type**: DP overlapping subproblems, Greedy local optimum
2. **Complexity first**: Rule out brute force
3. **Edge cases**: Empty, single element, duplicates
4. **Output questions**: Dry run carefully
5. **Syntax**: Know language-specific details (Java binarySearch returns, Python sort() returns None)
6. **Time management**: 200Q in 180 min = ~50 sec/question

---

**GOOD LUCK! REVISE KEY FORMULAS AND COMPLEXITY TABLES BEFORE EXAM.**
