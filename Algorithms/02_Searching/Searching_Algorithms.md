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

### Implementation (C)
```c
#include <stdio.h>

/**
 * Search for target in array sequentially
 * Returns: index if found, -1 otherwise
 */
int linear_search(int arr[], int n, int target) {
    for (int i = 0; i < n; i++) {
        if (arr[i] == target) {
            return i;
        }
    }
    return -1;
}

int main() {
    int arr[] = {10, 23, 45, 70, 11, 15};
    int n = sizeof(arr) / sizeof(arr[0]);
    int target = 70;
    
    int result = linear_search(arr, n, target);
    printf("Element found at index: %d\n", result);  // Output: 3
    
    // Not found case
    target = 100;
    result = linear_search(arr, n, target);
    printf("Element found at index: %d\n", result);  // Output: -1
    
    return 0;
}
```

### Implementation (C++)
```cpp
#include <iostream>
#include <vector>
using namespace std;

/**
 * Search for target in array sequentially
 * Returns: index if found, -1 otherwise
 */
int linear_search(const vector<int>& arr, int target) {
    for (int i = 0; i < arr.size(); i++) {
        if (arr[i] == target) {
            return i;
        }
    }
    return -1;
}

int main() {
    vector<int> arr = {10, 23, 45, 70, 11, 15};
    int target = 70;
    
    int result = linear_search(arr, target);
    cout << "Element found at index: " << result << endl;  // Output: 3
    
    // Not found case
    target = 100;
    result = linear_search(arr, target);
    cout << "Element found at index: " << result << endl;  // Output: -1
    
    return 0;
}
```

### Implementation (Java)
```java
public class LinearSearch {
    /**
     * Search for target in array sequentially
     * Returns: index if found, -1 otherwise
     */
    public static int linearSearch(int[] arr, int target) {
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] == target) {
                return i;
            }
        }
        return -1;
    }
    
    public static void main(String[] args) {
        int[] arr = {10, 23, 45, 70, 11, 15};
        int target = 70;
        
        int result = linearSearch(arr, target);
        System.out.println("Element found at index: " + result);  // Output: 3
        
        // Not found case
        target = 100;
        result = linearSearch(arr, target);
        System.out.println("Element found at index: " + result);  // Output: -1
    }
}
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

```c
#include <stdio.h>
#include <stdlib.h>

/**
 * Returns all indices where target appears
 * Returns count of occurrences
 */
int linear_search_all(int arr[], int n, int target, int** indices) {
    int* temp = (int*)malloc(n * sizeof(int));
    int count = 0;
    
    for (int i = 0; i < n; i++) {
        if (arr[i] == target) {
            temp[count++] = i;
        }
    }
    
    if (count > 0) {
        *indices = (int*)malloc(count * sizeof(int));
        for (int i = 0; i < count; i++) {
            (*indices)[i] = temp[i];
        }
    }
    
    free(temp);
    return count;
}

int main() {
    int arr[] = {10, 23, 45, 23, 11, 23};
    int n = sizeof(arr) / sizeof(arr[0]);
    int* indices = NULL;
    
    int count = linear_search_all(arr, n, 23, &indices);
    printf("Occurrences at indices: ");
    for (int i = 0; i < count; i++) {
        printf("%d ", indices[i]);
    }
    printf("\n");  // Output: 1 3 5
    
    free(indices);
    return 0;
}
```

```cpp
#include <iostream>
#include <vector>
using namespace std;

/**
 * Returns all indices where target appears
 */
vector<int> linear_search_all(const vector<int>& arr, int target) {
    vector<int> indices;
    
    for (int i = 0; i < arr.size(); i++) {
        if (arr[i] == target) {
            indices.push_back(i);
        }
    }
    
    return indices;
}

int main() {
    vector<int> arr = {10, 23, 45, 23, 11, 23};
    vector<int> result = linear_search_all(arr, 23);
    
    cout << "Occurrences at indices: ";
    for (int idx : result) {
        cout << idx << " ";
    }
    cout << endl;  // Output: 1 3 5
    
    return 0;
}
```

```java
import java.util.ArrayList;
import java.util.List;

