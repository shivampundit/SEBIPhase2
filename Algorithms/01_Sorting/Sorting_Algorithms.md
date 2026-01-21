# Sorting Algorithms - Comprehensive Notes

## Overview
Sorting is the process of arranging elements in a specific order (ascending or descending). Understanding sorting algorithms is crucial for:
- Data organization
- Search optimization
- Database operations
- Algorithm complexity analysis

---

## 1. Bubble Sort

### Concept
Bubble Sort repeatedly compares adjacent elements and swaps them if they're in wrong order. The largest element "bubbles up" to the end in each pass.

### Algorithm
```
for i = 0 to n-1:
    for j = 0 to n-i-2:
        if arr[j] > arr[j+1]:
            swap(arr[j], arr[j+1])
```

### Implementation (Python)
```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        swapped = False
        for j in range(0, n-i-1):
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]
                swapped = True
        if not swapped:  # Optimization: stop if no swaps
            break
    return arr

# Example
arr = [64, 34, 25, 12, 22, 11, 90]
print(bubble_sort(arr))  # Output: [11, 12, 22, 25, 34, 64, 90]
```

### Implementation (C)
```c
#include <stdio.h>
#include <stdbool.h>

void bubble_sort(int arr[], int n) {
    for (int i = 0; i < n; i++) {
        bool swapped = false;
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                // Swap arr[j] and arr[j+1]
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
                swapped = true;
            }
        }
        if (!swapped)  // Optimization: stop if no swaps
            break;
    }
}

// Example
int main() {
    int arr[] = {64, 34, 25, 12, 22, 11, 90};
    int n = sizeof(arr) / sizeof(arr[0]);
    bubble_sort(arr, n);
    
    printf("Sorted array: ");
    for (int i = 0; i < n; i++)
        printf("%d ", arr[i]);
    // Output: 11 12 22 25 34 64 90
    return 0;
}
```

### Implementation (C++)
```cpp
#include <iostream>
#include <vector>
using namespace std;

void bubble_sort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n; i++) {
        bool swapped = false;
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                // Swap arr[j] and arr[j+1]
                swap(arr[j], arr[j + 1]);
                swapped = true;
            }
        }
        if (!swapped)  // Optimization: stop if no swaps
            break;
    }
}

// Example
int main() {
    vector<int> arr = {64, 34, 25, 12, 22, 11, 90};
    bubble_sort(arr);
    
    cout << "Sorted array: ";
    for (int num : arr)
        cout << num << " ";
    // Output: 11 12 22 25 34 64 90
    return 0;
}
```

### Implementation (Java)
```java
public class BubbleSort {
    public static void bubbleSort(int[] arr) {
        int n = arr.length;
        for (int i = 0; i < n; i++) {
            boolean swapped = false;
            for (int j = 0; j < n - i - 1; j++) {
                if (arr[j] > arr[j + 1]) {
                    // Swap arr[j] and arr[j+1]
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                    swapped = true;
                }
            }
            if (!swapped)  // Optimization: stop if no swaps
                break;
        }
    }
    
    // Example
    public static void main(String[] args) {
        int[] arr = {64, 34, 25, 12, 22, 11, 90};
        bubbleSort(arr);
        
        System.out.print("Sorted array: ");
        for (int num : arr)
            System.out.print(num + " ");
        // Output: 11 12 22 25 34 64 90
    }
}
```

### Dry Run Example
```
Initial: [5, 3, 8, 4, 2]

Pass 1:
[5, 3, 8, 4, 2] → [3, 5, 8, 4, 2] (swap 5 and 3)
[3, 5, 8, 4, 2] → [3, 5, 8, 4, 2] (no swap)
[3, 5, 8, 4, 2] → [3, 5, 4, 8, 2] (swap 8 and 4)
[3, 5, 4, 8, 2] → [3, 5, 4, 2, 8] (swap 8 and 2)

Pass 2:
[3, 5, 4, 2, 8] → [3, 5, 4, 2, 8]
[3, 5, 4, 2, 8] → [3, 4, 5, 2, 8] (swap 5 and 4)
[3, 4, 5, 2, 8] → [3, 4, 2, 5, 8] (swap 5 and 2)

Pass 3:
[3, 4, 2, 5, 8] → [3, 4, 2, 5, 8]
[3, 4, 2, 5, 8] → [3, 2, 4, 5, 8] (swap 4 and 2)

Pass 4:
[3, 2, 4, 5, 8] → [2, 3, 4, 5, 8] (swap 3 and 2)

Final: [2, 3, 4, 5, 8]
```

### Complexity Analysis
- **Time Complexity**: 
  - Best Case: O(n) - when array is already sorted
  - Average Case: O(n²)
  - Worst Case: O(n²)
- **Space Complexity**: O(1) - in-place sorting
- **Stable**: Yes (maintains relative order of equal elements)

### When to Use
- Small datasets
- Nearly sorted arrays
- When simplicity is preferred over efficiency

---

## 2. Selection Sort

### Concept
Selection Sort divides the array into sorted and unsorted parts. It repeatedly selects the minimum element from unsorted part and places it at the beginning.

### Algorithm
```
for i = 0 to n-1:
    min_index = i
    for j = i+1 to n:
        if arr[j] < arr[min_index]:
            min_index = j
    swap(arr[i], arr[min_index])
```

