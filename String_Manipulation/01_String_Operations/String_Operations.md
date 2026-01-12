# String Operations - Comprehensive Notes

## Overview
Strings are immutable sequences of characters. Fundamental for text processing, pattern matching, and data manipulation.

### Key Properties
- **Immutable**: Cannot be changed after creation
- **Indexed**: Access by position (0-based)
- **Iterable**: Can loop through characters
- **Sequential**: Maintains character order

---

## String Basics

### 1. String Creation

```python
# Different ways to create strings
s1 = 'Hello'                    # Single quotes
s2 = "World"                    # Double quotes
s3 = '''Multi
line'''                          # Triple quotes
s4 = str(123)                    # Convert to string
s5 = "Hello" + " " + "World"    # Concatenation
s6 = "Python" * 3                # Repetition: PythonPythonPython
```

---

### 2. String Length

```python
def string_length(s):
    """
    Get length of string
    Time: O(1) - stored as property
    Space: O(1)
    """
    return len(s)

# Example
text = "Hello World"
print(len(text))  # 11

# Empty string
empty = ""
print(len(empty))  # 0

# String with spaces
spaced = "   "
print(len(spaced))  # 3
```

---

### 3. String Indexing and Slicing

```python
s = "Python Programming"

# Indexing
print(s[0])      # 'P' - first character
print(s[-1])     # 'g' - last character
print(s[-2])     # 'n' - second last

# Slicing: s[start:end:step]
print(s[0:6])    # 'Python'
print(s[:6])     # 'Python' - from start
print(s[7:])     # 'Programming' - to end
print(s[-11:])   # 'Programming' - last 11 chars
print(s[::2])    # 'Pto rgamn' - every 2nd char
print(s[::-1])   # 'gnimmargorP nohtyP' - reverse

# Slice with negative indices
print(s[-11:-1])  # 'Programmin'
```

#### Dry Run: Slicing
```
String: "Python"
Indices:  0  1  2  3  4  5
         -6 -5 -4 -3 -2 -1

s[1:4]  → s[1], s[2], s[3] → 'yth'
s[-3:]  → s[3], s[4], s[5] → 'hon'
s[::2]  → s[0], s[2], s[4] → 'Pto'
s[::-1] → reverse → 'nohtyP'
```

---

### 4. String Traversal

```python
def traverse_string(s):
    """
    Different ways to traverse string
    Time: O(n)
    """
    # Method 1: By index
    for i in range(len(s)):
        print(s[i], end=" ")
    print()
    
    # Method 2: Direct iteration
    for char in s:
        print(char, end=" ")
    print()
    
    # Method 3: Enumerate (index + char)
    for i, char in enumerate(s):
        print(f"{i}:{char}", end=" ")
    print()

# Example
traverse_string("Hello")
# H e l l o
# H e l l o
# 0:H 1:e 2:l 3:l 4:o
```

---

## String Methods

### 1. Case Conversion

```python
s = "Hello World"

# Case methods
print(s.upper())       # 'HELLO WORLD'
print(s.lower())       # 'hello world'
print(s.capitalize())  # 'Hello world'
print(s.title())       # 'Hello World'
print(s.swapcase())    # 'hELLO wORLD'

# Check case
print(s.isupper())     # False
print(s.islower())     # False
print(s.istitle())     # True

# Practical example
def normalize_name(name):
    """Convert name to title case"""
    return name.strip().title()

print(normalize_name("  john doe  "))  # 'John Doe'
```

---

### 2. Search Methods

```python
s = "Hello World Hello"

# Find (returns index or -1)
print(s.find('World'))     # 6
print(s.find('Python'))    # -1
print(s.find('Hello', 1))  # 12 - start from index 1

# Index (raises ValueError if not found)
print(s.index('World'))    # 6
# print(s.index('Python')) # ValueError

# Count occurrences
print(s.count('Hello'))    # 2
print(s.count('l'))        # 5

# Check start/end
print(s.startswith('Hello'))  # True
print(s.endswith('Hello'))    # True
print(s.startswith('World'))  # False
```

