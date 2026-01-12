# Regular Expressions - Comprehensive Notes

## Overview
Regular Expressions (regex) are patterns used to match character combinations in strings. Powerful tool for text search, validation, and manipulation.

### Key Uses
- Pattern matching
- Text validation
- Search and replace
- Data extraction
- Input sanitization

---

## Python re Module

```python
import re

# Basic pattern matching
pattern = r'\d+'  # One or more digits
text = "I have 25 apples and 30 oranges"
matches = re.findall(pattern, text)
print(matches)  # ['25', '30']
```

---

## Regex Metacharacters

### 1. Basic Metacharacters

```python
# . (dot) - Any character except newline
pattern = r'h.t'
print(re.findall(pattern, "hat hit hot"))  # ['hat', 'hit', 'hot']

# ^ - Start of string
pattern = r'^Hello'
print(re.match(pattern, "Hello World"))    # Match
print(re.match(pattern, "Say Hello"))      # None

# $ - End of string
pattern = r'World$'
print(re.search(pattern, "Hello World"))   # Match
print(re.search(pattern, "World Hello"))   # None

# * - Zero or more occurrences
pattern = r'ab*'
print(re.findall(pattern, "a ab abb abbb"))  # ['a', 'ab', 'abb', 'abbb']

# + - One or more occurrences
pattern = r'ab+'
print(re.findall(pattern, "a ab abb abbb"))  # ['ab', 'abb', 'abbb']

# ? - Zero or one occurrence
pattern = r'colou?r'
print(re.findall(pattern, "color colour"))   # ['color', 'colour']

# {n} - Exactly n occurrences
pattern = r'\d{3}'
print(re.findall(pattern, "123 45 6789"))    # ['123', '678']

# {n,m} - Between n and m occurrences
pattern = r'\d{2,4}'
print(re.findall(pattern, "1 12 123 1234 12345"))  # ['12', '123', '1234', '1234']

# {n,} - n or more occurrences
pattern = r'\d{3,}'
print(re.findall(pattern, "12 123 1234"))    # ['123', '1234']
```

---

### 2. Character Classes

```python
# [abc] - Match a, b, or c
pattern = r'[aeiou]'
print(re.findall(pattern, "hello"))  # ['e', 'o']

# [^abc] - Not a, b, or c
pattern = r'[^aeiou]'
print(re.findall(pattern, "hello"))  # ['h', 'l', 'l']

# [a-z] - Range (lowercase letters)
pattern = r'[a-z]+'
print(re.findall(pattern, "Hello World"))  # ['ello', 'orld']

# [A-Z] - Uppercase letters
pattern = r'[A-Z]+'
print(re.findall(pattern, "Hello World"))  # ['H', 'W']

# [0-9] - Digits
pattern = r'[0-9]+'
print(re.findall(pattern, "Room 123"))  # ['123']

# [a-zA-Z0-9] - Alphanumeric
pattern = r'[a-zA-Z0-9]+'
print(re.findall(pattern, "user_123"))  # ['user', '123']
```

---

### 3. Predefined Character Classes

```python
# \d - Digit [0-9]
pattern = r'\d+'
print(re.findall(pattern, "Age: 25"))  # ['25']

# \D - Non-digit [^0-9]
pattern = r'\D+'
print(re.findall(pattern, "Age: 25"))  # ['Age: ']

# \w - Word character [a-zA-Z0-9_]
pattern = r'\w+'
print(re.findall(pattern, "hello_world-123"))  # ['hello_world', '123']

# \W - Non-word character
pattern = r'\W+'
print(re.findall(pattern, "hello, world!"))  # [', ', '!']

# \s - Whitespace [ \t\n\r\f\v]
pattern = r'\s+'
print(re.findall(pattern, "hello\tworld\n"))  # ['\t', '\n']

# \S - Non-whitespace
pattern = r'\S+'
print(re.findall(pattern, "hello world"))  # ['hello', 'world']

# \b - Word boundary
pattern = r'\bcat\b'
print(re.findall(pattern, "cat cats caterpillar"))  # ['cat']

# \B - Non-word boundary
pattern = r'\Bcat\B'
print(re.findall(pattern, "concatenate"))  # ['cat']
```

---

## Regex Functions

### 1. re.match()

```python
# Matches at beginning of string only
pattern = r'\d+'
print(re.match(pattern, "123abc"))  # <Match object>
print(re.match(pattern, "abc123"))  # None

# Example
def validate_code(code):
    """Code must start with 3 digits"""
    pattern = r'^\d{3}'
    return re.match(pattern, code) is not None

print(validate_code("123ABC"))  # True
print(validate_code("ABC123"))  # False
```

---

### 2. re.search()

