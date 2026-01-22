# Pattern Searching - Comprehensive Notes

## Overview
Pattern searching (string matching) is the problem of finding occurrences of a pattern string within a text string. It's fundamental to text processing, search engines, DNA sequencing, and plagiarism detection.

### Problem Statement
Given:
- Text string T of length n
- Pattern string P of length m
Find: All occurrences of P in T

---

## 1. Naive Pattern Searching

### Concept
Check for pattern at every position in text using a sliding window.

### Algorithm
```
for i = 0 to n-m:
    j = 0
    while j < m and T[i+j] == P[j]:
        j++
    if j == m:
        pattern found at index i
```

### Implementation (Python)
```python
def naive_search(text, pattern):
    """
    Naive pattern searching
    Time: O((n-m+1) * m) = O(n*m)
    Space: O(1)
    """
    n = len(text)
    m = len(pattern)
    positions = []
    
    for i in range(n - m + 1):
        j = 0
        while j < m and text[i + j] == pattern[j]:
            j += 1
        
        if j == m:
            positions.append(i)
    
    return positions

# Example
text = "AABAACAADAABAABA"
pattern = "AABA"
result = naive_search(text, pattern)
print(f"Pattern found at positions: {result}")  # [0, 9, 12]
```

### Implementation (C)
```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

/**
 * Naive pattern searching
 * Time: O((n-m+1) * m) = O(n*m)
 * Space: O(1)
 */
int* naive_search(const char* text, const char* pattern, int* count) {
    int n = strlen(text);
    int m = strlen(pattern);
    int* positions = (int*)malloc(n * sizeof(int));
    *count = 0;
    
    for (int i = 0; i <= n - m; i++) {
        int j = 0;
        while (j < m && text[i + j] == pattern[j]) {
            j++;
        }
        
        if (j == m) {
            positions[(*count)++] = i;
        }
    }
    
    return positions;
}

// Example
int main() {
    const char* text = "AABAACAADAABAABA";
    const char* pattern = "AABA";
    int count;
    
    int* result = naive_search(text, pattern, &count);
    
    printf("Pattern found at positions: ");
    for (int i = 0; i < count; i++) {
        printf("%d ", result[i]);
    }
    printf("\n");  // Output: 0 9 12
    
    free(result);
    return 0;
}
```

### Implementation (C++)
```cpp
#include <iostream>
#include <vector>
#include <string>
using namespace std;

/**
 * Naive pattern searching
 * Time: O((n-m+1) * m) = O(n*m)
 * Space: O(1)
 */
vector<int> naive_search(const string& text, const string& pattern) {
    int n = text.length();
    int m = pattern.length();
    vector<int> positions;
    
    for (int i = 0; i <= n - m; i++) {
        int j = 0;
        while (j < m && text[i + j] == pattern[j]) {
            j++;
        }
        
        if (j == m) {
            positions.push_back(i);
        }
    }
    
    return positions;
}

// Example
int main() {
    string text = "AABAACAADAABAABA";
    string pattern = "AABA";
    
    vector<int> result = naive_search(text, pattern);
    
    cout << "Pattern found at positions: ";
    for (int pos : result) {
        cout << pos << " ";
    }
    cout << endl;  // Output: 0 9 12
    
    return 0;
}
```

### Implementation (Java)
```java
import java.util.ArrayList;
import java.util.List;

public class NaivePatternSearch {
    /**
     * Naive pattern searching
     * Time: O((n-m+1) * m) = O(n*m)
     * Space: O(1)
     */
    public static List<Integer> naiveSearch(String text, String pattern) {
        int n = text.length();
        int m = pattern.length();
        List<Integer> positions = new ArrayList<>();
        
        for (int i = 0; i <= n - m; i++) {
            int j = 0;
            while (j < m && text.charAt(i + j) == pattern.charAt(j)) {
                j++;
            }
            
            if (j == m) {
                positions.add(i);
            }
        }
        
        return positions;
    }
    
    // Example
    public static void main(String[] args) {
        String text = "AABAACAADAABAABA";
        String pattern = "AABA";
        
        List<Integer> result = naiveSearch(text, pattern);
        
        System.out.print("Pattern found at positions: ");
        for (int pos : result) {
            System.out.print(pos + " ");
        }
        System.out.println();  // Output: 0 9 12
    }
}
```

### Dry Run
```
Text:    A A B A A C A A D A A B A A B A
Pattern: A A B A

Position 0:
A A B A (text) vs A A B A (pattern) ✓ Match!

Position 1:
A B A A (text) vs A A B A (pattern) ✗ Mismatch at index 1

Position 2:
B A A C (text) vs A A B A (pattern) ✗ Mismatch at index 0

...continue...

Position 9:
A A B A (text) vs A A B A (pattern) ✓ Match!

Position 12:
A A B A (text) vs A A B A (pattern) ✓ Match!

Matches at: [0, 9, 12]
Total comparisons: ~50
```

### Complexity
- **Time**: O((n-m+1) × m) = O(n×m) worst case
- **Space**: O(1)
- **Best case**: O(n) when first character doesn't match
- **Worst case**: O(n×m) when all characters match until last

---

## 2. Knuth-Morris-Pratt (KMP) Algorithm

### Concept
Avoid rechecking matched characters by using a prefix function (LPS array) that stores longest proper prefix which is also suffix.

### LPS Array
LPS[i] = length of longest proper prefix of pattern[0...i] which is also a suffix

### Algorithm
```python
def compute_lps(pattern):
    """
    Compute LPS (Longest Prefix Suffix) array
    Time: O(m)
    """
    m = len(pattern)
    lps = [0] * m
    length = 0  # Length of previous longest prefix suffix
    i = 1
    
    while i < m:
        if pattern[i] == pattern[length]:
            length += 1
            lps[i] = length
            i += 1
        else:
            if length != 0:
                length = lps[length - 1]
            else:
                lps[i] = 0
                i += 1
    
    return lps

def kmp_search(text, pattern):
    """
    KMP pattern searching
    Time: O(n + m)
    Space: O(m)
    """
    n = len(text)
    m = len(pattern)
    
    # Compute LPS array
    lps = compute_lps(pattern)
    
    positions = []
    i = 0  # Index for text
    j = 0  # Index for pattern
    
    while i < n:
        if text[i] == pattern[j]:
            i += 1
            j += 1
        
        if j == m:
            positions.append(i - j)
            j = lps[j - 1]
        elif i < n and text[i] != pattern[j]:
            if j != 0:
                j = lps[j - 1]
            else:
                i += 1
    
    return positions

# Example
text = "ABABDABACDABABCABAB"
pattern = "ABABCABAB"
result = kmp_search(text, pattern)
print(f"Pattern found at positions: {result}")
```