#### Dry Run: Find
```
s = "Hello World Hello"
     012345678901234

s.find('World'):
- Search from index 0
- Match at index 6
- Return 6

s.find('Hello', 1):
- Search from index 1
- Skip first 'Hello' at 0
- Match at index 12
- Return 12
```

---

### 3. String Trimming

```python
s = "  Hello World  \n"

# Remove whitespace
print(s.strip())       # 'Hello World'
print(s.lstrip())      # 'Hello World  \n'
print(s.rstrip())      # '  Hello World'

# Remove specific characters
email = "user@example.com"
print(email.strip('.com'))  # 'user@example'

# Remove prefix/suffix (Python 3.9+)
url = "https://example.com"
print(url.removeprefix('https://'))  # 'example.com'
print(url.removesuffix('.com'))       # 'https://example'
```

---

### 4. String Splitting and Joining

```python
# Split
s = "apple,banana,cherry"
fruits = s.split(',')
print(fruits)  # ['apple', 'banana', 'cherry']

# Split by whitespace
text = "Hello  World  Python"
words = text.split()
print(words)  # ['Hello', 'World', 'Python']

# Split with limit
data = "a:b:c:d:e"
parts = data.split(':', 2)
print(parts)  # ['a', 'b', 'c:d:e']

# Splitlines
multiline = "Line1\nLine2\nLine3"
lines = multiline.splitlines()
print(lines)  # ['Line1', 'Line2', 'Line3']

# Join
words = ['Hello', 'World']
sentence = ' '.join(words)
print(sentence)  # 'Hello World'

# Join numbers (convert first)
numbers = [1, 2, 3]
result = ', '.join(map(str, numbers))
print(result)  # '1, 2, 3'
```

---

### 5. String Replacement

```python
s = "Hello World"

# Replace
print(s.replace('World', 'Python'))     # 'Hello Python'
print(s.replace('l', 'L'))              # 'HeLLo WorLd'
print(s.replace('l', 'L', 2))           # 'HeLLo World' - max 2

# Translate (character mapping)
# Remove vowels
vowels = str.maketrans('aeiou', '12345')
text = "hello world"
print(text.translate(vowels))  # 'h2ll4 w4rld'

# Remove characters
remove_digits = str.maketrans('', '', '0123456789')
text2 = "abc123def456"
print(text2.translate(remove_digits))  # 'abcdef'
```

---

### 6. String Validation

```python
# Check content type
print("hello".isalpha())      # True - all letters
print("hello123".isalnum())   # True - letters + numbers
print("123".isdigit())        # True - all digits
print("   ".isspace())        # True - all whitespace
print("HELLO".isupper())      # True - all uppercase
print("hello".islower())      # True - all lowercase

# Practical validation
def validate_username(username):
    """
    Valid username: 3-20 chars, alphanumeric + underscore
    """
    if len(username) < 3 or len(username) > 20:
        return False
    
    for char in username:
        if not (char.isalnum() or char == '_'):
            return False
    
    return True

print(validate_username("user_123"))   # True
print(validate_username("user@123"))   # False
```

---

## String Formatting

### 1. Old-style Formatting

```python
name = "Alice"
age = 25

# % operator
print("Name: %s, Age: %d" % (name, age))
# 'Name: Alice, Age: 25'

# Named placeholders
print("%(name)s is %(age)d years old" % {"name": name, "age": age})
```

---

### 2. str.format()

```python
name = "Bob"
age = 30

# Positional
print("{} is {} years old".format(name, age))

# Indexed
print("{0} is {1} years old. {0} likes Python.".format(name, age))

# Named
print("{name} is {age} years old".format(name=name, age=age))

# With formatting
pi = 3.14159
print("{:.2f}".format(pi))  # '3.14'

# Alignment
print("{:>10}".format("right"))   # '     right'
print("{:<10}".format("left"))    # 'left      '
print("{:^10}".format("center"))  # '  center  '
```

---

### 3. f-strings (Python 3.6+)

