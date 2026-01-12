# JSON Objects - Comprehensive Notes

## Overview
JSON (JavaScript Object Notation) is a lightweight data-interchange format. Easy for humans to read/write and machines to parse/generate.

### Key Features
- Text-based format
- Language-independent
- Human-readable
- Key-value pairs
- Supports nested structures

---

## JSON Syntax

### Data Types in JSON

```json
{
  "string": "Hello",
  "number": 42,
  "float": 3.14,
  "boolean": true,
  "null": null,
  "array": [1, 2, 3],
  "object": {"key": "value"}
}
```

### Rules
1. Data in name/value pairs
2. Data separated by commas
3. Curly braces hold objects
4. Square brackets hold arrays
5. Keys must be strings (in double quotes)
6. No trailing commas allowed

---

## Python JSON Module

```python
import json

# JSON string
json_string = '{"name": "John", "age": 30, "city": "New York"}'

# Parse JSON (string → dict)
data = json.loads(json_string)
print(data['name'])  # John

# Generate JSON (dict → string)
person = {"name": "Alice", "age": 25}
json_output = json.dumps(person)
print(json_output)  # {"name": "Alice", "age": 25}
```

---

## JSON Operations

### 1. Parse JSON String

```python
import json

def parse_json(json_string):
    """
    Convert JSON string to Python object
    Time: O(n)
    Space: O(n)
    """
    try:
        data = json.loads(json_string)
        return data
    except json.JSONDecodeError as e:
        print(f"Invalid JSON: {e}")
        return None

# Example
json_string = '{"name": "Bob", "age": 35, "skills": ["Python", "Java"]}'
data = parse_json(json_string)

# Access data
print(data['name'])        # Bob
print(data['skills'][0])   # Python
```

---

### 2. Convert to JSON String

```python
def to_json(python_obj, pretty=False):
    """
    Convert Python object to JSON string
    
    Args:
        python_obj: Python dict/list/etc
        pretty: Format with indentation
    """
    if pretty:
        return json.dumps(python_obj, indent=4, sort_keys=True)
    return json.dumps(python_obj)

# Example
data = {
    "name": "Charlie",
    "age": 28,
    "hobbies": ["reading", "coding"]
}

# Compact JSON
print(to_json(data))
# {"name": "Charlie", "age": 28, "hobbies": ["reading", "coding"]}

# Pretty JSON
print(to_json(data, pretty=True))
# {
#     "age": 28,
#     "hobbies": [
#         "reading",
#         "coding"
#     ],
#     "name": "Charlie"
# }
```

---

### 3. Read JSON from File

```python
def read_json_file(filename):
    """
    Read JSON from file
    Time: O(n)
    Space: O(n)
    """
    try:
        with open(filename, 'r') as file:
            data = json.load(file)
        return data
    except FileNotFoundError:
        print(f"File {filename} not found")
        return None
    except json.JSONDecodeError:
        print("Invalid JSON in file")
        return None

# Example
# data = read_json_file('users.json')
```

---

### 4. Write JSON to File

```python
def write_json_file(data, filename, pretty=True):
    """
    Write JSON to file
    """
    try:
        with open(filename, 'w') as file:
            if pretty:
                json.dump(data, file, indent=4)
            else:
                json.dump(data, file)
        return True
    except Exception as e:
        print(f"Error writing file: {e}")
        return False

# Example
data = {"users": [{"name": "Alice", "age": 25}]}
# write_json_file(data, 'output.json')
```

---

## Nested JSON Structures

### 1. Complex JSON Example

```python
complex_json = {
    "company": "Tech Corp",
    "employees": [
        {
            "id": 1,
            "name": "John Doe",
            "department": "Engineering",
            "skills": ["Python", "Java", "Go"],
            "address": {
                "street": "123 Main St",
                "city": "San Francisco",
                "zip": "94102"
            },
            "projects": [
                {"name": "Project A", "status": "Active"},
                {"name": "Project B", "status": "Complete"}
            ]
        },
        {
            "id": 2,
            "name": "Jane Smith",
            "department": "Marketing",
            "skills": ["SEO", "Content Writing"],
            "address": {
                "street": "456 Oak Ave",
                "city": "New York",
                "zip": "10001"
            },
            "projects": []
        }
    ],
    "founded": 2015,
    "active": True
}
```

---

### 2. Navigate Nested JSON

