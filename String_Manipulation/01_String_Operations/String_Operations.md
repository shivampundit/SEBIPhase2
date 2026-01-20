# String Operations - Comprehensive Multi-Language Notes

## Overview
Strings are sequences of characters used for text processing, pattern matching, and data manipulation. This guide covers string operations in **Python, C, C++, and Java**.

### Key Properties (Language Comparison)

| Property | Python | C | C++ | Java |
|----------|--------|---|-----|------|
| **Mutability** | Immutable | Mutable (char array) | Mutable (char array), Immutable (string) | Immutable |
| **Indexed** | 0-based | 0-based | 0-based | 0-based |
| **Null-terminated** | No | Yes ('\0') | Yes (C-style), No (std::string) | No |
| **Built-in Type** | Yes | No (char array) | Yes (std::string) | Yes (String class) |

---

## String Basics

### 1. String Creation

#### Python
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

#### C
```c
#include <stdio.h>
#include <string.h>

int main() {
    // Character array (null-terminated)
    char s1[] = "Hello";              // Automatic size
    char s2[20] = "World";            // Fixed size
    char s3[] = {'H', 'i', '\0'};     // Character by character
    
    // Concatenation (using strcat)
    char s4[50] = "Hello";
    strcat(s4, " World");             // s4 = "Hello World"
    
    // String from integer (using sprintf)
    char s5[10];
    int num = 123;
    sprintf(s5, "%d", num);           // s5 = "123"
    
    return 0;
}
```

#### C++
```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    // std::string (recommended)
    string s1 = "Hello";                    // Direct initialization
    string s2("World");                      // Constructor
    string s3(5, 'A');                       // "AAAAA"
    
    // C-style strings (char array)
    char s4[] = "Hello";
    
    // Convert to string
    string s5 = to_string(123);              // "123"
    
    // Concatenation
    string s6 = "Hello" + string(" World");  // "Hello World"
    string s7 = s1 + " " + s2;               // "Hello World"
    
    return 0;
}
```

#### Java
```java
public class StringCreation {
    public static void main(String[] args) {
        // Different ways to create strings
        String s1 = "Hello";                    // String literal
        String s2 = new String("World");        // String object
        
        // Multi-line strings (Java 15+)
        String s3 = """
                    Multi
                    line""";
        
        // Convert to string
        String s4 = String.valueOf(123);        // "123"
        String s5 = Integer.toString(123);      // "123"
        
        // Concatenation
        String s6 = "Hello" + " " + "World";    // "Hello World"
        String s7 = s1.concat(" ").concat(s2);  // "Hello World"
        
        // Repeat (Java 11+)
        String s8 = "Java".repeat(3);           // "JavaJavaJava"
    }
}
```

---

### 2. String Length

#### Python
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

#### C
```c
#include <stdio.h>
#include <string.h>

int main() {
    char text[] = "Hello World";
    
    // Using strlen() - O(n) time
    int length = strlen(text);
    printf("Length: %d\n", length);  // 11
    
    // Manual calculation
    int manual_len = 0;
    while (text[manual_len] != '\0') {
        manual_len++;
    }
    printf("Manual length: %d\n", manual_len);  // 11
    
    return 0;
}
```

#### C++
```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string text = "Hello World";
    
    // Using length() or size() - O(1) time
    cout << "Length: " << text.length() << endl;  // 11
    cout << "Size: " << text.size() << endl;      // 11
    
    // Empty check
    string empty = "";
    cout << "Empty? " << empty.empty() << endl;   // 1 (true)
    
    // C-style string
    char ctext[] = "Hello";
    cout << "C-string length: " << strlen(ctext) << endl;  // 5
    
    return 0;
}
```

#### Java
```java
public class StringLength {
    public static void main(String[] args) {
        String text = "Hello World";
        
        // Using length() - O(1) time
        System.out.println("Length: " + text.length());  // 11
        
        // Empty string
        String empty = "";
        System.out.println("Empty length: " + empty.length());     // 0
        System.out.println("Is empty? " + empty.isEmpty());        // true
        System.out.println("Is blank? " + empty.isBlank());        // true (Java 11+)
        
        // String with spaces
        String spaced = "   ";
        System.out.println("Spaced length: " + spaced.length());   // 3
        System.out.println("Is blank? " + spaced.isBlank());       // true (Java 11+)
    }
}
```

---

### 3. String Indexing and Slicing

