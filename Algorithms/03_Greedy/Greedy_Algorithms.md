# Greedy Algorithms - Comprehensive Notes

## Overview
Greedy algorithms make locally optimal choices at each step with the hope of finding a global optimum. They are characterized by:
- Making the best choice at each step
- Never reconsidering previous choices
- Fast and simple to implement
- Don't always guarantee optimal solution

---

## Key Characteristics

### 1. Greedy Choice Property
A global optimum can be arrived at by making locally optimal choices.

### 2. Optimal Substructure
An optimal solution contains optimal solutions to subproblems.

---

## Classic Greedy Problems

## 1. Activity Selection Problem

### Problem Statement
Given n activities with start and finish times, select maximum number of non-overlapping activities.

### Greedy Strategy
Select activities in order of their finishing times. Always pick the next activity that finishes first.

### Implementation (Python)
```python
def activity_selection(start, finish):
    """
    Select maximum number of non-overlapping activities
    Input: start times and finish times
    Output: list of selected activity indices
    """
    # Sort activities by finish time
    activities = [(finish[i], start[i], i) for i in range(len(start))]
    activities.sort()
    
    selected = []
    last_finish = 0
    
    for f, s, idx in activities:
        if s >= last_finish:  # Non-overlapping
            selected.append(idx)
            last_finish = f
    
    return selected

# Example
start = [1, 3, 0, 5, 8, 5]
finish = [2, 4, 6, 7, 9, 9]
result = activity_selection(start, finish)
print(f"Selected activities: {result}")
print(f"Maximum activities: {len(result)}")
```

### Implementation (C)
```c
#include <stdio.h>
#include <stdlib.h>

typedef struct {
    int finish;
    int start;
    int index;
} Activity;

// Comparison function for qsort
int compare(const void *a, const void *b) {
    Activity *act1 = (Activity *)a;
    Activity *act2 = (Activity *)b;
    return act1->finish - act2->finish;
}

void activity_selection(int start[], int finish[], int n) {
    // Create array of activities
    Activity *activities = (Activity *)malloc(n * sizeof(Activity));
    
    for (int i = 0; i < n; i++) {
        activities[i].finish = finish[i];
        activities[i].start = start[i];
        activities[i].index = i;
    }
    
    // Sort activities by finish time
    qsort(activities, n, sizeof(Activity), compare);
    
    // Select activities
    int selected[n];
    int count = 0;
    int last_finish = 0;
    
    for (int i = 0; i < n; i++) {
        if (activities[i].start >= last_finish) {
            selected[count++] = activities[i].index;
            last_finish = activities[i].finish;
        }
    }
    
    printf("Selected activities: ");
    for (int i = 0; i < count; i++) {
        printf("%d ", selected[i]);
    }
    printf("\nMaximum activities: %d\n", count);
    
    free(activities);
}

// Example usage
int main() {
    int start[] = {1, 3, 0, 5, 8, 5};
    int finish[] = {2, 4, 6, 7, 9, 9};
    int n = 6;
    
    activity_selection(start, finish, n);
    return 0;
}
```

### Implementation (C++)
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

struct Activity {
    int finish;
    int start;
    int index;
    
    // Comparison operator for sorting
    bool operator<(const Activity& other) const {
        return finish < other.finish;
    }
};

vector<int> activitySelection(vector<int>& start, vector<int>& finish) {
    int n = start.size();
    vector<Activity> activities(n);
    
    // Create activity objects
    for (int i = 0; i < n; i++) {
        activities[i] = {finish[i], start[i], i};
    }
    
    // Sort by finish time
    sort(activities.begin(), activities.end());
    
    vector<int> selected;
    int lastFinish = 0;
    
    for (const auto& act : activities) {
        if (act.start >= lastFinish) {
            selected.push_back(act.index);
            lastFinish = act.finish;
        }
    }
    
    return selected;
}

// Example usage
int main() {
    vector<int> start = {1, 3, 0, 5, 8, 5};
    vector<int> finish = {2, 4, 6, 7, 9, 9};
    
    vector<int> result = activitySelection(start, finish);
    
    cout << "Selected activities: ";
    for (int idx : result) {
        cout << idx << " ";
    }
    cout << "\nMaximum activities: " << result.size() << endl;
    
    return 0;
}
```

### Implementation (Java)
```java
import java.util.*;

class Activity implements Comparable<Activity> {
    int finish;
    int start;
    int index;
    
    Activity(int finish, int start, int index) {
        this.finish = finish;
        this.start = start;
        this.index = index;
    }
    
    @Override
    public int compareTo(Activity other) {
        return this.finish - other.finish;
    }
}

public class ActivitySelection {
    public static List<Integer> activitySelection(int[] start, int[] finish) {
        int n = start.length;
        Activity[] activities = new Activity[n];
        
        // Create activity objects
        for (int i = 0; i < n; i++) {
            activities[i] = new Activity(finish[i], start[i], i);
        }
        
        // Sort by finish time
        Arrays.sort(activities);
        
        List<Integer> selected = new ArrayList<>();
        int lastFinish = 0;
        
        for (Activity act : activities) {
            if (act.start >= lastFinish) {
                selected.add(act.index);
                lastFinish = act.finish;
            }
        }
        
        return selected;
    }
    
    // Example usage
    public static void main(String[] args) {
        int[] start = {1, 3, 0, 5, 8, 5};
        int[] finish = {2, 4, 6, 7, 9, 9};
        
        List<Integer> result = activitySelection(start, finish);
        
        System.out.print("Selected activities: ");
        for (int idx : result) {
            System.out.print(idx + " ");
        }
        System.out.println("\nMaximum activities: " + result.size());
    }
}
```

### Dry Run Example
```
Activities: [(Start, Finish)]
A1: (1, 2)
A2: (3, 4)
A3: (0, 6)
A4: (5, 7)
A5: (8, 9)
A6: (5, 9)

After sorting by finish time:
A1: (1, 2) ✓ Selected (last_finish = 2)
A2: (3, 4) ✓ Selected (3 >= 2, last_finish = 4)
A3: (0, 6) ✗ Rejected (0 < 4)
A4: (5, 7) ✓ Selected (5 >= 4, last_finish = 7)
A5: (8, 9) ✓ Selected (8 >= 7, last_finish = 9)
A6: (5, 9) ✗ Rejected (5 < 7)

Selected: A1, A2, A4, A5
Maximum: 4 activities
```

### Complexity
- **Time**: O(n log n) - sorting
- **Space**: O(n)

---

## 2. Fractional Knapsack

### Problem Statement
Given weights and values of n items, and a knapsack with capacity W, maximize the value by taking fractions of items.

### Greedy Strategy
Sort items by value-to-weight ratio in descending order. Pick items with highest ratio first.

### Implementation (Python)
```python
def fractional_knapsack(values, weights, capacity):
    """
    Fractional knapsack using greedy approach
    """
    n = len(values)
    
    # Calculate value-to-weight ratio
    items = [(values[i]/weights[i], values[i], weights[i], i) 
             for i in range(n)]
    items.sort(reverse=True)  # Sort by ratio descending
    
    total_value = 0
    fractions = [0] * n
    
    for ratio, value, weight, idx in items:
        if capacity >= weight:
            # Take full item
            fractions[idx] = 1.0
            total_value += value
            capacity -= weight
        else:
            # Take fraction of item
            fractions[idx] = capacity / weight
            total_value += value * (capacity / weight)
            break
    
    return total_value, fractions

