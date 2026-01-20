# String Manipulation - Complete Revision Guide

## Quick Reference: All Operations, Patterns & Regex (Python, C, C++, Java)

---

## LANGUAGE COMPARISON

### String Properties by Language

| Property | Python | C | C++ | Java |
|----------|--------|---|-----|------|
| **Mutability** | Immutable | Mutable (char[]) | Mutable (char[]), Immutable (string) | Immutable |
| **Indexing** | 0-based, negative | 0-based | 0-based | 0-based |
| **Null-terminated** | No | Yes (`\0`) | Yes (C-style) | No |
| **Built-in Type** | Yes (`str`) | No (char array) | Yes (`std::string`) | Yes (`String`) |
| **Length** | `len(s)` O(1) | `strlen(s)` O(n) | `s.length()` O(1) | `s.length()` O(1) |

### Quick Syntax Reference

| Operation | Python | C | C++ | Java |
|-----------|--------|---|-----|------|
| **Create** | `s = "Hello"` | `char s[] = "Hello";` | `string s = "Hello";` | `String s = "Hello";` |
| **Length** | `len(s)` | `strlen(s)` | `s.length()` | `s.length()` |
| **Access** | `s[0]` | `s[0]` | `s[0]` | `s.charAt(0)` |
| **Substring** | `s[0:5]` | `strncpy()` | `s.substr(0,5)` | `s.substring(0,5)` |
| **Concat** | `s1 + s2` | `strcat()` | `s1 + s2` | `s1 + s2` |
| **Compare** | `s1 == s2` | `strcmp()` | `s1 == s2` | `s1.equals(s2)` |
| **Find** | `s.find("x")` | `strstr()` | `s.find("x")` | `s.indexOf("x")` |
| **Upper** | `s.upper()` | `toupper()` | `transform()` | `s.toUpperCase()` |
| **Split** | `s.split()` | `strtok()` | custom | `s.split()` |
| **Replace** | `s.replace()` | custom | `regex_replace()` | `s.replaceAll()` |

---

## STRING FUNDAMENTALS

### Key Properties (Python-focused)

| Property | Description |
|----------|-------------|
| **Immutable** | Cannot change after creation (Python, Java) |
| **Indexed** | Access by position (0-based) |
| **Iterable** | Can loop through characters |
| **Sequential** | Maintains order |
| **Hashable** | Can be dict key (Python) |

### Time Complexities

| Operation | Time | Notes |
|-----------|------|-------|
| **Access** | O(1) | Direct indexing |
| **Length** | O(1) | Stored property (except C) |
| **Concatenation** | O(n+m) | Creates new string |
| **Slicing** | O(k) | k = slice length |
| **Search** | O(n×m) | Naive, O(n+m) KMP |
| **Split** | O(n) | |
| **Join** | O(n) | Total length |
| **Replace** | O(n) | |

---

## 1. STRING INDEXING & SLICING

### Indexing

```python
s = "Python"
#    0 1 2 3 4 5   (positive indices)
#   -6-5-4-3-2-1   (negative indices)

s[0]    # 'P' (first)
s[-1]   # 'n' (last)
s[2]    # 't'
s[-3]   # 'h'
```

### Slicing Syntax: `s[start:end:step]`

```python
s = "Python Programming"

# Basic slicing
s[0:6]     # 'Python'
s[:6]      # 'Python' (start=0)
s[7:]      # 'Programming' (end=len)
s[:]       # 'Python Programming' (copy)

# Negative indices
s[-11:]    # 'Programming'
s[:-12]    # 'Python'

# Step
s[::2]     # 'Pto rgamn' (every 2nd char)
s[1::2]    # 'yhnPoarmi' (start at 1, every 2nd)
s[::-1]    # 'gnimmargorP nohtyP' (reverse)

# Complex slicing
s[7:18:2]  # 'Pormig'
s[-5:-1]   # 'mmin'
```

### Slicing Rules

| Pattern | Result |
|---------|--------|
| `s[i:j]` | Characters from i to j-1 |
| `s[i:]` | From i to end |
| `s[:j]` | From start to j-1 |
| `s[i:j:k]` | From i to j-1, step k |
| `s[::-1]` | Reverse string |
| Negative start | Count from end |
| Out of range | No error, adjusts automatically |

