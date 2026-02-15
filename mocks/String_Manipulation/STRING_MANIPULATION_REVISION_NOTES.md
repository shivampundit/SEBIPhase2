# STRING MANIPULATION REVISION NOTES
**Quick Reference for GATE-Level SEBI Exam | 1-Day Study Guide**

---

## 1. STRING BASICS

### Language Differences
| Property | C | C++ | Java | Python |
|----------|---|-----|------|--------|
| Type | char[] | string | String | str |
| Mutability | Mutable | Mutable | Immutable ✓ | Immutable ✓ |
| Null Terminator | Yes ('\0') | No | No | No |
| Length | strlen() | .length()/.size() | .length() | len() |
| Comparison | strcmp() | ==, compare() | .equals() | == |

### Key Operations Time Complexity
| Operation | Time | Notes |
|-----------|------|-------|
| Access char | O(1) | Direct indexing |
| Length | O(1) | Stored in object |
| Concatenation | O(n) | Creates new string if immutable |
| Substring | O(k) | k = substring length |
| Compare | O(min(n,m)) | Character-by-character |
| Search | O(nm) naive, O(n+m) KMP | Pattern matching |

### Important Points
- **Immutable strings**: Java, Python create new objects on modification
- **String pool**: Java literals share references
- **StringBuilder/StringBuffer**: For efficient concatenation in Java
- **C strings**: Always end with '\0', size = length + 1
- **ASCII**: 'A'=65, 'a'=97, '0'=48, difference (A-a) = 32

---

## 2. COMMON STRING OPERATIONS

### Reversal
```
Two-pointer: O(n) time, O(1) space
Stack: O(n) time, O(n) space
```

### Palindrome Check
```
Two-pointer from ends: O(n) time, O(1) space
Reverse and compare: O(n) time, O(n) space
```

### Anagram Check
```
1. Sort both: O(n log n)
2. Frequency map: O(n) time, O(1) space (26 letters)
```

### First Non-Repeating Character
```
Frequency map + single pass: O(n)
```

### Remove Duplicates
```
Hash set: O(n) time, O(n) space
```

### Rotation Check
```
Check if s1 is substring of s2+s2: O(n)
Example: "waterbottle" in "erbottlewaterbottlewat"
```

---

## 3. PATTERN MATCHING ALGORITHMS

### Algorithm Comparison
| Algorithm | Preprocessing | Matching | Total | Use Case |
|-----------|---------------|----------|-------|----------|
| Naive | O(1) | O(nm) | O(nm) | Small inputs |
| KMP | O(m) | O(n) | O(n+m) | General purpose |
| Boyer-Moore | O(m+σ) | O(n/m) best | O(n+m) avg | Large alphabet |
| Rabin-Karp | O(m) | O(n+m) avg | O(n+m) | Multiple patterns |
| Aho-Corasick | O(Σm) | O(n+z) | O(n+Σm+z) | Multiple patterns |

### KMP (Knuth-Morris-Pratt)
- **LPS Array**: Longest Proper Prefix which is also Suffix
- **Build LPS**: For pattern "ABABC" → [0,0,1,2,0]
- **Key**: Don't restart from beginning after mismatch
- **Time**: O(n+m), **Space**: O(m)

### Rabin-Karp
- **Rolling Hash**: Update hash in O(1)
- **Formula**: `hash(next) = (d × (hash - text[i]×h) + text[i+m]) % q`
- **Collision**: Must verify match character-by-character
- **Time**: O(n+m) average, O(nm) worst case

### Boyer-Moore
- **Bad Character Rule**: Skip based on rightmost occurrence
- **Good Suffix Rule**: Additional skip improvement
- **Best Case**: O(n/m) when pattern not in text
- **Time**: O(n+m) average

---

## 4. REGULAR EXPRESSIONS

