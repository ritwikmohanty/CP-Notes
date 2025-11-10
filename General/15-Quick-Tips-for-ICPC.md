# QUICK TIPS FOR ICPC

### Common Patterns
```cpp
// Fast I/O
ios_base::sync_with_stdio(false);
cin.tie(NULL);

// Read until EOF
while (cin >> x) { /* process */ }

// Clear container efficiently
v.clear();
v.shrink_to_fit();          // release memory

// Remove duplicates
sort(v.begin(), v.end());
v.erase(unique(v.begin(), v.end()), v.end());

// Reverse iterate
for (auto it = v.rbegin(); it != v.rend(); ++it)

// Lambda comparator
auto cmp = [](const pair<int,int>& a, const pair<int,int>& b) {
    return a.first < b.first;
};
sort(v.begin(), v.end(), cmp);

// Check if element exists
if (m.find(key) != m.end()) { /* exists */ }
if (s.count(val)) { /* exists */ }

// Iterate map
for (auto& [key, val] : m) { /* use key, val */ }

// Two pointers
int l = 0, r = v.size() - 1;
while (l < r) {
    if (condition) l++;
    else r--;
}
```

### Time Complexity Quick Reference
| Operation | Vector | Deque | List | Set/Map | Unordered Set/Map |
|-----------|--------|-------|------|---------|-------------------|
| Access    | O(1)   | O(1)  | O(n) | O(log n)| O(1) avg          |
| Insert End| O(1)*  | O(1)  | O(1) | O(log n)| O(1) avg          |
| Insert    | O(n)   | O(n)  | O(1) | O(log n)| O(1) avg          |
| Delete End| O(1)   | O(1)  | O(1) | O(log n)| O(1) avg          |
| Delete    | O(n)   | O(n)  | O(1) | O(log n)| O(1) avg          |
| Search    | O(n)   | O(n)  | O(n) | O(log n)| O(1) avg          |

*amortized

### Space Complexity
- Vector: O(n)
- Deque: O(n)
- List: O(n) + overhead per node
- Set/Map: O(n) + overhead per node
- Unordered Set/Map: O(n) + bucket overhead

---

**Remember**: Practice these operations hands-on. Knowing the time complexity and when to use which container is crucial for ICPC success!