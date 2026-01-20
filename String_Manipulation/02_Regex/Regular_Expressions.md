# Regular Expressions - Comprehensive Multi-Language Notes

## Overview
Regular Expressions (regex) are patterns used to match character combinations in strings. Powerful tool for text search, validation, and manipulation across all programming languages.

### Key Uses
- Pattern matching
- Text validation
- Search and replace
- Data extraction
- Input sanitization

### Language Support Comparison

| Feature | Python (`re`) | C (POSIX) | C++ (`<regex>`) | Java (`Pattern`) |
|---------|--------------|-----------|-----------------|------------------|
| **Standard** | Built-in | POSIX regex | C++11+ | Built-in |
| **Performance** | Good | Fast | Good | Good |
| **Syntax** | Perl-like | POSIX ERE | ECMAScript | Java flavor |
| **Unicode** | Full | Limited | Full | Full |

---

## Basic Pattern Matching

### Python re Module

#### Python
```python
import re

# Basic pattern matching
pattern = r'\d+'  # One or more digits
text = "I have 25 apples and 30 oranges"
matches = re.findall(pattern, text)
print(matches)  # ['25', '30']

# Search for pattern
match = re.search(r'\d+', text)
if match:
    print(f"Found: {match.group()}")  # Found: 25

# Match at start
if re.match(r'I have', text):
    print("Text starts with 'I have'")
```

#### C (POSIX Regex)
```c
#include <stdio.h>
#include <regex.h>
#include <string.h>

int main() {
    regex_t regex;
    regmatch_t matches[10];
    char text[] = "I have 25 apples and 30 oranges";
    
    // Compile regex pattern
    // REG_EXTENDED = Extended Regular Expressions
    if (regcomp(&regex, "[0-9]+", REG_EXTENDED) != 0) {
        printf("Failed to compile regex\n");
        return 1;
    }
    
    // Execute regex
    if (regexec(&regex, text, 1, matches, 0) == 0) {
        printf("Match found at position %d\n", matches[0].rm_so);
        
        // Extract matched text
        int start = matches[0].rm_so;
        int end = matches[0].rm_eo;
        int len = end - start;
        char matched[len + 1];
        strncpy(matched, text + start, len);
        matched[len] = '\0';
        printf("Matched: %s\n", matched);  // Matched: 25
    }
    
    // Free regex
    regfree(&regex);
    
    return 0;
}
```

#### C++
```cpp
#include <iostream>
#include <regex>
#include <string>
using namespace std;

int main() {
    string text = "I have 25 apples and 30 oranges";
    regex pattern(R"(\d+)");  // One or more digits
    
    // Find first match
    smatch match;
    if (regex_search(text, match, pattern)) {
        cout << "Found: " << match.str() << endl;  // Found: 25
        cout << "Position: " << match.position() << endl;  // Position: 7
    }
    
    // Find all matches
    cout << "All matches: ";
    auto begin = sregex_iterator(text.begin(), text.end(), pattern);
    auto end = sregex_iterator();
    
    for (auto it = begin; it != end; ++it) {
        cout << it->str() << " ";  // 25 30
    }
    cout << endl;
    
    // Match at start
    regex start_pattern(R"(^I have)");
    if (regex_search(text, start_pattern)) {
        cout << "Text starts with 'I have'" << endl;
    }
    
    return 0;
}
```

#### Java
```java
import java.util.regex.*;

public class BasicRegex {
    public static void main(String[] args) {
        String text = "I have 25 apples and 30 oranges";
        String pattern = "\\d+";  // One or more digits (escaped backslash)
        
        // Find first match
        Pattern p = Pattern.compile(pattern);
        Matcher m = p.matcher(text);
        
        if (m.find()) {
            System.out.println("Found: " + m.group());      // Found: 25
            System.out.println("Position: " + m.start());   // Position: 7
        }
        
        // Find all matches
        System.out.print("All matches: ");
        m.reset();  // Reset matcher to find from start
        while (m.find()) {
            System.out.print(m.group() + " ");  // 25 30
        }
        System.out.println();
        
        // Match at start
        Pattern startPattern = Pattern.compile("^I have");
        if (startPattern.matcher(text).find()) {
            System.out.println("Text starts with 'I have'");
        }
        
        // Using matches() - must match entire string
        if (Pattern.matches("\\d+", "123")) {
            System.out.println("'123' is all digits");
        }
    }
}
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

#### Python
```python
import re

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