### Implementation (Python)
```python
def selection_sort(arr):
    n = len(arr)
    for i in range(n):
        min_idx = i
        for j in range(i+1, n):
            if arr[j] < arr[min_idx]:
                min_idx = j
        arr[i], arr[min_idx] = arr[min_idx], arr[i]
    return arr

# Example
arr = [64, 25, 12, 22, 11]
print(selection_sort(arr))  # Output: [11, 12, 22, 25, 64]
```

### Implementation (C)
```c
#include <stdio.h>

void selection_sort(int arr[], int n) {
    for (int i = 0; i < n; i++) {
        int min_idx = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[min_idx]) {
                min_idx = j;
            }
        }
        // Swap arr[i] and arr[min_idx]
        int temp = arr[i];
        arr[i] = arr[min_idx];
        arr[min_idx] = temp;
    }
}

// Example
int main() {
    int arr[] = {64, 25, 12, 22, 11};
    int n = sizeof(arr) / sizeof(arr[0]);
    selection_sort(arr, n);
    
    printf("Sorted array: ");
    for (int i = 0; i < n; i++)
        printf("%d ", arr[i]);
    // Output: 11 12 22 25 64
    return 0;
}
```

### Implementation (C++)
```cpp
#include <iostream>
#include <vector>
using namespace std;

void selection_sort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n; i++) {
        int min_idx = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[min_idx]) {
                min_idx = j;
            }
        }
        // Swap arr[i] and arr[min_idx]
        swap(arr[i], arr[min_idx]);
    }
}

// Example
int main() {
    vector<int> arr = {64, 25, 12, 22, 11};
    selection_sort(arr);
    
    cout << "Sorted array: ";
    for (int num : arr)
        cout << num << " ";
    // Output: 11 12 22 25 64
    return 0;
}
```

### Implementation (Java)
```java
public class SelectionSort {
    public static void selectionSort(int[] arr) {
        int n = arr.length;
        for (int i = 0; i < n; i++) {
            int min_idx = i;
            for (int j = i + 1; j < n; j++) {
                if (arr[j] < arr[min_idx]) {
                    min_idx = j;
                }
            }
            // Swap arr[i] and arr[min_idx]
            int temp = arr[i];
            arr[i] = arr[min_idx];
            arr[min_idx] = temp;
        }
    }
    
    // Example
    public static void main(String[] args) {
        int[] arr = {64, 25, 12, 22, 11};
        selectionSort(arr);
        
        System.out.print("Sorted array: ");
        for (int num : arr)
            System.out.print(num + " ");
        // Output: 11 12 22 25 64
    }
}
```

### Dry Run Example
```
Initial: [29, 10, 14, 37, 13]

Pass 1: Find min in [29, 10, 14, 37, 13] → 10
[10, 29, 14, 37, 13] (swap 29 and 10)

Pass 2: Find min in [29, 14, 37, 13] → 13
[10, 13, 14, 37, 29] (swap 29 and 13)

Pass 3: Find min in [14, 37, 29] → 14
[10, 13, 14, 37, 29] (no swap needed)

Pass 4: Find min in [37, 29] → 29
[10, 13, 14, 29, 37] (swap 37 and 29)

Final: [10, 13, 14, 29, 37]
```

### Complexity Analysis
- **Time Complexity**: O(n²) in all cases
- **Space Complexity**: O(1)
- **Stable**: No (can be made stable with modifications)

### Key Points
- Never makes more than O(n) swaps
- Useful when memory write is costly

---

## 3. Insertion Sort

### Concept
Insertion Sort builds the sorted array one item at a time by inserting each element into its correct position.

### Algorithm
```
for i = 1 to n-1:
    key = arr[i]
    j = i - 1
    while j >= 0 and arr[j] > key:
        arr[j+1] = arr[j]
        j = j - 1
    arr[j+1] = key
```

### Implementation (Python)
```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1
        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = key
    return arr

# Example
arr = [12, 11, 13, 5, 6]
print(insertion_sort(arr))  # Output: [5, 6, 11, 12, 13]
```

### Implementation (C)
```c
#include <stdio.h>

void insertion_sort(int arr[], int n) {
    for (int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;
        
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
    }
}

// Example
int main() {
    int arr[] = {12, 11, 13, 5, 6};
    int n = sizeof(arr) / sizeof(arr[0]);
    insertion_sort(arr, n);
    
    printf("Sorted array: ");
    for (int i = 0; i < n; i++)
        printf("%d ", arr[i]);
    // Output: 5 6 11 12 13
    return 0;
}
```

### Implementation (C++)
```cpp
#include <iostream>
#include <vector>
using namespace std;

void insertion_sort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;
        
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
    }
}

// Example
int main() {
    vector<int> arr = {12, 11, 13, 5, 6};
    insertion_sort(arr);
    
    cout << "Sorted array: ";
    for (int num : arr)
        cout << num << " ";
    // Output: 5 6 11 12 13
    return 0;
}
```

### Implementation (Java)
```java
public class InsertionSort {
    public static void insertionSort(int[] arr) {
        int n = arr.length;
        for (int i = 1; i < n; i++) {
            int key = arr[i];
            int j = i - 1;
            
            while (j >= 0 && arr[j] > key) {
                arr[j + 1] = arr[j];
                j--;
            }
            arr[j + 1] = key;
        }
    }
    
    // Example
    public static void main(String[] args) {
        int[] arr = {12, 11, 13, 5, 6};
        insertionSort(arr);
        
        System.out.print("Sorted array: ");
        for (int num : arr)
            System.out.print(num + " ");
        // Output: 5 6 11 12 13
    }
}
```

