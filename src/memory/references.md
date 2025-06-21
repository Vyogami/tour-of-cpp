# References

References provide an alternative way to indirectly access objects in C++. They act as aliases for existing variables, offering many of the benefits of pointers while being safer and more convenient to use. References are particularly useful for function parameters and return values.

## What is a Reference?

A reference is an alias - another name for an existing variable:

```cpp
#include <iostream>

int main() {
    int original = 42;
    int& alias = original;  // 'alias' is now another name for 'original'
    
    std::cout << "Original value: " << original << std::endl;    // 42
    std::cout << "Alias value: " << alias << std::endl;         // 42
    
    // Modifying through alias affects original
    alias = 100;
    std::cout << "After alias = 100:" << std::endl;
    std::cout << "Original value: " << original << std::endl;    // 100
    std::cout << "Alias value: " << alias << std::endl;         // 100
    
    // They have the same address
    std::cout << "Address of original: " << &original << std::endl;
    std::cout << "Address of alias: " << &alias << std::endl;
    
    return 0;
}
```

## References vs Pointers

### Key Differences

| Feature | References | Pointers |
|---------|------------|----------|
| **Initialization** | Must be initialized | Can be uninitialized |
| **Reassignment** | Cannot be reassigned | Can point to different objects |
| **Null values** | Cannot be null | Can be null |
| **Arithmetic** | No arithmetic operations | Supports arithmetic |
| **Syntax** | Direct access (no dereferencing) | Requires dereferencing (*) |
| **Memory overhead** | No extra memory | Stores an address |

### Comparison Example

```cpp
#include <iostream>

void demonstrateDifferences() {
    int value1 = 10;
    int value2 = 20;
    
    // References
    int& ref = value1;        // Must initialize
    // int& ref2;             // ✘ Error: references must be initialized
    std::cout << "Reference: " << ref << std::endl;  // Direct access
    
    // ref = value2;          // This assigns value2's content to value1!
    // ref = &value2;         // ✘ Error: cannot assign address to reference
    
    // Pointers
    int* ptr;                 // Can be uninitialized (but dangerous)
    ptr = &value1;            // Assign address
    std::cout << "Pointer: " << *ptr << std::endl;   // Must dereference
    
    ptr = &value2;            // Can reassign to different variable
    std::cout << "Pointer after reassignment: " << *ptr << std::endl;
    
    ptr = nullptr;            // Can be null
    if (ptr != nullptr) {
        std::cout << "Pointer value: " << *ptr << std::endl;
    } else {
        std::cout << "Pointer is null" << std::endl;
    }
}

int main() {
    demonstrateDifferences();
    return 0;
}
```

## Reference Initialization

### Basic Reference Types

```cpp
#include <iostream>

int main() {
    // Different types of references
    int integer = 42;
    double decimal = 3.14;
    char character = 'A';
    
    int& intRef = integer;
    double& doubleRef = decimal;
    char& charRef = character;
    
    std::cout << "Integer reference: " << intRef << std::endl;
    std::cout << "Double reference: " << doubleRef << std::endl;
    std::cout << "Char reference: " << charRef << std::endl;
    
    // Modify through references
    intRef = 100;
    doubleRef = 2.71;
    charRef = 'Z';
    
    std::cout << "After modification:" << std::endl;
    std::cout << "Original integer: " << integer << std::endl;
    std::cout << "Original double: " << decimal << std::endl;
    std::cout << "Original char: " << character << std::endl;
    
    return 0;
}
```

### Reference Initialization Rules

```cpp
#include <iostream>

int main() {
    int value = 50;
    
    // → Valid reference initializations
    int& ref1 = value;              // Reference to variable
    const int& ref2 = value;        // Const reference to variable
    const int& ref3 = 42;           // Const reference to literal
    
    // ✘ Invalid reference initializations
    // int& ref4;                   // Error: must initialize
    // int& ref5 = 42;              // Error: cannot bind non-const ref to literal
    // int& ref6 = nullptr;         // Error: references cannot be null
    
    std::cout << "ref1: " << ref1 << std::endl;
    std::cout << "ref2: " << ref2 << std::endl;
    std::cout << "ref3: " << ref3 << std::endl;
    
    return 0;
}
```

## Function Parameters with References

References are most commonly used as function parameters:

### Pass by Reference for Modification

```cpp
#include <iostream>

void swap(int& a, int& b) {
    int temp = a;
    a = b;
    b = temp;
}

void increment(int& value) {
    value++;
}

void reset(int& value) {
    value = 0;
}

int main() {
    int x = 10, y = 20;
    
    std::cout << "Before swap: x=" << x << ", y=" << y << std::endl;
    swap(x, y);
    std::cout << "After swap: x=" << x << ", y=" << y << std::endl;
    
    std::cout << "Before increment: x=" << x << std::endl;
    increment(x);
    std::cout << "After increment: x=" << x << std::endl;
    
    reset(x);
    std::cout << "After reset: x=" << x << std::endl;
    
    return 0;
}
```