### Metacharacters
| Symbol | Meaning | Example |
|--------|---------|---------|
| . | Any character | a.c matches "abc", "a c" |
| ^ | Start of string | ^abc matches "abc..." |
| $ | End of string | abc$ matches "...abc" |
| * | Zero or more | a* matches "", "a", "aa" |
| + | One or more | a+ matches "a", "aa" |
| ? | Zero or one | colou?r matches "color", "colour" |
| \d | Digit [0-9] | \d+ matches "123" |
| \w | Word char [a-zA-Z0-9_] | \w+ matches "hello_123" |
| \s | Whitespace | \s+ matches " ", "\t\n" |
| [abc] | Character class | [abc]+ matches "aabbcc" |
| [^abc] | Negated class | [^abc] matches any except a,b,c |
| \| | OR | cat\|dog matches "cat" or "dog" |
| () | Grouping | (ab)+ matches "ab", "abab" |

### Quantifiers
- **Greedy**: `*`, `+`, `?` match as much as possible
- **Non-greedy**: `*?`, `+?`, `??` match as little as possible
- **Example**: `"<.*>"` on "<a><b>" matches whole string (greedy), `"<.*?>"` matches "<a>" only

### Common Patterns
```
Email: ^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$
Phone (US): ^\d{3}-\d{3}-\d{4}$
URL: ^https?://[^\s/$.?#].[^\s]*$
Date (DD/MM/YYYY): ^\d{2}/\d{2}/\d{4}$
IP Address: ^\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}$
```

---

## 5. ADVANCED STRING ALGORITHMS

### Edit Distance (Levenshtein)
- **Operations**: Insert, Delete, Replace
- **DP Formula**: 
  ```
  if s1[i] == s2[j]: dp[i][j] = dp[i-1][j-1]
  else: dp[i][j] = 1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])
                        (delete)     (insert)      (replace)
  ```
- **Time**: O(mn), **Space**: O(mn) or O(min(m,n)) optimized

### Longest Common Subsequence (LCS)
- **Not necessarily contiguous**
- **DP Formula**:
  ```
  if s1[i] == s2[j]: dp[i][j] = 1 + dp[i-1][j-1]
  else: dp[i][j] = max(dp[i-1][j], dp[i][j-1])
  ```
- **Time**: O(mn)

### Longest Common Substring
- **Must be contiguous**
- **DP**: Similar to LCS but reset to 0 on mismatch
- **Time**: O(mn)

### Manacher's Algorithm
- **Problem**: Longest palindromic substring in O(n)
- **Idea**: Expand around center with clever reuse
- **Time**: O(n), **Space**: O(n)

### Z-Algorithm
- **Z[i]**: Length of longest substring starting from i matching prefix
- **Use**: Pattern matching in O(n+m)
- **Example**: "aabxaabxa" → Z = [9,1,0,0,4,1,0,0,1]

### Suffix Array
- **Definition**: Sorted array of all suffixes
- **Build**: O(n log n) or O(n) with advanced methods
- **Use**: Pattern matching, longest repeated substring

### Trie (Prefix Tree)
- **Operations**: Insert, Search, StartsWith all O(L), L = word length
- **Applications**: Autocomplete, spell checker
- **Space**: O(ALPHABET_SIZE × N × L)

---

## 6. STRING TRANSFORMATIONS

### Run-Length Encoding
```
Encode: "aaabbcccc" → "a3b2c4"
Decode: "a3b2c4" → "aaabb"
Time: O(n)
```

### String Compression
```
Check if compressed shorter before applying
"aabcccccaaa" → "a2b1c5a3" (11 → 10 chars)
If not shorter, return original
```

### Permutations
```
Backtracking: O(n! × n)
Count: n!
```

### Substring with Concatenation
```
Sliding window + hash map
Time: O(n × m) where m = word length
```

---

## 7. SLIDING WINDOW PROBLEMS

### Fixed Size Window
```
Max of all subarrays size k
Longest substring with k distinct chars
```

