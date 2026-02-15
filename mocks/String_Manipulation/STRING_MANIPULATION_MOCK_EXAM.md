# STRING MANIPULATION MOCK EXAM
**GATE-Level SEBI Exam | Total: 75 Questions | Weightage: 10% | Time: 1.5 Hours**

---

## SECTION A: STRING BASICS (25 Questions)

**Q1.** String length in C for "Hello" (including '\0')?  
A) 4 | B) 5 | C) 6 | D) Depends  
**Answer: C** | 5 characters + 1 null terminator = 6 bytes

**Q2.** Python string: `s = "abc"; s[0] = 'd'` causes?  
A) s = "dbc" | B) Error (immutable) | C) Warning | D) s = "d"  
**Answer: B** | Strings immutable in Python/Java

**Q3.** Output?  
```c
char s1[] = "Hello";
char s2[] = "Hello";
printf("%d", s1 == s2);
```
A) 0 | B) 1 | C) Error | D) Undefined  
**Answer: A** | Compares addresses, not content (use strcmp)

**Q4.** Java `String s1 = "abc"; String s2 = "abc";` then `s1 == s2`?  
A) true (string pool) | B) false | C) Error | D) Depends  
**Answer: A** | String literals use same pool reference

**Q5.** String concatenation in loop: `s = ""; for(i=0; i<n; i++) s += "a";` time?  
A) O(n) | B) O(n²) | C) O(n log n) | D) O(1)  
**Answer: B** | Each concat creates new string: O(n²)

**Q6.** Output?  
```python
s = "hello"
print(s[1:4])
```
A) "hel" | B) "ell" | C) "ello" | D) "llo"  
**Answer: B** | Python slice [1:4] = indices 1,2,3 = "ell"

**Q7.** C++ `string s = "abc"; s.length()` vs `s.size()`?  
A) Different | B) Same ✓ | C) length() includes '\0' | D) Error  
**Answer: B** | Both return same value (character count)

**Q8.** `strcmp("abc", "abd")` returns?  
A) Negative | B) 0 | C) Positive | D) 1  
**Answer: A** | 'c' < 'd', returns negative

**Q9.** Output?  
```java
String s = "Java";
s.concat(" Programming");
System.out.println(s);
```
A) "Java Programming" | B) "Java" | C) Error | D) null  
**Answer: B** | concat() returns new string, doesn't modify original

**Q10.** Reverse string "ABC" in-place?  
A) Swap first-last, move inward | B) Stack | C) Recursion | D) All work  
**Answer: D** | Multiple approaches possible

**Q11.** Check palindrome "racecar": approach?  
A) Two pointers | B) Reverse and compare | C) Both A & B | D) Regex  
**Answer: C** | Either approach works

**Q12.** Output?  
```cpp
string s = "hello";
cout << s.substr(1, 3);
```
A) "hel" | B) "ell" ✓ | C) "llo" | D) "lo"  
**Answer: B** | substr(start, length): from index 1, length 3 = "ell"

**Q13.** Anagram check: "listen" and "silent"?  
A) Sort both, compare | B) Frequency count | C) Both O(n log n) | D) A=O(n log n), B=O(n)  
**Answer: D** | Sort: O(n log n), Frequency map: O(n)

**Q14.** String contains only digits: approach?  
A) Check each char with isdigit() | B) Regex `^\d+$` | C) Both work | D) atoi()  
**Answer: C** | Either validation method works

**Q15.** Output?  
```python
s = "a b c"
print(len(s.split()))
```
A) 1 | B) 3 ✓ | C) 5 | D) 7  
**Answer: B** | split() on spaces: ["a", "b", "c"] = 3 elements

**Q16.** Convert "ABC" to lowercase using ASCII?  
A) Add 32 to each char | B) Subtract 32 | C) Depends on char | D) Use library  
**Answer: A** | 'A'=65, 'a'=97; difference = 32

**Q17.** Count vowels in "Education": how many?  
A) 3 | B) 4 | C) 5 ✓ | D) 6  
**Answer: C** | e, u, a, i, o = 5

**Q18.** Output?  
```java
String s = "  hello  ";
System.out.println(s.trim().length());
```
A) 5 ✓ | B) 7 | C) 9 | D) 11  
**Answer: A** | trim() removes spaces: "hello" = 5

**Q19.** Find first non-repeating char in "swiss"?  
A) 's' | B) 'w' ✓ | C) 'i' | D) None  
**Answer: B** | Frequency: s(3), w(1), i(1), w appears first unique