### Dry Run Example
```
Initial: [5, 2, 4, 6, 1]

Step 1: i=1, key=2
[5, 2, 4, 6, 1] → [2, 5, 4, 6, 1]

Step 2: i=2, key=4
[2, 5, 4, 6, 1] → [2, 4, 5, 6, 1]

Step 3: i=3, key=6
[2, 4, 5, 6, 1] → [2, 4, 5, 6, 1] (no change)

Step 4: i=4, key=1
[2, 4, 5, 6, 1] → [1, 2, 4, 5, 6]

Final: [1, 2, 4, 5, 6]
```

### Complexity Analysis
- **Time Complexity**:
  - Best Case: O(n) - array already sorted
  - Average Case: O(n²)
  - Worst Case: O(n²)
- **Space Complexity**: O(1)
- **Stable**: Yes

### When to Use
- Small datasets
- Nearly sorted arrays
- Online sorting (can sort as it receives data)

---

## 4. Merge Sort

### Concept
Merge Sort uses Divide and Conquer strategy. It divides array into halves, recursively sorts them, and merges the sorted halves.

### Algorithm
```
merge_sort(arr, left, right):
    if left < right:
        mid = (left + right) / 2
        merge_sort(arr, left, mid)
        merge_sort(arr, mid+1, right)
        merge(arr, left, mid, right)
```

### Implementation (Python)
```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    
    return merge(left, right)

def merge(left, right):
    result = []
    i = j = 0
    
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    
    result.extend(left[i:])
    result.extend(right[j:])
    return result

# Example
arr = [38, 27, 43, 3, 9, 82, 10]
print(merge_sort(arr))  # Output: [3, 9, 10, 27, 38, 43, 82]
```

### Implementation (C)
```c
#include <stdio.h>
#include <stdlib.h>

void merge(int arr[], int left, int mid, int right) {
    int n1 = mid - left + 1;
    int n2 = right - mid;
    
    // Create temporary arrays
    int* L = (int*)malloc(n1 * sizeof(int));
    int* R = (int*)malloc(n2 * sizeof(int));
    
    // Copy data to temporary arrays
    for (int i = 0; i < n1; i++)
        L[i] = arr[left + i];
    for (int j = 0; j < n2; j++)
        R[j] = arr[mid + 1 + j];
    
    // Merge the temporary arrays back
    int i = 0, j = 0, k = left;
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
            arr[k++] = L[i++];
        } else {
            arr[k++] = R[j++];
        }
    }
    
    // Copy remaining elements
    while (i < n1)
        arr[k++] = L[i++];
    while (j < n2)
        arr[k++] = R[j++];
    
    free(L);
    free(R);
}

void merge_sort(int arr[], int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        merge_sort(arr, left, mid);
        merge_sort(arr, mid + 1, right);
        merge(arr, left, mid, right);
    }
}

// Example
int main() {
    int arr[] = {38, 27, 43, 3, 9, 82, 10};
    int n = sizeof(arr) / sizeof(arr[0]);
    merge_sort(arr, 0, n - 1);
    
    printf("Sorted array: ");
    for (int i = 0; i < n; i++)
        printf("%d ", arr[i]);
    // Output: 3 9 10 27 38 43 82
    return 0;
}
```

### Implementation (C++)
```cpp
#include <iostream>
#include <vector>
using namespace std;

void merge(vector<int>& arr, int left, int mid, int right) {
    int n1 = mid - left + 1;
    int n2 = right - mid;
    
    // Create temporary vectors
    vector<int> L(n1), R(n2);
    
    // Copy data to temporary vectors
    for (int i = 0; i < n1; i++)
        L[i] = arr[left + i];
    for (int j = 0; j < n2; j++)
        R[j] = arr[mid + 1 + j];
    
    // Merge the temporary vectors back
    int i = 0, j = 0, k = left;
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
            arr[k++] = L[i++];
        } else {
            arr[k++] = R[j++];
        }
    }
    
    // Copy remaining elements
    while (i < n1)
        arr[k++] = L[i++];
    while (j < n2)
        arr[k++] = R[j++];
}

void merge_sort(vector<int>& arr, int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        merge_sort(arr, left, mid);
        merge_sort(arr, mid + 1, right);
        merge(arr, left, mid, right);
    }
}

// Example
int main() {
    vector<int> arr = {38, 27, 43, 3, 9, 82, 10};
    merge_sort(arr, 0, arr.size() - 1);
    
    cout << "Sorted array: ";
    for (int num : arr)
        cout << num << " ";
    // Output: 3 9 10 27 38 43 82
    return 0;
}
```