#### C
```c
#include <stdio.h>
#include <regex.h>

int validate_email(const char *email) {
    regex_t regex;
    int result;
    
    // Email pattern
    const char *pattern = "^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,}$";
    
    // Compile regex
    if (regcomp(&regex, pattern, REG_EXTENDED | REG_NOSUB) != 0) {
        return -1;  // Error
    }
    
    // Execute regex
    result = regexec(&regex, email, 0, NULL, 0);
    
    // Free regex
    regfree(&regex);
    
    return (result == 0) ? 1 : 0;  // 1 = valid, 0 = invalid
}

int main() {
    printf("user@example.com: %d\n", validate_email("user@example.com"));      // 1
    printf("invalid@: %d\n", validate_email("invalid@"));                        // 0
    
    return 0;
}
```

#### C++
```cpp
#include <iostream>
#include <regex>
#include <string>
using namespace std;

bool validate_email(const string& email) {
    regex pattern(R"(^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$)");
    return regex_match(email, pattern);
}

int main() {
    cout << "user@example.com: " << validate_email("user@example.com") << endl;      // 1
    cout << "user.name@test.co.uk: " << validate_email("user.name@test.co.uk") << endl;  // 1
    cout << "invalid@: " << validate_email("invalid@") << endl;                       // 0
    cout << "@invalid.com: " << validate_email("@invalid.com") << endl;               // 0
    
    return 0;
}
```

#### Java
```java
import java.util.regex.*;

public class EmailValidation {
    public static boolean validateEmail(String email) {
        String pattern = "^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,}$";
        return Pattern.matches(pattern, email);
    }
    
    public static void main(String[] args) {
        System.out.println("user@example.com: " + validateEmail("user@example.com"));      // true
        System.out.println("user.name@test.co.uk: " + validateEmail("user.name@test.co.uk"));  // true
        System.out.println("invalid@: " + validateEmail("invalid@"));                        // false
        System.out.println("@invalid.com: " + validateEmail("@invalid.com"));                // false
    }
}
```

#### Pattern Breakdown
```
^                   # Start of string
[a-zA-Z0-9._%+-]+  # Username: letters, digits, special chars
@                   # @ symbol (required)
[a-zA-Z0-9.-]+     # Domain name
\.                  # Dot (literal, escaped)
[a-zA-Z]{2,}       # TLD: 2+ letters (com, org, uk, etc.)
$                   # End of string
```

---

### 2. Phone Number Validation

#### Python
```python
import re

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

#### C
```c
#include <stdio.h>
#include <regex.h>

int validate_phone(const char *phone) {
    regex_t regex;
    int result;
    
    // Phone pattern
    const char *pattern = "^\\(?[0-9]{3}\\)?[-. ]?[0-9]{3}[-. ]?[0-9]{4}$";
    
    if (regcomp(&regex, pattern, REG_EXTENDED | REG_NOSUB) != 0) {
        return -1;
    }
    
    result = regexec(&regex, phone, 0, NULL, 0);
    regfree(&regex);
    
    return (result == 0) ? 1 : 0;
}

int main() {
    printf("(123) 456-7890: %d\n", validate_phone("(123) 456-7890"));  // 1
    printf("123-456-7890: %d\n", validate_phone("123-456-7890"));      // 1
    printf("1234567890: %d\n", validate_phone("1234567890"));          // 1
    
    return 0;
}
```

#### C++
```cpp
#include <iostream>
#include <regex>
#include <string>
using namespace std;

bool validate_phone(const string& phone) {
    regex pattern(R"(^\(?\d{3}\)?[-.\s]?\d{3}[-.\s]?\d{4}$)");
    return regex_match(phone, pattern);
}

int main() {
    cout << "(123) 456-7890: " << validate_phone("(123) 456-7890") << endl;  // 1
    cout << "123-456-7890: " << validate_phone("123-456-7890") << endl;      // 1
    cout << "1234567890: " << validate_phone("1234567890") << endl;          // 1
    cout << "123-45-6789: " << validate_phone("123-45-6789") << endl;        // 0
    
    return 0;
}
```

#### Java
```java
import java.util.regex.*;

public class PhoneValidation {
    public static boolean validatePhone(String phone) {
        String pattern = "^\\(?\\d{3}\\)?[-\.\\s]?\\d{3}[-\.\\s]?\\d{4}$";
        return Pattern.matches(pattern, phone);
    }
    
    public static void main(String[] args) {
        System.out.println("(123) 456-7890: " + validatePhone("(123) 456-7890"));  // true
        System.out.println("123-456-7890: " + validatePhone("123-456-7890"));      // true
        System.out.println("1234567890: " + validatePhone("1234567890"));          // true
        System.out.println("123-45-6789: " + validatePhone("123-45-6789"));        // false
    }
}
```

---

### 3. URL Validation

#### Python
```python
import re

