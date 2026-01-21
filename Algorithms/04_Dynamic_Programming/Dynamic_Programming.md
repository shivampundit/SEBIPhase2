# Dynamic Programming - Comprehensive Notes

## Overview
Dynamic Programming (DP) is an algorithmic technique for solving optimization problems by breaking them down into simpler subproblems. It stores results of subproblems to avoid redundant calculations.

### Key Characteristics
1. **Optimal Substructure**: Optimal solution contains optimal solutions to subproblems
2. **Overlapping Subproblems**: Same subproblems are solved multiple times
3. **Memoization/Tabulation**: Store and reuse solutions

---

## DP Approaches

### 1. Memoization (Top-Down)
- Start with original problem
- Recursively break into subproblems
- Store results in cache (usually dictionary/array)
- Return cached result if already computed

### 2. Tabulation (Bottom-Up)
- Start with smallest subproblems
- Build solution iteratively
- Store results in table (usually array)
- Compute larger problems using smaller ones

---

## Classic DP Problems

## 1. Fibonacci Sequence

### Problem
Find the nth Fibonacci number where F(n) = F(n-1) + F(n-2), F(0)=0, F(1)=1

### Naive Recursive (Exponential)
```python
def fibonacci_naive(n):
    """
    Time: O(2^n) - Exponential
    Space: O(n) - recursion stack
    """
    if n <= 1:
        return n
    return fibonacci_naive(n-1) + fibonacci_naive(n-2)

# Very slow for n > 40
```

```c
#include <stdio.h>

// Time: O(2^n) - Exponential
// Space: O(n) - recursion stack
int fibonacci_naive(int n) {
    if (n <= 1)
        return n;
    return fibonacci_naive(n-1) + fibonacci_naive(n-2);
}

// Very slow for n > 40
```

```cpp
#include <iostream>

// Time: O(2^n) - Exponential
// Space: O(n) - recursion stack
int fibonacci_naive(int n) {
    if (n <= 1)
        return n;
    return fibonacci_naive(n-1) + fibonacci_naive(n-2);
}

// Very slow for n > 40
```

```java
public class FibonacciNaive {
    /**
     * Time: O(2^n) - Exponential
     * Space: O(n) - recursion stack
     */
    public static int fibonacciNaive(int n) {
        if (n <= 1)
            return n;
        return fibonacciNaive(n-1) + fibonacciNaive(n-2);
    }
    
    // Very slow for n > 40
}
```

### Memoization Approach (Top-Down DP)
```python
def fibonacci_memo(n, memo={}):
    """
    Time: O(n)
    Space: O(n)
    """
    if n in memo:
        return memo[n]
    
    if n <= 1:
        return n
    
    memo[n] = fibonacci_memo(n-1, memo) + fibonacci_memo(n-2, memo)
    return memo[n]

# Example
print(fibonacci_memo(50))  # Fast even for large n
```

```c
#include <stdio.h>
#include <string.h>

#define MAX_N 1000

// Time: O(n)
// Space: O(n)
long long memo[MAX_N];

long long fibonacci_memo(int n) {
    if (n <= 1)
        return n;
    
    if (memo[n] != -1)
        return memo[n];
    
    memo[n] = fibonacci_memo(n-1) + fibonacci_memo(n-2);
    return memo[n];
}

// Initialize: memset(memo, -1, sizeof(memo));
// Example: printf("%lld\n", fibonacci_memo(50)); // Fast even for large n
```

```cpp
#include <iostream>
#include <unordered_map>

// Time: O(n)
// Space: O(n)
long long fibonacci_memo(int n, std::unordered_map<int, long long>& memo) {
    if (n <= 1)
        return n;
    
    if (memo.find(n) != memo.end())
        return memo[n];
    
    memo[n] = fibonacci_memo(n-1, memo) + fibonacci_memo(n-2, memo);
    return memo[n];
}

// Example
// std::unordered_map<int, long long> memo;
// std::cout << fibonacci_memo(50, memo) << std::endl; // Fast even for large n
```

```java
import java.util.HashMap;
import java.util.Map;

public class FibonacciMemo {
    /**
     * Time: O(n)
     * Space: O(n)
     */
    public static long fibonacciMemo(int n, Map<Integer, Long> memo) {
        if (n <= 1)
            return n;
        
        if (memo.containsKey(n))
            return memo.get(n);
        
        long result = fibonacciMemo(n-1, memo) + fibonacciMemo(n-2, memo);
        memo.put(n, result);
        return result;
    }
    
    // Example
    // Map<Integer, Long> memo = new HashMap<>();
    // System.out.println(fibonacciMemo(50, memo)); // Fast even for large n
}
```

### Tabulation Approach (Bottom-Up DP)
```python
def fibonacci_tab(n):
    """
    Time: O(n)
    Space: O(n)
    """
    if n <= 1:
        return n
    
    dp = [0] * (n + 1)
    dp[1] = 1
    
    for i in range(2, n + 1):
        dp[i] = dp[i-1] + dp[i-2]
    
    return dp[n]

print(fibonacci_tab(50))
```

```c
#include <stdio.h>
#include <stdlib.h>

// Time: O(n)
// Space: O(n)
long long fibonacci_tab(int n) {
    if (n <= 1)
        return n;
    
    long long *dp = (long long *)calloc(n + 1, sizeof(long long));
    dp[1] = 1;
    
    for (int i = 2; i <= n; i++) {
        dp[i] = dp[i-1] + dp[i-2];
    }
    
    long long result = dp[n];
    free(dp);
    return result;
}

// printf("%lld\n", fibonacci_tab(50));
```

```cpp
#include <iostream>
#include <vector>

// Time: O(n)
// Space: O(n)
long long fibonacci_tab(int n) {
    if (n <= 1)
        return n;
    
    std::vector<long long> dp(n + 1, 0);
    dp[1] = 1;
    
    for (int i = 2; i <= n; i++) {
        dp[i] = dp[i-1] + dp[i-2];
    }
    
    return dp[n];
}

// std::cout << fibonacci_tab(50) << std::endl;
```

```java
public class FibonacciTab {
    /**
     * Time: O(n)
     * Space: O(n)
     */
    public static long fibonacciTab(int n) {
        if (n <= 1)
            return n;
        
        long[] dp = new long[n + 1];
        dp[1] = 1;
        
        for (int i = 2; i <= n; i++) {
            dp[i] = dp[i-1] + dp[i-2];
        }
        
        return dp[n];
    }
    
    // System.out.println(fibonacciTab(50));
}
```

### Space Optimized
```python
def fibonacci_optimized(n):
    """
    Time: O(n)
    Space: O(1)
    """
    if n <= 1:
        return n
    
    prev2, prev1 = 0, 1
    
    for i in range(2, n + 1):
        current = prev1 + prev2
        prev2 = prev1
        prev1 = current
    
    return prev1

print(fibonacci_optimized(50))
```

```c
#include <stdio.h>

// Time: O(n)
// Space: O(1)
long long fibonacci_optimized(int n) {
    if (n <= 1)
        return n;
    
    long long prev2 = 0, prev1 = 1;
    
    for (int i = 2; i <= n; i++) {
        long long current = prev1 + prev2;
        prev2 = prev1;
        prev1 = current;
    }
    
    return prev1;
}

// printf("%lld\n", fibonacci_optimized(50));
```