### Implementation (Java)
```java
public class MergeSort {
    public static void merge(int[] arr, int left, int mid, int right) {
        int n1 = mid - left + 1;
        int n2 = right - mid;
        
        // Create temporary arrays
        int[] L = new int[n1];
        int[] R = new int[n2];
        
        // Copy data to temporary arrays
        for (int i = 0; i < n1; i++)
            L[i] = arr[left + i];
        for (int j = 0; j < n2; j++)
            R[j] = arr[mid + 1 + j];
        
        // Merge the temporary arrays back
        int i = 0, j = 0, k = left;
        while (i < n1 && j < n2) {
            if (L[i] <= R[j]) {
                arr[k++] = L[i++];
            } else {
                arr[k++] = R[j++];
            }
        }
        
        // Copy remaining elements
        while (i < n1)
            arr[k++] = L[i++];
        while (j < n2)
            arr[k++] = R[j++];
    }
    
    public static void mergeSort(int[] arr, int left, int right) {
        if (left < right) {
            int mid = left + (right - left) / 2;
            mergeSort(arr, left, mid);
            mergeSort(arr, mid + 1, right);
            merge(arr, left, mid, right);
        }
    }
    
    // Example
    public static void main(String[] args) {
        int[] arr = {38, 27, 43, 3, 9, 82, 10};
        mergeSort(arr, 0, arr.length - 1);
        
        System.out.print("Sorted array: ");
        for (int num : arr)
            System.out.print(num + " ");
        // Output: 3 9 10 27 38 43 82
    }
}
```

### Dry Run Example
```
Initial: [38, 27, 43, 3]

Split Phase:
[38, 27, 43, 3]
  ↓
[38, 27] | [43, 3]
  ↓          ↓
[38] [27] | [43] [3]

Merge Phase:
[38] [27] → [27, 38]
[43] [3]  → [3, 43]
[27, 38] [3, 43] → [3, 27, 38, 43]

Final: [3, 27, 38, 43]
```

### Complexity Analysis
- **Time Complexity**: O(n log n) in all cases
- **Space Complexity**: O(n) - requires extra space
- **Stable**: Yes

### When to Use
- Large datasets
- When stable sort is needed
- Linked lists (O(1) space complexity for linked lists)
- External sorting

---

## 5. Quick Sort

### Concept
Quick Sort uses Divide and Conquer. It picks a pivot element and partitions the array around it, then recursively sorts subarrays.

### Algorithm
```
quick_sort(arr, low, high):
    if low < high:
        pivot = partition(arr, low, high)
        quick_sort(arr, low, pivot-1)
        quick_sort(arr, pivot+1, high)

partition(arr, low, high):
    pivot = arr[high]
    i = low - 1
    for j = low to high-1:
        if arr[j] < pivot:
            i++
            swap(arr[i], arr[j])
    swap(arr[i+1], arr[high])
    return i+1
```

### Implementation (Python)
```python
def quick_sort(arr, low, high):
    if low < high:
        pivot_index = partition(arr, low, high)
        quick_sort(arr, low, pivot_index - 1)
        quick_sort(arr, pivot_index + 1, high)
    return arr

def partition(arr, low, high):
    pivot = arr[high]
    i = low - 1
    
    for j in range(low, high):
        if arr[j] < pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    
    arr[i + 1], arr[high] = arr[high], arr[i + 1]
    return i + 1

# Example
arr = [10, 7, 8, 9, 1, 5]
print(quick_sort(arr, 0, len(arr)-1))  # Output: [1, 5, 7, 8, 9, 10]
```

### Implementation (C)
```c
#include <stdio.h>

int partition(int arr[], int low, int high) {
    int pivot = arr[high];
    int i = low - 1;
    
    for (int j = low; j < high; j++) {
        if (arr[j] < pivot) {
            i++;
            // Swap arr[i] and arr[j]
            int temp = arr[i];
            arr[i] = arr[j];
            arr[j] = temp;
        }
    }
    
    // Swap arr[i+1] and arr[high]
    int temp = arr[i + 1];
    arr[i + 1] = arr[high];
    arr[high] = temp;
    return i + 1;
}

void quick_sort(int arr[], int low, int high) {
    if (low < high) {
        int pivot_index = partition(arr, low, high);
        quick_sort(arr, low, pivot_index - 1);
        quick_sort(arr, pivot_index + 1, high);
    }
}

// Example
int main() {
    int arr[] = {10, 7, 8, 9, 1, 5};
    int n = sizeof(arr) / sizeof(arr[0]);
    quick_sort(arr, 0, n - 1);
    
    printf("Sorted array: ");
    for (int i = 0; i < n; i++)
        printf("%d ", arr[i]);
    // Output: 1 5 7 8 9 10
    return 0;
}
```

### Implementation (C++)
```cpp
#include <iostream>
#include <vector>
using namespace std;

int partition(vector<int>& arr, int low, int high) {
    int pivot = arr[high];
    int i = low - 1;
    
    for (int j = low; j < high; j++) {
        if (arr[j] < pivot) {
            i++;
            swap(arr[i], arr[j]);
        }
    }
    
    swap(arr[i + 1], arr[high]);
    return i + 1;
}

void quick_sort(vector<int>& arr, int low, int high) {
    if (low < high) {
        int pivot_index = partition(arr, low, high);
        quick_sort(arr, low, pivot_index - 1);
        quick_sort(arr, pivot_index + 1, high);
    }
}

// Example
int main() {
    vector<int> arr = {10, 7, 8, 9, 1, 5};
    quick_sort(arr, 0, arr.size() - 1);
    
    cout << "Sorted array: ";
    for (int num : arr)
        cout << num << " ";
    // Output: 1 5 7 8 9 10
    return 0;
}
```

