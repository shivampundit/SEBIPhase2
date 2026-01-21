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

```c
// C Implementation
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

// Helper function prototypes (to be implemented based on problem)
bool is_base_case(void* problem);
void* solve_directly(void* problem);
void** divide_problem(void* problem, int* num_subproblems);
void* combine_solutions(void** sub_solutions, int num_solutions);

void* divide_and_conquer(void* problem) {
    // Base case
    if (is_base_case(problem)) {
        return solve_directly(problem);
    }
    
    // Divide
    int num_subproblems;
    void** subproblems = divide_problem(problem, &num_subproblems);
    
    // Conquer
    void** sub_solutions = malloc(num_subproblems * sizeof(void*));
    for (int i = 0; i < num_subproblems; i++) {
        sub_solutions[i] = divide_and_conquer(subproblems[i]);
    }
    
    // Combine
    void* result = combine_solutions(sub_solutions, num_subproblems);
    
    // Cleanup
    free(subproblems);
    free(sub_solutions);
    
    return result;
}
```

```cpp
// C++ Implementation
#include <iostream>
#include <vector>
using namespace std;

template<typename T>
class DivideAndConquer {
public:
    // Virtual functions to be overridden by specific problems
    virtual bool isBaseCase(const T& problem) = 0;
    virtual T solveDirect(const T& problem) = 0;
    virtual vector<T> divide(const T& problem) = 0;
    virtual T combine(const vector<T>& subSolutions) = 0;
    
    T solve(const T& problem) {
        // Base case
        if (isBaseCase(problem)) {
            return solveDirect(problem);
        }
        
        // Divide
        vector<T> subproblems = divide(problem);
        
        // Conquer
        vector<T> subSolutions;
        for (const auto& subproblem : subproblems) {
            subSolutions.push_back(solve(subproblem));
        }
        
        // Combine
        return combine(subSolutions);
    }
};
```

```java
// Java Implementation
import java.util.*;

public abstract class DivideAndConquer<T> {
    // Abstract methods to be implemented by specific problems
    protected abstract boolean isBaseCase(T problem);
    protected abstract T solveDirect(T problem);
    protected abstract List<T> divide(T problem);
    protected abstract T combine(List<T> subSolutions);
    
    public T solve(T problem) {
        // Base case
        if (isBaseCase(problem)) {
            return solveDirect(problem);
        }
        
        // Divide
        List<T> subproblems = divide(problem);
        
        // Conquer
        List<T> subSolutions = new ArrayList<>();
        for (T subproblem : subproblems) {
            subSolutions.add(solve(subproblem));
        }
        
        // Combine
        return combine(subSolutions);
    }
}
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

```c
// C Implementation
#include <stdio.h>
#include <limits.h>

int max(int a, int b) {
    return (a > b) ? a : b;
}

int max_subarray_dc(int arr[], int left, int right) {
    /*
     * Find maximum subarray sum using divide and conquer
     * Time: O(n log n)
     * Space: O(log n)
     */
    if (left == right) {
        return arr[left];
    }
    
    int mid = left + (right - left) / 2;  // Avoid overflow
    
    // Conquer
    int left_max = max_subarray_dc(arr, left, mid);
    int right_max = max_subarray_dc(arr, mid + 1, right);
    
    // Find max crossing subarray
    int left_sum = INT_MIN;
    int current_sum = 0;
    for (int i = mid; i >= left; i--) {
        current_sum += arr[i];
        left_sum = max(left_sum, current_sum);
    }
    
    int right_sum = INT_MIN;
    current_sum = 0;
    for (int i = mid + 1; i <= right; i++) {
        current_sum += arr[i];
        right_sum = max(right_sum, current_sum);
    }
    
    int cross_sum = left_sum + right_sum;
    
    // Combine
    return max(max(left_max, right_max), cross_sum);
}

// Example usage
int main() {
    int arr[] = {-2, 1, -3, 4, -1, 2, 1, -5, 4};
    int n = sizeof(arr) / sizeof(arr[0]);
    int result = max_subarray_dc(arr, 0, n - 1);
    printf("Maximum subarray sum: %d\n", result);  // 6: [4,-1,2,1]
    return 0;
}
```

```cpp
// C++ Implementation
#include <iostream>
#include <vector>
#include <algorithm>
#include <climits>
using namespace std;

class MaxSubarray {
public:
    /*
     * Find maximum subarray sum using divide and conquer
     * Time: O(n log n)
     * Space: O(log n)
     */
    int maxSubarrayDC(vector<int>& arr, int left, int right) {
        if (left == right) {
            return arr[left];
        }
        
        int mid = left + (right - left) / 2;  // Avoid overflow
        
        // Conquer
        int leftMax = maxSubarrayDC(arr, left, mid);
        int rightMax = maxSubarrayDC(arr, mid + 1, right);
        
        // Find max crossing subarray
        int leftSum = INT_MIN;
        int currentSum = 0;
        for (int i = mid; i >= left; i--) {
            currentSum += arr[i];
            leftSum = max(leftSum, currentSum);
        }
        
        int rightSum = INT_MIN;
        currentSum = 0;
        for (int i = mid + 1; i <= right; i++) {
            currentSum += arr[i];
            rightSum = max(rightSum, currentSum);
        }
        
        int crossSum = leftSum + rightSum;
        
        // Combine
        return max({leftMax, rightMax, crossSum});
    }
};

