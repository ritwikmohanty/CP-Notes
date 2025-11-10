# STACK

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