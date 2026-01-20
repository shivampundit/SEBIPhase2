# Array - Comprehensive Notes

## Overview
An array is a linear data structure that stores elements of the same type in contiguous memory locations. Arrays are one of the most fundamental and widely used data structures.

### Key Characteristics
1. **Fixed size** (in most languages)
2. **Contiguous memory** allocation
3. **Random access** - O(1) time
4. **Indexed** - starting from 0
5. **Homogeneous** - same data type

---

## Array Declaration and Initialization

### C
```c
#include <stdio.h>
#include <stdlib.h>

// Fixed size array
int arr[5];

// With initialization
int arr[] = {1, 2, 3, 4, 5};
int arr[5] = {1, 2, 3, 4, 5};

// Dynamic array using malloc
int* arr = (int*)malloc(5 * sizeof(int));

// 2D array
int matrix[3][3];
int matrix[3][3] = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}};

// Dynamic 2D array
int** matrix = (int**)malloc(3 * sizeof(int*));
for (int i = 0; i < 3; i++) {
    matrix[i] = (int*)malloc(3 * sizeof(int));
}
```

### C++
```cpp
#include <iostream>
#include <vector>
#include <array>
using namespace std;

// Fixed size array
int arr[5];

// With initialization
int arr[] = {1, 2, 3, 4, 5};
int arr[5] = {1, 2, 3, 4, 5};

// Using std::array (C++11)
array<int, 5> arr = {1, 2, 3, 4, 5};

// Using std::vector (dynamic)
vector<int> arr;
vector<int> arr = {1, 2, 3, 4, 5};
vector<int> arr(5);  // 5 elements initialized to 0
vector<int> arr(5, 10);  // 5 elements initialized to 10

// 2D array
int matrix[3][3];
int matrix[3][3] = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}};

// 2D vector
vector<vector<int>> matrix(3, vector<int>(3));  // 3x3 matrix
vector<vector<int>> matrix = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}};
```

### Java
```java
import java.util.ArrayList;
import java.util.Arrays;

// Declaration
int[] arr = new int[5];

// With initialization
int[] arr = {1, 2, 3, 4, 5};
int[] arr = new int[]{1, 2, 3, 4, 5};

// Using ArrayList (dynamic)
ArrayList<Integer> arr = new ArrayList<>();
ArrayList<Integer> arr = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));

// 2D array
int[][] matrix = new int[3][3];
int[][] matrix = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}};

// 2D ArrayList
ArrayList<ArrayList<Integer>> matrix = new ArrayList<>();
```

### Python
```python
# Empty array
arr = []

# With initial values
arr = [1, 2, 3, 4, 5]

# Using list comprehension
arr = [0] * 10  # [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
arr = [i for i in range(5)]  # [0, 1, 2, 3, 4]

# 2D array
matrix = [[0] * 3 for _ in range(3)]  # 3x3 matrix
```

---

## Basic Operations

### 1. Access/Read

#### C
```c
int access_element(int arr[], int size, int index) {
    /*
     * Access element at given index
     * Time: O(1)
     * Space: O(1)
     */
    if (index >= 0 && index < size) {
        return arr[index];
    }
    return -1;  // Error value
}

int main() {
    int arr[] = {10, 20, 30, 40, 50};
    int size = sizeof(arr) / sizeof(arr[0]);
    printf("%d\n", access_element(arr, size, 2));  // 30
    return 0;
}
```

#### C++
```cpp
int access_element(vector<int>& arr, int index) {
    /*
     * Access element at given index
     * Time: O(1)
     * Space: O(1)
     */
    if (index >= 0 && index < arr.size()) {
        return arr[index];
    }
    throw out_of_range("Index out of bounds");
}

int main() {
    vector<int> arr = {10, 20, 30, 40, 50};
    cout << access_element(arr, 2) << endl;  // 30
    return 0;
}
```

#### Java
```java
public static int accessElement(int[] arr, int index) {
    /*
     * Access element at given index
     * Time: O(1)
     * Space: O(1)
     */
    if (index >= 0 && index < arr.length) {
        return arr[index];
    }
    throw new IndexOutOfBoundsException("Index out of bounds");
}

public static void main(String[] args) {
    int[] arr = {10, 20, 30, 40, 50};
    System.out.println(accessElement(arr, 2));  // 30
}
```

