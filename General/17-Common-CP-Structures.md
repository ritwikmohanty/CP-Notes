# COMMON CP DATA STRUCTURE PATTERNS

These are common combinations of STL containers used in competitive programming. Use the simplest representation that fits the ID range and operations you need.

## Graph Adjacency Lists

For vertices numbered `0..n-1`, a vector of vectors is the usual representation for an unweighted graph:

```cpp
int n, m;
cin >> n >> m;
vector<vector<int>> graph(n);

for (int i = 0; i < m; ++i) {
    int u, v;
    cin >> u >> v;
    graph[u].push_back(v);
    graph[v].push_back(u); // omit for a directed graph
}

for (int u = 0; u < n; ++u) {
    for (int v : graph[u]) {
        // process edge u -> v
    }
}
```

For weighted edges, store `(neighbor, weight)` pairs:

```cpp
vector<vector<pair<int, int>>> graph(n); // (neighbor, weight)
graph[u].push_back({v, weight});

for (const auto& [v, weight] : graph[u]) {
    // process edge u -> v with this weight
}
```

Adjacency lists use `O(n + m)` space and are a good default for sparse graphs. An adjacency matrix, `vector<vector<int>> matrix(n, vector<int>(n))`, uses `O(n^2)` space and is useful when the graph is dense or edge-existence checks must be constant time.

## Grouping Values by ID

Use a vector of vectors for dense IDs and a map/unordered map for sparse or arbitrary IDs:

```cpp
// Dense IDs: ids are in [0, n).
vector<vector<int>> by_id(n);
by_id[id].push_back(value);

// Sparse IDs, with sorted ID iteration.
map<int, vector<int>> ordered_by_id;
ordered_by_id[id].push_back(value);

// Sparse IDs, when iteration order does not matter.
unordered_map<int, vector<int>> by_sparse_id;
by_sparse_id[id].push_back(value);
```

Iterate over a map of ID to vector (C++17):

```cpp
for (const auto& [id, items] : ordered_by_id) {
    for (int item : items) {
        // use id and item
    }
}
```

## Common Nested Containers

```cpp
vector<pair<int, int>> edges;                    // list of (u, v) edges
vector<pair<long long, int>> events;             // (time/cost, id)
vector<array<int, 3>> points;                     // fixed-size triples
map<int, set<int>> neighbors;                     // unique sorted values per key
unordered_map<string, int> frequency;             // count each string
```

`pair` and `array` compare lexicographically, so they work naturally with `sort`. Use a small named `struct` instead when field names make the data easier to understand.

## Looping and Passing Nested Containers

Use references to avoid copying inner vectors or pairs:

```cpp
for (auto& row : grid) {             // mutable row
    for (int& value : row) ++value;
}

for (const auto& edge : edges) {     // read-only pair
    int u = edge.first;
    int v = edge.second;
}

void process(const vector<vector<int>>& graph) {
    // const reference avoids copying the whole graph
}
```

For index-dependent work, use indexed loops; range-based loops do not provide the element index automatically.