#### Python
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

#### C
```c
#include <stdio.h>
#include <string.h>

int main() {
    char s[] = "Python Programming";
    int len = strlen(s);
    
    // Indexing
    printf("First char: %c\n", s[0]);           // 'P'
    printf("Last char: %c\n", s[len-1]);        // 'g'
    printf("Second last: %c\n", s[len-2]);      // 'n'
    
    // Substring (manual)
    char sub[7];
    strncpy(sub, s, 6);      // Copy first 6 chars
    sub[6] = '\0';           // Null terminate
    printf("Substring: %s\n", sub);  // "Python"
    
    // Extract from position
    char *ptr = s + 7;       // Pointer to position 7
    printf("From position 7: %s\n", ptr);  // "Programming"
    
    // Reverse string (in-place)
    char rev[] = "Python";
    int rev_len = strlen(rev);
    for (int i = 0; i < rev_len/2; i++) {
        char temp = rev[i];
        rev[i] = rev[rev_len-1-i];
        rev[rev_len-1-i] = temp;
    }
    printf("Reversed: %s\n", rev);  // "nohtyP"
    
    return 0;
}
```

#### C++
```cpp
#include <iostream>
#include <string>
#include <algorithm>
using namespace std;

int main() {
    string s = "Python Programming";
    
    // Indexing
    cout << "First char: " << s[0] << endl;              // 'P'
    cout << "Last char: " << s[s.length()-1] << endl;    // 'g'
    cout << "At position 7: " << s.at(7) << endl;        // 'P' (with bounds checking)
    
    // Substring: substr(start, length)
    cout << "First 6 chars: " << s.substr(0, 6) << endl;      // "Python"
    cout << "From position 7: " << s.substr(7) << endl;       // "Programming"
    cout << "Last 11 chars: " << s.substr(s.length()-11) << endl;  // "Programming"
    
    // Reverse
    string rev = s;
    reverse(rev.begin(), rev.end());
    cout << "Reversed: " << rev << endl;  // "gnimmargorP nohtyP"
    
    // Extract every 2nd character
    string every2nd = "";
    for (size_t i = 0; i < s.length(); i += 2) {
        every2nd += s[i];
    }
    cout << "Every 2nd char: " << every2nd << endl;  // "Pto rgamn"
    
    return 0;
}
```

#### Java
```java
public class StringIndexing {
    public static void main(String[] args) {
        String s = "Python Programming";
        
        // Indexing
        System.out.println("First char: " + s.charAt(0));              // 'P'
        System.out.println("Last char: " + s.charAt(s.length()-1));    // 'g'
        System.out.println("Second last: " + s.charAt(s.length()-2));  // 'n'
        
        // Substring: substring(start, end)
        System.out.println("First 6 chars: " + s.substring(0, 6));           // "Python"
        System.out.println("From position 7: " + s.substring(7));            // "Programming"
        System.out.println("Position 7-18: " + s.substring(7, 18));          // "Programming"
        System.out.println("Last 11 chars: " + s.substring(s.length()-11));  // "Programming"
        
        // Reverse
        String rev = new StringBuilder(s).reverse().toString();
        System.out.println("Reversed: " + rev);  // "gnimmargorP nohtyP"
        
        // Extract every 2nd character
        StringBuilder every2nd = new StringBuilder();
        for (int i = 0; i < s.length(); i += 2) {
            every2nd.append(s.charAt(i));
        }
        System.out.println("Every 2nd char: " + every2nd);  // "Pto rgamn"
        
        // Get character array
        char[] chars = s.toCharArray();
        System.out.println("First char from array: " + chars[0]);  // 'P'
    }
}
```

#### Indexing Notes
```
String: "Python"
Indices:  0  1  2  3  4  5
         -6 -5 -4 -3 -2 -1  (Python only supports negative indices)

s[1:4]  → s[1], s[2], s[3] → 'yth'
s[-3:]  → s[3], s[4], s[5] → 'hon'
s[::2]  → s[0], s[2], s[4] → 'Pto'
s[::-1] → reverse → 'nohtyP'
```

---

### 4. String Traversal

#### Python
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