### Implementation (C)
```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

/**
 * Compute LPS (Longest Prefix Suffix) array
 * Time: O(m)
 */
void compute_lps(const char* pattern, int m, int* lps) {
    int length = 0;  // Length of previous longest prefix suffix
    lps[0] = 0;
    int i = 1;
    
    while (i < m) {
        if (pattern[i] == pattern[length]) {
            length++;
            lps[i] = length;
            i++;
        } else {
            if (length != 0) {
                length = lps[length - 1];
            } else {
                lps[i] = 0;
                i++;
            }
        }
    }
}

/**
 * KMP pattern searching
 * Time: O(n + m)
 * Space: O(m)
 */
int* kmp_search(const char* text, const char* pattern, int* count) {
    int n = strlen(text);
    int m = strlen(pattern);
    
    // Compute LPS array
    int* lps = (int*)malloc(m * sizeof(int));
    compute_lps(pattern, m, lps);
    
    int* positions = (int*)malloc(n * sizeof(int));
    *count = 0;
    
    int i = 0;  // Index for text
    int j = 0;  // Index for pattern
    
    while (i < n) {
        if (text[i] == pattern[j]) {
            i++;
            j++;
        }
        
        if (j == m) {
            positions[(*count)++] = i - j;
            j = lps[j - 1];
        } else if (i < n && text[i] != pattern[j]) {
            if (j != 0) {
                j = lps[j - 1];
            } else {
                i++;
            }
        }
    }
    
    free(lps);
    return positions;
}

// Example
int main() {
    const char* text = "ABABDABACDABABCABAB";
    const char* pattern = "ABABCABAB";
    int count;
    
    int* result = kmp_search(text, pattern, &count);
    
    printf("Pattern found at positions: ");
    for (int i = 0; i < count; i++) {
        printf("%d ", result[i]);
    }
    printf("\n");
    
    free(result);
    return 0;
}
```

### Implementation (C++)
```cpp
#include <iostream>
#include <vector>
#include <string>
using namespace std;

/**
 * Compute LPS (Longest Prefix Suffix) array
 * Time: O(m)
 */
vector<int> compute_lps(const string& pattern) {
    int m = pattern.length();
    vector<int> lps(m, 0);
    int length = 0;  // Length of previous longest prefix suffix
    int i = 1;
    
    while (i < m) {
        if (pattern[i] == pattern[length]) {
            length++;
            lps[i] = length;
            i++;
        } else {
            if (length != 0) {
                length = lps[length - 1];
            } else {
                lps[i] = 0;
                i++;
            }
        }
    }
    
    return lps;
}

/**
 * KMP pattern searching
 * Time: O(n + m)
 * Space: O(m)
 */
vector<int> kmp_search(const string& text, const string& pattern) {
    int n = text.length();
    int m = pattern.length();
    
    // Compute LPS array
    vector<int> lps = compute_lps(pattern);
    
    vector<int> positions;
    int i = 0;  // Index for text
    int j = 0;  // Index for pattern
    
    while (i < n) {
        if (text[i] == pattern[j]) {
            i++;
            j++;
        }
        
        if (j == m) {
            positions.push_back(i - j);
            j = lps[j - 1];
        } else if (i < n && text[i] != pattern[j]) {
            if (j != 0) {
                j = lps[j - 1];
            } else {
                i++;
            }
        }
    }
    
    return positions;
}

// Example
int main() {
    string text = "ABABDABACDABABCABAB";
    string pattern = "ABABCABAB";
    
    vector<int> result = kmp_search(text, pattern);
    
    cout << "Pattern found at positions: ";
    for (int pos : result) {
        cout << pos << " ";
    }
    cout << endl;
    
    return 0;
}
```

### Implementation (Java)
```java
import java.util.ArrayList;
import java.util.List;

public class KMPPatternSearch {
    /**
     * Compute LPS (Longest Prefix Suffix) array
     * Time: O(m)
     */
    public static int[] computeLPS(String pattern) {
        int m = pattern.length();
        int[] lps = new int[m];
        int length = 0;  // Length of previous longest prefix suffix
        int i = 1;
        
        while (i < m) {
            if (pattern.charAt(i) == pattern.charAt(length)) {
                length++;
                lps[i] = length;
                i++;
            } else {
                if (length != 0) {
                    length = lps[length - 1];
                } else {
                    lps[i] = 0;
                    i++;
                }
            }
        }
        
        return lps;
    }
    
    /**
     * KMP pattern searching
     * Time: O(n + m)
     * Space: O(m)
     */
    public static List<Integer> kmpSearch(String text, String pattern) {
        int n = text.length();
        int m = pattern.length();
        
        // Compute LPS array
        int[] lps = computeLPS(pattern);
        
        List<Integer> positions = new ArrayList<>();
        int i = 0;  // Index for text
        int j = 0;  // Index for pattern
        
        while (i < n) {
            if (text.charAt(i) == pattern.charAt(j)) {
                i++;
                j++;
            }
            
            if (j == m) {
                positions.add(i - j);
                j = lps[j - 1];
            } else if (i < n && text.charAt(i) != pattern.charAt(j)) {
                if (j != 0) {
                    j = lps[j - 1];
                } else {
                    i++;
                }
            }
        }
        
        return positions;
    }
    
    // Example
    public static void main(String[] args) {
        String text = "ABABDABACDABABCABAB";
        String pattern = "ABABCABAB";
        
        List<Integer> result = kmpSearch(text, pattern);
        
        System.out.print("Pattern found at positions: ");
        for (int pos : result) {
            System.out.print(pos + " ");
        }
        System.out.println();
    }
}
```

### LPS Array Computation Example
```
Pattern: A B A B C A B A B

Index:  0 1 2 3 4 5 6 7 8
Pattern: A B A B C A B A B
LPS:    0 0 1 2 0 1 2 3 4

Explanation:
Index 0: A → LPS[0] = 0 (no proper prefix)
Index 1: AB → LPS[1] = 0 (no matching prefix-suffix)
Index 2: ABA → LPS[2] = 1 ("A")
Index 3: ABAB → LPS[3] = 2 ("AB")
Index 4: ABABC → LPS[4] = 0 (no matching)
Index 5: ABABCA → LPS[5] = 1 ("A")
Index 6: ABABCAB → LPS[6] = 2 ("AB")
Index 7: ABABCABA → LPS[7] = 3 ("ABA")
Index 8: ABABCABAB → LPS[8] = 4 ("ABAB")
```