```python
# Searches entire string, returns first match
pattern = r'\d+'
print(re.search(pattern, "abc123def"))  # <Match object>

# Get match details
match = re.search(r'\d+', "abc123def")
if match:
    print(match.group())   # '123'
    print(match.start())   # 3
    print(match.end())     # 6
    print(match.span())    # (3, 6)
```

#### Dry Run
```
text = "abc123def456"
pattern = r'\d+'

search() scans from left:
- 'a' no match
- 'b' no match
- 'c' no match
- '1' match start!
- '2' continues
- '3' continues
- 'd' stops matching

Returns match object for '123' at positions (3, 6)
```

---

### 3. re.findall()

```python
# Find all non-overlapping matches
pattern = r'\d+'
text = "Room 101, Floor 5, Building 23"
matches = re.findall(pattern, text)
print(matches)  # ['101', '5', '23']

# With groups
pattern = r'(\w+)@(\w+\.\w+)'
text = "Contact: alice@example.com, bob@test.org"
matches = re.findall(pattern, text)
print(matches)  # [('alice', 'example.com'), ('bob', 'test.org')]
```

---

### 4. re.finditer()

```python
# Returns iterator of match objects
pattern = r'\d+'
text = "abc123def456"

for match in re.finditer(pattern, text):
    print(f"Found {match.group()} at {match.span()}")

# Output:
# Found 123 at (3, 6)
# Found 456 at (9, 12)
```

---

### 5. re.sub()

```python
# Replace matches with replacement string
pattern = r'\d+'
text = "Price: $100, Quantity: 5"
result = re.sub(pattern, 'X', text)
print(result)  # 'Price: $X, Quantity: X'

# With limit
result = re.sub(pattern, 'X', text, count=1)
print(result)  # 'Price: $X, Quantity: 5'

# With function
def double(match):
    return str(int(match.group()) * 2)

result = re.sub(r'\d+', double, "10 20 30")
print(result)  # '20 40 60'

# Backreferences in replacement
pattern = r'(\w+) (\w+)'
text = "John Doe"
result = re.sub(pattern, r'\2, \1', text)
print(result)  # 'Doe, John'
```

---

### 6. re.split()

```python
# Split string by pattern
pattern = r'[,;]'
text = "apple,banana;cherry"
parts = re.split(pattern, text)
print(parts)  # ['apple', 'banana', 'cherry']

# Split by multiple whitespace
pattern = r'\s+'
text = "hello    world\t\tpython"
parts = re.split(pattern, text)
print(parts)  # ['hello', 'world', 'python']

# With limit
pattern = r','
text = "a,b,c,d,e"
parts = re.split(pattern, text, maxsplit=2)
print(parts)  # ['a', 'b', 'c,d,e']
```

---

## Groups and Capturing

### 1. Basic Groups

```python
# () - Capturing group
pattern = r'(\d{3})-(\d{4})'
text = "Phone: 123-4567"
match = re.search(pattern, text)

if match:
    print(match.group(0))  # '123-4567' - entire match
    print(match.group(1))  # '123' - first group
    print(match.group(2))  # '4567' - second group
    print(match.groups())  # ('123', '4567') - all groups

# Named groups
pattern = r'(?P<area>\d{3})-(?P<number>\d{4})'
match = re.search(pattern, text)

if match:
    print(match.group('area'))    # '123'
    print(match.group('number'))  # '4567'
    print(match.groupdict())      # {'area': '123', 'number': '4567'}
```

---

### 2. Non-Capturing Groups

```python
# (?:...) - Non-capturing group
# Groups for matching but not capturing

pattern = r'(?:Mr|Ms|Mrs)\.\s(\w+)'
text = "Hello Mr. Smith and Ms. Jones"

matches = re.findall(pattern, text)
print(matches)  # ['Smith', 'Jones']
# Only names captured, not titles
```

---

### 3. Lookahead and Lookbehind

```python
# Positive lookahead (?=...)
# Match if followed by pattern
pattern = r'\d+(?= dollars)'
text = "Price: 100 dollars and 50 cents"
matches = re.findall(pattern, text)
print(matches)  # ['100']

# Negative lookahead (?!...)
# Match if NOT followed by pattern
pattern = r'\d+(?! dollars)'
text = "Price: 100 dollars and 50 cents"
matches = re.findall(pattern, text)
print(matches)  # ['10', '5', '0'] (digits not followed by ' dollars')

# Positive lookbehind (?<=...)
# Match if preceded by pattern
pattern = r'(?<=\$)\d+'
text = "Price: $100 and €50"
matches = re.findall(pattern, text)
print(matches)  # ['100']

# Negative lookbehind (?<!...)
# Match if NOT preceded by pattern
pattern = r'(?<!\$)\d+'
text = "$100 and 50"
matches = re.findall(pattern, text)
print(matches)  # ['0', '0', '50'] (digits not preceded by $)
```

---

## Common Regex Patterns

### 1. Email Validation

