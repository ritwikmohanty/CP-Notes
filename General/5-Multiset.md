# MULTISET

### Declaration & Initialization
```cpp
#include <set>
multiset<int> ms;                       // empty multiset
multiset<int> ms = {1, 2, 2, 3, 3, 3}; // allows duplicates
multiset<int, greater<int>> ms;         // descending
```

### Core Operations
```cpp
// Modification
ms.insert(x)            // O(log n) - allows duplicates
ms.emplace(x)           // O(log n)
ms.erase(x)             // O(k + log n) - removes ALL occurrences
ms.erase(it)            // O(1) amortized - single element
ms.clear()              // O(n)

// Search
ms.find(x)              // O(log n) - first occurrence
ms.count(x)             // O(k + log n) - number of occurrences
ms.equal_range(x)       // O(log n) - pair [first, last)

// Size
ms.size()               // total number of elements
ms.empty()               // returns true if empty

// Bounds
ms.lower_bound(x)       // O(log n)
ms.upper_bound(x)       // O(log n)

// Iteration
for(int x : ms)         // sorted order with duplicates
ms.begin(), ms.end()
ms.rbegin(), ms.rend()

// Other
ms.swap(ms2)            // O(1)
ms.merge(ms2)           // O(n log n)
```

### Time Complexity Summary
- Insert: O(log n)
- Delete single: O(1) amortized
- Delete all occurrences: O(k + log n)
- Search: O(log n)