### Dry Run (Text="ABABCABAB", Pattern="ABABCABAB")
```
LPS array for pattern: [0, 0, 1, 2, 0, 1, 2, 3, 4]

i=0, j=0: text[0]='A' == pattern[0]='A', i++, j++
i=1, j=1: text[1]='B' == pattern[1]='B', i++, j++
i=2, j=2: text[2]='A' == pattern[2]='A', i++, j++
i=3, j=3: text[3]='B' == pattern[3]='B', i++, j++
i=4, j=4: text[4]='C' == pattern[4]='C', i++, j++
i=5, j=5: text[5]='A' == pattern[5]='A', i++, j++
i=6, j=6: text[6]='B' == pattern[6]='B', i++, j++
i=7, j=7: text[7]='A' == pattern[7]='A', i++, j++
i=8, j=8: text[8]='B' == pattern[8]='B', i++, j++

j == m: Pattern found at position 0!

Total comparisons: 9 (vs 81 for naive)
```

### Complexity
- **Preprocessing**: O(m)
- **Searching**: O(n)
- **Total**: O(n + m)
- **Space**: O(m)

---

## 3. Rabin-Karp Algorithm

### Concept
Use hashing to find pattern matches. Compare hash values instead of characters.

### Rolling Hash
- Initial hash: h(pattern[0...m-1])
- Next hash: remove first char, add next char
- Formula: hash = (d × (hash - text[i] × h) + text[i + m]) % q

### Implementation
```python
def rabin_karp(text, pattern, d=256, q=101):
    """
    Rabin-Karp pattern searching using rolling hash
    Time: O(n+m) average, O(n*m) worst
    Space: O(1)
    
    d = number of characters in alphabet
    q = prime number for modulo
    """
    n = len(text)
    m = len(pattern)
    h = pow(d, m - 1, q)  # d^(m-1) % q
    p_hash = 0  # Pattern hash
    t_hash = 0  # Text hash
    positions = []
    
    # Calculate initial hash values
    for i in range(m):
        p_hash = (d * p_hash + ord(pattern[i])) % q
        t_hash = (d * t_hash + ord(text[i])) % q
    
    # Slide pattern over text
    for i in range(n - m + 1):
        # Check if hash values match
        if p_hash == t_hash:
            # Verify character by character
            if text[i:i+m] == pattern:
                positions.append(i)
        
        # Calculate hash for next window
        if i < n - m:
            t_hash = (d * (t_hash - ord(text[i]) * h) + ord(text[i + m])) % q
            
            # Handle negative hash
            if t_hash < 0:
                t_hash += q
    
    return positions

# Example
text = "GEEKS FOR GEEKS"
pattern = "GEEK"
result = rabin_karp(text, pattern)
print(f"Pattern found at positions: {result}")  # [0, 10]
```

### Implementation (C)
```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <stdbool.h>

/**
 * Rabin-Karp pattern searching using rolling hash
 * Time: O(n+m) average, O(n*m) worst
 * Space: O(1)
 * 
 * d = number of characters in alphabet (256)
 * q = prime number for modulo (101)
 */
int* rabin_karp(const char* text, const char* pattern, int d, int q, int* count) {
    int n = strlen(text);
    int m = strlen(pattern);
    int h = 1;  // d^(m-1) % q
    int p_hash = 0;  // Pattern hash
    int t_hash = 0;  // Text hash
    int* positions = (int*)malloc(n * sizeof(int));
    *count = 0;
    
    // Calculate h = d^(m-1) % q
    for (int i = 0; i < m - 1; i++) {
        h = (h * d) % q;
    }
    
    // Calculate initial hash values
    for (int i = 0; i < m; i++) {
        p_hash = (d * p_hash + pattern[i]) % q;
        t_hash = (d * t_hash + text[i]) % q;
    }
    
    // Slide pattern over text
    for (int i = 0; i <= n - m; i++) {
        // Check if hash values match
        if (p_hash == t_hash) {
            // Verify character by character
            bool match = true;
            for (int j = 0; j < m; j++) {
                if (text[i + j] != pattern[j]) {
                    match = false;
                    break;
                }
            }
            if (match) {
                positions[(*count)++] = i;
            }
        }
        
        // Calculate hash for next window
        if (i < n - m) {
            t_hash = (d * (t_hash - text[i] * h) + text[i + m]) % q;
            
            // Handle negative hash
            if (t_hash < 0) {
                t_hash += q;
            }
        }
    }
    
    return positions;
}

// Example
int main() {
    const char* text = "GEEKS FOR GEEKS";
    const char* pattern = "GEEK";
    int count;
    
    int* result = rabin_karp(text, pattern, 256, 101, &count);
    
    printf("Pattern found at positions: ");
    for (int i = 0; i < count; i++) {
        printf("%d ", result[i]);
    }
    printf("\n");  // Output: 0 10
    
    free(result);
    return 0;
}
```

### Implementation (C++)
```cpp
#include <iostream>
#include <vector>
#include <string>
using namespace std;

/**
 * Rabin-Karp pattern searching using rolling hash
 * Time: O(n+m) average, O(n*m) worst
 * Space: O(1)
 * 
 * d = number of characters in alphabet (256)
 * q = prime number for modulo (101)
 */
vector<int> rabin_karp(const string& text, const string& pattern, int d = 256, int q = 101) {
    int n = text.length();
    int m = pattern.length();
    int h = 1;  // d^(m-1) % q
    int p_hash = 0;  // Pattern hash
    int t_hash = 0;  // Text hash
    vector<int> positions;
    
    // Calculate h = d^(m-1) % q
    for (int i = 0; i < m - 1; i++) {
        h = (h * d) % q;
    }
    
    // Calculate initial hash values
    for (int i = 0; i < m; i++) {
        p_hash = (d * p_hash + pattern[i]) % q;
        t_hash = (d * t_hash + text[i]) % q;
    }
    
    // Slide pattern over text
    for (int i = 0; i <= n - m; i++) {
        // Check if hash values match
        if (p_hash == t_hash) {
            // Verify character by character
            bool match = true;
            for (int j = 0; j < m; j++) {
                if (text[i + j] != pattern[j]) {
                    match = false;
                    break;
                }
            }
            if (match) {
                positions.push_back(i);
            }
        }
        
        // Calculate hash for next window
        if (i < n - m) {
            t_hash = (d * (t_hash - text[i] * h) + text[i + m]) % q;
            
            // Handle negative hash
            if (t_hash < 0) {
                t_hash += q;
            }
        }
    }
    
    return positions;
}

// Example
int main() {
    string text = "GEEKS FOR GEEKS";
    string pattern = "GEEK";
    
    vector<int> result = rabin_karp(text, pattern);
    
    cout << "Pattern found at positions: ";
    for (int pos : result) {
        cout << pos << " ";
    }
    cout << endl;  // Output: 0 10
    
    return 0;
}
```