// Example usage
int main() {
    vector<int> arr = {-2, 1, -3, 4, -1, 2, 1, -5, 4};
    MaxSubarray solver;
    int result = solver.maxSubarrayDC(arr, 0, arr.size() - 1);
    cout << "Maximum subarray sum: " << result << endl;  // 6: [4,-1,2,1]
    return 0;
}
```

```java
// Java Implementation
public class MaxSubarray {
    /*
     * Find maximum subarray sum using divide and conquer
     * Time: O(n log n)
     * Space: O(log n)
     */
    public int maxSubarrayDC(int[] arr, int left, int right) {
        if (left == right) {
            return arr[left];
        }
        
        int mid = left + (right - left) / 2;  // Avoid overflow
        
        // Conquer
        int leftMax = maxSubarrayDC(arr, left, mid);
        int rightMax = maxSubarrayDC(arr, mid + 1, right);
        
        // Find max crossing subarray
        int leftSum = Integer.MIN_VALUE;
        int currentSum = 0;
        for (int i = mid; i >= left; i--) {
            currentSum += arr[i];
            leftSum = Math.max(leftSum, currentSum);
        }
        
        int rightSum = Integer.MIN_VALUE;
        currentSum = 0;
        for (int i = mid + 1; i <= right; i++) {
            currentSum += arr[i];
            rightSum = Math.max(rightSum, currentSum);
        }
        
        int crossSum = leftSum + rightSum;
        
        // Combine
        return Math.max(Math.max(leftMax, rightMax), crossSum);
    }
    
    // Example usage
    public static void main(String[] args) {
        int[] arr = {-2, 1, -3, 4, -1, 2, 1, -5, 4};
        MaxSubarray solver = new MaxSubarray();
        int result = solver.maxSubarrayDC(arr, 0, arr.length - 1);
        System.out.println("Maximum subarray sum: " + result);  // 6: [4,-1,2,1]
    }
}
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

```c
// C Implementation
#include <stdio.h>

int max(int a, int b) {
    return (a > b) ? a : b;
}

int max_subarray_kadane(int arr[], int n) {
    /*
     * Optimal solution using dynamic programming
     * Time: O(n)
     * Space: O(1)
     */
    int max_sum = arr[0];
    int current_sum = arr[0];
    
    for (int i = 1; i < n; i++) {
        current_sum = max(arr[i], current_sum + arr[i]);
        max_sum = max(max_sum, current_sum);
    }
    
    return max_sum;
}
```

```cpp
// C++ Implementation
#include <vector>
#include <algorithm>
using namespace std;

int maxSubarrayKadane(vector<int>& arr) {
    /*
     * Optimal solution using dynamic programming
     * Time: O(n)
     * Space: O(1)
     */
    int maxSum = arr[0];
    int currentSum = arr[0];
    
    for (int i = 1; i < arr.size(); i++) {
        currentSum = max(arr[i], currentSum + arr[i]);
        maxSum = max(maxSum, currentSum);
    }
    
    return maxSum;
}
```

```java
// Java Implementation
public class MaxSubarrayKadane {
    /*
     * Optimal solution using dynamic programming
     * Time: O(n)
     * Space: O(1)
     */
    public int maxSubarrayKadane(int[] arr) {
        int maxSum = arr[0];
        int currentSum = arr[0];
        
        for (int i = 1; i < arr.length; i++) {
            currentSum = Math.max(arr[i], currentSum + arr[i]);
            maxSum = Math.max(maxSum, currentSum);
        }
        
        return maxSum;
    }
}
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

```c
// C Implementation
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
#include <float.h>

typedef struct {
    double x, y;
} Point;

double distance(Point p1, Point p2) {
    return sqrt((p1.x - p2.x) * (p1.x - p2.x) + (p1.y - p2.y) * (p1.y - p2.y));
}

double min(double a, double b) {
    return (a < b) ? a : b;
}

// Comparator functions for qsort
int compare_x(const void* a, const void* b) {
    Point* p1 = (Point*)a;
    Point* p2 = (Point*)b;
    return (p1->x > p2->x) - (p1->x < p2->x);
}

int compare_y(const void* a, const void* b) {
    Point* p1 = (Point*)a;
    Point* p2 = (Point*)b;
    return (p1->y > p2->y) - (p1->y < p2->y);
}

double brute_force(Point points[], int n) {
    double min_dist = DBL_MAX;
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            double dist = distance(points[i], points[j]);
            if (dist < min_dist) {
                min_dist = dist;
            }
        }
    }
    return min_dist;
}

double closest_in_strip(Point strip[], int n, double d) {
    double min_dist = d;
    qsort(strip, n, sizeof(Point), compare_y);
    
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n && (strip[j].y - strip[i].y) < min_dist; j++) {
            double dist = distance(strip[i], strip[j]);
            if (dist < min_dist) {
                min_dist = dist;
            }
        }
    }
    
    return min_dist;
}

double closest_util(Point px[], int n) {
    /*
     * Find closest pair of points
     * Time: O(n log n)
     * Space: O(n)
     */
    if (n <= 3) {
        return brute_force(px, n);
    }
    
    int mid = n / 2;
    Point midpoint = px[mid];
    
    double dl = closest_util(px, mid);
    double dr = closest_util(px + mid, n - mid);
    
    double d = min(dl, dr);
    
    // Find points in strip
    Point* strip = (Point*)malloc(n * sizeof(Point));
    int strip_size = 0;
    for (int i = 0; i < n; i++) {
        if (fabs(px[i].x - midpoint.x) < d) {
            strip[strip_size++] = px[i];
        }
    }
    
    double strip_dist = closest_in_strip(strip, strip_size, d);
    free(strip);
    
    return min(d, strip_dist);
}

double closest_pair(Point points[], int n) {
    qsort(points, n, sizeof(Point), compare_x);
    return closest_util(points, n);
}

// Example usage
int main() {
    Point points[] = {{2, 3}, {12, 30}, {40, 50}, {5, 1}, {12, 10}, {3, 4}};
    int n = sizeof(points) / sizeof(points[0]);
    double min_distance = closest_pair(points, n);
    printf("Minimum distance: %.2f\n", min_distance);
    return 0;
}
```

```cpp
// C++ Implementation
#include <iostream>
#include <vector>
#include <algorithm>
#include <cmath>
#include <limits>
using namespace std;

