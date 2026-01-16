# UNORDERED_MAP

### Declaration & Initialization
```cpp
#include <unordered_map>
unordered_map<int, string> m;                     // empty unordered map
unordered_map<int, string> m = {{1,"a"}, {2,"b"}}; // initializer list
```

### Core Operations
```cpp
// Access
m[key]                  // O(1) avg, O(n) worst - insert if not exists
m.at(key)               // O(1) avg, O(n) worst - throws if not exists
m.find(key)             // O(1) avg, O(n) worst - returns iterator or end()
m.count(key)            // O(1) avg, O(n) worst - returns 0 or 1

// Modification
m[key] = value          // O(1) avg, O(n) worst - insert or update
m.insert({key, val})    // O(1) avg, O(n) worst
m.insert(make_pair(k,v))// O(1) avg, O(n) worst
m.emplace(key, val)     // O(1) avg, O(n) worst - construct in place
m.erase(key)            // O(1) avg, O(n) worst - by key
m.erase(it)             // O(1) avg, O(n) worst - by iterator
m.erase(it1, it2)       // O(k) - erase range
m.clear()               // O(n) - remove all

// Size
m.size()                // number of elements
m.empty()               // returns true if empty
m.max_size()            // maximum possible size

// Iteration (UNORDERED - no guaranteed order)
for(auto& [key, val] : m)           // structured binding (C++17)
for(auto it = m.begin(); it != m.end(); ++it)
    // it->first (key), it->second (value)
m.begin(), m.end()      // unordered iteration
// m.rbegin(), m.rend()  // NOT AVAILABLE

// Hash Function & Buckets
m.bucket_count()        // number of buckets
m.max_bucket_count()    // maximum possible buckets
m.bucket_size(i)        // elements in bucket i
m.bucket(key)           // bucket index for key
m.load_factor()         // size / bucket_count
m.max_load_factor()     // maximum load factor
m.rehash(n)             // set minimum n buckets
m.reserve(n)            // prepare for n elements

// Other
m.swap(m2)              // O(1) - swap contents
m.hash_function()       // returns hash function
m.key_eq()              // returns equality function
```

### Time Complexity Summary
- Average: O(1) for Access/Insert/Delete/Search
- Worst case: O(n) when hash collisions occur

---

## MAP vs UNORDERED_MAP Comparison

| Feature | map | unordered_map |
|---------|-----|---------------|
| **Time Complexity** | O(log n) | O(1) avg, O(n) worst |
| **Internal Structure** | Red-Black Tree | Hash Table |
| **Order** | Sorted by key | No guaranteed order |
| **Range queries** | ✓ (lower_bound, upper_bound) | ✗ |
| **Reverse iteration** | ✓ (rbegin, rend) | ✗ |
| **Memory overhead** | Lower | Higher (hash table) |
| **Cache locality** | Good | Poor |
| **Predictable perf** | ✓ | ✗ (collision worst case) |

### When to use MAP
- Need sorted order of keys
- Need range queries (all keys in range)
- Need reverse iteration
- Want guaranteed O(log n) performance
- Custom comparators required
- Small datasets (overhead matters less)

### When to use UNORDERED_MAP
- Need fast average lookup/insert/delete
- Order doesn't matter
- Very large datasets
- Hash function works well for your key type
- No need for range queries

### Example: Performance Comparison
```cpp
// For 1M lookups on 100K elements
map<int, int> m;              // ~20M operations (100K * log 100K ≈ 1.6M per lookup)
unordered_map<int, int> um;   // ~1-5M operations (very fast with good hash)

// But with poor hash function or adversarial input:
unordered_map performance degrades to O(n) per operation
map remains O(log n) - predictable and stable
```

### Custom Hash Function (if needed)
```cpp
struct CustomHash {
    size_t operator()(const int& key) const {
        return hash<int>{}(key);
    }
};

unordered_map<int, string, CustomHash> m;
```

### Initialization with custom comparator
```cpp
// Can use default hash functions for: int, string, float, double, etc.
unordered_map<string, int> m;  // string keys work out of the box

// For custom types, need to define hash function
```