### Implementation (Java)
```java
import java.util.ArrayList;
import java.util.List;

public class RabinKarpPatternSearch {
    /**
     * Rabin-Karp pattern searching using rolling hash
     * Time: O(n+m) average, O(n*m) worst
     * Space: O(1)
     * 
     * d = number of characters in alphabet (256)
     * q = prime number for modulo (101)
     */
    public static List<Integer> rabinKarp(String text, String pattern, int d, int q) {
        int n = text.length();
        int m = pattern.length();
        int h = 1;  // d^(m-1) % q
        int p_hash = 0;  // Pattern hash
        int t_hash = 0;  // Text hash
        List<Integer> positions = new ArrayList<>();
        
        // Calculate h = d^(m-1) % q
        for (int i = 0; i < m - 1; i++) {
            h = (h * d) % q;
        }
        
        // Calculate initial hash values
        for (int i = 0; i < m; i++) {
            p_hash = (d * p_hash + pattern.charAt(i)) % q;
            t_hash = (d * t_hash + text.charAt(i)) % q;
        }
        
        // Slide pattern over text
        for (int i = 0; i <= n - m; i++) {
            // Check if hash values match
            if (p_hash == t_hash) {
                // Verify character by character
                boolean match = true;
                for (int j = 0; j < m; j++) {
                    if (text.charAt(i + j) != pattern.charAt(j)) {
                        match = false;
                        break;
                    }
                }
                if (match) {
                    positions.add(i);
                }
            }
            
            // Calculate hash for next window
            if (i < n - m) {
                t_hash = (d * (t_hash - text.charAt(i) * h) + text.charAt(i + m)) % q;
                
                // Handle negative hash
                if (t_hash < 0) {
                    t_hash += q;
                }
            }
        }
        
        return positions;
    }
    
    public static List<Integer> rabinKarp(String text, String pattern) {
        return rabinKarp(text, pattern, 256, 101);
    }
    
    // Example
    public static void main(String[] args) {
        String text = "GEEKS FOR GEEKS";
        String pattern = "GEEK";
        
        List<Integer> result = rabinKarp(text, pattern);
        
        System.out.print("Pattern found at positions: ");
        for (int pos : result) {
            System.out.print(pos + " ");
        }
        System.out.println();  // Output: 0 10
    }
}
```

### Dry Run (Text="ABCAB", Pattern="CAB", d=256, q=11)
```
Pattern hash:
p_hash = (256*0 + ord('C')) % 11 = 67 % 11 = 1
p_hash = (256*1 + ord('A')) % 11 = 321 % 11 = 2
p_hash = (256*2 + ord('B')) % 11 = 578 % 11 = 6

Initial text hash (ABC):
t_hash = (256*0 + ord('A')) % 11 = 65 % 11 = 10
t_hash = (256*10 + ord('B')) % 11 = 2626 % 11 = 7
t_hash = (256*7 + ord('C')) % 11 = 1859 % 11 = 1

Position 0 (ABC):
t_hash = 1, p_hash = 6
Hash mismatch, skip

Position 1 (BCA):
Remove 'A': t_hash = (256 * (1 - 65*256^2) + ord('A')) % 11
Add 'A': Calculate new hash
t_hash != p_hash, skip

Position 2 (CAB):
Calculate hash...
t_hash = 6, p_hash = 6
Hash match! Verify: "CAB" == "CAB" ✓
Position found: 2
```

### Complexity
- **Average case**: O(n + m)
- **Worst case**: O(n × m) when many spurious hits
- **Space**: O(1)

---

## 4. Boyer-Moore Algorithm

### Concept
Search from right to left of pattern. Skip sections of text using two heuristics:
1. **Bad Character Rule**: Skip based on rightmost occurrence
2. **Good Suffix Rule**: Skip based on matching suffix

### Bad Character Heuristic
```python
def bad_character_table(pattern):
    """
    Create bad character table
    """
    table = {}
    m = len(pattern)
    
    for i in range(m):
        table[pattern[i]] = i
    
    return table

def boyer_moore_bad_char(text, pattern):
    """
    Boyer-Moore with bad character heuristic only
    Time: O(n/m) best, O(n*m) worst
    Space: O(k) where k is alphabet size
    """
    n = len(text)
    m = len(pattern)
    
    bad_char = bad_character_table(pattern)
    positions = []
    
    s = 0  # Shift of pattern
    
    while s <= n - m:
        j = m - 1
        
        # Compare from right to left
        while j >= 0 and pattern[j] == text[s + j]:
            j -= 1
        
        if j < 0:
            # Pattern found
            positions.append(s)
            s += m - bad_char.get(text[s + m], -1) if s + m < n else 1
        else:
            # Shift pattern
            s += max(1, j - bad_char.get(text[s + j], -1))
    
    return positions

# Example
text = "ABAAABCDABABC"
pattern = "ABC"
result = boyer_moore_bad_char(text, pattern)
print(f"Pattern found at positions: {result}")
```

### Implementation (C)
```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <limits.h>

#define NO_OF_CHARS 256

/**
 * Create bad character table
 */
void bad_character_table(const char* pattern, int m, int badChar[]) {
    // Initialize all occurrences as -1
    for (int i = 0; i < NO_OF_CHARS; i++) {
        badChar[i] = -1;
    }
    
    // Fill the actual value of last occurrence
    for (int i = 0; i < m; i++) {
        badChar[(unsigned char)pattern[i]] = i;
    }
}

/**
 * Boyer-Moore with bad character heuristic only
 * Time: O(n/m) best, O(n*m) worst
 * Space: O(k) where k is alphabet size
 */
int* boyer_moore_bad_char(const char* text, const char* pattern, int* count) {
    int n = strlen(text);
    int m = strlen(pattern);
    
    int badChar[NO_OF_CHARS];
    bad_character_table(pattern, m, badChar);
    
    int* positions = (int*)malloc(n * sizeof(int));
    *count = 0;
    
    int s = 0;  // Shift of pattern
    
    while (s <= n - m) {
        int j = m - 1;
        
        // Compare from right to left
        while (j >= 0 && pattern[j] == text[s + j]) {
            j--;
        }
        
        if (j < 0) {
            // Pattern found
            positions[(*count)++] = s;
            s += (s + m < n) ? m - badChar[(unsigned char)text[s + m]] : 1;
        } else {
            // Shift pattern
            int shift = j - badChar[(unsigned char)text[s + j]];
            s += (shift > 1) ? shift : 1;
        }
    }
    
    return positions;
}

// Example
int main() {
    const char* text = "ABAAABCDABABC";
    const char* pattern = "ABC";
    int count;
    
    int* result = boyer_moore_bad_char(text, pattern, &count);
    
    printf("Pattern found at positions: ");
    for (int i = 0; i < count; i++) {
        printf("%d ", result[i]);
    }
    printf("\n");
    
    free(result);
    return 0;
}
```

