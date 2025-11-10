# STRING

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
s.clear()               // O(n) - clears string
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
string::npos            // Value returned by find() on failure (until the end of the string, not found)
s.find(str)             // O(n*m) - find first occurrence of str
s.find(str, pos)        // O(n*m) - find from pos
s.rfind(str)            // O(n*m) - find last occurrence
s.find_first_of(str)    // O(n*m) - find first char present in str
s.find_last_of(str)     // O(n*m) - find last char present in str
s.find_first_not_of(str)// O(n*m) - find first char NOT in str
s.find_last_not_of(str) // O(n*m) - find last char NOT in str
if (s.find(str) != string::npos) { /* str exists as substring in s */ } // O(n*m) - check if substring exists

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