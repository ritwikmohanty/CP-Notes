# VECTOR

### Declaration & Initialization
```cpp
#include <vector>
vector<int> v;                          // empty vector
vector<int> v(5);                       // size 5, default values
vector<int> v(5, 10);                   // size 5, all 10
vector<int> v = {1, 2, 3, 4};          // initializer list
vector<int> v({1, 2, 3});              // constructor style
```

### Core Operations
```cpp
// Access
v[i]                    // O(1) - no bounds check
v.at(i)                 // O(1) - with bounds check
v.front()               // first element
v.back()                // last element
v.data()                // pointer to underlying array

// Modification
v.push_back(x)          // O(1) amortized
v.pop_back()            // O(1)
v.insert(it, x)         // O(n) - insert before iterator
v.erase(it)             // O(n) - erase at iterator
v.erase(it1, it2)       // O(n) - erase range [it1, it2)
v.clear()               // O(n) - remove all elements
v.assign(n, val)        // replace with n copies of val
v.emplace_back(x)       // O(1) - construct in place
v.emplace(it, x)        // O(n) - construct before iterator

// Size & Capacity
v.size()                // number of elements
v.empty()               // returns true if empty
v.capacity()            // allocated capacity
v.max_size()            // maximum possible size
v.resize(n)             // change size to n
v.resize(n, val)        // resize and fill with val
v.reserve(n)            // reserve capacity
v.shrink_to_fit()       // reduce capacity to fit size

// Iteration
for(int x : v)          // range-based
for(auto it = v.begin(); it != v.end(); ++it)
v.begin(), v.end()      // forward iterators
v.rbegin(), v.rend()    // reverse iterators
v.cbegin(), v.cend()    // const iterators
v.crbegin(), v.crend()  // const reverse iterators

// Other
v.swap(v2)              // O(1) - swap contents
v1 == v2, v1 != v2      // comparison operators
v1 < v2, v1 > v2        // lexicographical comparison
```

### Time Complexity Summary
- Access: O(1)
- Insert/Delete at end: O(1) amortized
- Insert/Delete at position: O(n)
- Search: O(n)