### Implementation (C++)
```cpp
#include <iostream>
#include <vector>
#include <string>
#include <unordered_map>
#include <algorithm>
using namespace std;

/**
 * Create bad character table
 */
unordered_map<char, int> bad_character_table(const string& pattern) {
    unordered_map<char, int> table;
    int m = pattern.length();
    
    for (int i = 0; i < m; i++) {
        table[pattern[i]] = i;
    }
    
    return table;
}

/**
 * Boyer-Moore with bad character heuristic only
 * Time: O(n/m) best, O(n*m) worst
 * Space: O(k) where k is alphabet size
 */
vector<int> boyer_moore_bad_char(const string& text, const string& pattern) {
    int n = text.length();
    int m = pattern.length();
    
    unordered_map<char, int> badChar = bad_character_table(pattern);
    vector<int> positions;
    
    int s = 0;  // Shift of pattern
    
    while (s <= n - m) {
        int j = m - 1;
        
        // Compare from right to left
        while (j >= 0 && pattern[j] == text[s + j]) {
            j--;
        }
        
        if (j < 0) {
            // Pattern found
            positions.push_back(s);
            if (s + m < n && badChar.find(text[s + m]) != badChar.end()) {
                s += m - badChar[text[s + m]];
            } else {
                s += 1;
            }
        } else {
            // Shift pattern
            int shift = (badChar.find(text[s + j]) != badChar.end()) 
                        ? j - badChar[text[s + j]] : j + 1;
            s += max(1, shift);
        }
    }
    
    return positions;
}

// Example
int main() {
    string text = "ABAAABCDABABC";
    string pattern = "ABC";
    
    vector<int> result = boyer_moore_bad_char(text, pattern);
    
    cout << "Pattern found at positions: ";
    for (int pos : result) {
        cout << pos << " ";
    }
    cout << endl;
    
    return 0;
}
```

### Implementation (Java)
```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class BoyerMoorePatternSearch {
    /**
     * Create bad character table
     */
    public static Map<Character, Integer> badCharacterTable(String pattern) {
        Map<Character, Integer> table = new HashMap<>();
        int m = pattern.length();
        
        for (int i = 0; i < m; i++) {
            table.put(pattern.charAt(i), i);
        }
        
        return table;
    }
    
    /**
     * Boyer-Moore with bad character heuristic only
     * Time: O(n/m) best, O(n*m) worst
     * Space: O(k) where k is alphabet size
     */
    public static List<Integer> boyerMooreBadChar(String text, String pattern) {
        int n = text.length();
        int m = pattern.length();
        
        Map<Character, Integer> badChar = badCharacterTable(pattern);
        List<Integer> positions = new ArrayList<>();
        
        int s = 0;  // Shift of pattern
        
        while (s <= n - m) {
            int j = m - 1;
            
            // Compare from right to left
            while (j >= 0 && pattern.charAt(j) == text.charAt(s + j)) {
                j--;
            }
            
            if (j < 0) {
                // Pattern found
                positions.add(s);
                if (s + m < n) {
                    s += m - badChar.getOrDefault(text.charAt(s + m), -1);
                } else {
                    s += 1;
                }
            } else {
                // Shift pattern
                int shift = j - badChar.getOrDefault(text.charAt(s + j), -1);
                s += Math.max(1, shift);
            }
        }
        
        return positions;
    }
    
    // Example
    public static void main(String[] args) {
        String text = "ABAAABCDABABC";
        String pattern = "ABC";
        
        List<Integer> result = boyerMooreBadChar(text, pattern);
        
        System.out.print("Pattern found at positions: ");
        for (int pos : result) {
            System.out.print(pos + " ");
        }
        System.out.println();
    }
}
```

### Dry Run (Text="ABAAABCDAB", Pattern="ABC")
```
Bad Character Table for "ABC":
{'A': 0, 'B': 1, 'C': 2}

Position 0:
Compare right to left:
text[2]='A' vs pattern[2]='C' ✗
Mismatch at position 2
Bad char 'A' last occurs at 0 in pattern
Shift = max(1, 2-0) = 2

Position 2:
text[4]='A' vs pattern[2]='C' ✗
Mismatch at 'A'
Shift = max(1, 2-0) = 2

Position 4:
text[6]='C' vs pattern[2]='C' ✓
text[5]='B' vs pattern[1]='B' ✓
text[4]='A' vs pattern[0]='A' ✓
Match found at position 4! ✓

Significantly fewer comparisons than naive!
```

### Complexity
- **Best case**: O(n/m) - sublinear!
- **Average case**: O(n)
- **Worst case**: O(n×m)
- **Space**: O(k) where k is alphabet size

---

## 5. Z-Algorithm

### Concept
For each position i, calculate Z[i] = length of longest substring starting from i which is also a prefix.

### Implementation
```python
def z_algorithm(text, pattern):
    """
    Z-algorithm for pattern searching
    Time: O(n + m)
    Space: O(n + m)
    """
    s = pattern + "$" + text
    n = len(s)
    z = [0] * n
    
    left, right = 0, 0
    
    for i in range(1, n):
        if i > right:
            left = right = i
            while right < n and s[right] == s[right - left]:
                right += 1
            z[i] = right - left
            right -= 1
        else:
            k = i - left
            if z[k] < right - i + 1:
                z[i] = z[k]
            else:
                left = i
                while right < n and s[right] == s[right - left]:
                    right += 1
                z[i] = right - left
                right -= 1
    
    # Find matches
    m = len(pattern)
    positions = []
    
    for i in range(len(z)):
        if z[i] == m:
            positions.append(i - m - 1)
    
    return positions

# Example
text = "AABAACAADAABAAABAA"
pattern = "AABA"
result = z_algorithm(text, pattern)
print(f"Pattern found at positions: {result}")
```

### Implementation (C)
```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

/**
 * Z-algorithm for pattern searching
 * Time: O(n + m)
 * Space: O(n + m)
 */
int* z_algorithm(const char* text, const char* pattern, int* count) {
    int n = strlen(text);
    int m = strlen(pattern);
    
    // Concatenate pattern + "$" + text
    char* s = (char*)malloc((n + m + 2) * sizeof(char));
    sprintf(s, "%s$%s", pattern, text);
    int len = strlen(s);
    
    // Compute Z array
    int* z = (int*)calloc(len, sizeof(int));
    int left = 0, right = 0;
    
    for (int i = 1; i < len; i++) {
        if (i > right) {
            left = right = i;
            while (right < len && s[right] == s[right - left]) {
                right++;
            }
            z[i] = right - left;
            right--;
        } else {
            int k = i - left;
            if (z[k] < right - i + 1) {
                z[i] = z[k];
            } else {
                left = i;
                while (right < len && s[right] == s[right - left]) {
                    right++;
                }
                z[i] = right - left;
                right--;
            }
        }
    }
    
    // Find matches
    int* positions = (int*)malloc(n * sizeof(int));
    *count = 0;
    
    for (int i = 0; i < len; i++) {
        if (z[i] == m) {
            positions[(*count)++] = i - m - 1;
        }
    }
    
    free(s);
    free(z);
    return positions;
}

// Example
int main() {
    const char* text = "AABAACAADAABAAABAA";
    const char* pattern = "AABA";
    int count;
    
    int* result = z_algorithm(text, pattern, &count);
    
    printf("Pattern found at positions: ");
    for (int i = 0; i < count; i++) {
        printf("%d ", result[i]);
    }
    printf("\n");
    
    free(result);
    return 0;
}
```