#### C
```c
#include <stdio.h>
#include <string.h>

void traverse_string(char *s) {
    int len = strlen(s);
    
    // Method 1: Using index
    printf("Method 1: ");
    for (int i = 0; i < len; i++) {
        printf("%c ", s[i]);
    }
    printf("\n");
    
    // Method 2: Using pointer
    printf("Method 2: ");
    for (char *ptr = s; *ptr != '\0'; ptr++) {
        printf("%c ", *ptr);
    }
    printf("\n");
    
    // Method 3: With index display
    printf("Method 3: ");
    for (int i = 0; s[i] != '\0'; i++) {
        printf("%d:%c ", i, s[i]);
    }
    printf("\n");
}

int main() {
    char text[] = "Hello";
    traverse_string(text);
    return 0;
}
```

#### C++
```cpp
#include <iostream>
#include <string>
using namespace std;

void traverse_string(const string& s) {
    // Method 1: Using index
    cout << "Method 1: ";
    for (size_t i = 0; i < s.length(); i++) {
        cout << s[i] << " ";
    }
    cout << endl;
    
    // Method 2: Range-based for loop
    cout << "Method 2: ";
    for (char c : s) {
        cout << c << " ";
    }
    cout << endl;
    
    // Method 3: Using iterators
    cout << "Method 3: ";
    for (auto it = s.begin(); it != s.end(); ++it) {
        cout << *it << " ";
    }
    cout << endl;
    
    // Method 4: With index display
    cout << "Method 4: ";
    for (size_t i = 0; i < s.length(); i++) {
        cout << i << ":" << s[i] << " ";
    }
    cout << endl;
}

int main() {
    string text = "Hello";
    traverse_string(text);
    return 0;
}
```

#### Java
```java
public class StringTraversal {
    public static void traverseString(String s) {
        // Method 1: Using index
        System.out.print("Method 1: ");
        for (int i = 0; i < s.length(); i++) {
            System.out.print(s.charAt(i) + " ");
        }
        System.out.println();
        
        // Method 2: Using toCharArray()
        System.out.print("Method 2: ");
        for (char c : s.toCharArray()) {
            System.out.print(c + " ");
        }
        System.out.println();
        
        // Method 3: Using chars() stream (Java 8+)
        System.out.print("Method 3: ");
        s.chars().forEach(c -> System.out.print((char)c + " "));
        System.out.println();
        
        // Method 4: With index display
        System.out.print("Method 4: ");
        for (int i = 0; i < s.length(); i++) {
            System.out.print(i + ":" + s.charAt(i) + " ");
        }
        System.out.println();
    }
    
    public static void main(String[] args) {
        String text = "Hello";
        traverseString(text);
    }
}
```

---

## String Methods

### 1. Case Conversion

#### Python
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

#### C
```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>

void to_upper(char *s) {
    for (int i = 0; s[i] != '\0'; i++) {
        s[i] = toupper(s[i]);
    }
}

void to_lower(char *s) {
    for (int i = 0; s[i] != '\0'; i++) {
        s[i] = tolower(s[i]);
    }
}

void capitalize(char *s) {
    if (s[0] != '\0') {
        s[0] = toupper(s[0]);
        for (int i = 1; s[i] != '\0'; i++) {
            s[i] = tolower(s[i]);
        }
    }
}

int is_upper(char *s) {
    for (int i = 0; s[i] != '\0'; i++) {
        if (isalpha(s[i]) && !isupper(s[i])) {
            return 0;
        }
    }
    return 1;
}

int main() {
    char s1[] = "Hello World";
    char s2[] = "Hello World";
    char s3[] = "Hello World";
    
    to_upper(s1);
    printf("Upper: %s\n", s1);  // "HELLO WORLD"
    
    to_lower(s2);
    printf("Lower: %s\n", s2);  // "hello world"
    
    capitalize(s3);
    printf("Capitalize: %s\n", s3);  // "Hello world"
    
    return 0;
}
```

#### C++
```cpp
#include <iostream>
#include <string>
#include <algorithm>
#include <cctype>
using namespace std;

string to_upper(string s) {
    transform(s.begin(), s.end(), s.begin(), ::toupper);
    return s;
}

string to_lower(string s) {
    transform(s.begin(), s.end(), s.begin(), ::tolower);
    return s;
}

string capitalize(string s) {
    if (!s.empty()) {
        s[0] = toupper(s[0]);
        for (size_t i = 1; i < s.length(); i++) {
            s[i] = tolower(s[i]);
        }
    }
    return s;
}

bool is_upper(const string& s) {
    for (char c : s) {
        if (isalpha(c) && !isupper(c)) {
            return false;
        }
    }
    return true;
}

int main() {
    string s = "Hello World";
    
    cout << "Upper: " << to_upper(s) << endl;         // "HELLO WORLD"
    cout << "Lower: " << to_lower(s) << endl;         // "hello world"
    cout << "Capitalize: " << capitalize(s) << endl;  // "Hello world"
    cout << "Is upper? " << is_upper("HELLO") << endl;  // 1 (true)
    
    return 0;
}
```

