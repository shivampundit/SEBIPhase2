# Multi-Language Programming Guide for Data Structures

## Overview
This guide provides comprehensive data structure implementations in **C, C++, Java, and Python**, making it a versatile resource for learning data structures across multiple programming languages.

---

## Supported Languages

### 1. **C**
- Low-level memory management
- Manual pointer operations
- Direct memory allocation with `malloc`/`free`
- Best for understanding fundamental concepts

### 2. **C++**
- Object-oriented approach
- Standard Template Library (STL) support
- Modern C++ features (C++11 and beyond)
- Balance between low-level control and high-level abstraction

### 3. **Java**
- Pure object-oriented programming
- Built-in Collections Framework
- Automatic memory management (Garbage Collection)
- Platform independence

### 4. **Python**
- High-level scripting language
- Dynamic typing
- Built-in data structures
- Concise and readable syntax

---

## Language-Specific Features

### Memory Management

| Language | Memory Management | Syntax |
|----------|------------------|--------|
| C | Manual (`malloc`/`free`) | `int* arr = (int*)malloc(n * sizeof(int));` |
| C++ | Manual + Smart Pointers | `vector<int> arr;` or `unique_ptr<int[]> arr(new int[n]);` |
| Java | Automatic (GC) | `int[] arr = new int[n];` or `ArrayList<Integer> arr = new ArrayList<>();` |
| Python | Automatic (GC) | `arr = []` or `arr = [0] * n` |

### Array/List Declaration

#### C
```c
// Static array
int arr[5];
int arr[] = {1, 2, 3, 4, 5};

// Dynamic array
int* arr = (int*)malloc(5 * sizeof(int));
```

#### C++
```cpp
// Static array
int arr[5];
int arr[] = {1, 2, 3, 4, 5};

// Dynamic with STL
vector<int> arr;
vector<int> arr = {1, 2, 3, 4, 5};
array<int, 5> arr = {1, 2, 3, 4, 5};
```

#### Java
```java
// Static array
int[] arr = new int[5];
int[] arr = {1, 2, 3, 4, 5};

// Dynamic with ArrayList
ArrayList<Integer> arr = new ArrayList<>();
ArrayList<Integer> arr = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));
```

#### Python
```python
# List (dynamic array)
arr = []
arr = [1, 2, 3, 4, 5]
arr = [0] * 5
```

---

## Data Structure Comparison

### Array Operations Time Complexity

| Operation | C | C++ (vector) | Java (ArrayList) | Python (list) |
|-----------|---|--------------|------------------|---------------|
| Access by index | O(1) | O(1) | O(1) | O(1) |
| Insert at end | O(1)* | O(1) amortized | O(1) amortized | O(1) amortized |
| Insert at beginning | O(n) | O(n) | O(n) | O(n) |
| Delete from end | O(1) | O(1) | O(1) | O(1) |
| Search | O(n) | O(n) | O(n) | O(n) |

*Requires manual reallocation in C

### Linked List Operations

| Operation | C | C++ | Java | Python |
|-----------|---|-----|------|--------|
| Insert at beginning | O(1) | O(1) | O(1) | O(1) |
| Insert at end | O(n) | O(n) | O(n) | O(n) |
| Delete node | O(1)* | O(1)* | O(1)* | O(1)* |
| Search | O(n) | O(n) | O(n) | O(n) |

*With node reference

### Stack Operations

| Operation | C | C++ (stack) | Java (Stack) | Python (list) |
|-----------|---|-------------|--------------|---------------|
| Push | O(1) | O(1) | O(1) | O(1) |
| Pop | O(1) | O(1) | O(1) | O(1) |
| Peek | O(1) | O(1) | O(1) | O(1) |

### Queue Operations

| Operation | C | C++ (queue) | Java (Queue) | Python (deque) |
|-----------|---|-------------|--------------|----------------|
| Enqueue | O(1) | O(1) | O(1) | O(1) |
| Dequeue | O(1) | O(1) | O(1) | O(1) |
| Peek | O(1) | O(1) | O(1) | O(1) |

---

## Standard Library Support

### C++ Standard Template Library (STL)

```cpp
#include <vector>      // Dynamic array
#include <list>        // Doubly linked list
#include <stack>       // Stack adapter
#include <queue>       // Queue adapter
#include <deque>       // Double-ended queue
#include <set>         // Ordered set
#include <map>         // Ordered map
#include <unordered_set>  // Hash set
#include <unordered_map>  // Hash map
```

### Java Collections Framework

```java
import java.util.ArrayList;      // Dynamic array
import java.util.LinkedList;     // Doubly linked list
import java.util.Stack;          // Stack
import java.util.Queue;          // Queue interface
import java.util.PriorityQueue;  // Priority queue
import java.util.HashSet;        // Hash set
import java.util.HashMap;        // Hash map
import java.util.TreeSet;        // Ordered set
import java.util.TreeMap;        // Ordered map
```

### Python Built-in Data Structures