### Variable Size Window
```
Smallest subarray with sum ≥ S
Longest substring without repeating
Minimum window containing all chars of pattern
```

**Template**:
```
left = right = 0
while right < n:
    expand window (add right element)
    while (window invalid):
        shrink from left
    update result
    right++
```

---

## 8. SYNTAX QUICK REFERENCE

### C/C++
```cpp
// C strings
char s[] = "hello";  // length = 5, size = 6 (with '\0')
strlen(s);           // 5
strcmp(s1, s2);      // 0=equal, <0=s1<s2, >0=s1>s2
strcpy(dest, src);
strcat(dest, src);
strstr(haystack, needle); // find substring

// C++ strings
string s = "hello";
s.length(); s.size();    // Both same
s.substr(pos, len);      // Substring
s.find("ell");           // Returns position or string::npos
s.rfind("lo");           // Last occurrence
s.compare(s2);           // Like strcmp
s1 + s2;                 // Concatenation
transform(s.begin(), s.end(), s.begin(), ::tolower);
```

### Java
```java
String s = "hello";
s.length();              // Length
s.charAt(0);             // Access char
s.substring(1, 4);       // "ell" (start inclusive, end exclusive)
s.indexOf("ell");        // Position
s.lastIndexOf("l");      // Last occurrence
s.equals(s2);            // Compare content
s.compareTo(s2);         // Lexicographic
s.toLowerCase();
s.toUpperCase();
s.trim();                // Remove whitespace
s.split(" ");            // Split by delimiter
s.replace("l", "L");     // Replace all
s.contains("ell");       // Check substring

// StringBuilder for efficiency
StringBuilder sb = new StringBuilder();
sb.append("text");
sb.toString();
```

### Python
```python
s = "hello"
len(s)                   # Length
s[0], s[-1]              # First, last char
s[1:4]                   # Slice "ell" (start inclusive, end exclusive)
s.find("ell")            # Position or -1
s.rfind("l")             # Last occurrence
s.lower(), s.upper()
s.strip()                # Remove whitespace
s.split()                # Split by whitespace
s.replace("l", "L")
"ell" in s               # Check substring
s.count("l")             # Count occurrences
s.startswith("he"), s.endswith("lo")
"".join(list)            # Join list to string
```

---

## 9. COMMON PITFALLS

### C/C++
- **Buffer overflow**: Always ensure size for '\0'
- **strcmp vs ==**: Use strcmp for content comparison
- **Memory leak**: Free dynamically allocated strings

### Java
- **String vs StringBuilder**: Use StringBuilder in loops
- **.equals() vs ==**: Use .equals() for content
- **substring()**: Creates new string (memory)

### Python
- **Immutability**: s[0]='a' causes error
- **Slice notation**: [start:end] - end is exclusive

### All Languages
- **Unicode**: Multi-byte characters affect length
- **Case sensitivity**: "ABC" ≠ "abc"
- **Whitespace**: ' ', '\t', '\n' handled differently

---

## 10. PROBLEM PATTERNS

### Two Pointer
- Palindrome, reverse, pair sum, remove duplicates

### Hash Map
- Anagram grouping, first non-repeating, substring problems

### Sliding Window
- Longest substring, minimum window, all anagrams

### DP
- LCS, LCA, edit distance, word break, palindrome partitioning

### Backtracking
- All permutations, all palindrome partitions, word search

### Trie
- Autocomplete, word search in grid, longest common prefix

---

## EXAM TIPS

1. **Language specifics**: Know string mutability, indexing, methods
2. **Edge cases**: Empty string, single char, all same chars
3. **Output questions**: Watch for off-by-one in substring/slice
4. **Pattern matching**: KMP for efficiency, naive if simple
5. **Regex**: Know metacharacters, quantifiers, character classes
6. **Time limits**: Prefer O(n) over O(n²) for large inputs

---

**REVIEW LANGUAGE-SPECIFIC STRING METHODS AND PATTERN MATCHING ALGORITHMS!**