#### Java
```java
public class CaseConversion {
    public static void main(String[] args) {
        String s = "Hello World";
        
        // Case methods
        System.out.println("Upper: " + s.toUpperCase());       // "HELLO WORLD"
        System.out.println("Lower: " + s.toLowerCase());       // "hello world"
        
        // Capitalize first letter
        String capitalized = s.substring(0, 1).toUpperCase() + 
                           s.substring(1).toLowerCase();
        System.out.println("Capitalize: " + capitalized);      // "Hello world"
        
        // Title case (capitalize each word)
        String[] words = s.split(" ");
        StringBuilder titleCase = new StringBuilder();
        for (String word : words) {
            if (word.length() > 0) {
                titleCase.append(Character.toUpperCase(word.charAt(0)))
                        .append(word.substring(1).toLowerCase())
                        .append(" ");
            }
        }
        System.out.println("Title: " + titleCase.toString().trim());  // "Hello World"
        
        // Check case
        System.out.println("Is upper? " + s.equals(s.toUpperCase()));  // false
        System.out.println("Is lower? " + s.equals(s.toLowerCase()));  // false
        
        // Normalize name
        String name = "  john doe  ";
        String normalized = Arrays.stream(name.trim().split(" "))
            .map(w -> w.substring(0, 1).toUpperCase() + w.substring(1).toLowerCase())
            .collect(Collectors.joining(" "));
        System.out.println("Normalized: " + normalized);  // "John Doe"
    }
}
```

---

### 2. Search Methods

#### Python
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

#### C
```c
#include <stdio.h>
#include <string.h>

int find(const char *str, const char *sub) {
    char *pos = strstr(str, sub);
    return (pos != NULL) ? (pos - str) : -1;
}

int count_occurrences(const char *str, const char *sub) {
    int count = 0;
    int sub_len = strlen(sub);
    const char *pos = str;
    
    while ((pos = strstr(pos, sub)) != NULL) {
        count++;
        pos += sub_len;
    }
    return count;
}

int starts_with(const char *str, const char *prefix) {
    return strncmp(str, prefix, strlen(prefix)) == 0;
}

int ends_with(const char *str, const char *suffix) {
    int str_len = strlen(str);
    int suffix_len = strlen(suffix);
    
    if (suffix_len > str_len) return 0;
    return strcmp(str + str_len - suffix_len, suffix) == 0;
}

int main() {
    char s[] = "Hello World Hello";
    
    printf("Find 'World': %d\n", find(s, "World"));      // 6
    printf("Find 'Python': %d\n", find(s, "Python"));    // -1
    printf("Count 'Hello': %d\n", count_occurrences(s, "Hello"));  // 2
    printf("Starts with 'Hello': %d\n", starts_with(s, "Hello"));  // 1
    printf("Ends with 'Hello': %d\n", ends_with(s, "Hello"));      // 1
    
    return 0;
}
```

#### C++
```cpp
#include <iostream>
#include <string>
#include <algorithm>
using namespace std;

int main() {
    string s = "Hello World Hello";
    
    // Find (returns position or string::npos)
    size_t pos = s.find("World");
    cout << "Find 'World': " << pos << endl;  // 6
    
    pos = s.find("Python");
    if (pos == string::npos) {
        cout << "Find 'Python': -1" << endl;  // -1
    }
    
    // Find from position
    pos = s.find("Hello", 1);
    cout << "Find 'Hello' from 1: " << pos << endl;  // 12
    
    // Count occurrences
    int count = 0;
    size_t search_pos = 0;
    while ((search_pos = s.find("Hello", search_pos)) != string::npos) {
        count++;
        search_pos++;
    }
    cout << "Count 'Hello': " << count << endl;  // 2
    
    // Check start/end
    bool starts = (s.rfind("Hello", 0) == 0);
    cout << "Starts with 'Hello': " << starts << endl;  // 1
    
    string suffix = "Hello";
    bool ends = (s.length() >= suffix.length() && 
                 s.compare(s.length() - suffix.length(), suffix.length(), suffix) == 0);
    cout << "Ends with 'Hello': " << ends << endl;  // 1
    
    return 0;
}
```