#### Python
```python
def access_element(arr, index):
    """
    Access element at given index
    Time: O(1)
    Space: O(1)
    """
    if 0 <= index < len(arr):
        return arr[index]
    return None

arr = [10, 20, 30, 40, 50]
print(access_element(arr, 2))  # 30
```

### 2. Insert

#### At End

##### C
```c
int* insert_at_end(int* arr, int* size, int* capacity, int element) {
    /*
     * Insert at end (dynamic array)
     * Time: O(1) amortized
     * Space: O(1)
     */
    if (*size >= *capacity) {
        *capacity *= 2;
        arr = (int*)realloc(arr, (*capacity) * sizeof(int));
    }
    arr[*size] = element;
    (*size)++;
    return arr;
}
```

##### C++
```cpp
void insert_at_end(vector<int>& arr, int element) {
    /*
     * Insert at end
     * Time: O(1) amortized
     * Space: O(1)
     */
    arr.push_back(element);
}

// Example
vector<int> arr = {1, 2, 3};
insert_at_end(arr, 4);
// arr is now {1, 2, 3, 4}
```

##### Java
```java
public static ArrayList<Integer> insertAtEnd(ArrayList<Integer> arr, int element) {
    /*
     * Insert at end
     * Time: O(1) amortized
     * Space: O(1)
     */
    arr.add(element);
    return arr;
}

// Example
ArrayList<Integer> arr = new ArrayList<>(Arrays.asList(1, 2, 3));
insertAtEnd(arr, 4);
// arr is now [1, 2, 3, 4]
```

##### Python
```python
def insert_at_end(arr, element):
    """
    Insert at end
    Time: O(1) amortized in Python
    Space: O(1)
    """
    arr.append(element)
    return arr

arr = [1, 2, 3]
insert_at_end(arr, 4)
print(arr)  # [1, 2, 3, 4]
```

#### At Beginning

##### C
```c
int* insert_at_beginning(int* arr, int* size, int* capacity, int element) {
    /*
     * Insert at beginning
     * Time: O(n) - all elements shift
     * Space: O(1)
     */
    if (*size >= *capacity) {
        *capacity *= 2;
        arr = (int*)realloc(arr, (*capacity) * sizeof(int));
    }
    for (int i = *size; i > 0; i--) {
        arr[i] = arr[i - 1];
    }
    arr[0] = element;
    (*size)++;
    return arr;
}
```

##### C++
```cpp
void insert_at_beginning(vector<int>& arr, int element) {
    /*
     * Insert at beginning
     * Time: O(n) - all elements shift
     * Space: O(1)
     */
    arr.insert(arr.begin(), element);
}

// Example
vector<int> arr = {2, 3, 4};
insert_at_beginning(arr, 1);
// arr is now {1, 2, 3, 4}
```

##### Java
```java
public static ArrayList<Integer> insertAtBeginning(ArrayList<Integer> arr, int element) {
    /*
     * Insert at beginning
     * Time: O(n) - all elements shift
     * Space: O(1)
     */
    arr.add(0, element);
    return arr;
}

// Example
ArrayList<Integer> arr = new ArrayList<>(Arrays.asList(2, 3, 4));
insertAtBeginning(arr, 1);
// arr is now [1, 2, 3, 4]
```

##### Python
```python
def insert_at_beginning(arr, element):
    """
    Insert at beginning
    Time: O(n) - all elements shift
    Space: O(1)
    """
    arr.insert(0, element)
    return arr

arr = [2, 3, 4]
insert_at_beginning(arr, 1)
print(arr)  # [1, 2, 3, 4]
```

#### At Position

##### C
```c
int* insert_at_position(int* arr, int* size, int* capacity, int index, int element) {
    /*
     * Insert at specific position
     * Time: O(n) - elements after index shift
     * Space: O(1)
     */
    if (index < 0 || index > *size) return arr;
    
    if (*size >= *capacity) {
        *capacity *= 2;
        arr = (int*)realloc(arr, (*capacity) * sizeof(int));
    }
    
    for (int i = *size; i > index; i--) {
        arr[i] = arr[i - 1];
    }
    arr[index] = element;
    (*size)++;
    return arr;
}
```

