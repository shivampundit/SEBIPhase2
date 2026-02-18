# String Manipulation Quick Revision - Multi-Language
## **Last Minute Revision - Length | Substring | Regex | Search**

### 📚 Coverage: C | C++ | Java | Python
### ⚡ Focus: Concepts over Code | Tables | Quick Reference
### 🎯 Syllabus: **Length** | **Substring** | **Regex** | **Search** (Very Deep)

---

## 1. STRING LENGTH OPERATIONS

### 1.1 Basic Length Functions

| Language | Function/Method | Returns | Includes Null? | Time |
|----------|----------------|---------|----------------|------|
| **C** | `strlen(str)` | int | ❌ Excludes '\0' | O(n) |
| **C** | `sizeof(str)` | size_t | ✅ Includes '\0' | O(1) |
| **C++** | `str.length()` | size_t | ❌ Character count | O(1) |
| **C++** | `str.size()` | size_t | ❌ Same as length() | O(1) |
| **Java** | `str.length()` | int | ❌ Character count | O(1) |
| **Python** | `len(str)` | int | ❌ Character count | O(1) |

### 1.2 Critical Differences

**C - Arrays vs Pointers:**
```c
char str1[] = "Hello";       // Array
char *str2 = "Hello";        // Pointer

sizeof(str1);  // 6 (5 chars + '\0')
sizeof(str2);  // 4 or 8 (pointer size!)

strlen(str1);  // 5
strlen(str2);  // 5
```

**Key Rules:**
- `sizeof(array)` → Total bytes including '\0'
- `sizeof(pointer)` → Pointer size (4 on 32-bit, 8 on 64-bit)
- `strlen()` → Counts until '\0' (excludes null terminator)

### 1.3 Edge Cases Table

| Case | C/C++ `strlen` | Java `length()` | Python `len()` |
|------|---------------|-----------------|---------------|
| Empty string `""` | 0 | 0 | 0 |
| Single char `"a"` | 1 | 1 | 1 |
| Space `" "` | 1 | 1 | 1 |
| Null char `"\0"` | 0 | 1 (treats as char) | 1 |
| Multi-byte UTF-8 | Byte count | Character count | Character count |
| Null pointer (C/C++) | Undefined behavior | N/A | N/A |

### 1.4 Unicode/Multibyte Length

| Language | Method | Counts What? | Example |
|----------|--------|-------------|---------|
| **C** | `strlen("你好")` | **Bytes** | 6 (UTF-8: 3 bytes each) |
| **C** | `mbstowcs()` + `wcslen()` | **Characters** | 2 |
| **C++** | `std::string.length()` | **Bytes** | 6 |
| **C++** | `std::wstring.length()` | **Characters** | 2 |
| **Java** | `str.length()` | **UTF-16 units** | 2 |
| **Java** | `str.codePointCount()` | **Characters** | 2 |
| **Python** | `len("你好")` | **Characters** | 2 |

### 1.5 Performance Comparison

| Operation | Time | Space | Notes |
|-----------|------|-------|-------|
| **C `strlen()`** | O(n) | O(1) | Iterates to find '\0' |
| **C++ `str.length()`** | O(1) | O(1) | Stores length internally |
| **Java `str.length()`** | O(1) | O(1) | Final field access |
| **Python `len()`** | O(1) | O(1) | Ob_size field |

---

## 2. SUBSTRING OPERATIONS (IN DEPTH)

### 2.1 Substring Syntax Comparison

| Language | Syntax | Parameters | Example | Result |
|----------|--------|-----------|---------|--------|
| **C** | `strncpy(dest, src+start, len)` | Manual | `strncpy(d, s+2, 3)` from "hello" | "llo" |
| **C** | `memcpy(dest, src+start, len)` | Manual | `memcpy(d, s+1, 3)` from "hello" | "ell" |
| **C++** | `str.substr(pos, len)` | pos, length | `str.substr(1, 3)` from "hello" | "ell" |
| **Java** | `str.substring(start, end)` | start, **end** | `str.substring(1, 4)` from "hello" | "ell" |
| **Python** | `str[start:end:step]` | start, end, step | `"hello"[1:4]` | "ell" |

### 2.2 Index Rules (Critical!)

