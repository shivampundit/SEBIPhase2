# Hashing - Comprehensive Notes

## Overview
Hashing is a technique to store and retrieve data in O(1) average time using a hash function that maps keys to array indices.

### Key Concepts
- **Hash Function**: Converts key to array index
- **Hash Table**: Array that stores key-value pairs
- **Collision**: Two keys map to same index
- **Load Factor**: n/m (entries/table size)

### Basic Structure
```
Key → Hash Function → Index → Value

Example:
"apple" → hash("apple") → 5 → 100
```

---

## Hash Function

### Properties of Good Hash Function
1. **Deterministic**: Same key → same hash value
2. **Uniform Distribution**: Evenly distributes keys
3. **Fast Computation**: O(1) time
4. **Minimize Collisions**: Different keys → different values

### Common Hash Functions

```python
# 1. Division Method
def hash_division(key, table_size):
    """
    h(k) = k % m
    Time: O(1)
    """
    return key % table_size

# 2. Multiplication Method
def hash_multiplication(key, table_size):
    """
    h(k) = floor(m * (k*A mod 1))
    A = (√5 - 1)/2 ≈ 0.618 (golden ratio)
    """
    A = 0.6180339887
    return int(table_size * ((key * A) % 1))

# 3. String Hashing
def hash_string(s, table_size):
    """
    Polynomial rolling hash
    Time: O(len(s))
    """
    hash_value = 0
    prime = 31
    
    for char in s:
        hash_value = (hash_value * prime + ord(char)) % table_size
    
    return hash_value

# 4. Python's hash()
def hash_python(key, table_size):
    """
    Using built-in hash function
    """
    return hash(key) % table_size
```

---

## Collision Resolution Techniques

### 1. Chaining (Open Hashing)

Each array index contains a linked list of colliding elements.

```python
class HashTableChaining:
    """
    Hash table using chaining
    Time: O(1) average, O(n) worst
    Space: O(n)
    """
    def __init__(self, size=10):
        self.size = size
        self.table = [[] for _ in range(size)]
    
    def hash_function(self, key):
        """Hash function using division method"""
        return hash(key) % self.size
    
    def insert(self, key, value):
        """
        Insert key-value pair
        Time: O(1) average
        """
        index = self.hash_function(key)
        
        # Update if key exists
        for i, (k, v) in enumerate(self.table[index]):
            if k == key:
                self.table[index][i] = (key, value)
                return
        
        # Add new key-value
        self.table[index].append((key, value))
    
    def search(self, key):
        """
        Search for key
        Time: O(1) average
        """
        index = self.hash_function(key)
        
        for k, v in self.table[index]:
            if k == key:
                return v
        
        return None
    
    def delete(self, key):
        """
        Delete key
        Time: O(1) average
        """
        index = self.hash_function(key)
        
        for i, (k, v) in enumerate(self.table[index]):
            if k == key:
                del self.table[index][i]
                return True
        
        return False
    
    def display(self):
        """Display hash table"""
        for i, bucket in enumerate(self.table):
            print(f"{i}: {bucket}")
```

#### Dry Run: Chaining
```
Table size = 7
Insert: 10, 20, 15, 7, 22

hash(10) = 10 % 7 = 3
hash(20) = 20 % 7 = 6
hash(15) = 15 % 7 = 1
hash(7) = 7 % 7 = 0
hash(22) = 22 % 7 = 1 (collision!)

Table:
0: [7]
1: [15, 22]  ← Collision handled by chaining
2: []
3: [10]
4: []
5: []
6: [20]
```

---

### 2. Open Addressing

Store all elements in the hash table itself. When collision occurs, probe for next empty slot.

#### a) Linear Probing