# Example
values = [60, 100, 120]
weights = [10, 20, 30]
capacity = 50

max_value, fractions = fractional_knapsack(values, weights, capacity)
print(f"Maximum value: {max_value}")
print(f"Fractions taken: {fractions}")
```

### Implementation (C)
```c
#include <stdio.h>
#include <stdlib.h>

typedef struct {
    double ratio;
    int value;
    int weight;
    int index;
} Item;

// Comparison function for descending order
int compare(const void *a, const void *b) {
    Item *item1 = (Item *)a;
    Item *item2 = (Item *)b;
    if (item2->ratio > item1->ratio) return 1;
    if (item2->ratio < item1->ratio) return -1;
    return 0;
}

double fractional_knapsack(int values[], int weights[], int n, int capacity) {
    // Create items with value-to-weight ratio
    Item *items = (Item *)malloc(n * sizeof(Item));
    
    for (int i = 0; i < n; i++) {
        items[i].ratio = (double)values[i] / weights[i];
        items[i].value = values[i];
        items[i].weight = weights[i];
        items[i].index = i;
    }
    
    // Sort by ratio in descending order
    qsort(items, n, sizeof(Item), compare);
    
    double total_value = 0.0;
    double fractions[n];
    for (int i = 0; i < n; i++) fractions[i] = 0.0;
    
    for (int i = 0; i < n; i++) {
        int idx = items[i].index;
        
        if (capacity >= items[i].weight) {
            // Take full item
            fractions[idx] = 1.0;
            total_value += items[i].value;
            capacity -= items[i].weight;
        } else {
            // Take fraction of item
            fractions[idx] = (double)capacity / items[i].weight;
            total_value += items[i].value * fractions[idx];
            break;
        }
    }
    
    printf("Maximum value: %.2f\n", total_value);
    printf("Fractions taken: ");
    for (int i = 0; i < n; i++) {
        printf("%.3f ", fractions[i]);
    }
    printf("\n");
    
    free(items);
    return total_value;
}

// Example usage
int main() {
    int values[] = {60, 100, 120};
    int weights[] = {10, 20, 30};
    int capacity = 50;
    int n = 3;
    
    fractional_knapsack(values, weights, n, capacity);
    return 0;
}
```

### Implementation (C++)
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

struct Item {
    double ratio;
    int value;
    int weight;
    int index;
    
    bool operator>(const Item& other) const {
        return ratio > other.ratio;
    }
};

pair<double, vector<double>> fractionalKnapsack(vector<int>& values, 
                                                  vector<int>& weights, 
                                                  int capacity) {
    int n = values.size();
    vector<Item> items(n);
    
    // Calculate ratio and create items
    for (int i = 0; i < n; i++) {
        items[i] = {(double)values[i] / weights[i], values[i], weights[i], i};
    }
    
    // Sort by ratio in descending order
    sort(items.begin(), items.end(), greater<Item>());
    
    double totalValue = 0.0;
    vector<double> fractions(n, 0.0);
    
    for (const auto& item : items) {
        if (capacity >= item.weight) {
            // Take full item
            fractions[item.index] = 1.0;
            totalValue += item.value;
            capacity -= item.weight;
        } else {
            // Take fraction of item
            fractions[item.index] = (double)capacity / item.weight;
            totalValue += item.value * fractions[item.index];
            break;
        }
    }
    
    return {totalValue, fractions};
}

// Example usage
int main() {
    vector<int> values = {60, 100, 120};
    vector<int> weights = {10, 20, 30};
    int capacity = 50;
    
    auto [maxValue, fractions] = fractionalKnapsack(values, weights, capacity);
    
    cout << "Maximum value: " << maxValue << endl;
    cout << "Fractions taken: ";
    for (double f : fractions) {
        cout << f << " ";
    }
    cout << endl;
    
    return 0;
}
```

### Implementation (Java)
```java
import java.util.*;

class Item implements Comparable<Item> {
    double ratio;
    int value;
    int weight;
    int index;
    
    Item(double ratio, int value, int weight, int index) {
        this.ratio = ratio;
        this.value = value;
        this.weight = weight;
        this.index = index;
    }
    
    @Override
    public int compareTo(Item other) {
        return Double.compare(other.ratio, this.ratio); // Descending order
    }
}

public class FractionalKnapsack {
    public static class Result {
        double totalValue;
        double[] fractions;
        
        Result(double totalValue, double[] fractions) {
            this.totalValue = totalValue;
            this.fractions = fractions;
        }
    }
    
    public static Result fractionalKnapsack(int[] values, int[] weights, int capacity) {
        int n = values.length;
        Item[] items = new Item[n];
        
        // Create items with ratio
        for (int i = 0; i < n; i++) {
            double ratio = (double) values[i] / weights[i];
            items[i] = new Item(ratio, values[i], weights[i], i);
        }
        
        // Sort by ratio in descending order
        Arrays.sort(items);
        
        double totalValue = 0.0;
        double[] fractions = new double[n];
        
        for (Item item : items) {
            if (capacity >= item.weight) {
                // Take full item
                fractions[item.index] = 1.0;
                totalValue += item.value;
                capacity -= item.weight;
            } else {
                // Take fraction of item
                fractions[item.index] = (double) capacity / item.weight;
                totalValue += item.value * fractions[item.index];
                break;
            }
        }
        
        return new Result(totalValue, fractions);
    }
    
    // Example usage
    public static void main(String[] args) {
        int[] values = {60, 100, 120};
        int[] weights = {10, 20, 30};
        int capacity = 50;
        
        Result result = fractionalKnapsack(values, weights, capacity);
        
        System.out.println("Maximum value: " + result.totalValue);
        System.out.print("Fractions taken: ");
        for (double f : result.fractions) {
            System.out.print(f + " ");
        }
        System.out.println();
    }
}
```

### Dry Run Example
```
Items: [(Value, Weight, Ratio)]
I1: (60, 10, 6.0)
I2: (100, 20, 5.0)
I3: (120, 30, 4.0)
Capacity: 50

After sorting by ratio:
I1: ratio=6.0, weight=10
    Take full: capacity=40, value=60

I2: ratio=5.0, weight=20
    Take full: capacity=20, value=160

I3: ratio=4.0, weight=30
    Take 20/30 fraction: capacity=0, value=160+80=240

Maximum Value: 240
Fractions: [1.0, 1.0, 0.667]
```

### Complexity
- **Time**: O(n log n)
- **Space**: O(n)

---

## 3. Job Sequencing Problem

### Problem Statement
Given jobs with deadlines and profits, schedule jobs to maximize profit (one job per unit time).

### Greedy Strategy
Sort jobs by profit in descending order. Assign each job to the latest available slot before its deadline.

### Implementation (Python)
```python
def job_sequencing(jobs, n_slots):
    """
    Job sequencing to maximize profit
    jobs: list of (id, deadline, profit)
    """
    # Sort jobs by profit (descending)
    jobs.sort(key=lambda x: x[2], reverse=True)
    
    # Track free slots
    slots = [-1] * n_slots
    total_profit = 0
    job_sequence = []
    
    for job_id, deadline, profit in jobs:
        # Find latest available slot before deadline
        for slot in range(min(n_slots, deadline) - 1, -1, -1):
            if slots[slot] == -1:
                slots[slot] = job_id
                total_profit += profit
                job_sequence.append(job_id)
                break
    
    return total_profit, job_sequence

# Example
jobs = [
    ('J1', 2, 100),
    ('J2', 1, 19),
    ('J3', 2, 27),
    ('J4', 1, 25),
    ('J5', 3, 15)
]

profit, sequence = job_sequencing(jobs, 3)
print(f"Maximum profit: {profit}")
print(f"Job sequence: {sequence}")
```

