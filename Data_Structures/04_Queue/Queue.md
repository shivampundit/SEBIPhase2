# Queue - Comprehensive Notes

## Overview
A Queue is a linear data structure that follows the **FIFO (First In First Out)** principle. The first element added is the first one to be removed.

### Real-World Analogies
- Line at ticket counter
- Print queue
- CPU task scheduling
- Call center systems

### Key Characteristics
1. **FIFO principle**: First In First Out
2. **Two ends**: Front (dequeue) and Rear (enqueue)
3. **Two main operations**: Enqueue and Dequeue
4. **Linear structure**: Sequential access

---

## Queue Operations

### Primary Operations
1. **Enqueue**: Add element at rear - O(1)
2. **Dequeue**: Remove element from front - O(1)
3. **Front/Peek**: View front element - O(1)
4. **IsEmpty**: Check if queue is empty - O(1)
5. **IsFull**: Check if queue is full - O(1) (array-based)
6. **Size**: Get number of elements - O(1)

---

## Types of Queues

### 1. Simple Queue (Linear Queue)
Basic FIFO queue

### 2. Circular Queue
Rear connects to front when end is reached

### 3. Priority Queue
Elements have priorities, highest priority dequeued first

### 4. Double-ended Queue (Deque)
Insert and delete from both ends

---

## Implementation Methods

## 1. Array-based Queue

### Simple Queue
```python
class ArrayQueue:
    def __init__(self, capacity=100):
        """
        Initialize queue with fixed capacity
        """
        self.capacity = capacity
        self.queue = [None] * capacity
        self.front = 0
        self.rear = -1
        self.size = 0
    
    def enqueue(self, data):
        """
        Add element to rear
        Time: O(1)
        """
        if self.is_full():
            raise OverflowError("Queue is full")
        
        self.rear = (self.rear + 1) % self.capacity
        self.queue[self.rear] = data
        self.size += 1
    
    def dequeue(self):
        """
        Remove element from front
        Time: O(1)
        """
        if self.is_empty():
            raise IndexError("Queue is empty")
        
        data = self.queue[self.front]
        self.queue[self.front] = None
        self.front = (self.front + 1) % self.capacity
        self.size -= 1
        return data
    
    def peek(self):
        """
        View front element
        Time: O(1)
        """
        if self.is_empty():
            raise IndexError("Queue is empty")
        
        return self.queue[self.front]
    
    def is_empty(self):
        """Check if queue is empty"""
        return self.size == 0
    
    def is_full(self):
        """Check if queue is full"""
        return self.size == self.capacity
    
    def get_size(self):
        """Return queue size"""
        return self.size
    
    def display(self):
        """Display queue elements"""
        if self.is_empty():
            print("Queue is empty")
            return
        
        print("Queue (front to rear):")
        i = self.front
        for _ in range(self.size):
            print(f"[{self.queue[i]}]", end=" ")
            i = (i + 1) % self.capacity
        print()

# Example Usage
queue = ArrayQueue(5)
queue.enqueue(10)
queue.enqueue(20)
queue.enqueue(30)
queue.display()  # [10] [20] [30]
print(f"Dequeue: {queue.dequeue()}")  # 10
print(f"Front: {queue.peek()}")  # 20
```

### Dry Run Example
```
Operations: enqueue(5), enqueue(3), dequeue(), enqueue(7), peek()

Initial: [] (capacity=5)
front=0, rear=-1, size=0

enqueue(5):
Queue: [5, _, _, _, _]
front=0, rear=0, size=1

enqueue(3):
Queue: [5, 3, _, _, _]
front=0, rear=1, size=2

dequeue():
Returns: 5
Queue: [_, 3, _, _, _]
front=1, rear=1, size=1

enqueue(7):
Queue: [_, 3, 7, _, _]
front=1, rear=2, size=2

peek():
Returns: 3 (no change to queue)
```

---

## 2. Circular Queue

