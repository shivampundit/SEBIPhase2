# Divide and Conquer - Comprehensive Notes

## Overview
Divide and Conquer is an algorithm design paradigm that works by recursively breaking down a problem into subproblems until they become simple enough to solve directly.

### Three Steps
1. **Divide**: Break problem into smaller subproblems
2. **Conquer**: Solve subproblems recursively
3. **Combine**: Merge solutions of subproblems

### General Template
```python
def divide_and_conquer(problem):
    if is_base_case(problem):
        return solve_directly(problem)
    
    # Divide
    subproblems = divide(problem)
    
    # Conquer
    sub_solutions = [divide_and_conquer(sub) for sub in subproblems]
    
    # Combine
    return combine(sub_solutions)
```

---

## Classic Divide and Conquer Problems

## 1. Merge Sort (Already covered in Sorting)

### Recurrence Relation
T(n) = 2T(n/2) + O(n)

Using Master Theorem: T(n) = O(n log n)

---

## 2. Quick Sort (Already covered in Sorting)

### Recurrence Relation
- Best/Average: T(n) = 2T(n/2) + O(n) = O(n log n)
- Worst: T(n) = T(n-1) + O(n) = O(n²)

---

## 3. Binary Search (Already covered in Searching)

### Recurrence Relation
T(n) = T(n/2) + O(1) = O(log n)

---

## 4. Maximum Subarray Problem (Kadane's Algorithm)

### Problem
Find contiguous subarray with maximum sum.

### Divide and Conquer Approach
```python
def max_subarray_dc(arr, left, right):
    """
    Find maximum subarray sum using divide and conquer
    Time: O(n log n)
    Space: O(log n)
    """
    if left == right:
        return arr[left]
    
    mid = (left + right) // 2
    
    # Conquer
    left_max = max_subarray_dc(arr, left, mid)
    right_max = max_subarray_dc(arr, mid + 1, right)
    
    # Find max crossing subarray
    left_sum = float('-inf')
    current_sum = 0
    for i in range(mid, left - 1, -1):
        current_sum += arr[i]
        left_sum = max(left_sum, current_sum)
    
    right_sum = float('-inf')
    current_sum = 0
    for i in range(mid + 1, right + 1):
        current_sum += arr[i]
        right_sum = max(right_sum, current_sum)
    
    cross_sum = left_sum + right_sum
    
    # Combine
    return max(left_max, right_max, cross_sum)

# Example
arr = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
result = max_subarray_dc(arr, 0, len(arr) - 1)
print(f"Maximum subarray sum: {result}")  # 6: [4,-1,2,1]
```

### Kadane's Algorithm (Optimal - Not D&C)
```python
def max_subarray_kadane(arr):
    """
    Optimal solution using dynamic programming
    Time: O(n)
    Space: O(1)
    """
    max_sum = arr[0]
    current_sum = arr[0]
    
    for i in range(1, len(arr)):
        current_sum = max(arr[i], current_sum + arr[i])
        max_sum = max(max_sum, current_sum)
    
    return max_sum
```

### Dry Run (arr=[-2,1,-3,4,-1,2,1,-5,4])
```
Divide and Conquer Approach:

Level 1: [−2,1,−3,4,−1,2,1,−5,4]
         Split at mid=4
         
Level 2: [−2,1,−3,4,−1] | [2,1,−5,4]
         Split each
         
Level 3: [−2,1,−3] | [4,−1] | [2,1] | [−5,4]

Conquer phase:
Left half [−2,1,−3]: max = 1
Right half [4,−1]: max = 4
Left half [2,1]: max = 3
Right half [−5,4]: max = 4

Crossing subarray [−2,1,−3,4,−1]:
From mid left: max(−3, −3+1, −3+1−2) = −2
From mid right: max(4, 4−1) = 4
Cross sum = −2 + 4 = 2

Combine: max(1, 4, 2) = 4 for left partition
Combine: max(3, 4, ...) for right partition

Final crossing through middle:
Left part max sum ending at mid-1: 3 (from 4,−1,2,1)
Right part max sum starting at mid: 2,1
Total: 4−1+2+1 = 6

Maximum: 6
```

---

## 5. Closest Pair of Points

### Problem
Given n points in 2D plane, find pair with minimum distance.