### Implementation (C)
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct {
    char id[10];
    int deadline;
    int profit;
} Job;

// Comparison function for sorting by profit (descending)
int compare(const void *a, const void *b) {
    Job *job1 = (Job *)a;
    Job *job2 = (Job *)b;
    return job2->profit - job1->profit;
}

void job_sequencing(Job jobs[], int n, int n_slots) {
    // Sort jobs by profit in descending order
    qsort(jobs, n, sizeof(Job), compare);
    
    // Track free slots
    int slots[n_slots];
    for (int i = 0; i < n_slots; i++) {
        slots[i] = -1;
    }
    
    char sequence[n_slots][10];
    int total_profit = 0;
    int count = 0;
    
    // Iterate through all jobs
    for (int i = 0; i < n; i++) {
        // Find latest available slot before deadline
        for (int j = (jobs[i].deadline < n_slots ? jobs[i].deadline : n_slots) - 1; j >= 0; j--) {
            if (slots[j] == -1) {
                slots[j] = i;
                strcpy(sequence[count], jobs[i].id);
                total_profit += jobs[i].profit;
                count++;
                break;
            }
        }
    }
    
    printf("Maximum profit: %d\n", total_profit);
    printf("Job sequence: ");
    for (int i = 0; i < count; i++) {
        printf("%s ", sequence[i]);
    }
    printf("\n");
}

// Example usage
int main() {
    Job jobs[] = {
        {"J1", 2, 100},
        {"J2", 1, 19},
        {"J3", 2, 27},
        {"J4", 1, 25},
        {"J5", 3, 15}
    };
    int n = 5;
    int n_slots = 3;
    
    job_sequencing(jobs, n, n_slots);
    return 0;
}
```

### Implementation (C++)
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <string>
using namespace std;

struct Job {
    string id;
    int deadline;
    int profit;
    
    bool operator>(const Job& other) const {
        return profit > other.profit;
    }
};

pair<int, vector<string>> jobSequencing(vector<Job>& jobs, int nSlots) {
    // Sort jobs by profit in descending order
    sort(jobs.begin(), jobs.end(), greater<Job>());
    
    // Track free slots
    vector<int> slots(nSlots, -1);
    int totalProfit = 0;
    vector<string> sequence;
    
    for (const auto& job : jobs) {
        // Find latest available slot before deadline
        for (int slot = min(nSlots, job.deadline) - 1; slot >= 0; slot--) {
            if (slots[slot] == -1) {
                slots[slot] = 1;
                totalProfit += job.profit;
                sequence.push_back(job.id);
                break;
            }
        }
    }
    
    return {totalProfit, sequence};
}

// Example usage
int main() {
    vector<Job> jobs = {
        {"J1", 2, 100},
        {"J2", 1, 19},
        {"J3", 2, 27},
        {"J4", 1, 25},
        {"J5", 3, 15}
    };
    
    auto [profit, sequence] = jobSequencing(jobs, 3);
    
    cout << "Maximum profit: " << profit << endl;
    cout << "Job sequence: ";
    for (const string& id : sequence) {
        cout << id << " ";
    }
    cout << endl;
    
    return 0;
}
```

### Implementation (Java)
```java
import java.util.*;

class Job implements Comparable<Job> {
    String id;
    int deadline;
    int profit;
    
    Job(String id, int deadline, int profit) {
        this.id = id;
        this.deadline = deadline;
        this.profit = profit;
    }
    
    @Override
    public int compareTo(Job other) {
        return other.profit - this.profit; // Descending order
    }
}

public class JobSequencing {
    public static class Result {
        int totalProfit;
        List<String> sequence;
        
        Result(int totalProfit, List<String> sequence) {
            this.totalProfit = totalProfit;
            this.sequence = sequence;
        }
    }
    
    public static Result jobSequencing(List<Job> jobs, int nSlots) {
        // Sort jobs by profit in descending order
        Collections.sort(jobs);
        
        // Track free slots
        int[] slots = new int[nSlots];
        Arrays.fill(slots, -1);
        
        int totalProfit = 0;
        List<String> sequence = new ArrayList<>();
        
        for (Job job : jobs) {
            // Find latest available slot before deadline
            for (int slot = Math.min(nSlots, job.deadline) - 1; slot >= 0; slot--) {
                if (slots[slot] == -1) {
                    slots[slot] = 1;
                    totalProfit += job.profit;
                    sequence.add(job.id);
                    break;
                }
            }
        }
        
        return new Result(totalProfit, sequence);
    }
    
    // Example usage
    public static void main(String[] args) {
        List<Job> jobs = Arrays.asList(
            new Job("J1", 2, 100),
            new Job("J2", 1, 19),
            new Job("J3", 2, 27),
            new Job("J4", 1, 25),
            new Job("J5", 3, 15)
        );
        
        Result result = jobSequencing(jobs, 3);
        
        System.out.println("Maximum profit: " + result.totalProfit);
        System.out.print("Job sequence: ");
        for (String id : result.sequence) {
            System.out.print(id + " ");
        }
        System.out.println();
    }
}
```

### Dry Run Example
```
Jobs: (ID, Deadline, Profit)
J1: (2, 100)
J2: (1, 19)
J3: (2, 27)
J4: (1, 25)
J5: (3, 15)

After sorting by profit:
J1: (2, 100) - Assign to slot 1 → [_, J1, _]
J3: (2, 27)  - Assign to slot 0 → [J3, J1, _]
J4: (1, 25)  - No slot available before deadline 1
J2: (1, 19)  - No slot available before deadline 1
J5: (3, 15)  - Assign to slot 2 → [J3, J1, J5]

Total Profit: 100 + 27 + 15 = 142
Sequence: [J1, J3, J5]
```

### Complexity
- **Time**: O(n²)
- **Space**: O(n)

---

## 4. Huffman Coding

### Problem Statement
Given characters and their frequencies, generate optimal prefix-free binary codes to minimize total encoding length.

### Greedy Strategy
Build a binary tree by repeatedly combining two nodes with smallest frequencies.

### Implementation (Python)
```python
import heapq
from collections import defaultdict

class Node:
    def __init__(self, char, freq):
        self.char = char
        self.freq = freq
        self.left = None
        self.right = None
    
    def __lt__(self, other):
        return self.freq < other.freq

def huffman_coding(char_freq):
    """
    Generate Huffman codes for given character frequencies
    """
    # Create min heap
    heap = [Node(char, freq) for char, freq in char_freq.items()]
    heapq.heapify(heap)
    
    # Build Huffman tree
    while len(heap) > 1:
        left = heapq.heappop(heap)
        right = heapq.heappop(heap)
        
        merged = Node(None, left.freq + right.freq)
        merged.left = left
        merged.right = right
        
        heapq.heappush(heap, merged)
    
    # Generate codes
    root = heap[0]
    codes = {}
    
    def generate_codes(node, code):
        if node.char is not None:
            codes[node.char] = code
            return
        
        if node.left:
            generate_codes(node.left, code + '0')
        if node.right:
            generate_codes(node.right, code + '1')
    
    generate_codes(root, '')
    return codes

# Example
char_freq = {'a': 5, 'b': 9, 'c': 12, 'd': 13, 'e': 16, 'f': 45}
codes = huffman_coding(char_freq)

for char, code in sorted(codes.items()):
    print(f"{char}: {code}")

# Calculate average length
total_freq = sum(char_freq.values())
avg_length = sum(len(codes[char]) * char_freq[char] for char in codes) / total_freq
print(f"Average code length: {avg_length:.2f}")
```