**Q20.** String rotation: is "waterbottle" rotation of "erbottlewat"?  
A) Yes ✓ | B) No  
**Answer: A** | Check if s1 is substring of s2+s2

**Q21.** Output?  
```c
char s[] = "12345";
int n = atoi(s);
printf("%d", n + 5);
```
A) 12345 | B) 12350 ✓ | C) Error | D) 67890  
**Answer: B** | 12345 + 5 = 12350

**Q22.** Remove duplicates from "banana" keeping order?  
A) "ban" ✓ | B) "bana" | C) "bn" | D) "banan"  
**Answer: A** | First occurrence: b, a, n

**Q23.** Longest common prefix of ["flower","flow","flight"]?  
A) "f" | B) "fl" ✓ | C) "flo" | D) "flow"  
**Answer: B** | Common: "fl"

**Q24.** Output?  
```python
s = "Python"
print(s[-1])
```
A) 'P' | B) 'n' ✓ | C) Error | D) 'o'  
**Answer: B** | Negative index: -1 = last char = 'n'

**Q25.** Convert string to integer "123abc"?  
A) 123 | B) Error | C) 0 | D) Depends on language  
**Answer: D** | C atoi()=123, Java parseInt() throws exception

---

## SECTION B: PATTERN MATCHING (20 Questions)

**Q26.** KMP: LPS array for "AAAA"?  
A) [0,1,2,3] ✓ | B) [0,0,0,0] | C) [1,2,3,4] | D) [0,1,1,1]  
**Answer: A** | Longest proper prefix = suffix

**Q27.** Naive pattern search "abc" in "ababcabc": comparisons?  
A) 9 | B) 11 | C) 15 | D) Depends on implementation  
**Answer: C** | Each mismatch slides by 1

**Q28.** Rabin-Karp: hash collision means?  
A) Match found | B) Maybe match, verify ✓ | C) No match | D) Error  
**Answer: B** | Collision requires character-by-character verification

**Q29.** Output?  
```cpp
string text = "AABAACAADAABAABA";
string pattern = "AABA";
// KMP finds pattern at indices?
```
A) 0, 9, 12 ✓ | B) 0, 9 | C) 0, 12 | D) 9, 12  
**Answer: A** | Pattern at indices 0, 9, 12

**Q30.** Boyer-Moore: best case for "PATTERN" in 1000-char text?  
A) O(n/m) ✓ | B) O(n) | C) O(nm) | D) O(n²)  
**Answer: A** | Can skip characters: O(n/m) best case

**Q31.** Z-Algorithm: Z[i] represents?  
A) Frequency | B) Length of substring from i matching prefix ✓ | C) Index | D) Hash  
**Answer: B** | Z[i] = longest match with prefix starting at i

**Q32.** Aho-Corasick: used for?  
A) Single pattern | B) Multiple patterns simultaneously ✓ | C) Approximate match | D) Compression  
**Answer: B** | Trie + failure links for multiple patterns

**Q33.** Output?  
```python
import re
text = "My number is 123-456-7890"
match = re.search(r'\d+-\d+-\d+', text)
print("Found" if match else "Not Found")
```
A) Found ✓ | B) Not Found | C) Error | D) None  
**Answer: A** | Regex matches phone pattern

**Q34.** KMP time complexity (text=n, pattern=m)?  
A) O(n+m) ✓ | B) O(nm) | C) O(n²) | D) O(m²)  
**Answer: A** | Preprocess m + Search n = O(n+m)

**Q35.** Finite Automata pattern matching: preprocessing time?  
A) O(m) | B) O(m²) | C) O(m×σ) ✓ | D) O(n)  
**Answer: C** | Build DFA: O(m × alphabet_size)

**Q36.** Manacher's Algorithm finds?  
A) All palindromes | B) Longest palindromic substring ✓ in O(n) | C) KMP | D) Anagrams  
**Answer: B** | Linear time longest palindrome

**Q37.** Count occurrences of "aa" in "aaaa"?  
A) 1 | B) 2 | C) 3 ✓ | D) 4  
**Answer: C** | Positions: (0,1), (1,2), (2,3) = 3 overlapping

**Q38.** Output?  
```java
String s = "abcabcabc";
System.out.println(s.indexOf("abc", 4));
```
A) 3 | B) 6 ✓ | C) 0 | D) -1  
**Answer: B** | Search from index 4 onwards: finds at 6