```python
name = "Charlie"
age = 28

# Basic
print(f"{name} is {age} years old")

# Expressions
print(f"{name} will be {age + 1} next year")

# Formatting
pi = 3.14159
print(f"Pi: {pi:.2f}")  # 'Pi: 3.14'

# Alignment
print(f"{name:>10}")    # '   Charlie'

# Debug (Python 3.8+)
x = 10
print(f"{x=}")          # 'x=10'

# Multi-line
message = (
    f"Name: {name}\n"
    f"Age: {age}\n"
    f"Next year: {age + 1}"
)
```

---

## Substring Operations

### 1. Extract Substring

```python
def extract_substring(s, start, length):
    """
    Extract substring starting at index
    Time: O(k) where k = length
    """
    return s[start:start+length]

# Example
text = "Python Programming"
print(extract_substring(text, 0, 6))   # 'Python'
print(extract_substring(text, 7, 11))  # 'Programming'
```

---

### 2. Check Substring

```python
def contains_substring(s, sub):
    """
    Check if substring exists
    Time: O(n*m) naive, O(n+m) KMP
    """
    return sub in s

# Example
text = "Hello World"
print(contains_substring(text, "World"))   # True
print(contains_substring(text, "Python"))  # False

# Case-insensitive
def contains_ignore_case(s, sub):
    return sub.lower() in s.lower()

print(contains_ignore_case("Hello World", "world"))  # True
```

---

### 3. Find All Occurrences

```python
def find_all_occurrences(s, sub):
    """
    Find all starting indices of substring
    Time: O(n*m)
    """
    positions = []
    start = 0
    
    while True:
        index = s.find(sub, start)
        if index == -1:
            break
        positions.append(index)
        start = index + 1
    
    return positions

# Example
text = "hello world hello python hello"
print(find_all_occurrences(text, "hello"))
# [0, 12, 25]
```

#### Dry Run
```
s = "hello world hello"
sub = "hello"

start = 0:
  index = s.find("hello", 0) = 0
  positions = [0]
  start = 0 + 1 = 1

start = 1:
  index = s.find("hello", 1) = 12
  positions = [0, 12]
  start = 12 + 1 = 13

start = 13:
  index = s.find("hello", 13) = -1
  break

Result: [0, 12]
```

---

### 4. Longest Common Substring

```python
def longest_common_substring(s1, s2):
    """
    Find longest common substring
    Time: O(m*n)
    Space: O(m*n)
    """
    m, n = len(s1), len(s2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    
    max_length = 0
    end_index = 0
    
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if s1[i-1] == s2[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1
                
                if dp[i][j] > max_length:
                    max_length = dp[i][j]
                    end_index = i
    
    return s1[end_index - max_length:end_index]

# Example
s1 = "abcdefgh"
s2 = "xbcdeyz"
print(longest_common_substring(s1, s2))  # 'bcde'
```

---

## String Comparison

```python
# Equality
print("hello" == "hello")    # True
print("hello" == "Hello")    # False

# Lexicographic comparison
print("apple" < "banana")    # True
print("abc" < "abd")         # True

# Case-insensitive comparison
s1 = "Hello"
s2 = "hello"
print(s1.lower() == s2.lower())  # True

# Compare ignoring whitespace
def compare_ignore_space(s1, s2):
    return s1.replace(" ", "") == s2.replace(" ", "")

print(compare_ignore_space("hello world", "helloworld"))  # True
```

---

## Common String Problems

### 1. Reverse String

```python
def reverse_string(s):
    """
    Reverse string
    Time: O(n)
    Space: O(n)
    """
    # Method 1: Slicing
    return s[::-1]

# Method 2: Reverse list
def reverse_string_list(s):
    chars = list(s)
    chars.reverse()
    return ''.join(chars)

# Method 3: Two pointers (in-place for list)
def reverse_chars(chars):
    """chars is list of characters"""
    left, right = 0, len(chars) - 1
    
    while left < right:
        chars[left], chars[right] = chars[right], chars[left]
        left += 1
        right -= 1
    
    return chars
```

---

### 2. Palindrome Check