### Implementation (Java)
```java
public class QuickSort {
    public static int partition(int[] arr, int low, int high) {
        int pivot = arr[high];
        int i = low - 1;
        
        for (int j = low; j < high; j++) {
            if (arr[j] < pivot) {
                i++;
                // Swap arr[i] and arr[j]
                int temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }
        }
        
        // Swap arr[i+1] and arr[high]
        int temp = arr[i + 1];
        arr[i + 1] = arr[high];
        arr[high] = temp;
        return i + 1;
    }
    
    public static void quickSort(int[] arr, int low, int high) {
        if (low < high) {
            int pivot_index = partition(arr, low, high);
            quickSort(arr, low, pivot_index - 1);
            quickSort(arr, pivot_index + 1, high);
        }
    }
    
    // Example
    public static void main(String[] args) {
        int[] arr = {10, 7, 8, 9, 1, 5};
        quickSort(arr, 0, arr.length - 1);
        
        System.out.print("Sorted array: ");
        for (int num : arr)
            System.out.print(num + " ");
        // Output: 1 5 7 8 9 10
    }
}
```

### Dry Run Example
```
Initial: [10, 80, 30, 90, 40, 50, 70]
Pivot = 70 (last element)

Partitioning:
i = -1
j = 0: 10 < 70, i=0, swap arr[0] with arr[0] → [10, 80, 30, 90, 40, 50, 70]
j = 1: 80 > 70, no swap
j = 2: 30 < 70, i=1, swap arr[1] with arr[2] → [10, 30, 80, 90, 40, 50, 70]
j = 3: 90 > 70, no swap
j = 4: 40 < 70, i=2, swap arr[2] with arr[4] → [10, 30, 40, 90, 80, 50, 70]
j = 5: 50 < 70, i=3, swap arr[3] with arr[5] → [10, 30, 40, 50, 80, 90, 70]

Place pivot: swap arr[4] with arr[6] → [10, 30, 40, 50, 70, 90, 80]
Pivot index = 4

Recursively sort: [10, 30, 40, 50] and [90, 80]
Final: [10, 30, 40, 50, 70, 80, 90]
```

### Complexity Analysis
- **Time Complexity**:
  - Best Case: O(n log n)
  - Average Case: O(n log n)
  - Worst Case: O(n²) - when pivot is always smallest/largest
- **Space Complexity**: O(log n) - recursion stack
- **Stable**: No

### Pivot Selection Strategies
1. **Last element** (shown above)
2. **First element**
3. **Random element** (avoids worst case)
4. **Median of three** (first, middle, last)

### When to Use
- General-purpose sorting
- When average case performance matters
- In-place sorting required

---

## 6. Heap Sort

### Concept
Heap Sort uses a Binary Heap data structure. It builds a max heap, then repeatedly extracts the maximum element.

### Algorithm
```
heap_sort(arr):
    # Build max heap
    for i = n/2 - 1 down to 0:
        heapify(arr, n, i)
    
    # Extract elements from heap
    for i = n-1 down to 1:
        swap(arr[0], arr[i])
        heapify(arr, i, 0)
```

### Implementation (Python)
```python
def heap_sort(arr):
    n = len(arr)
    
    # Build max heap
    for i in range(n // 2 - 1, -1, -1):
        heapify(arr, n, i)
    
    # Extract elements one by one
    for i in range(n - 1, 0, -1):
        arr[0], arr[i] = arr[i], arr[0]
        heapify(arr, i, 0)
    
    return arr

def heapify(arr, n, i):
    largest = i
    left = 2 * i + 1
    right = 2 * i + 2
    
    if left < n and arr[left] > arr[largest]:
        largest = left
    
    if right < n and arr[right] > arr[largest]:
        largest = right
    
    if largest != i:
        arr[i], arr[largest] = arr[largest], arr[i]
        heapify(arr, n, largest)

# Example
arr = [12, 11, 13, 5, 6, 7]
print(heap_sort(arr))  # Output: [5, 6, 7, 11, 12, 13]
```

### Implementation (C)
```c
#include <stdio.h>

void heapify(int arr[], int n, int i) {
    int largest = i;
    int left = 2 * i + 1;
    int right = 2 * i + 2;
    
    if (left < n && arr[left] > arr[largest])
        largest = left;
    
    if (right < n && arr[right] > arr[largest])
        largest = right;
    
    if (largest != i) {
        // Swap arr[i] and arr[largest]
        int temp = arr[i];
        arr[i] = arr[largest];
        arr[largest] = temp;
        heapify(arr, n, largest);
    }
}

void heap_sort(int arr[], int n) {
    // Build max heap
    for (int i = n / 2 - 1; i >= 0; i--)
        heapify(arr, n, i);
    
    // Extract elements one by one
    for (int i = n - 1; i > 0; i--) {
        // Swap arr[0] and arr[i]
        int temp = arr[0];
        arr[0] = arr[i];
        arr[i] = temp;
        heapify(arr, i, 0);
    }
}

// Example
int main() {
    int arr[] = {12, 11, 13, 5, 6, 7};
    int n = sizeof(arr) / sizeof(arr[0]);
    heap_sort(arr, n);
    
    printf("Sorted array: ");
    for (int i = 0; i < n; i++)
        printf("%d ", arr[i]);
    // Output: 5 6 7 11 12 13
    return 0;
}
```