| Language | Start Index | End Index | End Inclusive? |
|----------|------------|-----------|---------------|
| **C** | Manual pointer | Manual length | N/A |
| **C++** | 0-based | Length parameter | ✅ Includes last |
| **Java** | 0-based | End index | ❌ Excludes end |
| **Python** | 0-based (or -1 from end) | End index | ❌ Excludes end |

**Example "HELLO" (indices 0-4):**
```
H  E  L  L  O
0  1  2  3  4  (forward)
-5 -4 -3 -2 -1 (Python backward)
```

### 2.3 Substring Edge Cases

| Operation | C++ | Java | Python |
|-----------|-----|------|--------|
| **Empty substring** | `substr(0, 0)` → `""` | `substring(0, 0)` → `""` | `"hello"[0:0]` → `""` |
| **Start = 0** | `substr(0, 3)` → "hel" | `substring(0, 3)` → "hel" | `"hello"[0:3]` → "hel" |
| **From middle to end** | `substr(2)` → "llo" | `substring(2)` → "llo" | `"hello"[2:]` → "llo" |
| **Last character** | `substr(4, 1)` → "o" | `substring(4, 5)` → "o" | `"hello"[4]` or `[-1]` → "o" |
| **Out of bounds** | Undefined/Exception | Exception | Returns available |
| **Negative indices** | Not supported | Not supported | ✅ Supported! |

### 2.4 Python Slicing (Deep Dive)

| Slice | From "HELLO" | Result | Rule |
|-------|-------------|--------|------|
| `[1:4]` | Indices 1,2,3 | "ELL" | Excludes end |
| `[:3]` | Start to 3 | "HEL" | Implicit start=0 |
| `[2:]` | From 2 to end | "LLO" | Implicit end=len |
| `[:]` | Full copy | "HELLO" | Complete slice |
| `[-2:]` | Last 2 chars | "LO" | Negative start |
| `[:-2]` | All except last 2 | "HEL" | Negative end |
| `[-4:-1]` | Indices -4,-3,-2 | "ELL" | Both negative |
| `[::2]` | Every 2nd char | "HLO" | Step=2 |
| `[::-1]` | Reverse | "OLLEH" | Step=-1 |
| `[1:5:2]` | Start=1, step=2 | "EL" | All params |

### 2.5 Advanced Substring Operations

| Operation | C++ | Java | Python |
|-----------|-----|------|--------|
| **Find & extract** | `find()` + `substr()` | `indexOf()` + `substring()` | `find()` + slice |
| **Split by delimiter** | Manual loop | `split()` | `split()` |
| **Multiple substrings** | Loop + `substr()` | `split()` or regex | List comprehension |
| **Replace substring** | `replace(pos, len, str)` | `replace()` | `replace()` |

**Example: Extract between delimiters**
```python
# "Name: John, Age: 30" → Extract "John"
s = "Name: John, Age: 30"

# C++ approach
size_t start = s.find(": ") + 2;
size_t end = s.find(",");
string name = s.substr(start, end - start);  // "John"

# Java approach
int start = s.indexOf(": ") + 2;
int end = s.indexOf(",");
String name = s.substring(start, end);  // "John"

# Python approach
name = s[s.find(": ")+2:s.find(",")]  # "John"
# Or using split:
name = s.split(": ")[1].split(",")[0]  # "John"
```

### 2.6 Performance Analysis

| Operation | Time | Space | Notes |
|-----------|------|-------|-------|
| **Java `substring()`** | O(1)* | O(1)* | JDK 7+: creates new array (O(n)) |
| **Java `substring()` old** | O(1) | O(1) | JDK 6-: shared char array |
| **C++ `substr()`** | O(n) | O(n) | Always copies |
| **Python `slice`** | O(k) | O(k) | k = slice length |

---

## 3. REGULAR EXPRESSIONS (VERY DEEP)

### 3.1 Regex Module/Library

| Language | Import/Include | Compile | Match/Search |
|----------|---------------|---------|--------------|
| **C** | `#include <regex.h>` | `regcomp()` | `regexec()` |
| **C++** | `#include <regex>` | `std::regex pattern` | `regex_match()`, `regex_search()` |
| **Java** | `import java.util.regex.*` | `Pattern.compile()` | `matcher.find()` |
| **Python** | `import re` | `re.compile()` | `re.search()`, `re.match()` |

### 3.2 Core Regex Patterns

