# String Manipulation - Multiple Choice Questions

## Comprehensive MCQ Set with Answers & Explanations

---

## STRING OPERATIONS

### Question 1: Logic Flow Completion
**Complete the string reversal function:**

```python
def reverse_string(s):
    # ??? Which method reverses correctly?
    return result
```

A) `s.reverse()`  
B) `s[::-1]`  
C) `reversed(s)`  
D) `s[-1:0:-1]`

**Answer: B**

**Explanation:**  
`s[::-1]` is the correct and most Pythonic way:

```python
s = "hello"
s[::-1]  # "olleh" ✓
```

- Option A: Strings don't have reverse() method (AttributeError)
- Option C: Returns iterator, needs `''.join(reversed(s))`
- Option D: Missing first character (`-1:0` excludes index 0)

Correct Option D would be: `s[::-1]` or `s[-1::-1]`

---

### Question 2: Debugging
**What's wrong with this code?**

```python
text = "Hello World"
text[0] = 'h'
print(text)
```

A) Wrong syntax for assignment  
B) Strings are immutable  
C) Need to use text.replace()  
D) Index out of range

**Answer: B**

**Explanation:**  
**Strings are immutable** in Python - cannot modify characters in place.

```python
TypeError: 'str' object does not support item assignment
```

Correct approaches:
```python
# Method 1: Create new string
text = 'h' + text[1:]  # "hello World"

# Method 2: Use replace
text = text.replace('H', 'h')

# Method 3: Convert to list, modify, join
chars = list(text)
chars[0] = 'h'
text = ''.join(chars)
```

---

### Question 3: Dry Run Output
**What is the output?**

```python
s = "  Python  "
print(f"'{s.strip()}'")
print(f"'{s.lstrip()}'")
print(f"'{s.rstrip()}'")
```

A) 'Python', 'Python  ', '  Python'  
B) 'Python', 'Python', 'Python'  
C) '  Python', 'Python  ', 'Python'  
D) Error

**Answer: A**

**Explanation:**  
Strip methods remove whitespace:

```python
s = "  Python  "

s.strip()   # 'Python'    - removes both sides
s.lstrip()  # 'Python  '  - removes left only
s.rstrip()  # '  Python'  - removes right only
```

Original string remains unchanged (strings are immutable).

---

### Question 4: Syntax Understanding
**What does this accomplish?**

```python
text = "hello-world-python"
result = text.split('-')
print(result)
```

A) Removes all dashes  
B) Splits into list of words  
C) Replaces dashes with spaces  
D) Counts dashes

**Answer: B**

**Explanation:**  
`split('-')` **splits string into list** using delimiter:

```python
result = ['hello', 'world', 'python']
```

Common split operations:
```python
"a,b,c".split(',')      # ['a', 'b', 'c']
"a b c".split()         # ['a', 'b', 'c'] (whitespace)
"a::b::c".split('::')   # ['a', 'b', 'c']
"abc".split('')         # ValueError (empty separator)
```

Reverse operation: `'-'.join(result)` → "hello-world-python"

---

### Question 5: Data Analysis
**Which is most efficient for building large strings?**

A) `result = ""; for i in items: result += i`  
B) `result = "".join(items)`  
C) `result = ""; for i in items: result = result + i`  
D) `result = reduce(lambda x, y: x + y, items)`

**Answer: B**

**Explanation:**  
`''.join(items)` is **most efficient**:

```python
# ✓ Best: O(n) - single allocation
result = ''.join(items)

# ✗ Worst: O(n²) - creates new string each iteration
result = ""
for item in items:
    result += item  # New string allocation each time!
```

Performance comparison (10,000 items):
- `join()`: ~0.001s
- `+=` loop: ~0.5s (500x slower!)

Use `join()` for string concatenation in loops.

---

## STRING FORMATTING

### Question 6: Logic Flow Completion
**Complete the f-string formatting:**

