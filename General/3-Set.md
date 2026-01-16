# SET

### Declaration & Initialization
```cpp
#include <set>
set<int> s;                             // empty set
set<int> s = {1, 2, 3, 4};             // initializer list
set<int, greater<int>> s;               // descending order
```

### Core Operations
```cpp
// Modification
s.insert(x)             // O(log n) - returns pair<iterator, bool>
s.emplace(x)            // O(log n) - construct in place
s.erase(x)              // O(log n) - by value
s.erase(it)             // O(1) amortized - by iterator
s.erase(it1, it2)       // O(k) - erase range
s.clear()               // O(n) - remove all

// Search
s.find(x)               // O(log n) - returns iterator or end()
s.count(x)              // O(log n) - returns 0 or 1
s.contains(x)           // O(log n) - C++20, returns bool

// Size
s.size()                // number of elements
s.empty()               // returns true if empty
s.max_size()            // maximum possible size

// Bounds & Range
s.lower_bound(x)        // O(log n) - first >= x
s.upper_bound(x)        // O(log n) - first > x
s.equal_range(x)        // O(log n) - pair of bounds

// Iteration
for(int x : s)          // sorted order
s.begin(), s.end()      // forward iterators
s.rbegin(), s.rend()    // reverse iterators

// Other
s.swap(s2)              // O(1) - swap contents
s.merge(s2)             // O(n log n) - merge s2 into s
```

### Time Complexity Summary
- Insert/Delete/Search: O(log n)

//traversing all elememnts of set of pairs
```cpp
vector<pair<int, int>> sp = {{1, 2}, {3, 4}, {5, 6}};
for (const auto& p : sp) {
    cout << p.first << ", " << p.second << endl;
}
```         