### Implementation (C)
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_TREE_HT 100

// Huffman tree node
typedef struct MinHeapNode {
    char data;
    unsigned freq;
    struct MinHeapNode *left, *right;
} MinHeapNode;

// Min heap structure
typedef struct MinHeap {
    unsigned size;
    unsigned capacity;
    MinHeapNode** array;
} MinHeap;

// Create a new min heap node
MinHeapNode* newNode(char data, unsigned freq) {
    MinHeapNode* temp = (MinHeapNode*)malloc(sizeof(MinHeapNode));
    temp->left = temp->right = NULL;
    temp->data = data;
    temp->freq = freq;
    return temp;
}

// Create min heap
MinHeap* createMinHeap(unsigned capacity) {
    MinHeap* minHeap = (MinHeap*)malloc(sizeof(MinHeap));
    minHeap->size = 0;
    minHeap->capacity = capacity;
    minHeap->array = (MinHeapNode**)malloc(minHeap->capacity * sizeof(MinHeapNode*));
    return minHeap;
}

// Swap two min heap nodes
void swapMinHeapNode(MinHeapNode** a, MinHeapNode** b) {
    MinHeapNode* t = *a;
    *a = *b;
    *b = t;
}

// Min heapify
void minHeapify(MinHeap* minHeap, int idx) {
    int smallest = idx;
    int left = 2 * idx + 1;
    int right = 2 * idx + 2;
    
    if (left < minHeap->size && minHeap->array[left]->freq < minHeap->array[smallest]->freq)
        smallest = left;
    
    if (right < minHeap->size && minHeap->array[right]->freq < minHeap->array[smallest]->freq)
        smallest = right;
    
    if (smallest != idx) {
        swapMinHeapNode(&minHeap->array[smallest], &minHeap->array[idx]);
        minHeapify(minHeap, smallest);
    }
}

// Extract min from heap
MinHeapNode* extractMin(MinHeap* minHeap) {
    MinHeapNode* temp = minHeap->array[0];
    minHeap->array[0] = minHeap->array[minHeap->size - 1];
    --minHeap->size;
    minHeapify(minHeap, 0);
    return temp;
}

// Insert into min heap
void insertMinHeap(MinHeap* minHeap, MinHeapNode* minHeapNode) {
    ++minHeap->size;
    int i = minHeap->size - 1;
    
    while (i && minHeapNode->freq < minHeap->array[(i - 1) / 2]->freq) {
        minHeap->array[i] = minHeap->array[(i - 1) / 2];
        i = (i - 1) / 2;
    }
    minHeap->array[i] = minHeapNode;
}

// Build min heap
void buildMinHeap(MinHeap* minHeap) {
    int n = minHeap->size - 1;
    for (int i = (n - 1) / 2; i >= 0; --i)
        minHeapify(minHeap, i);
}

// Check if size is one
int isSizeOne(MinHeap* minHeap) {
    return (minHeap->size == 1);
}

// Build Huffman tree
MinHeapNode* buildHuffmanTree(char data[], int freq[], int size) {
    MinHeapNode *left, *right, *top;
    
    // Create min heap
    MinHeap* minHeap = createMinHeap(size);
    
    for (int i = 0; i < size; ++i)
        minHeap->array[i] = newNode(data[i], freq[i]);
    
    minHeap->size = size;
    buildMinHeap(minHeap);
    
    // Build Huffman tree
    while (!isSizeOne(minHeap)) {
        left = extractMin(minHeap);
        right = extractMin(minHeap);
        
        top = newNode('$', left->freq + right->freq);
        top->left = left;
        top->right = right;
        
        insertMinHeap(minHeap, top);
    }
    
    return extractMin(minHeap);
}

// Print codes from Huffman tree
void printCodes(MinHeapNode* root, int arr[], int top) {
    if (root->left) {
        arr[top] = 0;
        printCodes(root->left, arr, top + 1);
    }
    
    if (root->right) {
        arr[top] = 1;
        printCodes(root->right, arr, top + 1);
    }
    
    if (!(root->left) && !(root->right)) {
        printf("%c: ", root->data);
        for (int i = 0; i < top; ++i)
            printf("%d", arr[i]);
        printf("\n");
    }
}

// Huffman coding
void huffmanCoding(char data[], int freq[], int size) {
    MinHeapNode* root = buildHuffmanTree(data, freq, size);
    
    int arr[MAX_TREE_HT], top = 0;
    printCodes(root, arr, top);
}

// Example usage
int main() {
    char arr[] = {'a', 'b', 'c', 'd', 'e', 'f'};
    int freq[] = {5, 9, 12, 13, 16, 45};
    int size = sizeof(arr) / sizeof(arr[0]);
    
    huffmanCoding(arr, freq, size);
    return 0;
}
```

### Implementation (C++)
```cpp
#include <iostream>
#include <queue>
#include <unordered_map>
#include <vector>
using namespace std;

// Huffman tree node
struct Node {
    char ch;
    int freq;
    Node *left, *right;
    
    Node(char ch, int freq) : ch(ch), freq(freq), left(nullptr), right(nullptr) {}
};

// Comparison object for priority queue
struct Compare {
    bool operator()(Node* l, Node* r) {
        return l->freq > r->freq;
    }
};

// Generate codes from Huffman tree
void generateCodes(Node* root, string code, unordered_map<char, string>& codes) {
    if (!root) return;
    
    if (root->ch != '\0') {
        codes[root->ch] = code;
        return;
    }
    
    generateCodes(root->left, code + "0", codes);
    generateCodes(root->right, code + "1", codes);
}

// Huffman coding
unordered_map<char, string> huffmanCoding(unordered_map<char, int>& charFreq) {
    // Create min heap
    priority_queue<Node*, vector<Node*>, Compare> minHeap;
    
    for (auto& pair : charFreq) {
        minHeap.push(new Node(pair.first, pair.second));
    }
    
    // Build Huffman tree
    while (minHeap.size() > 1) {
        Node* left = minHeap.top(); minHeap.pop();
        Node* right = minHeap.top(); minHeap.pop();
        
        Node* merged = new Node('\0', left->freq + right->freq);
        merged->left = left;
        merged->right = right;
        
        minHeap.push(merged);
    }
    
    // Generate codes
    Node* root = minHeap.top();
    unordered_map<char, string> codes;
    generateCodes(root, "", codes);
    
    return codes;
}

