# Heap Data Structure - Comprehensive Notes

## Overview
A Heap is a complete binary tree that satisfies the heap property. Used to implement priority queues efficiently.

### Types of Heaps

**1. Max Heap**: Parent ≥ Children
```
       90
      /  \
    80    70
   / \    /
  60 50  40
```

**2. Min Heap**: Parent ≤ Children
```
       10
      /  \
    20    30
   / \    /
  40 50  60
```

### Key Properties
1. **Complete Binary Tree**: All levels filled except possibly last, filled left to right
2. **Heap Property**: Parent-child relationship maintained
3. **Array Representation**: No pointers needed

---

## Array Representation

```
Index:  0   1   2   3   4   5   6
Value: 90  80  70  60  50  40  30

Tree representation:
       90
      /  \
    80    70
   / \    / \
  60 50  40 30

For node at index i:
- Parent: (i-1) // 2
- Left Child: 2*i + 1
- Right Child: 2*i + 2
```

---

## Min Heap Implementation

```python
class MinHeap:
    def __init__(self):
        self.heap = []
    
    def parent(self, i):
        return (i - 1) // 2
    
    def left_child(self, i):
        return 2 * i + 1
    
    def right_child(self, i):
        return 2 * i + 2
    
    def swap(self, i, j):
        self.heap[i], self.heap[j] = self.heap[j], self.heap[i]
    
    def insert(self, key):
        """
        Insert element into heap
        Time: O(log n)
        """
        # Add at end
        self.heap.append(key)
        
        # Heapify up
        index = len(self.heap) - 1
        while index > 0:
            parent_idx = self.parent(index)
            
            if self.heap[index] < self.heap[parent_idx]:
                self.swap(index, parent_idx)
                index = parent_idx
            else:
                break
    
    def extract_min(self):
        """
        Remove and return minimum element
        Time: O(log n)
        """
        if not self.heap:
            return None
        
        if len(self.heap) == 1:
            return self.heap.pop()
        
        # Store min
        min_val = self.heap[0]
        
        # Move last to root
        self.heap[0] = self.heap.pop()
        
        # Heapify down
        self.heapify_down(0)
        
        return min_val
    
    def heapify_down(self, index):
        """
        Restore heap property downward
        Time: O(log n)
        """
        smallest = index
        left = self.left_child(index)
        right = self.right_child(index)
        
        if left < len(self.heap) and self.heap[left] < self.heap[smallest]:
            smallest = left
        
        if right < len(self.heap) and self.heap[right] < self.heap[smallest]:
            smallest = right
        
        if smallest != index:
            self.swap(index, smallest)
            self.heapify_down(smallest)
    
    def get_min(self):
        """
        Get minimum without removing
        Time: O(1)
        """
        return self.heap[0] if self.heap else None
    
    def size(self):
        return len(self.heap)
    
    def is_empty(self):
        return len(self.heap) == 0
```

---

## Max Heap Implementation

```python
class MaxHeap:
    def __init__(self):
        self.heap = []
    
    def parent(self, i):
        return (i - 1) // 2
    
    def left_child(self, i):
        return 2 * i + 1
    
    def right_child(self, i):
        return 2 * i + 2
    
    def swap(self, i, j):
        self.heap[i], self.heap[j] = self.heap[j], self.heap[i]
    
    def insert(self, key):
        """
        Insert element into max heap
        Time: O(log n)
        """
        self.heap.append(key)
        index = len(self.heap) - 1
        
        while index > 0:
            parent_idx = self.parent(index)
            
            if self.heap[index] > self.heap[parent_idx]:
                self.swap(index, parent_idx)
                index = parent_idx
            else:
                break
    
    def extract_max(self):
        """
        Remove and return maximum element
        Time: O(log n)
        """
        if not self.heap:
            return None
        
        if len(self.heap) == 1:
            return self.heap.pop()
        
        max_val = self.heap[0]
        self.heap[0] = self.heap.pop()
        self.heapify_down(0)
        
        return max_val
    
    def heapify_down(self, index):
        """
        Restore heap property downward
        """
        largest = index
        left = self.left_child(index)
        right = self.right_child(index)
        
        if left < len(self.heap) and self.heap[left] > self.heap[largest]:
            largest = left
        
        if right < len(self.heap) and self.heap[right] > self.heap[largest]:
            largest = right
        
        if largest != index:
            self.swap(index, largest)
            self.heapify_down(largest)
    
    def get_max(self):
        """Get maximum without removing"""
        return self.heap[0] if self.heap else None
```

---

## Heapify Operation

### Build Heap from Array

