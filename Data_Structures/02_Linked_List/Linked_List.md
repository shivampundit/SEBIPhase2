# Linked List - Comprehensive Notes

## Overview
A Linked List is a linear data structure where elements (nodes) are stored in non-contiguous memory locations. Each node contains data and a reference (pointer) to the next node.

### Key Characteristics
1. **Dynamic size** - grows/shrinks at runtime
2. **Non-contiguous memory** allocation
3. **Sequential access** - O(n) time
4. **Easy insertion/deletion** - O(1) with node reference
5. **No random access** - must traverse from head

---

## Types of Linked Lists

### 1. Singly Linked List
Each node points to next node only.

```
[Data|Next] → [Data|Next] → [Data|Next] → NULL
```

### 2. Doubly Linked List
Each node has pointers to both next and previous nodes.

```
NULL ← [Prev|Data|Next] ↔ [Prev|Data|Next] ↔ [Prev|Data|Next] → NULL
```

### 3. Circular Linked List
Last node points back to first node.

```
[Data|Next] → [Data|Next] → [Data|Next] ⤴
      ↑_______________________________|
```

---

## Singly Linked List Implementation

### Node Structure

#### C
```c
#include <stdio.h>
#include <stdlib.h>

// Node structure
typedef struct Node {
    int data;
    struct Node* next;
} Node;

// Create a new node
Node* createNode(int data) {
    Node* newNode = (Node*)malloc(sizeof(Node));
    newNode->data = data;
    newNode->next = NULL;
    return newNode;
}

// Linked List structure
typedef struct {
    Node* head;
} LinkedList;

// Initialize linked list
LinkedList* createLinkedList() {
    LinkedList* list = (LinkedList*)malloc(sizeof(LinkedList));
    list->head = NULL;
    return list;
}
```

#### C++
```cpp
#include <iostream>
using namespace std;

// Node structure
struct Node {
    int data;
    Node* next;
    
    Node(int val) : data(val), next(nullptr) {}
};

// Linked List class
class LinkedList {
private:
    Node* head;
    
public:
    LinkedList() : head(nullptr) {}
    
    // Methods will be defined below
};
```

#### Java
```java
// Node class
class Node {
    int data;
    Node next;
    
    Node(int data) {
        this.data = data;
        this.next = null;
    }
}

// Linked List class
class LinkedList {
    private Node head;
    
    public LinkedList() {
        this.head = null;
    }
    
    // Methods will be defined below
}
```

#### Python
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None
```

### Basic Operations

#### 1. Insert at Beginning

##### C
```c
void insert_at_beginning(LinkedList* list, int data) {
    /*
     * Insert node at beginning
     * Time: O(1)
     * Space: O(1)
     */
    Node* newNode = createNode(data);
    newNode->next = list->head;
    list->head = newNode;
}

// Usage
LinkedList* ll = createLinkedList();
insert_at_beginning(ll, 3);
insert_at_beginning(ll, 2);
insert_at_beginning(ll, 1);
// List: 1 → 2 → 3 → NULL
```

##### C++
```cpp
void insertAtBeginning(int data) {
    /*
     * Insert node at beginning
     * Time: O(1)
     * Space: O(1)
     */
    Node* newNode = new Node(data);
    newNode->next = head;
    head = newNode;
}

// Usage
LinkedList ll;
ll.insertAtBeginning(3);
ll.insertAtBeginning(2);
ll.insertAtBeginning(1);
// List: 1 → 2 → 3 → NULL
```

##### Java
```java
public void insertAtBeginning(int data) {
    /*
     * Insert node at beginning
     * Time: O(1)
     * Space: O(1)
     */
    Node newNode = new Node(data);
    newNode.next = head;
    head = newNode;
}

// Usage
LinkedList ll = new LinkedList();
ll.insertAtBeginning(3);
ll.insertAtBeginning(2);
ll.insertAtBeginning(1);
// List: 1 → 2 → 3 → NULL
```

##### Python
```python
def insert_at_beginning(self, data):
    """
    Insert node at beginning
    Time: O(1)
    Space: O(1)
    """
    new_node = Node(data)
    new_node.next = self.head
    self.head = new_node

# Usage
ll = LinkedList()
ll.insert_at_beginning(3)
ll.insert_at_beginning(2)
ll.insert_at_beginning(1)
# List: 1 → 2 → 3 → NULL
```

#### 2. Insert at End
```python
def insert_at_end(self, data):
    """
    Insert node at end
    Time: O(n)
    Space: O(1)
    """
    new_node = Node(data)
    
    if not self.head:
        self.head = new_node
        return
    
    current = self.head
    while current.next:
        current = current.next
    
    current.next = new_node