---

## 2. STRING METHODS - QUICK REFERENCE

### Case Conversion

```python
s = "Hello World"

s.upper()       # 'HELLO WORLD'
s.lower()       # 'hello world'
s.capitalize()  # 'Hello world' (first char upper)
s.title()       # 'Hello World' (each word capitalized)
s.swapcase()    # 'hELLO wORLD'
```

### Search & Find

```python
s = "Hello World Hello"

s.find('World')        # 6 (index of first occurrence)
s.find('Python')       # -1 (not found)
s.find('Hello', 1)     # 12 (start search from index 1)

s.index('World')       # 6 (raises ValueError if not found)
s.count('Hello')       # 2 (number of occurrences)
s.count('l')           # 5

s.startswith('Hello')  # True
s.endswith('Hello')    # True
s.startswith('World')  # False
```

### Trimming

```python
s = "  Hello World  \n"

s.strip()      # 'Hello World'
s.lstrip()     # 'Hello World  \n'
s.rstrip()     # '  Hello World'

# Strip specific characters
"***Hello***".strip('*')  # 'Hello'

# Python 3.9+
"https://example.com".removeprefix('https://')  # 'example.com'
"file.txt".removesuffix('.txt')                 # 'file'
```

### Split & Join

```python
# Split
"a,b,c".split(',')              # ['a', 'b', 'c']
"hello  world".split()          # ['hello', 'world'] (any whitespace)
"a:b:c:d".split(':', 2)         # ['a', 'b', 'c:d'] (max split = 2)
"line1\nline2\nline3".splitlines()  # ['line1', 'line2', 'line3']

# Join
','.join(['a', 'b', 'c'])       # 'a,b,c'
' '.join(['Hello', 'World'])    # 'Hello World'
''.join(['H', 'i'])             # 'Hi'
','.join(map(str, [1,2,3]))     # '1,2,3'
```

### Replace

```python
s = "Hello World"

s.replace('World', 'Python')    # 'Hello Python'
s.replace('l', 'L')             # 'HeLLo WorLd'
s.replace('l', 'L', 2)          # 'HeLLo World' (max 2 replacements)

# Translate (character mapping)
trans = str.maketrans('aeiou', '12345')
"hello".translate(trans)        # 'h2ll4'

# Remove digits
trans = str.maketrans('', '', '0123456789')
"abc123def".translate(trans)    # 'abcdef'
```

### Validation

```python
"hello".isalpha()      # True (all letters)
"hello123".isalnum()   # True (letters + numbers)
"123".isdigit()        # True (all digits)
"   ".isspace()        # True (all whitespace)
"HELLO".isupper()      # True (all uppercase)
"hello".islower()      # True (all lowercase)
"Hello World".istitle() # True (title case)
"123".isnumeric()      # True (numeric characters)
"".isidentifier()      # True if valid Python identifier
```

---

## 3. STRING FORMATTING

### Three Methods Comparison

| Method | Syntax | Python Version |
|--------|--------|----------------|
| **% operator** | `"%s %d" % (name, age)` | Old style |
| **str.format()** | `"{} {}".format(name, age)` | Python 2.6+ |
| **f-strings** | `f"{name} {age}"` | Python 3.6+ ✅ |

### f-strings (Recommended)

```python
name = "Alice"
age = 25
pi = 3.14159

# Basic
f"{name} is {age} years old"

# Expressions
f"{name} will be {age + 1} next year"
f"{2 + 2}"                    # '4'
f"{name.upper()}"             # 'ALICE'

# Formatting
f"{pi:.2f}"                   # '3.14' (2 decimals)
f"{pi:.4f}"                   # '3.1416'
f"{1234:,}"                   # '1,234' (comma separator)
f"{42:05d}"                   # '00042' (zero-padded)

# Alignment
f"{name:>10}"                 # '     Alice' (right)
f"{name:<10}"                 # 'Alice     ' (left)
f"{name:^10}"                 # '  Alice   ' (center)

# Debug (Python 3.8+)
x = 10
f"{x=}"                       # 'x=10'
f"{name=}"                    # 'name=Alice'
```

### Format Specification Mini-Language