```python
def build_min_heap(arr):
    """
    Build min heap from unsorted array
    Time: O(n) - more efficient than n insertions
    Space: O(1)
    """
    n = len(arr)
    
    # Start from last non-leaf node
    # Last non-leaf = parent of last leaf = (n-1-1)//2
    for i in range(n // 2 - 1, -1, -1):
        heapify_down(arr, n, i)
    
    return arr

def heapify_down(arr, n, i):
    """
    Heapify subtree rooted at index i
    """
    smallest = i
    left = 2 * i + 1
    right = 2 * i + 2
    
    if left < n and arr[left] < arr[smallest]:
        smallest = left
    
    if right < n and arr[right] < arr[smallest]:
        smallest = right
    
    if smallest != i:
        arr[i], arr[smallest] = arr[smallest], arr[i]
        heapify_down(arr, n, smallest)
```

#### Dry Run: Build Min Heap
```
Array: [4, 10, 3, 5, 1]

Initial tree:
       4
      / \
     10  3
    / \
   5   1

Step 1: Start from last non-leaf (index 1)
        i = 1, node = 10
        Children: 5, 1
        Smallest: 1
        Swap 10 and 1
        
       4
      / \
     1   3
    / \
   5  10

Step 2: i = 0, node = 4
        Children: 1, 3
        Smallest: 1
        Swap 4 and 1
        
       1
      / \
     4   3
    / \
   5  10
   
Step 3: Heapify down from index 1
        i = 1, node = 4
        Children: 5, 10
        No swap needed (4 < 5)

Final Min Heap:
       1
      / \
     4   3
    / \
   5  10

Array: [1, 4, 3, 5, 10]
```

---

## Heap Sort

```python
def heap_sort(arr):
    """
    Sort array using heap
    Time: O(n log n)
    Space: O(1)
    """
    n = len(arr)
    
    # Build max heap
    for i in range(n // 2 - 1, -1, -1):
        heapify_down_max(arr, n, i)
    
    # Extract elements one by one
    for i in range(n - 1, 0, -1):
        # Move current root to end
        arr[0], arr[i] = arr[i], arr[0]
        
        # Heapify reduced heap
        heapify_down_max(arr, i, 0)
    
    return arr

def heapify_down_max(arr, n, i):
    """
    Max heapify for heap sort
    """
    largest = i
    left = 2 * i + 1
    right = 2 * i + 2
    
    if left < n and arr[left] > arr[largest]:
        largest = left
    
    if right < n and arr[right] > arr[largest]:
        largest = right
    
    if largest != i:
        arr[i], arr[largest] = arr[largest], arr[i]
        heapify_down_max(arr, n, largest)
```

#### Dry Run: Heap Sort
```
Array: [4, 10, 3, 5, 1]

Step 1: Build Max Heap
       10
      /  \
     5    3
    / \
   4   1
Array: [10, 5, 3, 4, 1]

Step 2: Extract 10, heapify
Swap 10 and 1: [1, 5, 3, 4, | 10]
Heapify: [5, 4, 3, 1, | 10]

Step 3: Extract 5
Swap 5 and 1: [1, 4, 3, | 5, 10]
Heapify: [4, 1, 3, | 5, 10]

Step 4: Extract 4
Swap 4 and 3: [3, 1, | 4, 5, 10]

Step 5: Extract 3
Swap 3 and 1: [1, | 3, 4, 5, 10]

Final Sorted: [1, 3, 4, 5, 10]
```

---

## Priority Queue using Heap

```python
import heapq

class PriorityQueue:
    """
    Priority Queue using Python's heapq (min heap)
    """
    def __init__(self):
        self.heap = []
    
    def push(self, item, priority):
        """
        Add item with priority
        Lower priority number = higher priority
        Time: O(log n)
        """
        heapq.heappush(self.heap, (priority, item))
    
    def pop(self):
        """
        Remove and return highest priority item
        Time: O(log n)
        """
        if self.heap:
            return heapq.heappop(self.heap)[1]
        return None
    
    def peek(self):
        """
        Get highest priority item without removing
        Time: O(1)
        """
        return self.heap[0][1] if self.heap else None
    
    def is_empty(self):
        return len(self.heap) == 0

# Using heapq directly
import heapq

# Min heap
heap = []
heapq.heappush(heap, 4)
heapq.heappush(heap, 1)
heapq.heappush(heap, 7)
print(heapq.heappop(heap))  # 1

# Max heap (use negative values)
max_heap = []
heapq.heappush(max_heap, -4)
heapq.heappush(max_heap, -1)
heapq.heappush(max_heap, -7)
print(-heapq.heappop(max_heap))  # 7
```

---

## Classic Heap Problems

### 1. Kth Largest Element