```python
name = "Alice"
age = 25
price = 19.99

# Format: "Alice (25 years) - $20.00"
result = ???
```

A) `f"{name} ({age} years) - ${price:.2f}"`  
B) `"%s (%d years) - $%.2f" % (name, age, price)`  
C) `"{} ({} years) - ${:.2f}".format(name, age, price)`  
D) All of the above

**Answer: D**

**Explanation:**  
All three formatting methods work:

```python
# f-strings (Python 3.6+, preferred)
f"{name} ({age} years) - ${price:.2f}"

# %-formatting (old style)
"%s (%d years) - $%.2f" % (name, age, price)

# str.format() (newer style)
"{} ({} years) - ${:.2f}".format(name, age, price)
```

All produce: "Alice (25 years) - $20.00"

f-strings are most readable and fastest.

---

### Question 7: Debugging
**What's wrong with this formatting?**

```python
data = {'name': 'Bob', 'age': 30}
message = f"Hello {name}, you are {age} years old"
print(message)
```

A) Wrong f-string syntax  
B) Variables name and age don't exist  
C) Should use .format() instead  
D) Missing quotes around variables

**Answer: B**

**Explanation:**  
`name` and `age` are **not defined** - causes `NameError`.

f-strings evaluate expressions directly:
```python
# ✗ Wrong - name and age not defined
f"Hello {name}"

# ✓ Correct - access dictionary
f"Hello {data['name']}, you are {data['age']} years old"

# ✓ Alternative
name = data['name']
age = data['age']
f"Hello {name}, you are {age} years old"
```

---

### Question 8: Dry Run Output
**What is the output?**

```python
x = 10
y = 3
print(f"{x:05d}")
print(f"{y/x:.2%}")
print(f"{x:b}")
```

A) 00010, 30.00%, 1010  
B) 10000, 0.30%, 1010  
C) 00010, 30%, 1010  
D) Error

**Answer: A**

**Explanation:**  
f-string format specifiers:

```python
x = 10, y = 3

f"{x:05d}"    # 00010    - 5 digits, zero-padded
f"{y/x:.2%}"  # 30.00%   - percentage, 2 decimals
f"{x:b}"      # 1010     - binary representation
```

Format syntax: `{value:flags width.precision type}`

Common types: `d`(int), `f`(float), `%`(percent), `b`(binary), `x`(hex)

---

### Question 9: Syntax Understanding
**What does this format specifier do?**

```python
number = 1234567.89
print(f"{number:,.2f}")
```

A) Rounds to 2 decimals  
B) Adds thousand separators  
C) Both A and B  
D) Converts to percentage

**Answer: C**

**Explanation:**  
`:,.2f` combines two formatting operations:

```python
number = 1234567.89
f"{number:,.2f}"  # "1,234,567.89"
```

Format breakdown:
- `,` - thousand separator
- `.2` - 2 decimal places
- `f` - fixed-point notation

Other examples:
```python
f"{number:,.0f}"   # "1,234,568" (no decimals)
f"{number:_.2f}"   # "1_234_567.89" (underscore separator)
f"{number:.2e}"    # "1.23e+06" (scientific notation)
```

---

### Question 10: Data Analysis
**Which formatting is best for logging?**

```python
# Scenario: Log millions of messages
level = "INFO"
message = "User logged in"
timestamp = "2024-01-01"
```

A) `f"[{timestamp}] {level}: {message}"`  
B) `"[%s] %s: %s" % (timestamp, level, message)`  
C) `"[{}] {}: {}".format(timestamp, level, message)`  
D) All perform the same

**Answer: A**

**Explanation:**  
**f-strings are fastest** for logging:

Performance (1M iterations):
```python
f-strings:     0.15s  ✓ Fastest
%-formatting:  0.28s
str.format():  0.35s  ✗ Slowest
```

f-strings advantages:
- Evaluated at compile time (faster)
- More readable
- Support expressions: `f"{user.name.upper()}"`

