# C++ STL ICPC Quick Reference

## Table of Contents
0. [CP Boilerplate & Fast I/O](#cp-boilerplate--fast-io)
1. [Vector](#vector)
2. [Map](#map)
2.5. [Unordered Map](#unordered-map)
3. [Set](#set)
3.5. [Unordered Set](#unordered-set)
4. [Multimap](#multimap)
5. [Multiset](#multiset)
6. [List](#list)
7. [Queue](#queue)
8. [Deque](#deque)
9. [Priority Queue](#priority-queue)
10. [Stack](#stack)
11. [Pair](#pair)
12. [Iterators](#iterators)
13. [STL Algorithms](#stl-algorithms)
14. [Exception Handling](#exception-handling)
15. [Quick Tips for ICPC](#quick-tips-for-icpc)
16. [String](#string)

---

## CP BOILERPLATE & FAST I/O

### Basic Boilerplate
```cpp
#include <bits/stdc++.h>
using namespace std;

// Type definitions
typedef long long ll;
typedef long double ld;
typedef pair<int, int> pii;
typedef pair<ll, ll> pll;
typedef vector<int> vi;
typedef vector<ll> vll;
typedef vector<pii> vpii;
typedef vector<pll> vpll;
typedef vector<string> vs;
typedef unordered_map<int, int> umii;
typedef unordered_map<ll, ll> umll;
typedef map<int, int> mii;
typedef map<ll, ll> mll;
typedef set<int> si;
typedef set<ll> sll;

// Macros
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define sz(x) (int)((x).size())
#define all(x) (x).begin(), (x).end()
#define rall(x) (x).rbegin(), (x).rend()
#define rep(i, a, b) for(int i = (a); i < (b); i++)
#define per(i, a, b) for(int i = (a); i >= (b); i--)
#define trav(a, x) for(auto& a : x)
#define endl '\n'
#define MOD 1000000007
#define INF 1e18
#define PI 3.1415926535897932384626

// Debug
#define deb(x) cout << #x << " = " << x << endl
#define deb2(x, y) cout << #x << " = " << x << ", " << #y << " = " << y << endl

// Fast I/O
void fastIO() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    cout.tie(NULL);
}

// Main template
int main() {
    fastIO();
    
    int t = 1;
    // cin >> t;  // Uncomment for multiple test cases
    
    while(t--) {
        // Your code here
    }
    
    return 0;
}
```

### Minimal Boilerplate
```cpp
#include <bits/stdc++.h>
using namespace std;

#define ll long long
#define vi vector<int>
#define vll vector<ll>
#define pb push_back
#define all(x) x.begin(), x.end()
#define rep(i,n) for(int i=0;i<n;i++)

void solve() {
    // Your solution here
}

int main() {
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    
    int t; cin >> t;
    while(t--) solve();
    
    return 0;
}
```

### Fast I/O Techniques

#### Standard Fast I/O
```cpp
// Basic - Always use these
ios_base::sync_with_stdio(false);
cin.tie(NULL);
cout.tie(NULL);

// For interactive problems, you may need:
cout << flush;  // or endl (which auto-flushes)
```

#### Custom Fast Input (Fastest)
```cpp
// Ultra-fast integer input
inline int fastRead() {
    int x = 0, neg = 0;
    char c = getchar_unlocked();
    while(c < '0' || c > '9') {
        if(c == '-') neg = 1;
        c = getchar_unlocked();
    }
    while(c >= '0' && c <= '9') {
        x = (x << 3) + (x << 1) + c - '0';
        c = getchar_unlocked();
    }
    return neg ? -x : x;
}

// Ultra-fast output
inline void fastWrite(int x) {
    if(x < 0) {
        putchar_unlocked('-');
        x = -x;
    }
    if(x > 9) fastWrite(x / 10);
    putchar_unlocked(x % 10 + '0');
}
```

#### File I/O
```cpp
void setIO(string name = "") {
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    
    if(sz(name)) {
        freopen((name + ".in").c_str(), "r", stdin);
        freopen((name + ".out").c_str(), "w", stdout);
    }
}

// Usage: setIO("problem");
```

### Utility Functions

#### Mathematical Functions
```cpp
// GCD and LCM
ll gcd(ll a, ll b) { return b ? gcd(b, a % b) : a; }
ll lcm(ll a, ll b) { return (a / gcd(a, b)) * b; }

// Power with modulo
ll power(ll a, ll b, ll mod = MOD) {
    ll res = 1;
    a %= mod;
    while(b > 0) {
        if(b & 1) res = (res * a) % mod;
        a = (a * a) % mod;
        b >>= 1;
    }
    return res;
}

// Modular inverse (using Fermat's little theorem)
ll modinv(ll a, ll mod = MOD) {
    return power(a, mod - 2, mod);
}

// Binary exponentiation
ll binpow(ll a, ll b) {
    ll res = 1;
    while(b > 0) {
        if(b & 1) res *= a;
        a *= a;
        b >>= 1;
    }
    return res;
}

// Check if prime
bool isPrime(ll n) {
    if(n <= 1) return false;
    if(n <= 3) return true;
    if(n % 2 == 0 || n % 3 == 0) return false;
    for(ll i = 5; i * i <= n; i += 6)
        if(n % i == 0 || n % (i + 2) == 0)
            return false;
    return true;
}

// Factorial with modulo
ll factorial(ll n, ll mod = MOD) {
    ll res = 1;
    for(ll i = 2; i <= n; i++)
        res = (res * i) % mod;
    return res;
}

// nCr with modulo
ll nCr(ll n, ll r, ll mod = MOD) {
    if(r > n) return 0;
    ll num = 1, den = 1;
    for(ll i = 0; i < r; i++) {
        num = (num * (n - i)) % mod;
        den = (den * (i + 1)) % mod;
    }
    return (num * modinv(den, mod)) % mod;
}
```

#### Array/Vector Utilities
```cpp
// Print vector
template<typename T>
void print(vector<T>& v) {
    for(auto x : v) cout << x << " ";
    cout << endl;
}

// Print 2D vector
template<typename T>
void print2D(vector<vector<T>>& v) {
    for(auto& row : v) {
        for(auto x : row) cout << x << " ";
        cout << endl;
    }
}

// Read vector
template<typename T>
void read(vector<T>& v, int n) {
    v.resize(n);
    for(int i = 0; i < n; i++) cin >> v[i];
}

// Sum of vector
template<typename T>
T sum(vector<T>& v) {
    return accumulate(all(v), (T)0);
}

// Max/Min in vector
template<typename T>
T vmax(vector<T>& v) {
    return *max_element(all(v));
}

template<typename T>
T vmin(vector<T>& v) {
    return *min_element(all(v));
}

// Unique elements count
template<typename T>
int unique_count(vector<T> v) {
    sort(all(v));
    return unique(all(v)) - v.begin();
}
```

#### String Utilities
```cpp
// Split string by delimiter
vector<string> split(string s, char delim) {
    vector<string> res;
    stringstream ss(s);
    string token;
    while(getline(ss, token, delim))
        res.pb(token);
    return res;
}

// String to lowercase/uppercase
string toLower(string s) {
    transform(all(s), s.begin(), ::tolower);
    return s;
}

string toUpper(string s) {
    transform(all(s), s.begin(), ::toupper);
    return s;
}

// Check if palindrome
bool isPalindrome(string s) {
    int n = sz(s);
    rep(i, n/2) if(s[i] != s[n-i-1]) return false;
    return true;
}
```

#### Bit Manipulation
```cpp
// Count set bits
int countSetBits(ll n) {
    return __builtin_popcountll(n);
}

// Check if kth bit is set
bool isKthBitSet(ll n, int k) {
    return (n & (1LL << k)) != 0;
}

// Set kth bit
ll setKthBit(ll n, int k) {
    return n | (1LL << k);
}

// Clear kth bit
ll clearKthBit(ll n, int k) {
    return n & ~(1LL << k);
}

// Toggle kth bit
ll toggleKthBit(ll n, int k) {
    return n ^ (1LL << k);
}

// Check if power of 2
bool isPowerOf2(ll n) {
    return n > 0 && (n & (n - 1)) == 0;
}

// MSB position (0-indexed from right)
int msbPos(ll n) {
    return 63 - __builtin_clzll(n);
}
```

#### Graph Utilities
```cpp
// Adjacency list representation
typedef vector<vi> graph;

// Create graph with n nodes
graph createGraph(int n) {
    return graph(n);
}

// Add edge (undirected)
void addEdge(graph& g, int u, int v) {
    g[u].pb(v);
    g[v].pb(u);
}

// Add directed edge
void addDirectedEdge(graph& g, int u, int v) {
    g[u].pb(v);
}

// Weighted graph
typedef vector<vector<pii>> wgraph;

void addWeightedEdge(wgraph& g, int u, int v, int w) {
    g[u].pb({v, w});
    g[v].pb({u, w});
}
```

#### Coordinate Compression
```cpp
// Compress coordinates
void compress(vi& v) {
    vi temp = v;
    sort(all(temp));
    temp.erase(unique(all(temp)), temp.end());
    
    for(int& x : v) {
        x = lower_bound(all(temp), x) - temp.begin();
    }
}
```

### Common Tricks

#### Grid Movement (4 directions)
```cpp
int dx[] = {-1, 1, 0, 0};
int dy[] = {0, 0, -1, 1};

bool isValid(int x, int y, int n, int m) {
    return x >= 0 && x < n && y >= 0 && y < m;
}

// Usage in BFS/DFS
for(int i = 0; i < 4; i++) {
    int nx = x + dx[i];
    int ny = y + dy[i];
    if(isValid(nx, ny, n, m)) {
        // process
    }
}
```

#### Grid Movement (8 directions)
```cpp
int dx[] = {-1, -1, -1, 0, 0, 1, 1, 1};
int dy[] = {-1, 0, 1, -1, 1, -1, 0, 1};
```

#### Reading Multiple Test Cases
```cpp
// Method 1: Known number of test cases
int t; cin >> t;
while(t--) {
    solve();
}

// Method 2: Until EOF
int n;
while(cin >> n) {
    // process
}

// Method 3: Until specific input
int n;
while(cin >> n && n != 0) {
    // process
}
```

#### Precision Setting
```cpp
// Set decimal precision
cout << fixed << setprecision(10);

// Alternative
printf("%.10lf\n", value);
```

#### Timing Code
```cpp
#include <chrono>

auto start = chrono::high_resolution_clock::now();
// Your code here
auto end = chrono::high_resolution_clock::now();
auto duration = chrono::duration_cast<chrono::milliseconds>(end - start);
cout << "Time: " << duration.count() << "ms" << endl;
```

#### Random Number Generation
```cpp
#include <random>

mt19937 rng(chrono::steady_clock::now().time_since_epoch().count());

// Random integer in range [l, r]
int randomInt(int l, int r) {
    return uniform_int_distribution<int>(l, r)(rng);
}

// Random shuffle
shuffle(all(v), rng);
```

### Common STL Tricks

#### Using Set as Priority Queue Alternative
```cpp
// For when you need to delete arbitrary elements
set<pii> pq;  // {priority, id}
pq.insert({priority, id});
auto [pr, id] = *pq.begin();  // get min
pq.erase(pq.begin());  // remove min
```

#### Map with Default Value
```cpp
// Automatically initializes to 0
map<int, int> freq;
freq[key]++;  // works even if key doesn't exist

// Custom default
map<int, int> m;
int val = m.count(key) ? m[key] : default_val;
```

#### Sorting with Custom Comparator
```cpp
// Sort pairs by second element
sort(all(v), [](pii a, pii b) {
    return a.second < b.second;
});

// Sort in descending order
sort(all(v), greater<int>());

// Sort by multiple criteria
sort(all(v), [](pii a, pii b) {
    if(a.first != b.first) return a.first < b.first;
    return a.second > b.second;
});
```

#### Binary Search Variants
```cpp
// Find first element >= x
auto it = lower_bound(all(v), x);

// Find first element > x
auto it = upper_bound(all(v), x);

// Count elements in range [l, r]
int cnt = upper_bound(all(v), r) - lower_bound(all(v), l);

// Custom comparator
auto it = lower_bound(all(v), x, [](int a, int b) {
    return a < b;
});
```

#### Two Pointers Template
```cpp
// Finding pairs with sum = target
int l = 0, r = n - 1;
while(l < r) {
    int sum = arr[l] + arr[r];
    if(sum == target) {
        // found
        l++; r--;
    } else if(sum < target) {
        l++;
    } else {
        r--;
    }
}

// Sliding window
int l = 0, r = 0, sum = 0;
while(r < n) {
    sum += arr[r];
    while(sum > target) {
        sum -= arr[l];
        l++;
    }
    // process window [l, r]
    r++;
}
```

### Memory Limits

```cpp
// Rough memory calculations
// 1 MB = 10^6 bytes ≈ 250,000 integers (4 bytes each)
// 256 MB ≈ 64 million integers
// 512 MB ≈ 128 million integers

// For 2D array:
// int arr[1000][1000] ≈ 4 MB
// int arr[10000][10000] ≈ 400 MB (might TLE/MLE)

// Use vectors for dynamic sizing
// Prefer 1D representation for large 2D arrays:
// int arr[N*M] and access as arr[i*M + j]
```

### Useful Constants
```cpp
const int N = 1e5 + 5;          // Common size limit
const ll MOD = 1e9 + 7;         // 1000000007
const ll MOD2 = 998244353;      // Alternative MOD
const ll INF = 1e18;            // Large value for ll
const int INF_INT = 1e9;        // Large value for int
const double EPS = 1e-9;        // For floating point comparison
const double PI = acos(-1.0);   // Pi value
```

---

## VECTOR

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

## MAP

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

---

## UNORDERED_MAP

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

### MAP vs UNORDERED_MAP Comparison
| Feature | map | unordered_map |
|---------|-----|---------------|
| **Time Complexity** | O(log n) | O(1) avg, O(n) worst |
| **Internal Structure** | Red-Black Tree | Hash Table |
| **Order** | Sorted by key | No guaranteed order |
| **Range queries** | ✓ (lower_bound, upper_bound) | ✗ |
| **Reverse iteration** | ✓ (rbegin, rend) | ✗ |
| **Predictable perf** | ✓ | ✗ (collision worst case) |

**When to use MAP**: Need sorted order, range queries, guaranteed O(log n), custom comparators
**When to use UNORDERED_MAP**: Need fastest average lookup, order doesn't matter, very large datasets

---

## SET

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

---

## UNORDERED_SET

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
```

### Time Complexity Summary
- Average: O(1) for Insert/Delete/Search
- Worst case: O(n) when hash collisions occur

### SET vs UNORDERED_SET Comparison
| Feature | set | unordered_set |
|---------|-----|---------------|
| **Time Complexity** | O(log n) | O(1) avg, O(n) worst |
| **Internal Structure** | Red-Black Tree | Hash Table |
| **Order** | Sorted order | No guaranteed order |
| **Range queries** | ✓ (lower_bound, upper_bound) | ✗ |
| **Reverse iteration** | ✓ (rbegin, rend) | ✗ |
| **Predictable perf** | ✓ Guaranteed O(log n) | ✗ Worst case O(n) |

**When to use SET**: Need sorted/ordered elements, range queries, guaranteed O(log n), custom comparators
**When to use UNORDERED_SET**: Need fastest average lookup, order doesn't matter, very large datasets, no range queries needed

---

## MULTIMAP

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

---

## MULTISET

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
ms.empty()              // returns true if empty

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

---

## LIST

### Declaration & Initialization
```cpp
#include <list>
list<int> l;                            // empty list
list<int> l(5);                         // 5 default values
list<int> l(5, 10);                     // 5 elements, all 10
list<int> l = {1, 2, 3, 4};            // initializer list
```

### Core Operations
```cpp
// Access (NO random access!)
l.front()               // first element
l.back()                // last element

// Modification
l.push_back(x)          // O(1)
l.push_front(x)         // O(1)
l.pop_back()            // O(1)
l.pop_front()           // O(1)
l.insert(it, x)         // O(1) - insert before iterator
l.erase(it)             // O(1) - erase at iterator
l.erase(it1, it2)       // O(k) - erase range
l.clear()               // O(n)
l.emplace_back(x)       // O(1)
l.emplace_front(x)      // O(1)
l.emplace(it, x)        // O(1)
l.assign(n, val)        // replace with n copies

// Size
l.size()                // number of elements
l.empty()               // returns true if empty
l.max_size()            // maximum possible size
l.resize(n)             // change size
l.resize(n, val)        // resize with value

// List-specific operations
l.remove(val)           // O(n) - remove all == val
l.remove_if(pred)       // O(n) - remove if predicate true
l.unique()              // O(n) - remove consecutive duplicates
l.sort()                // O(n log n) - sort list
l.sort(comp)            // O(n log n) - custom comparator
l.reverse()             // O(n) - reverse order
l.merge(l2)             // O(n) - merge sorted lists
l.splice(it, l2)        // O(1) - move all from l2
l.splice(it, l2, it2)   // O(1) - move single element
l.splice(it, l2, it2, it3) // O(k) - move range

// Iteration
for(int x : l)
l.begin(), l.end()      // bidirectional iterators
l.rbegin(), l.rend()

// Other
l.swap(l2)              // O(1)
```

### Time Complexity Summary
- Insert/Delete at ends: O(1)
- Insert/Delete at position: O(1) with iterator
- Random access: Not supported
- Search: O(n)
- Sort: O(n log n)

---

## QUEUE

### Declaration & Initialization
```cpp
#include <queue>
queue<int> q;                           // empty queue
```

### Core Operations
```cpp
// Access
q.front()               // first element (to be popped)
q.back()                // last element (most recently pushed)

// Modification
q.push(x)               // O(1) - add to back
q.pop()                 // O(1) - remove from front
q.emplace(x)            // O(1) - construct in place

// Size
q.size()                // number of elements
q.empty()               // returns true if empty

// Other
q.swap(q2)              // O(1) - swap contents

// NOTE: NO iteration support!
```

### Time Complexity Summary
- Push/Pop: O(1)
- Access front/back: O(1)

---

## DEQUE

### Declaration & Initialization
```cpp
#include <deque>
deque<int> dq;                          // empty deque
deque<int> dq(5);                       // size 5
deque<int> dq(5, 10);                   // size 5, all 10
deque<int> dq = {1, 2, 3, 4};          // initializer list
```

### Core Operations
```cpp
// Access
dq[i]                   // O(1) - no bounds check
dq.at(i)                // O(1) - with bounds check
dq.front()              // first element
dq.back()               // last element

// Modification (both ends!)
dq.push_back(x)         // O(1)
dq.push_front(x)        // O(1)
dq.pop_back()           // O(1)
dq.pop_front()          // O(1)
dq.insert(it, x)        // O(n) - insert before iterator
dq.erase(it)            // O(n) - erase at iterator
dq.erase(it1, it2)      // O(n) - erase range
dq.clear()              // O(n)
dq.emplace_back(x)      // O(1)
dq.emplace_front(x)     // O(1)
dq.emplace(it, x)       // O(n)

// Size
dq.size()               // number of elements
dq.empty()              // returns true if empty
dq.max_size()           // maximum possible size
dq.resize(n)            // change size
dq.resize(n, val)       // resize with value
dq.shrink_to_fit()      // reduce memory usage

// Iteration
for(int x : dq)
dq.begin(), dq.end()    // random access iterators
dq.rbegin(), dq.rend()
dq.cbegin(), dq.cend()
dq.crbegin(), dq.crend()

// Other
dq.swap(dq2)            // O(1)
dq.assign(n, val)       // replace with n copies
```

### Time Complexity Summary
- Access: O(1)
- Insert/Delete at ends: O(1)
- Insert/Delete at position: O(n)

---

## PRIORITY QUEUE

### Declaration & Initialization
```cpp
#include <queue>
priority_queue<int> pq;                     // max heap (default)
priority_queue<int, vector<int>, greater<int>> pq; // min heap
```

### Core Operations
```cpp
// Access
pq.top()                // O(1) - highest priority element

// Modification
pq.push(x)              // O(log n) - insert element
pq.pop()                // O(log n) - remove top element
pq.emplace(x)           // O(log n) - construct in place

// Size
pq.size()               // number of elements
pq.empty()              // returns true if empty

// Other
pq.swap(pq2)            // O(1) - swap contents

// NOTE: NO iteration support!
```

### Custom Comparator
```cpp
// Using lambda (C++11)
auto cmp = [](int a, int b) { return a > b; }; // min heap
priority_queue<int, vector<int>, decltype(cmp)> pq(cmp);

// Using struct
struct Compare {
    bool operator()(int a, int b) { return a > b; }
};
priority_queue<int, vector<int>, Compare> pq;
```

### Time Complexity Summary
- Insert: O(log n)
- Delete top: O(log n)
- Access top: O(1)

---

## STACK

### Declaration & Initialization
```cpp
#include <stack>
stack<int> st;                          // empty stack
```

### Core Operations
```cpp
// Access
st.top()                // O(1) - top element

// Modification
st.push(x)              // O(1) - add to top
st.pop()                // O(1) - remove from top
st.emplace(x)           // O(1) - construct in place

// Size
st.size()               // number of elements
st.empty()              // returns true if empty

// Other
st.swap(st2)            // O(1) - swap contents

// NOTE: NO iteration support!
```

### Time Complexity Summary
- Push/Pop: O(1)
- Access top: O(1)

---

## PAIR

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

---

## ITERATORS

### Types of Iterators
1. **Input Iterator**: Read-only, forward only
2. **Output Iterator**: Write-only, forward only
3. **Forward Iterator**: Read/write, forward only
4. **Bidirectional Iterator**: Read/write, forward and backward
5. **Random Access Iterator**: Read/write, any position

### Iterator Operations
```cpp
// Declaration
vector<int>::iterator it;
auto it = v.begin();            // type inference

// Dereferencing
*it                             // access value
it->member                      // access member (for objects)

// Movement
++it, it++                      // forward
--it, it--                      // backward (bidirectional+)
it + n, it - n                  // jump (random access only)
it += n, it -= n                // compound (random access only)

// Comparison
it1 == it2, it1 != it2
it1 < it2, it1 > it2            // random access only
it1 <= it2, it1 >= it2

// Distance
it2 - it1                       // number of elements between
```

### Container Iterator Functions
```cpp
// Forward
c.begin()                       // first element
c.end()                         // one past last
c.cbegin()                      // const version
c.cend()

// Reverse
c.rbegin()                      // last element
c.rend()                        // one before first
c.crbegin()                     // const version
c.crend()
```

### Iterator Utility Functions
```cpp
#include <iterator>

// Advance iterator
advance(it, n);                 // move it by n positions (modifies it)

// Distance
int d = distance(it1, it2);     // elements between iterators

// Next/Prev (don't modify original)
auto it2 = next(it, n);         // iterator n positions ahead
auto it2 = prev(it, n);         // iterator n positions back

// Insert iterators
back_inserter(c)                // insert at end
front_inserter(c)               // insert at front
inserter(c, pos)                // insert at position
```

---

## STL ALGORITHMS

### Include
```cpp
#include <algorithm>
#include <numeric>  // for accumulate, iota, etc.
```

### Non-modifying Sequence Operations

#### Searching
```cpp
// Find
auto it = find(first, last, val);           // O(n) - first occurrence
auto it = find_if(first, last, pred);       // O(n) - first match
auto it = find_if_not(first, last, pred);   // O(n)

// Count
int cnt = count(first, last, val);          // O(n)
int cnt = count_if(first, last, pred);      // O(n)

// Binary search (requires sorted range)
bool found = binary_search(first, last, val);           // O(log n)
auto it = lower_bound(first, last, val);    // O(log n) - first >= val
auto it = upper_bound(first, last, val);    // O(log n) - first > val
auto [it1, it2] = equal_range(first, last, val); // O(log n)

// Search subsequence
auto it = search(first1, last1, first2, last2);         // O(n*m)
auto it = find_end(first1, last1, first2, last2);       // last occurrence

// Adjacent
auto it = adjacent_find(first, last);       // O(n) - consecutive equal
auto it = adjacent_find(first, last, pred); // O(n) - custom comparison
```

#### Comparing
```cpp
bool eq = equal(first1, last1, first2);                 // O(n)
bool eq = equal(first1, last1, first2, pred);

auto [it1, it2] = mismatch(first1, last1, first2);     // O(n)

bool lex = lexicographical_compare(first1, last1, first2, last2);
```

#### Checking Properties
```cpp
bool all = all_of(first, last, pred);       // O(n) - all match
bool any = any_of(first, last, pred);       // O(n) - at least one
bool none = none_of(first, last, pred);     // O(n) - none match
```

### Modifying Sequence Operations

#### Copy
```cpp
copy(first, last, dest);                    // O(n)
copy_if(first, last, dest, pred);           // O(n) - conditional
copy_n(first, n, dest);                     // O(n)
copy_backward(first, last, dest_end);       // O(n) - from end

move(first, last, dest);                    // O(n) - move semantics
move_backward(first, last, dest_end);       // O(n)
```

#### Transform
```cpp
transform(first, last, dest, unary_op);     // O(n) - apply function
transform(first1, last1, first2, dest, binary_op); // O(n) - two ranges
```

#### Fill & Generate
```cpp
fill(first, last, val);                     // O(n) - assign value
fill_n(first, n, val);                      // O(n)
generate(first, last, gen);                 // O(n) - call generator
generate_n(first, n, gen);                  // O(n)
iota(first, last, val);                     // O(n) - sequential values
```

#### Replace
```cpp
replace(first, last, old_val, new_val);     // O(n)
replace_if(first, last, pred, new_val);     // O(n)
replace_copy(first, last, dest, old_val, new_val);
replace_copy_if(first, last, dest, pred, new_val);
```

#### Remove
```cpp
auto new_end = remove(first, last, val);    // O(n) - doesn't erase!
auto new_end = remove_if(first, last, pred);
v.erase(new_end, v.end());                  // actually remove

remove_copy(first, last, dest, val);        // copy without value
remove_copy_if(first, last, dest, pred);

auto new_end = unique(first, last);         // O(n) - consecutive dups
v.erase(new_end, v.end());
unique_copy(first, last, dest);
```

#### Reverse & Rotate
```cpp
reverse(first, last);                       // O(n)
reverse_copy(first, last, dest);

rotate(first, middle, last);                // O(n) - middle becomes first
rotate_copy(first, middle, last, dest);
```

#### Shuffle & Sample
```cpp
#include <random>
random_shuffle(first, last);                // deprecated C++14
shuffle(first, last, rng);                  // O(n) - use random engine

sample(first, last, dest, n, rng);          // O(n) - random n elements
```

#### Swap
```cpp
swap(a, b);                                 // swap two values
iter_swap(it1, it2);                        // swap via iterators
swap_ranges(first1, last1, first2);         // O(n)
```

### Sorting & Related

#### Sort
```cpp
sort(first, last);                          // O(n log n) - ascending
sort(first, last, comp);                    // custom comparator
sort(first, last, greater<int>());          // descending

stable_sort(first, last);                   // O(n log n) - preserves order
stable_sort(first, last, comp);

partial_sort(first, middle, last);          // O(n log k) - k smallest sorted
partial_sort_copy(first, last, dest_first, dest_last);

nth_element(first, nth, last);              // O(n) - nth element in position
is_sorted(first, last);                     // O(n)
is_sorted_until(first, last);               // O(n)
```

#### Partition
```cpp
auto mid = partition(first, last, pred);    // O(n) - reorder by predicate
auto mid = stable_partition(first, last, pred); // O(n log n)

bool p = is_partitioned(first, last, pred); // O(n)
auto it = partition_point(first, last, pred); // O(log n) on partitioned
```

### Set Operations (on sorted ranges)
```cpp
// Union
auto end = set_union(first1, last1, first2, last2, dest);

// Intersection
auto end = set_intersection(first1, last1, first2, last2, dest);

// Difference
auto end = set_difference(first1, last1, first2, last2, dest);

// Symmetric difference
auto end = set_symmetric_difference(first1, last1, first2, last2, dest);

// Check
bool subset = includes(first1, last1, first2, last2);
```

### Heap Operations
```cpp
make_heap(first, last);                     // O(n)
make_heap(first, last, comp);

push_heap(first, last);                     // O(log n) - add to heap
pop_heap(first, last);                      // O(log n) - move max to end
sort_heap(first, last);                     // O(n log n) - sort heap

bool is_heap = is_heap(first, last);        // O(n)
auto it = is_heap_until(first, last);       // O(n)
```

### Min/Max
```cpp
int mn = min(a, b);
int mx = max(a, b);
auto [mn, mx] = minmax(a, b);

int mn = min({a, b, c, d});                 // initializer list
int mx = max({a, b, c, d});

auto it = min_element(first, last);         // O(n)
auto it = max_element(first, last);         // O(n)
auto [it_min, it_max] = minmax_element(first, last); // O(n)

clamp(val, lo, hi);                         // C++17 - constrain to range
```

### Permutations
```cpp
bool changed = next_permutation(first, last);    // O(n) - lexicographic
bool changed = prev_permutation(first, last);    // O(n)

is_permutation(first1, last1, first2);           // O(n²)
```

### Numeric Operations
```cpp
#include <numeric>

// Accumulate
int sum = accumulate(first, last, init);         // O(n)
int prod = accumulate(first, last, 1, multiplies<int>());

// Reduce (parallel capable)
int sum = reduce(first, last, init);             // C++17

// Inner product
int dot = inner_product(first1, last1, first2, init);

// Partial sum
partial_sum(first, last, dest);                  // cumulative sum
partial_sum(first, last, dest, op);

// Adjacent difference
adjacent_difference(first, last, dest);
adjacent_difference(first, last, dest, op);

// GCD & LCM
int g = gcd(a, b);                              // C++17
int l = lcm(a, b);                              // C++17

// Fill with sequence
iota(first, last, start_val);                   // 0, 1, 2, 3...
```

### Lambda Examples
```cpp
// Sorting
sort(v.begin(), v.end(), [](int a, int b) { return a > b; });

// Transform
transform(v.begin(), v.end(), v.begin(), [](int x) { return x * 2; });

// Find if
auto it = find_if(v.begin(), v.end(), [](int x) { return x > 10; });

// Count if
int cnt = count_if(v.begin(), v.end(), [](int x) { return x % 2 == 0; });

// Remove if
auto new_end = remove_if(v.begin(), v.end(), [](int x) { return x < 0; });
v.erase(new_end, v.end());

// For each
for_each(v.begin(), v.end(), [](int x) { cout << x << " "; });
```

---

## EXCEPTION HANDLING

### Basic Syntax
```cpp
try {
    // code that may throw
    if (error_condition) {
        throw exception_object;
    }
} catch (ExceptionType1& e) {
    // handle exception type 1
} catch (ExceptionType2& e) {
    // handle exception type 2
} catch (...) {
    // catch all other exceptions
}
```

### Standard Exceptions
```cpp
#include <exception>
#include <stdexcept>

// Base class
std::exception              // base for all standard exceptions

// Logic errors (programming bugs)
std::logic_error
  └─ std::invalid_argument
  └─ std::domain_error
  └─ std::length_error
  └─ std::out_of_range

// Runtime errors
std::runtime_error
  └─ std::range_error
  └─ std::overflow_error
  └─ std::underflow_error

// Other
std::bad_alloc              // memory allocation failed
std::bad_cast               // dynamic_cast failed
std::bad_typeid             // typeid with null pointer
std::bad_exception          // unexpected exception
```

### Exception Usage
```cpp
// Throw
throw std::invalid_argument("message");
throw std::runtime_error("error occurred");
throw 42;                   // can throw any type

// Catch and access message
try {
    throw std::runtime_error("error");
} catch (const std::exception& e) {
    cout << e.what() << endl;   // get error message
}

// Rethrow
try {
    // code
} catch (const std::exception& e) {
    // partial handling
    throw;                  // rethrow same exception
}

// Nested try-catch
try {
    try {
        throw std::runtime_error("inner");
    } catch (const std::exception& e) {
        cout << "Inner: " << e.what() << endl;
        throw;              // rethrow to outer
    }
} catch (const std::exception& e) {
    cout << "Outer: " << e.what() << endl;
}
```

### Custom Exception Class
```cpp
class MyException : public std::exception {
private:
    std::string msg;
public:
    MyException(const std::string& message) : msg(message) {}
    const char* what() const noexcept override {
        return msg.c_str();
    }
};

// Usage
try {
    throw MyException("custom error");
} catch (const MyException& e) {
    cout << e.what() << endl;
}
```

---

## STRING

### Headers
```cpp
#include <string>
#include <algorithm> // for sort, reverse
using namespace std;
```

### Declaration & Initialization
```cpp
string s;                          // empty string
string s = "hello";                // from C-style string literal
string s("hello");                 // from C-style string literal
string s2 = s;                     // copy constructor
string s(5, 'a');                  // "aaaaa" - fill constructor
string s(s1, 3);                   // copy s1 from index 3 to end
string s(s1, 1, 3);                // copy "ell" (from s1="hello", pos 1, len 3)
```

### Core Operations
```cpp
// Access
s[i]                    // O(1) - no bounds check
s.at(i)                 // O(1) - with bounds check
s.front()               // O(1) - first char
s.back()                // O(1) - last char
s.c_str()               // O(1) (C++11+) - pointer to C-style string

// Modification
s.push_back('c')          // O(1) amortized - append char
s.pop_back()            // O(1) - remove last char
s += "world"            // O(m) - append string (m=len of "world")
s += '!'                // O(1) amortized - append char
s1 + s2                 // O(n+m) - returns new concatenated string
s.insert(pos, str)      // O(n) - insert str at index pos
s.erase(pos, len)       // O(n) - erase len chars starting at pos
s.erase(it)             // O(n) - erase char at iterator
s.clear()               // O(1) or O(n) - clears string
s.replace(pos, len, str) // O(n) - replace len chars at pos with str
```

### Size & Capacity
```cpp
s.size()                // O(1) - number of chars
s.length()              // O(1) - same as size()
s.empty()               // O(1) - returns true if empty
s.resize(n)             // O(n) - change size to n
s.resize(n, 'c')        // O(n) - resize and fill with 'c'
s.capacity()            // O(1) - allocated capacity
s.reserve(n)            // O(n) - reserve capacity
s.shrink_to_fit()       // O(n) - reduce capacity
```

### Iteration
```cpp
for(char c : s)         // range-based loop
for(auto& c : s)        // range-based (by ref, for modification)
for(auto it = s.begin(); it != s.end(); ++it)
s.begin(), s.end()      // forward iterators
s.rbegin(), s.rend()    // reverse iterators

// Useful with <algorithm>
reverse(s.begin(), s.end()); // O(n)
sort(s.begin(), s.end());    // O(n log n)
```

### String-Specific Operations
```cpp
// Substring
s.substr(pos)           // O(len) - substring from pos to end
s.substr(pos, len)      // O(len) - substring of length len

// Find
string::npos            // Value returned by find() on failure
s.find(str)             // O(n*m) - find first occurrence of str
s.find(str, pos)        // O(n*m) - find from pos
s.rfind(str)            // O(n*m) - find last occurrence
s.find_first_of(str)    // O(n*m) - find first char present in str
s.find_last_of(str)     // O(n*m) - find last char present in str
s.find_first_not_of(str)// O(n*m) - find first char NOT in str
s.find_last_not_of(str) // O(n*m) - find last char NOT in str

// Compare
s.compare(str)          // O(n) - returns <0, 0, or >0 (like strcmp)
s1 == s2, s1 != s2      // O(n) - comparison
s1 < s2, s1 > s2        // O(n) - lexicographical comparison
```

### I/O & Conversions (Critical for CP)
```cpp
// Input
cin >> s;               // reads until whitespace
getline(cin, s);        // reads entire line until '\n'
getline(cin, s, delim); // reads until delimiter 'delim'

// Conversions (requires <string> header)
to_string(123)          // "123" (int to string)
to_string(3.14)         // "3.140000" (double to string)

stoi(s)                 // "123" -> 123 (string to int)
stoll(s)                // string to long long
stod(s)                 // string to double
```

### Other
```cpp
s.swap(s2)              // O(1) (C++11+) - swap contents
```

### Time Complexity Summary
- Access ([], front, back): O(1)
- Append/Remove char at end (push_back, pop_back): O(1) amortized
- Insert/Delete at position (insert, erase): O(n)
- Concatenation (+): O(n + m)
- Find (find, rfind, etc.): O(n * m)
- Substring (substr): O(len) (where len is length of substring)
- Lexicographical Compare (==, <, compare): O(n)

---

## QUICK TIPS FOR ICPC

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