```python
# list - Dynamic array
# tuple - Immutable sequence
# dict - Hash map
# set - Hash set
from collections import deque    # Double-ended queue
from collections import defaultdict  # Dict with default values
from heapq import *              # Heap operations
```

---

## Common Patterns Across Languages

### 1. Iterating Through Arrays

#### C
```c
for (int i = 0; i < size; i++) {
    printf("%d ", arr[i]);
}
```

#### C++
```cpp
// Traditional
for (int i = 0; i < arr.size(); i++) {
    cout << arr[i] << " ";
}

// Range-based (C++11)
for (int num : arr) {
    cout << num << " ";
}
```

#### Java
```java
// Traditional
for (int i = 0; i < arr.length; i++) {
    System.out.print(arr[i] + " ");
}

// Enhanced for loop
for (int num : arr) {
    System.out.print(num + " ");
}
```

#### Python
```python
# Index-based
for i in range(len(arr)):
    print(arr[i], end=" ")

# Direct iteration
for num in arr:
    print(num, end=" ")
```

### 2. Sorting Arrays

#### C
```c
#include <stdlib.h>

int compare(const void* a, const void* b) {
    return (*(int*)a - *(int*)b);
}

qsort(arr, size, sizeof(int), compare);
```

#### C++
```cpp
#include <algorithm>

sort(arr.begin(), arr.end());  // Ascending
sort(arr.begin(), arr.end(), greater<int>());  // Descending
```

#### Java
```java
import java.util.Arrays;
import java.util.Collections;

Arrays.sort(arr);  // Ascending
Arrays.sort(arr, Collections.reverseOrder());  // Descending
```

#### Python
```python
arr.sort()  # In-place
sorted_arr = sorted(arr)  # Returns new list
arr.sort(reverse=True)  # Descending
```

### 3. Searching in Arrays

#### C
```c
// Linear search
int search(int arr[], int size, int target) {
    for (int i = 0; i < size; i++) {
        if (arr[i] == target) return i;
    }
    return -1;
}
```

#### C++
```cpp
#include <algorithm>

// Linear search
auto it = find(arr.begin(), arr.end(), target);
if (it != arr.end()) {
    int index = distance(arr.begin(), it);
}

// Binary search (sorted array)
bool found = binary_search(arr.begin(), arr.end(), target);
```

#### Java
```java
// Linear search
int index = arr.indexOf(target);

// Binary search (sorted array)
int index = Arrays.binarySearch(arr, target);
```

#### Python
```python
# Linear search
try:
    index = arr.index(target)
except ValueError:
    index = -1

# Binary search (sorted array)
from bisect import bisect_left
index = bisect_left(arr, target)
```

---

## Language Selection Guide

### Choose C When:
- Learning fundamental memory management
- Working on embedded systems
- Performance is critical with low-level control
- Understanding pointer operations

### Choose C++ When:
- Need both low-level control and high-level abstractions
- Using object-oriented design
- Want STL for built-in data structures
- Building high-performance applications

### Choose Java When:
- Building enterprise applications
- Need platform independence
- Want automatic memory management
- Working with large codebases

### Choose Python When:
- Prototyping quickly
- Focus on algorithm logic over syntax
- Data analysis or scientific computing
- Scripting and automation

---

## Best Practices

### Memory Safety
- **C**: Always `free()` allocated memory
- **C++**: Use smart pointers (`unique_ptr`, `shared_ptr`)
- **Java/Python**: Trust garbage collection but avoid circular references

### Performance
- **C/C++**: Fastest execution, closest to hardware
- **Java**: JIT compilation provides good performance
- **Python**: Slowest but most readable

### Code Readability
- **Python**: Most readable and concise
- **Java**: Verbose but very clear
- **C++**: Modern C++ is readable with good practices
- **C**: Can be terse, requires good documentation

---

## Study Recommendations

### For Beginners
1. Start with **Python** to understand concepts
2. Move to **Java** for object-oriented thinking
3. Learn **C++** for performance and STL
4. Study **C** for memory management fundamentals

### For Interview Preparation
- Practice in your strongest language
- Know **C++** for competitive programming
- Be comfortable with **Java** or **Python** for most tech interviews
- Understand time/space complexity in all languages

### For System Design
- **Java** for backend services
- **C++** for performance-critical components
- **Python** for rapid prototyping
- **C** for system-level programming

---

## Resources by Language

### C Resources
- Structure implementations with pointers
- Manual memory management examples
- Low-level data structure operations

### C++ Resources
- STL container usage
- Modern C++ features (auto, range-for, lambdas)
- Object-oriented design patterns

### Java Resources
- Collections Framework
- Object-oriented principles
- Exception handling

### Python Resources
- Built-in data structures
- List comprehensions
- Pythonic idioms

---

## Summary

This multi-language approach helps you:
1. **Understand concepts** independent of language
2. **Compare implementations** across different paradigms
3. **Choose the right language** for your project
4. **Prepare for interviews** in multiple languages
5. **Learn transferable skills** that work everywhere

Master data structures in one language, and you can adapt to any language!