```
{value:[[fill]align][sign][#][0][width][,][.precision][type]}

fill    : Any character (default: space)
align   : < (left), > (right), ^ (center), = (padding after sign)
sign    : + (always), - (only negative), space
#       : Alternate form (0b, 0o, 0x prefix)
0       : Zero-padding
width   : Minimum field width
,       : Comma separator
.prec   : Precision for floats
type    : d (int), f (float), s (string), x (hex), etc.
```

### Examples

```python
# Numbers
f"{42:5d}"          # '   42'
f"{42:05d}"         # '00042'
f"{1234567:,}"      # '1,234,567'
f"{255:x}"          # 'ff' (hex)
f"{255:08x}"        # '000000ff'

# Floats
f"{3.14159:.2f}"    # '3.14'
f"{1234.56:10.2f}"  # '   1234.56'
f"{0.123:.2%}"      # '12.30%'

# Alignment with fill
f"{'test':*>10}"    # '******test'
f"{'test':*<10}"    # 'test******'
f"{'test':*^10}"    # '***test***'
```

---

## 4. COMMON STRING PROBLEMS

### Reverse String

```python
# Method 1: Slicing (fastest)
def reverse(s):
    return s[::-1]

# Method 2: Two pointers (in-place for list)
def reverse_list(chars):
    """chars is list of characters"""
    left, right = 0, len(chars) - 1
    while left < right:
        chars[left], chars[right] = chars[right], chars[left]
        left += 1
        right -= 1
    return chars
```

### Palindrome Check

```python
def is_palindrome(s):
    """
    Time: O(n), Space: O(1)
    """
    left, right = 0, len(s) - 1
    
    while left < right:
        if s[left] != s[right]:
            return False
        left += 1
        right -= 1
    
    return True

# Ignore non-alphanumeric
def is_palindrome_clean(s):
    """For: 'A man, a plan, a canal: Panama'"""
    s = ''.join(c.lower() for c in s if c.isalnum())
    return s == s[::-1]
```

### Anagram Check

```python
def is_anagram(s1, s2):
    """
    Time: O(n log n), Space: O(1)
    """
    return sorted(s1) == sorted(s2)

# Using Counter
from collections import Counter

def is_anagram_counter(s1, s2):
    """Time: O(n), Space: O(n)"""
    return Counter(s1) == Counter(s2)
```

### First Non-Repeating Character

```python
def first_non_repeating(s):
    """
    Time: O(n), Space: O(n)
    """
    freq = {}
    
    # Count frequencies
    for char in s:
        freq[char] = freq.get(char, 0) + 1
    
    # Find first with count 1
    for char in s:
        if freq[char] == 1:
            return char
    
    return None
```

### Longest Substring Without Repeating

```python
def length_longest_substring(s):
    """
    Sliding window approach
    Time: O(n), Space: O(min(n, m))
    """
    seen = {}
    max_len = start = 0
    
    for i, char in enumerate(s):
        if char in seen and seen[char] >= start:
            start = seen[char] + 1
        
        seen[char] = i
        max_len = max(max_len, i - start + 1)
    
    return max_len
```

### Valid Parentheses

```python
def is_valid_parentheses(s):
    """
    Time: O(n), Space: O(n)
    """
    stack = []
    pairs = {'(': ')', '[': ']', '{': '}'}
    
    for char in s:
        if char in pairs:
            stack.append(char)
        elif not stack or pairs[stack.pop()] != char:
            return False
    
    return len(stack) == 0
```

### String Compression

```python
def compress(s):
    """
    'aaabbcc' → 'a3b2c2'
    Time: O(n), Space: O(n)
    """
    if not s:
        return ""
    
    result = []
    count = 1
    
    for i in range(1, len(s)):
        if s[i] == s[i-1]:
            count += 1
        else:
            result.append(s[i-1] + str(count))
            count = 1
    
    result.append(s[-1] + str(count))
    
    compressed = ''.join(result)
    return compressed if len(compressed) < len(s) else s
```

---

## 5. PATTERN MATCHING ALGORITHMS

### Naive Pattern Search

```python
def naive_search(text, pattern):
    """
    Time: O(n×m)
    Space: O(1)
    """
    n, m = len(text), len(pattern)
    positions = []
    
    for i in range(n - m + 1):
        if text[i:i+m] == pattern:
            positions.append(i)
    
    return positions
```

