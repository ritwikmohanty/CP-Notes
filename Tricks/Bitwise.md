# C++ Bitwise Tricks

1. **Check if a number `n` is a power of 2**
    ```cpp
    bool isPowerOfTwo = (n > 0) && ((n & (n - 1)) == 0);
    ```

2. **Check if the i-th bit is set**
    ```cpp
    bool isSet = (n & (1 << i)) != 0;
    ```

3. **Set the i-th bit**
    ```cpp
    n = n | (1 << i);
    ```

4. **Unset (clear) the i-th bit**
    ```cpp
    n = n & ~(1 << i);
    ```

5. **Toggle the i-th bit**
    ```cpp
    n = n ^ (1 << i);
    ```

6. **Clear the lowest set bit**
    ```cpp
    n = n & (n - 1);
    ```

7. **Get the lowest set bit**
    ```cpp
    int lowBit = n & -n;
    ```

8. **Count the number of set bits (Population Count)**
    ```cpp
    int count = __builtin_popcount(n); // for int
    int countLL = __builtin_popcountll(n); // for long long
    ```

9. **Count leading zeros**
    ```cpp
    int leadingZeros = __builtin_clz(n); // for int
    ```

10. **Count trailing zeros**
    ```cpp
    int trailingZeros = __builtin_ctz(n); // for int
    ```

11. **Swap two numbers without a temporary variable**
    ```cpp
    a ^= b;
    b ^= a;
    a ^= b;
    ```

12. **Calculate `n % (2^k)`**
    To calculate modulus with a power of 2, you can use bitwise AND.
    ```cpp
    // Equivalent to n % (1 << k)
    int rem = n & ((1 << k) - 1);
    ```

13. **Check if a number is odd or even**
    ```cpp
    if (n & 1) {
        // Odd
    } else {
        // Even
    }
    ```
