# EXCEPTION HANDLING

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