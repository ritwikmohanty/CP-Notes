# PAIR

### Declaration & Initialization
```cpp
#include <utility>
pair<int, string> p;                    // default values
pair<int, string> p = {1, "hello"};    // initializer list
pair<int, string> p(1, "hello");       // constructor
auto p = make_pair(1, "hello");        // using make_pair
```

### Core Operations
```cpp
// Access
p.first                 // first element
p.second                // second element

// Modification
p.first = new_val;      // update first
p.second = new_val;     // update second

// Comparison (lexicographical)
p1 == p2                // compares both elements
p1 != p2
p1 < p2                 // compares first, then second
p1 > p2
p1 <= p2
p1 >= p2

// Unpacking
int x; string y;
tie(x, y) = p;          // unpack into variables
auto [x, y] = p;        // structured binding (C++17)

// Other
p.swap(p2)              // swap contents
make_pair(a, b)         // create pair without explicit types
```

### Nested Pairs
```cpp
pair<int, pair<int, int>> p = {1, {2, 3}};
p.first                 // 1
p.second.first          // 2
p.second.second         // 3
```