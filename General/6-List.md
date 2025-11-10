# LIST

### Declaration & Initialization
```cpp
#include <list>
list<int> l;                            // empty list
list<int> l(5);                         // 5 default values
list<int> l(5, 10);                     // 5 elements, all 10
list<int> l = {1, 2, 3, 4};            // initializer list
```

### Core Operations
```cpp
// Access (NO random access!)
l.front()               // first element
l.back()                // last element

// Modification
l.push_back(x)          // O(1)
l.push_front(x)         // O(1)
l.pop_back()            // O(1)
l.pop_front()           // O(1)
l.insert(it, x)         // O(1) - insert before iterator
l.erase(it)             // O(1) - erase at iterator
l.erase(it1, it2)       // O(k) - erase range
l.clear()               // O(n)
l.emplace_back(x)       // O(1)
l.emplace_front(x)      // O(1)
l.emplace(it, x)        // O(1)
l.assign(n, val)        // replace with n copies

// Size
l.size()                // number of elements
l.empty()               // returns true if empty
l.max_size()            // maximum possible size
l.resize(n)             // change size
l.resize(n, val)        // resize with value

// List-specific operations
l.remove(val)           // O(n) - remove all == val
l.remove_if(pred)       // O(n) - remove if predicate true
l.unique()              // O(n) - remove consecutive duplicates
l.sort()                // O(n log n) - sort list
l.sort(comp)            // O(n log n) - custom comparator
l.reverse()             // O(n) - reverse order
l.merge(l2)             // O(n) - merge sorted lists
l.splice(it, l2)        // O(1) - move all from l2
l.splice(it, l2, it2)   // O(1) - move single element
l.splice(it, l2, it2, it3) // O(k) - move range

// Iteration
for(int x : l)
l.begin(), l.end()      // bidirectional iterators
l.rbegin(), l.rend()

// Other
l.swap(l2)              // O(1)
```

### Time Complexity Summary
- Insert/Delete at ends: O(1)
- Insert/Delete at position: O(1) with iterator
- Random access: Not supported
- Search: O(n)
- Sort: O(n log n)