```python
def validate_email(email):
    """
    Validate email address
    """
    pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
    return re.match(pattern, email) is not None

print(validate_email("user@example.com"))      # True
print(validate_email("user.name@test.co.uk"))  # True
print(validate_email("invalid@"))              # False
print(validate_email("@invalid.com"))          # False
```

#### Pattern Breakdown
```
^                   # Start
[a-zA-Z0-9._%+-]+  # Username: letters, digits, special chars
@                   # @ symbol
[a-zA-Z0-9.-]+     # Domain name
\.                  # Dot (escaped)
[a-zA-Z]{2,}       # TLD: 2+ letters
$                   # End
```

---

### 2. Phone Number

```python
def validate_phone(phone):
    """
    Validate phone number
    Formats: (123) 456-7890, 123-456-7890, 1234567890
    """
    pattern = r'^\(?(\d{3})\)?[-.\s]?(\d{3})[-.\s]?(\d{4})$'
    return re.match(pattern, phone) is not None

print(validate_phone("(123) 456-7890"))  # True
print(validate_phone("123-456-7890"))    # True
print(validate_phone("1234567890"))      # True
print(validate_phone("123-45-6789"))     # False
```

---

### 3. URL Validation

```python
def validate_url(url):
    """
    Validate URL
    """
    pattern = r'^https?://(?:www\.)?[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}\b(?:[-a-zA-Z0-9()@:%_\+.~#?&/=]*)$'
    return re.match(pattern, url) is not None

print(validate_url("https://example.com"))         # True
print(validate_url("http://www.example.com/path")) # True
print(validate_url("example.com"))                 # False
```

---

### 4. Password Strength

```python
def check_password_strength(password):
    """
    Password must have:
    - At least 8 characters
    - At least one uppercase
    - At least one lowercase
    - At least one digit
    - At least one special character
    """
    if len(password) < 8:
        return False
    
    checks = [
        r'[A-Z]',           # Uppercase
        r'[a-z]',           # Lowercase
        r'\d',              # Digit
        r'[!@#$%^&*(),.?":{}|<>]'  # Special char
    ]
    
    for pattern in checks:
        if not re.search(pattern, password):
            return False
    
    return True

print(check_password_strength("Weak123"))         # False (no special char)
print(check_password_strength("Strong123!"))      # True
```

---

### 5. Extract Data

```python
# Extract phone numbers
def extract_phone_numbers(text):
    pattern = r'\(?\d{3}\)?[-.\s]?\d{3}[-.\s]?\d{4}'
    return re.findall(pattern, text)

text = "Call me at (123) 456-7890 or 987-654-3210"
print(extract_phone_numbers(text))
# ['(123) 456-7890', '987-654-3210']

# Extract emails
def extract_emails(text):
    pattern = r'[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}'
    return re.findall(pattern, text)

text = "Contact: alice@example.com, bob@test.org"
print(extract_emails(text))
# ['alice@example.com', 'bob@test.org']

# Extract hashtags
def extract_hashtags(text):
    pattern = r'#\w+'
    return re.findall(pattern, text)

text = "Check out #python and #regex #programming"
print(extract_hashtags(text))
# ['#python', '#regex', '#programming']

# Extract URLs
def extract_urls(text):
    pattern = r'https?://(?:[-\w.])+(?:/[\w/_.-]*)?'
    return re.findall(pattern, text)

text = "Visit https://example.com and http://test.org/path"
print(extract_urls(text))
# ['https://example.com', 'http://test.org/path']
```

---

### 6. Date Formats

```python
def extract_dates(text):
    """
    Extract dates in various formats
    MM/DD/YYYY, DD-MM-YYYY, YYYY.MM.DD
    """
    patterns = [
        r'\d{2}/\d{2}/\d{4}',  # MM/DD/YYYY
        r'\d{2}-\d{2}-\d{4}',  # DD-MM-YYYY
        r'\d{4}\.\d{2}\.\d{2}' # YYYY.MM.DD
    ]
    
    dates = []
    for pattern in patterns:
        dates.extend(re.findall(pattern, text))
    
    return dates

text = "Dates: 12/31/2023, 31-12-2023, 2023.12.31"
print(extract_dates(text))
# ['12/31/2023', '31-12-2023', '2023.12.31']
```

---

## Regex Flags

```python
# re.IGNORECASE (re.I) - Case-insensitive
pattern = r'hello'
print(re.findall(pattern, "Hello HELLO hello", re.I))
# ['Hello', 'HELLO', 'hello']

# re.MULTILINE (re.M) - ^ and $ match line boundaries
text = "Line1\nLine2\nLine3"
pattern = r'^Line\d'
print(re.findall(pattern, text, re.M))
# ['Line1', 'Line2', 'Line3']

# re.DOTALL (re.S) - . matches newline
text = "Hello\nWorld"
pattern = r'Hello.World'
print(re.search(pattern, text, re.S))  # Match

# re.VERBOSE (re.X) - Allow comments and whitespace
pattern = r'''
    \d{3}    # Area code
    [-.]     # Separator
    \d{4}    # Number
'''
print(re.findall(pattern, "123-4567 890.1234", re.X))
# ['123-4567', '890.1234']

# Combine flags
pattern = r'hello'
text = "Hello\nHELLO"
print(re.findall(pattern, text, re.I | re.M))
# ['Hello', 'HELLO']
```