#### Java
```java
public class SearchMethods {
    public static void main(String[] args) {
        String s = "Hello World Hello";
        
        // Find (returns index or -1)
        System.out.println("Find 'World': " + s.indexOf("World"));      // 6
        System.out.println("Find 'Python': " + s.indexOf("Python"));    // -1
        System.out.println("Find 'Hello' from 1: " + s.indexOf("Hello", 1));  // 12
        
        // Last occurrence
        System.out.println("Last 'Hello': " + s.lastIndexOf("Hello"));  // 12
        
        // Count occurrences
        int count = 0;
        int index = 0;
        while ((index = s.indexOf("Hello", index)) != -1) {
            count++;
            index++;
        }
        System.out.println("Count 'Hello': " + count);  // 2
        
        // Check start/end
        System.out.println("Starts with 'Hello': " + s.startsWith("Hello"));  // true
        System.out.println("Ends with 'Hello': " + s.endsWith("Hello"));      // true
        System.out.println("Starts with 'World': " + s.startsWith("World"));  // false
        
        // Contains
        System.out.println("Contains 'World': " + s.contains("World"));  // true
    }
}
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

#### Python
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

#### C
```c
#include <stdio.h>
#include <string.h>

void split_example() {
    // Split using strtok (modifies original string)
    char s[] = "apple,banana,cherry";
    char *token;
    
    token = strtok(s, ",");
    while (token != NULL) {
        printf("%s\n", token);
        token = strtok(NULL, ",");
    }
    // Output: apple, banana, cherry
}

void join_example() {
    // Join strings
    char result[100] = "";
    char *words[] = {"Hello", "World", "C"};
    int count = 3;
    
    for (int i = 0; i < count; i++) {
        strcat(result, words[i]);
        if (i < count - 1) {
            strcat(result, " ");
        }
    }
    printf("%s\n", result);  // "Hello World C"
}

int main() {
    split_example();
    join_example();
    return 0;
}
```

#### C++
```cpp
#include <iostream>
#include <string>
#include <vector>
#include <sstream>
using namespace std;

vector<string> split(const string& s, char delimiter) {
    vector<string> tokens;
    stringstream ss(s);
    string token;
    
    while (getline(ss, token, delimiter)) {
        tokens.push_back(token);
    }
    return tokens;
}

string join(const vector<string>& tokens, const string& delimiter) {
    string result;
    for (size_t i = 0; i < tokens.size(); i++) {
        result += tokens[i];
        if (i < tokens.size() - 1) {
            result += delimiter;
        }
    }
    return result;
}

int main() {
    // Split
    string s = "apple,banana,cherry";
    vector<string> fruits = split(s, ',');
    for (const auto& fruit : fruits) {
        cout << fruit << endl;  // apple, banana, cherry
    }
    
    // Join
    vector<string> words = {"Hello", "World", "C++"};
    string sentence = join(words, " ");
    cout << sentence << endl;  // "Hello World C++"
    
    return 0;
}
```

#### Java
```java
import java.util.Arrays;
import java.util.stream.Collectors;

public class SplitJoin {
    public static void main(String[] args) {
        // Split
        String s = "apple,banana,cherry";
        String[] fruits = s.split(",");
        System.out.println(Arrays.toString(fruits));  // [apple, banana, cherry]
        
        // Split by whitespace
        String text = "Hello  World  Python";
        String[] words = text.split("\\s+");  // Split by one or more spaces
        System.out.println(Arrays.toString(words));  // [Hello, World, Python]
        
        // Split with limit
        String data = "a:b:c:d:e";
        String[] parts = data.split(":", 3);  // Split into at most 3 parts
        System.out.println(Arrays.toString(parts));  // [a, b, c:d:e]
        
        // Split lines
        String multiline = "Line1\nLine2\nLine3";
        String[] lines = multiline.split("\n");
        System.out.println(Arrays.toString(lines));  // [Line1, Line2, Line3]
        
        // Join
        String[] wordsArray = {"Hello", "World"};
        String sentence = String.join(" ", wordsArray);
        System.out.println(sentence);  // "Hello World"
        
        // Join with stream (Java 8+)
        String joined = Arrays.stream(wordsArray)
                             .collect(Collectors.joining(" "));
        System.out.println(joined);  // "Hello World"
        
        // Join numbers
        int[] numbers = {1, 2, 3};
        String result = Arrays.stream(numbers)
                             .mapToObj(String::valueOf)
                             .collect(Collectors.joining(", "));
        System.out.println(result);  // "1, 2, 3"
    }
}
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