| Symbol | Meaning | Example | Matches |
|--------|---------|---------|---------|
| `.` | Any character (except \n) | `a.c` | "abc", "a1c", "a c" |
| `^` | Start of string | `^Hello` | "Hello world" ✅, "Say Hello" ❌ |
| `$` | End of string | `world$` | "Hello world" ✅, "world Hello" ❌ |
| `*` | 0 or more | `ab*c` | "ac", "abc", "abbbbc" |
| `+` | 1 or more | `ab+c` | "abc", "abbc" (NOT "ac") |
| `?` | 0 or 1 (optional) | `ab?c` | "ac", "abc" (NOT "abbc") |
| `{n}` | Exactly n | `a{3}` | "aaa" |
| `{n,}` | n or more | `a{2,}` | "aa", "aaa", "aaaa..." |
| `{n,m}` | Between n and m | `a{2,4}` | "aa", "aaa", "aaaa" |
| `[]` | Character class | `[aeiou]` | Any vowel |
| `[^]` | Negated class | `[^0-9]` | Non-digit |
| `|` | OR (alternation) | `cat|dog` | "cat" or "dog" |
| `()` | Group/Capture | `(ab)+` | "ab", "abab", "ababab" |
| `\d` | Digit | `\d{3}` | "123", "456" |
| `\D` | Non-digit | `\D+` | "abc", "xyz" |
| `\w` | Word char [A-Za-z0-9_] | `\w+` | "hello_123" |
| `\W` | Non-word char | `\W+` | "!@#$" |
| `\s` | Whitespace | `\s+` | " ", "\t", "\n" |
| `\S` | Non-whitespace | `\S+` | "hello" |
| `\b` | Word boundary | `\bcat\b` | "cat" in "the cat sat" ✅ |
| `\B` | Non-word boundary | `\Bcat` | "cat" in "bobcat" ✅ |

### 3.3 Character Classes (Deep)

| Class | Equivalent | Matches | Example |
|-------|-----------|---------|---------|
| `[abc]` | a or b or c | Single char from set | `[aeiou]` = vowel |
| `[a-z]` | a through z | Lowercase letters | `[a-zA-Z]` = any letter |
| `[0-9]` | 0 through 9 | Digits | `[0-9]{3}` = 3 digits |
| `[^abc]` | NOT a, b, or c | Complement | `[^0-9]` = non-digit |
| `[\d]` | Same as [0-9] | Digit | `[\d]+` = integer |
| `[\w]` | [A-Za-z0-9_] | Word character | `[\w]+` = identifier |
| `[\s]` | [ \t\n\r\f\v] | Whitespace | `[\s]+` = spaces |
| `[.-]` | . or - | Literal within [] | `[a-z.-]` matches letters, dot, hyphen |

### 3.4 Escape Sequences

| In Regex | Matches | Example | C/C++/Java String | Python Raw |
|----------|---------|---------|------------------|-----------|
| `\.` | Literal dot | `file.txt` | `"\\."` | `r"\."` |
| `\*` | Literal asterisk | `2*3` | `"\\*"` | `r"\*"` |
| `\\` | Literal backslash | `C:\path` | `"\\\\"` | `r"\\"` |
| `\(` | Literal paren | `(hello)` | `"\\("` | `r"\("` |
| `\$` | Literal dollar | `$100` | `"\\$"` | `r"\$"` |
| `\^` | Literal caret | `^top` | `"\\^"` | `r"\^"` |

**Critical: Double Escaping!**
```java
// Java: Escape for string AND regex
String pattern = "\\d+";     // String literal needs \\
// In regex engine, sees: \d+

// Python with raw strings
pattern = r"\d+"            # Raw string - no double escape
# Or without raw:
pattern = "\\d+"           # Double escape like Java
```

### 3.5 Greedy vs Lazy Quantifiers

| Quantifier | Type | Matches | Example |
|------------|------|---------|---------|
| `*` | Greedy | Maximum possible | `<.*>` in `"<a>b<c>"` → `"<a>b<c>"` (full) |
| `*?` | Lazy | Minimum possible | `<.*?>` in `"<a>b<c>"` → `"<a>"` (first) |
| `+` | Greedy | Maximum | `".+"` in `"abc"` → `"abc"` |
| `+?` | Lazy | Minimum | `".+?"` in `"abc"` → `"a"` |
| `{n,m}` | Greedy | Maximum in range | `\d{2,4}` in `"12345"` → `"1234"` |
| `{n,m}?` | Lazy | Minimum in range | `\d{2,4}?` in `"12345"` → `"12"` |