# List: 1 → 2 → 3 → 4 → NULL
```

#### 3. Insert at Position
```python
def insert_at_position(self, data, position):
    """
    Insert at specific position (0-indexed)
    Time: O(n)
    Space: O(1)
    """
    new_node = Node(data)
    
    if position == 0:
        new_node.next = self.head
        self.head = new_node
        return
    
    current = self.head
    for _ in range(position - 1):
        if not current:
            return
        current = current.next
    
    if not current:
        return
    
    new_node.next = current.next
    current.next = new_node
```

#### 4. Delete from Beginning
```python
def delete_from_beginning(self):
    """
    Delete first node
    Time: O(1)
    Space: O(1)
    """
    if not self.head:
        return None
    
    deleted_data = self.head.data
    self.head = self.head.next
    return deleted_data
```

#### 5. Delete from End
```python
def delete_from_end(self):
    """
    Delete last node
    Time: O(n)
    Space: O(1)
    """
    if not self.head:
        return None
    
    if not self.head.next:
        deleted_data = self.head.data
        self.head = None
        return deleted_data
    
    current = self.head
    while current.next.next:
        current = current.next
    
    deleted_data = current.next.data
    current.next = None
    return deleted_data
```

#### 6. Delete by Value
```python
def delete_by_value(self, value):
    """
    Delete node with specific value
    Time: O(n)
    Space: O(1)
    """
    if not self.head:
        return False
    
    # If head node contains value
    if self.head.data == value:
        self.head = self.head.next
        return True
    
    current = self.head
    while current.next:
        if current.next.data == value:
            current.next = current.next.next
            return True
        current = current.next
    
    return False
```

#### 7. Search
```python
def search(self, value):
    """
    Search for value in list
    Time: O(n)
    Space: O(1)
    """
    current = self.head
    position = 0
    
    while current:
        if current.data == value:
            return position
        current = current.next
        position += 1
    
    return -1
```

#### 8. Display/Traverse
```python
def display(self):
    """
    Print all nodes
    Time: O(n)
    Space: O(1)
    """
    current = self.head
    elements = []
    
    while current:
        elements.append(str(current.data))
        current = current.next
    
    print(" → ".join(elements) + " → NULL")

#### 9. Length
```python
def length(self):
    """
    Get length of list
    Time: O(n)
    Space: O(1)
    """
    count = 0
    current = self.head
    
    while current:
        count += 1
        current = current.next
    
    return count
```

---

## Common Linked List Problems

### 1. Reverse Linked List

#### C
```c
void reverse(LinkedList* list) {
    /*
     * Reverse linked list
     * Time: O(n)
     * Space: O(1)
     */
    Node* prev = NULL;
    Node* current = list->head;
    Node* next = NULL;
    
    while (current != NULL) {
        next = current->next;
        current->next = prev;
        prev = current;
        current = next;
    }
    
    list->head = prev;
}

// Dry Run:
// Initial: 1 → 2 → 3 → NULL
// Step 1: NULL ← 1   2 → 3 → NULL
// Step 2: NULL ← 1 ← 2   3 → NULL
// Step 3: NULL ← 1 ← 2 ← 3
// Result: 3 → 2 → 1 → NULL
```

#### C++
```cpp
void reverse() {
    /*
     * Reverse linked list
     * Time: O(n)
     * Space: O(1)
     */
    Node* prev = nullptr;
    Node* current = head;
    Node* next = nullptr;
    
    while (current != nullptr) {
        next = current->next;
        current->next = prev;
        prev = current;
        current = next;
    }
    
    head = prev;
}

// Dry Run:
// Initial: 1 → 2 → 3 → NULL
// Step 1: NULL ← 1   2 → 3 → NULL
// Step 2: NULL ← 1 ← 2   3 → NULL
// Step 3: NULL ← 1 ← 2 ← 3
// Result: 3 → 2 → 1 → NULL
```

#### Java
```java
public void reverse() {
    /*
     * Reverse linked list
     * Time: O(n)
     * Space: O(1)
     */
    Node prev = null;
    Node current = head;
    Node next = null;
    
    while (current != null) {
        next = current.next;
        current.next = prev;
        prev = current;
        current = next;
    }
    
    head = prev;
}

// Dry Run:
// Initial: 1 → 2 → 3 → NULL
// Step 1: NULL ← 1   2 → 3 → NULL
// Step 2: NULL ← 1 ← 2   3 → NULL
// Step 3: NULL ← 1 ← 2 ← 3
// Result: 3 → 2 → 1 → NULL
```