```cpp
#include <iostream>

// Time: O(n)
// Space: O(1)
long long fibonacci_optimized(int n) {
    if (n <= 1)
        return n;
    
    long long prev2 = 0, prev1 = 1;
    
    for (int i = 2; i <= n; i++) {
        long long current = prev1 + prev2;
        prev2 = prev1;
        prev1 = current;
    }
    
    return prev1;
}

// std::cout << fibonacci_optimized(50) << std::endl;
```

```java
public class FibonacciOptimized {
    /**
     * Time: O(n)
     * Space: O(1)
     */
    public static long fibonacciOptimized(int n) {
        if (n <= 1)
            return n;
        
        long prev2 = 0, prev1 = 1;
        
        for (int i = 2; i <= n; i++) {
            long current = prev1 + prev2;
            prev2 = prev1;
            prev1 = current;
        }
        
        return prev1;
    }
    
    // System.out.println(fibonacciOptimized(50));
}
```

### Dry Run (Tabulation, n=6)
```
dp = [0, 1, 0, 0, 0, 0, 0]

i=2: dp[2] = dp[1] + dp[0] = 1 + 0 = 1
dp = [0, 1, 1, 0, 0, 0, 0]

i=3: dp[3] = dp[2] + dp[1] = 1 + 1 = 2
dp = [0, 1, 1, 2, 0, 0, 0]

i=4: dp[4] = dp[3] + dp[2] = 2 + 1 = 3
dp = [0, 1, 1, 2, 3, 0, 0]

i=5: dp[5] = dp[4] + dp[3] = 3 + 2 = 5
dp = [0, 1, 1, 2, 3, 5, 0]

i=6: dp[6] = dp[5] + dp[4] = 5 + 3 = 8
dp = [0, 1, 1, 2, 3, 5, 8]

Return: 8
```

---

## 2. Longest Common Subsequence (LCS)

### Problem
Find the length of longest subsequence common to two strings.

### Recursive Solution
```python
def lcs_recursive(s1, s2, m, n):
    """
    Time: O(2^(m+n))
    Space: O(m+n)
    """
    if m == 0 or n == 0:
        return 0
    
    if s1[m-1] == s2[n-1]:
        return 1 + lcs_recursive(s1, s2, m-1, n-1)
    else:
        return max(lcs_recursive(s1, s2, m-1, n),
                   lcs_recursive(s1, s2, m, n-1))
```

```c
#include <stdio.h>
#include <string.h>

// Time: O(2^(m+n))
// Space: O(m+n)
int max(int a, int b) {
    return (a > b) ? a : b;
}

int lcs_recursive(char *s1, char *s2, int m, int n) {
    if (m == 0 || n == 0)
        return 0;
    
    if (s1[m-1] == s2[n-1])
        return 1 + lcs_recursive(s1, s2, m-1, n-1);
    else
        return max(lcs_recursive(s1, s2, m-1, n),
                   lcs_recursive(s1, s2, m, n-1));
}
```

```cpp
#include <iostream>
#include <string>
#include <algorithm>

// Time: O(2^(m+n))
// Space: O(m+n)
int lcs_recursive(std::string s1, std::string s2, int m, int n) {
    if (m == 0 || n == 0)
        return 0;
    
    if (s1[m-1] == s2[n-1])
        return 1 + lcs_recursive(s1, s2, m-1, n-1);
    else
        return std::max(lcs_recursive(s1, s2, m-1, n),
                        lcs_recursive(s1, s2, m, n-1));
}
```

```java
public class LCSRecursive {
    /**
     * Time: O(2^(m+n))
     * Space: O(m+n)
     */
    public static int lcsRecursive(String s1, String s2, int m, int n) {
        if (m == 0 || n == 0)
            return 0;
        
        if (s1.charAt(m-1) == s2.charAt(n-1))
            return 1 + lcsRecursive(s1, s2, m-1, n-1);
        else
            return Math.max(lcsRecursive(s1, s2, m-1, n),
                           lcsRecursive(s1, s2, m, n-1));
    }
}
```

### DP Solution (Tabulation)
```python
def lcs_dp(s1, s2):
    """
    Time: O(m*n)
    Space: O(m*n)
    """
    m, n = len(s1), len(s2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if s1[i-1] == s2[j-1]:
                dp[i][j] = 1 + dp[i-1][j-1]
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
    
    return dp[m][n]

# Example
s1 = "ABCDGH"
s2 = "AEDFHR"
print(f"LCS length: {lcs_dp(s1, s2)}")  # Output: 3 (ADH)
```

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

// Time: O(m*n)
// Space: O(m*n)
int max(int a, int b) {
    return (a > b) ? a : b;
}

int lcs_dp(char *s1, char *s2) {
    int m = strlen(s1);
    int n = strlen(s2);
    
    // Allocate 2D array
    int **dp = (int **)malloc((m + 1) * sizeof(int *));
    for (int i = 0; i <= m; i++)
        dp[i] = (int *)calloc(n + 1, sizeof(int));
    
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (s1[i-1] == s2[j-1])
                dp[i][j] = 1 + dp[i-1][j-1];
            else
                dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
        }
    }
    
    int result = dp[m][n];
    
    // Free memory
    for (int i = 0; i <= m; i++)
        free(dp[i]);
    free(dp);
    
    return result;
}

// Example
// char s1[] = "ABCDGH";
// char s2[] = "AEDFHR";
// printf("LCS length: %d\n", lcs_dp(s1, s2));  // Output: 3 (ADH)
```

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

// Time: O(m*n)
// Space: O(m*n)
int lcs_dp(std::string s1, std::string s2) {
    int m = s1.length();
    int n = s2.length();
    std::vector<std::vector<int>> dp(m + 1, std::vector<int>(n + 1, 0));
    
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (s1[i-1] == s2[j-1])
                dp[i][j] = 1 + dp[i-1][j-1];
            else
                dp[i][j] = std::max(dp[i-1][j], dp[i][j-1]);
        }
    }
    
    return dp[m][n];
}

// Example
// std::string s1 = "ABCDGH";
// std::string s2 = "AEDFHR";
// std::cout << "LCS length: " << lcs_dp(s1, s2) << std::endl;  // Output: 3 (ADH)
```

```java
public class LCSDP {
    /**
     * Time: O(m*n)
     * Space: O(m*n)
     */
    public static int lcsDp(String s1, String s2) {
        int m = s1.length();
        int n = s2.length();
        int[][] dp = new int[m + 1][n + 1];
        
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (s1.charAt(i-1) == s2.charAt(j-1))
                    dp[i][j] = 1 + dp[i-1][j-1];
                else
                    dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
            }
        }
        
        return dp[m][n];
    }
    
    // Example
    // String s1 = "ABCDGH";
    // String s2 = "AEDFHR";
    // System.out.println("LCS length: " + lcsDp(s1, s2));  // Output: 3 (ADH)
}
```

### Finding the LCS String
```python
def lcs_string(s1, s2):
    """
    Returns the actual LCS string
    """
    m, n = len(s1), len(s2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    
    # Fill DP table
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if s1[i-1] == s2[j-1]:
                dp[i][j] = 1 + dp[i-1][j-1]
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
    
    # Backtrack to find LCS string
    lcs = []
    i, j = m, n
    
    while i > 0 and j > 0:
        if s1[i-1] == s2[j-1]:
            lcs.append(s1[i-1])
            i -= 1
            j -= 1
        elif dp[i-1][j] > dp[i][j-1]:
            i -= 1
        else:
            j -= 1
    
    return ''.join(reversed(lcs))

print(lcs_string("ABCDGH", "AEDFHR"))  # Output: "ADH"
```

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

// Returns the actual LCS string
int max(int a, int b) {
    return (a > b) ? a : b;
}