**Example: Extract HTML tags**
```python
html = "<div>Hello</div><span>World</span>"

# Greedy - WRONG!
re.findall(r"<.*>", html)  # ['<div>Hello</div><span>World</span>']

# Lazy - CORRECT!
re.findall(r"<.*?>", html)  # ['<div>', '</div>', '<span>', '</span>']
```

### 3.6 Anchors and Boundaries

| Anchor | Matches | Example | Test String | Match? |
|--------|---------|---------|-------------|--------|
| `^` | Start of string | `^Hello` | "Hello world" | ✅ |
| `^` | Start of string | `^Hello` | "Say Hello" | ❌ |
| `$` | End of string | `world$` | "Hello world" | ✅ |
| `$` | End of string | `world$` | "world of" | ❌ |
| `\b` | Word boundary | `\bcat\b` | "the cat sat" | ✅ (cat) |
| `\b` | Word boundary | `\bcat\b` | "concatenate" | ❌ |
| `\B` | Non-boundary | `\Bcat` | "bobcat" | ✅ |
| `\A` | Start (multiline) | `\AFirst` | "First\nSecond" | ✅ |
| `\Z` | End (multiline) | `Last\Z` | "First\nLast" | ✅ |

### 3.7 Groups and Capturing

| Type | Syntax | Purpose | Example |
|------|--------|---------|---------|
| **Capturing group** | `(...)` | Capture for later use | `(\d{3})-(\d{4})` |
| **Non-capturing group** | `(?:...)` | Group without capture | `(?:red|blue) car` |
| **Named group (Python)** | `(?P<name>...)` | Capture with name | `(?P<area>\d{3})` |
| **Named group (Java)** | `(?<name>...)` | Capture with name | `(?<area>\d{3})` |
| **Backreference** | `\1, \2` | Refer to captured | `(\w+)\s+\1` (repeated word) |

**Example: Phone Number Parsing**
```python
# Pattern: (123) 456-7890
pattern = r"\((\d{3})\)\s(\d{3})-(\d{4})"
text = "Call me at (555) 123-4567"

match = re.search(pattern, text)
match.group(0)  # "(555) 123-4567" (full match)
match.group(1)  # "555" (area code)
match.group(2)  # "123" (prefix)
match.group(3)  # "4567" (line number)

# Named groups
pattern = r"\((?P<area>\d{3})\)\s(?P<prefix>\d{3})-(?P<line>\d{4})"
match = re.search(pattern, text)
match.group('area')     # "555"
match.group('prefix')   # "123"
match.group('line')     # "4567"
```

### 3.8 Lookahead and Lookbehind (Advanced)

| Type | Syntax | Meaning | Example |
|------|--------|---------|---------|
| **Positive lookahead** | `(?=...)` | Followed by | `\d+(?= dollars)` → matches "100" in "100 dollars" |
| **Negative lookahead** | `(?!...)` | NOT followed by | `\d+(?! dollars)` → matches "50" in "50 euros" |
| **Positive lookbehind** | `(?<=...)` | Preceded by | `(?<=\$)\d+` → matches "100" in "$100" |
| **Negative lookbehind** | `(?<!...)` | NOT preceded by | `(?<!\$)\d+` → matches "50" in "€50" |

**Example: Password Validation**
```python
# Password must contain:
# - At least 8 characters
# - At least one uppercase
# - At least one lowercase
# - At least one digit

pattern = r"^(?=.*[a-z])(?=.*[A-Z])(?=.*\d).{8,}$"

# Breakdown:
# ^              - Start
# (?=.*[a-z])    - Lookahead: contains lowercase
# (?=.*[A-Z])    - Lookahead: contains uppercase
# (?=.*\d)       - Lookahead: contains digit
# .{8,}          - At least 8 characters
# $              - End

re.match(pattern, "Pass1234")  # ✅ Valid
re.match(pattern, "password")  # ❌ No uppercase or digit
```

### 3.9 Common Regex Patterns