#### Python
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

#### C
```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>

int compare_ignore_case(const char *s1, const char *s2) {
    while (*s1 && *s2) {
        if (tolower(*s1) != tolower(*s2)) {
            return tolower(*s1) - tolower(*s2);
        }
        s1++;
        s2++;
    }
    return tolower(*s1) - tolower(*s2);
}

int main() {
    // Equality
    char s1[] = "hello";
    char s2[] = "hello";
    char s3[] = "Hello";
    
    printf("s1 == s2: %d\n", strcmp(s1, s2) == 0);  // 1 (true)
    printf("s1 == s3: %d\n", strcmp(s1, s3) == 0);  // 0 (false)
    
    // Lexicographic comparison
    printf("apple < banana: %d\n", strcmp("apple", "banana") < 0);  // 1
    
    // Case-insensitive
    printf("hello == Hello: %d\n", compare_ignore_case("hello", "Hello") == 0);  // 1
    
    return 0;
}
```

#### C++
```cpp
#include <iostream>
#include <string>
#include <algorithm>
using namespace std;

bool compare_ignore_case(const string& s1, const string& s2) {
    if (s1.length() != s2.length()) return false;
    
    return equal(s1.begin(), s1.end(), s2.begin(),
                [](char a, char b) {
                    return tolower(a) == tolower(b);
                });
}

int main() {
    // Equality
    cout << ("hello" == "hello") << endl;    // 1 (true)
    cout << ("hello" == "Hello") << endl;    // 0 (false)
    
    // Lexicographic comparison
    cout << ("apple" < "banana") << endl;    // 1
    cout << ("abc" < "abd") << endl;         // 1
    
    // Case-insensitive
    cout << compare_ignore_case("hello", "Hello") << endl;  // 1
    
    // Using compare()
    string s1 = "hello";
    string s2 = "hello";
    cout << (s1.compare(s2) == 0) << endl;   // 1 (equal)
    
    return 0;
}
```

#### Java
```java
public class StringComparison {
    public static void main(String[] args) {
        // Equality
        System.out.println("hello".equals("hello"));      // true
        System.out.println("hello".equals("Hello"));      // false
        
        // Note: Don't use == for string comparison in Java
        // == compares references, not content
        String s1 = new String("hello");
        String s2 = new String("hello");
        System.out.println(s1 == s2);                     // false (different objects)
        System.out.println(s1.equals(s2));                // true (same content)
        
        // Lexicographic comparison
        System.out.println("apple".compareTo("banana") < 0);  // true
        System.out.println("abc".compareTo("abd") < 0);       // true
        
        // Case-insensitive comparison
        System.out.println("hello".equalsIgnoreCase("Hello"));  // true
        System.out.println("hello".compareToIgnoreCase("Hello") == 0);  // true
        
        // Compare ignoring whitespace
        String str1 = "hello world";
        String str2 = "helloworld";
        System.out.println(str1.replace(" ", "").equals(str2));  // true
    }
}
```

---

## Common String Problems

### 1. Reverse String

#### Python
```python
def reverse_string(s):
    """
    Reverse string
    Time: O(n)
    Space: O(n)
    """
    # Method 1: Slicing (fastest)
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

print(reverse_string("hello"))  # "olleh"
```

#### C
```c
#include <stdio.h>
#include <string.h>

void reverse_string(char *s) {
    int left = 0;
    int right = strlen(s) - 1;
    
    while (left < right) {
        // Swap characters
        char temp = s[left];
        s[left] = s[right];
        s[right] = temp;
        
        left++;
        right--;
    }
}

int main() {
    char s[] = "hello";
    printf("Original: %s\n", s);
    
    reverse_string(s);
    printf("Reversed: %s\n", s);  // "olleh"
    
    return 0;
}
```