```python
class HashTableLinearProbing:
    """
    Hash table using linear probing
    h(k, i) = (h'(k) + i) % m
    """
    def __init__(self, size=10):
        self.size = size
        self.keys = [None] * size
        self.values = [None] * size
        self.count = 0
    
    def hash_function(self, key):
        return hash(key) % self.size
    
    def insert(self, key, value):
        """
        Insert using linear probing
        Time: O(1) average
        """
        if self.count >= self.size:
            raise Exception("Hash table is full")
        
        index = self.hash_function(key)
        
        # Linear probing
        while self.keys[index] is not None:
            if self.keys[index] == key:
                # Update existing key
                self.values[index] = value
                return
            
            index = (index + 1) % self.size
        
        # Insert at empty slot
        self.keys[index] = key
        self.values[index] = value
        self.count += 1
    
    def search(self, key):
        """
        Search using linear probing
        """
        index = self.hash_function(key)
        start_index = index
        
        while self.keys[index] is not None:
            if self.keys[index] == key:
                return self.values[index]
            
            index = (index + 1) % self.size
            
            if index == start_index:
                break
        
        return None
    
    def delete(self, key):
        """
        Delete with rehashing
        """
        index = self.hash_function(key)
        
        while self.keys[index] is not None:
            if self.keys[index] == key:
                self.keys[index] = None
                self.values[index] = None
                self.count -= 1
                
                # Rehash subsequent entries
                self._rehash_cluster(index)
                return True
            
            index = (index + 1) % self.size
        
        return False
    
    def _rehash_cluster(self, start):
        """Rehash cluster after deletion"""
        index = (start + 1) % self.size
        
        while self.keys[index] is not None:
            key = self.keys[index]
            value = self.values[index]
            
            self.keys[index] = None
            self.values[index] = None
            self.count -= 1
            
            self.insert(key, value)
            
            index = (index + 1) % self.size
```

#### Dry Run: Linear Probing
```
Table size = 7
Insert: 10, 17, 7, 3

hash(10) = 10 % 7 = 3
Insert at index 3

hash(17) = 17 % 7 = 3 (collision!)
Probe: (3+1) % 7 = 4
Insert at index 4

hash(7) = 7 % 7 = 0
Insert at index 0

hash(3) = 3 % 7 = 3 (collision!)
Probe: (3+1) % 7 = 4 (occupied)
Probe: (4+1) % 7 = 5
Insert at index 5

Table:
Index: 0  1  2  3  4  5  6
Value: 7  -  -  10 17 3  -
```

---

#### b) Quadratic Probing

```python
class HashTableQuadraticProbing:
    """
    Quadratic probing: h(k, i) = (h'(k) + c1*i + c2*i²) % m
    """
    def __init__(self, size=10):
        self.size = size
        self.keys = [None] * size
        self.values = [None] * size
    
    def hash_function(self, key):
        return hash(key) % self.size
    
    def insert(self, key, value):
        """
        Insert using quadratic probing
        """
        index = self.hash_function(key)
        i = 0
        
        while i < self.size:
            # Quadratic probe
            probe_index = (index + i * i) % self.size
            
            if self.keys[probe_index] is None or self.keys[probe_index] == key:
                self.keys[probe_index] = key
                self.values[probe_index] = value
                return
            
            i += 1
        
        raise Exception("Hash table is full")
    
    def search(self, key):
        """Search using quadratic probing"""
        index = self.hash_function(key)
        i = 0
        
        while i < self.size:
            probe_index = (index + i * i) % self.size
            
            if self.keys[probe_index] == key:
                return self.values[probe_index]
            
            if self.keys[probe_index] is None:
                return None
            
            i += 1
        
        return None
```

---

#### c) Double Hashing

```python
class HashTableDoubleHashing:
    """
    Double hashing: h(k, i) = (h1(k) + i * h2(k)) % m
    """
    def __init__(self, size=10):
        self.size = size
        self.keys = [None] * size
        self.values = [None] * size
    
    def hash1(self, key):
        """Primary hash function"""
        return hash(key) % self.size
    
    def hash2(self, key):
        """Secondary hash function"""
        # Should never return 0
        return 1 + (hash(key) % (self.size - 1))
    
    def insert(self, key, value):
        """Insert using double hashing"""
        index = self.hash1(key)
        step = self.hash2(key)
        i = 0
        
        while i < self.size:
            probe_index = (index + i * step) % self.size
            
            if self.keys[probe_index] is None or self.keys[probe_index] == key:
                self.keys[probe_index] = key
                self.values[probe_index] = value
                return
            
            i += 1
        
        raise Exception("Hash table is full")
```