### Implementation (C++)
```cpp
#include <iostream>
#include <vector>
using namespace std;

void heapify(vector<int>& arr, int n, int i) {
    int largest = i;
    int left = 2 * i + 1;
    int right = 2 * i + 2;
    
    if (left < n && arr[left] > arr[largest])
        largest = left;
    
    if (right < n && arr[right] > arr[largest])
        largest = right;
    
    if (largest != i) {
        swap(arr[i], arr[largest]);
        heapify(arr, n, largest);
    }
}

void heap_sort(vector<int>& arr) {
    int n = arr.size();
    
    // Build max heap
    for (int i = n / 2 - 1; i >= 0; i--)
        heapify(arr, n, i);
    
    // Extract elements one by one
    for (int i = n - 1; i > 0; i--) {
        swap(arr[0], arr[i]);
        heapify(arr, i, 0);
    }
}

// Example
int main() {
    vector<int> arr = {12, 11, 13, 5, 6, 7};
    heap_sort(arr);
    
    cout << "Sorted array: ";
    for (int num : arr)
        cout << num << " ";
    // Output: 5 6 7 11 12 13
    return 0;
}
```

### Implementation (Java)
```java
public class HeapSort {
    public static void heapify(int[] arr, int n, int i) {
        int largest = i;
        int left = 2 * i + 1;
        int right = 2 * i + 2;
        
        if (left < n && arr[left] > arr[largest])
            largest = left;
        
        if (right < n && arr[right] > arr[largest])
            largest = right;
        
        if (largest != i) {
            // Swap arr[i] and arr[largest]
            int temp = arr[i];
            arr[i] = arr[largest];
            arr[largest] = temp;
            heapify(arr, n, largest);
        }
    }
    
    public static void heapSort(int[] arr) {
        int n = arr.length;
        
        // Build max heap
        for (int i = n / 2 - 1; i >= 0; i--)
            heapify(arr, n, i);
        
        // Extract elements one by one
        for (int i = n - 1; i > 0; i--) {
            // Swap arr[0] and arr[i]
            int temp = arr[0];
            arr[0] = arr[i];
            arr[i] = temp;
            heapify(arr, i, 0);
        }
    }
    
    // Example
    public static void main(String[] args) {
        int[] arr = {12, 11, 13, 5, 6, 7};
        heapSort(arr);
        
        System.out.print("Sorted array: ");
        for (int num : arr)
            System.out.print(num + " ");
        // Output: 5 6 7 11 12 13
    }
}
```

### Dry Run Example
```
Initial: [4, 10, 3, 5, 1]

Build Max Heap:
       4
      / \
     10  3
    / \
   5   1

After heapify:
       10
      /  \
     5    3
    / \
   4   1

Extract Max (10): [1, 5, 3, 4 | 10]
Heapify: [5, 4, 3, 1 | 10]

Extract Max (5): [1, 4, 3 | 5, 10]
Heapify: [4, 1, 3 | 5, 10]

Extract Max (4): [3, 1 | 4, 5, 10]
Heapify: [3, 1 | 4, 5, 10]

Extract Max (3): [1 | 3, 4, 5, 10]

Final: [1, 3, 4, 5, 10]
```

### Complexity Analysis
- **Time Complexity**: O(n log n) in all cases
- **Space Complexity**: O(1)
- **Stable**: No

### When to Use
- When consistent O(n log n) is needed
- In-place sorting required
- Memory is limited

---

## 7. Counting Sort

### Concept
Counting Sort is a non-comparison based algorithm. It counts occurrences of each element and uses arithmetic to determine positions.

### Algorithm
```
counting_sort(arr):
    max_val = max(arr)
    count = array of size (max_val + 1)
    
    # Count occurrences
    for element in arr:
        count[element]++
    
    # Cumulative count
    for i = 1 to max_val:
        count[i] += count[i-1]
    
    # Build output
    for i = n-1 down to 0:
        output[count[arr[i]] - 1] = arr[i]
        count[arr[i]]--
```

### Implementation (Python)
```python
def counting_sort(arr):
    if not arr:
        return arr
    
    max_val = max(arr)
    min_val = min(arr)
    range_size = max_val - min_val + 1
    
    count = [0] * range_size
    output = [0] * len(arr)
    
    # Count occurrences
    for num in arr:
        count[num - min_val] += 1
    
    # Cumulative count
    for i in range(1, len(count)):
        count[i] += count[i - 1]
    
    # Build output array
    for i in range(len(arr) - 1, -1, -1):
        output[count[arr[i] - min_val] - 1] = arr[i]
        count[arr[i] - min_val] -= 1
    
    return output

# Example
arr = [4, 2, 2, 8, 3, 3, 1]
print(counting_sort(arr))  # Output: [1, 2, 2, 3, 3, 4, 8]
```

### Implementation (C)
```c
#include <stdio.h>
#include <stdlib.h>

void counting_sort(int arr[], int n) {
    if (n == 0) return;
    
    // Find min and max values
    int max_val = arr[0], min_val = arr[0];
    for (int i = 1; i < n; i++) {
        if (arr[i] > max_val) max_val = arr[i];
        if (arr[i] < min_val) min_val = arr[i];
    }
    
    int range_size = max_val - min_val + 1;
    
    // Create count and output arrays
    int* count = (int*)calloc(range_size, sizeof(int));
    int* output = (int*)malloc(n * sizeof(int));
    
    // Count occurrences
    for (int i = 0; i < n; i++)
        count[arr[i] - min_val]++;
    
    // Cumulative count
    for (int i = 1; i < range_size; i++)
        count[i] += count[i - 1];
    
    // Build output array
    for (int i = n - 1; i >= 0; i--) {
        output[count[arr[i] - min_val] - 1] = arr[i];
        count[arr[i] - min_val]--;
    }
    
    // Copy output to arr
    for (int i = 0; i < n; i++)
        arr[i] = output[i];
    
    free(count);
    free(output);
}

// Example
int main() {
    int arr[] = {4, 2, 2, 8, 3, 3, 1};
    int n = sizeof(arr) / sizeof(arr[0]);
    counting_sort(arr, n);
    
    printf("Sorted array: ");
    for (int i = 0; i < n; i++)
        printf("%d ", arr[i]);
    // Output: 1 2 2 3 3 4 8
    return 0;
}
```