def validate_url(url):
    """Validate URL"""
    pattern = r'^https?://(?:www\.)?[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}\b(?:[-a-zA-Z0-9()@:%_\+.~#?&/=]*)$'
    return re.match(pattern, url) is not None

print(validate_url("https://example.com"))         # True
print(validate_url("http://www.example.com/path")) # True
print(validate_url("example.com"))                 # False
```

#### C++
```cpp
#include <iostream>
#include <regex>
using namespace std;

bool validate_url(const string& url) {
    regex pattern(R"(^https?://(?:www\.)?[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}\b(?:[-a-zA-Z0-9()@:%_\+.~#?&/=]*)$)");
    return regex_match(url, pattern);
}

int main() {
    cout << "https://example.com: " << validate_url("https://example.com") << endl;  // 1
    cout << "http://www.example.com/path: " << validate_url("http://www.example.com/path") << endl;  // 1
    cout << "example.com: " << validate_url("example.com") << endl;  // 0
    
    return 0;
}
```

#### Java
```java
import java.util.regex.*;

public class URLValidation {
    public static boolean validateURL(String url) {
        String pattern = "^https?://(?:www\\.)?[-a-zA-Z0-9@:%._\\+~#=]{1,256}\\.[a-zA-Z0-9()]{1,6}\\b(?:[-a-zA-Z0-9()@:%_\\+.~#?&/=]*)$";
        return Pattern.matches(pattern, url);
    }
    
    public static void main(String[] args) {
        System.out.println("https://example.com: " + validateURL("https://example.com"));  // true
        System.out.println("http://www.example.com/path: " + validateURL("http://www.example.com/path"));  // true
        System.out.println("example.com: " + validateURL("example.com"));  // false
    }
}
```

---

### 4. Password Strength Check

#### Python
```python
import re

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

#### C++
```cpp
#include <iostream>
#include <regex>
using namespace std;

bool check_password_strength(const string& password) {
    if (password.length() < 8) return false;
    
    regex upper(R"([A-Z])");
    regex lower(R"([a-z])");
    regex digit(R"(\d)");
    regex special(R"([!@#$%^&*(),.?":{}|<>])");
    
    return regex_search(password, upper) &&
           regex_search(password, lower) &&
           regex_search(password, digit) &&
           regex_search(password, special);
}

int main() {
    cout << "Weak123: " << check_password_strength("Weak123") << endl;         // 0
    cout << "Strong123!: " << check_password_strength("Strong123!") << endl;   // 1
    
    return 0;
}
```

#### Java
```java
import java.util.regex.*;

public class PasswordStrength {
    public static boolean checkPasswordStrength(String password) {
        if (password.length() < 8) return false;
        
        String[] patterns = {
            "[A-Z]",                          // Uppercase
            "[a-z]",                          // Lowercase
            "\\d",                            // Digit
            "[!@#$%^&*(),.?\":{}|<>]"        // Special char
        };
        
        for (String pattern : patterns) {
            if (!Pattern.compile(pattern).matcher(password).find()) {
                return false;
            }
        }
        
        return true;
    }
    
    public static void main(String[] args) {
        System.out.println("Weak123: " + checkPasswordStrength("Weak123"));         // false
        System.out.println("Strong123!: " + checkPasswordStrength("Strong123!"));   // true
    }
}
```

---

### 5. Extract Data

#### Python
```python
import re

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
```

#### C++
```cpp
#include <iostream>
#include <regex>
#include <string>
#include <vector>
using namespace std;

vector<string> extract_phone_numbers(const string& text) {
    vector<string> results;
    regex pattern(R"(\(?\d{3}\)?[-.\s]?\d{3}[-.\s]?\d{4})");
    
    auto begin = sregex_iterator(text.begin(), text.end(), pattern);
    auto end = sregex_iterator();
    
    for (auto it = begin; it != end; ++it) {
        results.push_back(it->str());
    }
    
    return results;
}

int main() {
    string text = "Call me at (123) 456-7890 or 987-654-3210";
    vector<string> phones = extract_phone_numbers(text);
    
    cout << "Phone numbers: ";
    for (const auto& phone : phones) {
        cout << phone << " ";
    }
    cout << endl;
    // Phone numbers: (123) 456-7890 987-654-3210
    
    return 0;
}
```