| Purpose | Pattern | Explanation |
|---------|---------|-------------|
| **Email** | `^\w+@\w+\.\w+$` | Basic: word@word.word |
| **Email (better)** | `^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$` | More comprehensive |
| **Phone (US)** | `^\(?\d{3}\)?[-.\s]?\d{3}[-.\s]?\d{4}$` | (123) 456-7890 or variants |
| **URL** | `^https?://[^\s]+$` | http or https URL |
| **IP Address** | `^(\d{1,3}\.){3}\d{1,3}$` | Basic: 192.168.1.1 |
| **Date (MM/DD/YYYY)** | `^\d{2}/\d{2}/\d{4}$` | Basic: 12/31/2023 |
| **Time (24hr)** | `^([01]\d|2[0-3]):([0-5]\d)$` | 00:00 to 23:59 |
| **Hex Color** | `^#[0-9A-Fa-f]{6}$` | #RRGGBB |
| **Credit Card** | `^\d{4}[-\s]?\d{4}[-\s]?\d{4}[-\s]?\d{4}$` | 16 digits |
| **Username** | `^[a-zA-Z0-9_]{3,16}$` | 3-16 alphanumeric+underscore |
| **Integer** | `^-?\d+$` | Optional sign, digits |
| **Float** | `^-?\d+\.?\d*$` | Optional sign, optional decimal |
| **Alphanumeric** | `^[a-zA-Z0-9]+$` | Letters and numbers only |

### 3.10 Regex Functions Comparison

| Operation | Python | Java | C++ | C |
|-----------|--------|------|-----|---|
| **Compile** | `re.compile(r'...')` | `Pattern.compile("...")` | `regex r("...")` | `regcomp(&r, "...", flags)` |
| **Match (full)** | `re.match()` | `matcher.matches()` | `regex_match()` | N/A |
| **Search (partial)** | `re.search()` | `matcher.find()` | `regex_search()` | `regexec()` |
| **Find all** | `re.findall()` | Loop `find()` | `sregex_iterator` | Loop `regexec()` |
| **Replace** | `re.sub()` | `matcher.replaceAll()` | `regex_replace()` | N/A (manual) |
| **Split** | `re.split()` | `pattern.split()` | Manual | Manual |

### 3.11 Regex Flags/Modifiers

| Flag | Python | Java | C++ | Effect |
|------|--------|------|-----|--------|
| **Case-insensitive** | `re.IGNORECASE` or `re.I` | `Pattern.CASE_INSENSITIVE` | `regex_constants::icase` | Ignore case |
| **Multiline** | `re.MULTILINE` or `re.M` | `Pattern.MULTILINE` | `regex_constants::multiline` | ^ and $ match line boundaries |
| **Dot matches all** | `re.DOTALL` or `re.S` | `Pattern.DOTALL` | N/A | . matches \n too |
| **Verbose** | `re.VERBOSE` or `re.X` | `Pattern.COMMENTS` | N/A | Allow comments in regex |

**Example with Flags:**
```python
# Case-insensitive search
re.search(r"hello", "HELLO World", re.IGNORECASE)  # Matches!

# Multiline mode
text = "Line1\nLine2\nLine3"
re.findall(r"^Line", text, re.MULTILINE)  # ['Line', 'Line', 'Line']

# Combine flags
re.search(r"hello", "HELLO\nWORLD", re.I | re.M)
```

---

## 4. STRING SEARCHING ALGORITHMS (VERY DEEP)

### 4.1 Basic Search Functions

| Language | Function | Returns | Time | Notes |
|----------|----------|---------|------|-------|
| **C** | `strstr(haystack, needle)` | Pointer or NULL | O(n*m) | First occurrence |
| **C** | `strchr(str, char)` | Pointer or NULL | O(n) | First char occurrence |
| **C** | `strrchr(str, char)` | Pointer or NULL | O(n) | Last char occurrence |
| **C++** | `str.find(substr)` | size_t (or npos) | O(n*m) | First occurrence |
| **C++** | `str.rfind(substr)` | size_t (or npos) | O(n*m) | Last occurrence |
| **C++** | `str.find_first_of(chars)` | size_t | O(n*m) | First of any char |
| **C++** | `str.find_last_of(chars)` | size_t | O(n*m) | Last of any char |
| **Java** | `str.indexOf(substr)` | int (-1 if not found) | O(n*m) | First occurrence |
| **Java** | `str.lastIndexOf(substr)` | int | O(n*m) | Last occurrence |
| **Java** | `str.contains(substr)` | boolean | O(n*m) | Check existence |
| **Python** | `str.find(substr)` | int (-1 if not found) | O(n*m) | First occurrence |
| **Python** | `str.rfind(substr)` | int | O(n*m) | Last occurrence |
| **Python** | `str.index(substr)` | int (raises ValueError) | O(n*m) | First (exception) |
| **Python** | `substr in str` | boolean | O(n*m) | Check existence |

