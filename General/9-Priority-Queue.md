# PRIORITY QUEUE

### Declaration & Initialization
```cpp
#include <queue>
priority_queue<int> pq;                     // max heap (default)
priority_queue<int, vector<int>, greater<int>> pq; // min heap
```

### Core Operations
```cpp
// Access
pq.top()                // O(1) - highest priority element

// Modification
pq.push(x)              // O(log n) - insert element
pq.pop()                // O(log n) - remove top element
pq.emplace(x)           // O(log n) - construct in place

// Size
pq.size()               // number of elements
pq.empty()               // returns true if empty

// Other
pq.swap(pq2)            // O(1) - swap contents

// NOTE: NO iteration support!
```

### Custom Comparator
```cpp
// Using lambda (C++11)
auto cmp = [](int a, int b) { return a > b; }; // min heap
priority_queue<int, vector<int>, decltype(cmp)> pq(cmp);

// Using struct
struct Compare {
    bool operator()(int a, int b) { return a > b; }
};
priority_queue<int, vector<int>, Compare> pq;
```

### Time Complexity Summary
- Insert: O(log n)
- Delete top: O(log n)
- Access top: O(1)