#### Java
```java
import java.util.regex.*;
import java.util.ArrayList;
import java.util.List;

public class DataExtraction {
    public static List<String> extractPhoneNumbers(String text) {
        List<String> results = new ArrayList<>();
        String pattern = "\\(?\\d{3}\\)?[-\\.\\s]?\\d{3}[-\\.\\s]?\\d{4}";
        
        Pattern p = Pattern.compile(pattern);
        Matcher m = p.matcher(text);
        
        while (m.find()) {
            results.add(m.group());
        }
        
        return results;
    }
    
    public static List<String> extractEmails(String text) {
        List<String> results = new ArrayList<>();
        String pattern = "[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,}";
        
        Pattern p = Pattern.compile(pattern);
        Matcher m = p.matcher(text);
        
        while (m.find()) {
            results.add(m.group());
        }
        
        return results;
    }
    
    public static void main(String[] args) {
        String text1 = "Call me at (123) 456-7890 or 987-654-3210";
        System.out.println("Phone numbers: " + extractPhoneNumbers(text1));
        // Phone numbers: [(123) 456-7890, 987-654-3210]
        
        String text2 = "Contact: alice@example.com, bob@test.org";
        System.out.println("Emails: " + extractEmails(text2));
        // Emails: [alice@example.com, bob@test.org]
    }
}
```

---

## String Replacement with Regex

### Replace Matches

#### Python
```python
import re

# Replace all matches
pattern = r'\d+'
text = "Price: $100, Quantity: 5"
result = re.sub(pattern, 'X', text)
print(result)  # 'Price: $X, Quantity: X'

# Replace with limit
result = re.sub(pattern, 'X', text, count=1)
print(result)  # 'Price: $X, Quantity: 5'

# Replace with function
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

#### C++ 
```cpp
#include <iostream>
#include <regex>
#include <string>
using namespace std;

int main() {
    string text = "Price: $100, Quantity: 5";
    regex pattern(R"(\d+)");
    
    // Replace all matches
    string result = regex_replace(text, pattern, "X");
    cout << "Replace all: " << result << endl;
    // Replace all: Price: $X, Quantity: X
    
    // Replace first match only
    result = regex_replace(text, pattern, "X", 
                          regex_constants::format_first_only);
    cout << "Replace first: " << result << endl;
    // Replace first: Price: $X, Quantity: 5
    
    // Backreferences
    string name = "John Doe";
    regex name_pattern(R"((\w+) (\w+))");
    result = regex_replace(name, name_pattern, "$2, $1");
    cout << "Swapped: " << result << endl;
    // Swapped: Doe, John
    
    return 0;
}
```

#### Java
```java
import java.util.regex.*;

public class RegexReplace {
    public static void main(String[] args) {
        String text = "Price: $100, Quantity: 5";
        String pattern = "\\d+";
        
        // Replace all matches
        String result = text.replaceAll(pattern, "X");
        System.out.println("Replace all: " + result);
        // Replace all: Price: $X, Quantity: X
        
        // Replace first match
        result = text.replaceFirst(pattern, "X");
        System.out.println("Replace first: " + result);
        // Replace first: Price: $X, Quantity: 5
        
        // Replace with function (Java 9+)
        Pattern p = Pattern.compile(pattern);
        Matcher m = p.matcher("10 20 30");
        StringBuffer sb = new StringBuffer();
        
        while (m.find()) {
            int doubled = Integer.parseInt(m.group()) * 2;
            m.appendReplacement(sb, String.valueOf(doubled));
        }
        m.appendTail(sb);
        System.out.println("Doubled: " + sb.toString());
        // Doubled: 20 40 60
        
        // Backreferences
        String name = "John Doe";
        result = name.replaceAll("(\\w+) (\\w+)", "$2, $1");
        System.out.println("Swapped: " + result);
        // Swapped: Doe, John
    }
}
```

---

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

---

## Language-Specific Quick Reference

### Python (re module)
```python
import re

# Main functions
re.match(pattern, string)      # Match at start
re.search(pattern, string)     # Find first match
re.findall(pattern, string)    # Find all matches
re.finditer(pattern, string)   # Iterator of matches
re.sub(pattern, repl, string)  # Replace matches
re.split(pattern, string)      # Split by pattern

# Compile for reuse
pattern = re.compile(r'\d+')
pattern.findall(text)

# Flags
re.IGNORECASE or re.I         # Case-insensitive
re.MULTILINE or re.M          # ^ and $ match line boundaries
re.DOTALL or re.S             # . matches newline
re.VERBOSE or re.X            # Allow comments
```

### C (POSIX regex.h)
```c
#include <regex.h>

regex_t regex;
regmatch_t matches[10];

// Compile pattern
regcomp(&regex, pattern, REG_EXTENDED);

// Execute (0 = match, REG_NOMATCH = no match)
int result = regexec(&regex, text, nmatch, matches, 0);

// Free
regfree(&regex);

