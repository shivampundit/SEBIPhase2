# Stack - Comprehensive Notes

## Overview
A Stack is a linear data structure that follows the **LIFO (Last In First Out)** principle. The last element added is the first one to be removed.

### Real-World Analogies
- Stack of plates
- Browser back button
- Undo mechanism in editors
- Function call stack

### Key Characteristics
1. **LIFO principle**: Last In First Out
2. **Two main operations**: Push and Pop
3. **Top pointer**: Points to last inserted element
4. **Single-ended**: Access only from top

---

## Stack Operations

### Primary Operations
1. **Push**: Add element to top - O(1)
2. **Pop**: Remove element from top - O(1)
3. **Peek/Top**: View top element without removing - O(1)
4. **IsEmpty**: Check if stack is empty - O(1)
5. **Size**: Get number of elements - O(1)

---

## Implementation Methods

## 1. Array-based Stack

### C
```c
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

#define MAX_SIZE 100

typedef struct {
    int items[MAX_SIZE];
    int top;
} Stack;

// Initialize stack
Stack* createStack() {
    Stack* stack = (Stack*)malloc(sizeof(Stack));
    stack->top = -1;
    return stack;
}

// Check if stack is empty
bool isEmpty(Stack* stack) {
    return stack->top == -1;
}

// Check if stack is full
bool isFull(Stack* stack) {
    return stack->top == MAX_SIZE - 1;
}

// Push element
void push(Stack* stack, int data) {
    if (isFull(stack)) {
        printf("Stack Overflow\n");
        return;
    }
    stack->items[++stack->top] = data;
}

// Pop element
int pop(Stack* stack) {
    if (isEmpty(stack)) {
        printf("Stack Underflow\n");
        return -1;
    }
    return stack->items[stack->top--];
}

// Peek element
int peek(Stack* stack) {
    if (isEmpty(stack)) {
        printf("Stack is empty\n");
        return -1;
    }
    return stack->items[stack->top];
}

// Get size
int size(Stack* stack) {
    return stack->top + 1;
}
```

### C++
```cpp
#include <iostream>
#include <vector>
#include <stack>
#include <stdexcept>
using namespace std;

class ArrayStack {
private:
    vector<int> items;
    int capacity;
    
public:
    ArrayStack(int cap = 100) : capacity(cap) {}
    
    // Push element
    void push(int data) {
        /*
         * Time: O(1)
         * Space: O(1)
         */
        if (items.size() >= capacity) {
            throw overflow_error("Stack is full");
        }
        items.push_back(data);
    }
    
    // Pop element
    int pop() {
        /*
         * Time: O(1)
         * Space: O(1)
         */
        if (isEmpty()) {
            throw underflow_error("Stack is empty");
        }
        int value = items.back();
        items.pop_back();
        return value;
    }
    
    // Peek element
    int peek() {
        /*
         * Time: O(1)
         */
        if (isEmpty()) {
            throw underflow_error("Stack is empty");
        }
        return items.back();
    }
    
    // Check if empty
    bool isEmpty() {
        return items.empty();
    }
    
    // Get size
    int size() {
        return items.size();
    }
};

// Using STL stack
void useSTLStack() {
    stack<int> s;
    s.push(10);
    s.push(20);
    s.push(30);
    cout << s.top() << endl;  // 30
    s.pop();
    cout << s.size() << endl;  // 2
}
```

### Java
```java
import java.util.ArrayList;
import java.util.Stack;

class ArrayStack {
    private ArrayList<Integer> items;
    private int capacity;
    
    public ArrayStack(int capacity) {
        this.capacity = capacity;
        this.items = new ArrayList<>();
    }
    
    // Push element
    public void push(int data) {
        /*
         * Time: O(1)
         * Space: O(1)
         */
        if (items.size() >= capacity) {
            throw new StackOverflowError("Stack is full");
        }
        items.add(data);
    }
    
    // Pop element
    public int pop() {
        /*
         * Time: O(1)
         * Space: O(1)
         */
        if (isEmpty()) {
            throw new IllegalStateException("Stack is empty");
        }
        return items.remove(items.size() - 1);
    }
    
    // Peek element
    public int peek() {
        /*
         * Time: O(1)
         */
        if (isEmpty()) {
            throw new IllegalStateException("Stack is empty");
        }
        return items.get(items.size() - 1);
    }
    
    // Check if empty
    public boolean isEmpty() {
        return items.isEmpty();
    }
    
    // Get size
    public int size() {
        return items.size();
    }
}

// Using Java's Stack class
public class StackExample {
    public static void main(String[] args) {
        Stack<Integer> stack = new Stack<>();
        stack.push(10);
        stack.push(20);
        stack.push(30);
        System.out.println(stack.peek());  // 30
        stack.pop();
        System.out.println(stack.size());  // 2
    }
}
```