**Q39.** Wildcard matching: "*" matches?  
A) Single char | B) Zero or more chars ✓ | C) Digit | D) Alphanumeric  
**Answer: B** | * matches any sequence (including empty)

**Q40.** Edit distance: "horse" to "ros"?  
A) 2 | B) 3 ✓ | C) 4 | D) 5  
**Answer: B** | Replace h→r, delete r, delete e = 3 operations

**Q41.** Rolling hash formula (base d, mod q)?  
A) `(d × (hash - char[i]×h) + char[i+m]) % q` ✓ | B) `hash / d` | C) `hash + char` | D) `hash × char`  
**Answer: A** | Remove first, add next character

**Q42.** Output?  
```cpp
string s = "abcdefgh";
cout << s.find("def");
```
A) 2 | B) 3 ✓ | C) -1 | D) 0  
**Answer: B** | "def" starts at index 3

**Q43.** Suffix array for "banana": sorted suffixes?  
A) [5,3,1,0,4,2] ✓ | B) [0,1,2,3,4,5] | C) [2,4,0,5,3,1] | D) Random  
**Answer: A** | Suffixes: "a"(5), "ana"(3), "anana"(1), "banana"(0), "na"(4), "nana"(2)

**Q44.** Check if strings are rotations: "ABCD" and "CDAB"?  
A) Yes ✓ (if "ABCD" in "CDABCDAB") | B) No | C) Need KMP | D) Impossible  
**Answer: A** | s1 in s2+s2 checks rotation

