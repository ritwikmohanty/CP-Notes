# VECTOR

### Why & When to Use
- Prefer `vector` as the default sequence container: it is compact, cache-friendly, and gives O(1) indexing.
- Use it when you append mostly at the end, need sorting/binary search, or need to pass a contiguous range to algorithms.
- Choose `deque` when you also need frequent pushes/pops at the front; choose `list` only when you already have an iterator to frequent middle insertions/removals.
- For fixed-size small arrays, `array<T, N>` or a plain array can avoid dynamic allocation; for grid rows, `vector<string>` is often convenient.

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

---

## 2D VECTOR

### Why & When to Use
- Use a 2D vector for a dynamically sized table or grid, especially when dimensions are known at runtime.
- Prefer `vector<string>` for character grids; use a flat `vector<T>` when a very large rectangular grid needs contiguous storage and better cache locality.

### Declaration & Initialization
```cpp
int n = 3, m = 4;

vector<vector<int>> a;                  // empty 2D vector
vector<vector<int>> a(n);               // n empty rows
vector<vector<int>> a(n, vector<int>(m));       // n x m, default 0
vector<vector<int>> a(n, vector<int>(m, -1));   // n x m, all -1

vector<vector<int>> grid = {
    {1, 2, 3},
    {4, 5, 6}
};
```

### Access & Dimensions
```cpp
a[i][j]                 // element at row i, column j
a.size()                // number of rows
a[0].size()             // number of columns, if a is non-empty

int n = a.size();
int m = a.empty() ? 0 : a[0].size();
```

### Input & Output
```cpp
vector<vector<int>> a(n, vector<int>(m));

for(int i = 0; i < n; i++) {
    for(int j = 0; j < m; j++) {
        cin >> a[i][j];
    }
}

for(int i = 0; i < n; i++) {
    for(int j = 0; j < m; j++) {
        cout << a[i][j] << " ";
    }
    cout << '\n';
}
```

### Row Operations
```cpp
a.push_back(vector<int>(m, 0));     // add new row
a[i].push_back(x);                  // add element to row i
a[i].pop_back();                    // remove last element from row i
a.erase(a.begin() + i);             // erase row i, O(n * row_size)
a[i].erase(a[i].begin() + j);       // erase column j only from row i

sort(a[i].begin(), a[i].end());     // sort one row
reverse(a[i].begin(), a[i].end());  // reverse one row
```

### Fill / Reset
```cpp
// reset existing n x m matrix to 0
for(int i = 0; i < n; i++) {
    fill(a[i].begin(), a[i].end(), 0);
}

// rebuild with new size/value
a.assign(n, vector<int>(m, -1));
```

### Traversing Grid Neighbours
```cpp
int dx[4] = {-1, 0, 1, 0};
int dy[4] = {0, 1, 0, -1};

for(int dir = 0; dir < 4; dir++) {
    int ni = i + dx[dir];
    int nj = j + dy[dir];

    if(ni >= 0 && ni < n && nj >= 0 && nj < m) {
        // valid neighbour: a[ni][nj]
    }
}
```

### 8 Directions
```cpp
int dx[8] = {-1,-1,-1, 0, 0, 1, 1, 1};
int dy[8] = {-1, 0, 1,-1, 1,-1, 0, 1};
```

### Sorting 2D Vectors
```cpp
sort(a.begin(), a.end());       // lexicographical row-wise sort

// sort by first column, then second column
sort(a.begin(), a.end(), [](const vector<int>& x, const vector<int>& y) {
    if(x[0] != y[0]) return x[0] < y[0];
    return x[1] < y[1];
});

// sort by row sum
sort(a.begin(), a.end(), [](const vector<int>& x, const vector<int>& y) {
    return accumulate(x.begin(), x.end(), 0) < accumulate(y.begin(), y.end(), 0);
});
```

### Passing To Functions
```cpp
void solve(vector<vector<int>>& a) {          // pass by reference, can modify
    int n = a.size();
    int m = a.empty() ? 0 : a[0].size();
}

void print(const vector<vector<int>>& a) {    // const ref, cannot modify
    for(const auto& row : a) {
        for(int x : row) cout << x << " ";
        cout << '\n';
    }
}
```

### Common CP Patterns
```cpp
// visited matrix
vector<vector<int>> vis(n, vector<int>(m, 0));

// distance matrix for BFS
vector<vector<int>> dist(n, vector<int>(m, -1));

// char grid
vector<string> grid(n);
for(int i = 0; i < n; i++) cin >> grid[i];
grid[i][j]       // char access

// prefix sum matrix, 1-based
vector<vector<long long>> pref(n + 1, vector<long long>(m + 1, 0));

for(int i = 1; i <= n; i++) {
    for(int j = 1; j <= m; j++) {
        pref[i][j] = a[i-1][j-1]
                   + pref[i-1][j]
                   + pref[i][j-1]
                   - pref[i-1][j-1];
    }
}

// sum of rectangle: rows r1..r2, cols c1..c2, 0-based inclusive
long long sum = pref[r2+1][c2+1]
              - pref[r1][c2+1]
              - pref[r2+1][c1]
              + pref[r1][c1];
```

### Important Notes / Gotchas
- `vector<vector<int>>` rows are separate vectors, not one continuous 2D array.
- Always check `a.empty()` before using `a[0].size()`.
- Passing `vector<vector<int>> a` copies the whole matrix. Use `&` or `const &`.
- `a.resize(n, vector<int>(m))` changes rows, but existing rows keep their old column sizes.
- For a fixed rectangular grid, make every row the same size.
- Erasing from the middle is costly. Prefer marking/deleting later if many removals are needed.
- For huge grids, use global/static arrays if memory and speed are critical.
- `vector<string>` is often cleaner than `vector<vector<char>>` for character grids.
- Use `long long` for prefix sums, path sums, and large values.
- For DP tables, initialize carefully:
```cpp
const long long INF = 4e18;
vector<vector<long long>> dp(n, vector<long long>(m, INF));
```

---

## 3D VECTOR

Use a 3D vector for a runtime-sized collection of 2D tables, such as DP over `(layer, row, column)`. For fixed small dimensions, a built-in array can be simpler and faster.

```cpp
int x = 2, y = 3, z = 4;
vector<vector<vector<int>>> a(x, vector<vector<int>>(y, vector<int>(z, 0)));

a[i][j][k] = 7; // access one element

for (int i = 0; i < x; ++i)
    for (int j = 0; j < y; ++j)
        for (int k = 0; k < z; ++k)
            cin >> a[i][j][k];
```

To initialize every element to another value, replace the last `0` with that value. The nested vectors allocate rows separately; for a large fixed rectangular volume, a flat vector can improve locality:

```cpp
vector<int> flat(x * y * z, 0);
auto index = [=](int i, int j, int k) { return (i * y + j) * z + k; };
flat[index(i, j, k)] = 7;
```