```python
def get_nested_value(data, keys):
    """
    Get value from nested JSON using key path
    
    Args:
        data: JSON object
        keys: List of keys to navigate
    
    Returns:
        Value at nested location or None
    """
    result = data
    
    for key in keys:
        if isinstance(result, dict) and key in result:
            result = result[key]
        elif isinstance(result, list) and isinstance(key, int) and key < len(result):
            result = result[key]
        else:
            return None
    
    return result

# Example usage
data = {
    "users": [
        {"name": "Alice", "age": 25},
        {"name": "Bob", "age": 30}
    ]
}

# Get Bob's age
age = get_nested_value(data, ["users", 1, "age"])
print(age)  # 30
```

---

### 3. Flatten Nested JSON

```python
def flatten_json(data, parent_key='', separator='_'):
    """
    Flatten nested JSON structure
    Time: O(n)
    Space: O(n)
    """
    items = []
    
    if isinstance(data, dict):
        for key, value in data.items():
            new_key = f"{parent_key}{separator}{key}" if parent_key else key
            
            if isinstance(value, (dict, list)):
                items.extend(flatten_json(value, new_key, separator).items())
            else:
                items.append((new_key, value))
    
    elif isinstance(data, list):
        for i, value in enumerate(data):
            new_key = f"{parent_key}{separator}{i}"
            
            if isinstance(value, (dict, list)):
                items.extend(flatten_json(value, new_key, separator).items())
            else:
                items.append((new_key, value))
    
    else:
        items.append((parent_key, data))
    
    return dict(items)

# Example
nested = {
    "user": {
        "name": "Alice",
        "address": {
            "city": "NYC",
            "zip": "10001"
        }
    },
    "scores": [85, 90, 95]
}

flat = flatten_json(nested)
print(flat)
# {
#     'user_name': 'Alice',
#     'user_address_city': 'NYC',
#     'user_address_zip': '10001',
#     'scores_0': 85,
#     'scores_1': 90,
#     'scores_2': 95
# }
```

---

### 4. Search in JSON

```python
def search_json(data, search_key, search_value):
    """
    Search for key-value pair in nested JSON
    Returns all matching objects
    """
    results = []
    
    def search_recursive(obj, path=""):
        if isinstance(obj, dict):
            # Check current object
            if search_key in obj and obj[search_key] == search_value:
                results.append({"path": path, "object": obj})
            
            # Recursively search nested objects
            for key, value in obj.items():
                new_path = f"{path}.{key}" if path else key
                search_recursive(value, new_path)
        
        elif isinstance(obj, list):
            for i, item in enumerate(obj):
                new_path = f"{path}[{i}]"
                search_recursive(item, new_path)
    
    search_recursive(data)
    return results

# Example
data = {
    "users": [
        {"name": "Alice", "age": 25, "city": "NYC"},
        {"name": "Bob", "age": 30, "city": "SF"},
        {"name": "Charlie", "age": 25, "city": "LA"}
    ]
}

# Find all users aged 25
results = search_json(data, "age", 25)
for result in results:
    print(result["path"], ":", result["object"]["name"])
# users[0] : Alice
# users[2] : Charlie
```

---

### 5. Update Nested JSON

```python
def update_nested_value(data, keys, new_value):
    """
    Update value in nested JSON
    
    Args:
        data: JSON object
        keys: List of keys to navigate
        new_value: New value to set
    """
    if len(keys) == 1:
        if isinstance(data, dict):
            data[keys[0]] = new_value
        elif isinstance(data, list) and isinstance(keys[0], int):
            data[keys[0]] = new_value
        return
    
    key = keys[0]
    
    if isinstance(data, dict) and key in data:
        update_nested_value(data[key], keys[1:], new_value)
    elif isinstance(data, list) and isinstance(key, int) and key < len(data):
        update_nested_value(data[key], keys[1:], new_value)

# Example
data = {
    "user": {
        "name": "Alice",
        "settings": {
            "theme": "dark"
        }
    }
}

update_nested_value(data, ["user", "settings", "theme"], "light")
print(data)
# {'user': {'name': 'Alice', 'settings': {'theme': 'light'}}}
```

---

### 6. Merge JSON Objects