// Example usage
int main() {
    unordered_map<char, int> charFreq = {
        {'a', 5}, {'b', 9}, {'c', 12}, 
        {'d', 13}, {'e', 16}, {'f', 45}
    };
    
    auto codes = huffmanCoding(charFreq);
    
    cout << "Huffman Codes:\n";
    for (auto& pair : codes) {
        cout << pair.first << ": " << pair.second << endl;
    }
    
    // Calculate average length
    int totalFreq = 0;
    double avgLength = 0.0;
    for (auto& p : charFreq) totalFreq += p.second;
    for (auto& p : codes) {
        avgLength += codes[p.first].length() * charFreq[p.first];
    }
    avgLength /= totalFreq;
    
    cout << "Average code length: " << avgLength << endl;
    
    return 0;
}
```

### Implementation (Java)
```java
import java.util.*;

class Node implements Comparable<Node> {
    char ch;
    int freq;
    Node left, right;
    
    Node(char ch, int freq) {
        this.ch = ch;
        this.freq = freq;
        this.left = null;
        this.right = null;
    }
    
    @Override
    public int compareTo(Node other) {
        return this.freq - other.freq;
    }
}

public class HuffmanCoding {
    // Generate codes from Huffman tree
    private static void generateCodes(Node root, String code, Map<Character, String> codes) {
        if (root == null) return;
        
        if (root.ch != '\0') {
            codes.put(root.ch, code);
            return;
        }
        
        generateCodes(root.left, code + "0", codes);
        generateCodes(root.right, code + "1", codes);
    }
    
    public static Map<Character, String> huffmanCoding(Map<Character, Integer> charFreq) {
        // Create min heap
        PriorityQueue<Node> minHeap = new PriorityQueue<>();
        
        for (Map.Entry<Character, Integer> entry : charFreq.entrySet()) {
            minHeap.add(new Node(entry.getKey(), entry.getValue()));
        }
        
        // Build Huffman tree
        while (minHeap.size() > 1) {
            Node left = minHeap.poll();
            Node right = minHeap.poll();
            
            Node merged = new Node('\0', left.freq + right.freq);
            merged.left = left;
            merged.right = right;
            
            minHeap.add(merged);
        }
        
        // Generate codes
        Node root = minHeap.poll();
        Map<Character, String> codes = new HashMap<>();
        generateCodes(root, "", codes);
        
        return codes;
    }
    
    // Example usage
    public static void main(String[] args) {
        Map<Character, Integer> charFreq = new HashMap<>();
        charFreq.put('a', 5);
        charFreq.put('b', 9);
        charFreq.put('c', 12);
        charFreq.put('d', 13);
        charFreq.put('e', 16);
        charFreq.put('f', 45);
        
        Map<Character, String> codes = huffmanCoding(charFreq);
        
        System.out.println("Huffman Codes:");
        for (Map.Entry<Character, String> entry : codes.entrySet()) {
            System.out.println(entry.getKey() + ": " + entry.getValue());
        }
        
        // Calculate average length
        int totalFreq = 0;
        double avgLength = 0.0;
        for (int freq : charFreq.values()) totalFreq += freq;
        for (Map.Entry<Character, String> entry : codes.entrySet()) {
            avgLength += entry.getValue().length() * charFreq.get(entry.getKey());
        }
        avgLength /= totalFreq;
        
        System.out.printf("Average code length: %.2f\n", avgLength);
    }
}
```

### Dry Run Example
```
Characters: {a:5, b:9, c:12, d:13, e:16, f:45}

Step 1: Combine a(5) and b(9) → ab(14)
Heap: [c:12, d:13, ab:14, e:16, f:45]

Step 2: Combine c(12) and d(13) → cd(25)
Heap: [ab:14, e:16, cd:25, f:45]

Step 3: Combine ab(14) and e(16) → abe(30)
Heap: [cd:25, abe:30, f:45]

Step 4: Combine cd(25) and abe(30) → cdabe(55)
Heap: [f:45, cdabe:55]

Step 5: Combine f(45) and cdabe(55) → root(100)

Final Tree:
         100
        /    \
      45(f)   55
             /   \
           25     30
          / \    / \
        12  13  14  16
        c   d   /\   e
              5  9
              a  b

Codes:
f: 0
c: 100
d: 101
a: 1100
b: 1101
e: 111
```

### Complexity
- **Time**: O(n log n)
- **Space**: O(n)

---

## 5. Coin Change (Greedy)

### Problem Statement
Given coin denominations and amount, find minimum number of coins needed.

### Note
Greedy works for canonical coin systems (like US: 1, 5, 10, 25), but not for all systems.

### Implementation (Python)
```python
def coin_change_greedy(coins, amount):
    """
    Greedy coin change (may not always give optimal solution)
    """
    coins.sort(reverse=True)  # Sort descending
    
    count = 0
    result = []
    
    for coin in coins:
        while amount >= coin:
            amount -= coin
            count += 1
            result.append(coin)
    
    if amount == 0:
        return count, result
    else:
        return -1, []  # Cannot make exact change

# Example - Works for canonical system
coins = [1, 5, 10, 25]
amount = 63
count, result = coin_change_greedy(coins, amount)
print(f"Minimum coins: {count}")
print(f"Coins used: {result}")

# Example - Fails for non-canonical system
coins = [1, 3, 4]
amount = 6
count, result = coin_change_greedy(coins, amount)
print(f"Greedy result: {result}")  # [4, 1, 1] = 3 coins
print(f"Optimal: [3, 3]")  # 2 coins (greedy fails here)
```

### Implementation (C)
```c
#include <stdio.h>
#include <stdlib.h>

// Comparison function for descending sort
int compare(const void *a, const void *b) {
    return (*(int*)b - *(int*)a);
}

void coin_change_greedy(int coins[], int n, int amount) {
    // Sort coins in descending order
    qsort(coins, n, sizeof(int), compare);
    
    int count = 0;
    int result[1000];
    int resultSize = 0;
    
    for (int i = 0; i < n; i++) {
        while (amount >= coins[i]) {
            amount -= coins[i];
            result[resultSize++] = coins[i];
            count++;
        }
    }
    
    if (amount == 0) {
        printf("Minimum coins: %d\n", count);
        printf("Coins used: ");
        for (int i = 0; i < resultSize; i++) {
            printf("%d ", result[i]);
        }
        printf("\n");
    } else {
        printf("Cannot make exact change\n");
    }
}

// Example usage
int main() {
    // Works for canonical system
    int coins1[] = {1, 5, 10, 25};
    int n1 = 4;
    int amount1 = 63;
    
    printf("Example 1 (Canonical system):\n");
    coin_change_greedy(coins1, n1, amount1);
    
    // Fails for non-canonical system
    int coins2[] = {1, 3, 4};
    int n2 = 3;
    int amount2 = 6;
    
    printf("\nExample 2 (Non-canonical system):\n");
    coin_change_greedy(coins2, n2, amount2);
    printf("Optimal: [3, 3] = 2 coins (greedy fails here)\n");
    
    return 0;
}
```

### Implementation (C++)
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

pair<int, vector<int>> coinChangeGreedy(vector<int> coins, int amount) {
    // Sort coins in descending order
    sort(coins.begin(), coins.end(), greater<int>());
    
    int count = 0;
    vector<int> result;
    
    for (int coin : coins) {
        while (amount >= coin) {
            amount -= coin;
            result.push_back(coin);
            count++;
        }
    }
    
    if (amount == 0) {
        return {count, result};
    } else {
        return {-1, {}};  // Cannot make exact change
    }
}

// Example usage
int main() {
    // Works for canonical system
    vector<int> coins1 = {1, 5, 10, 25};
    int amount1 = 63;
    
    auto [count1, result1] = coinChangeGreedy(coins1, amount1);
    
    cout << "Example 1 (Canonical system):\n";
    cout << "Minimum coins: " << count1 << endl;
    cout << "Coins used: ";
    for (int coin : result1) {
        cout << coin << " ";
    }
    cout << endl;
    
    // Fails for non-canonical system
    vector<int> coins2 = {1, 3, 4};
    int amount2 = 6;
    
    auto [count2, result2] = coinChangeGreedy(coins2, amount2);
    
    cout << "\nExample 2 (Non-canonical system):\n";
    cout << "Greedy result: ";
    for (int coin : result2) {
        cout << coin << " ";
    }
    cout << " = " << count2 << " coins\n";
    cout << "Optimal: [3, 3] = 2 coins (greedy fails here)\n";
    
    return 0;
}
```