### Pass by Const Reference for Efficiency

```cpp
#include <iostream>
#include <string>
#include <vector>

// Efficient: no copy, cannot modify
void printString(const std::string& text) {
    std::cout << "String: " << text << std::endl;
    // text += "!";  // ✘ Error: cannot modify const reference
}

// Efficient: no copy of large vector
double calculateAverage(const std::vector<int>& numbers) {
    if (numbers.empty()) return 0.0;
    
    int sum = 0;
    for (int num : numbers) {
        sum += num;
    }
    return static_cast<double>(sum) / numbers.size();
}

// Compare different parameter passing methods
void demonstrateEfficiency() {
    std::string largeString(10000, 'x');  // Large string
    
    // Method 1: Pass by value (copies entire string)
    auto passByValue = [](std::string text) {
        return text.length();
    };
    
    // Method 2: Pass by const reference (no copy)
    auto passByConstRef = [](const std::string& text) {
        return text.length();
    };
    
    std::cout << "Length (by value): " << passByValue(largeString) << std::endl;
    std::cout << "Length (by const ref): " << passByConstRef(largeString) << std::endl;
    // Both produce same result, but const ref is much faster for large objects
}

int main() {
    std::string message = "Hello, World!";
    printString(message);
    
    std::vector<int> numbers = {10, 20, 30, 40, 50};
    std::cout << "Average: " << calculateAverage(numbers) << std::endl;
    
    demonstrateEfficiency();
    
    return 0;
}
```

## Function Return References

Functions can return references to allow modification of returned objects:

### Returning References for Chaining

```cpp
#include <iostream>

class Counter {
private:
    int value;
    
public:
    Counter(int initial = 0) : value(initial) {}
    
    // Return reference to allow chaining
    Counter& increment() {
        value++;
        return *this;
    }
    
    Counter& decrement() {
        value--;
        return *this;
    }
    
    Counter& add(int amount) {
        value += amount;
        return *this;
    }
    
    int getValue() const {
        return value;
    }
};

int main() {
    Counter counter(10);
    
    // Method chaining possible because we return references
    counter.increment()
           .increment()
           .add(5)
           .decrement();
    
    std::cout << "Final value: " << counter.getValue() << std::endl;  // 16
    
    return 0;
}
```

### Array Access with References

```cpp
#include <iostream>
#include <stdexcept>

class SafeArray {
private:
    int* data;
    size_t size;
    
public:
    SafeArray(size_t s) : size(s) {
        data = new int[size]();
    }
    
    ~SafeArray() {
        delete[] data;
    }
    
    // Return reference to allow modification
    int& at(size_t index) {
        if (index >= size) {
            throw std::out_of_range("Index out of bounds");
        }
        return data[index];
    }
    
    // Const version for read-only access
    const int& at(size_t index) const {
        if (index >= size) {
            throw std::out_of_range("Index out of bounds");
        }
        return data[index];
    }
    
    size_t getSize() const { return size; }
    
    // Operator overloading with references
    int& operator[](size_t index) {
        return at(index);
    }
    
    const int& operator[](size_t index) const {
        return at(index);
    }
};

int main() {
    SafeArray array(5);
    
    // Use references for modification
    for (size_t i = 0; i < array.getSize(); i++) {
        array[i] = i * i;  // Calls operator[] which returns reference
    }
    
    // Read values
    for (size_t i = 0; i < array.getSize(); i++) {
        std::cout << "array[" << i << "] = " << array[i] << std::endl;
    }
    
    // Direct modification through reference
    int& firstElement = array[0];
    firstElement = 999;
    std::cout << "Modified first element: " << array[0] << std::endl;
    
    return 0;
}
```

### Dangerous Reference Returns

```cpp
#include <iostream>

// ✘ Dangerous: returning reference to local variable
int& badFunction() {
    int localVar = 42;
    return localVar;  // Undefined behavior! localVar is destroyed
}

// ✘ Dangerous: returning reference to temporary
int& anotherBadFunction() {
    return int(42);  // Temporary object is destroyed immediately
}

// → Safe: returning reference to parameter
int& safeFunction(int& parameter) {
    return parameter;  // OK: parameter exists in caller's scope
}

// → Safe: returning reference to member
class SafeClass {
private:
    int memberVar;
    
public:
    SafeClass(int value) : memberVar(value) {}
    
    int& getValue() {
        return memberVar;  // OK: member exists as long as object exists
    }
};

int main() {
    // Don't do this!
    // int& dangerous = badFunction();  // Undefined behavior
    
    // Safe usage
    int value = 100;
    int& safe = safeFunction(value);
    std::cout << "Safe reference: " << safe << std::endl;
    
    SafeClass obj(200);
    int& member = obj.getValue();
    std::cout << "Member reference: " << member << std::endl;
    
    return 0;
}
```