#### Python
```python
def reverse(self):
    """
    Reverse linked list
    Time: O(n)
    Space: O(1)
    """
    prev = None
    current = self.head
    
    while current:
        next_node = current.next
        current.next = prev
        prev = current
        current = next_node
    
    self.head = prev

# Dry Run:
# Initial: 1 → 2 → 3 → NULL
# Step 1: NULL ← 1   2 → 3 → NULL
# Step 2: NULL ← 1 ← 2   3 → NULL
# Step 3: NULL ← 1 ← 2 ← 3
# Result: 3 → 2 → 1 → NULL
```

### 2. Detect Cycle (Floyd's Algorithm)

#### C
```c
int has_cycle(LinkedList* list) {
    /*
     * Detect cycle using Floyd's algorithm
     * Time: O(n)
     * Space: O(1)
     */
    if (list->head == NULL) {
        return 0;
    }
    
    Node* slow = list->head;
    Node* fast = list->head;
    
    while (fast != NULL && fast->next != NULL) {
        slow = slow->next;
        fast = fast->next->next;
        
        if (slow == fast) {
            return 1;  // Cycle detected
        }
    }
    
    return 0;  // No cycle
}
```

#### C++
```cpp
bool hasCycle() {
    /*
     * Detect cycle using Floyd's algorithm
     * Time: O(n)
     * Space: O(1)
     */
    if (head == nullptr) {
        return false;
    }
    
    Node* slow = head;
    Node* fast = head;
    
    while (fast != nullptr && fast->next != nullptr) {
        slow = slow->next;
        fast = fast->next->next;
        
        if (slow == fast) {
            return true;
        }
    }
    
    return false;
}
```

#### Java
```java
public boolean hasCycle() {
    /*
     * Detect cycle using Floyd's algorithm
     * Time: O(n)
     * Space: O(1)
     */
    if (head == null) {
        return false;
    }
    
    Node slow = head;
    Node fast = head;
    
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        
        if (slow == fast) {
            return true;
        }
    }
    
    return false;
}
```

#### Python
```python
def has_cycle(self):
    """
    Detect cycle using Floyd's algorithm
    Time: O(n)
    Space: O(1)
    """
    if not self.head:
        return False
    
    slow = fast = self.head
    
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        
        if slow == fast:
            return True
    
    return False
```

### 3. Find Middle Element
```python
def find_middle(self):
    """
    Find middle element using slow-fast pointers
    Time: O(n)
    Space: O(1)
    """
    if not self.head:
        return None
    
    slow = fast = self.head
    
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    
    return slow.data

# Example: 1 → 2 → 3 → 4 → 5
# When fast reaches end, slow is at middle (3)
```

### 4. Remove Nth Node from End
```python
def remove_nth_from_end(self, n):
    """
    Remove nth node from end
    Time: O(n)
    Space: O(1)
    """
    dummy = Node(0)
    dummy.next = self.head
    first = second = dummy
    
    # Move first n+1 steps ahead
    for _ in range(n + 1):
        if not first:
            return self.head
        first = first.next
    
    # Move both until first reaches end
    while first:
        first = first.next
        second = second.next
    
    # Remove nth node
    second.next = second.next.next
    self.head = dummy.next
```

### 5. Merge Two Sorted Lists
```python
def merge_sorted_lists(list1, list2):
    """
    Merge two sorted linked lists
    Time: O(m + n)
    Space: O(1)
    """
    dummy = Node(0)
    current = dummy
    
    while list1 and list2:
        if list1.data <= list2.data:
            current.next = list1
            list1 = list1.next
        else:
            current.next = list2
            list2 = list2.next
        current = current.next
    
    current.next = list1 if list1 else list2
    
    return dummy.next
```

### 6. Check if Palindrome
```python
def is_palindrome(self):
    """
    Check if linked list is palindrome
    Time: O(n)
    Space: O(1)
    """
    if not self.head or not self.head.next:
        return True
    
    # Find middle
    slow = fast = self.head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
    
    # Reverse second half
    prev = None
    current = slow
    while current:
        next_node = current.next
        current.next = prev
        prev = current
        current = next_node
    
    # Compare first and second half
    left, right = self.head, prev
    while right:
        if left.data != right.data:
            return False
        left = left.next
        right = right.next
    
    return True
```

### 7. Remove Duplicates from Sorted List
```python
def remove_duplicates_sorted(self):
    """
    Remove duplicates from sorted list
    Time: O(n)
    Space: O(1)
    """
    current = self.head
    
    while current and current.next:
        if current.data == current.next.data:
            current.next = current.next.next
        else:
            current = current.next
```