char* lcs_string(char *s1, char *s2) {
    int m = strlen(s1);
    int n = strlen(s2);
    
    // Allocate 2D array
    int **dp = (int **)malloc((m + 1) * sizeof(int *));
    for (int i = 0; i <= m; i++)
        dp[i] = (int *)calloc(n + 1, sizeof(int));
    
    // Fill DP table
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (s1[i-1] == s2[j-1])
                dp[i][j] = 1 + dp[i-1][j-1];
            else
                dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
        }
    }
    
    // Backtrack to find LCS string
    char *lcs = (char *)malloc((dp[m][n] + 1) * sizeof(char));
    int index = dp[m][n];
    lcs[index] = '\0';
    
    int i = m, j = n;
    while (i > 0 && j > 0) {
        if (s1[i-1] == s2[j-1]) {
            lcs[--index] = s1[i-1];
            i--;
            j--;
        } else if (dp[i-1][j] > dp[i][j-1]) {
            i--;
        } else {
            j--;
        }
    }
    
    // Free memory
    for (int i = 0; i <= m; i++)
        free(dp[i]);
    free(dp);
    
    return lcs;
}

// printf("%s\n", lcs_string("ABCDGH", "AEDFHR"));  // Output: "ADH"
```

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

// Returns the actual LCS string
std::string lcs_string(std::string s1, std::string s2) {
    int m = s1.length();
    int n = s2.length();
    std::vector<std::vector<int>> dp(m + 1, std::vector<int>(n + 1, 0));
    
    // Fill DP table
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (s1[i-1] == s2[j-1])
                dp[i][j] = 1 + dp[i-1][j-1];
            else
                dp[i][j] = std::max(dp[i-1][j], dp[i][j-1]);
        }
    }
    
    // Backtrack to find LCS string
    std::string lcs = "";
    int i = m, j = n;
    
    while (i > 0 && j > 0) {
        if (s1[i-1] == s2[j-1]) {
            lcs = s1[i-1] + lcs;
            i--;
            j--;
        } else if (dp[i-1][j] > dp[i][j-1]) {
            i--;
        } else {
            j--;
        }
    }
    
    return lcs;
}

// std::cout << lcs_string("ABCDGH", "AEDFHR") << std::endl;  // Output: "ADH"
```

```java
public class LCSString {
    /**
     * Returns the actual LCS string
     */
    public static String lcsString(String s1, String s2) {
        int m = s1.length();
        int n = s2.length();
        int[][] dp = new int[m + 1][n + 1];
        
        // Fill DP table
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (s1.charAt(i-1) == s2.charAt(j-1))
                    dp[i][j] = 1 + dp[i-1][j-1];
                else
                    dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
            }
        }
        
        // Backtrack to find LCS string
        StringBuilder lcs = new StringBuilder();
        int i = m, j = n;
        
        while (i > 0 && j > 0) {
            if (s1.charAt(i-1) == s2.charAt(j-1)) {
                lcs.append(s1.charAt(i-1));
                i--;
                j--;
            } else if (dp[i-1][j] > dp[i][j-1]) {
                i--;
            } else {
                j--;
            }
        }
        
        return lcs.reverse().toString();
    }
    
    // System.out.println(lcsString("ABCDGH", "AEDFHR"));  // Output: "ADH"
}
```

### Dry Run (s1="AGGTAB", s2="GXTXAYB")
```
DP Table:
    ""  G  X  T  X  A  Y  B
""   0  0  0  0  0  0  0  0
A    0  0  0  0  0  1  1  1
G    0  1  1  1  1  1  1  1
G    0  1  1  1  1  1  1  1
T    0  1  1  2  2  2  2  2
A    0  1  1  2  2  3  3  3
B    0  1  1  2  2  3  3  4

LCS Length: 4
LCS String: "GTAB"
```

---

## 3. 0/1 Knapsack Problem

### Problem
Given weights and values of n items and a knapsack capacity, maximize value without exceeding capacity. Can't take fractions.

### Recursive Solution
```python
def knapsack_recursive(weights, values, capacity, n):
    """
    Time: O(2^n)
    Space: O(n)
    """
    if n == 0 or capacity == 0:
        return 0
    
    # If weight exceeds capacity, skip item
    if weights[n-1] > capacity:
        return knapsack_recursive(weights, values, capacity, n-1)
    
    # Max of including or excluding current item
    include = values[n-1] + knapsack_recursive(
        weights, values, capacity - weights[n-1], n-1)
    exclude = knapsack_recursive(weights, values, capacity, n-1)
    
    return max(include, exclude)
```

```c
#include <stdio.h>

// Time: O(2^n)
// Space: O(n)
int max(int a, int b) {
    return (a > b) ? a : b;
}

int knapsack_recursive(int weights[], int values[], int capacity, int n) {
    if (n == 0 || capacity == 0)
        return 0;
    
    // If weight exceeds capacity, skip item
    if (weights[n-1] > capacity)
        return knapsack_recursive(weights, values, capacity, n-1);
    
    // Max of including or excluding current item
    int include = values[n-1] + knapsack_recursive(
        weights, values, capacity - weights[n-1], n-1);
    int exclude = knapsack_recursive(weights, values, capacity, n-1);
    
    return max(include, exclude);
}
```

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

// Time: O(2^n)
// Space: O(n)
int knapsack_recursive(std::vector<int>& weights, std::vector<int>& values, 
                      int capacity, int n) {
    if (n == 0 || capacity == 0)
        return 0;
    
    // If weight exceeds capacity, skip item
    if (weights[n-1] > capacity)
        return knapsack_recursive(weights, values, capacity, n-1);
    
    // Max of including or excluding current item
    int include = values[n-1] + knapsack_recursive(
        weights, values, capacity - weights[n-1], n-1);
    int exclude = knapsack_recursive(weights, values, capacity, n-1);
    
    return std::max(include, exclude);
}
```

```java
public class KnapsackRecursive {
    /**
     * Time: O(2^n)
     * Space: O(n)
     */
    public static int knapsackRecursive(int[] weights, int[] values, 
                                       int capacity, int n) {
        if (n == 0 || capacity == 0)
            return 0;
        
        // If weight exceeds capacity, skip item
        if (weights[n-1] > capacity)
            return knapsackRecursive(weights, values, capacity, n-1);
        
        // Max of including or excluding current item
        int include = values[n-1] + knapsackRecursive(
            weights, values, capacity - weights[n-1], n-1);
        int exclude = knapsackRecursive(weights, values, capacity, n-1);
        
        return Math.max(include, exclude);
    }
}
```

### DP Solution (Tabulation)
```python
def knapsack_dp(weights, values, capacity):
    """
    Time: O(n * capacity)
    Space: O(n * capacity)
    """
    n = len(weights)
    dp = [[0] * (capacity + 1) for _ in range(n + 1)]
    
    for i in range(1, n + 1):
        for w in range(1, capacity + 1):
            if weights[i-1] <= w:
                # Max of including or excluding
                dp[i][w] = max(
                    values[i-1] + dp[i-1][w - weights[i-1]],
                    dp[i-1][w]
                )
            else:
                # Can't include, take previous
                dp[i][w] = dp[i-1][w]
    
    return dp[n][capacity]

# Example
weights = [1, 3, 4, 5]
values = [1, 4, 5, 7]
capacity = 7
print(f"Maximum value: {knapsack_dp(weights, values, capacity)}")  # 9
```

```c
#include <stdio.h>
#include <stdlib.h>

// Time: O(n * capacity)
// Space: O(n * capacity)
int max(int a, int b) {
    return (a > b) ? a : b;
}