```python
def merge_json(obj1, obj2):
    """
    Deep merge two JSON objects
    obj2 overwrites obj1 on conflicts
    """
    if isinstance(obj1, dict) and isinstance(obj2, dict):
        result = obj1.copy()
        
        for key, value in obj2.items():
            if key in result and isinstance(result[key], dict) and isinstance(value, dict):
                result[key] = merge_json(result[key], value)
            else:
                result[key] = value
        
        return result
    
    return obj2

# Example
obj1 = {
    "name": "Alice",
    "settings": {
        "theme": "dark",
        "language": "en"
    }
}

obj2 = {
    "age": 25,
    "settings": {
        "theme": "light",
        "notifications": True
    }
}

merged = merge_json(obj1, obj2)
print(json.dumps(merged, indent=2))
# {
#   "name": "Alice",
#   "age": 25,
#   "settings": {
#     "theme": "light",
#     "language": "en",
#     "notifications": true
#   }
# }
```

---

## JSON Array Operations

### 1. Filter Array

```python
def filter_json_array(json_array, condition):
    """
    Filter JSON array based on condition
    
    Args:
        json_array: List of JSON objects
        condition: Function that returns True/False
    """
    return [item for item in json_array if condition(item)]

# Example
users = [
    {"name": "Alice", "age": 25, "active": True},
    {"name": "Bob", "age": 30, "active": False},
    {"name": "Charlie", "age": 28, "active": True}
]

# Filter active users
active_users = filter_json_array(users, lambda u: u["active"])
print([u["name"] for u in active_users])  # ['Alice', 'Charlie']

# Filter users over 25
older_users = filter_json_array(users, lambda u: u["age"] > 25)
print([u["name"] for u in older_users])  # ['Bob', 'Charlie']
```

---

### 2. Sort JSON Array

```python
def sort_json_array(json_array, key, reverse=False):
    """
    Sort JSON array by key
    """
    return sorted(json_array, key=lambda x: x.get(key), reverse=reverse)

# Example
users = [
    {"name": "Charlie", "age": 28},
    {"name": "Alice", "age": 25},
    {"name": "Bob", "age": 30}
]

# Sort by age
sorted_by_age = sort_json_array(users, "age")
print([u["name"] for u in sorted_by_age])  # ['Alice', 'Charlie', 'Bob']

# Sort by name (descending)
sorted_by_name = sort_json_array(users, "name", reverse=True)
print([u["name"] for u in sorted_by_name])  # ['Charlie', 'Bob', 'Alice']
```

---

### 3. Group JSON Array

```python
from collections import defaultdict

def group_json_array(json_array, key):
    """
    Group JSON array by key
    """
    groups = defaultdict(list)
    
    for item in json_array:
        group_key = item.get(key)
        groups[group_key].append(item)
    
    return dict(groups)

# Example
users = [
    {"name": "Alice", "city": "NYC", "age": 25},
    {"name": "Bob", "city": "SF", "age": 30},
    {"name": "Charlie", "city": "NYC", "age": 28}
]

# Group by city
by_city = group_json_array(users, "city")
print(json.dumps(by_city, indent=2))
# {
#   "NYC": [
#     {"name": "Alice", "city": "NYC", "age": 25},
#     {"name": "Charlie", "city": "NYC", "age": 28}
#   ],
#   "SF": [
#     {"name": "Bob", "city": "SF", "age": 30}
#   ]
# }
```

---

### 4. Transform JSON Array

```python
def transform_json_array(json_array, transformer):
    """
    Transform each item in JSON array
    
    Args:
        json_array: List of JSON objects
        transformer: Function to transform each item
    """
    return [transformer(item) for item in json_array]

# Example
users = [
    {"name": "alice", "age": 25},
    {"name": "bob", "age": 30}
]

# Capitalize names
def capitalize_name(user):
    user["name"] = user["name"].capitalize()
    return user

transformed = transform_json_array(users, capitalize_name)
print(json.dumps(transformed, indent=2))
# [
#   {"name": "Alice", "age": 25},
#   {"name": "Bob", "age": 30}
# ]
```

---

## JSON Validation

### 1. Check Required Fields

```python
def validate_required_fields(data, required_fields):
    """
    Check if all required fields exist
    """
    missing = []
    
    for field in required_fields:
        if field not in data or data[field] is None:
            missing.append(field)
    
    return len(missing) == 0, missing

# Example
user = {"name": "Alice", "email": "alice@example.com"}
required = ["name", "email", "age"]

valid, missing = validate_required_fields(user, required)
if not valid:
    print(f"Missing fields: {missing}")  # Missing fields: ['age']
```

---

### 2. Validate Data Types