For high-volume logging, use f-strings for best performance.

---

## STRING SEARCHING & PATTERN MATCHING

### Question 11: Logic Flow Completion
**Complete the substring search:**

```python
text = "Hello World Hello"
# Find all occurrences of "Hello"
positions = []
# ??? Missing loop logic
```

A) `for i in text: if i == "Hello": positions.append(i)`  
B) `start = 0; while True: idx = text.find("Hello", start); if idx == -1: break; positions.append(idx); start = idx + 1`  
C) `positions = [i for i in range(len(text)) if text[i] == "Hello"]`  
D) `positions = text.findall("Hello")`

**Answer: B**

**Explanation:**  
Find all occurrences using `find()` in loop:

```python
text = "Hello World Hello"
positions = []
start = 0

while True:
    idx = text.find("Hello", start)
    if idx == -1:  # Not found
        break
    positions.append(idx)
    start = idx + 1  # Continue searching after this occurrence

# positions = [0, 12]
```

- Option A: Iterates characters, not substrings
- Option C: Compares characters to string (always False)
- Option D: No findall() method for strings (regex has it)

---

### Question 12: Debugging
**What's wrong with this search?**

```python
text = "Python is awesome. python is great."
count = text.count("python")
print(count)
```

A) Syntax error  
B) Case-sensitive search misses "Python"  
C) Should use len() instead  
D) No error

**Answer: B**

**Explanation:**  
`count()` is **case-sensitive**, finds only lowercase "python":

```python
count = text.count("python")  # Returns 1 (misses "Python")
```

Case-insensitive search:
```python
# Method 1: Convert to lowercase
count = text.lower().count("python")  # 2 ✓

# Method 2: Regex with re.IGNORECASE
import re
count = len(re.findall(r"python", text, re.IGNORECASE))  # 2 ✓
```

---

### Question 13: Dry Run Output
**What is the output?**

```python
text = "abcdefghij"
print(text.find("def"))
print(text.find("xyz"))
print(text.index("def"))
try:
    print(text.index("xyz"))
except ValueError:
    print("ValueError")
```

A) 3, -1, 3, ValueError  
B) 3, None, 3, ValueError  
C) 3, -1, 3, -1  
D) 3, 0, 3, 0

**Answer: A**

**Explanation:**  
`find()` vs `index()`:

```python
text = "abcdefghij"

text.find("def")    # 3  - returns index
text.find("xyz")    # -1 - returns -1 if not found

text.index("def")   # 3  - returns index
text.index("xyz")   # ValueError - raises exception if not found
```

Key difference:
- `find()`: Returns -1 on failure (safe)
- `index()`: Raises ValueError on failure (use with try/except)

---

### Question 14: Syntax Understanding
**What do these methods do differently?**

```python
text = "Hello World"
print(text.startswith("Hello"))
print(text.endswith("World"))
print(text[:5] == "Hello")
```

A) All check prefixes  
B) startswith/endswith are optimized for prefix/suffix checks  
C) Slicing is more efficient  
D) They work on different data types

**Answer: B**

**Explanation:**  
`startswith()` and `endswith()` are **optimized for prefix/suffix checks**:

```python
# ✓ Preferred: Clear intent, optimized
text.startswith("Hello")  # True
text.endswith("World")    # True

# Works but less clear
text[:5] == "Hello"       # True
text[-5:] == "World"      # True
```

Benefits of startswith/endswith:
- More readable
- Accept tuple of strings: `text.startswith(('Hello', 'Hi'))`
- Can specify start/end range
- Faster for long strings

---

### Question 15: Data Analysis
**Which search method is most efficient for multiple patterns?**

```python
# Scenario: Check if text contains any of 1000 keywords
text = "Some long document..."
keywords = ["python", "java", "javascript", ...]  # 1000 items
```