int knapsack_dp(int weights[], int values[], int n, int capacity) {
    // Allocate 2D array
    int **dp = (int **)malloc((n + 1) * sizeof(int *));
    for (int i = 0; i <= n; i++)
        dp[i] = (int *)calloc(capacity + 1, sizeof(int));
    
    for (int i = 1; i <= n; i++) {
        for (int w = 1; w <= capacity; w++) {
            if (weights[i-1] <= w) {
                // Max of including or excluding
                dp[i][w] = max(
                    values[i-1] + dp[i-1][w - weights[i-1]],
                    dp[i-1][w]
                );
            } else {
                // Can't include, take previous
                dp[i][w] = dp[i-1][w];
            }
        }
    }
    
    int result = dp[n][capacity];
    
    // Free memory
    for (int i = 0; i <= n; i++)
        free(dp[i]);
    free(dp);
    
    return result;
}

// Example
// int weights[] = {1, 3, 4, 5};
// int values[] = {1, 4, 5, 7};
// int capacity = 7;
// printf("Maximum value: %d\n", knapsack_dp(weights, values, 4, capacity));  // 9
```

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

// Time: O(n * capacity)
// Space: O(n * capacity)
int knapsack_dp(std::vector<int>& weights, std::vector<int>& values, int capacity) {
    int n = weights.size();
    std::vector<std::vector<int>> dp(n + 1, std::vector<int>(capacity + 1, 0));
    
    for (int i = 1; i <= n; i++) {
        for (int w = 1; w <= capacity; w++) {
            if (weights[i-1] <= w) {
                // Max of including or excluding
                dp[i][w] = std::max(
                    values[i-1] + dp[i-1][w - weights[i-1]],
                    dp[i-1][w]
                );
            } else {
                // Can't include, take previous
                dp[i][w] = dp[i-1][w];
            }
        }
    }
    
    return dp[n][capacity];
}

// Example
// std::vector<int> weights = {1, 3, 4, 5};
// std::vector<int> values = {1, 4, 5, 7};
// int capacity = 7;
// std::cout << "Maximum value: " << knapsack_dp(weights, values, capacity) << std::endl;  // 9
```

```java
public class KnapsackDP {
    /**
     * Time: O(n * capacity)
     * Space: O(n * capacity)
     */
    public static int knapsackDp(int[] weights, int[] values, int capacity) {
        int n = weights.length;
        int[][] dp = new int[n + 1][capacity + 1];
        
        for (int i = 1; i <= n; i++) {
            for (int w = 1; w <= capacity; w++) {
                if (weights[i-1] <= w) {
                    // Max of including or excluding
                    dp[i][w] = Math.max(
                        values[i-1] + dp[i-1][w - weights[i-1]],
                        dp[i-1][w]
                    );
                } else {
                    // Can't include, take previous
                    dp[i][w] = dp[i-1][w];
                }
            }
        }
        
        return dp[n][capacity];
    }
    
    // Example
    // int[] weights = {1, 3, 4, 5};
    // int[] values = {1, 4, 5, 7};
    // int capacity = 7;
    // System.out.println("Maximum value: " + knapsackDp(weights, values, capacity));  // 9
}
```

### Finding Selected Items
```python
def knapsack_items(weights, values, capacity):
    """
    Returns max value and list of selected items
    """
    n = len(weights)
    dp = [[0] * (capacity + 1) for _ in range(n + 1)]
    
    # Fill DP table
    for i in range(1, n + 1):
        for w in range(1, capacity + 1):
            if weights[i-1] <= w:
                dp[i][w] = max(
                    values[i-1] + dp[i-1][w - weights[i-1]],
                    dp[i-1][w]
                )
            else:
                dp[i][w] = dp[i-1][w]
    
    # Backtrack to find items
    selected = []
    w = capacity
    
    for i in range(n, 0, -1):
        if dp[i][w] != dp[i-1][w]:
            selected.append(i-1)
            w -= weights[i-1]
    
    return dp[n][capacity], selected[::-1]

max_val, items = knapsack_items(weights, values, capacity)
print(f"Max value: {max_val}, Items: {items}")
```

```c
#include <stdio.h>
#include <stdlib.h>

// Returns max value and list of selected items
typedef struct {
    int max_value;
    int *items;
    int count;
} KnapsackResult;

int max(int a, int b) {
    return (a > b) ? a : b;
}

KnapsackResult knapsack_items(int weights[], int values[], int n, int capacity) {
    // Allocate 2D array
    int **dp = (int **)malloc((n + 1) * sizeof(int *));
    for (int i = 0; i <= n; i++)
        dp[i] = (int *)calloc(capacity + 1, sizeof(int));
    
    // Fill DP table
    for (int i = 1; i <= n; i++) {
        for (int w = 1; w <= capacity; w++) {
            if (weights[i-1] <= w) {
                dp[i][w] = max(
                    values[i-1] + dp[i-1][w - weights[i-1]],
                    dp[i-1][w]
                );
            } else {
                dp[i][w] = dp[i-1][w];
            }
        }
    }
    
    // Backtrack to find items
    int *selected = (int *)malloc(n * sizeof(int));
    int count = 0;
    int w = capacity;
    
    for (int i = n; i > 0; i--) {
        if (dp[i][w] != dp[i-1][w]) {
            selected[count++] = i-1;
            w -= weights[i-1];
        }
    }
    
    // Reverse the selected array
    for (int i = 0; i < count / 2; i++) {
        int temp = selected[i];
        selected[i] = selected[count - 1 - i];
        selected[count - 1 - i] = temp;
    }
    
    KnapsackResult result;
    result.max_value = dp[n][capacity];
    result.items = selected;
    result.count = count;
    
    // Free memory
    for (int i = 0; i <= n; i++)
        free(dp[i]);
    free(dp);
    
    return result;
}

// KnapsackResult result = knapsack_items(weights, values, n, capacity);
// printf("Max value: %d, Items: ", result.max_value);
// for (int i = 0; i < result.count; i++)
//     printf("%d ", result.items[i]);
// free(result.items);
```

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

// Returns max value and list of selected items
std::pair<int, std::vector<int>> knapsack_items(std::vector<int>& weights, 
                                                 std::vector<int>& values, 
                                                 int capacity) {
    int n = weights.size();
    std::vector<std::vector<int>> dp(n + 1, std::vector<int>(capacity + 1, 0));
    
    // Fill DP table
    for (int i = 1; i <= n; i++) {
        for (int w = 1; w <= capacity; w++) {
            if (weights[i-1] <= w) {
                dp[i][w] = std::max(
                    values[i-1] + dp[i-1][w - weights[i-1]],
                    dp[i-1][w]
                );
            } else {
                dp[i][w] = dp[i-1][w];
            }
        }
    }
    
    // Backtrack to find items
    std::vector<int> selected;
    int w = capacity;
    
    for (int i = n; i > 0; i--) {
        if (dp[i][w] != dp[i-1][w]) {
            selected.push_back(i-1);
            w -= weights[i-1];
        }
    }
    
    std::reverse(selected.begin(), selected.end());
    return {dp[n][capacity], selected};
}

// auto [max_val, items] = knapsack_items(weights, values, capacity);
// std::cout << "Max value: " << max_val << ", Items: ";
// for (int item : items) std::cout << item << " ";
```

```java
import java.util.*;

public class KnapsackItems {
    /**
     * Returns max value and list of selected items
     */
    public static class Result {
        int maxValue;
        List<Integer> items;
        