## Const References

Const references are extremely useful for read-only access:

### Const Reference Benefits

```cpp
#include <iostream>
#include <string>

class Book {
private:
    std::string title;
    std::string author;
    int pages;
    
public:
    Book(const std::string& t, const std::string& a, int p) 
        : title(t), author(a), pages(p) {}
    
    // Const reference getters - no copy, read-only
    const std::string& getTitle() const { return title; }
    const std::string& getAuthor() const { return author; }
    int getPages() const { return pages; }
    
    // Non-const reference setters - allow modification
    std::string& getTitle() { return title; }
    std::string& getAuthor() { return author; }
};

// Function taking const reference - efficient and safe
void printBookInfo(const Book& book) {
    std::cout << "Title: " << book.getTitle() << std::endl;
    std::cout << "Author: " << book.getAuthor() << std::endl;
    std::cout << "Pages: " << book.getPages() << std::endl;
    
    // book.getTitle() = "New Title";  // ✘ Error: const object
}

// Function taking non-const reference - can modify
void updateBook(Book& book) {
    book.getTitle() += " (Updated)";
    book.getAuthor() += " Jr.";
}

int main() {
    Book myBook("1984", "George Orwell", 328);
    
    std::cout << "Original book:" << std::endl;
    printBookInfo(myBook);
    
    updateBook(myBook);
    
    std::cout << "\nUpdated book:" << std::endl;
    printBookInfo(myBook);
    
    return 0;
}
```

### Const References with Temporaries

```cpp
#include <iostream>
#include <string>

std::string createString() {
    return "Temporary String";
}

int calculate() {
    return 42 * 2;
}

int main() {
    // Const references can bind to temporaries
    const std::string& tempRef = createString();
    const int& calcRef = calculate();
    const int& literalRef = 100;
    
    std::cout << "Temporary string: " << tempRef << std::endl;
    std::cout << "Calculated value: " << calcRef << std::endl;
    std::cout << "Literal value: " << literalRef << std::endl;
    
    // The temporaries live as long as the references exist
    // This is safe and efficient
    
    return 0;
}
```

## Reference Wrappers

For situations where you need reference-like behavior in containers:

```cpp
#include <iostream>
#include <vector>
#include <functional>

int main() {
    int a = 10, b = 20, c = 30;
    
    // ✘ Cannot store references directly in containers
    // std::vector<int&> refVector;  // Error!
    
    // → Use std::reference_wrapper
    std::vector<std::reference_wrapper<int>> refVector;
    refVector.push_back(std::ref(a));
    refVector.push_back(std::ref(b));
    refVector.push_back(std::ref(c));
    
    std::cout << "Original values: a=" << a << ", b=" << b << ", c=" << c << std::endl;
    
    // Modify through reference wrappers
    for (auto& ref : refVector) {
        ref.get() += 10;  // Add 10 to each referenced variable
    }
    
    std::cout << "After modification: a=" << a << ", b=" << b << ", c=" << c << std::endl;
    
    return 0;
}
```

## Practical Examples

### Matrix Class with Reference Access

```cpp
#include <iostream>
#include <vector>

class Matrix {
private:
    std::vector<std::vector<int>> data;
    size_t rows, cols;
    
public:
    Matrix(size_t r, size_t c) : rows(r), cols(c) {
        data.resize(rows);
        for (auto& row : data) {
            row.resize(cols, 0);
        }
    }
    
    // Return reference for modification
    std::vector<int>& operator[](size_t row) {
        return data[row];
    }
    
    // Const version for read-only access
    const std::vector<int>& operator[](size_t row) const {
        return data[row];
    }
    
    void print() const {
        for (size_t i = 0; i < rows; i++) {
            for (size_t j = 0; j < cols; j++) {
                std::cout << data[i][j] << " ";
            }
            std::cout << std::endl;
        }
    }
    
    size_t getRows() const { return rows; }
    size_t getCols() const { return cols; }
};

void fillMatrix(Matrix& matrix) {
    for (size_t i = 0; i < matrix.getRows(); i++) {
        for (size_t j = 0; j < matrix.getCols(); j++) {
            matrix[i][j] = i * matrix.getCols() + j + 1;
        }
    }
}

int main() {
    Matrix mat(3, 4);
    
    fillMatrix(mat);
    std::cout << "Matrix contents:" << std::endl;
    mat.print();
    
    // Direct modification through reference
    mat[1][2] = 999;
    std::cout << "\nAfter modifying mat[1][2]:" << std::endl;
    mat.print();
    
    return 0;
}
```