A) `any(k in text for k in keywords)`  
B) `regex = '|'.join(keywords); re.search(regex, text)`  
C) `for k in keywords: if k in text: return True`  
D) Build trie data structure

**Answer: D**

**Explanation:**  
For **multiple pattern matching**, data structures matter:

Performance comparison:
```python
# Worst: O(n*m*k) - n=text length, m=keywords, k=avg keyword length
for k in keywords:
    if k in text: return True

# Better: O(n*m) - compile once
regex = '|'.join(keywords)
re.search(regex, text)

# Best: O(n*k) - Trie/Aho-Corasick algorithm
trie = build_trie(keywords)
trie.search(text)  # Single pass through text
```

For 1000+ patterns, specialized algorithms (Aho-Corasick, suffix arrays) are significantly faster.

---

## REGULAR EXPRESSIONS

### Question 16: Logic Flow Completion
**Complete the email validation regex:**

```python
import re

pattern = ???
email = "user@example.com"
```

A) `r"[\w\.-]+@[\w\.-]+\.\w+"`  
B) `r".*@.*\..*"`  
C) `r"\w+@\w+\.\w+"`  
D) `r"^[\w\.-]+@[\w\.-]+\.\w{2,}$"`

**Answer: D**

**Explanation:**  
Most robust email pattern:

```python
pattern = r"^[\w\.-]+@[\w\.-]+\.\w{2,}$"
```

Breakdown:
- `^` - Start of string
- `[\w\.-]+` - Username: alphanumeric, dots, dashes
- `@` - Literal @
- `[\w\.-]+` - Domain name
- `\.` - Literal dot
- `\w{2,}` - TLD (at least 2 characters)
- `$` - End of string

Options A & C missing anchors (allow partial matches), Option B too permissive.

---

### Question 17: Debugging
**What's wrong with this regex?**

```python
import re
text = "Price: $99.99"
pattern = r"$\d+\.\d+"
match = re.search(pattern, text)
print(match)
```

A) Should escape $ as \$  
B) Pattern is too restrictive  
C) Missing re.compile()  
D) No error

**Answer: A**

**Explanation:**  
`$` is **special character** (end of string) in regex:

```python
# ✗ Wrong: $ matches end of string
pattern = r"$\d+\.\d+"  # Never matches

# ✓ Correct: Escape literal $
pattern = r"\$\d+\.\d+"  # Matches "$99.99"
```

Special characters need escaping:
```python
. → \.   (literal dot)
$ → \$   (literal dollar)
^ → \^   (literal caret)
* → \*   (literal asterisk)
\ → \\   (literal backslash)
```

---

### Question 18: Dry Run Output
**What is the output?**

```python
import re
text = "2024-01-15"
pattern = r"(\d{4})-(\d{2})-(\d{2})"
match = re.search(pattern, text)

print(match.group(0))
print(match.group(1))
print(match.group(2))
print(match.groups())
```

A) 2024-01-15, 2024, 01, (2024, 01, 15)  
B) 2024-01-15, 2024, 01-15, (2024, 01, 15)  
C) 2024, 01, 15, (2024, 01, 15)  
D) Error

**Answer: A**

**Explanation:**  
Regex groups capture subpatterns:

```python
match.group(0)   # "2024-01-15" - entire match
match.group(1)   # "2024" - first group (year)
match.group(2)   # "01" - second group (month)
match.groups()   # ("2024", "01", "15") - tuple of all groups
```

Named groups alternative:
```python
pattern = r"(?P<year>\d{4})-(?P<month>\d{2})-(?P<day>\d{2})"
match.group('year')  # "2024"
```

---

### Question 19: Syntax Understanding
**What does this regex match?**

```python
import re
pattern = r"\b\w{4}\b"
text = "The cat and the dog ran"
matches = re.findall(pattern, text)
```

A) All words  
B) All 4-letter words  
C) Words starting with 4 characters  
D) First 4-letter word only

**Answer: B**