        Result(int maxValue, List<Integer> items) {
            this.maxValue = maxValue;
            this.items = items;
        }
    }
    
    public static Result knapsackItems(int[] weights, int[] values, int capacity) {
        int n = weights.length;
        int[][] dp = new int[n + 1][capacity + 1];
        
        // Fill DP table
        for (int i = 1; i <= n; i++) {
            for (int w = 1; w <= capacity; w++) {
                if (weights[i-1] <= w) {
                    dp[i][w] = Math.max(
                        values[i-1] + dp[i-1][w - weights[i-1]],
                        dp[i-1][w]
                    );
                } else {
                    dp[i][w] = dp[i-1][w];
                }
            }
        }
        
        // Backtrack to find items
        List<Integer> selected = new ArrayList<>();
        int w = capacity;
        
        for (int i = n; i > 0; i--) {
            if (dp[i][w] != dp[i-1][w]) {
                selected.add(i-1);
                w -= weights[i-1];
            }
        }
        
        Collections.reverse(selected);
        return new Result(dp[n][capacity], selected);
    }
    
    // Result result = knapsackItems(weights, values, capacity);
    // System.out.println("Max value: " + result.maxValue + ", Items: " + result.items);
}
```

### Dry Run (weights=[2,1,3,2], values=[12,10,20,15], capacity=5)
```
DP Table:
     0  1  2  3  4  5
0    0  0  0  0  0  0
1    0  0 12 12 12 12  (item 0: w=2, v=12)
2    0 10 12 22 22 22  (item 1: w=1, v=10)
3    0 10 12 22 30 32  (item 2: w=3, v=20)
4    0 10 12 22 30 37  (item 3: w=2, v=15)

Maximum Value: 37
Selected Items: [1, 3] (weights: 1+2=3, values: 10+15+12=37)
Wait, recalculate: Items 0,1,3: weights=2+1+2=5, values=12+10+15=37 ✓
```

---

## 4. Coin Change Problem

### Problem 1: Minimum Coins
Given coin denominations and amount, find minimum number of coins needed.

```python
def coin_change_min(coins, amount):
    """
    Time: O(amount * n)
    Space: O(amount)
    """
    dp = [float('inf')] * (amount + 1)
    dp[0] = 0
    
    for i in range(1, amount + 1):
        for coin in coins:
            if coin <= i:
                dp[i] = min(dp[i], dp[i - coin] + 1)
    
    return dp[amount] if dp[amount] != float('inf') else -1

# Example
coins = [1, 5, 10, 25]
amount = 63
print(f"Minimum coins: {coin_change_min(coins, amount)}")  # 6
```

```c
#include <stdio.h>
#include <limits.h>

// Time: O(amount * n)
// Space: O(amount)
int min(int a, int b) {
    return (a < b) ? a : b;
}

int coin_change_min(int coins[], int n, int amount) {
    int *dp = (int *)malloc((amount + 1) * sizeof(int));
    
    dp[0] = 0;
    for (int i = 1; i <= amount; i++)
        dp[i] = INT_MAX;
    
    for (int i = 1; i <= amount; i++) {
        for (int j = 0; j < n; j++) {
            if (coins[j] <= i && dp[i - coins[j]] != INT_MAX) {
                dp[i] = min(dp[i], dp[i - coins[j]] + 1);
            }
        }
    }
    
    int result = (dp[amount] == INT_MAX) ? -1 : dp[amount];
    free(dp);
    return result;
}

// Example
// int coins[] = {1, 5, 10, 25};
// int amount = 63;
// printf("Minimum coins: %d\n", coin_change_min(coins, 4, amount));  // 6
```

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <limits>

// Time: O(amount * n)
// Space: O(amount)
int coin_change_min(std::vector<int>& coins, int amount) {
    std::vector<int> dp(amount + 1, INT_MAX);
    dp[0] = 0;
    
    for (int i = 1; i <= amount; i++) {
        for (int coin : coins) {
            if (coin <= i && dp[i - coin] != INT_MAX) {
                dp[i] = std::min(dp[i], dp[i - coin] + 1);
            }
        }
    }
    
    return dp[amount] == INT_MAX ? -1 : dp[amount];
}

// Example
// std::vector<int> coins = {1, 5, 10, 25};
// int amount = 63;
// std::cout << "Minimum coins: " << coin_change_min(coins, amount) << std::endl;  // 6
```

```java
public class CoinChangeMin {
    /**
     * Time: O(amount * n)
     * Space: O(amount)
     */
    public static int coinChangeMin(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        
        dp[0] = 0;
        for (int i = 1; i <= amount; i++)
            dp[i] = Integer.MAX_VALUE;
        
        for (int i = 1; i <= amount; i++) {
            for (int coin : coins) {
                if (coin <= i && dp[i - coin] != Integer.MAX_VALUE) {
                    dp[i] = Math.min(dp[i], dp[i - coin] + 1);
                }
            }
        }
        
        return dp[amount] == Integer.MAX_VALUE ? -1 : dp[amount];
    }
    
    // Example
    // int[] coins = {1, 5, 10, 25};
    // int amount = 63;
    // System.out.println("Minimum coins: " + coinChangeMin(coins, amount));  // 6
}
```

### Problem 2: Number of Ways
Count number of ways to make amount using coins (infinite supply).

```python
def coin_change_ways(coins, amount):
    """
    Time: O(amount * n)
    Space: O(amount)
    """
    dp = [0] * (amount + 1)
    dp[0] = 1  # One way to make 0
    
    for coin in coins:
        for i in range(coin, amount + 1):
            dp[i] += dp[i - coin]
    
    return dp[amount]

# Example
coins = [1, 2, 5]
amount = 5
print(f"Number of ways: {coin_change_ways(coins, amount)}")  # 4
# Ways: {5}, {2,2,1}, {2,1,1,1}, {1,1,1,1,1}
```

```c
#include <stdio.h>
#include <stdlib.h>

// Time: O(amount * n)
// Space: O(amount)
int coin_change_ways(int coins[], int n, int amount) {
    int *dp = (int *)calloc(amount + 1, sizeof(int));
    dp[0] = 1;  // One way to make 0
    
    for (int i = 0; i < n; i++) {
        for (int j = coins[i]; j <= amount; j++) {
            dp[j] += dp[j - coins[i]];
        }
    }
    
    int result = dp[amount];
    free(dp);
    return result;
}

// Example
// int coins[] = {1, 2, 5};
// int amount = 5;
// printf("Number of ways: %d\n", coin_change_ways(coins, 3, amount));  // 4
// Ways: {5}, {2,2,1}, {2,1,1,1}, {1,1,1,1,1}
```

```cpp
#include <iostream>
#include <vector>

// Time: O(amount * n)
// Space: O(amount)
int coin_change_ways(std::vector<int>& coins, int amount) {
    std::vector<int> dp(amount + 1, 0);
    dp[0] = 1;  // One way to make 0
    
    for (int coin : coins) {
        for (int i = coin; i <= amount; i++) {
            dp[i] += dp[i - coin];
        }
    }
    
    return dp[amount];
}

// Example
// std::vector<int> coins = {1, 2, 5};
// int amount = 5;
// std::cout << "Number of ways: " << coin_change_ways(coins, amount) << std::endl;  // 4
// Ways: {5}, {2,2,1}, {2,1,1,1}, {1,1,1,1,1}
```