public class LinearSearchAll {
    /**
     * Returns all indices where target appears
     */
    public static List<Integer> linearSearchAll(int[] arr, int target) {
        List<Integer> indices = new ArrayList<>();
        
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] == target) {
                indices.add(i);
            }
        }
        
        return indices;
    }
    
    public static void main(String[] args) {
        int[] arr = {10, 23, 45, 23, 11, 23};
        List<Integer> result = linearSearchAll(arr, 23);
        
        System.out.print("Occurrences at indices: ");
        for (int idx : result) {
            System.out.print(idx + " ");
        }
        System.out.println();  // Output: 1 3 5
    }
}
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

```c
#include <stdio.h>

/**
 * Optimized linear search using sentinel
 */
int sentinel_search(int arr[], int n, int target) {
    int last = arr[n - 1];
    arr[n - 1] = target;
    
    int i = 0;
    while (arr[i] != target) {
        i++;
    }
    
    arr[n - 1] = last;  // Restore last element
    
    if (i < n - 1 || arr[n - 1] == target) {
        return i;
    }
    return -1;
}

int main() {
    int arr[] = {10, 23, 45, 70, 11, 15};
    int n = sizeof(arr) / sizeof(arr[0]);
    
    int result = sentinel_search(arr, n, 70);
    printf("Element found at index: %d\n", result);
    
    return 0;
}
```

```cpp
#include <iostream>
#include <vector>
using namespace std;

/**
 * Optimized linear search using sentinel
 */
int sentinel_search(vector<int>& arr, int target) {
    int n = arr.size();
    int last = arr[n - 1];
    arr[n - 1] = target;
    
    int i = 0;
    while (arr[i] != target) {
        i++;
    }
    
    arr[n - 1] = last;  // Restore last element
    
    if (i < n - 1 || arr[n - 1] == target) {
        return i;
    }
    return -1;
}

int main() {
    vector<int> arr = {10, 23, 45, 70, 11, 15};
    
    int result = sentinel_search(arr, 70);
    cout << "Element found at index: " << result << endl;
    
    return 0;
}
```

```java
public class SentinelSearch {
    /**
     * Optimized linear search using sentinel
     */
    public static int sentinelSearch(int[] arr, int target) {
        int n = arr.length;
        int last = arr[n - 1];
        arr[n - 1] = target;
        
        int i = 0;
        while (arr[i] != target) {
            i++;
        }
        
        arr[n - 1] = last;  // Restore last element
        
        if (i < n - 1 || arr[n - 1] == target) {
            return i;
        }
        return -1;
    }
    
    public static void main(String[] args) {
        int[] arr = {10, 23, 45, 70, 11, 15};
        
        int result = sentinelSearch(arr, 70);
        System.out.println("Element found at index: " + result);
    }
}
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

```c
#include <stdio.h>

/**
 * Binary search using iteration
 * Precondition: arr must be sorted
 */
int binary_search_iterative(int arr[], int n, int target) {
    int left = 0;
    int right = n - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;  // Avoid overflow
        
        if (arr[mid] == target) {
            return mid;
        } else if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return -1;
}

int main() {
    int arr[] = {2, 3, 4, 10, 40, 50, 60, 70};
    int n = sizeof(arr) / sizeof(arr[0]);
    int target = 10;
    
    int result = binary_search_iterative(arr, n, target);
    printf("Element found at index: %d\n", result);  // Output: 3
    
    return 0;
}
```

```cpp
#include <iostream>
#include <vector>
using namespace std;

/**
 * Binary search using iteration
 * Precondition: arr must be sorted
 */
int binary_search_iterative(const vector<int>& arr, int target) {
    int left = 0;
    int right = arr.size() - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;  // Avoid overflow
        
        if (arr[mid] == target) {
            return mid;
        } else if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return -1;
}