// Flags
REG_EXTENDED      # Extended Regular Expressions
REG_ICASE         # Case-insensitive
REG_NOSUB         # Don't store match positions
REG_NEWLINE       # ^ and $ match line boundaries
```

### C++ (<regex>)
```cpp
#include <regex>
using namespace std;

regex pattern(R"(\d+)");
smatch match;

// Main functions
regex_match(text, match, pattern)      // Match entire string
regex_search(text, match, pattern)     // Find first match
regex_replace(text, pattern, repl)     // Replace matches

// Iterate all matches
sregex_iterator begin(text.begin(), text.end(), pattern);
sregex_iterator end;
for (auto it = begin; it != end; ++it) {
    cout << it->str();
}

// Flags (regex_constants::)
icase         # Case-insensitive
ECMAScript    # ECMAScript grammar (default)
extended      # POSIX extended grammar
```

### Java (java.util.regex)
```java
import java.util.regex.*;

Pattern pattern = Pattern.compile("\\d+");
Matcher matcher = pattern.matcher(text);

// Main methods
matcher.matches()     # Match entire string
matcher.find()        # Find next match
matcher.group()       # Get matched text
matcher.start()       # Start position
matcher.end()         # End position

// String methods
text.matches(pattern)           # Match entire string
text.replaceAll(pattern, repl)  # Replace all
text.replaceFirst(pattern, repl)# Replace first
text.split(pattern)             # Split by pattern

// Flags
Pattern.CASE_INSENSITIVE  # Case-insensitive
Pattern.MULTILINE         # ^ and $ match line boundaries
Pattern.DOTALL           # . matches newline
Pattern.COMMENTS         # Allow whitespace and comments
```

---

## Common Regex Patterns Reference

| Pattern | Description | Example |
|---------|-------------|---------|
| `.` | Any character | `a.c` matches abc, aXc |
| `^` | Start of string | `^Hello` matches "Hello..." |
| `$` | End of string | `World$` matches "...World" |
| `*` | 0 or more | `ab*` matches a, ab, abb |
| `+` | 1 or more | `ab+` matches ab, abb |
| `?` | 0 or 1 | `colou?r` matches color, colour |
| `{n}` | Exactly n | `\d{3}` matches 123 |
| `{n,m}` | Between n and m | `\d{2,4}` matches 12, 123, 1234 |
| `[abc]` | Character class | Matches a, b, or c |
| `[^abc]` | Negated class | Not a, b, or c |
| `\d` | Digit | `[0-9]` |
| `\w` | Word character | `[a-zA-Z0-9_]` |
| `\s` | Whitespace | Space, tab, newline |
| `\b` | Word boundary | Boundary between \w and \W |
| `\|` | OR | `cat\|dog` matches cat or dog |
| `()` | Capturing group | Group and capture |
| `(?:)` | Non-capturing group | Group without capturing |

---

## Time Complexity

| Operation | Python | C | C++ | Java |
|-----------|--------|---|-----|------|
| **Compile** | O(m) | O(m) | O(m) | O(m) |
| **Match** | O(n) avg | O(n) avg | O(n) avg | O(n) avg |
| **Search** | O(n×m) worst | O(n×m) worst | O(n×m) worst | O(n×m) worst |
| **Replace** | O(n×m) | - | O(n×m) | O(n×m) |

*Where n = text length, m = pattern length*

---

## Exam Tips

### Key Points
1. **Always compile patterns** for reuse (performance)
2. **Escape special characters**: `. * + ? ^ $ ( ) [ ] { } \ |`
3. **Use raw strings** in Python: `r'\d+'` not `'\\d+'`
4. **Double escape in Java**: `"\\d+"` not `"\d+"`
5. **Use character classes**: `\d`, `\w`, `\s` for clarity
6. **Non-greedy matching**: `.*?` instead of `.*`
7. **Anchors**: `^` and `$` for full string match
8. **Word boundaries**: `\b` for whole word matching

### Common Mistakes
```
❌ Wrong: pattern = "*.txt"        → Invalid (nothing to repeat)
✅ Right: pattern = ".*\.txt"      → Matches any .txt file

❌ Wrong: pattern = "\d+"          → Python interprets \d as escape
✅ Right: pattern = r"\d+"         → Raw string, regex sees \d

❌ Wrong: pattern = "^test"        → Only matches at string start
✅ Right: pattern = "test"         → Matches anywhere

❌ Wrong: email = ".+@.+\..+"      → Too greedy, allows invalid
✅ Right: email = "[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}"
```

---

## END OF REGULAR EXPRESSIONS GUIDE

**Master regex patterns for powerful text processing across all languages! 🚀**