### 4.2 Search Algorithm Complexities

| Algorithm | Best | Average | Worst | Space | Preprocessing |
|-----------|------|---------|-------|-------|--------------|
| **Naive (Brute Force)** | O(n) | O(n*m) | O(n*m) | O(1) | O(1) |
| **Rabin-Karp** | O(n+m) | O(n+m) | O(n*m) | O(1) | O(m) |
| **KMP (Knuth-Morris-Pratt)** | O(n+m) | O(n+m) | O(n+m) | O(m) | O(m) |
| **Boyer-Moore** | O(n/m) | O(n) | O(n*m) | O(m) | O(m+σ) |
| **Aho-Corasick** | O(n+m+z) | O(n+m+z) | O(n+m+z) | O(m*σ) | O(m) |

Where:
- n = text length
- m = pattern length
- σ = alphabet size
- z = number of matches

### 4.3 Naive Search (Brute Force)

**Algorithm:**
```
For each position i in text:
    Check if pattern matches starting at i
    If full match, return i
Return -1 (not found)
```

**Time:** O(n*m) - For each of n positions, check up to m characters

**Example:**
```
Text:    "ABCABCABD"
Pattern: "ABCABD"

Position 0: ABC... ✓ continue → ABCABC ✓ → ABCABCA ✗
Position 1: BCA... ✗
Position 2: CAB... ✗
Position 3: ABC... ✓ continue → ABCABD ✅ FOUND at 3!
```

### 4.4 KMP Algorithm (Knuth-Morris-Pratt)

**Key Idea:** Don't re-check already matched characters

**Preprocessing:** Build LPS (Longest Proper Prefix which is also Suffix) array

**LPS Array Examples:**
```
Pattern: "ABABC"
Index:    0 1 2 3 4
Char:     A B A B C
LPS:      0 0 1 2 0

Pattern: "AAAA"
LPS:      0 1 2 3

Pattern: "ABCDE"
LPS:      0 0 0 0 0

Pattern: "AABAACAABAA"
LPS:      0 1 0 1 2 0 1 2 3 4 5
```

**Time Complexity:**
- Preprocessing: O(m)
- Searching: O(n)
- **Total: O(n+m)** ✅ Linear!

**When to use:** Pattern has repeating prefixes

### 4.5 Boyer-Moore Algorithm

**Key Idea:** Start matching from END of pattern, skip intelligently

**Two Rules:**
1. **Bad Character Rule:** If mismatch, skip based on where that character appears in pattern
2. **Good Suffix Rule:** If mismatch after some matches, skip based on suffix

**Best Case:** O(n/m) - Can skip pattern length at a time!

**Example:**
```
Text:    "HERE IS A SIMPLE EXAMPLE"
Pattern: "EXAMPLE"

Position 0:
    E X A M P L E
    H           (Mismatch at E vs H, H not in pattern, skip 7)
    
Position 7:
    E X A M P L E
    S           (Mismatch, S not in pattern, skip 7)

Position 14:
    E X A M P L E
    E X A M P L E  ✅ FOUND!
```

**When to use:** Large texts, pattern doesn't occur often

### 4.6 Rabin-Karp (Rolling Hash)

**Key Idea:** Use hash to compare pattern vs substrings

**Algorithm:**
```
1. Compute hash of pattern
2. For each substring of length m in text:
    - Compute hash (rolling hash for efficiency)
    - If hash matches, verify actual characters
3. Return position if match
```

**Rolling Hash Formula:**
```
hash = (c1 * d^(m-1) + c2 * d^(m-2) + ... + cm * d^0) % q

where:
    d = number of characters in alphabet (e.g., 256)
    q = prime number
```

**Example:**
```
Pattern: "ABC" (hash = 'A'*256^2 + 'B'*256 + 'C')
Text: "XYZABC"

Hash "XYZ": ...
Hash "YZA": ... (roll)
Hash "ZAB": ... (roll)
Hash "ABC": MATCH! → Verify characters → FOUND!
```

