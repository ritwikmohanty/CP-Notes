# MAP

### Why & When to Use
- Use `map` when keys must stay sorted, you need predecessor/successor or range queries (`lower_bound`/`upper_bound`), or you want predictable O(log n) operations.
- Prefer it over `unordered_map` when order matters or inputs may cause hash-table slowdowns; use a vector plus sorting/compression if keys are known offline and performance is critical.
- Use `unordered_map` when you only need exact-key lookup and average speed matters more than order.

### Map from ID to a List of Values

Use `map<int, vector<int>>` when each ID is associated with a group of values and IDs should be processed in sorted order. Use `unordered_map<int, vector<int>>` if order is irrelevant, or `vector<vector<int>>` when IDs are dense from `0` to `n-1`.

```cpp
map<int, vector<int>> values;
values[10].push_back(3);
values[10].push_back(8);
values[20] = {4, 5};

// Structured binding: id is the key, vals is its vector.
for (const auto& [id, vals] : values) {
    cout << "id " << id << ":";
    for (int value : vals) cout << ' ' << value;
    cout << '\n';
}

// If values may be modified, use auto& instead of const auto&.
for (auto& [id, vals] : values) {
    vals.push_back(0);
}
```

For C++14 or earlier, iterate without structured bindings:

```cpp
for (const auto& entry : values) {
    int id = entry.first;
    const vector<int>& vals = entry.second;
}
```

### Declaration & Initialization
```cpp
#include <map>
map<int, string> m;                     // empty map
map<int, string> m = {{1,"a"}, {2,"b"}}; // initializer list
map<int, string, greater<int>> m;       // descending order
```

### Core Operations
```cpp
// Access
m[key]                  // O(log n) - insert if not exists
m.at(key)               // O(log n) - throws if not exists
m.find(key)             // O(log n) - returns iterator or end()
m.count(key)            // O(log n) - returns 0 or 1

// Modification
m[key] = value          // O(log n) - insert or update
m.insert({key, val})    // O(log n)
m.insert(make_pair(k,v))// O(log n)
m.emplace(key, val)     // O(log n) - construct in place
m.erase(key)            // O(log n) - by key
m.erase(it)             // O(1) amortized - by iterator
m.erase(it1, it2)       // O(k + log n) - erase range
m.clear()               // O(n) - remove all

// Size
m.size()                // number of elements
m.empty()               // returns true if empty
m.max_size()            // maximum possible size

// Bounds & Range
m.lower_bound(key)      // O(log n) - first >= key
m.upper_bound(key)      // O(log n) - first > key
m.equal_range(key)      // O(log n) - pair of bounds

// Iteration
for(auto& [key, val] : m)           // structured binding (C++17)
for(auto it = m.begin(); it != m.end(); ++it)
    // it->first (key), it->second (value)
m.begin(), m.end()      // sorted order iterators
m.rbegin(), m.rend()    // reverse iterators

// Other
m.swap(m2)              // O(1) - swap contents
m.merge(m2)             // O(n log n) - merge m2 into m
m.key_comp()            // returns comparison object
m.value_comp()          // returns value comparison object
```

### Time Complexity Summary
- Access/Insert/Delete: O(log n)
- Search: O(log n)