struct Point {
    double x, y;
};

class ClosestPair {
private:
    double distance(Point p1, Point p2) {
        return sqrt((p1.x - p2.x) * (p1.x - p2.x) + (p1.y - p2.y) * (p1.y - p2.y));
    }
    
    double bruteForce(vector<Point>& points) {
        double minDist = numeric_limits<double>::max();
        int n = points.size();
        
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                minDist = min(minDist, distance(points[i], points[j]));
            }
        }
        
        return minDist;
    }
    
    double closestInStrip(vector<Point>& strip, double d) {
        double minDist = d;
        sort(strip.begin(), strip.end(), [](Point a, Point b) { return a.y < b.y; });
        
        for (int i = 0; i < strip.size(); i++) {
            int j = i + 1;
            while (j < strip.size() && (strip[j].y - strip[i].y) < minDist) {
                minDist = min(minDist, distance(strip[i], strip[j]));
                j++;
            }
        }
        
        return minDist;
    }
    
    double closestUtil(vector<Point>& px, vector<Point>& py) {
        int n = px.size();
        
        if (n <= 3) {
            return bruteForce(px);
        }
        
        int mid = n / 2;
        Point midpoint = px[mid];
        
        vector<Point> pyl, pyr;
        for (auto& p : py) {
            if (p.x <= midpoint.x) {
                pyl.push_back(p);
            } else {
                pyr.push_back(p);
            }
        }
        
        vector<Point> pxl(px.begin(), px.begin() + mid);
        vector<Point> pxr(px.begin() + mid, px.end());
        
        double dl = closestUtil(pxl, pyl);
        double dr = closestUtil(pxr, pyr);
        
        double d = min(dl, dr);
        
        vector<Point> strip;
        for (auto& p : py) {
            if (abs(p.x - midpoint.x) < d) {
                strip.push_back(p);
            }
        }
        
        return min(d, closestInStrip(strip, d));
    }
    
public:
    /*
     * Find closest pair of points
     * Time: O(n log n)
     * Space: O(n)
     */
    double closestPair(vector<Point>& points) {
        vector<Point> px = points;
        vector<Point> py = points;
        
        sort(px.begin(), px.end(), [](Point a, Point b) { return a.x < b.x; });
        sort(py.begin(), py.end(), [](Point a, Point b) { return a.y < b.y; });
        
        return closestUtil(px, py);
    }
};

// Example usage
int main() {
    vector<Point> points = {{2, 3}, {12, 30}, {40, 50}, {5, 1}, {12, 10}, {3, 4}};
    ClosestPair solver;
    double minDistance = solver.closestPair(points);
    cout << "Minimum distance: " << fixed << minDistance << endl;
    return 0;
}
```

```java
// Java Implementation
import java.util.*;

class Point {
    double x, y;
    
    Point(double x, double y) {
        this.x = x;
        this.y = y;
    }
}

public class ClosestPair {
    private double distance(Point p1, Point p2) {
        return Math.sqrt((p1.x - p2.x) * (p1.x - p2.x) + (p1.y - p2.y) * (p1.y - p2.y));
    }
    
    private double bruteForce(List<Point> points) {
        double minDist = Double.MAX_VALUE;
        int n = points.size();
        
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                minDist = Math.min(minDist, distance(points.get(i), points.get(j)));
            }
        }
        
        return minDist;
    }
    
    private double closestInStrip(List<Point> strip, double d) {
        double minDist = d;
        Collections.sort(strip, Comparator.comparingDouble(p -> p.y));
        
        for (int i = 0; i < strip.size(); i++) {
            int j = i + 1;
            while (j < strip.size() && (strip.get(j).y - strip.get(i).y) < minDist) {
                minDist = Math.min(minDist, distance(strip.get(i), strip.get(j)));
                j++;
            }
        }
        
        return minDist;
    }
    
    private double closestUtil(List<Point> px, List<Point> py) {
        int n = px.size();
        
        if (n <= 3) {
            return bruteForce(px);
        }
        
        int mid = n / 2;
        Point midpoint = px.get(mid);
        
        List<Point> pyl = new ArrayList<>();
        List<Point> pyr = new ArrayList<>();
        for (Point p : py) {
            if (p.x <= midpoint.x) {
                pyl.add(p);
            } else {
                pyr.add(p);
            }
        }
        
        double dl = closestUtil(px.subList(0, mid), pyl);
        double dr = closestUtil(px.subList(mid, n), pyr);
        
        double d = Math.min(dl, dr);
        
        List<Point> strip = new ArrayList<>();
        for (Point p : py) {
            if (Math.abs(p.x - midpoint.x) < d) {
                strip.add(p);
            }
        }
        
        return Math.min(d, closestInStrip(strip, d));
    }
    
    /*
     * Find closest pair of points
     * Time: O(n log n)
     * Space: O(n)
     */
    public double closestPair(List<Point> points) {
        List<Point> px = new ArrayList<>(points);
        List<Point> py = new ArrayList<>(points);
        
        Collections.sort(px, Comparator.comparingDouble(p -> p.x));
        Collections.sort(py, Comparator.comparingDouble(p -> p.y));
        
        return closestUtil(px, py);
    }
    
    // Example usage
    public static void main(String[] args) {
        List<Point> points = Arrays.asList(
            new Point(2, 3), new Point(12, 30), new Point(40, 50),
            new Point(5, 1), new Point(12, 10), new Point(3, 4)
        );
        ClosestPair solver = new ClosestPair();
        double minDistance = solver.closestPair(points);
        System.out.printf("Minimum distance: %.2f\n", minDistance);
    }
}
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