int main() {
    vector<int> arr = {2, 3, 4, 10, 40, 50, 60, 70};
    int target = 10;
    
    int result = binary_search_iterative(arr, target);
    cout << "Element found at index: " << result << endl;  // Output: 3
    
    return 0;
}
```

```java
public class BinarySearchIterative {
    /**
     * Binary search using iteration
     * Precondition: arr must be sorted
     */
    public static int binarySearchIterative(int[] arr, int target) {
        int left = 0;
        int right = arr.length - 1;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;  // Avoid overflow
            
            if (arr[mid] == target) {
                return mid;
            } else if (arr[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        
        return -1;
    }
    
    public static void main(String[] args) {
        int[] arr = {2, 3, 4, 10, 40, 50, 60, 70};
        int target = 10;
        
        int result = binarySearchIterative(arr, target);
        System.out.println("Element found at index: " + result);  // Output: 3
    }
}
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

```c
#include <stdio.h>

/**
 * Binary search using recursion
 */
int binary_search_recursive(int arr[], int target, int left, int right) {
    if (left > right) {
        return -1;
    }
    
    int mid = left + (right - left) / 2;
    
    if (arr[mid] == target) {
        return mid;
    } else if (arr[mid] < target) {
        return binary_search_recursive(arr, target, mid + 1, right);
    } else {
        return binary_search_recursive(arr, target, left, mid - 1);
    }
}

int main() {
    int arr[] = {2, 3, 4, 10, 40, 50, 60, 70};
    int n = sizeof(arr) / sizeof(arr[0]);
    int target = 10;
    
    int result = binary_search_recursive(arr, target, 0, n - 1);
    printf("Element found at index: %d\n", result);  // Output: 3
    
    return 0;
}
```

```cpp
#include <iostream>
#include <vector>
using namespace std;

/**
 * Binary search using recursion
 */
int binary_search_recursive(const vector<int>& arr, int target, int left, int right) {
    if (left > right) {
        return -1;
    }
    
    int mid = left + (right - left) / 2;
    
    if (arr[mid] == target) {
        return mid;
    } else if (arr[mid] < target) {
        return binary_search_recursive(arr, target, mid + 1, right);
    } else {
        return binary_search_recursive(arr, target, left, mid - 1);
    }
}

int main() {
    vector<int> arr = {2, 3, 4, 10, 40, 50, 60, 70};
    int target = 10;
    
    int result = binary_search_recursive(arr, target, 0, arr.size() - 1);
    cout << "Element found at index: " << result << endl;  // Output: 3
    
    return 0;
}
```

```java
public class BinarySearchRecursive {
    /**
     * Binary search using recursion
     */
    public static int binarySearchRecursive(int[] arr, int target, int left, int right) {
        if (left > right) {
            return -1;
        }
        
        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target) {
            return mid;
        } else if (arr[mid] < target) {
            return binarySearchRecursive(arr, target, mid + 1, right);
        } else {
            return binarySearchRecursive(arr, target, left, mid - 1);
        }
    }
    
    public static void main(String[] args) {
        int[] arr = {2, 3, 4, 10, 40, 50, 60, 70};
        int target = 10;
        
        int result = binarySearchRecursive(arr, target, 0, arr.length - 1);
        System.out.println("Element found at index: " + result);  // Output: 3
    }
}
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

```c
#include <stdio.h>

/**
 * Find first occurrence of target
 */
int binary_search_first(int arr[], int n, int target) {
    int left = 0;
    int right = n - 1;
    int result = -1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target) {
            result = mid;
            right = mid - 1;  // Continue searching left
        } else if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return result;
}

int main() {
    int arr[] = {1, 2, 2, 2, 3, 4, 5};
    int n = sizeof(arr) / sizeof(arr[0]);
    
    printf("First occurrence at: %d\n", binary_search_first(arr, n, 2));  // Output: 1
    
    return 0;
}
```

```cpp
#include <iostream>
#include <vector>
using namespace std;

/**
 * Find first occurrence of target
 */
int binary_search_first(const vector<int>& arr, int target) {
    int left = 0;
    int right = arr.size() - 1;
    int result = -1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target) {
            result = mid;
            right = mid - 1;  // Continue searching left
        } else if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return result;
}