**Explanation:**  
Pattern matches **exactly 4-letter words**:

```python
pattern = r"\b\w{4}\b"
matches = re.findall(pattern, text)
# ['cat', 'and', 'the', 'dog', 'ran']  # Only 4-letter words
```

Breakdown:
- `\b` - Word boundary
- `\w{4}` - Exactly 4 word characters
- `\b` - Word boundary

Without boundaries:
```python
r"\w{4}"  # Matches first 4 chars of any word
# ['The', 'cat', 'and', 'the', 'dog', 'ran']  # "The" → "The"
```

---

### Question 20: Data Analysis
**Which regex flag is most important for case-insensitive search?**

```python
import re
text = "Python and PYTHON"
pattern = r"python"
```

A) `re.search(pattern, text, re.DOTALL)`  
B) `re.search(pattern, text, re.IGNORECASE)`  
C) `re.search(pattern, text, re.MULTILINE)`  
D) `re.search(pattern, text, re.VERBOSE)`

**Answer: B**

**Explanation:**  
`re.IGNORECASE` (or `re.I`) makes matching **case-insensitive**:

```python
# Without flag: Matches only lowercase
re.findall(r"python", text)  # ['python']

# With flag: Matches any case
re.findall(r"python", text, re.IGNORECASE)  # ['Python', 'PYTHON']
```

Other flags:
- `re.DOTALL` (re.S): `.` matches newlines
- `re.MULTILINE` (re.M): `^` and `$` match line boundaries
- `re.VERBOSE` (re.X): Ignore whitespace for readability

Combine flags: `re.IGNORECASE | re.MULTILINE`

---

## STRING ANALYSIS & PARSING

### Question 21: Logic Flow Completion
**Complete the word frequency counter:**

```python
text = "hello world hello python world"
# ??? Count word frequencies
```

A) `{word: text.count(word) for word in text.split()}`  
B) `{word: text.split().count(word) for word in set(text.split())}`  
C) `from collections import Counter; Counter(text.split())`  
D) Both B and C

**Answer: D**

**Explanation:**  
Both work correctly:

```python
# Method B: Dictionary comprehension with set
words = text.split()
freq = {word: words.count(word) for word in set(words)}
# {'hello': 2, 'world': 2, 'python': 1}

# Method C: Counter (preferred)
from collections import Counter
freq = Counter(text.split())
# Counter({'hello': 2, 'world': 2, 'python': 1})
```

Option A is inefficient (counts duplicates multiple times).

Counter is preferred: more readable, has useful methods like `most_common()`.

---

### Question 22: Debugging
**What's wrong with this CSV parsing?**

```python
csv_line = "John,Doe,30,New York"
fields = csv_line.split(',')
name = fields[0] + " " + fields[1]
age = fields[2]
city = fields[3]
```

A) No error  
B) Fails with embedded commas: "Smith, Jr.,30,..."  
C) Should use csv module  
D) Both B and C

**Answer: D**

**Explanation:**  
Simple `split(',')` **fails with embedded commas**:

```python
# ✗ Breaks with: "Smith, Jr.,30,New York, NY"
fields = line.split(',')  # 5 fields instead of 4!

# ✓ Use csv module
import csv
reader = csv.reader([line])
fields = next(reader)  # Handles quoted commas correctly
```

CSV complexities:
- Quoted fields: `"Smith, Jr.",30,"New York, NY"`
- Escaped quotes: `"She said ""Hello"""`
- Different delimiters

Always use `csv` module for robust parsing.

---

### Question 23: Dry Run Output
**What is the output?**

```python
text = "a1b2c3"
digits = ''.join(filter(str.isdigit, text))
letters = ''.join(filter(str.isalpha, text))
print(digits)
print(letters)
```

A) 123, abc  
B) abc, 123  
C) a1b2c3, a1b2c3  
D) Error

**Answer: A**

**Explanation:**  
`filter()` with string methods extracts characters:

```python
text = "a1b2c3"

# Extract digits
digits = ''.join(filter(str.isdigit, text))
# ''.join(['1', '2', '3']) → "123"

# Extract letters
letters = ''.join(filter(str.isalpha, text))
# ''.join(['a', 'b', 'c']) → "abc"
```

Alternative regex approach:
```python
import re
digits = ''.join(re.findall(r'\d', text))   # "123"
letters = ''.join(re.findall(r'[a-zA-Z]', text))  # "abc"
```

---

### Question 24: Syntax Understanding
**What does this accomplish?**

```python
text = "hello world"
result = text.title()
print(result)
```

A) Converts to uppercase  
B) Capitalizes first letter  
C) Capitalizes first letter of each word  
D) Converts to lowercase

**Answer: C**

**Explanation:**  
`title()` **capitalizes first letter of each word**:

```python
text = "hello world python"
text.title()  # "Hello World Python"
```

Case methods comparison:
```python
s = "hello WORLD"

s.upper()       # "HELLO WORLD"
s.lower()       # "hello world"
s.capitalize()  # "Hello world" (first char only)
s.title()       # "Hello World" (each word)
s.swapcase()    # "HELLO world" (swap case)
```

Note: `title()` can have unexpected behavior:
```python
"they're".title()  # "They'Re" (apostrophe counts as word boundary)
```

---

### Question 25: Data Analysis
**Which method is most efficient for checking multiple conditions?**

```python
text = "user@example.com"
# Check: contains @, has dot after @, ends with valid TLD
```

A) Multiple `if` statements with `in` and `endswith()`  
B) Single regex pattern  
C) Parse and validate each component  
D) Depends on frequency and complexity

**Answer: D**

**Explanation:**  
**Choice depends on use case**:

```python
# Simple checks (few validations): Use string methods
if '@' in text and '.' in text.split('@')[1] and text.endswith(('.com', '.org')):
    # Fast for simple cases

# Complex rules (email validation): Use regex
pattern = r"^[\w\.-]+@[\w\.-]+\.\w{2,}$"
if re.match(pattern, text):
    # Better for complex patterns

# High-volume production: Precompile regex
EMAIL_PATTERN = re.compile(r"^[\w\.-]+@[\w\.-]+\.\w{2,}$")
if EMAIL_PATTERN.match(text):
    # Fastest when called repeatedly
```

Guidelines:
- 1-2 checks: String methods
- Complex patterns: Regex
- High volume: Precompiled regex
- Parsing: Structured parsing (csv, json, etc.)

---

## STRING ENCODING & UNICODE

### Question 26: Logic Flow Completion
**Complete the Unicode handling:**

```python
text = "Hello 世界"
# ??? Encode to bytes and decode back
```

A) `text.encode(); text.decode()`  
B) `text.encode('utf-8'); bytes_data.decode('utf-8')`  
C) `bytes(text); str(bytes_data)`  
D) `text.encode('ascii')`

**Answer: B**

**Explanation:**  
Proper encoding/decoding cycle:

```python
text = "Hello 世界"

# Encode string → bytes
bytes_data = text.encode('utf-8')  # b'Hello \xe4\xb8\x96\xe7\x95\x8c'

# Decode bytes → string
decoded = bytes_data.decode('utf-8')  # "Hello 世界"
```

- Option A: strings don't have decode(), bytes don't have encode()
- Option C: `bytes(text)` fails (need encoding)
- Option D: `encode('ascii')` fails (can't encode Chinese characters)

Always specify encoding (UTF-8 recommended).

---

### Question 27: Debugging
**What causes this error?**

```python
text = "Café"
encoded = text.encode('ascii')
```

A) Missing import  
B) ASCII can't encode 'é'  
C) Wrong syntax  
D) No error

**Answer: B**

**Explanation:**  
**ASCII only supports 0-127** characters:

```python
text = "Café"
text.encode('ascii')  # UnicodeEncodeError: 'ascii' codec can't encode character 'é'
```