```c
// C Implementation
#include <stdio.h>
#include <stdlib.h>

void matrix_multiply_standard(int** A, int** B, int** C, int n) {
    /*
     * Standard matrix multiplication
     * Time: O(n³)
     */
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            C[i][j] = 0;
            for (int k = 0; k < n; k++) {
                C[i][j] += A[i][k] * B[k][j];
            }
        }
    }
}

// Helper function to allocate matrix
int** allocate_matrix(int n) {
    int** matrix = (int**)malloc(n * sizeof(int*));
    for (int i = 0; i < n; i++) {
        matrix[i] = (int*)malloc(n * sizeof(int));
    }
    return matrix;
}

// Helper function to free matrix
void free_matrix(int** matrix, int n) {
    for (int i = 0; i < n; i++) {
        free(matrix[i]);
    }
    free(matrix);
}
```

```cpp
// C++ Implementation
#include <vector>
using namespace std;

vector<vector<int>> matrixMultiplyStandard(const vector<vector<int>>& A, 
                                           const vector<vector<int>>& B) {
    /*
     * Standard matrix multiplication
     * Time: O(n³)
     */
    int n = A.size();
    vector<vector<int>> C(n, vector<int>(n, 0));
    
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            for (int k = 0; k < n; k++) {
                C[i][j] += A[i][k] * B[k][j];
            }
        }
    }
    
    return C;
}
```

```java
// Java Implementation
public class MatrixMultiplication {
    /*
     * Standard matrix multiplication
     * Time: O(n³)
     */
    public int[][] matrixMultiplyStandard(int[][] A, int[][] B) {
        int n = A.length;
        int[][] C = new int[n][n];
        
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                for (int k = 0; k < n; k++) {
                    C[i][j] += A[i][k] * B[k][j];
                }
            }
        }
        
        return C;
    }
}
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

```c
// C Implementation - Strassen's Algorithm
#include <stdio.h>
#include <stdlib.h>

// Helper functions for matrix operations
int** allocate_matrix(int n) {
    int** matrix = (int**)malloc(n * sizeof(int*));
    for (int i = 0; i < n; i++) {
        matrix[i] = (int*)calloc(n, sizeof(int));
    }
    return matrix;
}

void free_matrix(int** matrix, int n) {
    for (int i = 0; i < n; i++) {
        free(matrix[i]);
    }
    free(matrix);
}

void add_matrix(int** A, int** B, int** C, int n) {
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            C[i][j] = A[i][j] + B[i][j];
        }
    }
}

void subtract_matrix(int** A, int** B, int** C, int n) {
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            C[i][j] = A[i][j] - B[i][j];
        }
    }
}

void strassen_multiply(int** A, int** B, int** C, int n) {
    /*
     * Strassen's matrix multiplication
     * Time: O(n^2.807)
     */
    if (n == 1) {
        C[0][0] = A[0][0] * B[0][0];
        return;
    }
    
    int mid = n / 2;
    
    // Allocate submatrices
    int** A11 = allocate_matrix(mid);
    int** A12 = allocate_matrix(mid);
    int** A21 = allocate_matrix(mid);
    int** A22 = allocate_matrix(mid);
    int** B11 = allocate_matrix(mid);
    int** B12 = allocate_matrix(mid);
    int** B21 = allocate_matrix(mid);
    int** B22 = allocate_matrix(mid);
    
    // Divide matrices into quadrants
    for (int i = 0; i < mid; i++) {
        for (int j = 0; j < mid; j++) {
            A11[i][j] = A[i][j];
            A12[i][j] = A[i][j + mid];
            A21[i][j] = A[i + mid][j];
            A22[i][j] = A[i + mid][j + mid];
            B11[i][j] = B[i][j];
            B12[i][j] = B[i][j + mid];
            B21[i][j] = B[i + mid][j];
            B22[i][j] = B[i + mid][j + mid];
        }
    }
    
    // Compute 7 products
    int** M1 = allocate_matrix(mid);
    int** M2 = allocate_matrix(mid);
    int** M3 = allocate_matrix(mid);
    int** M4 = allocate_matrix(mid);
    int** M5 = allocate_matrix(mid);
    int** M6 = allocate_matrix(mid);
    int** M7 = allocate_matrix(mid);
    int** temp1 = allocate_matrix(mid);
    int** temp2 = allocate_matrix(mid);
    
    // M1 = (A11 + A22)(B11 + B22)
    add_matrix(A11, A22, temp1, mid);
    add_matrix(B11, B22, temp2, mid);
    strassen_multiply(temp1, temp2, M1, mid);
    
    // M2 = (A21 + A22)B11
    add_matrix(A21, A22, temp1, mid);
    strassen_multiply(temp1, B11, M2, mid);
    
    // M3 = A11(B12 - B22)
    subtract_matrix(B12, B22, temp1, mid);
    strassen_multiply(A11, temp1, M3, mid);
    
    // M4 = A22(B21 - B11)
    subtract_matrix(B21, B11, temp1, mid);
    strassen_multiply(A22, temp1, M4, mid);
    
    // M5 = (A11 + A12)B22
    add_matrix(A11, A12, temp1, mid);
    strassen_multiply(temp1, B22, M5, mid);
    
    // M6 = (A21 - A11)(B11 + B12)
    subtract_matrix(A21, A11, temp1, mid);
    add_matrix(B11, B12, temp2, mid);
    strassen_multiply(temp1, temp2, M6, mid);
    
    // M7 = (A12 - A22)(B21 + B22)
    subtract_matrix(A12, A22, temp1, mid);
    add_matrix(B21, B22, temp2, mid);
    strassen_multiply(temp1, temp2, M7, mid);
    
    // Compute C quadrants
    int** C11 = allocate_matrix(mid);
    int** C12 = allocate_matrix(mid);
    int** C21 = allocate_matrix(mid);
    int** C22 = allocate_matrix(mid);
    
    // C11 = M1 + M4 - M5 + M7
    add_matrix(M1, M4, temp1, mid);
    subtract_matrix(temp1, M5, temp2, mid);
    add_matrix(temp2, M7, C11, mid);
    
    // C12 = M3 + M5
    add_matrix(M3, M5, C12, mid);
    
    // C21 = M2 + M4
    add_matrix(M2, M4, C21, mid);
    
    // C22 = M1 + M3 - M2 + M6
    add_matrix(M1, M3, temp1, mid);
    subtract_matrix(temp1, M2, temp2, mid);
    add_matrix(temp2, M6, C22, mid);
    
    // Combine quadrants
    for (int i = 0; i < mid; i++) {
        for (int j = 0; j < mid; j++) {
            C[i][j] = C11[i][j];
            C[i][j + mid] = C12[i][j];
            C[i + mid][j] = C21[i][j];
            C[i + mid][j + mid] = C22[i][j];
        }
    }
    
    // Free memory
    free_matrix(A11, mid); free_matrix(A12, mid); free_matrix(A21, mid); free_matrix(A22, mid);
    free_matrix(B11, mid); free_matrix(B12, mid); free_matrix(B21, mid); free_matrix(B22, mid);
    free_matrix(M1, mid); free_matrix(M2, mid); free_matrix(M3, mid); free_matrix(M4, mid);
    free_matrix(M5, mid); free_matrix(M6, mid); free_matrix(M7, mid);
    free_matrix(temp1, mid); free_matrix(temp2, mid);
    free_matrix(C11, mid); free_matrix(C12, mid); free_matrix(C21, mid); free_matrix(C22, mid);
}
```

```cpp
// C++ Implementation - Strassen's Algorithm
#include <vector>
using namespace std;