int main() {
    vector<int> arr = {1, 2, 2, 2, 3, 4, 5};
    
    cout << "First occurrence at: " << binary_search_first(arr, 2) << endl;  // Output: 1
    
    return 0;
}
```

```java
public class BinarySearchFirst {
    /**
     * Find first occurrence of target
     */
    public static int binarySearchFirst(int[] arr, int target) {
        int left = 0;
        int right = arr.length - 1;
        int result = -1;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            
            if (arr[mid] == target) {
                result = mid;
                right = mid - 1;  // Continue searching left
            } else if (arr[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        
        return result;
    }
    
    public static void main(String[] args) {
        int[] arr = {1, 2, 2, 2, 3, 4, 5};
        
        System.out.println("First occurrence at: " + binarySearchFirst(arr, 2));  // Output: 1
    }
}
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

```c
#include <stdio.h>

/**
 * Find last occurrence of target
 */
int binary_search_last(int arr[], int n, int target) {
    int left = 0;
    int right = n - 1;
    int result = -1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target) {
            result = mid;
            left = mid + 1;  // Continue searching right
        } else if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return result;
}

int main() {
    int arr[] = {1, 2, 2, 2, 3, 4, 5};
    int n = sizeof(arr) / sizeof(arr[0]);
    
    printf("Last occurrence at: %d\n", binary_search_last(arr, n, 2));  // Output: 3
    
    return 0;
}
```

```cpp
#include <iostream>
#include <vector>
using namespace std;

/**
 * Find last occurrence of target
 */
int binary_search_last(const vector<int>& arr, int target) {
    int left = 0;
    int right = arr.size() - 1;
    int result = -1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target) {
            result = mid;
            left = mid + 1;  // Continue searching right
        } else if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return result;
}

int main() {
    vector<int> arr = {1, 2, 2, 2, 3, 4, 5};
    
    cout << "Last occurrence at: " << binary_search_last(arr, 2) << endl;  // Output: 3
    
    return 0;
}
```

```java
public class BinarySearchLast {
    /**
     * Find last occurrence of target
     */
    public static int binarySearchLast(int[] arr, int target) {
        int left = 0;
        int right = arr.length - 1;
        int result = -1;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            
            if (arr[mid] == target) {
                result = mid;
                left = mid + 1;  // Continue searching right
            } else if (arr[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        
        return result;
    }
    
    public static void main(String[] args) {
        int[] arr = {1, 2, 2, 2, 3, 4, 5};
        
        System.out.println("Last occurrence at: " + binarySearchLast(arr, 2));  // Output: 3
    }
}
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

```c
#include <stdio.h>

// Forward declarations (assuming previous functions are available)
int binary_search_first(int arr[], int n, int target);
int binary_search_last(int arr[], int n, int target);

/**
 * Count occurrences using binary search
 */
int count_occurrences(int arr[], int n, int target) {
    int first = binary_search_first(arr, n, target);
    if (first == -1) {
        return 0;
    }
    
    int last = binary_search_last(arr, n, target);
    return last - first + 1;
}

int main() {
    int arr[] = {1, 2, 2, 2, 2, 3, 4, 5};
    int n = sizeof(arr) / sizeof(arr[0]);
    
    printf("Count: %d\n", count_occurrences(arr, n, 2));  // Output: 4
    
    return 0;
}
```

```cpp
#include <iostream>
#include <vector>
using namespace std;

// Forward declarations (assuming previous functions are available)
int binary_search_first(const vector<int>& arr, int target);
int binary_search_last(const vector<int>& arr, int target);

/**
 * Count occurrences using binary search
 */
int count_occurrences(const vector<int>& arr, int target) {
    int first = binary_search_first(arr, target);
    if (first == -1) {
        return 0;
    }
    
    int last = binary_search_last(arr, target);
    return last - first + 1;
}

int main() {
    vector<int> arr = {1, 2, 2, 2, 2, 3, 4, 5};
    
    cout << "Count: " << count_occurrences(arr, 2) << endl;  // Output: 4
    
    return 0;
}
```

```java
public class CountOccurrences {
    // Assuming binarySearchFirst and binarySearchLast methods are available
    
    /**
     * Count occurrences using binary search
     */
    public static int countOccurrences(int[] arr, int target) {
        int first = binarySearchFirst(arr, target);
        if (first == -1) {
            return 0;
        }
        
        int last = binarySearchLast(arr, target);
        return last - first + 1;
    }
    
    public static void main(String[] args) {
        int[] arr = {1, 2, 2, 2, 2, 3, 4, 5};
        
        System.out.println("Count: " + countOccurrences(arr, 2));  // Output: 4
    }
}
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

### Implementation (C)
```c
#include <stdio.h>
#include <math.h>

/**
 * Jump search on sorted array
 * Optimal jump size is √n
 */
int jump_search(int arr[], int n, int target) {
    int step = (int)sqrt(n);
    int prev = 0;
    
    // Find the block where element may be present
    while (arr[(step < n ? step : n) - 1] < target) {
        prev = step;
        step += (int)sqrt(n);
        if (prev >= n) {
            return -1;
        }
    }
    
    // Linear search in the identified block
    while (arr[prev] < target) {
        prev++;
        if (prev == (step < n ? step : n)) {
            return -1;
        }
    }
    
    // If element is found
    if (arr[prev] == target) {
        return prev;
    }
    
    return -1;
}

int main() {
    int arr[] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
    int n = sizeof(arr) / sizeof(arr[0]);
    int target = 6;
    
    int result = jump_search(arr, n, target);
    printf("Element found at index: %d\n", result);  // Output: 6
    
    return 0;
}
```

### Implementation (C++)
```cpp
#include <iostream>
#include <vector>
#include <cmath>
using namespace std;

/**
 * Jump search on sorted array
 * Optimal jump size is √n
 */
int jump_search(const vector<int>& arr, int target) {
    int n = arr.size();
    int step = (int)sqrt(n);
    int prev = 0;
    
    // Find the block where element may be present
    while (arr[min(step, n) - 1] < target) {
        prev = step;
        step += (int)sqrt(n);
        if (prev >= n) {
            return -1;
        }
    }
    
    // Linear search in the identified block
    while (arr[prev] < target) {
        prev++;
        if (prev == min(step, n)) {
            return -1;
        }
    }
    
    // If element is found
    if (arr[prev] == target) {
        return prev;
    }
    
    return -1;
}

int main() {
    vector<int> arr = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
    int target = 6;
    
    int result = jump_search(arr, target);
    cout << "Element found at index: " << result << endl;  // Output: 6
    
    return 0;
}
```

### Implementation (Java)
```java
public class JumpSearch {
    /**
     * Jump search on sorted array
     * Optimal jump size is √n
     */
    public static int jumpSearch(int[] arr, int target) {
        int n = arr.length;
        int step = (int)Math.sqrt(n);
        int prev = 0;
        
        // Find the block where element may be present
        while (arr[Math.min(step, n) - 1] < target) {
            prev = step;
            step += (int)Math.sqrt(n);
            if (prev >= n) {
                return -1;
            }
        }
        
        // Linear search in the identified block
        while (arr[prev] < target) {
            prev++;
            if (prev == Math.min(step, n)) {
                return -1;
            }
        }
        
        // If element is found
        if (arr[prev] == target) {
            return prev;
        }
        
        return -1;
    }
    
    public static void main(String[] args) {
        int[] arr = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
        int target = 6;
        
        int result = jumpSearch(arr, target);
        System.out.println("Element found at index: " + result);  // Output: 6
    }
}
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

### Implementation (C)
```c
#include <stdio.h>

/**
 * Interpolation search on uniformly distributed sorted array
 * Estimates position based on value
 */
int interpolation_search(int arr[], int n, int target) {
    int low = 0;
    int high = n - 1;
    
    while (low <= high && arr[low] <= target && target <= arr[high]) {
        if (low == high) {
            if (arr[low] == target) {
                return low;
            }
            return -1;
        }
        
        // Estimate position using interpolation formula
        int pos = low + (((target - arr[low]) * (high - low)) / 
                        (arr[high] - arr[low]));
        
        if (arr[pos] == target) {
            return pos;
        } else if (arr[pos] < target) {
            low = pos + 1;
        } else {
            high = pos - 1;
        }
    }
    
    return -1;
}

int main() {
    int arr[] = {10, 20, 30, 40, 50, 60, 70, 80, 90, 100};
    int n = sizeof(arr) / sizeof(arr[0]);
    int target = 70;
    
    int result = interpolation_search(arr, n, target);
    printf("Element found at index: %d\n", result);  // Output: 6
    
    return 0;
}
```

### Implementation (C++)
```cpp
#include <iostream>
#include <vector>
using namespace std;

/**
 * Interpolation search on uniformly distributed sorted array
 * Estimates position based on value
 */
int interpolation_search(const vector<int>& arr, int target) {
    int low = 0;
    int high = arr.size() - 1;
    
    while (low <= high && arr[low] <= target && target <= arr[high]) {
        if (low == high) {
            if (arr[low] == target) {
                return low;
            }
            return -1;
        }
        
        // Estimate position using interpolation formula
        int pos = low + (((target - arr[low]) * (high - low)) / 
                        (arr[high] - arr[low]));
        
        if (arr[pos] == target) {
            return pos;
        } else if (arr[pos] < target) {
            low = pos + 1;
        } else {
            high = pos - 1;
        }
    }
    
    return -1;
}

int main() {
    vector<int> arr = {10, 20, 30, 40, 50, 60, 70, 80, 90, 100};
    int target = 70;
    
    int result = interpolation_search(arr, target);
    cout << "Element found at index: " << result << endl;  // Output: 6
    
    return 0;
}
```

### Implementation (Java)
```java
public class InterpolationSearch {
    /**
     * Interpolation search on uniformly distributed sorted array
     * Estimates position based on value
     */
    public static int interpolationSearch(int[] arr, int target) {
        int low = 0;
        int high = arr.length - 1;
        
        while (low <= high && arr[low] <= target && target <= arr[high]) {
            if (low == high) {
                if (arr[low] == target) {
                    return low;
                }
                return -1;
            }
            
            // Estimate position using interpolation formula
            int pos = low + (((target - arr[low]) * (high - low)) / 
                            (arr[high] - arr[low]));
            
            if (arr[pos] == target) {
                return pos;
            } else if (arr[pos] < target) {
                low = pos + 1;
            } else {
                high = pos - 1;
            }
        }
        
        return -1;
    }
    
    public static void main(String[] args) {
        int[] arr = {10, 20, 30, 40, 50, 60, 70, 80, 90, 100};
        int target = 70;
        
        int result = interpolationSearch(arr, target);
        System.out.println("Element found at index: " + result);  // Output: 6
    }
}
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

### Implementation (C)
```c
#include <stdio.h>

/**
 * Binary search in specified range
 */
int binary_search_range(int arr[], int target, int left, int right) {
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target) {
            return mid;
        } else if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return -1;
}

/**
 * Exponential search: find range then binary search
 * Useful for unbounded/infinite arrays
 */
int exponential_search(int arr[], int n, int target) {
    // If element is at first position
    if (arr[0] == target) {
        return 0;
    }
    
    // Find range for binary search by repeated doubling
    int i = 1;
    while (i < n && arr[i] <= target) {
        i *= 2;
    }
    
    // Binary search in the found range
    return binary_search_range(arr, target, i / 2, (i < n ? i : n) - 1);
}

int main() {
    int arr[] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 20, 30, 40, 50};
    int n = sizeof(arr) / sizeof(arr[0]);
    int target = 30;
    
    int result = exponential_search(arr, n, target);
    printf("Element found at index: %d\n", result);  // Output: 11
    
    return 0;
}
```

### Implementation (C++)
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

/**
 * Binary search in specified range
 */
int binary_search_range(const vector<int>& arr, int target, int left, int right) {
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target) {
            return mid;
        } else if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return -1;
}

/**
 * Exponential search: find range then binary search
 * Useful for unbounded/infinite arrays
 */
int exponential_search(const vector<int>& arr, int target) {
    int n = arr.size();
    
    // If element is at first position
    if (arr[0] == target) {
        return 0;
    }
    
    // Find range for binary search by repeated doubling
    int i = 1;
    while (i < n && arr[i] <= target) {
        i *= 2;
    }
    
    // Binary search in the found range
    return binary_search_range(arr, target, i / 2, min(i, n - 1));
}

int main() {
    vector<int> arr = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 20, 30, 40, 50};
    int target = 30;
    