**Q45.** Output?  
```python
s = "hello world"
```print(s.count('l'))
```
A) 2 | B) 3 ✓ | C) 4 | D) 1  
**Answer: B** | 'l' appears 3 times

---

## SECTION C: REGEX & ADVANCED (15 Questions)

**Q46.** Regex `\d+` matches?  
A) One or more digits ✓ | B) One digit | C) Zero or more | D) Non-digit  
**Answer: A** | \d=digit, +=one or more

**Q47.** Regex `^abc$` matches?  
A) Contains "abc" | B) Starts with "abc" | C) Ends with "abc" | D) Exactly "abc" ✓  
**Answer: D** | ^=start, $=end, exactly "abc"

**Q48.** `[a-zA-Z]+` matches?  
A) Any letter one or more times ✓ | B) Numbers | C) Special chars | D) Spaces  
**Answer: A** | Character class + one or more

**Q49.** Output?  
```java
String s = "a1b2c3";
String result = s.replaceAll("\\d", "");
System.out.println(result);
```
A) "a1b2c3" | B) "abc" ✓ | C) "123" | D) ""  
**Answer: B** | Removes all digits, leaves "abc"

**Q50.** Greedy vs Non-greedy quantifiers: `*` vs `*?`?  
A) * matches max, *? matches min ✓ | B) Same | C) * faster | D) *? error  
**Answer: A** | Greedy max, non-greedy min

**Q51.** Regex email validation: `[a-z]+@[a-z]+\.[a-z]+`?  
A) Valid basic pattern ✓ | B) Perfect | C) Too restrictive | D) Wrong  
**Answer: A/C** | Basic but misses numbers, dots in name, etc.

**Q52.** Backreference `(\w)\1` matches?  
A) Two same consecutive chars ✓ | B) Two chars | C) Word twice | D) Error  
**Answer: A** | Captures char, \1 refers to same char

**Q53.** Output?  
```python
import re
s = "Color: red, colour: blue"
result = re.findall(r'colou?r', s, re.I)
print(len(result))
```
A) 0 | B) 1 | C) 2 ✓ | D) 3  
**Answer: C** | Matches both "Color" and "colour" (case-insensitive)

**Q54.** Lookahead `abc(?=def)` matches?  
A) "abc" only if followed by "def" ✓ | B) "abcdef" | C) "abc" or "def" | D) Error  
**Answer: A** | Positive lookahead, doesn't consume "def"

**Q55.** Run-length encoding "aaabbcccc"?  
A) "a3b2c4" ✓ | B) "3a2b4c" | C) "aaabbcccc" | D) "abc"  
**Answer: A** | Encode consecutive: char+count

**Q56.** Decode run-length "a3b2"?  
A) "aaabb" ✓ | B) "a3b2" | C) "ab5" | D) Error  
**Answer: A** | Expand: aaa, bb

**Q57.** Output?  
```cpp
string s = "abcabc";
cout << s.rfind("abc");
```
A) 0 | B) 3 ✓ | C) 6 | D) -1  
**Answer: B** | rfind() finds last occurrence at index 3

**Q58.** String compression worth it? "aabcccccaaa" compressed to "a2b1c5a3"?  
A) Yes (11 vs 10) | B) No (same) | C) Yes (shorter) ✓ | D) Depends  
**Answer: C** | Original 11, compressed 10 (shorter)

**Q59.** Longest substring without repeating chars in "abcabcbb"?  
A) 2 | B) 3 ✓ | C) 4 | D) 8  
**Answer: B** | "abc" length 3

**Q60.** Output?  
```python
s = "Level"
print(s.lower() == s.lower()[::-1])
```
A) True ✓ | B) False | C) Error | D) None  
**Answer: A** | "level" == "level" (palindrome)

---

## SECTION D: STRING TRANSFORMATIONS (15 Questions)

**Q61.** Longest Common Subsequence: "ABCDGH" and "AEDFHR"?  
A) "AD" | B) "ADH" ✓ | C) "ABDH" | D) "ACDH"  
**Answer: B** | LCS = "ADH" (length 3)

**Q62.** Longest Common Substring: "ABCDGH" and "ACDGHR"?  
A) "CDG" | B) "CDGH" ✓ | C) "DGH" | D) "AD"  
**Answer: B** | Continuous: "CDGH" length 4

**Q63.** Minimum window substring: "ADOBECODEBANC" contains "ABC"?  
A) "ABC" | B) "BANC" ✓ | C) "ADOBEC" | D) "BECODEBA"  
**Answer: B** | Smallest window: "BANC"

**Q64.** Output?  
```java
String s1 = "abc", s2 = "def";
String s3 = s1 + s2;
System.out.println(s3 == "abcdef");
```
A) true | B) false ✓ | C) Error | D) Depends  
**Answer: B** | + creates new object, not from pool

**Q65.** Permutations of "ABC": count?  
A) 3 | B) 4 | C) 5 | D) 6 ✓  
**Answer: D** | 3! = 6

**Q66.** Check if s2 is scrambled string of s1: "great" and "rgeat"?  
A) Yes ✓ | B) No | C) Need rules | D) Impossible  
**Answer: A** | Can be obtained by swapping

**Q67.** Interleaving strings: "aab", "axy" can form "aaxaby"?  
A) Yes ✓ | B) No  
**Answer: A** | a(aab) a(axy) x(axy) a(aab) b(aab) y(axy)

**Q68.** Output?  
```cpp
string s = "Hello";
transform(s.begin(), s.end(), s.begin(), ::tolower);
cout << s;
```
A) "Hello" | B) "hello" ✓ | C) "HELLO" | D) Error  
**Answer: B** | Converts to lowercase

**Q69.** Distinct subsequences: "rabbbit" contains how many "rabbit"?  
A) 1 | B) 2 | C) 3 ✓ | D) 4  
**Answer: C** | 3 ways to form "rabbit"

**Q70.** String to integer with overflow check: "2147483648" (INT_MAX+1)?  
A) 2147483647 | B) -2147483648 | C) Overflow error ✓ | D) 2147483648  
**Answer: C** | Exceeds INT_MAX, handle overflow

**Q71.** Output?  
```python
s = "  a b  c  "
print(''.join(s.split()))
```
A) "a b c" | B) "abc" ✓ | C) " abc " | D) "  a b  c  "  
**Answer: B** | split() removes all spaces, join() with empty = "abc"

**Q72.** Repeat string "abc" 3 times?  
A) "abcabcabc" ✓ | B) "abc3" | C) "aaa" | D) Error  
**Answer: A** | Concatenate 3 times

**Q73.** Compare versions "1.0.1" and "1.0"?  
A) 1.0.1 > 1.0 ✓ | B) Equal | C) 1.0 > 1.0.1 | D) Error  
**Answer: A** | Parse and compare parts: 1=1, 0=0, 1>0

**Q74.** Output?  
```c
char s1[] = "Hello";
char s2[10];
strcpy(s2, s1);
strcat(s2, " World");
printf("%s", s2);
```
A) "Hello" | B) "Hello World" ✓ | C) Error | D) "World"  
**Answer: B** | Copy then concatenate

**Q75.** Convert Roman "IX" to integer?  
A) 6 | B) 9 ✓ | C) 11 | D) 19  
**Answer: B** | I before X means subtract: 10-1 = 9

---

## END OF EXAM

**Review answers carefully. Time management: 75 questions in 90 minutes ≈ 72 seconds/question**

**(Each question: 1 mark | Total: 75 marks)**