class StrassenMultiply {
private:
    vector<vector<int>> addMatrix(const vector<vector<int>>& X, const vector<vector<int>>& Y) {
        int n = X.size();
        vector<vector<int>> result(n, vector<int>(n));
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                result[i][j] = X[i][j] + Y[i][j];
            }
        }
        return result;
    }
    
    vector<vector<int>> subtractMatrix(const vector<vector<int>>& X, const vector<vector<int>>& Y) {
        int n = X.size();
        vector<vector<int>> result(n, vector<int>(n));
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                result[i][j] = X[i][j] - Y[i][j];
            }
        }
        return result;
    }
    
public:
    /*
     * Strassen's matrix multiplication
     * Time: O(n^2.807)
     */
    vector<vector<int>> strassenMultiply(const vector<vector<int>>& A, const vector<vector<int>>& B) {
        int n = A.size();
        
        if (n == 1) {
            return {{A[0][0] * B[0][0]}};
        }
        
        int mid = n / 2;
        
        // Divide matrices into quadrants
        vector<vector<int>> A11(mid, vector<int>(mid));
        vector<vector<int>> A12(mid, vector<int>(mid));
        vector<vector<int>> A21(mid, vector<int>(mid));
        vector<vector<int>> A22(mid, vector<int>(mid));
        vector<vector<int>> B11(mid, vector<int>(mid));
        vector<vector<int>> B12(mid, vector<int>(mid));
        vector<vector<int>> B21(mid, vector<int>(mid));
        vector<vector<int>> B22(mid, vector<int>(mid));
        
        for (int i = 0; i < mid; i++) {
            for (int j = 0; j < mid; j++) {
                A11[i][j] = A[i][j];
                A12[i][j] = A[i][j + mid];
                A21[i][j] = A[i + mid][j];
                A22[i][j] = A[i + mid][j + mid];
                B11[i][j] = B[i][j];
                B12[i][j] = B[i][j + mid];
                B21[i][j] = B[i + mid][j];
                B22[i][j] = B[i + mid][j + mid];
            }
        }
        
        // Compute 7 products (Strassen's formulas)
        auto M1 = strassenMultiply(addMatrix(A11, A22), addMatrix(B11, B22));
        auto M2 = strassenMultiply(addMatrix(A21, A22), B11);
        auto M3 = strassenMultiply(A11, subtractMatrix(B12, B22));
        auto M4 = strassenMultiply(A22, subtractMatrix(B21, B11));
        auto M5 = strassenMultiply(addMatrix(A11, A12), B22);
        auto M6 = strassenMultiply(subtractMatrix(A21, A11), addMatrix(B11, B12));
        auto M7 = strassenMultiply(subtractMatrix(A12, A22), addMatrix(B21, B22));
        
        // Combine
        auto C11 = addMatrix(subtractMatrix(addMatrix(M1, M4), M5), M7);
        auto C12 = addMatrix(M3, M5);
        auto C21 = addMatrix(M2, M4);
        auto C22 = addMatrix(subtractMatrix(addMatrix(M1, M3), M2), M6);
        
        // Construct result matrix
        vector<vector<int>> C(n, vector<int>(n));
        for (int i = 0; i < mid; i++) {
            for (int j = 0; j < mid; j++) {
                C[i][j] = C11[i][j];
                C[i][j + mid] = C12[i][j];
                C[i + mid][j] = C21[i][j];
                C[i + mid][j + mid] = C22[i][j];
            }
        }
        
        return C;
    }
};
```

```java
// Java Implementation - Strassen's Algorithm
public class StrassenMultiply {
    private int[][] addMatrix(int[][] X, int[][] Y) {
        int n = X.length;
        int[][] result = new int[n][n];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                result[i][j] = X[i][j] + Y[i][j];
            }
        }
        return result;
    }
    
    private int[][] subtractMatrix(int[][] X, int[][] Y) {
        int n = X.length;
        int[][] result = new int[n][n];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                result[i][j] = X[i][j] - Y[i][j];
            }
        }
        return result;
    }
    
    /*
     * Strassen's matrix multiplication
     * Time: O(n^2.807)
     */
    public int[][] strassenMultiply(int[][] A, int[][] B) {
        int n = A.length;
        
        if (n == 1) {
            return new int[][]{{A[0][0] * B[0][0]}};
        }
        
        int mid = n / 2;
        
        // Divide matrices into quadrants
        int[][] A11 = new int[mid][mid];
        int[][] A12 = new int[mid][mid];
        int[][] A21 = new int[mid][mid];
        int[][] A22 = new int[mid][mid];
        int[][] B11 = new int[mid][mid];
        int[][] B12 = new int[mid][mid];
        int[][] B21 = new int[mid][mid];
        int[][] B22 = new int[mid][mid];
        
        for (int i = 0; i < mid; i++) {
            for (int j = 0; j < mid; j++) {
                A11[i][j] = A[i][j];
                A12[i][j] = A[i][j + mid];
                A21[i][j] = A[i + mid][j];
                A22[i][j] = A[i + mid][j + mid];
                B11[i][j] = B[i][j];
                B12[i][j] = B[i][j + mid];
                B21[i][j] = B[i + mid][j];
                B22[i][j] = B[i + mid][j + mid];
            }
        }
        
        // Compute 7 products (Strassen's formulas)
        int[][] M1 = strassenMultiply(addMatrix(A11, A22), addMatrix(B11, B22));
        int[][] M2 = strassenMultiply(addMatrix(A21, A22), B11);
        int[][] M3 = strassenMultiply(A11, subtractMatrix(B12, B22));
        int[][] M4 = strassenMultiply(A22, subtractMatrix(B21, B11));
        int[][] M5 = strassenMultiply(addMatrix(A11, A12), B22);
        int[][] M6 = strassenMultiply(subtractMatrix(A21, A11), addMatrix(B11, B12));
        int[][] M7 = strassenMultiply(subtractMatrix(A12, A22), addMatrix(B21, B22));
        
        // Combine
        int[][] C11 = addMatrix(subtractMatrix(addMatrix(M1, M4), M5), M7);
        int[][] C12 = addMatrix(M3, M5);
        int[][] C21 = addMatrix(M2, M4);
        int[][] C22 = addMatrix(subtractMatrix(addMatrix(M1, M3), M2), M6);
        
        // Construct result matrix
        int[][] C = new int[n][n];
        for (int i = 0; i < mid; i++) {
            for (int j = 0; j < mid; j++) {
                C[i][j] = C11[i][j];
                C[i][j + mid] = C12[i][j];
                C[i + mid][j] = C21[i][j];
                C[i + mid][j + mid] = C22[i][j];
            }
        }
        
        return C;
    }
}
```

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

```c
// C Implementation
#include <stdio.h>
#include <stdlib.h>