```java
public class CoinChangeWays {
    /**
     * Time: O(amount * n)
     * Space: O(amount)
     */
    public static int coinChangeWays(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        dp[0] = 1;  // One way to make 0
        
        for (int coin : coins) {
            for (int i = coin; i <= amount; i++) {
                dp[i] += dp[i - coin];
            }
        }
        
        return dp[amount];
    }
    
    // Example
    // int[] coins = {1, 2, 5};
    // int amount = 5;
    // System.out.println("Number of ways: " + coinChangeWays(coins, amount));  // 4
    // Ways: {5}, {2,2,1}, {2,1,1,1}, {1,1,1,1,1}
}
```

### Dry Run (coins=[1,2,3], amount=4)
```
Minimum Coins:
dp = [0, inf, inf, inf, inf]

For amount 1:
  coin=1: dp[1] = min(inf, dp[0]+1) = 1
  dp = [0, 1, inf, inf, inf]

For amount 2:
  coin=1: dp[2] = min(inf, dp[1]+1) = 2
  coin=2: dp[2] = min(2, dp[0]+1) = 1
  dp = [0, 1, 1, inf, inf]

For amount 3:
  coin=1: dp[3] = min(inf, dp[2]+1) = 2
  coin=2: dp[3] = min(2, dp[1]+1) = 2
  coin=3: dp[3] = min(2, dp[0]+1) = 1
  dp = [0, 1, 1, 1, inf]

For amount 4:
  coin=1: dp[4] = min(inf, dp[3]+1) = 2
  coin=2: dp[4] = min(2, dp[2]+1) = 2
  coin=3: dp[4] = min(2, dp[1]+1) = 2
  dp = [0, 1, 1, 1, 2]

Minimum: 2 coins (2+2 or 3+1)
```

---

## 5. Longest Increasing Subsequence (LIS)

### Problem
Find length of longest strictly increasing subsequence.

### DP Solution O(n²)
```python
def lis_dp(arr):
    """
    Time: O(n²)
    Space: O(n)
    """
    n = len(arr)
    dp = [1] * n  # Each element is LIS of length 1
    
    for i in range(1, n):
        for j in range(i):
            if arr[j] < arr[i]:
                dp[i] = max(dp[i], dp[j] + 1)
    
    return max(dp)

# Example
arr = [10, 9, 2, 5, 3, 7, 101, 18]
print(f"LIS length: {lis_dp(arr)}")  # 4: [2,3,7,18] or [2,5,7,18]
```

```c
#include <stdio.h>
#include <stdlib.h>

// Time: O(n²)
// Space: O(n)
int max(int a, int b) {
    return (a > b) ? a : b;
}

int lis_dp(int arr[], int n) {
    int *dp = (int *)malloc(n * sizeof(int));
    
    // Each element is LIS of length 1
    for (int i = 0; i < n; i++)
        dp[i] = 1;
    
    for (int i = 1; i < n; i++) {
        for (int j = 0; j < i; j++) {
            if (arr[j] < arr[i]) {
                dp[i] = max(dp[i], dp[j] + 1);
            }
        }
    }
    
    // Find maximum in dp array
    int result = dp[0];
    for (int i = 1; i < n; i++)
        result = max(result, dp[i]);
    
    free(dp);
    return result;
}

// Example
// int arr[] = {10, 9, 2, 5, 3, 7, 101, 18};
// printf("LIS length: %d\n", lis_dp(arr, 8));  // 4: [2,3,7,18] or [2,5,7,18]
```

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

// Time: O(n²)
// Space: O(n)
int lis_dp(std::vector<int>& arr) {
    int n = arr.size();
    std::vector<int> dp(n, 1);  // Each element is LIS of length 1
    
    for (int i = 1; i < n; i++) {
        for (int j = 0; j < i; j++) {
            if (arr[j] < arr[i]) {
                dp[i] = std::max(dp[i], dp[j] + 1);
            }
        }
    }
    
    return *std::max_element(dp.begin(), dp.end());
}

// Example
// std::vector<int> arr = {10, 9, 2, 5, 3, 7, 101, 18};
// std::cout << "LIS length: " << lis_dp(arr) << std::endl;  // 4: [2,3,7,18] or [2,5,7,18]
```

```java
public class LISDP {
    /**
     * Time: O(n²)
     * Space: O(n)
     */
    public static int lisDp(int[] arr) {
        int n = arr.length;
        int[] dp = new int[n];
        
        // Each element is LIS of length 1
        for (int i = 0; i < n; i++)
            dp[i] = 1;
        
        for (int i = 1; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (arr[j] < arr[i]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
        }
        
        // Find maximum in dp array
        int result = dp[0];
        for (int i = 1; i < n; i++)
            result = Math.max(result, dp[i]);
        
        return result;
    }
    
    // Example
    // int[] arr = {10, 9, 2, 5, 3, 7, 101, 18};
    // System.out.println("LIS length: " + lisDp(arr));  // 4: [2,3,7,18] or [2,5,7,18]
}
```

### Binary Search Solution O(n log n)
```python
from bisect import bisect_left

def lis_binary_search(arr):
    """
    Time: O(n log n)
    Space: O(n)
    """
    tails = []
    
    for num in arr:
        pos = bisect_left(tails, num)
        
        if pos == len(tails):
            tails.append(num)
        else:
            tails[pos] = num
    
    return len(tails)

print(f"LIS length: {lis_binary_search(arr)}")  # 4
```

```c
#include <stdio.h>
#include <stdlib.h>

// Time: O(n log n)
// Space: O(n)
int binary_search_pos(int tails[], int len, int target) {
    int left = 0, right = len;
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (tails[mid] < target)
            left = mid + 1;
        else
            right = mid;
    }
    
    return left;
}

int lis_binary_search(int arr[], int n) {
    int *tails = (int *)malloc(n * sizeof(int));
    int len = 0;
    
    for (int i = 0; i < n; i++) {
        int pos = binary_search_pos(tails, len, arr[i]);
        
        tails[pos] = arr[i];
        if (pos == len)
            len++;
    }
    
    free(tails);
    return len;
}

// printf("LIS length: %d\n", lis_binary_search(arr, 8));  // 4
```

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

// Time: O(n log n)
// Space: O(n)
int lis_binary_search(std::vector<int>& arr) {
    std::vector<int> tails;
    
    for (int num : arr) {
        auto pos = std::lower_bound(tails.begin(), tails.end(), num);
        
        if (pos == tails.end())
            tails.push_back(num);
        else
            *pos = num;
    }
    
    return tails.size();
}

// std::cout << "LIS length: " << lis_binary_search(arr) << std::endl;  // 4
```

```java
import java.util.*;

public class LISBinarySearch {
    /**
     * Time: O(n log n)
     * Space: O(n)
     */
    public static int lisBinarySearch(int[] arr) {
        List<Integer> tails = new ArrayList<>();
        
        for (int num : arr) {
            int pos = Collections.binarySearch(tails, num);
            
            if (pos < 0)
                pos = -(pos + 1);
            
            if (pos == tails.size())
                tails.add(num);
            else
                tails.set(pos, num);
        }
        
        return tails.size();
    }
    
    // System.out.println("LIS length: " + lisBinarySearch(arr));  // 4
}
```