### Python
```python
class ArrayStack:
    def __init__(self, capacity=100):
        """
        Initialize stack with fixed capacity
        """
        self.capacity = capacity
        self.stack = []
        self.top = -1
    
    def push(self, data):
        """
        Push element to stack
        Time: O(1)
        Space: O(1)
        """
        if self.is_full():
            raise OverflowError("Stack is full")
        
        self.stack.append(data)
        self.top += 1
        return True
    
    def pop(self):
        """
        Remove and return top element
        Time: O(1)
        Space: O(1)
        """
        if self.is_empty():
            raise IndexError("Stack is empty")
        
        self.top -= 1
        return self.stack.pop()
    
    def peek(self):
        """
        Return top element without removing
        Time: O(1)
        """
        if self.is_empty():
            raise IndexError("Stack is empty")
        
        return self.stack[self.top]
    
    def is_empty(self):
        """Check if stack is empty"""
        return self.top == -1
    
    def is_full(self):
        """Check if stack is full"""
        return self.top == self.capacity - 1
    
    def size(self):
        """Return number of elements"""
        return self.top + 1
    
    def display(self):
        """Display stack elements"""
        if self.is_empty():
            print("Stack is empty")
            return
        
        print("Stack (top to bottom):")
        for i in range(self.top, -1, -1):
            print(f"| {self.stack[i]} |")
        print("+---+")

# Example Usage
stack = ArrayStack()
stack.push(10)
stack.push(20)
stack.push(30)
stack.display()
print(f"Pop: {stack.pop()}")  # 30
print(f"Peek: {stack.peek()}")  # 20
```

### Dry Run Example
```
Operations: push(5), push(3), pop(), push(7), peek(), pop()

Initial: []
top = -1

push(5):
Stack: [5]
top = 0

push(3):
Stack: [5, 3]
top = 1

pop():
Returns: 3
Stack: [5]
top = 0

push(7):
Stack: [5, 7]
top = 1

peek():
Returns: 7
Stack: [5, 7]
top = 1 (unchanged)

pop():
Returns: 7
Stack: [5]
top = 0
```

---

## 2. Linked List-based Stack

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedListStack:
    def __init__(self):
        """
        Initialize empty stack
        """
        self.top = None
        self.length = 0
    
    def push(self, data):
        """
        Push element to stack
        Time: O(1)
        """
        new_node = Node(data)
        new_node.next = self.top
        self.top = new_node
        self.length += 1
    
    def pop(self):
        """
        Remove and return top element
        Time: O(1)
        """
        if self.is_empty():
            raise IndexError("Stack is empty")
        
        popped_data = self.top.data
        self.top = self.top.next
        self.length -= 1
        return popped_data
    
    def peek(self):
        """
        Return top element
        Time: O(1)
        """
        if self.is_empty():
            raise IndexError("Stack is empty")
        
        return self.top.data
    
    def is_empty(self):
        """Check if stack is empty"""
        return self.top is None
    
    def size(self):
        """Return stack size"""
        return self.length
    
    def display(self):
        """Display all elements"""
        if self.is_empty():
            print("Stack is empty")
            return
        
        current = self.top
        print("Stack (top to bottom):")
        while current:
            print(f"| {current.data} |")
            current = current.next
        print("+---+")

# Example
stack = LinkedListStack()
stack.push(10)
stack.push(20)
stack.push(30)
print(f"Pop: {stack.pop()}")  # 30
```

---

## Classic Stack Problems

### 1. Balanced Parentheses

#### C
```c
#include <string.h>

bool isMatching(char open, char close) {
    return (open == '(' && close == ')') ||
           (open == '[' && close == ']') ||
           (open == '{' && close == '}');
}

bool isBalanced(char* expression) {
    /*
     * Check if parentheses are balanced
     * Time: O(n)
     * Space: O(n)
     */
    Stack* stack = createStack();
    
    for (int i = 0; i < strlen(expression); i++) {
        char ch = expression[i];
        
        if (ch == '(' || ch == '[' || ch == '{') {
            push(stack, ch);
        } else if (ch == ')' || ch == ']' || ch == '}') {
            if (isEmpty(stack)) {
                return false;
            }
            char top = pop(stack);
            if (!isMatching(top, ch)) {
                return false;
            }
        }
    }
    
    return isEmpty(stack);
}