int merge_count(int arr[], int temp[], int left, int mid, int right) {
    int i = left;       // Left subarray index
    int j = mid + 1;    // Right subarray index
    int k = left;       // Merged array index
    int inv_count = 0;
    
    while (i <= mid && j <= right) {
        if (arr[i] <= arr[j]) {
            temp[k++] = arr[i++];
        } else {
            temp[k++] = arr[j++];
            inv_count += (mid - i + 1);  // Key step
        }
    }
    
    while (i <= mid) {
        temp[k++] = arr[i++];
    }
    
    while (j <= right) {
        temp[k++] = arr[j++];
    }
    
    for (i = left; i <= right; i++) {
        arr[i] = temp[i];
    }
    
    return inv_count;
}

int merge_sort_count(int arr[], int temp[], int left, int right) {
    int inv_count = 0;
    
    if (left < right) {
        int mid = left + (right - left) / 2;  // Avoid overflow
        
        inv_count += merge_sort_count(arr, temp, left, mid);
        inv_count += merge_sort_count(arr, temp, mid + 1, right);
        inv_count += merge_count(arr, temp, left, mid, right);
    }
    
    return inv_count;
}

int count_inversions(int arr[], int n) {
    /*
     * Count inversions using modified merge sort
     * Time: O(n log n)
     * Space: O(n)
     */
    int* temp = (int*)malloc(n * sizeof(int));
    int result = merge_sort_count(arr, temp, 0, n - 1);
    free(temp);
    return result;
}

// Example usage
int main() {
    int arr[] = {2, 4, 1, 3, 5};
    int n = sizeof(arr) / sizeof(arr[0]);
    int inversions = count_inversions(arr, n);
    printf("Number of inversions: %d\n", inversions);  // 3: (2,1), (4,1), (4,3)
    return 0;
}
```

```cpp
// C++ Implementation
#include <iostream>
#include <vector>
using namespace std;

class InversionCounter {
private:
    int mergeCount(vector<int>& arr, vector<int>& temp, int left, int mid, int right) {
        int i = left;       // Left subarray index
        int j = mid + 1;    // Right subarray index
        int k = left;       // Merged array index
        int invCount = 0;
        
        while (i <= mid && j <= right) {
            if (arr[i] <= arr[j]) {
                temp[k++] = arr[i++];
            } else {
                temp[k++] = arr[j++];
                invCount += (mid - i + 1);  // Key step
            }
        }
        
        while (i <= mid) {
            temp[k++] = arr[i++];
        }
        
        while (j <= right) {
            temp[k++] = arr[j++];
        }
        
        for (int i = left; i <= right; i++) {
            arr[i] = temp[i];
        }
        
        return invCount;
    }
    
    int mergeSortCount(vector<int>& arr, vector<int>& temp, int left, int right) {
        int invCount = 0;
        
        if (left < right) {
            int mid = left + (right - left) / 2;  // Avoid overflow
            
            invCount += mergeSortCount(arr, temp, left, mid);
            invCount += mergeSortCount(arr, temp, mid + 1, right);
            invCount += mergeCount(arr, temp, left, mid, right);
        }
        
        return invCount;
    }
    
public:
    /*
     * Count inversions using modified merge sort
     * Time: O(n log n)
     * Space: O(n)
     */
    int countInversions(vector<int>& arr) {
        int n = arr.size();
        vector<int> temp(n);
        return mergeSortCount(arr, temp, 0, n - 1);
    }
};