```python
class CircularQueue:
    def __init__(self, capacity):
        """
        Initialize circular queue
        """
        self.capacity = capacity
        self.queue = [None] * capacity
        self.front = -1
        self.rear = -1
    
    def enqueue(self, data):
        """
        Add element to circular queue
        Time: O(1)
        """
        if self.is_full():
            raise OverflowError("Queue is full")
        
        if self.front == -1:
            self.front = 0
        
        self.rear = (self.rear + 1) % self.capacity
        self.queue[self.rear] = data
    
    def dequeue(self):
        """
        Remove element from front
        Time: O(1)
        """
        if self.is_empty():
            raise IndexError("Queue is empty")
        
        data = self.queue[self.front]
        
        if self.front == self.rear:  # Last element
            self.front = self.rear = -1
        else:
            self.front = (self.front + 1) % self.capacity
        
        return data
    
    def is_empty(self):
        """Check if empty"""
        return self.front == -1
    
    def is_full(self):
        """Check if full"""
        return (self.rear + 1) % self.capacity == self.front
    
    def display(self):
        """Display queue"""
        if self.is_empty():
            print("Queue is empty")
            return
        
        print("Circular Queue:")
        i = self.front
        while True:
            print(f"[{self.queue[i]}]", end=" ")
            if i == self.rear:
                break
            i = (i + 1) % self.capacity
        print()

# Example
cq = CircularQueue(5)
cq.enqueue(1)
cq.enqueue(2)
cq.enqueue(3)
cq.dequeue()
cq.enqueue(4)
cq.enqueue(5)
cq.display()
```

### Circular Queue Dry Run
```
Capacity = 5
Operations: enqueue(1), enqueue(2), enqueue(3), dequeue(), enqueue(4), enqueue(5), enqueue(6)

Initial:
[_, _, _, _, _]
front=-1, rear=-1

enqueue(1):
[1, _, _, _, _]
front=0, rear=0

enqueue(2):
[1, 2, _, _, _]
front=0, rear=1

enqueue(3):
[1, 2, 3, _, _]
front=0, rear=2

dequeue():
Returns 1
[_, 2, 3, _, _]
front=1, rear=2

enqueue(4):
[_, 2, 3, 4, _]
front=1, rear=3

enqueue(5):
[_, 2, 3, 4, 5]
front=1, rear=4

enqueue(6):
[6, 2, 3, 4, 5]  (rear wraps to 0)
front=1, rear=0

Queue is now full!
```

---

## 3. Linked List-based Queue

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedQueue:
    def __init__(self):
        """
        Initialize empty queue
        """
        self.front = None
        self.rear = None
        self.size = 0
    
    def enqueue(self, data):
        """
        Add element to rear
        Time: O(1)
        """
        new_node = Node(data)
        
        if self.rear is None:
            self.front = self.rear = new_node
        else:
            self.rear.next = new_node
            self.rear = new_node
        
        self.size += 1
    
    def dequeue(self):
        """
        Remove element from front
        Time: O(1)
        """
        if self.is_empty():
            raise IndexError("Queue is empty")
        
        data = self.front.data
        self.front = self.front.next
        
        if self.front is None:
            self.rear = None
        
        self.size -= 1
        return data
    
    def peek(self):
        """View front element"""
        if self.is_empty():
            raise IndexError("Queue is empty")
        
        return self.front.data
    
    def is_empty(self):
        """Check if empty"""
        return self.front is None
    
    def get_size(self):
        """Get queue size"""
        return self.size
    
    def display(self):
        """Display all elements"""
        if self.is_empty():
            print("Queue is empty")
            return
        
        current = self.front
        print("Queue (front to rear):")
        while current:
            print(f"[{current.data}]", end=" → ")
            current = current.next
        print("NULL")

# Example
lq = LinkedQueue()
lq.enqueue(10)
lq.enqueue(20)
lq.enqueue(30)
lq.display()
print(f"Dequeue: {lq.dequeue()}")
```

---

## 4. Priority Queue

```python
import heapq

class PriorityQueue:
    def __init__(self):
        """
        Initialize priority queue using min-heap
        """
        self.heap = []
        self.index = 0
    
    def enqueue(self, item, priority):
        """
        Add item with priority
        Time: O(log n)
        """
        heapq.heappush(self.heap, (priority, self.index, item))
        self.index += 1
    
    def dequeue(self):
        """
        Remove highest priority item
        Time: O(log n)
        """
        if self.is_empty():
            raise IndexError("Queue is empty")
        
        return heapq.heappop(self.heap)[2]
    
    def peek(self):
        """View highest priority item"""
        if self.is_empty():
            raise IndexError("Queue is empty")
        
        return self.heap[0][2]
    
    def is_empty(self):
        """Check if empty"""
        return len(self.heap) == 0
    
    def size(self):
        """Get size"""
        return len(self.heap)

