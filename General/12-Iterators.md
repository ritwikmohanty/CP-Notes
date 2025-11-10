# ITERATORS

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