##### C++
```cpp
void insert_at_position(vector<int>& arr, int index, int element) {
    /*
     * Insert at specific position
     * Time: O(n) - elements after index shift
     * Space: O(1)
     */
    if (index >= 0 && index <= arr.size()) {
        arr.insert(arr.begin() + index, element);
    }
}

// Example
vector<int> arr = {1, 2, 4, 5};
insert_at_position(arr, 2, 3);
// arr is now {1, 2, 3, 4, 5}
```

##### Java
```java
public static ArrayList<Integer> insertAtPosition(ArrayList<Integer> arr, int index, int element) {
    /*
     * Insert at specific position
     * Time: O(n) - elements after index shift
     * Space: O(1)
     */
    if (index >= 0 && index <= arr.size()) {
        arr.add(index, element);
    }
    return arr;
}

// Example
ArrayList<Integer> arr = new ArrayList<>(Arrays.asList(1, 2, 4, 5));
insertAtPosition(arr, 2, 3);
// arr is now [1, 2, 3, 4, 5]
```

##### Python
```python
def insert_at_position(arr, index, element):
    """
    Insert at specific position
    Time: O(n) - elements after index shift
    Space: O(1)
    """
    if 0 <= index <= len(arr):
        arr.insert(index, element)
    return arr

arr = [1, 2, 4, 5]
insert_at_position(arr, 2, 3)
print(arr)  # [1, 2, 3, 4, 5]
```

### 3. Delete

#### From End
```python
def delete_from_end(arr):
    """
    Delete last element
    Time: O(1)
    Space: O(1)
    """
    if arr:
        return arr.pop()
    return None

arr = [1, 2, 3, 4]
deleted = delete_from_end(arr)
print(arr)  # [1, 2, 3]
print(deleted)  # 4
```

#### From Beginning
```python
def delete_from_beginning(arr):
    """
    Delete first element
    Time: O(n)
    Space: O(1)
    """
    if arr:
        return arr.pop(0)
    return None
```

#### From Position
```python
def delete_from_position(arr, index):
    """
    Delete element at index
    Time: O(n)
    Space: O(1)
    """
    if 0 <= index < len(arr):
        return arr.pop(index)
    return None
```

### 4. Search

#### Linear Search

##### C
```c
int linear_search(int arr[], int size, int target) {
    /*
     * Search for target element
     * Time: O(n)
     * Space: O(1)
     */
    for (int i = 0; i < size; i++) {
        if (arr[i] == target) {
            return i;
        }
    }
    return -1;
}

int main() {
    int arr[] = {10, 23, 45, 70, 11, 15};
    int size = sizeof(arr) / sizeof(arr[0]);
    printf("%d\n", linear_search(arr, size, 70));  // 3
    return 0;
}
```

##### C++
```cpp
#include <algorithm>

int linear_search(vector<int>& arr, int target) {
    /*
     * Search for target element
     * Time: O(n)
     * Space: O(1)
     */
    for (int i = 0; i < arr.size(); i++) {
        if (arr[i] == target) {
            return i;
        }
    }
    return -1;
}

// Alternative using STL
int linear_search_stl(vector<int>& arr, int target) {
    auto it = find(arr.begin(), arr.end(), target);
    return (it != arr.end()) ? distance(arr.begin(), it) : -1;
}

int main() {
    vector<int> arr = {10, 23, 45, 70, 11, 15};
    cout << linear_search(arr, 70) << endl;  // 3
    return 0;
}
```

##### Java
```java
public static int linearSearch(int[] arr, int target) {
    /*
     * Search for target element
     * Time: O(n)
     * Space: O(1)
     */
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == target) {
            return i;
        }
    }
    return -1;
}

public static void main(String[] args) {
    int[] arr = {10, 23, 45, 70, 11, 15};
    System.out.println(linearSearch(arr, 70));  // 3
}
```

##### Python
```python
def linear_search(arr, target):
    """
    Search for target element
    Time: O(n)
    Space: O(1)
    """
    for i in range(len(arr)):
        if arr[i] == target:
            return i
    return -1

arr = [10, 23, 45, 70, 11, 15]
print(linear_search(arr, 70))  # 3
```

#### Binary Search (Sorted Array)

##### C
```c
int binary_search(int arr[], int size, int target) {
    /*
     * Binary search on sorted array
     * Time: O(log n)
     * Space: O(1)
     */
    int left = 0, right = size - 1;
    
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

int main() {
    int arr[] = {2, 5, 8, 12, 16, 23, 38, 45};
    int size = sizeof(arr) / sizeof(arr[0]);
    printf("%d\n", binary_search(arr, size, 23));  // 5
    return 0;
}
```

