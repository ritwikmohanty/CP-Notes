# QUEUE

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