---

## Advanced Patterns

### 1. Backreferences

```python
# \1, \2, ... refer to captured groups
# Find repeated words
pattern = r'\b(\w+)\s+\1\b'
text = "This is is a test test"
matches = re.findall(pattern, text)
print(matches)  # ['is', 'test']

# Remove repeated words
result = re.sub(pattern, r'\1', text)
print(result)  # 'This is a test'
```

---

### 2. Greedy vs Non-Greedy

```python
text = "<tag>content</tag>"

# Greedy (default) - matches as much as possible
pattern = r'<.*>'
print(re.findall(pattern, text))  # ['<tag>content</tag>']

# Non-greedy - matches as little as possible
pattern = r'<.*?>'
print(re.findall(pattern, text))  # ['<tag>', '</tag>']

# Example: Extract tag content
pattern = r'<.*?>(.*?)</.*?>'
print(re.findall(pattern, text))  # ['content']
```

---

### 3. Conditional Patterns

```python
# (?(condition)yes|no)
# Match phone with optional country code
pattern = r'^(\+\d{1,3})?\(?\d{3}\)?[-.\s]?\d{3}[-.\s]?\d{4}$'

print(re.match(pattern, "+1 (123) 456-7890"))  # Match
print(re.match(pattern, "(123) 456-7890"))     # Match
print(re.match(pattern, "123-456-7890"))       # Match
```

---

## Regex Compilation

```python
# Compile pattern for reuse (more efficient)
pattern = re.compile(r'\d+')

text1 = "abc 123 def"
text2 = "xyz 456 uvw"

print(pattern.findall(text1))  # ['123']
print(pattern.findall(text2))  # ['456']

# With flags
pattern = re.compile(r'hello', re.IGNORECASE)
print(pattern.findall("Hello HELLO hello"))
# ['Hello', 'HELLO', 'hello']
```

---

## Common Regex Mistakes

### 1. Not Escaping Special Characters

```python
# Wrong
pattern = r'$100'  # $ is end-of-line anchor
text = "$100"
print(re.search(pattern, text))  # None

# Correct
pattern = r'\$100'
print(re.search(pattern, text))  # Match
```

---

### 2. Greedy Matching Issues

```python
# Wrong - too greedy
pattern = r'".*"'
text = '"first" and "second"'
print(re.findall(pattern, text))  # ['"first" and "second"']

# Correct - non-greedy
pattern = r'".*?"'
print(re.findall(pattern, text))  # ['"first"', '"second"']
```

---

### 3. Forgetting Anchors

```python
# Wrong - partial match
pattern = r'\d{4}'
print(re.match(pattern, "12345"))  # Match (first 4 digits)

# Correct - exact match
pattern = r'^\d{4}$'
print(re.match(pattern, "12345"))  # None
print(re.match(pattern, "1234"))   # Match
```

---

## Time Complexity

| Operation | Average | Worst |
|-----------|---------|-------|
| Match | O(n) | O(2^n) |
| Search | O(n) | O(2^n) |
| Findall | O(n*m) | O(2^n*m) |
| Sub | O(n*m) | O(2^n*m) |

Note: Worst case with catastrophic backtracking

---

## Exam Tips

### 1. MCQ Questions
**Q**: \d+ matches?
**A**: One or more digits

**Q**: ^ in regex means?
**A**: Start of string (or line with MULTILINE)

**Q**: * vs +?
**A**: * is 0 or more, + is 1 or more

**Q**: Greedy vs non-greedy?
**A**: Greedy matches max, non-greedy (.*?) matches min

### 2. Quick Reference
```
.     Any character
\d    Digit
\w    Word character
\s    Whitespace
^     Start
$     End
*     0 or more
+     1 or more
?     0 or 1
{n}   Exactly n
()    Group
[]    Character class
|     Or
```

### Key Points
1. **Metacharacters**: Special meaning characters
2. **Character Classes**: [abc], [a-z], \d, \w, \s
3. **Quantifiers**: *, +, ?, {n}, {n,m}
4. **Anchors**: ^, $, \b
5. **Groups**: (), (?:), (?P<name>)
6. **Lookahead/Lookbehind**: (?=), (?!), (?<=), (?<!)
7. **Flags**: re.I, re.M, re.S, re.X
8. **Performance**: Compile patterns for reuse
