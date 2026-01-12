# Searching Algorithms - Comprehensive Notes

## Overview
Searching is the process of finding a specific element in a data structure. Efficient searching is fundamental to:
- Database queries
- Information retrieval
- Data analysis
- Algorithm optimization

---

## 1. Linear Search

### Concept
Linear Search sequentially checks each element until the target is found or the list ends. It's the simplest searching algorithm.

### Algorithm
```
linear_search(arr, target):
    for i = 0 to n-1:
        if arr[i] == target:
            return i
    return -1
```

### Implementation (Python)
```python
def linear_search(arr, target):
    """
    Search for target in array sequentially
    Returns: index if found, -1 otherwise
    """
    for i in range(len(arr)):
        if arr[i] == target:
            return i
    return -1

# Example
arr = [10, 23, 45, 70, 11, 15]
target = 70
result = linear_search(arr, target)
print(f"Element found at index: {result}")  # Output: 3

# Not found case
target = 100
result = linear_search(arr, target)
print(f"Element found at index: {result}")  # Output: -1
```

### Variations

#### 1. Search All Occurrences
```python
def linear_search_all(arr, target):
    """Returns all indices where target appears"""
    indices = []
    for i in range(len(arr)):
        if arr[i] == target:
            indices.append(i)
    return indices if indices else -1

# Example
arr = [10, 23, 45, 23, 11, 23]
print(linear_search_all(arr, 23))  # Output: [1, 3, 5]
```

#### 2. Sentinel Linear Search
```python
def sentinel_search(arr, target):
    """Optimized linear search using sentinel"""
    n = len(arr)
    last = arr[n-1]
    arr[n-1] = target
    
    i = 0
    while arr[i] != target:
        i += 1
    
    arr[n-1] = last  # Restore last element
    
    if i < n-1 or arr[n-1] == target:
        return i
    return -1
```

### Dry Run Example
```
Array: [5, 3, 7, 9, 2, 8]
Target: 9

Step 1: Check arr[0] = 5 ≠ 9
Step 2: Check arr[1] = 3 ≠ 9
Step 3: Check arr[2] = 7 ≠ 9
Step 4: Check arr[3] = 9 = 9 ✓
Return: 3

Total comparisons: 4
```

### Complexity Analysis
- **Time Complexity**:
  - Best Case: O(1) - element at first position
  - Average Case: O(n)
  - Worst Case: O(n) - element at last position or not present
- **Space Complexity**: O(1)

### When to Use
- Unsorted data
- Small datasets
- Single search operation
- Simple implementation needed

---

## 2. Binary Search

### Concept
Binary Search works on **sorted arrays**. It repeatedly divides the search interval in half by comparing the target with the middle element.

### Algorithm
```
binary_search(arr, target):
    left = 0, right = n-1
    while left <= right:
        mid = left + (right - left) / 2
        if arr[mid] == target:
            return mid
        else if arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

### Implementation (Python)

#### Iterative Approach
```python
def binary_search_iterative(arr, target):
    """
    Binary search using iteration
    Precondition: arr must be sorted
    """
    left, right = 0, len(arr) - 1
    
    while left <= right:
        mid = left + (right - left) // 2  # Avoid overflow
        
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return -1

# Example
arr = [2, 3, 4, 10, 40, 50, 60, 70]
target = 10
result = binary_search_iterative(arr, target)
print(f"Element found at index: {result}")  # Output: 3
```

#### Recursive Approach
```python
def binary_search_recursive(arr, target, left, right):
    """
    Binary search using recursion
    """
    if left > right:
        return -1
    
    mid = left + (right - left) // 2
    
    if arr[mid] == target:
        return mid
    elif arr[mid] < target:
        return binary_search_recursive(arr, target, mid + 1, right)
    else:
        return binary_search_recursive(arr, target, left, mid - 1)

# Example
arr = [2, 3, 4, 10, 40, 50, 60, 70]
target = 10
result = binary_search_recursive(arr, target, 0, len(arr) - 1)
print(f"Element found at index: {result}")  # Output: 3
```

### Dry Run Example
```
Array: [2, 5, 8, 12, 16, 23, 38, 45, 56, 67, 78]
Target: 23

Step 1:
left=0, right=10, mid=5
arr[5] = 23 = 23 ✓
Return: 5

Example 2 - Target: 56
Step 1: left=0, right=10, mid=5
        arr[5]=23 < 56, search right half
        
Step 2: left=6, right=10, mid=8
        arr[8]=56 = 56 ✓
Return: 8

