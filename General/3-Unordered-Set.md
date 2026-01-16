# UNORDERED_SET

### Declaration & Initialization
```cpp
#include <unordered_set>
unordered_set<int> s;                    // empty unordered set
unordered_set<int> s = {1, 2, 3, 4};    // initializer list
// unordered_set<int, greater<int>> s;  // NOT AVAILABLE - no custom comparator
```

### Core Operations
```cpp
// Modification
s.insert(x)             // O(1) avg, O(n) worst - returns pair<iterator, bool>
s.emplace(x)            // O(1) avg, O(n) worst - construct in place
s.erase(x)              // O(1) avg, O(n) worst - by value
s.erase(it)             // O(1) avg, O(n) worst - by iterator
s.erase(it1, it2)       // O(k) - erase range
s.clear()               // O(n) - remove all

// Search
s.find(x)               // O(1) avg, O(n) worst - returns iterator or end()
s.count(x)              // O(1) avg, O(n) worst - returns 0 or 1
s.contains(x)           // O(1) avg, O(n) worst - C++20, returns bool

// Size
s.size()                // number of elements
s.empty()               // returns true if empty
s.max_size()            // maximum possible size

// Bounds & Range (NOT AVAILABLE)
// s.lower_bound(x)     // ✗ NOT AVAILABLE
// s.upper_bound(x)     // ✗ NOT AVAILABLE
// s.equal_range(x)     // ✗ NOT AVAILABLE

// Iteration (UNORDERED - no guaranteed order)
for(int x : s)          // unordered iteration
s.begin(), s.end()      // unordered iterators
// s.rbegin(), s.rend() // NOT AVAILABLE - no reverse iterators

// Hash Function & Buckets
s.bucket_count()        // number of buckets
s.max_bucket_count()    // maximum possible buckets
s.bucket_size(i)        // elements in bucket i
s.bucket(x)             // bucket index for element x
s.load_factor()         // size / bucket_count
s.max_load_factor()     // maximum load factor
s.rehash(n)             // set minimum n buckets
s.reserve(n)            // prepare for n elements

// Other
s.swap(s2)              // O(1) - swap contents
s.hash_function()       // returns hash function
s.key_eq()              // returns equality function
// s.merge(s2)          // ✗ NOT AVAILABLE
```

### Time Complexity Summary
- Average: O(1) for Insert/Delete/Search
- Worst case: O(n) when hash collisions occur

---

## SET vs UNORDERED_SET Comparison

| Feature | set | unordered_set |
|---------|-----|---------------|
| **Time Complexity** | O(log n) | O(1) avg, O(n) worst |
| **Internal Structure** | Red-Black Tree | Hash Table |
| **Order** | Sorted order | No guaranteed order |
| **Range queries** | ✓ (lower_bound, upper_bound) | ✗ |
| **Reverse iteration** | ✓ (rbegin, rend) | ✗ |
| **Custom comparator** | ✓ | ✗ (only hash) |
| **Memory overhead** | Lower | Higher |
| **Cache locality** | Good | Poor |
| **Predictable perf** | ✓ Guaranteed O(log n) | ✗ Worst case O(n) |
| **Duplicate elements** | ✗ | ✗ (both don't allow) |

### When to use SET
- Need sorted/ordered elements
- Need range queries (all elements between x and y)
- Need reverse iteration
- Want guaranteed O(log n) performance
- Custom comparators required (e.g., descending order)
- Predictability is critical

### When to use UNORDERED_SET
- Need fastest average lookup/insert/delete
- Order doesn't matter
- Very large datasets
- Hash function works well for your elements
- No need for range queries
- Working with large datasets where O(1) vs O(log n) matters

### Example: Performance Comparison
```cpp
// Find all elements - 100K lookups on 10K elements
set<int> s;              // ~1.3M operations (100K * log 10K ≈ 13 per lookup)
unordered_set<int> us;   // ~100K operations (very fast with good hash)

// But with poor hash function:
unordered_set performance can degrade to ~1M operations (O(n) per lookup)
set remains predictable at ~1.3M operations
```

### Common Use Cases

**Use SET when:**
```cpp
// Find all numbers in range [10, 100]
set<int> s = {5, 10, 15, 20, ..., 100, 105};
auto lower = s.lower_bound(10);
auto upper = s.upper_bound(100);
// Can iterate from lower to upper

// Descending order elements
set<int, greater<int>> s;

// Leaderboard (sorted by scores)
set<pair<int, int>> scores;  // (score, player_id)
```

**Use UNORDERED_SET when:**
```cpp
// Check if element exists in massive dataset
unordered_set<int> visited;  // O(1) lookup
for(auto x : huge_data) {
    if(visited.count(x)) continue;  // Fast check
    visited.insert(x);
}

// Remove duplicates from array (order doesn't matter)
unordered_set<int> unique(arr.begin(), arr.end());

// Frequency counting
unordered_set<string> words;
```

### Traversing all elements
```cpp
// Set of pairs
set<pair<int, int>> sp = {{1, 2}, {3, 4}, {5, 6}};
for (const auto& p : sp) {
    cout << p.first << ", " << p.second << endl;
}

// Unordered set of pairs (need custom hash)
struct PairHash {
    size_t operator()(const pair<int, int>& p) const {
        return hash<int>{}(p.first) ^ (hash<int>{}(p.second) << 1);
    }
};
unordered_set<pair<int, int>, PairHash> usp;
```

### Hash Function for Custom Types
```cpp
struct MyType {
    int a, b;
};

struct MyTypeHash {
    size_t operator()(const MyType& x) const {
        return hash<int>{}(x.a) ^ (hash<int>{}(x.b) << 1);
    }
};

struct MyTypeEqual {
    bool operator()(const MyType& a, const MyType& b) const {
        return a.a == b.a && a.b == b.b;
    }
};

unordered_set<MyType, MyTypeHash, MyTypeEqual> s;
```
