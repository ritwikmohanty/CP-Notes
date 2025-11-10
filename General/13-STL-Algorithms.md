# STL ALGORITHMS

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