    int result = exponential_search(arr, target);
    cout << "Element found at index: " << result << endl;  // Output: 11
    
    return 0;
}
```

### Implementation (Java)
```java
public class ExponentialSearch {
    /**
     * Binary search in specified range
     */
    public static int binarySearchRange(int[] arr, int target, int left, int right) {
        while (left <= right) {
            int mid = left + (right - left) / 2;
            
            if (arr[mid] == target) {
                return mid;
            } else if (arr[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        
        return -1;
    }
    
    /**
     * Exponential search: find range then binary search
     * Useful for unbounded/infinite arrays
     */
    public static int exponentialSearch(int[] arr, int target) {
        int n = arr.length;
        
        // If element is at first position
        if (arr[0] == target) {
            return 0;
        }
        
        // Find range for binary search by repeated doubling
        int i = 1;
        while (i < n && arr[i] <= target) {
            i *= 2;
        }
        
        // Binary search in the found range
        return binarySearchRange(arr, target, i / 2, Math.min(i, n - 1));
    }
    
    public static void main(String[] args) {
        int[] arr = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 20, 30, 40, 50};
        int target = 30;
        
        int result = exponentialSearch(arr, target);
        System.out.println("Element found at index: " + result);  // Output: 11
    }
}
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

### Implementation (C)
```c
#include <stdio.h>

/**
 * Ternary search divides array into 3 parts
 */
int ternary_search(int arr[], int target, int left, int right) {
    if (left <= right) {
        // Divide array into 3 parts
        int mid1 = left + (right - left) / 3;
        int mid2 = right - (right - left) / 3;
        
        if (arr[mid1] == target) {
            return mid1;
        }
        if (arr[mid2] == target) {
            return mid2;
        }
        
        // Determine which third to search
        if (target < arr[mid1]) {
            return ternary_search(arr, target, left, mid1 - 1);
        } else if (target > arr[mid2]) {
            return ternary_search(arr, target, mid2 + 1, right);
        } else {
            return ternary_search(arr, target, mid1 + 1, mid2 - 1);
        }
    }
    
    return -1;
}

int main() {
    int arr[] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    int n = sizeof(arr) / sizeof(arr[0]);
    int target = 7;
    
    int result = ternary_search(arr, target, 0, n - 1);
    printf("Element found at index: %d\n", result);  // Output: 6
    
    return 0;
}
```

### Implementation (C++)
```cpp
#include <iostream>
#include <vector>
using namespace std;

/**
 * Ternary search divides array into 3 parts
 */
int ternary_search(const vector<int>& arr, int target, int left, int right) {
    if (left <= right) {
        // Divide array into 3 parts
        int mid1 = left + (right - left) / 3;
        int mid2 = right - (right - left) / 3;
        
        if (arr[mid1] == target) {
            return mid1;
        }
        if (arr[mid2] == target) {
            return mid2;
        }
        
        // Determine which third to search
        if (target < arr[mid1]) {
            return ternary_search(arr, target, left, mid1 - 1);
        } else if (target > arr[mid2]) {
            return ternary_search(arr, target, mid2 + 1, right);
        } else {
            return ternary_search(arr, target, mid1 + 1, mid2 - 1);
        }
    }
    
    return -1;
}

int main() {
    vector<int> arr = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    int target = 7;
    
    int result = ternary_search(arr, target, 0, arr.size() - 1);
    cout << "Element found at index: " << result << endl;  // Output: 6
    
    return 0;
}
```

### Implementation (Java)
```java
public class TernarySearch {
    /**
     * Ternary search divides array into 3 parts
     */
    public static int ternarySearch(int[] arr, int target, int left, int right) {
        if (left <= right) {
            // Divide array into 3 parts
            int mid1 = left + (right - left) / 3;
            int mid2 = right - (right - left) / 3;
            
            if (arr[mid1] == target) {
                return mid1;
            }
            if (arr[mid2] == target) {
                return mid2;
            }
            
            // Determine which third to search
            if (target < arr[mid1]) {
                return ternarySearch(arr, target, left, mid1 - 1);
            } else if (target > arr[mid2]) {
                return ternarySearch(arr, target, mid2 + 1, right);
            } else {
                return ternarySearch(arr, target, mid1 + 1, mid2 - 1);
            }
        }
        
        return -1;
    }
    
    public static void main(String[] args) {
        int[] arr = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
        int target = 7;
        
        int result = ternarySearch(arr, target, 0, arr.length - 1);
        System.out.println("Element found at index: " + result);  // Output: 6
    }
}
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

```c
#include <stdio.h>

/**
 * Find maximum in unimodal array (increases then decreases)
 */
int ternary_search_max(int arr[], int left, int right) {
    while (right - left > 2) {
        int mid1 = left + (right - left) / 3;
        int mid2 = right - (right - left) / 3;
        
        if (arr[mid1] < arr[mid2]) {
            left = mid1;
        } else {
            right = mid2;
        }
    }
    
    // Check remaining elements
    int max_val = arr[left];
    int max_idx = left;
    for (int i = left + 1; i <= right; i++) {
        if (arr[i] > max_val) {
            max_val = arr[i];
            max_idx = i;
        }
    }
    
    return max_idx;
}

int main() {
    int arr[] = {1, 3, 5, 7, 9, 8, 6, 4, 2};
    int n = sizeof(arr) / sizeof(arr[0]);
    
    int result = ternary_search_max(arr, 0, n - 1);
    printf("Maximum at index: %d, value: %d\n", result, arr[result]);  // Output: 4, 9
    
    return 0;
}
```

```cpp
#include <iostream>
#include <vector>
using namespace std;

/**
 * Find maximum in unimodal array (increases then decreases)
 */
int ternary_search_max(const vector<int>& arr, int left, int right) {
    while (right - left > 2) {
        int mid1 = left + (right - left) / 3;
        int mid2 = right - (right - left) / 3;
        
        if (arr[mid1] < arr[mid2]) {
            left = mid1;
        } else {
            right = mid2;
        }
    }
    
    // Check remaining elements
    int max_val = arr[left];
    int max_idx = left;
    for (int i = left + 1; i <= right; i++) {
        if (arr[i] > max_val) {
            max_val = arr[i];
            max_idx = i;
        }
    }
    
    return max_idx;
}

int main() {
    vector<int> arr = {1, 3, 5, 7, 9, 8, 6, 4, 2};
    
    int result = ternary_search_max(arr, 0, arr.size() - 1);
    cout << "Maximum at index: " << result << ", value: " << arr[result] << endl;  // Output: 4, 9
    
    return 0;
}
```

```java
public class TernarySearchMax {
    /**
     * Find maximum in unimodal array (increases then decreases)
     */
    public static int ternarySearchMax(int[] arr, int left, int right) {
        while (right - left > 2) {
            int mid1 = left + (right - left) / 3;
            int mid2 = right - (right - left) / 3;
            
            if (arr[mid1] < arr[mid2]) {
                left = mid1;
            } else {
                right = mid2;
            }
        }
        
        // Check remaining elements
        int max_val = arr[left];
        int max_idx = left;
        for (int i = left + 1; i <= right; i++) {
            if (arr[i] > max_val) {
                max_val = arr[i];
                max_idx = i;
            }
        }
        
        return max_idx;
    }
    
    public static void main(String[] args) {
        int[] arr = {1, 3, 5, 7, 9, 8, 6, 4, 2};
        
        int result = ternarySearchMax(arr, 0, arr.length - 1);
        System.out.println("Maximum at index: " + result + ", value: " + arr[result]);  // Output: 4, 9
    }
}
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