// Test cases
int main() {
    printf("%d\n", isBalanced("()"));          // 1 (true)
    printf("%d\n", isBalanced("()[]{}"));      // 1 (true)
    printf("%d\n", isBalanced("(]"));          // 0 (false)
    printf("%d\n", isBalanced("([)]"));        // 0 (false)
    printf("%d\n", isBalanced("{[]}"));        // 1 (true)
    return 0;
}
```

#### C++
```cpp
#include <unordered_map>

bool isBalanced(string expression) {
    /*
     * Check if parentheses are balanced
     * Time: O(n)
     * Space: O(n)
     */
    stack<char> s;
    unordered_map<char, char> matching = {
        {'(', ')'}, {'[', ']'}, {'{', '}'}
    };
    
    for (char ch : expression) {
        if (matching.count(ch)) {  // Opening bracket
            s.push(ch);
        } else if (ch == ')' || ch == ']' || ch == '}') {  // Closing bracket
            if (s.empty() || matching[s.top()] != ch) {
                return false;
            }
            s.pop();
        }
    }
    
    return s.empty();
}

// Test cases
int main() {
    cout << isBalanced("()") << endl;          // 1 (true)
    cout << isBalanced("()[]{}") << endl;      // 1 (true)
    cout << isBalanced("(]") << endl;          // 0 (false)
    cout << isBalanced("([)]") << endl;        // 0 (false)
    cout << isBalanced("{[]}") << endl;        // 1 (true)
    return 0;
}
```

#### Java
```java
import java.util.HashMap;
import java.util.Stack;

public static boolean isBalanced(String expression) {
    /*
     * Check if parentheses are balanced
     * Time: O(n)
     * Space: O(n)
     */
    Stack<Character> stack = new Stack<>();
    HashMap<Character, Character> matching = new HashMap<>();
    matching.put('(', ')');
    matching.put('[', ']');
    matching.put('{', '}');
    
    for (char ch : expression.toCharArray()) {
        if (matching.containsKey(ch)) {  // Opening bracket
            stack.push(ch);
        } else if (ch == ')' || ch == ']' || ch == '}') {  // Closing bracket
            if (stack.isEmpty() || matching.get(stack.pop()) != ch) {
                return false;
            }
        }
    }
    
    return stack.isEmpty();
}

// Test cases
public static void main(String[] args) {
    System.out.println(isBalanced("()"));          // true
    System.out.println(isBalanced("()[]{}"));      // true
    System.out.println(isBalanced("(]"));          // false
    System.out.println(isBalanced("([)]"));        // false
    System.out.println(isBalanced("{[]}"));        // true
}
```

#### Python
```python
def is_balanced(expression):
    """
    Check if parentheses are balanced
    Time: O(n)
    Space: O(n)
    """
    stack = []
    matching = {'(': ')', '[': ']', '{': '}'}
    
    for char in expression:
        if char in matching:  # Opening bracket
            stack.append(char)
        elif char in matching.values():  # Closing bracket
            if not stack or matching[stack.pop()] != char:
                return False
    
    return len(stack) == 0

# Test cases
print(is_balanced("()"))          # True
print(is_balanced("()[]{}"))      # True
print(is_balanced("(]"))          # False
print(is_balanced("([)]"))        # False
print(is_balanced("{[]}"))        # True
```

#### Dry Run
```
Expression: "{[()]}"

char = '{': stack = ['{']
char = '[': stack = ['{', '[']
char = '(': stack = ['{', '[', '(']
char = ')': pop '(' matches ')', stack = ['{', '[']
char = ']': pop '[' matches ']', stack = ['{']
char = '}': pop '{' matches '}', stack = []

