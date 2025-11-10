# MULTIMAP

### Declaration & Initialization
```cpp
#include <map>
multimap<int, string> mm;               // empty multimap
multimap<int, string> mm = {{1,"a"}, {1,"b"}, {2,"c"}};
multimap<int, string, greater<int>> mm; // descending
```

### Core Operations
```cpp
// Modification (NO [] operator!)
mm.insert({key, val})   // O(log n) - allows duplicates
mm.emplace(key, val)    // O(log n)
mm.erase(key)           // O(k + log n) - removes ALL with key
mm.erase(it)            // O(1) amortized - single element
mm.clear()              // O(n)

// Search
mm.find(key)            // O(log n) - first occurrence
mm.count(key)           // O(k + log n) - number of elements with key
mm.equal_range(key)     // O(log n) - pair of iterators [first, last)

// Size
mm.size()               // total number of elements
mm.empty()              // returns true if empty

// Bounds
mm.lower_bound(key)     // O(log n)
mm.upper_bound(key)     // O(log n)

// Iteration
for(auto& [key, val] : mm)
mm.begin(), mm.end()
mm.rbegin(), mm.rend()

// Other
mm.swap(mm2)            // O(1)
mm.merge(mm2)           // O(n log n)
```

### Time Complexity Summary
- Insert: O(log n)
- Delete single: O(1) amortized
- Delete all with key: O(k + log n)
- Search: O(log n)