### Implementation (Java)
```java
import java.util.*;

public class CoinChangeGreedy {
    public static class Result {
        int count;
        List<Integer> coins;
        
        Result(int count, List<Integer> coins) {
            this.count = count;
            this.coins = coins;
        }
    }
    
    public static Result coinChangeGreedy(int[] coins, int amount) {
        // Sort coins in descending order
        Integer[] coinsBoxed = new Integer[coins.length];
        for (int i = 0; i < coins.length; i++) {
            coinsBoxed[i] = coins[i];
        }
        Arrays.sort(coinsBoxed, Collections.reverseOrder());
        
        int count = 0;
        List<Integer> result = new ArrayList<>();
        
        for (int coin : coinsBoxed) {
            while (amount >= coin) {
                amount -= coin;
                result.add(coin);
                count++;
            }
        }
        
        if (amount == 0) {
            return new Result(count, result);
        } else {
            return new Result(-1, new ArrayList<>());  // Cannot make exact change
        }
    }
    
    // Example usage
    public static void main(String[] args) {
        // Works for canonical system
        int[] coins1 = {1, 5, 10, 25};
        int amount1 = 63;
        
        Result result1 = coinChangeGreedy(coins1, amount1);
        
        System.out.println("Example 1 (Canonical system):");
        System.out.println("Minimum coins: " + result1.count);
        System.out.print("Coins used: ");
        for (int coin : result1.coins) {
            System.out.print(coin + " ");
        }
        System.out.println();
        
        // Fails for non-canonical system
        int[] coins2 = {1, 3, 4};
        int amount2 = 6;
        
        Result result2 = coinChangeGreedy(coins2, amount2);
        
        System.out.println("\nExample 2 (Non-canonical system):");
        System.out.print("Greedy result: ");
        for (int coin : result2.coins) {
            System.out.print(coin + " ");
        }
        System.out.println(" = " + result2.count + " coins");
        System.out.println("Optimal: [3, 3] = 2 coins (greedy fails here)");
    }
}
```

### Complexity
- **Time**: O(n log n + amount/min_coin)
- **Space**: O(k) where k is number of coins used

---

## 6. Minimum Spanning Tree - Prim's Algorithm

### Problem Statement
Find minimum cost spanning tree in a weighted undirected graph.

### Greedy Strategy
Start with any vertex. Always add the minimum weight edge that connects a vertex in MST to a vertex outside.

### Implementation (Python)
```python
import heapq

def prims_mst(graph, start=0):
    """
    Prim's algorithm for Minimum Spanning Tree
    graph: adjacency list with weights
    """
    n = len(graph)
    visited = [False] * n
    min_heap = [(0, start, -1)]  # (weight, vertex, parent)
    
    mst_edges = []
    total_cost = 0
    
    while min_heap:
        weight, u, parent = heapq.heappop(min_heap)
        
        if visited[u]:
            continue
        
        visited[u] = True
        if parent != -1:
            mst_edges.append((parent, u, weight))
            total_cost += weight
        
        # Add edges to unvisited neighbors
        for v, w in graph[u]:
            if not visited[v]:
                heapq.heappush(min_heap, (w, v, u))
    
    return mst_edges, total_cost

# Example
graph = [
    [(1, 2), (3, 6)],           # 0: edges to 1(weight 2), 3(weight 6)
    [(0, 2), (2, 3), (3, 8), (4, 5)],  # 1
    [(1, 3), (4, 7)],           # 2
    [(0, 6), (1, 8)],           # 3
    [(1, 5), (2, 7)]            # 4
]

mst, cost = prims_mst(graph)
print(f"MST edges: {mst}")
print(f"Total cost: {cost}")
```

### Implementation (C)
```c
#include <stdio.h>
#include <stdlib.h>
#include <limits.h>
#include <stdbool.h>

#define MAX_V 100

// Function to find vertex with minimum key value
int minKey(int key[], bool mstSet[], int V) {
    int min = INT_MAX, min_index;
    
    for (int v = 0; v < V; v++) {
        if (!mstSet[v] && key[v] < min) {
            min = key[v];
            min_index = v;
        }
    }
    
    return min_index;
}

// Prim's MST algorithm
void primsMST(int graph[MAX_V][MAX_V], int V) {
    int parent[V];     // Array to store constructed MST
    int key[V];        // Key values used to pick minimum weight edge
    bool mstSet[V];    // To represent set of vertices included in MST
    
    // Initialize all keys as INFINITE
    for (int i = 0; i < V; i++) {
        key[i] = INT_MAX;
        mstSet[i] = false;
    }
    
    // Always include first vertex in MST
    key[0] = 0;
    parent[0] = -1;
    
    int totalCost = 0;
    
    // The MST will have V vertices
    for (int count = 0; count < V - 1; count++) {
        // Pick the minimum key vertex from the set of vertices not yet included
        int u = minKey(key, mstSet, V);
        mstSet[u] = true;
        
        // Update key value and parent index of adjacent vertices
        for (int v = 0; v < V; v++) {
            if (graph[u][v] && !mstSet[v] && graph[u][v] < key[v]) {
                parent[v] = u;
                key[v] = graph[u][v];
            }
        }
    }
    
    // Print the constructed MST
    printf("MST edges:\n");
    for (int i = 1; i < V; i++) {
        printf("(%d, %d, %d)\n", parent[i], i, graph[i][parent[i]]);
        totalCost += graph[i][parent[i]];
    }
    printf("Total cost: %d\n", totalCost);
}

// Example usage
int main() {
    int V = 5;
    int graph[MAX_V][MAX_V] = {
        {0, 2, 0, 6, 0},
        {2, 0, 3, 8, 5},
        {0, 3, 0, 0, 7},
        {6, 8, 0, 0, 0},
        {0, 5, 7, 0, 0}
    };
    
    primsMST(graph, V);
    
    return 0;
}
```