### KMP Algorithm

```python
def kmp_search(text, pattern):
    """
    Knuth-Morris-Pratt
    Time: O(n+m)
    Space: O(m)
    """
    def build_lps(pattern):
        """Longest Proper Prefix which is also Suffix"""
        m = len(pattern)
        lps = [0] * m
        length = 0
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
    
    n, m = len(text), len(pattern)
    lps = build_lps(pattern)
    positions = []
    
    i = j = 0
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
```

### Rabin-Karp Algorithm

```python
def rabin_karp(text, pattern):
    """
    Rolling hash approach
    Time: O(n+m) average, O(n×m) worst
    Space: O(1)
    """
    n, m = len(text), len(pattern)
    d = 256  # Number of characters
    q = 101  # Prime number
    
    pattern_hash = 0
    text_hash = 0
    h = pow(d, m-1, q)
    positions = []
    
    # Calculate initial hashes
    for i in range(m):
        pattern_hash = (d * pattern_hash + ord(pattern[i])) % q
        text_hash = (d * text_hash + ord(text[i])) % q
    
    # Slide pattern over text
    for i in range(n - m + 1):
        if pattern_hash == text_hash:
            # Hash match - verify characters
            if text[i:i+m] == pattern:
                positions.append(i)
        
        # Calculate hash for next window
        if i < n - m:
            text_hash = (d * (text_hash - ord(text[i]) * h) + 
                        ord(text[i + m])) % q
            if text_hash < 0:
                text_hash += q
    
    return positions
```

---

## 6. REGULAR EXPRESSIONS (REGEX)

### Metacharacters Quick Reference

| Metachar | Meaning | Example |
|----------|---------|---------|
| `.` | Any character (except \n) | `a.c` → abc, aXc |
| `^` | Start of string | `^Hello` → Hello... |
| `$` | End of string | `World$` → ...World |
| `*` | 0 or more | `ab*` → a, ab, abb |
| `+` | 1 or more | `ab+` → ab, abb |
| `?` | 0 or 1 | `colou?r` → color, colour |
| `{n}` | Exactly n | `\d{3}` → 123 |
| `{n,m}` | Between n and m | `\d{2,4}` → 12, 123, 1234 |
| `{n,}` | n or more | `\d{3,}` → 123, 1234, ... |
| `[]` | Character class | `[aeiou]` → a, e, i, o, u |
| `[^]` | Negated class | `[^0-9]` → not digit |
| `|` | OR | `cat|dog` → cat, dog |
| `()` | Group | `(ab)+` → ab, abab |
| `\` | Escape | `\.` → literal dot |

### Character Classes

| Class | Meaning | Equivalent |
|-------|---------|------------|
| `\d` | Digit | `[0-9]` |
| `\D` | Non-digit | `[^0-9]` |
| `\w` | Word character | `[a-zA-Z0-9_]` |
| `\W` | Non-word | `[^a-zA-Z0-9_]` |
| `\s` | Whitespace | `[ \t\n\r\f\v]` |
| `\S` | Non-whitespace | `[^ \t\n\r\f\v]` |
| `\b` | Word boundary | |
| `\B` | Non-word boundary | |

### Python re Module Functions

```python
import re

text = "Hello 123 World 456"
pattern = r'\d+'

# 1. match() - Match at start only
re.match(pattern, text)        # None (doesn't start with digit)
re.match(r'Hello', text)       # <Match object>

# 2. search() - Find first match anywhere
match = re.search(pattern, text)
if match:
    print(match.group())       # '123'
    print(match.start())       # 6
    print(match.end())         # 9
    print(match.span())        # (6, 9)

# 3. findall() - Find all matches
re.findall(pattern, text)      # ['123', '456']

# 4. finditer() - Iterator of match objects
for match in re.finditer(pattern, text):
    print(match.group(), match.span())

# 5. sub() - Replace matches
re.sub(pattern, 'X', text)     # 'Hello X World X'
re.sub(pattern, 'X', text, count=1)  # 'Hello X World 456'

# 6. split() - Split by pattern
re.split(r'\s+', text)         # ['Hello', '123', 'World', '456']
```

### Groups and Capturing

```python
# Basic groups
pattern = r'(\d{3})-(\d{4})'
match = re.search(pattern, "Phone: 123-4567")