### Implementation (C++)
```cpp
#include <iostream>
#include <vector>
#include <string>
using namespace std;

/**
 * Z-algorithm for pattern searching
 * Time: O(n + m)
 * Space: O(n + m)
 */
vector<int> z_algorithm(const string& text, const string& pattern) {
    string s = pattern + "$" + text;
    int n = s.length();
    vector<int> z(n, 0);
    
    int left = 0, right = 0;
    
    for (int i = 1; i < n; i++) {
        if (i > right) {
            left = right = i;
            while (right < n && s[right] == s[right - left]) {
                right++;
            }
            z[i] = right - left;
            right--;
        } else {
            int k = i - left;
            if (z[k] < right - i + 1) {
                z[i] = z[k];
            } else {
                left = i;
                while (right < n && s[right] == s[right - left]) {
                    right++;
                }
                z[i] = right - left;
                right--;
            }
        }
    }
    
    // Find matches
    int m = pattern.length();
    vector<int> positions;
    
    for (int i = 0; i < n; i++) {
        if (z[i] == m) {
            positions.push_back(i - m - 1);
        }
    }
    
    return positions;
}

// Example
int main() {
    string text = "AABAACAADAABAAABAA";
    string pattern = "AABA";
    
    vector<int> result = z_algorithm(text, pattern);
    
    cout << "Pattern found at positions: ";
    for (int pos : result) {
        cout << pos << " ";
    }
    cout << endl;
    
    return 0;
}
```

### Implementation (Java)
```java
import java.util.ArrayList;
import java.util.List;

public class ZAlgorithmPatternSearch {
    /**
     * Z-algorithm for pattern searching
     * Time: O(n + m)
     * Space: O(n + m)
     */
    public static List<Integer> zAlgorithm(String text, String pattern) {
        String s = pattern + "$" + text;
        int n = s.length();
        int[] z = new int[n];
        
        int left = 0, right = 0;
        
        for (int i = 1; i < n; i++) {
            if (i > right) {
                left = right = i;
                while (right < n && s.charAt(right) == s.charAt(right - left)) {
                    right++;
                }
                z[i] = right - left;
                right--;
            } else {
                int k = i - left;
                if (z[k] < right - i + 1) {
                    z[i] = z[k];
                } else {
                    left = i;
                    while (right < n && s.charAt(right) == s.charAt(right - left)) {
                        right++;
                    }
                    z[i] = right - left;
                    right--;
                }
            }
        }
        
        // Find matches
        int m = pattern.length();
        List<Integer> positions = new ArrayList<>();
        
        for (int i = 0; i < n; i++) {
            if (z[i] == m) {
                positions.add(i - m - 1);
            }
        }
        
        return positions;
    }
    
    // Example
    public static void main(String[] args) {
        String text = "AABAACAADAABAAABAA";
        String pattern = "AABA";
        
        List<Integer> result = zAlgorithm(text, pattern);
        
        System.out.print("Pattern found at positions: ");
        for (int pos : result) {
            System.out.print(pos + " ");
        }
        System.out.println();
    }
}
```

### Z-Array Example
```
String: A A B A A B A A B
Z-array: 0 1 0 2 1 0 4 1 0

Explanation:
Index 0: First position, Z[0] = 0 by convention
Index 1: "AABAABAB" vs "AABAABAB" → match length = 1 (just 'A')
Index 2: "BAABAB" vs "AABAABAB" → no match, Z[2] = 0
Index 3: "ABAABAB" vs "AABAABAB" → match "AB" = 2
Index 4: "ABAB" vs "AABAABAB" → match "A" = 1
Index 5: "BAB" vs "AABAABAB" → no match = 0
Index 6: "AABAB" vs "AABAABAB" → match "AABA" = 4
Index 7: "AB" vs "AABAABAB" → match "A" = 1
Index 8: "B" vs "AABAABAB" → no match = 0
```

### Complexity
- **Time**: O(n + m)
- **Space**: O(n + m)

---

## 6. Aho-Corasick Algorithm

### Concept
Multiple pattern matching using trie + failure links. Efficient for searching multiple patterns simultaneously.

### Implementation
```python
from collections import deque, defaultdict

class AhoCorasick:
    def __init__(self):
        self.goto = defaultdict(dict)
        self.fail = {}
        self.output = defaultdict(list)
        self.state_count = 0
    
    def add_pattern(self, pattern, pattern_id):
        """Add pattern to trie"""
        state = 0
        for char in pattern:
            if char not in self.goto[state]:
                self.state_count += 1
                self.goto[state][char] = self.state_count
            state = self.goto[state][char]
        self.output[state].append(pattern_id)
    
    def build_failure_links(self):
        """Build failure links using BFS"""
        queue = deque()
        
        # Failure links for depth 1
        for char in self.goto[0]:
            state = self.goto[0][char]
            self.fail[state] = 0
            queue.append(state)
        
        # BFS for other depths
        while queue:
            curr_state = queue.popleft()
            
            for char, next_state in self.goto[curr_state].items():
                queue.append(next_state)
                
                fail_state = self.fail[curr_state]
                while fail_state != 0 and char not in self.goto[fail_state]:
                    fail_state = self.fail[fail_state]
                
                self.fail[next_state] = self.goto[fail_state].get(char, 0)
                self.output[next_state].extend(self.output[self.fail[next_state]])
    
    def search(self, text):
        """Search for all patterns in text"""
        state = 0
        results = []
        
        for i, char in enumerate(text):
            while state != 0 and char not in self.goto[state]:
                state = self.fail[state]
            
            state = self.goto[state].get(char, 0)
            
            if self.output[state]:
                for pattern_id in self.output[state]:
                    results.append((i, pattern_id))
        
        return results

# Example
ac = AhoCorasick()
patterns = ["he", "she", "his", "hers"]

for i, pattern in enumerate(patterns):
    ac.add_pattern(pattern, i)

ac.build_failure_links()

text = "ahishers"
matches = ac.search(text)

print("Matches found:")
for pos, pattern_id in matches:
    print(f"Pattern '{patterns[pattern_id]}' ends at position {pos}")
```

### Implementation (C)
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <stdbool.h>