##### C++
```cpp
#include <algorithm>

int binary_search(vector<int>& arr, int target) {
    /*
     * Binary search on sorted array
     * Time: O(log n)
     * Space: O(1)
     */
    int left = 0, right = arr.size() - 1;
    
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

// Alternative using STL
int binary_search_stl(vector<int>& arr, int target) {
    auto it = lower_bound(arr.begin(), arr.end(), target);
    if (it != arr.end() && *it == target) {
        return distance(arr.begin(), it);
    }
    return -1;
}

int main() {
    vector<int> arr = {2, 5, 8, 12, 16, 23, 38, 45};
    cout << binary_search(arr, 23) << endl;  // 5
    return 0;
}
```

##### Java
```java
public static int binarySearch(int[] arr, int target) {
    /*
     * Binary search on sorted array
     * Time: O(log n)
     * Space: O(1)
     */
    int left = 0, right = arr.length - 1;
    
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

// Alternative using Java's built-in method
public static int binarySearchBuiltin(int[] arr, int target) {
    return Arrays.binarySearch(arr, target);
}

public static void main(String[] args) {
    int[] arr = {2, 5, 8, 12, 16, 23, 38, 45};
    System.out.println(binarySearch(arr, 23));  // 5
}
```

##### Python
```python
def binary_search(arr, target):
    """
    Binary search on sorted array
    Time: O(log n)
    Space: O(1)
    """
    left, right = 0, len(arr) - 1
    
    while left <= right:
        mid = left + (right - left) // 2
        
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return -1

arr = [2, 5, 8, 12, 16, 23, 38, 45]
print(binary_search(arr, 23))  # 5
```

---

## Common Array Problems

### 1. Reverse Array

#### C
```c
void reverse_array(int arr[], int size) {
    /*
     * Reverse array in-place
     * Time: O(n)
     * Space: O(1)
     */
    int left = 0, right = size - 1;
    
    while (left < right) {
        int temp = arr[left];
        arr[left] = arr[right];
        arr[right] = temp;
        left++;
        right--;
    }
}

int main() {
    int arr[] = {1, 2, 3, 4, 5};
    int size = sizeof(arr) / sizeof(arr[0]);
    reverse_array(arr, size);
    // arr is now {5, 4, 3, 2, 1}
    return 0;
}
```

#### C++
```cpp
void reverse_array(vector<int>& arr) {
    /*
     * Reverse array in-place
     * Time: O(n)
     * Space: O(1)
     */
    int left = 0, right = arr.size() - 1;
    
    while (left < right) {
        swap(arr[left], arr[right]);
        left++;
        right--;
    }
}

// Alternative using STL
void reverse_array_stl(vector<int>& arr) {
    reverse(arr.begin(), arr.end());
}

int main() {
    vector<int> arr = {1, 2, 3, 4, 5};
    reverse_array(arr);
    // arr is now {5, 4, 3, 2, 1}
    return 0;
}
```

#### Java
```java
import java.util.ArrayList;
import java.util.Collections;

public static void reverseArray(int[] arr) {
    /*
     * Reverse array in-place
     * Time: O(n)
     * Space: O(1)
     */
    int left = 0, right = arr.length - 1;
    
    while (left < right) {
        int temp = arr[left];
        arr[left] = arr[right];
        arr[right] = temp;
        left++;
        right--;
    }
}

// For ArrayList
public static void reverseArrayList(ArrayList<Integer> arr) {
    Collections.reverse(arr);
}

public static void main(String[] args) {
    int[] arr = {1, 2, 3, 4, 5};
    reverseArray(arr);
    // arr is now {5, 4, 3, 2, 1}
}
```

#### Python
```python
def reverse_array(arr):
    """
    Reverse array in-place
    Time: O(n)
    Space: O(1)
    """
    left, right = 0, len(arr) - 1
    
    while left < right:
        arr[left], arr[right] = arr[right], arr[left]
        left += 1
        right -= 1
    
    return arr

arr = [1, 2, 3, 4, 5]
print(reverse_array(arr))  # [5, 4, 3, 2, 1]
```