// Example usage
int main() {
    vector<int> arr = {2, 4, 1, 3, 5};
    InversionCounter counter;
    int inversions = counter.countInversions(arr);
    cout << "Number of inversions: " << inversions << endl;  // 3: (2,1), (4,1), (4,3)
    return 0;
}
```

```java
// Java Implementation
public class InversionCounter {
    private int mergeCount(int[] arr, int[] temp, int left, int mid, int right) {
        int i = left;       // Left subarray index
        int j = mid + 1;    // Right subarray index
        int k = left;       // Merged array index
        int invCount = 0;
        
        while (i <= mid && j <= right) {
            if (arr[i] <= arr[j]) {
                temp[k++] = arr[i++];
            } else {
                temp[k++] = arr[j++];
                invCount += (mid - i + 1);  // Key step
            }
        }
        
        while (i <= mid) {
            temp[k++] = arr[i++];
        }
        
        while (j <= right) {
            temp[k++] = arr[j++];
        }
        
        for (int idx = left; idx <= right; idx++) {
            arr[idx] = temp[idx];
        }
        
        return invCount;
    }
    
    private int mergeSortCount(int[] arr, int[] temp, int left, int right) {
        int invCount = 0;
        
        if (left < right) {
            int mid = left + (right - left) / 2;  // Avoid overflow
            
            invCount += mergeSortCount(arr, temp, left, mid);
            invCount += mergeSortCount(arr, temp, mid + 1, right);
            invCount += mergeCount(arr, temp, left, mid, right);
        }
        
        return invCount;
    }
    
    /*
     * Count inversions using modified merge sort
     * Time: O(n log n)
     * Space: O(n)
     */
    public int countInversions(int[] arr) {
        int n = arr.length;
        int[] temp = new int[n];
        return mergeSortCount(arr, temp, 0, n - 1);
    }
    
    // Example usage
    public static void main(String[] args) {
        int[] arr = {2, 4, 1, 3, 5};
        InversionCounter counter = new InversionCounter();
        int inversions = counter.countInversions(arr);
        System.out.println("Number of inversions: " + inversions);  // 3: (2,1), (4,1), (4,3)
    }
}
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

```c
// C Implementation
#include <stdio.h>
#include <math.h>

long long karatsuba(long long x, long long y) {
    /*
     * Karatsuba multiplication algorithm
     * Time: O(n^1.585) vs O(n²) for standard
     */
    // Base case
    if (x < 10 || y < 10) {
        return x * y;
    }
    
    // Calculate size
    int n = 0;
    long long temp = (x > y) ? x : y;
    while (temp > 0) {
        n++;
        temp /= 10;
    }
    int m = n / 2;
    
    // Split numbers
    long long power = (long long)pow(10, m);
    long long high1 = x / power;
    long long low1 = x % power;
    long long high2 = y / power;
    long long low2 = y % power;
    
    // Three recursive calls
    long long z0 = karatsuba(low1, low2);
    long long z1 = karatsuba(low1 + high1, low2 + high2);
    long long z2 = karatsuba(high1, high2);
    
    // Combine
    return z2 * (long long)pow(10, 2 * m) + (z1 - z2 - z0) * (long long)pow(10, m) + z0;
}

// Example usage
int main() {
    long long x = 1234;
    long long y = 5678;
    long long result = karatsuba(x, y);
    printf("%lld × %lld = %lld\n", x, y, result);
    printf("Verification: %lld\n", x * y);
    return 0;
}
```

```cpp
// C++ Implementation
#include <iostream>
#include <cmath>
#include <string>
using namespace std;

long long karatsuba(long long x, long long y) {
    /*
     * Karatsuba multiplication algorithm
     * Time: O(n^1.585) vs O(n²) for standard
     */
    // Base case
    if (x < 10 || y < 10) {
        return x * y;
    }
    
    // Calculate size
    int n = max(to_string(x).length(), to_string(y).length());
    int m = n / 2;
    
    // Split numbers
    long long power = pow(10, m);
    long long high1 = x / power;
    long long low1 = x % power;
    long long high2 = y / power;
    long long low2 = y % power;
    
    // Three recursive calls
    long long z0 = karatsuba(low1, low2);
    long long z1 = karatsuba(low1 + high1, low2 + high2);
    long long z2 = karatsuba(high1, high2);
    
    // Combine
    return z2 * pow(10, 2 * m) + (z1 - z2 - z0) * pow(10, m) + z0;
}

// Example usage
int main() {
    long long x = 1234;
    long long y = 5678;
    long long result = karatsuba(x, y);
    cout << x << " × " << y << " = " << result << endl;
    cout << "Verification: " << x * y << endl;
    return 0;
}
```

```java
// Java Implementation
public class Karatsuba {
    /*
     * Karatsuba multiplication algorithm
     * Time: O(n^1.585) vs O(n²) for standard
     */
    public long karatsuba(long x, long y) {
        // Base case
        if (x < 10 || y < 10) {
            return x * y;
        }
        
        // Calculate size
        int n = Math.max(String.valueOf(x).length(), String.valueOf(y).length());
        int m = n / 2;
        
        // Split numbers
        long power = (long)Math.pow(10, m);
        long high1 = x / power;
        long low1 = x % power;
        long high2 = y / power;
        long low2 = y % power;
        
        // Three recursive calls
        long z0 = karatsuba(low1, low2);
        long z1 = karatsuba(low1 + high1, low2 + high2);
        long z2 = karatsuba(high1, high2);
        
        // Combine
        return z2 * (long)Math.pow(10, 2 * m) + (z1 - z2 - z0) * (long)Math.pow(10, m) + z0;
    }
    
    // Example usage
    public static void main(String[] args) {
        Karatsuba solver = new Karatsuba();
        long x = 1234;
        long y = 5678;
        long result = solver.karatsuba(x, y);
        System.out.println(x + " × " + y + " = " + result);
        System.out.println("Verification: " + (x * y));
    }
}
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

```c
// C Implementation
#include <stdio.h>