### Implementation (C++)
```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <climits>
using namespace std;

typedef pair<int, int> pii;  // (weight, vertex)

struct Edge {
    int u, v, weight;
};

vector<Edge> primsMST(vector<vector<pii>>& graph, int start = 0) {
    int n = graph.size();
    vector<bool> visited(n, false);
    priority_queue<pair<int, pii>, vector<pair<int, pii>>, greater<pair<int, pii>>> minHeap;
    // minHeap stores (weight, (vertex, parent))
    
    minHeap.push({0, {start, -1}});
    
    vector<Edge> mstEdges;
    int totalCost = 0;
    
    while (!minHeap.empty()) {
        auto [weight, vp] = minHeap.top();
        auto [u, parent] = vp;
        minHeap.pop();
        
        if (visited[u]) continue;
        
        visited[u] = true;
        
        if (parent != -1) {
            mstEdges.push_back({parent, u, weight});
            totalCost += weight;
        }
        
        // Add edges to unvisited neighbors
        for (auto [v, w] : graph[u]) {
            if (!visited[v]) {
                minHeap.push({w, {v, u}});
            }
        }
    }
    
    cout << "MST edges:\n";
    for (const auto& edge : mstEdges) {
        cout << "(" << edge.u << ", " << edge.v << ", " << edge.weight << ")\n";
    }
    cout << "Total cost: " << totalCost << endl;
    
    return mstEdges;
}

// Example usage
int main() {
    // Graph represented as adjacency list with weights
    vector<vector<pii>> graph = {
        {{1, 2}, {3, 6}},              // 0: edges to 1(weight 2), 3(weight 6)
        {{0, 2}, {2, 3}, {3, 8}, {4, 5}},  // 1
        {{1, 3}, {4, 7}},              // 2
        {{0, 6}, {1, 8}},              // 3
        {{1, 5}, {2, 7}}               // 4
    };
    
    primsMST(graph);
    
    return 0;
}
```

### Implementation (Java)
```java
import java.util.*;

class Edge {
    int u, v, weight;
    
    Edge(int u, int v, int weight) {
        this.u = u;
        this.v = v;
        this.weight = weight;
    }
}

class HeapNode implements Comparable<HeapNode> {
    int weight, vertex, parent;
    
    HeapNode(int weight, int vertex, int parent) {
        this.weight = weight;
        this.vertex = vertex;
        this.parent = parent;
    }
    
    @Override
    public int compareTo(HeapNode other) {
        return this.weight - other.weight;
    }
}

public class PrimsMST {
    public static List<Edge> primsMST(List<List<int[]>> graph, int start) {
        int n = graph.size();
        boolean[] visited = new boolean[n];
        PriorityQueue<HeapNode> minHeap = new PriorityQueue<>();
        
        minHeap.add(new HeapNode(0, start, -1));
        
        List<Edge> mstEdges = new ArrayList<>();
        int totalCost = 0;
        
        while (!minHeap.isEmpty()) {
            HeapNode node = minHeap.poll();
            int weight = node.weight;
            int u = node.vertex;
            int parent = node.parent;
            
            if (visited[u]) continue;
            
            visited[u] = true;
            
            if (parent != -1) {
                mstEdges.add(new Edge(parent, u, weight));
                totalCost += weight;
            }
            
            // Add edges to unvisited neighbors
            for (int[] edge : graph.get(u)) {
                int v = edge[0];
                int w = edge[1];
                if (!visited[v]) {
                    minHeap.add(new HeapNode(w, v, u));
                }
            }
        }
        
        System.out.println("MST edges:");
        for (Edge edge : mstEdges) {
            System.out.println("(" + edge.u + ", " + edge.v + ", " + edge.weight + ")");
        }
        System.out.println("Total cost: " + totalCost);
        
        return mstEdges;
    }
    
    // Example usage
    public static void main(String[] args) {
        // Graph represented as adjacency list with weights
        List<List<int[]>> graph = new ArrayList<>();
        
        graph.add(Arrays.asList(new int[]{1, 2}, new int[]{3, 6}));  // 0
        graph.add(Arrays.asList(new int[]{0, 2}, new int[]{2, 3}, 
                                new int[]{3, 8}, new int[]{4, 5}));  // 1
        graph.add(Arrays.asList(new int[]{1, 3}, new int[]{4, 7}));  // 2
        graph.add(Arrays.asList(new int[]{0, 6}, new int[]{1, 8}));  // 3
        graph.add(Arrays.asList(new int[]{1, 5}, new int[]{2, 7}));  // 4
        
        primsMST(graph, 0);
    }
}
```

---

## 7. Minimum Spanning Tree - Kruskal's Algorithm

### Greedy Strategy
Sort all edges by weight. Add edges in increasing order if they don't form a cycle (use Union-Find).

### Implementation (Python)
```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n
    
    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
    
    def union(self, x, y):
        px, py = self.find(x), self.find(y)
        if px == py:
            return False
        
        if self.rank[px] < self.rank[py]:
            px, py = py, px
        
        self.parent[py] = px
        if self.rank[px] == self.rank[py]:
            self.rank[px] += 1
        
        return True

def kruskals_mst(n, edges):
    """
    Kruskal's algorithm for MST
    n: number of vertices
    edges: list of (u, v, weight)
    """
    edges.sort(key=lambda x: x[2])  # Sort by weight
    uf = UnionFind(n)
    
    mst_edges = []
    total_cost = 0
    
    for u, v, weight in edges:
        if uf.union(u, v):
            mst_edges.append((u, v, weight))
            total_cost += weight
            
            if len(mst_edges) == n - 1:
                break
    
    return mst_edges, total_cost

# Example
n = 5
edges = [
    (0, 1, 2), (0, 3, 6), (1, 2, 3),
    (1, 3, 8), (1, 4, 5), (2, 4, 7)
]

mst, cost = kruskals_mst(n, edges)
print(f"MST edges: {mst}")
print(f"Total cost: {cost}")
```

### Implementation (C)
```c
#include <stdio.h>
#include <stdlib.h>

// Union-Find (Disjoint Set Union) structure
typedef struct {
    int *parent;
    int *rank;
    int n;
} UnionFind;

// Edge structure
typedef struct {
    int u, v, weight;
} Edge;

// Initialize Union-Find
UnionFind* createUnionFind(int n) {
    UnionFind* uf = (UnionFind*)malloc(sizeof(UnionFind));
    uf->n = n;
    uf->parent = (int*)malloc(n * sizeof(int));
    uf->rank = (int*)malloc(n * sizeof(int));
    
    for (int i = 0; i < n; i++) {
        uf->parent[i] = i;
        uf->rank[i] = 0;
    }
    
    return uf;
}

// Find with path compression
int find(UnionFind* uf, int x) {
    if (uf->parent[x] != x) {
        uf->parent[x] = find(uf, uf->parent[x]);
    }
    return uf->parent[x];
}

// Union by rank
int unionSets(UnionFind* uf, int x, int y) {
    int px = find(uf, x);
    int py = find(uf, y);
    
    if (px == py) return 0;
    
    if (uf->rank[px] < uf->rank[py]) {
        int temp = px;
        px = py;
        py = temp;
    }
    
    uf->parent[py] = px;
    if (uf->rank[px] == uf->rank[py]) {
        uf->rank[px]++;
    }
    
    return 1;
}

// Comparison function for sorting edges
int compareEdges(const void* a, const void* b) {
    Edge* e1 = (Edge*)a;
    Edge* e2 = (Edge*)b;
    return e1->weight - e2->weight;
}

// Kruskal's MST algorithm
void kruskalsMST(int n, Edge edges[], int edgeCount) {
    // Sort edges by weight
    qsort(edges, edgeCount, sizeof(Edge), compareEdges);
    
    UnionFind* uf = createUnionFind(n);
    
    Edge mstEdges[n-1];
    int mstCount = 0;
    int totalCost = 0;
    
    for (int i = 0; i < edgeCount; i++) {
        if (unionSets(uf, edges[i].u, edges[i].v)) {
            mstEdges[mstCount++] = edges[i];
            totalCost += edges[i].weight;
            
            if (mstCount == n - 1) break;
        }
    }
    
    printf("MST edges:\n");
    for (int i = 0; i < mstCount; i++) {
        printf("(%d, %d, %d)\n", mstEdges[i].u, mstEdges[i].v, mstEdges[i].weight);
    }
    printf("Total cost: %d\n", totalCost);
    
    free(uf->parent);
    free(uf->rank);
    free(uf);
}

// Example usage
int main() {
    int n = 5;
    Edge edges[] = {
        {0, 1, 2}, {0, 3, 6}, {1, 2, 3},
        {1, 3, 8}, {1, 4, 5}, {2, 4, 7}
    };
    int edgeCount = 6;
    
    kruskalsMST(n, edges, edgeCount);
    
    return 0;
}
```