```python
import heapq

def find_kth_largest(nums, k):
    """
    Find kth largest element
    Time: O(n log k)
    Space: O(k)
    
    Method: Use min heap of size k
    """
    # Min heap of size k
    heap = nums[:k]
    heapq.heapify(heap)
    
    for num in nums[k:]:
        if num > heap[0]:
            heapq.heapreplace(heap, num)
    
    return heap[0]

# Alternative: Max heap approach
def find_kth_largest_maxheap(nums, k):
    """
    Using max heap
    Time: O(n + k log n)
    """
    max_heap = [-num for num in nums]
    heapq.heapify(max_heap)
    
    for _ in range(k - 1):
        heapq.heappop(max_heap)
    
    return -heapq.heappop(max_heap)
```

---

### 2. Merge K Sorted Lists

```python
import heapq

def merge_k_sorted_lists(lists):
    """
    Merge k sorted lists
    Time: O(n log k) where n = total elements
    Space: O(k)
    """
    heap = []
    result = []
    
    # Add first element from each list
    for i, lst in enumerate(lists):
        if lst:
            heapq.heappush(heap, (lst[0], i, 0))
    
    while heap:
        val, list_idx, elem_idx = heapq.heappop(heap)
        result.append(val)
        
        # Add next element from same list
        if elem_idx + 1 < len(lists[list_idx]):
            next_val = lists[list_idx][elem_idx + 1]
            heapq.heappush(heap, (next_val, list_idx, elem_idx + 1))
    
    return result
```

#### Dry Run
```
Lists: [[1,4,5], [1,3,4], [2,6]]

Initial heap: [(1,0,0), (1,1,0), (2,2,0)]

Step 1: Pop (1,0,0), result=[1]
        Push (4,0,1)
        heap: [(1,1,0), (2,2,0), (4,0,1)]

Step 2: Pop (1,1,0), result=[1,1]
        Push (3,1,1)
        heap: [(2,2,0), (3,1,1), (4,0,1)]

Step 3: Pop (2,2,0), result=[1,1,2]
        Push (6,2,1)
        heap: [(3,1,1), (4,0,1), (6,2,1)]

Continue...
Final: [1,1,2,3,4,4,5,6]
```

---

### 3. Top K Frequent Elements

```python
import heapq
from collections import Counter

def top_k_frequent(nums, k):
    """
    Find k most frequent elements
    Time: O(n log k)
    Space: O(n)
    """
    # Count frequencies
    count = Counter(nums)
    
    # Min heap of size k
    heap = []
    
    for num, freq in count.items():
        heapq.heappush(heap, (freq, num))
        
        if len(heap) > k:
            heapq.heappop(heap)
    
    return [num for freq, num in heap]

# Alternative: Using bucket sort
def top_k_frequent_bucket(nums, k):
    """
    Using bucket sort
    Time: O(n)
    """
    count = Counter(nums)
    buckets = [[] for _ in range(len(nums) + 1)]
    
    for num, freq in count.items():
        buckets[freq].append(num)
    
    result = []
    for freq in range(len(buckets) - 1, 0, -1):
        result.extend(buckets[freq])
        if len(result) >= k:
            return result[:k]
    
    return result
```

---

### 4. Find Median from Data Stream

```python
import heapq

class MedianFinder:
    """
    Find median from stream of integers
    Time: O(log n) for add, O(1) for median
    Space: O(n)
    
    Uses two heaps:
    - Max heap for smaller half
    - Min heap for larger half
    """
    def __init__(self):
        self.small = []  # Max heap (use negative values)
        self.large = []  # Min heap
    
    def addNum(self, num):
        """Add number to data structure"""
        # Add to max heap (small)
        heapq.heappush(self.small, -num)
        
        # Balance: move largest from small to large
        if self.small and self.large and (-self.small[0] > self.large[0]):
            val = -heapq.heappop(self.small)
            heapq.heappush(self.large, val)
        
        # Balance sizes
        if len(self.small) > len(self.large) + 1:
            val = -heapq.heappop(self.small)
            heapq.heappush(self.large, val)
        
        if len(self.large) > len(self.small):
            val = heapq.heappop(self.large)
            heapq.heappush(self.small, -val)
    
    def findMedian(self):
        """Return median"""
        if len(self.small) > len(self.large):
            return -self.small[0]
        
        return (-self.small[0] + self.large[0]) / 2
```

#### Dry Run
```
Stream: [1, 2, 3, 4, 5]

After 1: small=[-1], large=[], median=1
After 2: small=[-1], large=[2], median=1.5
After 3: small=[-2,-1], large=[3], median=2
After 4: small=[-2,-1], large=[3,4], median=2.5
After 5: small=[-3,-2,-1], large=[4,5], median=3
```

---

### 5. Kth Smallest Element in Sorted Matrix

