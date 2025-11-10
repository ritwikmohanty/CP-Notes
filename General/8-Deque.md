# DEQUE

### Declaration & Initialization
```cpp
#include <deque>
deque<int> dq;                          // empty deque
deque<int> dq(5);                       // size 5
deque<int> dq(5, 10);                   // size 5, all 10
deque<int> dq = {1, 2, 3, 4};          // initializer list
```

### Core Operations
```cpp
// Access
dq[i]                   // O(1) - no bounds check
dq.at(i)                // O(1) - with bounds check
dq.front()              // first element
dq.back()               // last element

// Modification (both ends!)
dq.push_back(x)         // O(1)
dq.push_front(x)        // O(1)
dq.pop_back()           // O(1)
dq.pop_front()           // O(1)
dq.insert(it, x)        // O(n) - insert before iterator
dq.erase(it)            // O(n) - erase at iterator
dq.erase(it1, it2)      // O(n) - erase range
dq.clear()              // O(n)
dq.emplace_back(x)      // O(1)
dq.emplace_front(x)     // O(1)
dq.emplace(it, x)       // O(n)

// Size
dq.size()               // number of elements
dq.empty()              // returns true if empty
dq.max_size()           // maximum possible size
dq.resize(n)            // change size
dq.resize(n, val)       // resize with value
dq.shrink_to_fit()      // reduce memory usage

// Iteration
for(int x : dq)
dq.begin(), dq.end()    // random access iterators
dq.rbegin(), dq.rend()
dq.cbegin(), dq.cend()
dq.crbegin(), dq.crend()

// Other
dq.swap(dq2)            // O(1)
dq.assign(n, val)       // replace with n copies
```

### Time Complexity Summary
- Access: O(1)
- Insert/Delete at ends: O(1)
- Insert/Delete at position: O(n)