### 2. Rotate Array
```python
def rotate_array(arr, k):
    """
    Rotate array by k positions to the right
    Time: O(n)
    Space: O(1)
    """
    n = len(arr)
    k = k % n  # Handle k > n
    
    # Reverse entire array
    reverse_array(arr)
    # Reverse first k elements
    arr[:k] = arr[:k][::-1]
    # Reverse remaining elements
    arr[k:] = arr[k:][::-1]
    
    return arr

arr = [1, 2, 3, 4, 5, 6, 7]
print(rotate_array(arr, 3))  # [5, 6, 7, 1, 2, 3, 4]
```

### 3. Find Maximum and Minimum
```python
def find_max_min(arr):
    """
    Find maximum and minimum in array
    Time: O(n)
    Space: O(1)
    """
    if not arr:
        return None, None
    
    max_val = arr[0]
    min_val = arr[0]
    
    for num in arr[1:]:
        if num > max_val:
            max_val = num
        if num < min_val:
            min_val = num
    
    return max_val, min_val

arr = [12, 35, 1, 10, 34, 1]
max_val, min_val = find_max_min(arr)
print(f"Max: {max_val}, Min: {min_val}")  # Max: 35, Min: 1
```

### 4. Remove Duplicates from Sorted Array
```python
def remove_duplicates_sorted(arr):
    """
    Remove duplicates from sorted array
    Time: O(n)
    Space: O(1)
    """
    if not arr:
        return 0
    
    write_index = 1
    
    for i in range(1, len(arr)):
        if arr[i] != arr[i-1]:
            arr[write_index] = arr[i]
            write_index += 1
    
    return write_index  # New length

arr = [1, 1, 2, 2, 2, 3, 4, 4, 5]
new_length = remove_duplicates_sorted(arr)
print(arr[:new_length])  # [1, 2, 3, 4, 5]
```

### 5. Merge Two Sorted Arrays
```python
def merge_sorted_arrays(arr1, arr2):
    """
    Merge two sorted arrays
    Time: O(m + n)
    Space: O(m + n)
    """
    result = []
    i, j = 0, 0
    
    while i < len(arr1) and j < len(arr2):
        if arr1[i] <= arr2[j]:
            result.append(arr1[i])
            i += 1
        else:
            result.append(arr2[j])
            j += 1
    
    result.extend(arr1[i:])
    result.extend(arr2[j:])
    
    return result

arr1 = [1, 3, 5, 7]
arr2 = [2, 4, 6, 8]
print(merge_sorted_arrays(arr1, arr2))  # [1, 2, 3, 4, 5, 6, 7, 8]
```

### 6. Find Missing Number
```python
def find_missing_number(arr, n):
    """
    Find missing number in array 1 to n
    Time: O(n)
    Space: O(1)
    """
    # Sum formula: n * (n + 1) / 2
    expected_sum = n * (n + 1) // 2
    actual_sum = sum(arr)
    return expected_sum - actual_sum

arr = [1, 2, 4, 5, 6]  # Missing 3
print(find_missing_number(arr, 6))  # 3
```

### 7. Two Sum Problem
```python
def two_sum(arr, target):
    """
    Find two numbers that add up to target
    Time: O(n)
    Space: O(n)
    """
    seen = {}
    
    for i, num in enumerate(arr):
        complement = target - num
        if complement in seen:
            return [seen[complement], i]
        seen[num] = i
    
    return []

arr = [2, 7, 11, 15]
target = 9
print(two_sum(arr, target))  # [0, 1]
```

### 8. Maximum Subarray Sum (Kadane's Algorithm)
```python
def max_subarray_sum(arr):
    """
    Find maximum sum of contiguous subarray
    Time: O(n)
    Space: O(1)
    """
    max_sum = arr[0]
    current_sum = arr[0]
    
    for i in range(1, len(arr)):
        current_sum = max(arr[i], current_sum + arr[i])
        max_sum = max(max_sum, current_sum)
    
    return max_sum

arr = [-2, 1, -3, 4, -1, 2, 1, -5, 4]
print(max_subarray_sum(arr))  # 6 (subarray: [4, -1, 2, 1])
```

### 9. Move Zeros to End
```python
def move_zeros(arr):
    """
    Move all zeros to end while maintaining order
    Time: O(n)
    Space: O(1)
    """
    write_index = 0
    
    # Move non-zeros forward
    for i in range(len(arr)):
        if arr[i] != 0:
            arr[write_index] = arr[i]
            write_index += 1
    
    # Fill rest with zeros
    while write_index < len(arr):
        arr[write_index] = 0
        write_index += 1
    
    return arr

arr = [0, 1, 0, 3, 12]
print(move_zeros(arr))  # [1, 3, 12, 0, 0]
```