```python
def validate_types(data, schema):
    """
    Validate data types against schema
    
    Args:
        data: JSON object
        schema: Dict of {field: type}
    """
    errors = []
    
    for field, expected_type in schema.items():
        if field in data:
            if not isinstance(data[field], expected_type):
                errors.append(f"{field}: expected {expected_type}, got {type(data[field])}")
    
    return len(errors) == 0, errors

# Example
user = {"name": "Alice", "age": "25"}  # age should be int
schema = {"name": str, "age": int}

valid, errors = validate_types(user, schema)
if not valid:
    print("Validation errors:", errors)
    # Validation errors: ['age: expected <class 'int'>, got <class 'str'>']
```

---

## JSON Schema Example

```python
# Complex validation schema
user_schema = {
    "type": "object",
    "required": ["name", "email", "age"],
    "properties": {
        "name": {"type": "string", "minLength": 1},
        "email": {"type": "string", "pattern": r"^[\w\.-]+@[\w\.-]+\.\w+$"},
        "age": {"type": "integer", "minimum": 0, "maximum": 150},
        "address": {
            "type": "object",
            "properties": {
                "street": {"type": "string"},
                "city": {"type": "string"},
                "zip": {"type": "string"}
            }
        },
        "hobbies": {
            "type": "array",
            "items": {"type": "string"}
        }
    }
}

# Valid JSON
valid_user = {
    "name": "Alice",
    "email": "alice@example.com",
    "age": 25,
    "address": {
        "street": "123 Main St",
        "city": "NYC",
        "zip": "10001"
    },
    "hobbies": ["reading", "coding"]
}
```

---

## Common JSON Patterns

### 1. Pagination Response

```json
{
  "data": [
    {"id": 1, "name": "Item 1"},
    {"id": 2, "name": "Item 2"}
  ],
  "pagination": {
    "page": 1,
    "per_page": 10,
    "total": 100,
    "total_pages": 10
  }
}
```

### 2. API Error Response

```json
{
  "error": {
    "code": 400,
    "message": "Invalid request",
    "details": [
      {
        "field": "email",
        "issue": "Invalid email format"
      }
    ]
  }
}
```

### 3. Nested Resource

```json
{
  "user": {
    "id": 1,
    "name": "Alice",
    "profile": {
      "bio": "Software Engineer",
      "avatar": "https://example.com/avatar.jpg"
    },
    "posts": [
      {
        "id": 101,
        "title": "First Post",
        "comments": [
          {"id": 1001, "text": "Great post!"}
        ]
      }
    ]
  }
}
```

---

## Best Practices

### 1. Naming Conventions
```python
# Use snake_case or camelCase consistently
{
    "user_name": "Alice",      # snake_case
    "userName": "Alice"        # camelCase
}
```

### 2. Handle Null Values
```python
# Explicit null vs missing key
{
    "name": "Alice",
    "middle_name": null,       # Has no middle name
    # "nickname" not present    # Unknown if has nickname
}
```

### 3. Use Arrays for Multiple Values
```python
{
    "tags": ["python", "json", "data"],  # ✓ Good
    "tag1": "python",                     # ✗ Bad
    "tag2": "json"
}
```

---

## Time Complexity

| Operation | Time | Space |
|-----------|------|-------|
| Parse JSON | O(n) | O(n) |
| Serialize | O(n) | O(n) |
| Search | O(n) | O(1) |
| Flatten | O(n) | O(n) |
| Merge | O(n+m) | O(n+m) |

where n = number of elements

---

## Exam Tips

### 1. MCQ Questions
**Q**: JSON stands for?
**A**: JavaScript Object Notation

**Q**: Valid JSON data types?
**A**: String, Number, Boolean, Null, Object, Array

**Q**: JSON keys must be?
**A**: Strings in double quotes

**Q**: Time to parse JSON string?
**A**: O(n) where n is string length

### 2. Common Patterns
- **Nested access**: Use dot notation or bracket
- **Validation**: Check required fields and types
- **Transformation**: Map/filter/reduce operations
- **Flattening**: Convert nested to flat structure
- **Search**: Recursive traversal

### Key Points
1. **Text Format**: Language-independent data interchange
2. **Data Types**: string, number, boolean, null, object, array
3. **Keys**: Must be strings in double quotes
4. **Nested**: Supports complex hierarchical structures
5. **Parse/Serialize**: O(n) time complexity
6. **Python**: Use `json` module
7. **Validation**: Check structure and types
8. **Common Use**: APIs, config files, data storage
