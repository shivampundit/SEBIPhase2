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