Example 3 - Target: 100 (not present)
Step 1: left=0, right=10, mid=5
        arr[5]=23 < 100, search right
        
Step 2: left=6, right=10, mid=8
        arr[8]=56 < 100, search right
        
Step 3: left=9, right=10, mid=9
        arr[9]=67 < 100, search right
        
Step 4: left=10, right=10, mid=10
        arr[10]=78 < 100, search right
        
Step 5: left=11, right=10
        left > right, element not found
Return: -1
```

### Complexity Analysis
- **Time Complexity**:
  - Best Case: O(1) - element at middle
  - Average Case: O(log n)
  - Worst Case: O(log n)
- **Space Complexity**: 
  - Iterative: O(1)
  - Recursive: O(log n) - recursion stack

### Variations

#### 1. First Occurrence
```python
def binary_search_first(arr, target):
    """Find first occurrence of target"""
    left, right = 0, len(arr) - 1
    result = -1
    
    while left <= right:
        mid = left + (right - left) // 2
        
        if arr[mid] == target:
            result = mid
            right = mid - 1  # Continue searching left
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return result

# Example
arr = [1, 2, 2, 2, 3, 4, 5]
print(binary_search_first(arr, 2))  # Output: 1
```

#### 2. Last Occurrence
```python
def binary_search_last(arr, target):
    """Find last occurrence of target"""
    left, right = 0, len(arr) - 1
    result = -1
    
    while left <= right:
        mid = left + (right - left) // 2
        
        if arr[mid] == target:
            result = mid
            left = mid + 1  # Continue searching right
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return result

# Example
arr = [1, 2, 2, 2, 3, 4, 5]
print(binary_search_last(arr, 2))  # Output: 3
```

#### 3. Count Occurrences
```python
def count_occurrences(arr, target):
    """Count occurrences using binary search"""
    first = binary_search_first(arr, target)
    if first == -1:
        return 0
    
    last = binary_search_last(arr, target)
    return last - first + 1

# Example
arr = [1, 2, 2, 2, 2, 3, 4, 5]
print(count_occurrences(arr, 2))  # Output: 4
```

### When to Use
- Sorted data
- Large datasets
- Multiple search operations
- When O(log n) performance is needed

---

## 3. Jump Search

### Concept
Jump Search works on sorted arrays by jumping ahead by fixed steps and performing linear search in the identified block.

### Algorithm
```
jump_search(arr, target):
    n = length(arr)
    step = sqrt(n)
    prev = 0
    
    # Find block
    while arr[min(step, n)-1] < target:
        prev = step
        step += sqrt(n)
        if prev >= n:
            return -1
    
    # Linear search in block
    while arr[prev] < target:
        prev++
        if prev == min(step, n):
            return -1
    
    if arr[prev] == target:
        return prev
    return -1
```

### Implementation (Python)
```python
import math

def jump_search(arr, target):
    """
    Jump search on sorted array
    Optimal jump size is √n
    """
    n = len(arr)
    step = int(math.sqrt(n))
    prev = 0
    
    # Find the block where element may be present
    while arr[min(step, n) - 1] < target:
        prev = step
        step += int(math.sqrt(n))
        if prev >= n:
            return -1
    
    # Linear search in the identified block
    while arr[prev] < target:
        prev += 1
        if prev == min(step, n):
            return -1
    
    # If element is found
    if arr[prev] == target:
        return prev
    
    return -1

# Example
arr = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
target = 6
result = jump_search(arr, target)
print(f"Element found at index: {result}")  # Output: 6
```

### Dry Run Example
```
Array: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
Target: 8
n = 11, step = √11 ≈ 3

Jump Phase:
Step 1: Check arr[2]=2 < 8, jump to 3
Step 2: Check arr[5]=5 < 8, jump to 6
Step 3: Check arr[8]=8 ≥ 8, stop jumping

Linear Search Phase:
prev=6: arr[6]=6 < 8
prev=7: arr[7]=7 < 8
prev=8: arr[8]=8 = 8 ✓

Return: 8
```

### Complexity Analysis
- **Time Complexity**: O(√n)
- **Space Complexity**: O(1)
- **Optimal Jump Size**: √n

### When to Use
- Sorted array
- Better than linear, simpler than binary
- When jumping back is costly

---

## 4. Interpolation Search

### Concept
Interpolation Search is an improved variant of binary search for **uniformly distributed** sorted data. It uses the value to estimate position.

### Algorithm
```
interpolation_search(arr, target):
    low = 0, high = n-1
    
    while low <= high and target >= arr[low] and target <= arr[high]:
        if low == high:
            if arr[low] == target:
                return low
            return -1
        
        # Estimate position
        pos = low + ((target - arr[low]) * (high - low)) / (arr[high] - arr[low])
        
        if arr[pos] == target:
            return pos
        if arr[pos] < target:
            low = pos + 1
        else:
            high = pos - 1
    
    return -1