**Time:** O(n+m) average, O(n*m) worst (hash collisions)

**When to use:** Multiple pattern search, plagiarism detection

### 4.7 Aho-Corasick (Multiple Patterns)

**Key Idea:** Build trie + failure links to search multiple patterns simultaneously

**Use Case:** Find ANY of k patterns in text

**Time:** O(n + m + z) where:
- n = text length
- m = total length of all patterns
- z = number of matches

**Example:**
```
Patterns: ["he", "she", "his", "hers"]
Text: "she sells seashells"

Build trie:
       root
      /  |  \
     h   s   ...
    /|\  |
   e i s h
   | |   |
   r s   e
   |     |
   s     (matches)
```

**When to use:** 
- Dictionary search
- Virus scanning
- Content filtering
- Multiple keyword search

### 4.8 Search with Wildcards

| Pattern | Matches | Algorithm |
|---------|---------|-----------|
| `"a*c"` | "abc", "adc", "a123c" | Modified naive |
| `"a?c"` | "abc", "adc" (single char) | Modified naive |
| `"a.*c"` (regex) | Anything starting a, ending c | Regex engine |

### 4.9 Substring Count

| Language | Method | Example |
|----------|--------|---------|
| **Python** | `str.count(sub)` | `"hello".count("l")` → 2 |
| **Java** | Manual loop | Count with `indexOf()` in loop |
| **C++** | Manual loop | Count with `find()` in loop |

**Algorithm:**
```python
def count_occurrences(text, pattern):
    count = 0
    start = 0
    while True:
        pos = text.find(pattern, start)
        if pos == -1:
            break
        count += 1
        start = pos + 1  # Overlapping: +1, Non-overlapping: +len(pattern)
    return count
```

### 4.10 Case-Insensitive Search

| Language | Method |
|----------|--------|
| **C** | Manual: convert both to lowercase |
| **C** | POSIX: `strcasecmp()` |
| **C++** | Convert to lowercase: `std::transform()` + `::tolower` |
| **C++** | Custom comparator in `find()` |
| **Java** | `str.toLowerCase().indexOf()` |
| **Java** | `str.regionMatches(ignoreCase, ...)` |
| **Python** | `str.lower().find()` |
| **Python** | Regex: `re.search(r'pattern', text, re.IGNORECASE)` |

---

## 5. COMPREHENSIVE COMPARISON TABLES

### 5.1 String Mutability

| Language | Mutable? | Consequence |
|----------|----------|-------------|
| **C** | ✅ Yes | Can modify char array directly |
| **C++** | ✅ Yes (std::string) | Can modify with `str[i] = 'x'` |
| **Java** | ❌ No (String) | Each modification creates new object |
| **Java** | ✅ Yes (StringBuilder) | Mutable version for efficiency |
| **Python** | ❌ No | Each modification creates new object |

### 5.2 String Concatenation Performance

| Language | Method | Single Concat | Loop (n times) | Better Alternative |
|----------|--------|--------------|----------------|-------------------|
| **C** | `strcat()` | O(n+m) | O(n²) | Pre-allocate buffer |
| **C++** | `+` operator | O(n+m) | O(n²) | `reserve()` + `+=` |
| **Java** | `+` operator | O(n+m) | O(n²) | `StringBuilder` → O(n) |
| **Python** | `+` operator | O(n+m) | O(n²) | `''.join(list)` → O(n) |

### 5.3 Common String Operations Time Complexity

| Operation | C/C++ | Java | Python |
|-----------|-------|------|--------|
| **Length** | O(n) C, O(1) C++ | O(1) | O(1) |
| **Access char** | O(1) | O(1) | O(1) |
| **Concatenate** | O(n+m) | O(n+m) | O(n+m) |
| **Substring** | O(k) | O(k) | O(k) |
| **Find** | O(n*m) | O(n*m) | O(n*m) |
| **Replace** | O(n) | O(n) | O(n) |
| **Split** | O(n) | O(n) | O(n) |
| **Join** | O(n) | O(n) | O(n) |

---

## 6. CRITICAL EXAM POINTS

### Must Remember - Length