---

## Python Dictionary Implementation

```python
# Python dict uses hash table internally

# Basic operations
hash_map = {}

# Insert
hash_map['apple'] = 5
hash_map['banana'] = 3

# Search
print(hash_map['apple'])  # 5

# Delete
del hash_map['banana']

# Check existence
if 'apple' in hash_map:
    print("Found")

# Get with default
value = hash_map.get('orange', 0)

# Iterate
for key, value in hash_map.items():
    print(f"{key}: {value}")

# Python Counter (specialized dict)
from collections import Counter

# Count frequencies
arr = [1, 2, 2, 3, 3, 3]
count = Counter(arr)
# Counter({3: 3, 2: 2, 1: 1})

# defaultdict (dict with default value)
from collections import defaultdict

dd = defaultdict(int)
dd['a'] += 1  # No KeyError, default is 0
```

---

## Classic Hashing Problems

### 1. Two Sum

```python
def two_sum(nums, target):
    """
    Find indices of two numbers that sum to target
    Time: O(n)
    Space: O(n)
    """
    seen = {}
    
    for i, num in enumerate(nums):
        complement = target - num
        
        if complement in seen:
            return [seen[complement], i]
        
        seen[num] = i
    
    return []
```

#### Dry Run
```
nums = [2, 7, 11, 15], target = 9

i=0, num=2:
  complement = 9-2 = 7
  7 not in seen
  seen = {2: 0}

i=1, num=7:
  complement = 9-7 = 2
  2 in seen! ✓
  return [seen[2], 1] = [0, 1]
```

---

### 2. Group Anagrams

```python
from collections import defaultdict

def group_anagrams(strs):
    """
    Group anagrams together
    Time: O(n * k log k) where k = max string length
    Space: O(n * k)
    """
    anagram_map = defaultdict(list)
    
    for s in strs:
        # Sort string as key
        key = ''.join(sorted(s))
        anagram_map[key].append(s)
    
    return list(anagram_map.values())

# Alternative: Using character count
def group_anagrams_count(strs):
    """
    Using character count as key
    Time: O(n * k) - faster!
    """
    anagram_map = defaultdict(list)
    
    for s in strs:
        # Count array as key
        count = [0] * 26
        for char in s:
            count[ord(char) - ord('a')] += 1
        
        # Tuple as key (lists not hashable)
        key = tuple(count)
        anagram_map[key].append(s)
    
    return list(anagram_map.values())
```

#### Dry Run
```
strs = ["eat", "tea", "tan", "ate", "nat", "bat"]

Sorted keys:
"eat" → "aet"
"tea" → "aet"
"tan" → "ant"
"ate" → "aet"
"nat" → "ant"
"bat" → "abt"

Groups:
"aet": ["eat", "tea", "ate"]
"ant": ["tan", "nat"]
"abt": ["bat"]

Result: [["eat","tea","ate"], ["tan","nat"], ["bat"]]
```

---

### 3. Longest Consecutive Sequence

```python
def longest_consecutive(nums):
    """
    Find length of longest consecutive sequence
    Time: O(n)
    Space: O(n)
    """
    if not nums:
        return 0
    
    num_set = set(nums)
    longest = 0
    
    for num in num_set:
        # Start of sequence
        if num - 1 not in num_set:
            current_num = num
            current_length = 1
            
            # Count sequence length
            while current_num + 1 in num_set:
                current_num += 1
                current_length += 1
            
            longest = max(longest, current_length)
    
    return longest
```