```

### Implementation (Python)
```python
def interpolation_search(arr, target):
    """
    Interpolation search on uniformly distributed sorted array
    Estimates position based on value
    """
    low, high = 0, len(arr) - 1
    
    while low <= high and arr[low] <= target <= arr[high]:
        if low == high:
            if arr[low] == target:
                return low
            return -1
        
        # Estimate position using interpolation formula
        pos = low + int(((target - arr[low]) * (high - low)) / 
                       (arr[high] - arr[low]))
        
        if arr[pos] == target:
            return pos
        elif arr[pos] < target:
            low = pos + 1
        else:
            high = pos - 1
    
    return -1

# Example
arr = [10, 20, 30, 40, 50, 60, 70, 80, 90, 100]
target = 70
result = interpolation_search(arr, target)
print(f"Element found at index: {result}")  # Output: 6
```

### Dry Run Example
```
Array: [10, 20, 30, 40, 50, 60, 70, 80, 90, 100]
Target: 70

Step 1:
low=0, high=9
arr[low]=10, arr[high]=100
pos = 0 + ((70-10) * (9-0)) / (100-10)
    = 0 + (60 * 9) / 90
    = 0 + 6 = 6
arr[6] = 70 = 70 ✓

Return: 6

Comparisons: Only 1!
```

### Complexity Analysis
- **Time Complexity**:
  - Best Case: O(1)
  - Average Case: O(log log n) - for uniform distribution
  - Worst Case: O(n) - for non-uniform distribution
- **Space Complexity**: O(1)

### When to Use
- Sorted array with uniform distribution
- Large datasets
- Values are relatively evenly distributed

---

## 5. Exponential Search

### Concept
Exponential Search finds the range where the element may be present by repeated doubling, then uses binary search.

### Algorithm
```
exponential_search(arr, target):
    if arr[0] == target:
        return 0
    
    # Find range for binary search
    i = 1
    while i < n and arr[i] <= target:
        i = i * 2
    
    # Binary search in found range
    return binary_search(arr, target, i/2, min(i, n-1))
```

### Implementation (Python)
```python
def exponential_search(arr, target):
    """
    Exponential search: find range then binary search
    Useful for unbounded/infinite arrays
    """
    n = len(arr)
    
    # If element is at first position
    if arr[0] == target:
        return 0
    
    # Find range for binary search by repeated doubling
    i = 1
    while i < n and arr[i] <= target:
        i *= 2
    
    # Binary search in the found range
    return binary_search_range(arr, target, i // 2, min(i, n - 1))

def binary_search_range(arr, target, left, right):
    """Binary search in specified range"""
    while left <= right:
        mid = left + (right - left) // 2
        
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return -1

# Example
arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 20, 30, 40, 50]
target = 30
result = exponential_search(arr, target)
print(f"Element found at index: {result}")  # Output: 11
```

### Dry Run Example
```
Array: [1, 2, 4, 8, 16, 32, 64, 128, 256]
Target: 64

Step 1: arr[0]=1 ≠ 64

Doubling Phase:
i=1: arr[1]=2 ≤ 64, continue
i=2: arr[2]=4 ≤ 64, continue
i=4: arr[4]=16 ≤ 64, continue
i=8: arr[8]=256 > 64, stop

Binary Search in range [4, 8]:
Range: [16, 32, 64, 128, 256]
Binary search finds 64 at index 6

Return: 6
```

### Complexity Analysis
- **Time Complexity**: O(log n)
- **Space Complexity**: O(1)

### When to Use
- Unbounded or infinite arrays
- When target is closer to beginning
- Better than binary search for small positions

---

## 6. Ternary Search

### Concept
Ternary Search divides the array into three parts instead of two. Mainly used for **unimodal functions** (finding maximum/minimum).

### Algorithm
```
ternary_search(arr, target, left, right):
    if left <= right:
        mid1 = left + (right - left) / 3
        mid2 = right - (right - left) / 3
        
        if arr[mid1] == target:
            return mid1
        if arr[mid2] == target:
            return mid2
        
        if target < arr[mid1]:
            return ternary_search(arr, target, left, mid1-1)
        else if target > arr[mid2]:
            return ternary_search(arr, target, mid2+1, right)
        else:
            return ternary_search(arr, target, mid1+1, mid2-1)
    
    return -1