double power_recursive(double x, int n) {
    /*
     * Calculate x^n using divide and conquer
     * Time: O(log n)
     * Space: O(log n)
     */
    // Base cases
    if (n == 0) return 1.0;
    if (n == 1) return x;
    if (n < 0) return 1.0 / power_recursive(x, -n);
    
    // Divide
    double half = power_recursive(x, n / 2);
    
    // Combine
    if (n % 2 == 0) {
        return half * half;
    } else {
        return half * half * x;
    }
}

double power_iterative(double x, int n) {
    /*
     * Iterative power calculation
     * Time: O(log n)
     * Space: O(1)
     */
    if (n < 0) {
        x = 1.0 / x;
        n = -n;
    }
    
    double result = 1.0;
    double current = x;
    
    while (n > 0) {
        if (n % 2 == 1) {
            result *= current;
        }
        current *= current;
        n /= 2;
    }
    
    return result;
}

// Example usage
int main() {
    printf("2^10 = %.0f\n", power_recursive(2, 10));  // 1024
    printf("3^5 = %.0f\n", power_recursive(3, 5));    // 243
    printf("2^-3 = %.3f\n", power_recursive(2, -3));  // 0.125
    return 0;
}
```

```cpp
// C++ Implementation
#include <iostream>
using namespace std;

class Power {
public:
    /*
     * Calculate x^n using divide and conquer
     * Time: O(log n)
     * Space: O(log n)
     */
    double powerRecursive(double x, int n) {
        // Base cases
        if (n == 0) return 1.0;
        if (n == 1) return x;
        if (n < 0) return 1.0 / powerRecursive(x, -n);
        
        // Divide
        double half = powerRecursive(x, n / 2);
        
        // Combine
        if (n % 2 == 0) {
            return half * half;
        } else {
            return half * half * x;
        }
    }
    
    /*
     * Iterative power calculation
     * Time: O(log n)
     * Space: O(1)
     */
    double powerIterative(double x, int n) {
        if (n < 0) {
            x = 1.0 / x;
            n = -n;
        }
        
        double result = 1.0;
        double current = x;
        
        while (n > 0) {
            if (n % 2 == 1) {
                result *= current;
            }
            current *= current;
            n /= 2;
        }
        
        return result;
    }
};

// Example usage
int main() {
    Power solver;
    cout << "2^10 = " << solver.powerRecursive(2, 10) << endl;  // 1024
    cout << "3^5 = " << solver.powerRecursive(3, 5) << endl;    // 243
    cout << "2^-3 = " << solver.powerRecursive(2, -3) << endl;  // 0.125
    return 0;
}
```

```java
// Java Implementation
public class Power {
    /*
     * Calculate x^n using divide and conquer
     * Time: O(log n)
     * Space: O(log n)
     */
    public double powerRecursive(double x, int n) {
        // Base cases
        if (n == 0) return 1.0;
        if (n == 1) return x;
        if (n < 0) return 1.0 / powerRecursive(x, -n);
        
        // Divide
        double half = powerRecursive(x, n / 2);
        
        // Combine
        if (n % 2 == 0) {
            return half * half;
        } else {
            return half * half * x;
        }
    }
    
    /*
     * Iterative power calculation
     * Time: O(log n)
     * Space: O(1)
     */
    public double powerIterative(double x, int n) {
        if (n < 0) {
            x = 1.0 / x;
            n = -n;
        }
        
        double result = 1.0;
        double current = x;
        
        while (n > 0) {
            if (n % 2 == 1) {
                result *= current;
            }
            current *= current;
            n /= 2;
        }
        
        return result;
    }
    
    // Example usage
    public static void main(String[] args) {
        Power solver = new Power();
        System.out.println("2^10 = " + solver.powerRecursive(2, 10));  // 1024
        System.out.println("3^5 = " + solver.powerRecursive(3, 5));    // 243
        System.out.println("2^-3 = " + solver.powerRecursive(2, -3));  // 0.125
    }
}
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

```python
# Exam template
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

```c
// C Exam template
void* divide_conquer(void* input) {
    // Base case
    if (is_small(input)) {
        return solve_directly(input);
    }
    
    // Divide
    int num_parts;
    void** parts = split(input, &num_parts);
    
    // Conquer (recurse)
    void** results = malloc(num_parts * sizeof(void*));
    for (int i = 0; i < num_parts; i++) {
        results[i] = divide_conquer(parts[i]);
    }
    
    // Combine
    void* result = merge_results(results, num_parts);
    
    free(parts);
    free(results);
    return result;
}
```

```cpp
// C++ Exam template
template<typename T>
T divideConquer(T input) {
    // Base case
    if (isSmall(input)) {
        return solveDirect(input);
    }
    
    // Divide
    vector<T> parts = split(input);
    
    // Conquer (recurse)
    vector<T> results;
    for (const auto& part : parts) {
        results.push_back(divideConquer(part));
    }
    
    // Combine
    return merge(results);
}
```

```java
// Java Exam template
public <T> T divideConquer(T input) {
    // Base case
    if (isSmall(input)) {
        return solveDirect(input);
    }
    
    // Divide
    List<T> parts = split(input);
    
    // Conquer (recurse)
    List<T> results = new ArrayList<>();
    for (T part : parts) {
        results.add(divideConquer(part));
    }
    
    // Combine
    return merge(results);
}
```

### Key Points
1. **Three steps**: Divide, Conquer, Combine
2. **Base case**: Essential for recursion termination
3. **Independent subproblems**: Unlike DP
4. **Master Theorem**: Quick complexity analysis
5. **Examples**: Merge Sort, Quick Sort, Binary Search
6. **Optimization**: Often achieves O(n log n)