```python
import heapq

def kth_smallest_matrix(matrix, k):
    """
    Find kth smallest in n×n matrix
    Each row and column sorted
    Time: O(k log n)
    Space: O(n)
    """
    n = len(matrix)
    heap = []
    
    # Add first element of each row
    for i in range(min(n, k)):
        heapq.heappush(heap, (matrix[i][0], i, 0))
    
    result = 0
    for _ in range(k):
        result, row, col = heapq.heappop(heap)
        
        if col + 1 < n:
            heapq.heappush(heap, (matrix[row][col + 1], row, col + 1))
    
    return result
```

---

### 6. Reorganize String

```python
import heapq
from collections import Counter

def reorganize_string(s):
    """
    Rearrange so no two adjacent characters are same
    Time: O(n log k) where k = unique chars
    Space: O(k)
    """
    count = Counter(s)
    
    # Max heap by frequency
    max_heap = [(-freq, char) for char, freq in count.items()]
    heapq.heapify(max_heap)
    
    result = []
    prev_freq, prev_char = 0, ''
    
    while max_heap:
        freq, char = heapq.heappop(max_heap)
        result.append(char)
        
        # Add previous back
        if prev_freq < 0:
            heapq.heappush(max_heap, (prev_freq, prev_char))
        
        # Update previous
        prev_freq = freq + 1
        prev_char = char
    
    result_str = ''.join(result)
    
    # Check if valid
    return result_str if len(result_str) == len(s) else ""
```

---

### 7. Task Scheduler

```python
import heapq
from collections import Counter

def least_interval(tasks, n):
    """
    Minimum intervals to complete tasks with cooldown n
    Time: O(total_time)
    Space: O(26) = O(1)
    """
    # Count frequencies
    count = Counter(tasks)
    
    # Max heap
    max_heap = [-freq for freq in count.values()]
    heapq.heapify(max_heap)
    
    time = 0
    
    while max_heap:
        temp = []
        
        # Process n+1 tasks
        for _ in range(n + 1):
            if max_heap:
                freq = heapq.heappop(max_heap)
                if freq + 1 < 0:
                    temp.append(freq + 1)
            
            time += 1
            
            # If heap empty and no temp, done
            if not max_heap and not temp:
                break
        
        # Add back to heap
        for freq in temp:
            heapq.heappush(max_heap, freq)
    
    return time
```

---

### 8. Meeting Rooms II

```python
import heapq

def min_meeting_rooms(intervals):
    """
    Minimum meeting rooms needed
    Time: O(n log n)
    Space: O(n)
    """
    if not intervals:
        return 0
    
    # Sort by start time
    intervals.sort(key=lambda x: x[0])
    
    # Min heap for end times
    heap = []
    heapq.heappush(heap, intervals[0][1])
    
    for i in range(1, len(intervals)):
        # If room available, reuse it
        if intervals[i][0] >= heap[0]:
            heapq.heappop(heap)
        
        # Add current meeting's end time
        heapq.heappush(heap, intervals[i][1])
    
    return len(heap)
```

---

## Time Complexity Summary

| Operation | Time | Space |
|-----------|------|-------|
| Insert | O(log n) | O(1) |
| Extract Min/Max | O(log n) | O(1) |
| Get Min/Max | O(1) | O(1) |
| Build Heap | O(n) | O(1) |
| Heapify | O(log n) | O(1) |
| Heap Sort | O(n log n) | O(1) |

---

## Heap vs Other Structures

| Feature | Heap | BST | Array |
|---------|------|-----|-------|
| Find Min/Max | O(1) | O(log n) | O(1) sorted |
| Extract Min/Max | O(log n) | O(log n) | O(n) |
| Insert | O(log n) | O(log n) | O(1) end |
| Search | O(n) | O(log n) | O(log n) sorted |
| Space | O(1) | O(n) | O(1) |

---

## Exam Tips

### 1. MCQ Questions
**Q**: Time to build heap from array?
**A**: O(n) using heapify

**Q**: Time to extract min from min heap?
**A**: O(log n)

**Q**: Parent of node at index i?
**A**: (i-1) // 2

**Q**: Heap sort time complexity?
**A**: O(n log n)

### 2. Quick Reference
```
Min Heap: Parent < Children
Max Heap: Parent > Children
Complete: All levels filled left to right
```

### Key Points
1. **Complete Binary Tree**: Efficient array representation
2. **Heap Property**: Parent-child relationship
3. **O(1) Access**: Top element in constant time
4. **O(log n) Ops**: Insert and extract
5. **Build Heap**: O(n) more efficient than n insertions
6. **Priority Queue**: Main application of heaps
7. **Heapsort**: In-place O(n log n) sorting
8. **K Problems**: Use heap of size k