#### C++
```cpp
#include <iostream>
#include <string>
#include <algorithm>
using namespace std;

string reverse_string(string s) {
    // Method 1: Using reverse()
    reverse(s.begin(), s.end());
    return s;
}

string reverse_string_manual(string s) {
    // Method 2: Two pointers
    int left = 0;
    int right = s.length() - 1;
    
    while (left < right) {
        swap(s[left], s[right]);
        left++;
        right--;
    }
    return s;
}

int main() {
    string s = "hello";
    cout << "Original: " << s << endl;
    cout << "Reversed: " << reverse_string(s) << endl;  // "olleh"
    
    return 0;
}
```

#### Java
```java
public class ReverseString {
    // Method 1: Using StringBuilder
    public static String reverseString(String s) {
        return new StringBuilder(s).reverse().toString();
    }
    
    // Method 2: Two pointers (convert to char array)
    public static String reverseStringManual(String s) {
        char[] chars = s.toCharArray();
        int left = 0;
        int right = chars.length - 1;
        
        while (left < right) {
            // Swap characters
            char temp = chars[left];
            chars[left] = chars[right];
            chars[right] = temp;
            
            left++;
            right--;
        }
        
        return new String(chars);
    }
    
    // Method 3: Using Stream (Java 8+)
    public static String reverseStringStream(String s) {
        return s.chars()
                .mapToObj(c -> (char) c)
                .reduce("", (acc, c) -> c + acc, (s1, s2) -> s2 + s1);
    }
    
    public static void main(String[] args) {
        String s = "hello";
        System.out.println("Original: " + s);
        System.out.println("Reversed: " + reverseString(s));  // "olleh"
    }
}
```

---

### 2. Palindrome Check

#### Python
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

#### C
```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>

int is_palindrome(const char *s) {
    int left = 0;
    int right = strlen(s) - 1;
    
    while (left < right) {
        if (s[left] != s[right]) {
            return 0;  // false
        }
        left++;
        right--;
    }
    
    return 1;  // true
}

int is_palindrome_alphanumeric(const char *s) {
    int left = 0;
    int right = strlen(s) - 1;
    
    while (left < right) {
        // Skip non-alphanumeric from left
        while (left < right && !isalnum(s[left])) {
            left++;
        }
        // Skip non-alphanumeric from right
        while (left < right && !isalnum(s[right])) {
            right--;
        }
        
        // Compare (case-insensitive)
        if (tolower(s[left]) != tolower(s[right])) {
            return 0;
        }
        left++;
        right--;
    }
    
    return 1;
}

int main() {
    printf("Is 'racecar' palindrome? %d\n", is_palindrome("racecar"));  // 1
    printf("Is 'hello' palindrome? %d\n", is_palindrome("hello"));      // 0
    printf("Is 'A man, a plan, a canal: Panama' palindrome? %d\n", 
           is_palindrome_alphanumeric("A man, a plan, a canal: Panama"));  // 1
    
    return 0;
}
```

#### C++
```cpp
#include <iostream>
#include <string>
#include <algorithm>
#include <cctype>
using namespace std;

bool is_palindrome(const string& s) {
    int left = 0;
    int right = s.length() - 1;
    
    while (left < right) {
        if (s[left] != s[right]) {
            return false;
        }
        left++;
        right--;
    }
    
    return true;
}

bool is_palindrome_alphanumeric(const string& s) {
    int left = 0;
    int right = s.length() - 1;
    
    while (left < right) {
        // Skip non-alphanumeric from left
        while (left < right && !isalnum(s[left])) {
            left++;
        }
        // Skip non-alphanumeric from right
        while (left < right && !isalnum(s[right])) {
            right--;
        }
        
        // Compare (case-insensitive)
        if (tolower(s[left]) != tolower(s[right])) {
            return false;
        }
        left++;
        right--;
    }
    
    return true;
}

int main() {
    cout << "Is 'racecar' palindrome? " << is_palindrome("racecar") << endl;  // 1
    cout << "Is 'hello' palindrome? " << is_palindrome("hello") << endl;      // 0
    cout << "Is 'A man, a plan, a canal: Panama' palindrome? " 
         << is_palindrome_alphanumeric("A man, a plan, a canal: Panama") << endl;  // 1
    
    return 0;
}
```

