# CP BOILERPLATE & FAST I/O

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