#define MAX_CHARS 256
#define MAX_STATES 1000

typedef struct {
    int goto_table[MAX_STATES][MAX_CHARS];
    int fail[MAX_STATES];
    int output[MAX_STATES][10];  // Store up to 10 pattern IDs per state
    int output_count[MAX_STATES];
    int state_count;
} AhoCorasick;

/**
 * Initialize Aho-Corasick structure
 */
void init_aho_corasick(AhoCorasick* ac) {
    ac->state_count = 0;
    
    for (int i = 0; i < MAX_STATES; i++) {
        for (int j = 0; j < MAX_CHARS; j++) {
            ac->goto_table[i][j] = -1;
        }
        ac->fail[i] = 0;
        ac->output_count[i] = 0;
    }
}

/**
 * Add pattern to trie
 */
void add_pattern(AhoCorasick* ac, const char* pattern, int pattern_id) {
    int state = 0;
    int len = strlen(pattern);
    
    for (int i = 0; i < len; i++) {
        unsigned char ch = (unsigned char)pattern[i];
        if (ac->goto_table[state][ch] == -1) {
            ac->state_count++;
            ac->goto_table[state][ch] = ac->state_count;
        }
        state = ac->goto_table[state][ch];
    }
    
    ac->output[state][ac->output_count[state]++] = pattern_id;
}

/**
 * Build failure links using BFS
 */
void build_failure_links(AhoCorasick* ac) {
    int queue[MAX_STATES];
    int front = 0, rear = 0;
    
    // Failure links for depth 1
    for (int ch = 0; ch < MAX_CHARS; ch++) {
        if (ac->goto_table[0][ch] != -1) {
            int state = ac->goto_table[0][ch];
            ac->fail[state] = 0;
            queue[rear++] = state;
        } else {
            ac->goto_table[0][ch] = 0;
        }
    }
    
    // BFS for other depths
    while (front < rear) {
        int curr_state = queue[front++];
        
        for (int ch = 0; ch < MAX_CHARS; ch++) {
            int next_state = ac->goto_table[curr_state][ch];
            if (next_state == -1) continue;
            
            queue[rear++] = next_state;
            
            int fail_state = ac->fail[curr_state];
            while (ac->goto_table[fail_state][ch] == -1 && fail_state != 0) {
                fail_state = ac->fail[fail_state];
            }
            
            ac->fail[next_state] = (ac->goto_table[fail_state][ch] != -1) 
                                    ? ac->goto_table[fail_state][ch] : 0;
            
            // Copy output from failure state
            for (int i = 0; i < ac->output_count[ac->fail[next_state]]; i++) {
                ac->output[next_state][ac->output_count[next_state]++] = 
                    ac->output[ac->fail[next_state]][i];
            }
        }
    }
}

/**
 * Search for all patterns in text
 */
void search(AhoCorasick* ac, const char* text, const char** patterns) {
    int state = 0;
    int len = strlen(text);
    
    printf("Matches found:\n");
    
    for (int i = 0; i < len; i++) {
        unsigned char ch = (unsigned char)text[i];
        
        while (ac->goto_table[state][ch] == -1 && state != 0) {
            state = ac->fail[state];
        }
        
        state = (ac->goto_table[state][ch] != -1) ? ac->goto_table[state][ch] : 0;
        
        for (int j = 0; j < ac->output_count[state]; j++) {
            int pattern_id = ac->output[state][j];
            printf("Pattern '%s' ends at position %d\n", patterns[pattern_id], i);
        }
    }
}

// Example
int main() {
    AhoCorasick ac;
    init_aho_corasick(&ac);
    
    const char* patterns[] = {"he", "she", "his", "hers"};
    int num_patterns = 4;
    
    for (int i = 0; i < num_patterns; i++) {
        add_pattern(&ac, patterns[i], i);
    }
    
    build_failure_links(&ac);
    
    const char* text = "ahishers";
    search(&ac, text, patterns);
    
    return 0;
}
```

### Implementation (C++)
```cpp
#include <iostream>
#include <vector>
#include <string>
#include <queue>
#include <map>
using namespace std;

class AhoCorasick {
private:
    map<int, map<char, int>> goto_table;
    map<int, int> fail;
    map<int, vector<int>> output;
    int state_count;
    
public:
    AhoCorasick() : state_count(0) {}
    
    /**
     * Add pattern to trie
     */
    void add_pattern(const string& pattern, int pattern_id) {
        int state = 0;
        for (char ch : pattern) {
            if (goto_table[state].find(ch) == goto_table[state].end()) {
                state_count++;
                goto_table[state][ch] = state_count;
            }
            state = goto_table[state][ch];
        }
        output[state].push_back(pattern_id);
    }
    
    /**
     * Build failure links using BFS
     */
    void build_failure_links() {
        queue<int> q;
        
        // Failure links for depth 1
        for (auto& pair : goto_table[0]) {
            int state = pair.second;
            fail[state] = 0;
            q.push(state);
        }
        
        // BFS for other depths
        while (!q.empty()) {
            int curr_state = q.front();
            q.pop();
            
            for (auto& pair : goto_table[curr_state]) {
                char ch = pair.first;
                int next_state = pair.second;
                q.push(next_state);
                
                int fail_state = fail[curr_state];
                while (fail_state != 0 && goto_table[fail_state].find(ch) == goto_table[fail_state].end()) {
                    fail_state = fail[fail_state];
                }
                
                fail[next_state] = (goto_table[fail_state].find(ch) != goto_table[fail_state].end()) 
                                   ? goto_table[fail_state][ch] : 0;
                
                // Copy output from failure state
                for (int pattern_id : output[fail[next_state]]) {
                    output[next_state].push_back(pattern_id);
                }
            }
        }
    }
    
    /**
     * Search for all patterns in text
     */
    vector<pair<int, int>> search(const string& text) {
        int state = 0;
        vector<pair<int, int>> results;
        
        for (int i = 0; i < text.length(); i++) {
            char ch = text[i];
            
            while (state != 0 && goto_table[state].find(ch) == goto_table[state].end()) {
                state = fail[state];
            }
            
            state = (goto_table[state].find(ch) != goto_table[state].end()) 
                    ? goto_table[state][ch] : 0;
            
            for (int pattern_id : output[state]) {
                results.push_back({i, pattern_id});
            }
        }
        
        return results;
    }
};

// Example
int main() {
    AhoCorasick ac;
    vector<string> patterns = {"he", "she", "his", "hers"};
    
    for (int i = 0; i < patterns.size(); i++) {
        ac.add_pattern(patterns[i], i);
    }
    
    ac.build_failure_links();
    
    string text = "ahishers";
    vector<pair<int, int>> matches = ac.search(text);
    
    cout << "Matches found:" << endl;
    for (auto& match : matches) {
        cout << "Pattern '" << patterns[match.second] << "' ends at position " << match.first << endl;
    }
    
    return 0;
}
```

### Implementation (Java)
```java
import java.util.*;