### Dry Run (arr=[10,9,2,5,3,7,101,18])
```
Using DP approach:
dp = [1, 1, 1, 1, 1, 1, 1, 1]

i=1 (arr[1]=9):
  j=0: 10 > 9, no update
  dp = [1, 1, 1, 1, 1, 1, 1, 1]

i=2 (arr[2]=2):
  j=0,1: both > 2, no update
  dp = [1, 1, 1, 1, 1, 1, 1, 1]

i=3 (arr[3]=5):
  j=2: 2 < 5, dp[3] = max(1, dp[2]+1) = 2
  dp = [1, 1, 1, 2, 1, 1, 1, 1]

i=4 (arr[4]=3):
  j=2: 2 < 3, dp[4] = max(1, dp[2]+1) = 2
  dp = [1, 1, 1, 2, 2, 1, 1, 1]

i=5 (arr[5]=7):
  j=2: 2 < 7, dp[5] = 2
  j=3: 5 < 7, dp[5] = max(2, dp[3]+1) = 3
  j=4: 3 < 7, dp[5] = max(3, dp[4]+1) = 3
  dp = [1, 1, 1, 2, 2, 3, 1, 1]

i=6 (arr[6]=101):
  All previous < 101, dp[6] = 4
  dp = [1, 1, 1, 2, 2, 3, 4, 1]

i=7 (arr[7]=18):
  dp[7] = 4
  dp = [1, 1, 1, 2, 2, 3, 4, 4]

Maximum: 4
```

---

## 6. Edit Distance (Levenshtein Distance)

### Problem
Find minimum operations (insert, delete, replace) to convert string s1 to s2.

### DP Solution
```python
def edit_distance(s1, s2):
    """
    Time: O(m*n)
    Space: O(m*n)
    """
    m, n = len(s1), len(s2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    
    # Base cases
    for i in range(m + 1):
        dp[i][0] = i  # Delete all from s1
    for j in range(n + 1):
        dp[0][j] = j  # Insert all from s2
    
    # Fill table
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if s1[i-1] == s2[j-1]:
                dp[i][j] = dp[i-1][j-1]  # No operation
            else:
                dp[i][j] = 1 + min(
                    dp[i-1][j],      # Delete
                    dp[i][j-1],      # Insert
                    dp[i-1][j-1]     # Replace
                )
    
    return dp[m][n]

# Example
s1 = "sunday"
s2 = "saturday"
print(f"Edit distance: {edit_distance(s1, s2)}")  # 3
```

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

// Time: O(m*n)
// Space: O(m*n)
int min3(int a, int b, int c) {
    int min = a;
    if (b < min) min = b;
    if (c < min) min = c;
    return min;
}

int edit_distance(char *s1, char *s2) {
    int m = strlen(s1);
    int n = strlen(s2);
    
    // Allocate 2D array
    int **dp = (int **)malloc((m + 1) * sizeof(int *));
    for (int i = 0; i <= m; i++)
        dp[i] = (int *)malloc((n + 1) * sizeof(int));
    
    // Base cases
    for (int i = 0; i <= m; i++)
        dp[i][0] = i;  // Delete all from s1
    for (int j = 0; j <= n; j++)
        dp[0][j] = j;  // Insert all from s2
    
    // Fill table
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (s1[i-1] == s2[j-1]) {
                dp[i][j] = dp[i-1][j-1];  // No operation
            } else {
                dp[i][j] = 1 + min3(
                    dp[i-1][j],      // Delete
                    dp[i][j-1],      // Insert
                    dp[i-1][j-1]     // Replace
                );
            }
        }
    }
    
    int result = dp[m][n];
    
    // Free memory
    for (int i = 0; i <= m; i++)
        free(dp[i]);
    free(dp);
    
    return result;
}

// Example
// char s1[] = "sunday";
// char s2[] = "saturday";
// printf("Edit distance: %d\n", edit_distance(s1, s2));  // 3
```

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

// Time: O(m*n)
// Space: O(m*n)
int edit_distance(std::string s1, std::string s2) {
    int m = s1.length();
    int n = s2.length();
    std::vector<std::vector<int>> dp(m + 1, std::vector<int>(n + 1));
    
    // Base cases
    for (int i = 0; i <= m; i++)
        dp[i][0] = i;  // Delete all from s1
    for (int j = 0; j <= n; j++)
        dp[0][j] = j;  // Insert all from s2
    
    // Fill table
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (s1[i-1] == s2[j-1]) {
                dp[i][j] = dp[i-1][j-1];  // No operation
            } else {
                dp[i][j] = 1 + std::min({
                    dp[i-1][j],      // Delete
                    dp[i][j-1],      // Insert
                    dp[i-1][j-1]     // Replace
                });
            }
        }
    }
    
    return dp[m][n];
}

// Example
// std::string s1 = "sunday";
// std::string s2 = "saturday";
// std::cout << "Edit distance: " << edit_distance(s1, s2) << std::endl;  // 3
```

```java
public class EditDistance {
    /**
     * Time: O(m*n)
     * Space: O(m*n)
     */
    public static int editDistance(String s1, String s2) {
        int m = s1.length();
        int n = s2.length();
        int[][] dp = new int[m + 1][n + 1];
        
        // Base cases
        for (int i = 0; i <= m; i++)
            dp[i][0] = i;  // Delete all from s1
        for (int j = 0; j <= n; j++)
            dp[0][j] = j;  // Insert all from s2
        
        // Fill table
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (s1.charAt(i-1) == s2.charAt(j-1)) {
                    dp[i][j] = dp[i-1][j-1];  // No operation
                } else {
                    dp[i][j] = 1 + Math.min(Math.min(
                        dp[i-1][j],      // Delete
                        dp[i][j-1]),     // Insert
                        dp[i-1][j-1]     // Replace
                    );
                }
            }
        }
        
        return dp[m][n];
    }
    
    // Example
    // String s1 = "sunday";
    // String s2 = "saturday";
    // System.out.println("Edit distance: " + editDistance(s1, s2));  // 3
}
```

### Dry Run (s1="cat", s2="cut")
```
DP Table:
      ""  c  u  t
""     0  1  2  3
c      1  0  1  2
a      2  1  1  2
t      3  2  2  1

Operations:
1. Keep 'c' (match)
2. Replace 'a' with 'u'
3. Keep 't' (match)

Edit Distance: 1
```

---

## 7. Matrix Chain Multiplication

### Problem
Find minimum number of scalar multiplications needed to compute product of matrices.

### DP Solution
```python
def matrix_chain_order(dims):
    """
    dims[i] represents dimension of matrix i
    Matrix i has dimensions dims[i-1] x dims[i]
    Time: O(n³)
    Space: O(n²)
    """
    n = len(dims) - 1  # Number of matrices
    dp = [[0] * n for _ in range(n)]
    
    # l is chain length
    for l in range(2, n + 1):
        for i in range(n - l + 1):
            j = i + l - 1
            dp[i][j] = float('inf')
            
            for k in range(i, j):
                cost = (dp[i][k] + dp[k+1][j] + 
                       dims[i] * dims[k+1] * dims[j+1])
                dp[i][j] = min(dp[i][j], cost)
    
    return dp[0][n-1]

# Example
# Matrices: A(10x20), B(20x30), C(30x40), D(40x30)
dims = [10, 20, 30, 40, 30]
print(f"Minimum multiplications: {matrix_chain_order(dims)}")  # 30000
```