### 8. Intersection of Two Lists
```python
def get_intersection_node(head1, head2):
    """
    Find intersection point of two lists
    Time: O(m + n)
    Space: O(1)
    """
    if not head1 or not head2:
        return None
    
    ptr1, ptr2 = head1, head2
    
    while ptr1 != ptr2:
        ptr1 = ptr1.next if ptr1 else head2
        ptr2 = ptr2.next if ptr2 else head1
    
    return ptr1
```

---

## Doubly Linked List

### Node Structure
```python
class DNode:
    def __init__(self, data):
        self.data = data
        self.prev = None
        self.next = None

class DoublyLinkedList:
    def __init__(self):
        self.head = None
```

### Operations

#### Insert at Beginning
```python
def insert_at_beginning(self, data):
    """
    Insert at beginning
    Time: O(1)
    """
    new_node = DNode(data)
    new_node.next = self.head
    
    if self.head:
        self.head.prev = new_node
    
    self.head = new_node
```

#### Insert at End
```python
def insert_at_end(self, data):
    """
    Insert at end
    Time: O(n)
    """
    new_node = DNode(data)
    
    if not self.head:
        self.head = new_node
        return
    
    current = self.head
    while current.next:
        current = current.next
    
    current.next = new_node
    new_node.prev = current
```

#### Delete Node
```python
def delete_node(self, node):
    """
    Delete given node
    Time: O(1) with node reference
    """
    if not node:
        return
    
    if node.prev:
        node.prev.next = node.next
    else:
        self.head = node.next
    
    if node.next:
        node.next.prev = node.prev
```

---

## Circular Linked List

### Check if Circular
```python
def is_circular(head):
    """
    Check if list is circular
    Time: O(n)
    Space: O(1)
    """
    if not head:
        return False
    
    current = head.next
    
    while current and current != head:
        current = current.next
    
    return current == head
```

---

## Linked List vs Array

| Feature | Array | Linked List |
|---------|-------|-------------|
| Size | Fixed | Dynamic |
| Memory | Contiguous | Non-contiguous |
| Access | O(1) | O(n) |
| Insert/Delete (beginning) | O(n) | O(1) |
| Insert/Delete (end) | O(1) | O(n)* |
| Space | No overhead | Extra space for pointers |
| Cache | Cache-friendly | Cache-unfriendly |

*O(1) if tail pointer is maintained

---

## Time Complexity Summary

| Operation | Singly LL | Doubly LL |
|-----------|-----------|-----------|
| Insert at beginning | O(1) | O(1) |
| Insert at end | O(n) | O(n)† |
| Insert at position | O(n) | O(n) |
| Delete from beginning | O(1) | O(1) |
| Delete from end | O(n) | O(n)† |
| Delete with reference | O(n) | O(1) |
| Search | O(n) | O(n) |
| Access | O(n) | O(n) |

†O(1) with tail pointer

---

## Exam Tips

### 1. Common MCQ Topics
- **Advantages of Linked List**: Dynamic size, easy insertion/deletion
- **Disadvantages**: No random access, extra memory for pointers
- **When to use**: Frequent insertions/deletions, unknown size
- **When not to use**: Need random access, memory is limited

### 2. Dry Run Practice
```
Reverse: 1 → 2 → 3 → NULL

Step 1: prev=NULL, current=1, next=2
        NULL ← 1   2 → 3 → NULL
        
Step 2: prev=1, current=2, next=3
        NULL ← 1 ← 2   3 → NULL
        
Step 3: prev=2, current=3, next=NULL
        NULL ← 1 ← 2 ← 3
        
Result: 3 → 2 → 1 → NULL
```

### 3. Debugging Questions
```python
# Bug: Lost reference
def insert_at_end_wrong(self, data):
    new_node = Node(data)
    current = self.head
    while current:  # Bug: infinite loop
        current = current.next
    current.next = new_node  # Bug: current is None

# Fix
def insert_at_end_correct(self, data):
    new_node = Node(data)
    if not self.head:
        self.head = new_node
        return
    current = self.head
    while current.next:  # Correct
        current = current.next
    current.next = new_node
```

### 4. Algorithm Patterns
- **Two pointers**: Slow-fast for cycle detection, middle finding
- **Dummy node**: Simplifies edge cases in merge, deletion
- **Recursion**: Natural for reversal, traversal
- **In-place**: Most operations can be done without extra space

### Key Points to Remember
1. **Head pointer**: Always maintain reference to first node
2. **NULL check**: Always check for NULL/None before accessing
3. **Edge cases**: Empty list, single node, two nodes
4. **Memory**: Remember to free deleted nodes (in languages with manual memory management)
5. **Traversal**: Must traverse from head, no random access
6. **Insertion**: Easy with node reference, otherwise O(n)
7. **Circular**: Last node points to head
8. **Doubly**: Can traverse backward