Solutions:
```python
# 1. Use UTF-8 (supports all Unicode)
text.encode('utf-8')  # b'Caf\xc3\xa9' ✓

# 2. Error handling
text.encode('ascii', errors='ignore')   # b'Caf' (removes é)
text.encode('ascii', errors='replace')  # b'Caf?' (replaces é with ?)
text.encode('ascii', errors='xmlcharrefreplace')  # b'Caf&#233;' (XML entity)
```

Always use UTF-8 unless specific requirements.

---

### Question 28: Dry Run Output
**What is the output?**

```python
text = "Python"
print(len(text))
print(len(text.encode('utf-8')))
print(len(text.encode('utf-16')))
```

A) 6, 6, 6  
B) 6, 6, 12  
C) 6, 6, 14  
D) 6, 12, 12

**Answer: C**

**Explanation:**  
Different encodings have different byte lengths:

```python
text = "Python"

len(text)                    # 6 characters
len(text.encode('utf-8'))    # 6 bytes (ASCII-compatible)
len(text.encode('utf-16'))   # 14 bytes (2 bytes/char + 2 BOM)
```

Encoding sizes:
- UTF-8: 1-4 bytes per character (ASCII = 1 byte)
- UTF-16: 2 or 4 bytes per character + 2-byte BOM (Byte Order Mark)
- UTF-32: 4 bytes per character

Chinese example:
```python
"世".encode('utf-8')   # 3 bytes
"世".encode('utf-16')  # 4 bytes (2 + 2 BOM)
```

---

### Question 29: Syntax Understanding
**What does this achieve?**

```python
text = "hello"
result = text.encode('utf-8').decode('utf-8')
print(result)
print(type(result))
```

A) Converts to bytes  
B) No effect, returns same string  
C) Validates UTF-8 encoding  
D) Converts encoding

**Answer: B**

**Explanation:**  
Round-trip **encoding/decoding returns identical string**:

```python
text = "hello"
result = text.encode('utf-8').decode('utf-8')
# result == "hello" (same string)
# type(result) == <class 'str'>
```

This pattern is useful for:
```python
# 1. Validation: Ensure text is valid UTF-8
try:
    text.encode('utf-8').decode('utf-8')
except UnicodeError:
    print("Invalid UTF-8")

# 2. Normalize encoding from unknown source
data = unknown_source()
clean_data = data.encode('utf-8', errors='replace').decode('utf-8')
```

---

### Question 30: Data Analysis
**Which encoding should you use for international applications?**

A) ASCII - smallest size  
B) Latin-1 - supports Western languages  
C) UTF-8 - supports all Unicode  
D) UTF-16 - used by Windows

**Answer: C**

**Explanation:**  
**UTF-8 is the universal standard**:

Advantages:
```python
# ✓ Supports all Unicode characters (emoji, all languages)
"Hello مرحبا 你好 🌍".encode('utf-8')  # Works!

# ✓ Backward compatible with ASCII (1 byte for ASCII chars)
"ASCII".encode('utf-8')  # b'ASCII' (same as ASCII)

# ✓ Variable length (efficient for mixed content)
"hello".encode('utf-8')    # 5 bytes
"你好".encode('utf-8')      # 6 bytes (3 per character)

# ✓ Most widely used (web, JSON, Python 3 default)
```

Use UTF-8 unless you have specific constraints:
- Legacy systems (might need Latin-1)
- Windows APIs (sometimes UTF-16)
- Size optimization for pure ASCII (UTF-8 = ASCII anyway)

---

**Total Questions: 30**  
**Topics Covered**: String Operations (5), String Formatting (5), Searching & Pattern Matching (5), Regular Expressions (5), Analysis & Parsing (5), Encoding & Unicode (5)  
**Question Types**: Logic Flow, Debugging, Dry Run, Syntax Understanding, Data Analysis