### Implementation
```python
import math

def closest_pair(points):
    """
    Find closest pair of points
    Time: O(n log n)
    Space: O(n)
    """
    def distance(p1, p2):
        return math.sqrt((p1[0] - p2[0])**2 + (p1[1] - p2[1])**2)
    
    def brute_force(points):
        min_dist = float('inf')
        n = len(points)
        
        for i in range(n):
            for j in range(i + 1, n):
                min_dist = min(min_dist, distance(points[i], points[j]))
        
        return min_dist
    
    def closest_in_strip(strip, d):
        min_dist = d
        strip.sort(key=lambda p: p[1])  # Sort by y-coordinate
        
        for i in range(len(strip)):
            j = i + 1
            while j < len(strip) and (strip[j][1] - strip[i][1]) < min_dist:
                min_dist = min(min_dist, distance(strip[i], strip[j]))
                j += 1
        
        return min_dist
    
    def closest_util(px, py):
        n = len(px)
        
        # Base case: use brute force for small arrays
        if n <= 3:
            return brute_force(px)
        
        # Divide
        mid = n // 2
        midpoint = px[mid]
        
        pyl = [p for p in py if p[0] <= midpoint[0]]
        pyr = [p for p in py if p[0] > midpoint[0]]
        
        # Conquer
        dl = closest_util(px[:mid], pyl)
        dr = closest_util(px[mid:], pyr)
        
        d = min(dl, dr)
        
        # Find points in strip
        strip = [p for p in py if abs(p[0] - midpoint[0]) < d]
        
        # Combine
        return min(d, closest_in_strip(strip, d))
    
    # Sort by x and y coordinates
    px = sorted(points, key=lambda p: p[0])
    py = sorted(points, key=lambda p: p[1])
    
    return closest_util(px, py)

# Example
points = [(2, 3), (12, 30), (40, 50), (5, 1), (12, 10), (3, 4)]
min_distance = closest_pair(points)
print(f"Minimum distance: {min_distance:.2f}")
```

---

## 6. Strassen's Matrix Multiplication

### Problem
Multiply two n×n matrices faster than O(n³).

### Standard Matrix Multiplication: O(n³)
```python
def matrix_multiply_standard(A, B):
    """
    Standard matrix multiplication
    Time: O(n³)
    """
    n = len(A)
    C = [[0] * n for _ in range(n)]
    
    for i in range(n):
        for j in range(n):
            for k in range(n):
                C[i][j] += A[i][k] * B[k][j]
    
    return C
```

### Strassen's Algorithm: O(n^2.807)
```python
def strassen_multiply(A, B):
    """
    Strassen's matrix multiplication
    Time: O(n^2.807)
    """
    n = len(A)
    
    # Base case
    if n == 1:
        return [[A[0][0] * B[0][0]]]
    
    # Divide matrices into quadrants
    mid = n // 2
    
    A11 = [row[:mid] for row in A[:mid]]
    A12 = [row[mid:] for row in A[:mid]]
    A21 = [row[:mid] for row in A[mid:]]
    A22 = [row[mid:] for row in A[mid:]]
    
    B11 = [row[:mid] for row in B[:mid]]
    B12 = [row[mid:] for row in B[:mid]]
    B21 = [row[:mid] for row in B[mid:]]
    B22 = [row[mid:] for row in B[mid:]]
    
    # Helper functions
    def add_matrix(X, Y):
        return [[X[i][j] + Y[i][j] for j in range(len(X[0]))] 
                for i in range(len(X))]
    
    def subtract_matrix(X, Y):
        return [[X[i][j] - Y[i][j] for j in range(len(X[0]))] 
                for i in range(len(X))]
    
    # Compute 7 products (Strassen's formulas)
    M1 = strassen_multiply(add_matrix(A11, A22), add_matrix(B11, B22))
    M2 = strassen_multiply(add_matrix(A21, A22), B11)
    M3 = strassen_multiply(A11, subtract_matrix(B12, B22))
    M4 = strassen_multiply(A22, subtract_matrix(B21, B11))
    M5 = strassen_multiply(add_matrix(A11, A12), B22)
    M6 = strassen_multiply(subtract_matrix(A21, A11), add_matrix(B11, B12))
    M7 = strassen_multiply(subtract_matrix(A12, A22), add_matrix(B21, B22))
    
    # Combine
    C11 = add_matrix(subtract_matrix(add_matrix(M1, M4), M5), M7)
    C12 = add_matrix(M3, M5)
    C21 = add_matrix(M2, M4)
    C22 = add_matrix(subtract_matrix(add_matrix(M1, M3), M2), M6)
    
    # Construct result matrix
    C = []
    for i in range(mid):
        C.append(C11[i] + C12[i])
    for i in range(mid):
        C.append(C21[i] + C22[i])
    
    return C
```