# Example
pq = PriorityQueue()
pq.enqueue("Task1", 3)
pq.enqueue("Task2", 1)  # Higher priority (lower number)
pq.enqueue("Task3", 2)

print(pq.dequeue())  # Task2 (priority 1)
print(pq.dequeue())  # Task3 (priority 2)
print(pq.dequeue())  # Task1 (priority 3)
```

---

## 5. Deque (Double-ended Queue)

```python
from collections import deque

class Deque:
    def __init__(self):
        """Initialize deque"""
        self.items = deque()
    
    def add_front(self, item):
        """Add to front - O(1)"""
        self.items.appendleft(item)
    
    def add_rear(self, item):
        """Add to rear - O(1)"""
        self.items.append(item)
    
    def remove_front(self):
        """Remove from front - O(1)"""
        if self.is_empty():
            raise IndexError("Deque is empty")
        return self.items.popleft()
    
    def remove_rear(self):
        """Remove from rear - O(1)"""
        if self.is_empty():
            raise IndexError("Deque is empty")
        return self.items.pop()
    
    def peek_front(self):
        """View front"""
        return self.items[0] if self.items else None
    
    def peek_rear(self):
        """View rear"""
        return self.items[-1] if self.items else None
    
    def is_empty(self):
        """Check if empty"""
        return len(self.items) == 0
    
    def size(self):
        """Get size"""
        return len(self.items)

# Example
dq = Deque()
dq.add_rear(1)
dq.add_rear(2)
dq.add_front(3)
print(dq.items)  # deque([3, 1, 2])
```

---

## Classic Queue Problems

### 1. Implement Stack using Queues

```python
from collections import deque

class StackUsingQueues:
    def __init__(self):
        """
        Implement stack using two queues
        """
        self.q1 = deque()
        self.q2 = deque()
    
    def push(self, x):
        """
        Push element
        Time: O(n)
        """
        self.q2.append(x)
        
        while self.q1:
            self.q2.append(self.q1.popleft())
        
        self.q1, self.q2 = self.q2, self.q1
    
    def pop(self):
        """
        Pop element
        Time: O(1)
        """
        if not self.q1:
            raise IndexError("Stack is empty")
        
        return self.q1.popleft()
    
    def top(self):
        """Get top element"""
        return self.q1[0] if self.q1 else None
    
    def empty(self):
        """Check if empty"""
        return len(self.q1) == 0
```

---

### 2. Generate Binary Numbers

```python
from collections import deque

def generate_binary_numbers(n):
    """
    Generate binary numbers from 1 to n
    Time: O(n)
    Space: O(n)
    """
    result = []
    queue = deque(["1"])
    
    for _ in range(n):
        front = queue.popleft()
        result.append(front)
        
        queue.append(front + "0")
        queue.append(front + "1")
    
    return result

# Example
print(generate_binary_numbers(10))
# ['1', '10', '11', '100', '101', '110', '111', '1000', '1001', '1010']
```

#### Dry Run
```
n = 5

Initial: queue = ['1']

i=0:
  front = '1', result = ['1']
  queue = ['10', '11']

i=1:
  front = '10', result = ['1', '10']
  queue = ['11', '100', '101']

i=2:
  front = '11', result = ['1', '10', '11']
  queue = ['100', '101', '110', '111']

i=3:
  front = '100', result = ['1', '10', '11', '100']
  queue = ['101', '110', '111', '1000', '1001']

i=4:
  front = '101', result = ['1', '10', '11', '100', '101']

Final: ['1', '10', '11', '100', '101']
```

---

### 3. First Non-repeating Character in Stream

```python
from collections import deque, defaultdict

def first_non_repeating(stream):
    """
    Find first non-repeating character at each step
    Time: O(n)
    """
    freq = defaultdict(int)
    queue = deque()
    result = []
    
    for char in stream:
        freq[char] += 1
        queue.append(char)
        
        while queue and freq[queue[0]] > 1:
            queue.popleft()
        
        result.append(queue[0] if queue else '#')
    
    return result

# Example
stream = "aabccxb"
print(first_non_repeating(stream))  # ['a', '#', 'b', 'b', 'b', 'x', 'x']
```

---

### 4. Sliding Window Maximum

```python
from collections import deque