#### Dry Run
```
nums = [100, 4, 200, 1, 3, 2]
num_set = {100, 4, 200, 1, 3, 2}

Check 100:
  99 not in set → start of sequence
  100 → 101 not in set
  length = 1

Check 4:
  3 in set → not start, skip

Check 200:
  199 not in set → start
  200 → 201 not in set
  length = 1

Check 1:
  0 not in set → start
  1 → 2 → 3 → 4 → 5 not in set
  length = 4

Result: 4 (sequence: 1,2,3,4)
```

---

### 4. Subarray Sum Equals K

```python
from collections import defaultdict

def subarray_sum(nums, k):
    """
    Count subarrays with sum equal to k
    Time: O(n)
    Space: O(n)
    """
    count = 0
    prefix_sum = 0
    sum_count = defaultdict(int)
    sum_count[0] = 1  # Important!
    
    for num in nums:
        prefix_sum += num
        
        # Check if (prefix_sum - k) exists
        if (prefix_sum - k) in sum_count:
            count += sum_count[prefix_sum - k]
        
        sum_count[prefix_sum] += 1
    
    return count
```

#### Dry Run
```
nums = [1, 2, 3], k = 3

Initial: sum_count = {0: 1}

i=0, num=1:
  prefix_sum = 1
  (1-3) = -2 not in sum_count
  sum_count = {0:1, 1:1}

i=1, num=2:
  prefix_sum = 3
  (3-3) = 0 in sum_count! count += 1
  sum_count = {0:1, 1:1, 3:1}

i=2, num=3:
  prefix_sum = 6
  (6-3) = 3 in sum_count! count += 1
  sum_count = {0:1, 1:1, 3:1, 6:1}

Result: 2 (subarrays: [3] and [1,2])
```

---

### 5. First Unique Character

```python
from collections import Counter

def first_uniq_char(s):
    """
    Find first unique character index
    Time: O(n)
    Space: O(1) - at most 26 letters
    """
    count = Counter(s)
    
    for i, char in enumerate(s):
        if count[char] == 1:
            return i
    
    return -1

# Alternative: Two passes
def first_uniq_char_twopass(s):
    """
    Using dictionary
    """
    # First pass: count
    char_count = {}
    for char in s:
        char_count[char] = char_count.get(char, 0) + 1
    
    # Second pass: find first unique
    for i, char in enumerate(s):
        if char_count[char] == 1:
            return i
    
    return -1
```

---

### 6. Valid Anagram

```python
from collections import Counter

def is_anagram(s, t):
    """
    Check if t is anagram of s
    Time: O(n)
    Space: O(1) - at most 26 letters
    """
    if len(s) != len(t):
        return False
    
    return Counter(s) == Counter(t)

# Alternative: Sorting
def is_anagram_sort(s, t):
    """
    Time: O(n log n)
    Space: O(1)
    """
    return sorted(s) == sorted(t)

# Alternative: Character count array
def is_anagram_array(s, t):
    """
    Time: O(n)
    Space: O(1)
    """
    if len(s) != len(t):
        return False
    
    count = [0] * 26
    
    for char in s:
        count[ord(char) - ord('a')] += 1
    
    for char in t:
        count[ord(char) - ord('a')] -= 1
        if count[ord(char) - ord('a')] < 0:
            return False
    
    return True
```

---

### 7. Contains Duplicate II

```python
def contains_nearby_duplicate(nums, k):
    """
    Check if duplicate exists within distance k
    Time: O(n)
    Space: O(min(n, k))
    """
    seen = {}
    
    for i, num in enumerate(nums):
        if num in seen and i - seen[num] <= k:
            return True
        
        seen[num] = i
    
    return False

# Alternative: Sliding window with set
def contains_nearby_duplicate_window(nums, k):
    """
    Using sliding window
    """
    window = set()
    
    for i, num in enumerate(nums):
        if num in window:
            return True
        
        window.add(num)
        
        # Maintain window size k
        if len(window) > k:
            window.remove(nums[i - k])
    
    return False
```

---

### 8. Isomorphic Strings