match.group(0)     # '123-4567' (entire match)
match.group(1)     # '123'
match.group(2)     # '4567'
match.groups()     # ('123', '4567')

# Named groups
pattern = r'(?P<area>\d{3})-(?P<num>\d{4})'
match = re.search(pattern, "Phone: 123-4567")

match.group('area')    # '123'
match.group('num')     # '4567'
match.groupdict()      # {'area': '123', 'num': '4567'}

# Non-capturing group (?:...)
pattern = r'(?:Mr|Ms)\. (\w+)'
re.findall(pattern, "Mr. Smith and Ms. Jones")  # ['Smith', 'Jones']
```

### Lookahead and Lookbehind

```python
# Positive lookahead (?=...)
# Match if followed by pattern
re.findall(r'\d+(?= dollars)', "100 dollars and 50 cents")  # ['100']

# Negative lookahead (?!...)
# Match if NOT followed by pattern
re.findall(r'\d+(?! dollars)', "100 dollars and 50 cents")  # ['10', '0', '5', '0']

# Positive lookbehind (?<=...)
# Match if preceded by pattern
re.findall(r'(?<=\$)\d+', "$100 and €50")  # ['100']

# Negative lookbehind (?<!...)
# Match if NOT preceded by pattern
re.findall(r'(?<!\$)\d+', "$100 and 50")  # ['0', '0', '50']
```

### Common Regex Patterns

```python
# Email
email_pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
re.match(email_pattern, "user@example.com")  # Valid

# Phone (US format)
phone_pattern = r'^\(?(\d{3})\)?[-.\s]?(\d{3})[-.\s]?(\d{4})$'
re.match(phone_pattern, "(123) 456-7890")    # Valid
re.match(phone_pattern, "123-456-7890")      # Valid

# URL
url_pattern = r'https?://(?:www\.)?[\w.-]+\.[a-z]{2,}'
re.match(url_pattern, "https://example.com")  # Valid

# IP Address
ip_pattern = r'^(\d{1,3}\.){3}\d{1,3}$'
re.match(ip_pattern, "192.168.1.1")          # Valid

# Password (8+ chars, letter + digit)
pwd_pattern = r'^(?=.*[A-Za-z])(?=.*\d).{8,}$'
re.match(pwd_pattern, "Pass1234")            # Valid

# Date (YYYY-MM-DD)
date_pattern = r'^\d{4}-\d{2}-\d{2}$'
re.match(date_pattern, "2024-01-15")         # Valid

# Credit card (basic, no Luhn check)
cc_pattern = r'^\d{4}[-\s]?\d{4}[-\s]?\d{4}[-\s]?\d{4}$'
re.match(cc_pattern, "1234-5678-9012-3456")  # Valid
```

### Regex Flags

```python
# re.IGNORECASE or re.I
re.search(r'hello', 'HELLO', re.IGNORECASE)  # Match

# re.MULTILINE or re.M
text = "line1\nline2"
re.findall(r'^line', text, re.MULTILINE)     # ['line', 'line']

# re.DOTALL or re.S
re.search(r'a.b', 'a\nb', re.DOTALL)         # Match (. matches \n)

# re.VERBOSE or re.X (allow comments)
pattern = r'''
    \d{3}    # Area code
    -        # Separator
    \d{4}    # Number
'''
re.search(pattern, "123-4567", re.VERBOSE)
```

---

## 7. STRING ALGORITHMS CHEAT SHEET

### Algorithm Comparison

| Algorithm | Time | Space | Use Case |
|-----------|------|-------|----------|
| **Naive Search** | O(n×m) | O(1) | Small patterns |
| **KMP** | O(n+m) | O(m) | Longer patterns, repeated searches |
| **Rabin-Karp** | O(n+m) avg | O(1) | Multiple pattern search |
| **Boyer-Moore** | O(n/m) best | O(m) | Large texts |
| **Aho-Corasick** | O(n+m+z) | O(m) | Multiple patterns simultaneously |

---

## 8. QUICK TIPS & TRICKS

### String Immutability Workaround

```python
# ❌ Slow - creates new string each time
s = ""
for char in "hello":
    s += char  # O(n²) total