### Implementation (C++)
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

// Union-Find class
class UnionFind {
private:
    vector<int> parent;
    vector<int> rank;
    
public:
    UnionFind(int n) {
        parent.resize(n);
        rank.resize(n, 0);
        for (int i = 0; i < n; i++) {
            parent[i] = i;
        }
    }
    
    int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]);
        }
        return parent[x];
    }
    
    bool unite(int x, int y) {
        int px = find(x);
        int py = find(y);
        
        if (px == py) return false;
        
        if (rank[px] < rank[py]) {
            swap(px, py);
        }
        
        parent[py] = px;
        if (rank[px] == rank[py]) {
            rank[px]++;
        }
        
        return true;
    }
};

// Edge structure
struct Edge {
    int u, v, weight;
    
    bool operator<(const Edge& other) const {
        return weight < other.weight;
    }
};

pair<vector<Edge>, int> kruskalsMST(int n, vector<Edge>& edges) {
    // Sort edges by weight
    sort(edges.begin(), edges.end());
    
    UnionFind uf(n);
    vector<Edge> mstEdges;
    int totalCost = 0;
    
    for (const auto& edge : edges) {
        if (uf.unite(edge.u, edge.v)) {
            mstEdges.push_back(edge);
            totalCost += edge.weight;
            
            if (mstEdges.size() == n - 1) break;
        }
    }
    
    return {mstEdges, totalCost};
}

// Example usage
int main() {
    int n = 5;
    vector<Edge> edges = {
        {0, 1, 2}, {0, 3, 6}, {1, 2, 3},
        {1, 3, 8}, {1, 4, 5}, {2, 4, 7}
    };
    
    auto [mst, cost] = kruskalsMST(n, edges);
    
    cout << "MST edges:\n";
    for (const auto& edge : mst) {
        cout << "(" << edge.u << ", " << edge.v << ", " << edge.weight << ")\n";
    }
    cout << "Total cost: " << cost << endl;
    
    return 0;
}
```

### Implementation (Java)
```java
import java.util.*;

class UnionFind {
    private int[] parent;
    private int[] rank;
    
    public UnionFind(int n) {
        parent = new int[n];
        rank = new int[n];
        
        for (int i = 0; i < n; i++) {
            parent[i] = i;
            rank[i] = 0;
        }
    }
    
    public int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]);
        }
        return parent[x];
    }
    
    public boolean union(int x, int y) {
        int px = find(x);
        int py = find(y);
        
        if (px == py) return false;
        
        if (rank[px] < rank[py]) {
            int temp = px;
            px = py;
            py = temp;
        }
        
        parent[py] = px;
        if (rank[px] == rank[py]) {
            rank[px]++;
        }
        
        return true;
    }
}

class Edge implements Comparable<Edge> {
    int u, v, weight;
    
    Edge(int u, int v, int weight) {
        this.u = u;
        this.v = v;
        this.weight = weight;
    }
    
    @Override
    public int compareTo(Edge other) {
        return this.weight - other.weight;
    }
}

public class KruskalsMST {
    public static class Result {
        List<Edge> mstEdges;
        int totalCost;
        
        Result(List<Edge> mstEdges, int totalCost) {
            this.mstEdges = mstEdges;
            this.totalCost = totalCost;
        }
    }
    
    public static Result kruskalsMST(int n, List<Edge> edges) {
        // Sort edges by weight
        Collections.sort(edges);
        
        UnionFind uf = new UnionFind(n);
        List<Edge> mstEdges = new ArrayList<>();
        int totalCost = 0;
        
        for (Edge edge : edges) {
            if (uf.union(edge.u, edge.v)) {
                mstEdges.add(edge);
                totalCost += edge.weight;
                
                if (mstEdges.size() == n - 1) break;
            }
        }
        
        return new Result(mstEdges, totalCost);
    }
    
    // Example usage
    public static void main(String[] args) {
        int n = 5;
        List<Edge> edges = Arrays.asList(
            new Edge(0, 1, 2), new Edge(0, 3, 6), new Edge(1, 2, 3),
            new Edge(1, 3, 8), new Edge(1, 4, 5), new Edge(2, 4, 7)
        );
        
        Result result = kruskalsMST(n, edges);
        
        System.out.println("MST edges:");
        for (Edge edge : result.mstEdges) {
            System.out.println("(" + edge.u + ", " + edge.v + ", " + edge.weight + ")");
        }
        System.out.println("Total cost: " + result.totalCost);
    }
}
```

---

## When Greedy Works vs Fails

### ✅ Greedy Works (Optimal)
1. Activity Selection
2. Fractional Knapsack
3. Huffman Coding
4. Minimum Spanning Tree (Prim's, Kruskal's)
5. Dijkstra's Shortest Path (non-negative weights)

### ❌ Greedy Fails (Not Optimal)
1. **0/1 Knapsack** - Need Dynamic Programming
2. **Longest Path** - NP-hard
3. **Traveling Salesman** - NP-hard
4. **Graph Coloring** - Need backtracking
5. **Coin Change** (non-canonical systems) - Need DP

---

## Greedy vs Dynamic Programming

| Aspect | Greedy | Dynamic Programming |
|--------|--------|---------------------|
| Choice | Makes one choice at each step | Considers all choices |
| Reconsider | Never | Can reconsider |
| Complexity | Usually faster | Usually slower |
| Optimality | Not always optimal | Always optimal |
| Examples | Activity Selection | 0/1 Knapsack |

---

## Exam Tips

### 1. Identifying Greedy Problems
- Look for "maximum" or "minimum" in problem
- Check if greedy choice property holds
- Verify optimal substructure

### 2. Common Patterns
```python
# Pattern 1: Sort and select
items.sort(key=lambda x: some_criteria)
for item in items:
    if can_select(item):
        select(item)

# Pattern 2: Min heap approach
heap = []
while heap:
    current = heappop(heap)
    process(current)
    add_neighbors_to_heap()
```

### 3. Dry Run Tips
- Always sort first if needed
- Track the greedy choice at each step
- Verify no cycles (for graph problems)
- Calculate running total

### 4. Complexity Analysis
- Sorting: O(n log n)
- Heap operations: O(log n)
- Union-Find: O(α(n)) ≈ O(1)

### Key Points
1. Greedy makes locally optimal choice
2. Not guaranteed to find global optimum
3. Must prove greedy choice property
4. Usually faster than DP
5. Common in optimization problems