---

## 7. Counting Inversions

### Problem
Count number of inversions in array (pairs where i < j but arr[i] > arr[j]).

### Implementation
```python
def count_inversions(arr):
    """
    Count inversions using modified merge sort
    Time: O(n log n)
    Space: O(n)
    """
    def merge_count(arr, temp, left, mid, right):
        i = left    # Left subarray index
        j = mid + 1 # Right subarray index
        k = left    # Merged array index
        inv_count = 0
        
        while i <= mid and j <= right:
            if arr[i] <= arr[j]:
                temp[k] = arr[i]
                i += 1
            else:
                temp[k] = arr[j]
                inv_count += (mid - i + 1)  # Key step
                j += 1
            k += 1
        
        while i <= mid:
            temp[k] = arr[i]
            i += 1
            k += 1
        
        while j <= right:
            temp[k] = arr[j]
            j += 1
            k += 1
        
        for i in range(left, right + 1):
            arr[i] = temp[i]
        
        return inv_count
    
    def merge_sort_count(arr, temp, left, right):
        inv_count = 0
        
        if left < right:
            mid = (left + right) // 2
            
            inv_count += merge_sort_count(arr, temp, left, mid)
            inv_count += merge_sort_count(arr, temp, mid + 1, right)
            inv_count += merge_count(arr, temp, left, mid, right)
        
        return inv_count
    
    n = len(arr)
    temp = [0] * n
    return merge_sort_count(arr, temp, 0, n - 1)

# Example
arr = [2, 4, 1, 3, 5]
inversions = count_inversions(arr)
print(f"Number of inversions: {inversions}")  # 3: (2,1), (4,1), (4,3)
```

### Dry Run (arr=[2,4,1,3,5])
```
Divide:
[2, 4, 1, 3, 5]
  ↓
[2, 4] [1, 3, 5]
  ↓        ↓
[2] [4]  [1] [3, 5]
              ↓
            [3] [5]

Conquer and Count:
[2] [4]: no inversions, merge → [2, 4]

[3] [5]: no inversions, merge → [3, 5]

[1] [3, 5]: 
  Compare 1 vs 3: 1 < 3, no inversion
  Merge → [1, 3, 5]

[2, 4] [1, 3, 5]:
  Compare 2 vs 1: 2 > 1, count=2 (2,1) and (4,1)
  Compare 2 vs 3: 2 < 3
  Compare 4 vs 3: 4 > 3, count+=1 (4,3)
  Compare 4 vs 5: 4 < 5
  Merge → [1, 2, 3, 4, 5]

Total inversions: 3
```

---

## 8. Karatsuba Multiplication

### Problem
Multiply two large numbers faster than O(n²).

### Implementation
```python
def karatsuba(x, y):
    """
    Karatsuba multiplication algorithm
    Time: O(n^1.585) vs O(n²) for standard
    """
    # Base case
    if x < 10 or y < 10:
        return x * y
    
    # Calculate size
    n = max(len(str(x)), len(str(y)))
    m = n // 2
    
    # Split numbers
    high1, low1 = divmod(x, 10**m)
    high2, low2 = divmod(y, 10**m)
    
    # Three recursive calls
    z0 = karatsuba(low1, low2)
    z1 = karatsuba(low1 + high1, low2 + high2)
    z2 = karatsuba(high1, high2)
    
    # Combine
    return z2 * 10**(2*m) + (z1 - z2 - z0) * 10**m + z0

# Example
x = 1234
y = 5678
result = karatsuba(x, y)
print(f"{x} × {y} = {result}")
print(f"Verification: {x * y}")
```

---

## 9. Power Function

### Problem
Calculate x^n efficiently.