public class AhoCorasickPatternSearch {
    private Map<Integer, Map<Character, Integer>> gotoTable;
    private Map<Integer, Integer> fail;
    private Map<Integer, List<Integer>> output;
    private int stateCount;
    
    public AhoCorasickPatternSearch() {
        gotoTable = new HashMap<>();
        fail = new HashMap<>();
        output = new HashMap<>();
        stateCount = 0;
        gotoTable.put(0, new HashMap<>());
    }
    
    /**
     * Add pattern to trie
     */
    public void addPattern(String pattern, int patternId) {
        int state = 0;
        for (char ch : pattern.toCharArray()) {
            if (!gotoTable.containsKey(state)) {
                gotoTable.put(state, new HashMap<>());
            }
            if (!gotoTable.get(state).containsKey(ch)) {
                stateCount++;
                gotoTable.get(state).put(ch, stateCount);
                gotoTable.put(stateCount, new HashMap<>());
            }
            state = gotoTable.get(state).get(ch);
        }
        
        if (!output.containsKey(state)) {
            output.put(state, new ArrayList<>());
        }
        output.get(state).add(patternId);
    }
    
    /**
     * Build failure links using BFS
     */
    public void buildFailureLinks() {
        Queue<Integer> queue = new LinkedList<>();
        
        // Failure links for depth 1
        for (char ch : gotoTable.get(0).keySet()) {
            int state = gotoTable.get(0).get(ch);
            fail.put(state, 0);
            queue.offer(state);
        }
        
        // BFS for other depths
        while (!queue.isEmpty()) {
            int currState = queue.poll();
            
            if (!gotoTable.containsKey(currState)) continue;
            
            for (Map.Entry<Character, Integer> entry : gotoTable.get(currState).entrySet()) {
                char ch = entry.getKey();
                int nextState = entry.getValue();
                queue.offer(nextState);
                
                int failState = fail.get(currState);
                while (failState != 0 && 
                       (!gotoTable.containsKey(failState) || !gotoTable.get(failState).containsKey(ch))) {
                    failState = fail.get(failState);
                }
                
                if (gotoTable.containsKey(failState) && gotoTable.get(failState).containsKey(ch)) {
                    fail.put(nextState, gotoTable.get(failState).get(ch));
                } else {
                    fail.put(nextState, 0);
                }
                
                // Copy output from failure state
                if (!output.containsKey(nextState)) {
                    output.put(nextState, new ArrayList<>());
                }
                if (output.containsKey(fail.get(nextState))) {
                    output.get(nextState).addAll(output.get(fail.get(nextState)));
                }
            }
        }
    }
    
    /**
     * Search for all patterns in text
     */
    public List<int[]> search(String text) {
        int state = 0;
        List<int[]> results = new ArrayList<>();
        
        for (int i = 0; i < text.length(); i++) {
            char ch = text.charAt(i);
            
            while (state != 0 && 
                   (!gotoTable.containsKey(state) || !gotoTable.get(state).containsKey(ch))) {
                state = fail.get(state);
            }
            
            if (gotoTable.containsKey(state) && gotoTable.get(state).containsKey(ch)) {
                state = gotoTable.get(state).get(ch);
            } else {
                state = 0;
            }
            
            if (output.containsKey(state)) {
                for (int patternId : output.get(state)) {
                    results.add(new int[]{i, patternId});
                }
            }
        }
        
        return results;
    }
    
    // Example
    public static void main(String[] args) {
        AhoCorasickPatternSearch ac = new AhoCorasickPatternSearch();
        String[] patterns = {"he", "she", "his", "hers"};
        
        for (int i = 0; i < patterns.length; i++) {
            ac.addPattern(patterns[i], i);
        }
        
        ac.buildFailureLinks();
        
        String text = "ahishers";
        List<int[]> matches = ac.search(text);
        
        System.out.println("Matches found:");
        for (int[] match : matches) {
            System.out.println("Pattern '" + patterns[match[1]] + "' ends at position " + match[0]);
        }
    }
}
```

### Complexity
- **Preprocessing**: O(sum of pattern lengths)
- **Searching**: O(n + number of matches)
- **Space**: O(sum of pattern lengths)

---

## Pattern Matching Comparison

| Algorithm | Preprocessing | Searching | Space | Best For |
|-----------|--------------|-----------|-------|----------|
| Naive | O(1) | O(n×m) | O(1) | Small texts/patterns |
| KMP | O(m) | O(n) | O(m) | General purpose |
| Rabin-Karp | O(m) | O(n+m) avg | O(1) | Multiple patterns |
| Boyer-Moore | O(m+k) | O(n/m) best | O(k) | Large texts |
| Z-Algorithm | O(n+m) | O(n+m) | O(n+m) | Multiple searches |
| Aho-Corasick | O(Σpatterns) | O(n) | O(Σpatterns) | Multiple patterns |

---

## Exam Tips

### 1. Algorithm Selection
**Question**: Which algorithm for finding "pattern" in 1GB file?
**Answer**: Boyer-Moore (sublinear best case)

**Question**: Search multiple patterns simultaneously?
**Answer**: Aho-Corasick or Rabin-Karp

### 2. Code Completion
**KMP LPS Array**:
```python
if pattern[i] == pattern[length]:
    length += 1
    lps[i] = length  # Missing line
    i += 1
```

### 3. Dry Run Questions
- Trace KMP with LPS array
- Calculate rolling hash for Rabin-Karp
- Boyer-Moore shift calculation

### 4. Complexity Questions
- KMP: O(n + m)
- Boyer-Moore best: O(n/m)
- Naive worst: O(n × m)

### 5. Debugging
**Bug**: Not handling negative hash in Rabin-Karp
**Fix**: `if hash < 0: hash += q`

### Key Concepts
1. **Naive**: Simple but slow O(n×m)
2. **KMP**: Uses LPS array, O(n+m)
3. **Rabin-Karp**: Rolling hash, good for multiple patterns
4. **Boyer-Moore**: Right-to-left, sublinear best case
5. **Aho-Corasick**: Multiple patterns, trie-based
6. **Preprocessing**: Time spent before searching
7. **Online vs Offline**: KMP is online (can process as text arrives)

### Common Mistakes
- Forgetting to preprocess (compute LPS, bad char table)
- Off-by-one errors in sliding window
- Not handling hash collisions in Rabin-Karp
- Incorrect shift calculation in Boyer-Moore

### Practice Problems
1. Find all occurrences of pattern
2. Count pattern occurrences
3. Find longest repeating substring
4. Check if string is rotation of another
5. Multiple pattern matching