# ✅ Fast - use list + join
chars = []
for char in "hello":
    chars.append(char)
s = ''.join(chars)  # O(n) total
```

### Common Patterns

```python
# Remove vowels
''.join(c for c in s if c not in 'aeiouAEIOU')

# Count words
len(s.split())

# Capitalize each word
' '.join(word.capitalize() for word in s.split())

# Remove duplicates (preserve order)
from collections import OrderedDict
''.join(OrderedDict.fromkeys(s))

# Check if all unique
len(s) == len(set(s))

# Most frequent character
from collections import Counter
Counter(s).most_common(1)[0]
```

---

## 9. INTERVIEW CHECKLIST

### Common String Interview Questions

1. ✓ Reverse string
2. ✓ Check palindrome
3. ✓ Find first non-repeating character
4. ✓ Check if anagrams
5. ✓ Longest substring without repeating characters
6. ✓ String compression
7. ✓ Valid parentheses
8. ✓ Group anagrams
9. ✓ Longest common prefix
10. ✓ String to integer (atoi)
11. ✓ Pattern matching (KMP, Rabin-Karp)
12. ✓ Regular expression matching

### Edge Cases to Consider

- Empty string `""`
- Single character
- All same characters `"aaaa"`
- Special characters, spaces
- Unicode characters
- Very long strings
- Case sensitivity

### Time/Space Optimization

| Technique | Benefit |
|-----------|---------|
| **Two pointers** | O(1) space for reversal, palindrome |
| **Hash map** | O(1) lookup for frequency, anagrams |
| **Sliding window** | O(n) for substring problems |
| **StringBuilder** | Avoid O(n²) concatenation |
| **KMP/Rabin-Karp** | O(n+m) pattern matching |

---

## 10. REGEX DECISION TREE

```
Need to validate format?
├─ Email → r'^[\w.+-]+@[\w.-]+\.\w{2,}$'
├─ Phone → r'^\d{3}-\d{3}-\d{4}$'
├─ URL → r'https?://[\w.-]+\.\w{2,}'
└─ Custom → Build pattern

Need to extract data?
├─ Numbers → r'\d+'
├─ Words → r'\w+'
├─ Emails → r'[\w.+-]+@[\w.-]+\.\w+'
└─ Custom → Use groups ()

Need to replace?
├─ Simple → str.replace()
├─ Pattern → re.sub()
└─ Multiple patterns → Multiple re.sub()