```

### Implementation (Python)
```python
def ternary_search(arr, target, left, right):
    """
    Ternary search divides array into 3 parts
    """
    if left <= right:
        # Divide array into 3 parts
        mid1 = left + (right - left) // 3
        mid2 = right - (right - left) // 3
        
        if arr[mid1] == target:
            return mid1
        if arr[mid2] == target:
            return mid2
        
        # Determine which third to search
        if target < arr[mid1]:
            return ternary_search(arr, target, left, mid1 - 1)
        elif target > arr[mid2]:
            return ternary_search(arr, target, mid2 + 1, right)
        else:
            return ternary_search(arr, target, mid1 + 1, mid2 - 1)
    
    return -1

# Example
arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
target = 7
result = ternary_search(arr, target, 0, len(arr) - 1)
print(f"Element found at index: {result}")  # Output: 6
```

### Finding Maximum in Unimodal Array
```python
def ternary_search_max(arr, left, right):
    """
    Find maximum in unimodal array (increases then decreases)
    """
    while right - left > 2:
        mid1 = left + (right - left) // 3
        mid2 = right - (right - left) // 3
        
        if arr[mid1] < arr[mid2]:
            left = mid1
        else:
            right = mid2
    
    # Check remaining elements
    max_val = arr[left]
    max_idx = left
    for i in range(left + 1, right + 1):
        if arr[i] > max_val:
            max_val = arr[i]
            max_idx = i
    
    return max_idx

# Example
arr = [1, 3, 5, 7, 9, 8, 6, 4, 2]
result = ternary_search_max(arr, 0, len(arr) - 1)
print(f"Maximum at index: {result}, value: {arr[result]}")  # Output: 4, 9
```

### Complexity Analysis
- **Time Complexity**: O(log₃ n) ≈ O(log n)
- **Space Complexity**: O(log n) - recursion
- **Note**: Binary search is more efficient than ternary search for finding elements

---

## Searching Algorithm Comparison

| Algorithm         | Time (Best) | Time (Avg) | Time (Worst) | Space | Prerequisite |
|------------------|-------------|------------|--------------|-------|--------------|
| Linear Search    | O(1)        | O(n)       | O(n)         | O(1)  | None         |
| Binary Search    | O(1)        | O(log n)   | O(log n)     | O(1)  | Sorted       |
| Jump Search      | O(1)        | O(√n)      | O(√n)        | O(1)  | Sorted       |
| Interpolation    | O(1)        | O(log log n)| O(n)        | O(1)  | Sorted+Uniform|
| Exponential      | O(1)        | O(log n)   | O(log n)     | O(1)  | Sorted       |
| Ternary Search   | O(1)        | O(log₃ n)  | O(log₃ n)    | O(log n)| Sorted     |

---

## Exam Tips for MCQs

### 1. Logic Flow Completion
**Question**: Complete the binary search code:
```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = left + (right - left) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            # Missing line
```
**Answer**: `left = mid + 1`

### 2. Debugging Questions
**Question**: Find the bug:
```python
def binary_search(arr, target):
    left, right = 0, len(arr)  # Bug here
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
```
**Answer**: Should be `right = len(arr) - 1`

### 3. Dry Run Outputs
**Question**: How many comparisons in binary search?
```
Array: [2, 4, 6, 8, 10, 12, 14, 16]
Target: 10
```
**Answer**: 3 comparisons (check positions 3→5→4)

### 4. Complexity Questions
- Binary search on array of 1024 elements: max comparisons?
  - **Answer**: log₂(1024) = 10
- Linear search average comparisons on array of 100?
  - **Answer**: 50 (n/2)

### 5. Algorithm Selection
- **Unsorted small array**: Linear Search
- **Sorted large array**: Binary Search
- **Uniformly distributed**: Interpolation Search
- **Unbounded array**: Exponential Search
- **Finding max in unimodal**: Ternary Search

### Key Points to Remember
1. **Binary search requires sorted array**
2. **Middle calculation**: Use `left + (right - left) // 2` to avoid overflow
3. **Loop condition**: `while left <= right` (equal is important)
4. **Interpolation formula**: `pos = low + ((x-arr[low]) * (high-low) / (arr[high]-arr[low]))`
5. **Jump search step size**: √n is optimal
6. **Time complexity hierarchy**: O(1) < O(log log n) < O(log n) < O(√n) < O(n)