```python
def is_palindrome(s):
    """
    Check if string is palindrome
    Time: O(n)
    Space: O(1)
    """
    left, right = 0, len(s) - 1
    
    while left < right:
        if s[left] != s[right]:
            return False
        left += 1
        right -= 1
    
    return True

# Ignore non-alphanumeric
def is_palindrome_alphanumeric(s):
    """
    Palindrome check ignoring non-alphanumeric
    """
    s = ''.join(c.lower() for c in s if c.isalnum())
    return s == s[::-1]

print(is_palindrome("racecar"))  # True
print(is_palindrome_alphanumeric("A man, a plan, a canal: Panama"))  # True
```

---

### 3. Anagram Check

```python
from collections import Counter

def is_anagram(s1, s2):
    """
    Check if two strings are anagrams
    Time: O(n)
    Space: O(1) - at most 26 letters
    """
    if len(s1) != len(s2):
        return False
    
    return Counter(s1) == Counter(s2)

# Alternative: Sorting
def is_anagram_sort(s1, s2):
    """Time: O(n log n)"""
    return sorted(s1) == sorted(s2)

print(is_anagram("listen", "silent"))  # True
print(is_anagram("hello", "world"))    # False
```

---

### 4. First Unique Character

```python
from collections import Counter

def first_unique_char(s):
    """
    Find index of first unique character
    Time: O(n)
    Space: O(1) - at most 26 letters
    """
    count = Counter(s)
    
    for i, char in enumerate(s):
        if count[char] == 1:
            return i
    
    return -1

print(first_unique_char("leetcode"))    # 0 ('l')
print(first_unique_char("loveleetcode")) # 2 ('v')
```

---

### 5. Longest Substring Without Repeating

```python
def length_of_longest_substring(s):
    """
    Length of longest substring without repeating chars
    Time: O(n)
    Space: O(min(m, n)) where m = charset size
    """
    char_index = {}
    max_length = 0
    start = 0
    
    for end, char in enumerate(s):
        if char in char_index and char_index[char] >= start:
            start = char_index[char] + 1
        
        char_index[char] = end
        max_length = max(max_length, end - start + 1)
    
    return max_length

print(length_of_longest_substring("abcabcbb"))  # 3 ('abc')
print(length_of_longest_substring("bbbbb"))     # 1 ('b')
print(length_of_longest_substring("pwwkew"))    # 3 ('wke')
```

---

### 6. String Compression

```python
def compress_string(s):
    """
    Compress string: 'aabcccccaaa' → 'a2b1c5a3'
    Time: O(n)
    Space: O(n)
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
    
    # Add last group
    result.append(s[-1] + str(count))
    
    compressed = ''.join(result)
    return compressed if len(compressed) < len(s) else s

print(compress_string("aabcccccaaa"))  # 'a2b1c5a3'
print(compress_string("abc"))          # 'abc' (no compression)
```

---

## Time Complexity Summary

| Operation | Time | Space |
|-----------|------|-------|
| Length | O(1) | O(1) |
| Access by index | O(1) | O(1) |
| Slicing | O(k) | O(k) |
| Concatenation | O(n+m) | O(n+m) |
| Search (in) | O(n*m) | O(1) |
| Replace | O(n) | O(n) |
| Split | O(n) | O(n) |
| Join | O(n) | O(n) |

---

## Exam Tips

### 1. MCQ Questions
**Q**: Time to find length of string?
**A**: O(1)

**Q**: Strings in Python are?
**A**: Immutable

**Q**: s[::-1] does what?
**A**: Reverses the string

**Q**: Time for substring search?
**A**: O(n*m) naive, O(n+m) KMP

### 2. Common Patterns
- **Two pointers**: Palindrome, reverse
- **Sliding window**: Longest substring
- **Hash map**: Anagrams, unique chars
- **String builder**: Avoid repeated concatenation
- **Slicing**: Quick substring operations

### Key Points
1. **Immutable**: Create new string for changes
2. **Indexing**: 0-based, negative for reverse
3. **Slicing**: s[start:end:step]
4. **Methods**: Many built-in string methods
5. **Formatting**: f-strings preferred (Python 3.6+)
6. **Comparison**: Lexicographic order
7. **Search**: Use `in` operator or methods
8. **Performance**: Use join() for multiple concatenations