```c
#include <stdio.h>
#include <stdlib.h>
#include <limits.h>

// Time: O(n³)
// Space: O(n²)
int min(int a, int b) {
    return (a < b) ? a : b;
}

int matrix_chain_order(int dims[], int n) {
    // n is number of matrices (length of dims - 1)
    int size = n - 1;
    
    // Allocate 2D array
    int **dp = (int **)malloc(size * sizeof(int *));
    for (int i = 0; i < size; i++) {
        dp[i] = (int *)calloc(size, sizeof(int));
    }
    
    // l is chain length
    for (int l = 2; l <= size; l++) {
        for (int i = 0; i < size - l + 1; i++) {
            int j = i + l - 1;
            dp[i][j] = INT_MAX;
            
            for (int k = i; k < j; k++) {
                int cost = dp[i][k] + dp[k+1][j] + 
                          dims[i] * dims[k+1] * dims[j+1];
                dp[i][j] = min(dp[i][j], cost);
            }
        }
    }
    
    int result = dp[0][size-1];
    
    // Free memory
    for (int i = 0; i < size; i++)
        free(dp[i]);
    free(dp);
    
    return result;
}

// Example
// Matrices: A(10x20), B(20x30), C(30x40), D(40x30)
// int dims[] = {10, 20, 30, 40, 30};
// printf("Minimum multiplications: %d\n", matrix_chain_order(dims, 5));  // 30000
```

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <limits>

// Time: O(n³)
// Space: O(n²)
int matrix_chain_order(std::vector<int>& dims) {
    int n = dims.size() - 1;  // Number of matrices
    std::vector<std::vector<int>> dp(n, std::vector<int>(n, 0));
    
    // l is chain length
    for (int l = 2; l <= n; l++) {
        for (int i = 0; i < n - l + 1; i++) {
            int j = i + l - 1;
            dp[i][j] = INT_MAX;
            
            for (int k = i; k < j; k++) {
                int cost = dp[i][k] + dp[k+1][j] + 
                          dims[i] * dims[k+1] * dims[j+1];
                dp[i][j] = std::min(dp[i][j], cost);
            }
        }
    }
    
    return dp[0][n-1];
}

// Example
// Matrices: A(10x20), B(20x30), C(30x40), D(40x30)
// std::vector<int> dims = {10, 20, 30, 40, 30};
// std::cout << "Minimum multiplications: " << matrix_chain_order(dims) << std::endl;  // 30000
```

```java
public class MatrixChainMultiplication {
    /**
     * dims[i] represents dimension of matrix i
     * Matrix i has dimensions dims[i-1] x dims[i]
     * Time: O(n³)
     * Space: O(n²)
     */
    public static int matrixChainOrder(int[] dims) {
        int n = dims.length - 1;  // Number of matrices
        int[][] dp = new int[n][n];
        
        // l is chain length
        for (int l = 2; l <= n; l++) {
            for (int i = 0; i < n - l + 1; i++) {
                int j = i + l - 1;
                dp[i][j] = Integer.MAX_VALUE;
                
                for (int k = i; k < j; k++) {
                    int cost = dp[i][k] + dp[k+1][j] + 
                              dims[i] * dims[k+1] * dims[j+1];
                    dp[i][j] = Math.min(dp[i][j], cost);
                }
            }
        }
        
        return dp[0][n-1];
    }
    
    // Example
    // Matrices: A(10x20), B(20x30), C(30x40), D(40x30)
    // int[] dims = {10, 20, 30, 40, 30};
    // System.out.println("Minimum multiplications: " + matrixChainOrder(dims));  // 30000
}
```

---

## 8. Rod Cutting Problem

### Problem
Given rod of length n and prices for different lengths, maximize profit by cutting rod.

```python
def rod_cutting(prices, n):
    """
    Time: O(n²)
    Space: O(n)
    """
    dp = [0] * (n + 1)
    
    for i in range(1, n + 1):
        for j in range(i):
            dp[i] = max(dp[i], prices[j] + dp[i - j - 1])
    
    return dp[n]

# Example
prices = [1, 5, 8, 9, 10, 17, 17, 20]
length = 8
print(f"Maximum profit: {rod_cutting(prices, length)}")  # 22
```

```c
#include <stdio.h>
#include <stdlib.h>

// Time: O(n²)
// Space: O(n)
int max(int a, int b) {
    return (a > b) ? a : b;
}

int rod_cutting(int prices[], int n) {
    int *dp = (int *)calloc(n + 1, sizeof(int));
    
    for (int i = 1; i <= n; i++) {
        for (int j = 0; j < i; j++) {
            dp[i] = max(dp[i], prices[j] + dp[i - j - 1]);
        }
    }
    
    int result = dp[n];
    free(dp);
    return result;
}

// Example
// int prices[] = {1, 5, 8, 9, 10, 17, 17, 20};
// int length = 8;
// printf("Maximum profit: %d\n", rod_cutting(prices, length));  // 22
```

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

// Time: O(n²)
// Space: O(n)
int rod_cutting(std::vector<int>& prices, int n) {
    std::vector<int> dp(n + 1, 0);
    
    for (int i = 1; i <= n; i++) {
        for (int j = 0; j < i; j++) {
            dp[i] = std::max(dp[i], prices[j] + dp[i - j - 1]);
        }
    }
    
    return dp[n];
}

// Example
// std::vector<int> prices = {1, 5, 8, 9, 10, 17, 17, 20};
// int length = 8;
// std::cout << "Maximum profit: " << rod_cutting(prices, length) << std::endl;  // 22
```

```java
public class RodCutting {
    /**
     * Time: O(n²)
     * Space: O(n)
     */
    public static int rodCutting(int[] prices, int n) {
        int[] dp = new int[n + 1];
        
        for (int i = 1; i <= n; i++) {
            for (int j = 0; j < i; j++) {
                dp[i] = Math.max(dp[i], prices[j] + dp[i - j - 1]);
            }
        }
        
        return dp[n];
    }
    
    // Example
    // int[] prices = {1, 5, 8, 9, 10, 17, 17, 20};
    // int length = 8;
    // System.out.println("Maximum profit: " + rodCutting(prices, length));  // 22
}
```

---

## DP Patterns

### 1. Linear DP
- Single dimensional array
- Examples: Fibonacci, Climbing Stairs, House Robber

### 2. 2D DP
- Two dimensional table
- Examples: LCS, Edit Distance, Knapsack

### 3. Interval DP
- Process intervals of different lengths
- Example: Matrix Chain Multiplication

### 4. State Machine DP
- Different states at each step
- Example: Stock Buy/Sell problems

### 5. Bitmask DP
- Use bits to represent states
- Example: Traveling Salesman

---

## DP vs Recursion vs Greedy

| Feature | Recursion | DP | Greedy |
|---------|-----------|----|----|
| Approach | Top-down | Top-down or Bottom-up | Sequential |
| Overlapping Subproblems | Recomputes | Stores | N/A |
| Complexity | Often exponential | Polynomial | Usually lower |
| Optimality | Can be optimal | Always optimal | Not always |
| Space | Stack | Additional storage | Minimal |

---

## Exam Tips

### 1. Identifying DP Problems
Look for:
- "Maximum/Minimum"
- "Count number of ways"
- "Longest/Shortest"
- Overlapping subproblems
- Optimal substructure

### 2. Common DP Patterns
```python
# Pattern 1: 1D DP
dp[i] = function(dp[i-1], dp[i-2], ...)

# Pattern 2: 2D DP
dp[i][j] = function(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])

# Pattern 3: Knapsack
if weight[i] <= capacity:
    dp[i][w] = max(include, exclude)
```

### 3. Dry Run Steps
1. Initialize DP table
2. Fill base cases
3. Apply recurrence relation
4. Track each cell calculation
5. Return final answer

### 4. Complexity Analysis
- Time: Usually O(n²) or O(n×m)
- Space: Can often optimize to O(n) or O(1)

### Key Points to Remember
1. **Memoization**: Store in dictionary/array
2. **Tabulation**: Build table bottom-up
3. **State definition**: What does dp[i] represent?
4. **Base cases**: Initialize properly
5. **Recurrence relation**: Core logic
6. **Space optimization**: Track only needed previous states