def max_sliding_window(nums, k):
    """
    Find maximum in each sliding window
    Time: O(n)
    Space: O(k)
    """
    deq = deque()
    result = []
    
    for i in range(len(nums)):
        # Remove elements outside window
        if deq and deq[0] < i - k + 1:
            deq.popleft()
        
        # Remove smaller elements
        while deq and nums[deq[-1]] < nums[i]:
            deq.pop()
        
        deq.append(i)
        
        if i >= k - 1:
            result.append(nums[deq[0]])
    
    return result

# Example
nums = [1, 3, -1, -3, 5, 3, 6, 7]
k = 3
print(max_sliding_window(nums, k))  # [3, 3, 5, 5, 6, 7]
```

---

### 5. Rotten Oranges (BFS)

```python
from collections import deque

def oranges_rotting(grid):
    """
    Find minimum time to rot all oranges
    Time: O(m*n)
    Space: O(m*n)
    """
    rows, cols = len(grid), len(grid[0])
    queue = deque()
    fresh = 0
    
    # Count fresh oranges and add rotten to queue
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == 2:
                queue.append((r, c, 0))
            elif grid[r][c] == 1:
                fresh += 1
    
    directions = [(0, 1), (1, 0), (0, -1), (-1, 0)]
    max_time = 0
    
    while queue:
        r, c, time = queue.popleft()
        max_time = max(max_time, time)
        
        for dr, dc in directions:
            nr, nc = r + dr, c + dc
            
            if (0 <= nr < rows and 0 <= nc < cols and
                grid[nr][nc] == 1):
                grid[nr][nc] = 2
                fresh -= 1
                queue.append((nr, nc, time + 1))
    
    return max_time if fresh == 0 else -1

# Example
grid = [
    [2, 1, 1],
    [1, 1, 0],
    [0, 1, 1]
]
print(oranges_rotting(grid))  # 4
```

---

## Applications of Queue

### 1. CPU Scheduling
```python
class Process:
    def __init__(self, pid, burst_time):
        self.pid = pid
        self.burst_time = burst_time

def round_robin_scheduling(processes, time_quantum):
    """
    Round Robin CPU scheduling
    """
    queue = deque(processes)
    time = 0
    
    while queue:
        process = queue.popleft()
        
        if process.burst_time <= time_quantum:
            time += process.burst_time
            print(f"Process {process.pid} completed at time {time}")
        else:
            time += time_quantum
            process.burst_time -= time_quantum
            queue.append(process)
```

### 2. Breadth-First Search (BFS)
```python
def bfs(graph, start):
    """
    BFS traversal using queue
    """
    visited = set()
    queue = deque([start])
    visited.add(start)
    
    while queue:
        vertex = queue.popleft()
        print(vertex, end=" ")
        
        for neighbor in graph[vertex]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
```

---

## Queue vs Stack

| Feature | Queue | Stack |
|---------|-------|-------|
| Principle | FIFO | LIFO |
| Operations | Enqueue, Dequeue | Push, Pop |
| Access | Front and Rear | Top only |
| Use case | Scheduling, BFS | Recursion, DFS |
| Real-world | Line at counter | Stack of plates |

---

## Time Complexity Summary

| Operation | Array Queue | Linked Queue | Priority Queue |
|-----------|-------------|--------------|----------------|
| Enqueue | O(1) | O(1) | O(log n) |
| Dequeue | O(1) | O(1) | O(log n) |
| Peek | O(1) | O(1) | O(1) |
| IsEmpty | O(1) | O(1) | O(1) |
| Search | O(n) | O(n) | O(n) |

---

## Exam Tips

### 1. MCQ Topics
**Q**: Which follows FIFO?
**A**: Queue

**Q**: Circular queue advantage?
**A**: Efficient space utilization

**Q**: BFS uses which data structure?
**A**: Queue

### 2. Code Completion
```python
def enqueue(self, data):
    new_node = Node(data)
    if self.rear:
        self.rear.next = new_node  # Link to new node
    else:
        self.front = new_node      # First element
    self.rear = new_node           # Update rear
```

### 3. Common Mistakes
- Not handling empty queue in dequeue
- Forgetting to update front/rear pointers
- Confusing FIFO with LIFO
- Circular queue full condition: `(rear+1) % capacity == front`

### Key Points
1. **FIFO**: First In First Out
2. **Two pointers**: Front and Rear
3. **Applications**: Scheduling, BFS, buffering
4. **Circular queue**: Better space utilization
5. **Priority queue**: Based on priority, not FIFO
6. **Deque**: Insert/delete from both ends
7. **Time complexity**: O(1) for enqueue/dequeue in simple queue