### 10. Find Majority Element
```python
def majority_element(arr):
    """
    Find element appearing more than n/2 times
    Boyer-Moore Voting Algorithm
    Time: O(n)
    Space: O(1)
    """
    candidate = None
    count = 0
    
    # Find candidate
    for num in arr:
        if count == 0:
            candidate = num
        count += 1 if num == candidate else -1
    
    # Verify candidate
    if arr.count(candidate) > len(arr) // 2:
        return candidate
    
    return None

arr = [2, 2, 1, 1, 1, 2, 2]
print(majority_element(arr))  # 2
```

---

## Multi-dimensional Arrays

### 2D Array Operations

#### Declaration
```python
# Create 3x3 matrix
matrix = [[1, 2, 3],
          [4, 5, 6],
          [7, 8, 9]]

# Using list comprehension
matrix = [[0] * 3 for _ in range(3)]
```

#### Traverse
```python
def traverse_2d(matrix):
    """
    Traverse 2D array
    Time: O(rows * cols)
    """
    rows = len(matrix)
    cols = len(matrix[0]) if matrix else 0
    
    # Row-wise traversal
    for i in range(rows):
        for j in range(cols):
            print(matrix[i][j], end=' ')
        print()
    
    # Column-wise traversal
    for j in range(cols):
        for i in range(rows):
            print(matrix[i][j], end=' ')
        print()
```

#### Matrix Transpose
```python
def transpose(matrix):
    """
    Transpose matrix
    Time: O(n²) for n×n matrix
    Space: O(n²)
    """
    rows = len(matrix)
    cols = len(matrix[0])
    
    transposed = [[matrix[i][j] for i in range(rows)] 
                  for j in range(cols)]
    
    return transposed

matrix = [[1, 2, 3],
          [4, 5, 6]]
print(transpose(matrix))
# [[1, 4],
#  [2, 5],
#  [3, 6]]
```

#### Matrix Rotation
```python
def rotate_matrix_90(matrix):
    """
    Rotate matrix 90 degrees clockwise
    Time: O(n²)
    Space: O(1) in-place
    """
    n = len(matrix)
    
    # Transpose
    for i in range(n):
        for j in range(i, n):
            matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
    
    # Reverse each row
    for i in range(n):
        matrix[i].reverse()
    
    return matrix

matrix = [[1, 2, 3],
          [4, 5, 6],
          [7, 8, 9]]
print(rotate_matrix_90(matrix))
# [[7, 4, 1],
#  [8, 5, 2],
#  [9, 6, 3]]
```

---

## Time Complexity Summary

| Operation | Average | Worst |
|-----------|---------|-------|
| Access | O(1) | O(1) |
| Search | O(n) | O(n) |
| Insert at end | O(1) | O(n) |
| Insert at beginning | O(n) | O(n) |
| Delete from end | O(1) | O(1) |
| Delete from beginning | O(n) | O(n) |

---

## Exam Tips

### 1. Common Mistakes
- Array index out of bounds
- Not handling empty arrays
- Modifying array while iterating
- Forgetting 0-based indexing

### 2. Debugging Questions
```python
# Bug: Index out of range
for i in range(len(arr) + 1):  # Wrong
    print(arr[i])

# Fix
for i in range(len(arr)):  # Correct
    print(arr[i])
```

### 3. Dry Run Practice
```
Array: [5, 2, 8, 1, 9]
Reverse:
Step 1: Swap arr[0] and arr[4]: [9, 2, 8, 1, 5]
Step 2: Swap arr[1] and arr[3]: [9, 1, 8, 2, 5]
Result: [9, 1, 8, 2, 5]
```

### 4. MCQ Tips
- **Array size**: Fixed after creation (in most languages)
- **Memory**: Contiguous allocation
- **Access time**: O(1) due to index calculation
- **Formula**: address = base + (index × element_size)

### Key Points
1. **Random access**: O(1) using index
2. **Fixed size**: Cannot grow/shrink easily
3. **Contiguous memory**: Fast access, cache-friendly
4. **0-based indexing**: First element at index 0
5. **Efficient for**: Random access, iteration
6. **Inefficient for**: Insertion/deletion in middle