#### Java
```java
public class PalindromeCheck {
    public static boolean isPalindrome(String s) {
        int left = 0;
        int right = s.length() - 1;
        
        while (left < right) {
            if (s.charAt(left) != s.charAt(right)) {
                return false;
            }
            left++;
            right--;
        }
        
        return true;
    }
    
    public static boolean isPalindromeAlphanumeric(String s) {
        int left = 0;
        int right = s.length() - 1;
        
        while (left < right) {
            // Skip non-alphanumeric from left
            while (left < right && !Character.isLetterOrDigit(s.charAt(left))) {
                left++;
            }
            // Skip non-alphanumeric from right
            while (left < right && !Character.isLetterOrDigit(s.charAt(right))) {
                right--;
            }
            
            // Compare (case-insensitive)
            if (Character.toLowerCase(s.charAt(left)) != 
                Character.toLowerCase(s.charAt(right))) {
                return false;
            }
            left++;
            right--;
        }
        
        return true;
    }
    
    public static void main(String[] args) {
        System.out.println("Is 'racecar' palindrome? " + isPalindrome("racecar"));  // true
        System.out.println("Is 'hello' palindrome? " + isPalindrome("hello"));      // false
        System.out.println("Is 'A man, a plan, a canal: Panama' palindrome? " + 
                          isPalindromeAlphanumeric("A man, a plan, a canal: Panama"));  // true
    }
}
```

---

### 3. Anagram Check

#### Python
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

#### C
```c
#include <stdio.h>
#include <string.h>
#include <stdbool.h>

bool is_anagram(const char *s1, const char *s2) {
    if (strlen(s1) != strlen(s2)) {
        return false;
    }
    
    // Count character frequencies
    int count[256] = {0};  // ASCII characters
    
    for (int i = 0; s1[i] != '\0'; i++) {
        count[(unsigned char)s1[i]]++;
        count[(unsigned char)s2[i]]--;
    }
    
    // Check if all counts are zero
    for (int i = 0; i < 256; i++) {
        if (count[i] != 0) {
            return false;
        }
    }
    
    return true;
}

int main() {
    printf("Is 'listen' and 'silent' anagram? %d\n", 
           is_anagram("listen", "silent"));  // 1
    printf("Is 'hello' and 'world' anagram? %d\n", 
           is_anagram("hello", "world"));    // 0
    
    return 0;
}
```

#### C++
```cpp
#include <iostream>
#include <string>
#include <algorithm>
#include <unordered_map>
using namespace std;

bool is_anagram(const string& s1, const string& s2) {
    if (s1.length() != s2.length()) {
        return false;
    }
    
    unordered_map<char, int> count;
    
    for (char c : s1) {
        count[c]++;
    }
    
    for (char c : s2) {
        count[c]--;
        if (count[c] < 0) {
            return false;
        }
    }
    
    for (auto& pair : count) {
        if (pair.second != 0) {
            return false;
        }
    }
    
    return true;
}

bool is_anagram_sort(string s1, string s2) {
    sort(s1.begin(), s1.end());
    sort(s2.begin(), s2.end());
    return s1 == s2;
}

int main() {
    cout << "Is 'listen' and 'silent' anagram? " << is_anagram("listen", "silent") << endl;  // 1
    cout << "Is 'hello' and 'world' anagram? " << is_anagram("hello", "world") << endl;      // 0
    
    return 0;
}
```

#### Java
```java
import java.util.Arrays;
import java.util.HashMap;
import java.util.Map;

public class AnagramCheck {
    // Method 1: Using HashMap
    public static boolean isAnagram(String s1, String s2) {
        if (s1.length() != s2.length()) {
            return false;
        }
        
        Map<Character, Integer> count = new HashMap<>();
        
        // Count characters in s1
        for (char c : s1.toCharArray()) {
            count.put(c, count.getOrDefault(c, 0) + 1);
        }
        
        // Decrement counts for s2
        for (char c : s2.toCharArray()) {
            count.put(c, count.getOrDefault(c, 0) - 1);
            if (count.get(c) < 0) {
                return false;
            }
        }
        
        return true;
    }
    
    // Method 2: Using sorting
    public static boolean isAnagramSort(String s1, String s2) {
        if (s1.length() != s2.length()) {
            return false;
        }
        
        char[] arr1 = s1.toCharArray();
        char[] arr2 = s2.toCharArray();
        
        Arrays.sort(arr1);
        Arrays.sort(arr2);
        
        return Arrays.equals(arr1, arr2);
    }
    
    public static void main(String[] args) {
        System.out.println("Is 'listen' and 'silent' anagram? " + 
                          isAnagram("listen", "silent"));  // true
        System.out.println("Is 'hello' and 'world' anagram? " + 
                          isAnagram("hello", "world"));    // false
    }
}
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