### Implementation
```python
def power(x, n):
    """
    Calculate x^n using divide and conquer
    Time: O(log n)
    Space: O(log n)
    """
    # Base cases
    if n == 0:
        return 1
    if n == 1:
        return x
    if n < 0:
        return 1 / power(x, -n)
    
    # Divide
    half = power(x, n // 2)
    
    # Combine
    if n % 2 == 0:
        return half * half
    else:
        return half * half * x

# Iterative version
def power_iterative(x, n):
    """
    Iterative power calculation
    Time: O(log n)
    Space: O(1)
    """
    if n < 0:
        x = 1 / x
        n = -n
    
    result = 1
    current = x
    
    while n > 0:
        if n % 2 == 1:
            result *= current
        current *= current
        n //= 2
    
    return result

# Example
print(f"2^10 = {power(2, 10)}")  # 1024
print(f"3^5 = {power(3, 5)}")    # 243
print(f"2^-3 = {power(2, -3)}")  # 0.125
```

### Dry Run (x=2, n=10)
```
power(2, 10)
├─ half = power(2, 5)
│  ├─ half = power(2, 2)
│  │  ├─ half = power(2, 1)
│  │  │  └─ return 2
│  │  └─ return 2 * 2 = 4
│  └─ return 4 * 4 * 2 = 32
└─ return 32 * 32 = 1024

Only 4 recursive calls instead of 10!
```

---

## Master Theorem

### For recurrence: T(n) = aT(n/b) + f(n)

**Case 1**: If f(n) = O(n^c) where c < log_b(a)
- Then T(n) = Θ(n^(log_b(a)))

**Case 2**: If f(n) = Θ(n^c log^k(n)) where c = log_b(a)
- Then T(n) = Θ(n^c log^(k+1)(n))

**Case 3**: If f(n) = Ω(n^c) where c > log_b(a)
- Then T(n) = Θ(f(n))

### Examples
1. **Merge Sort**: T(n) = 2T(n/2) + n
   - a=2, b=2, f(n)=n
   - log₂(2) = 1, f(n) = Θ(n¹)
   - Case 2: T(n) = Θ(n log n)

2. **Binary Search**: T(n) = T(n/2) + 1
   - a=1, b=2, f(n)=1
   - log₂(1) = 0, f(n) = Θ(1)
   - Case 2: T(n) = Θ(log n)

3. **Strassen's**: T(n) = 7T(n/2) + n²
   - a=7, b=2, f(n)=n²
   - log₂(7) ≈ 2.807 > 2
   - Case 1: T(n) = Θ(n^2.807)

---

## Divide and Conquer vs Other Paradigms

| Feature | D&C | Dynamic Programming | Greedy |
|---------|-----|---------------------|--------|
| Subproblems | Independent | Overlapping | N/A |
| Approach | Top-down (recursive) | Can be both | Sequential |
| Complexity | Often O(n log n) | Often O(n²) | Often O(n) |
| Combine step | Required | Not always | No |
| Examples | Merge Sort, Binary Search | LCS, Knapsack | Activity Selection |

---

## Exam Tips

### 1. Recognizing D&C Problems
Look for:
- Problems that can be split into similar subproblems
- Independent subproblems
- Need to combine results
- Recursive nature

### 2. Template Structure
```python
def divide_conquer(input):
    # Base case
    if is_small(input):
        return solve_directly(input)
    
    # Divide
    parts = split(input)
    
    # Conquer (recurse)
    results = [divide_conquer(part) for part in parts]
    
    # Combine
    return merge(results)
```

### 3. Analyzing Complexity
- Write recurrence relation
- Apply Master Theorem
- Consider combine step cost

### 4. Common Patterns
- **Binary division**: T(n) = 2T(n/2) + f(n)
- **Ternary division**: T(n) = 3T(n/3) + f(n)
- **Unequal division**: T(n) = T(n/3) + T(2n/3) + f(n)

### Key Points
1. **Three steps**: Divide, Conquer, Combine
2. **Base case**: Essential for recursion termination
3. **Independent subproblems**: Unlike DP
4. **Master Theorem**: Quick complexity analysis
5. **Examples**: Merge Sort, Quick Sort, Binary Search
6. **Optimization**: Often achieves O(n log n)