### Implementation (C++)
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

vector<int> counting_sort(vector<int> arr) {
    if (arr.empty()) return arr;
    
    int max_val = *max_element(arr.begin(), arr.end());
    int min_val = *min_element(arr.begin(), arr.end());
    int range_size = max_val - min_val + 1;
    
    vector<int> count(range_size, 0);
    vector<int> output(arr.size());
    
    // Count occurrences
    for (int num : arr)
        count[num - min_val]++;
    
    // Cumulative count
    for (int i = 1; i < range_size; i++)
        count[i] += count[i - 1];
    
    // Build output array
    for (int i = arr.size() - 1; i >= 0; i--) {
        output[count[arr[i] - min_val] - 1] = arr[i];
        count[arr[i] - min_val]--;
    }
    
    return output;
}

// Example
int main() {
    vector<int> arr = {4, 2, 2, 8, 3, 3, 1};
    arr = counting_sort(arr);
    
    cout << "Sorted array: ";
    for (int num : arr)
        cout << num << " ";
    // Output: 1 2 2 3 3 4 8
    return 0;
}
```

### Implementation (Java)
```java
public class CountingSort {
    public static int[] countingSort(int[] arr) {
        if (arr.length == 0) return arr;
        
        // Find min and max values
        int max_val = arr[0], min_val = arr[0];
        for (int i = 1; i < arr.length; i++) {
            if (arr[i] > max_val) max_val = arr[i];
            if (arr[i] < min_val) min_val = arr[i];
        }
        
        int range_size = max_val - min_val + 1;
        
        // Create count and output arrays
        int[] count = new int[range_size];
        int[] output = new int[arr.length];
        
        // Count occurrences
        for (int num : arr)
            count[num - min_val]++;
        
        // Cumulative count
        for (int i = 1; i < range_size; i++)
            count[i] += count[i - 1];
        
        // Build output array
        for (int i = arr.length - 1; i >= 0; i--) {
            output[count[arr[i] - min_val] - 1] = arr[i];
            count[arr[i] - min_val]--;
        }
        
        return output;
    }
    
