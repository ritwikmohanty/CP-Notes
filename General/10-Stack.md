# STACK

### Why & When to Use
- Use a LIFO stack for nested/recent-first work: DFS, iterative tree traversal, bracket matching, expression parsing, and monotonic-stack problems.
- Prefer the `stack` adapter when only top/push/pop are needed; use a vector when you also need indexing or iteration.
- Use a queue for FIFO processing instead.

### Declaration & Initialization
```cpp
#include <stack>
stack<int> st;                          // empty stack
```

### Core Operations
```cpp
// Access
st.top()                // O(1) - top element

// Modification
st.push(x)              // O(1) - add to top
st.pop()                // O(1) - remove from top
st.emplace(x)           // O(1) - construct in place

// Size
st.size()               // number of elements
st.empty()              // returns true if empty

// Other
st.swap(st2)            // O(1) - swap contents

// NOTE: NO iteration support!
```

### Time Complexity Summary
- Push/Pop: O(1)
- Access top: O(1)