1. ✅ C: `strlen()` excludes '\0', `sizeof()` includes it
2. ✅ `sizeof(pointer)` ≠ `sizeof(array)`
3. ✅ C++ `length()` == `size()` (both O(1))
4. ✅ UTF-8: strlen() counts bytes, not characters
5. ✅ All languages: empty string length = 0

### Must Remember - Substring

1. ✅ C++ `substr(pos, len)` - LENGTH parameter
2. ✅ Java `substring(start, end)` - END index (exclusive)
3. ✅ Python `[start:end:step]` - END exclusive, step optional
4. ✅ Python supports negative indices (from end)
5. ✅ Out of bounds: C++/Java exception, Python returns available

### Must Remember - Regex

1. ✅ `.` matches any char EXCEPT newline (use DOTALL flag)
2. ✅ `^` and `$` are anchors (start/end)
3. ✅ `*` is greedy, `*?` is lazy
4. ✅ `\d` is digit, `\D` is non-digit
5. ✅ Must escape special chars: `. * + ? [ ] { } ( ) | \ ^ $`
6. ✅ Lookahead/behind don't consume characters
7. ✅ Groups: `()` captures, `(?:)` doesn't capture

### Must Remember - Search

1. ✅ Naive search: O(n*m), simple but slow
2. ✅ KMP: O(n+m), best for patterns with repetition
3. ✅ Boyer-Moore: O(n/m) best case, good for large texts
4. ✅ Rabin-Karp: O(n+m) average, good for multiple patterns
5. ✅ Aho-Corasick: Best for searching many patterns simultaneously

---

## 7. QUICK INTERVIEW CHECKS

### Questions to Ask Yourself

1. **C string length?** → strlen() or sizeof()? Know the difference!
2. **Java substring(2, 5)?** → Characters at indices 2, 3, 4 (NOT 5)
3. **Python "hello"[-1]?** → "o" (last character)
4. **Regex `\d+` vs `\d*`?** → + requires at least 1, * allows 0
5. **Find all pattern occurrences?** → Use KMP or regex findall()
6. **Case-insensitive search?** → Convert both or use regex flag
7. **Multiple patterns to find?** → Aho-Corasick algorithm
8. **Pattern has repetition?** → KMP is efficient
9. **Large text, rare pattern?** → Boyer-Moore
10. **Validate email/phone?** → Use regex with proper pattern

---

## 8. FINAL REVISION CHECKLIST

### Before Exam - Confirm You Know:

- [ ] C: strlen() vs sizeof() difference
- [ ] C++ substr() takes LENGTH parameter
- [ ] Java substring() takes END index (exclusive)
- [ ] Python slicing syntax [start:end:step]
- [ ] Python negative indices
- [ ] Basic regex symbols: . ^ $ * + ? [] {} () |
- [ ] Regex escape sequences: \d \w \s \b
- [ ] Greedy vs lazy quantifiers (* vs *?)
- [ ] Regex groups and backreferences
- [ ] Lookahead (?=) and lookbehind (?<=)
- [ ] Search algorithms: Naive, KMP, Boyer-Moore
- [ ] KMP best for repetitive patterns
- [ ] Boyer-Moore best for large texts
- [ ] Time complexities: O(n*m) naive, O(n+m) KMP
- [ ] String immutability: Java String, Python str

---

## 9. COMMON TRAPS & GOTCHAS

### Length Traps
- ❌ C: Forgetting strlen() is O(n)
- ❌ C: sizeof(pointer) vs sizeof(array)
- ❌ UTF-8: strlen() counts bytes, not characters

### Substring Traps
- ❌ Java: substring(2, 5) does NOT include index 5
- ❌ C++: substr(2, 5) means start=2, LENGTH=5
- ❌ Python: Forgetting negative indices work

### Regex Traps
- ❌ Forgetting to escape special characters
- ❌ Using greedy when lazy is needed
- ❌ Forgetting ^ and $ anchors
- ❌ Not using raw strings in Python: r""

### Search Traps
- ❌ Using naive search for large texts
- ❌ Not preprocessing pattern for KMP/Boyer-Moore
- ❌ Case-sensitivity issues

---

## END OF QUICK REVISION

### 🎯 You're now ready for string manipulation questions!
### ✅ All concepts covered in depth
### 🔥 Good luck with your exam!

---

**Last Updated**: Perfect for SEBI Phase 2
**Coverage**: Length | Substring | Regex | Search - VERY DEEP
**Time to Review**: 60-75 minutes