    // Example
    public static void main(String[] args) {
        int[] arr = {4, 2, 2, 8, 3, 3, 1};
        arr = countingSort(arr);
        
        System.out.print("Sorted array: ");
        for (int num : arr)
            System.out.print(num + " ");
        // Output: 1 2 2 3 3 4 8
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(n + k) where k is range
- **Space Complexity**: O(n + k)
- **Stable**: Yes

### When to Use
- Small range of integers
- When stability is required
- O(n) sorting is needed

---

## 8. Radix Sort

### Concept
Radix Sort processes digits from least significant to most significant, using a stable sort (usually counting sort) for each digit.

### Implementation (Python)
```python
def counting_sort_for_radix(arr, exp):
    n = len(arr)
    output = [0] * n
    count = [0] * 10
    
    for i in range(n):
        index = arr[i] // exp
        count[index % 10] += 1
    
    for i in range(1, 10):
        count[i] += count[i - 1]
    
    i = n - 1
    while i >= 0:
        index = arr[i] // exp
        output[count[index % 10] - 1] = arr[i]
        count[index % 10] -= 1
        i -= 1
    
    for i in range(n):
        arr[i] = output[i]

def radix_sort(arr):
    max_val = max(arr)
    exp = 1
    
    while max_val // exp > 0:
        counting_sort_for_radix(arr, exp)
        exp *= 10
    
    return arr

# Example
arr = [170, 45, 75, 90, 802, 24, 2, 66]
print(radix_sort(arr))  # Output: [2, 24, 45, 66, 75, 90, 170, 802]
```

### Implementation (C)
```c
#include <stdio.h>

void counting_sort_for_radix(int arr[], int n, int exp) {
    int output[n];
    int count[10] = {0};
    
    // Count occurrences
    for (int i = 0; i < n; i++) {
        int index = (arr[i] / exp) % 10;
        count[index]++;
    }
    
    // Cumulative count
    for (int i = 1; i < 10; i++)
        count[i] += count[i - 1];
    
    // Build output array
    for (int i = n - 1; i >= 0; i--) {
        int index = (arr[i] / exp) % 10;
        output[count[index] - 1] = arr[i];
        count[index]--;
    }
    
    // Copy output to arr
    for (int i = 0; i < n; i++)
        arr[i] = output[i];
}

void radix_sort(int arr[], int n) {
    // Find maximum value
    int max_val = arr[0];
    for (int i = 1; i < n; i++)
        if (arr[i] > max_val)
            max_val = arr[i];
    
    // Sort by each digit
    for (int exp = 1; max_val / exp > 0; exp *= 10)
        counting_sort_for_radix(arr, n, exp);
}

// Example
int main() {
    int arr[] = {170, 45, 75, 90, 802, 24, 2, 66};
    int n = sizeof(arr) / sizeof(arr[0]);
    radix_sort(arr, n);
    
    printf("Sorted array: ");
    for (int i = 0; i < n; i++)
        printf("%d ", arr[i]);
    // Output: 2 24 45 66 75 90 170 802
    return 0;
}
```

### Implementation (C++)
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

void counting_sort_for_radix(vector<int>& arr, int exp) {
    int n = arr.size();
    vector<int> output(n);
    vector<int> count(10, 0);
    
    // Count occurrences
    for (int i = 0; i < n; i++) {
        int index = (arr[i] / exp) % 10;
        count[index]++;
    }
    
    // Cumulative count
    for (int i = 1; i < 10; i++)
        count[i] += count[i - 1];
    
    // Build output array
    for (int i = n - 1; i >= 0; i--) {
        int index = (arr[i] / exp) % 10;
        output[count[index] - 1] = arr[i];
        count[index]--;
    }
    
    // Copy output to arr
    for (int i = 0; i < n; i++)
        arr[i] = output[i];
}

void radix_sort(vector<int>& arr) {
    // Find maximum value
    int max_val = *max_element(arr.begin(), arr.end());
    
    // Sort by each digit
    for (int exp = 1; max_val / exp > 0; exp *= 10)
        counting_sort_for_radix(arr, exp);
}

// Example
int main() {
    vector<int> arr = {170, 45, 75, 90, 802, 24, 2, 66};
    radix_sort(arr);
    
    cout << "Sorted array: ";
    for (int num : arr)
        cout << num << " ";
    // Output: 2 24 45 66 75 90 170 802
    return 0;
}
```

### Implementation (Java)
```java
public class RadixSort {
    public static void countingSortForRadix(int[] arr, int exp) {
        int n = arr.length;
        int[] output = new int[n];
        int[] count = new int[10];
        
        // Count occurrences
        for (int i = 0; i < n; i++) {
            int index = (arr[i] / exp) % 10;
            count[index]++;
        }
        
        // Cumulative count
        for (int i = 1; i < 10; i++)
            count[i] += count[i - 1];
        
        // Build output array
        for (int i = n - 1; i >= 0; i--) {
            int index = (arr[i] / exp) % 10;
            output[count[index] - 1] = arr[i];
            count[index]--;
        }
        
        // Copy output to arr
        for (int i = 0; i < n; i++)
            arr[i] = output[i];
    }
    
    public static void radixSort(int[] arr) {
        // Find maximum value
        int max_val = arr[0];
        for (int i = 1; i < arr.length; i++)
            if (arr[i] > max_val)
                max_val = arr[i];
        
        // Sort by each digit
        for (int exp = 1; max_val / exp > 0; exp *= 10)
            countingSortForRadix(arr, exp);
    }
    
    // Example
    public static void main(String[] args) {
        int[] arr = {170, 45, 75, 90, 802, 24, 2, 66};
        radixSort(arr);
        
        System.out.print("Sorted array: ");
        for (int num : arr)
            System.out.print(num + " ");
        // Output: 2 24 45 66 75 90 170 802
    }
}
```

### Complexity Analysis
- **Time Complexity**: O(d × (n + k)) where d is number of digits
- **Space Complexity**: O(n + k)
- **Stable**: Yes

---

## Comparison Table

| Algorithm      | Best      | Average   | Worst     | Space | Stable |
|---------------|-----------|-----------|-----------|-------|--------|
| Bubble Sort   | O(n)      | O(n²)     | O(n²)     | O(1)  | Yes    |
| Selection Sort| O(n²)     | O(n²)     | O(n²)     | O(1)  | No     |
| Insertion Sort| O(n)      | O(n²)     | O(n²)     | O(1)  | Yes    |
| Merge Sort    | O(n log n)| O(n log n)| O(n log n)| O(n)  | Yes    |
| Quick Sort    | O(n log n)| O(n log n)| O(n²)     | O(log n)| No   |
| Heap Sort     | O(n log n)| O(n log n)| O(n log n)| O(1)  | No     |
| Counting Sort | O(n+k)    | O(n+k)    | O(n+k)    | O(k)  | Yes    |
| Radix Sort    | O(d×(n+k))| O(d×(n+k))| O(d×(n+k))| O(n+k)| Yes    |

---

## Exam Tips for MCQs

### Logic Flow Completion
**Question**: What is the missing line in bubble sort?
```python
for i in range(n):
    for j in range(0, n-i-1):
        if arr[j] > arr[j+1]:
            # Missing line
```
**Answer**: `arr[j], arr[j+1] = arr[j+1], arr[j]`

### Debugging
**Question**: What's wrong with this code?
```python
def bubble_sort(arr):
    for i in range(len(arr)):
        for j in range(len(arr)):  # Bug here
            if arr[j] > arr[j+1]:
                arr[j], arr[j+1] = arr[j+1], arr[j]
```
**Answer**: Should be `range(len(arr)-i-1)` to avoid index out of bounds

### Syntax Understanding
- Know the syntax of swap operations
- Understand loop boundaries
- Recognize recursive calls

### Program Dry Run
- Practice tracing through 3-5 element arrays
- Track variable changes step by step
- Note the number of comparisons and swaps

### Key Concepts to Remember
1. **Stability**: Maintains relative order of equal elements
2. **In-place**: Uses O(1) extra space
3. **Comparison vs Non-comparison**: Comparison sorts have O(n log n) lower bound
4. **Adaptive**: Performs better on partially sorted data
5. **Online**: Can sort data as it arrives