Stack empty → Balanced ✓
```

---

### 2. Postfix Evaluation

```python
def evaluate_postfix(expression):
    """
    Evaluate postfix expression
    Time: O(n)
    Space: O(n)
    """
    stack = []
    operators = {'+', '-', '*', '/', '^'}
    
    for token in expression.split():
        if token not in operators:
            stack.append(int(token))
        else:
            b = stack.pop()
            a = stack.pop()
            
            if token == '+':
                stack.append(a + b)
            elif token == '-':
                stack.append(a - b)
            elif token == '*':
                stack.append(a * b)
            elif token == '/':
                stack.append(a // b)
            elif token == '^':
                stack.append(a ** b)
    
    return stack.pop()

# Example
expr = "2 3 1 * + 9 -"
print(f"Result: {evaluate_postfix(expr)}")  # -4
```

#### Dry Run
```
Expression: "2 3 1 * + 9 -"

Token '2': stack = [2]
Token '3': stack = [2, 3]
Token '1': stack = [2, 3, 1]
Token '*': pop 1, 3; push 3*1=3; stack = [2, 3]
Token '+': pop 3, 2; push 2+3=5; stack = [5]
Token '9': stack = [5, 9]
Token '-': pop 9, 5; push 5-9=-4; stack = [-4]

Result: -4
```

---

### 3. Infix to Postfix Conversion

```python
def infix_to_postfix(expression):
    """
    Convert infix to postfix notation
    Time: O(n)
    Space: O(n)
    """
    precedence = {'+': 1, '-': 1, '*': 2, '/': 2, '^': 3}
    stack = []
    output = []
    
    for char in expression:
        if char.isalnum():  # Operand
            output.append(char)
        elif char == '(':
            stack.append(char)
        elif char == ')':
            while stack and stack[-1] != '(':
                output.append(stack.pop())
            stack.pop()  # Remove '('
        else:  # Operator
            while (stack and stack[-1] != '(' and
                   stack[-1] in precedence and
                   precedence[stack[-1]] >= precedence[char]):
                output.append(stack.pop())
            stack.append(char)
    
    while stack:
        output.append(stack.pop())
    
    return ''.join(output)

# Example
print(infix_to_postfix("A+B*C"))      # ABC*+
print(infix_to_postfix("(A+B)*C"))    # AB+C*
print(infix_to_postfix("A+B*C-D"))    # ABC*+D-
```

---

### 4. Next Greater Element

```python
def next_greater_elements(arr):
    """
    Find next greater element for each element
    Time: O(n)
    Space: O(n)
    """
    n = len(arr)
    result = [-1] * n
    stack = []
    
    for i in range(n):
        while stack and arr[stack[-1]] < arr[i]:
            index = stack.pop()
            result[index] = arr[i]
        stack.append(i)
    
    return result

# Example
arr = [4, 5, 2, 25, 7, 8]
print(next_greater_elements(arr))  # [5, 25, 25, -1, 8, -1]
```

#### Dry Run
```
Array: [4, 5, 2, 25]

i=0, arr[0]=4: stack = [0]
i=1, arr[1]=5: 5 > arr[0]=4
    result[0] = 5
    stack = [1]
i=2, arr[2]=2: 2 < 5
    stack = [1, 2]
i=3, arr[3]=25: 25 > arr[2]=2
    result[2] = 25
    25 > arr[1]=5
    result[1] = 25
    stack = [3]

Result: [5, 25, 25, -1]
```

---

### 5. Stock Span Problem

```python
def calculate_span(prices):
    """
    Calculate stock span for each day
    Time: O(n)
    Space: O(n)
    """
    n = len(prices)
    span = [0] * n
    stack = []
    
    for i in range(n):
        while stack and prices[stack[-1]] <= prices[i]:
            stack.pop()
        
        span[i] = i + 1 if not stack else i - stack[-1]
        stack.append(i)
    
    return span

# Example
prices = [100, 80, 60, 70, 60, 75, 85]
print(calculate_span(prices))  # [1, 1, 1, 2, 1, 4, 6]
```

---

### 6. Min Stack

```python
class MinStack:
    """
    Stack with O(1) getMin operation
    """
    def __init__(self):
        self.stack = []
        self.min_stack = []
    
    def push(self, val):
        """
        Push value to stack
        Time: O(1)
        """
        self.stack.append(val)
        
        if not self.min_stack or val <= self.min_stack[-1]:
            self.min_stack.append(val)
    
    def pop(self):
        """
        Pop from stack
        Time: O(1)
        """
        if not self.stack:
            return None
        
        val = self.stack.pop()
        if val == self.min_stack[-1]:
            self.min_stack.pop()
        
        return val
    
    def top(self):
        """Get top element"""
        return self.stack[-1] if self.stack else None
    
    def get_min(self):
        """
        Get minimum element
        Time: O(1)
        """
        return self.min_stack[-1] if self.min_stack else None

# Example
min_stack = MinStack()
min_stack.push(5)
min_stack.push(2)
min_stack.push(3)
print(f"Min: {min_stack.get_min()}")  # 2
min_stack.pop()
print(f"Min: {min_stack.get_min()}")  # 2
```

---

### 7. Valid Parenthesis String

```python
def check_valid_string(s):
    """
    Check if string is valid with wildcards
    Time: O(n)
    Space: O(n)
    """
    open_stack = []
    star_stack = []
    
    for i, char in enumerate(s):
        if char == '(':
            open_stack.append(i)
        elif char == '*':
            star_stack.append(i)
        else:  # ')'
            if open_stack:
                open_stack.pop()
            elif star_stack:
                star_stack.pop()
            else:
                return False
    
    # Match remaining '(' with '*'
    while open_stack and star_stack:
        if open_stack[-1] > star_stack[-1]:
            return False
        open_stack.pop()
        star_stack.pop()
    
    return len(open_stack) == 0

# Example
print(check_valid_string("()"))      # True
print(check_valid_string("(*)"))     # True
print(check_valid_string("(*))"))    # True
```

---

### 8. Largest Rectangle in Histogram

```python
def largest_rectangle_area(heights):
    """
    Find largest rectangle in histogram
    Time: O(n)
    Space: O(n)
    """
    stack = []
    max_area = 0
    index = 0
    
    while index < len(heights):
        if not stack or heights[stack[-1]] <= heights[index]:
            stack.append(index)
            index += 1
        else:
            top = stack.pop()
            width = index if not stack else index - stack[-1] - 1
            area = heights[top] * width
            max_area = max(max_area, area)
    
    while stack:
        top = stack.pop()
        width = index if not stack else index - stack[-1] - 1
        area = heights[top] * width
        max_area = max(max_area, area)
    
    return max_area

# Example
heights = [2, 1, 5, 6, 2, 3]
print(largest_rectangle_area(heights))  # 10
```

---

## Applications of Stack

### 1. Function Call Stack
```python
def factorial(n):
    """
    Recursion uses stack internally
    """
    if n <= 1:
        return 1
    return n * factorial(n - 1)

# Call stack for factorial(3):
# factorial(3)
#   → factorial(2)
#     → factorial(1) returns 1
#   → 2 * 1 = 2
# → 3 * 2 = 6
```

### 2. Undo Mechanism
```python
class TextEditor:
    def __init__(self):
        self.text = ""
        self.undo_stack = []
    
    def write(self, text):
        self.undo_stack.append(self.text)
        self.text += text
    
    def undo(self):
        if self.undo_stack:
            self.text = self.undo_stack.pop()
```

### 3. Browser History
```python
class BrowserHistory:
    def __init__(self):
        self.back_stack = []
        self.forward_stack = []
        self.current = None
    
    def visit(self, url):
        if self.current:
            self.back_stack.append(self.current)
        self.current = url
        self.forward_stack = []
    
    def back(self):
        if self.back_stack:
            self.forward_stack.append(self.current)
            self.current = self.back_stack.pop()
    
    def forward(self):
        if self.forward_stack:
            self.back_stack.append(self.current)
            self.current = self.forward_stack.pop()
```

---

## Stack vs Other Data Structures

| Feature | Stack | Queue | Array |
|---------|-------|-------|-------|
| Access | LIFO | FIFO | Random |
| Insert | O(1) at top | O(1) at rear | O(1) at end |
| Delete | O(1) from top | O(1) from front | O(1) from end |
| Search | O(n) | O(n) | O(n) |
| Space | Dynamic | Dynamic | Fixed/Dynamic |

---

## Time Complexity Summary

| Operation | Time Complexity |
|-----------|----------------|
| Push | O(1) |
| Pop | O(1) |
| Peek/Top | O(1) |
| IsEmpty | O(1) |
| Size | O(1) |
| Search | O(n) |

---

## Exam Tips

### 1. MCQ Topics
**Q**: Which data structure uses LIFO?
**A**: Stack

**Q**: Time complexity of push/pop in stack?
**A**: O(1)

**Q**: Application of stack?
**A**: Function calls, expression evaluation, backtracking

### 2. Code Completion
```python
def push(self, data):
    new_node = Node(data)
    new_node.next = self.top  # Link to current top
    self.top = new_node       # Update top
```

### 3. Dry Run Output
```
Stack operations: push(1), push(2), pop(), push(3), peek()

After operations:
Stack: [1, 3]  (top = 3)
pop() returned: 2
peek() returned: 3
```

### 4. Common Mistakes
- Forgetting to check empty before pop/peek
- Not updating top pointer correctly
- Confusing LIFO with FIFO
- Overflow in fixed-size array implementation

### 5. Algorithm Selection
- **Balanced parentheses**: Stack
- **Expression evaluation**: Stack
- **Backtracking**: Stack (or recursion)
- **DFS traversal**: Stack
- **Undo/Redo**: Stack

### Key Points to Remember
1. **LIFO**: Last In First Out
2. **Two main operations**: Push and Pop (both O(1))
3. **Top pointer**: Always points to last element
4. **No random access**: Can only access top
5. **Applications**: Recursion, parsing, backtracking
6. **Overflow**: In array-based implementation
7. **Underflow**: When popping from empty stack
8. **Expression evaluation**: Postfix uses one stack, infix to postfix uses stack for operators