### Reference-Based Configuration System

```cpp
#include <iostream>
#include <string>
#include <map>

class Config {
private:
    std::map<std::string, std::string> settings;
    
public:
    // Return reference to allow modification
    std::string& operator[](const std::string& key) {
        return settings[key];
    }
    
    // Const version for read-only access
    const std::string& at(const std::string& key) const {
        return settings.at(key);
    }
    
    bool exists(const std::string& key) const {
        return settings.find(key) != settings.end();
    }
    
    void print() const {
        for (const auto& pair : settings) {
            std::cout << pair.first << " = " << pair.second << std::endl;
        }
    }
};

int main() {
    Config config;
    
    // Set configuration values using references
    config["server_port"] = "8080";
    config["database_url"] = "localhost:5432";
    config["debug_mode"] = "true";
    
    std::cout << "Configuration:" << std::endl;
    config.print();
    
    // Modify existing value
    std::string& port = config["server_port"];
    port = "9090";
    
    std::cout << "\nAfter port change:" << std::endl;
    std::cout << "Server port: " << config["server_port"] << std::endl;
    
    return 0;
}
```

## Common Reference Mistakes

### Reference Reassignment Confusion

```cpp
#include <iostream>

int main() {
    int a = 10;
    int b = 20;
    
    int& ref = a;  // ref is an alias for a
    
    std::cout << "Initially: a=" << a << ", b=" << b << ", ref=" << ref << std::endl;
    
    ref = b;  // This does NOT make ref refer to b!
              // This assigns b's value to a (through ref)
    
    std::cout << "After ref = b: a=" << a << ", b=" << b << ", ref=" << ref << std::endl;
    
    // ref is still an alias for a
    ref = 999;
    std::cout << "After ref = 999: a=" << a << ", b=" << b << std::endl;
    
    return 0;
}
```

### Lifetime Issues

```cpp
#include <iostream>

const int& problematicFunction() {
    int local = 42;
    return local;  // ✘ Returning reference to local variable
}

int main() {
    // ✘ Dangerous: reference to destroyed object
    // const int& dangling = problematicFunction();
    // std::cout << dangling;  // Undefined behavior
    
    // → Safe alternatives:
    // 1. Return by value
    // 2. Pass parameter by reference to modify
    // 3. Return reference to member variable
    // 4. Return reference to static variable
    
    return 0;
}
```

## Best Practices

### When to Use References

```cpp
#include <iostream>
#include <string>

// → Use const reference for large objects (read-only)
void processLargeObject(const std::string& largeString) {
    // Efficient: no copy, safe: cannot modify
}

// → Use reference for modification
void modifyObject(std::string& str) {
    str += " (modified)";
}

// → Use reference for chaining
class Builder {
public:
    Builder& setName(const std::string& name) { return *this; }
    Builder& setAge(int age) { return *this; }
    // Enable: builder.setName("John").setAge(25);
};

// ✘ Don't use reference for small types that are cheap to copy
void processInt(const int& value) {  // Overkill for int
    std::cout << value << std::endl;
}

void processIntBetter(int value) {   // Better for simple types
    std::cout << value << std::endl;
}
```

## Exercises

### Exercise 1: Reference Utilities

Create functions using references:

- Swap three variables in a single function call
- Find min and max values in an array, returning both via references
- Implement a function that sorts three numbers using references

### Exercise 2: Reference-Based Data Structures

Implement:

- A simple vector class with reference-based element access
- A key-value store that returns references to values
- A circular buffer with reference-based access methods

### Exercise 3: Builder Pattern

Create a builder class that:

- Uses reference returns for method chaining
- Configures a complex object step by step
- Validates configuration at build time

### Exercise 4: Reference Performance

Compare performance of:

- Pass by value vs pass by const reference for large objects
- Reference-based vs pointer-based linked list operations
- Different ways of returning data from functions

## Key Takeaways

<i class="fa-solid fa-arrow-right"></i> **References are aliases** - they provide another name for existing objects  
<i class="fa-solid fa-arrow-right"></i> **References must be initialized** and cannot be reassigned  
<i class="fa-solid fa-arrow-right"></i> **Use const references for read-only access** to large objects  
<i class="fa-solid fa-arrow-right"></i> **References are safer than pointers** - no null references, no arithmetic  
<i class="fa-solid fa-arrow-right"></i> **Return references for chaining** and efficient access  
<i class="fa-solid fa-arrow-right"></i> **Be careful with reference lifetimes** - don't return references to locals

---

**Next**: Learn about dynamic memory allocation // →  [Dynamic Memory Allocation](dynamic-memory.md)
