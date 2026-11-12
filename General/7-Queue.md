# QUEUE

### Why & When to Use
- Use a FIFO queue when items must be processed in arrival order, especially for BFS and level-order traversal.
- Prefer `deque` if you need to inspect/iterate/index the sequence or modify both ends; a queue adapter exposes only front/back and push/pop.
- For repeated access to the best (minimum/maximum) item, use `priority_queue` instead.

### Declaration & Initialization
```cpp
#include <queue>
queue<int> q;                           // empty queue
```

### Core Operations
```cpp
// Access
q.front()               // first element (to be popped)
q.back()                // last element (most recently pushed)

// Modification
q.push(x)               // O(1) - add to back
q.pop()                 // O(1) - remove from front
q.emplace(x)            // O(1) - construct in place

// Size
q.size()                // number of elements
q.empty()               // returns true if empty

// Other
q.swap(q2)              // O(1) - swap contents

// NOTE: NO iteration support!
```

### Time Complexity Summary
- Push/Pop: O(1)
- Access front/back: O(1)