```python
def is_isomorphic(s, t):
    """
    Check if strings are isomorphic
    Time: O(n)
    Space: O(1) - at most 256 ASCII chars
    """
    if len(s) != len(t):
        return False
    
    s_to_t = {}
    t_to_s = {}
    
    for char_s, char_t in zip(s, t):
        # Check s to t mapping
        if char_s in s_to_t:
            if s_to_t[char_s] != char_t:
                return False
        else:
            s_to_t[char_s] = char_t
        
        # Check t to s mapping
        if char_t in t_to_s:
            if t_to_s[char_t] != char_s:
                return False
        else:
            t_to_s[char_t] = char_s
    
    return True
```

---

### 9. Design HashMap

```python
class MyHashMap:
    """
    Design HashMap from scratch
    """
    def __init__(self):
        self.size = 1000
        self.table = [[] for _ in range(self.size)]
    
    def hash(self, key):
        return key % self.size
    
    def put(self, key, value):
        """Insert or update"""
        index = self.hash(key)
        
        for i, (k, v) in enumerate(self.table[index]):
            if k == key:
                self.table[index][i] = (key, value)
                return
        
        self.table[index].append((key, value))
    
    def get(self, key):
        """Get value by key"""
        index = self.hash(key)
        
        for k, v in self.table[index]:
            if k == key:
                return v
        
        return -1
    
    def remove(self, key):
        """Remove key"""
        index = self.hash(key)
        
        for i, (k, v) in enumerate(self.table[index]):
            if k == key:
                del self.table[index][i]
                return
```

---

### 10. LRU Cache

```python
from collections import OrderedDict

class LRUCache:
    """
    Least Recently Used Cache
    Time: O(1) for get and put
    Space: O(capacity)
    """
    def __init__(self, capacity):
        self.cache = OrderedDict()
        self.capacity = capacity
    
    def get(self, key):
        """Get value and mark as recently used"""
        if key not in self.cache:
            return -1
        
        # Move to end (most recent)
        self.cache.move_to_end(key)
        return self.cache[key]
    
    def put(self, key, value):
        """Put key-value pair"""
        if key in self.cache:
            # Update and move to end
            self.cache.move_to_end(key)
        
        self.cache[key] = value
        
        # Evict least recently used
        if len(self.cache) > self.capacity:
            self.cache.popitem(last=False)
```

---

## Time Complexity Summary

| Operation | Average | Worst |
|-----------|---------|-------|
| Search | O(1) | O(n) |
| Insert | O(1) | O(n) |
| Delete | O(1) | O(n) |
| Space | O(n) | O(n) |

---

## Load Factor

```
Load Factor α = n/m
n = number of entries
m = table size

Good practice: α ≤ 0.75 for chaining
               α ≤ 0.5 for open addressing

When α exceeds threshold, rehash to larger table
```

---

## Exam Tips

### 1. MCQ Questions
**Q**: Average time for search in hash table?
**A**: O(1)

**Q**: Worst case time for hash table?
**A**: O(n) when all keys collide

**Q**: Which collision resolution uses linked lists?
**A**: Chaining

**Q**: In open addressing, what happens when collision occurs?
**A**: Probe for next empty slot

### 2. Key Concepts
- Hash function maps keys to indices
- Collisions are inevitable (Pigeonhole Principle)
- Chaining vs Open Addressing
- Load factor affects performance
- Python dict uses hash table

### 3. Common Patterns
- **Frequency counting**: Use Counter or dict
- **Two pointer with hash**: Two Sum pattern
- **Seen/visited tracking**: Use set
- **Grouping**: defaultdict(list)
- **Running sum**: Prefix sum with hash

### Key Points
1. **O(1) Operations**: Average case for search, insert, delete
2. **Collisions**: Handle with chaining or open addressing
3. **Hash Function**: Should be deterministic and uniform
4. **Load Factor**: Keep below 0.75 for good performance
5. **Python dict**: Implements hash table internally
6. **Applications**: Databases, caches, symbol tables, sets
7. **Trade-off**: Space for time efficiency