Need case-insensitive?
└─ Use re.IGNORECASE flag
```

---

## 11. MULTI-LANGUAGE STRING OPERATIONS

### Common String Problems Across Languages

#### Reverse String

**Python:**
```python
s[::-1]  # Slicing
```

**C:**
```c
// Two pointers, in-place
void reverse(char *s) {
    int l = 0, r = strlen(s) - 1;
    while (l < r) {
        char temp = s[l]; s[l] = s[r]; s[r] = temp;
        l++; r--;
    }
}
```

**C++:**
```cpp
reverse(s.begin(), s.end());  // STL
```

**Java:**
```java
new StringBuilder(s).reverse().toString();
```

---

#### Palindrome Check

**Python:**
```python
s == s[::-1]  # Simple
# Or two pointers for O(1) space
```

**C:**
```c
int is_palindrome(char *s) {
    int l = 0, r = strlen(s) - 1;
    while (l < r) {
        if (s[l++] != s[r--]) return 0;
    }
    return 1;
}
```

**C++:**
```cpp
bool is_palindrome(string s) {
    int l = 0, r = s.length() - 1;
    while (l < r) {
        if (s[l++] != s[r--]) return false;
    }
    return true;
}
```

**Java:**
```java
boolean isPalindrome(String s) {
    int l = 0, r = s.length() - 1;
    while (l < r) {
        if (s.charAt(l++) != s.charAt(r--)) return false;
    }
    return true;
}
```

---

#### Anagram Check

**Python:**
```python
from collections import Counter
Counter(s1) == Counter(s2)  # O(n) time
sorted(s1) == sorted(s2)    # O(n log n) time
```

**C:**
```c
int is_anagram(char *s1, char *s2) {
    int count[256] = {0};
    for (int i = 0; s1[i]; i++) count[s1[i]]++;
    for (int i = 0; s2[i]; i++) count[s2[i]]--;
    for (int i = 0; i < 256; i++) {
        if (count[i] != 0) return 0;
    }
    return 1;
}
```

**C++:**
```cpp
bool is_anagram(string s1, string s2) {
    sort(s1.begin(), s1.end());
    sort(s2.begin(), s2.end());
    return s1 == s2;
}
```

**Java:**
```java
boolean isAnagram(String s1, String s2) {
    char[] a1 = s1.toCharArray();
    char[] a2 = s2.toCharArray();
    Arrays.sort(a1);
    Arrays.sort(a2);
    return Arrays.equals(a1, a2);
}
```

---

### Regular Expressions Across Languages

#### Email Validation Pattern
```
^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$
```

**Python:**
```python
import re
re.match(r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$', email)
```

**C (POSIX):**
```c
#include <regex.h>
regex_t regex;
regcomp(&regex, "^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,}$", REG_EXTENDED);
regexec(&regex, email, 0, NULL, 0);
regfree(&regex);
```

**C++:**
```cpp
#include <regex>
regex pattern(R"(^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$)");
regex_match(email, pattern);
```

**Java:**
```java
import java.util.regex.*;
Pattern.matches("^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,}$", email);
```

---

### String Libraries Reference

#### Python
```python
# Built-in str methods
s.upper(), s.lower(), s.strip()
s.split(), s.join(), s.replace()
s.startswith(), s.endswith()
s.find(), s.index(), s.count()

# re module for regex
import re
re.match(), re.search(), re.findall()
re.sub(), re.split()
```

#### C
```c
// <string.h>
strlen(), strcpy(), strcat(), strcmp()
strstr(), strchr(), strtok()

// <ctype.h>
toupper(), tolower(), isalpha(), isdigit()

// <regex.h> for POSIX regex
regcomp(), regexec(), regfree()
```

#### C++
```cpp
// <string>
s.length(), s.substr(), s.find()
s.replace(), s.compare()

// <algorithm>
reverse(), sort(), transform()

// <regex>
regex, regex_match(), regex_search()
regex_replace(), sregex_iterator
```

#### Java
```java
// String class
s.length(), s.charAt(), s.substring()
s.indexOf(), s.lastIndexOf()
s.replace(), s.replaceAll()
s.split(), String.join()
s.toUpperCase(), s.toLowerCase()
s.trim(), s.strip() // Java 11+
s.startsWith(), s.endsWith()
s.contains(), s.equals()

// StringBuilder for mutable strings
StringBuilder sb = new StringBuilder();
sb.append(), sb.reverse()

// Pattern/Matcher for regex
Pattern.compile(), matcher.find()
matcher.group(), matcher.replaceAll()
```

---

## 12. LANGUAGE-SPECIFIC GOTCHAS

### Python
- ✅ Strings are immutable - create new string for changes
- ✅ Negative indexing: `s[-1]` for last character
- ✅ Slicing: `s[start:end:step]`
- ⚠️ Use `''.join(list)` for concatenation in loops (not `+=`)

### C
- ⚠️ Always null-terminate strings with `'\0'`
- ⚠️ `strlen()` is O(n) - cache if using multiple times
- ⚠️ Buffer overflow risk - use `strncpy()`, `strncat()`
- ⚠️ Must manually allocate/free memory

### C++
- ✅ Use `std::string`, not C-style char arrays
- ✅ `s.length()` and `s.size()` are equivalent
- ⚠️ Use `.c_str()` when C functions needed
- ⚠️ Include `<string>` for string, `<cstring>` for C functions

### Java
- ⚠️ Use `.equals()` for comparison, NOT `==`
- ⚠️ `==` compares references, not content
- ✅ Strings are immutable - use `StringBuilder` for mutations
- ✅ Use `+` for simple concat, `StringBuilder` for loops
- ⚠️ Regex patterns need double backslash: `"\\d+"` not `"\d+"`

---

## END OF STRING MANIPULATION REVISION GUIDE

**Master strings across Python, C, C++, and Java for comprehensive programming skills! 🚀**

### Additional Resources
- For detailed examples: See `01_String_Operations/String_Operations.md`
- For regex patterns: See `02_Regex/Regular_Expressions.md`
- For practice: See `STRING_MANIPULATION_MCQ.md`
