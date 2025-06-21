# Pointers and Addresses

Pointers are variables that store memory addresses rather than values directly. They provide powerful capabilities for memory manipulation, dynamic allocation, and efficient data structures. Understanding pointers is crucial for mastering C++ memory management.

## What is a Pointer?

A pointer is a variable that contains the memory address of another variable:

```cpp
#include <iostream>

int main() {
    int value = 42;        // Regular variable storing a value
    int* ptr = &value;     // Pointer storing the address of 'value'
    
    std::cout << "Value: " << value << std::endl;           // 42
    std::cout << "Address of value: " << &value << std::endl;  // Memory address
    std::cout << "Pointer contains: " << ptr << std::endl;     // Same address
    std::cout << "Value through pointer: " << *ptr << std::endl;  // 42
    
    return 0;
}
```

**Output:**

```
Value: 42
Address of value: 0x7fff5fbff6ac
Pointer contains: 0x7fff5fbff6ac
Value through pointer: 42
```

## Pointer Syntax and Operations

### Declaration and Initialization

```cpp
#include <iostream>

int main() {
    int number = 100;
    
    // Pointer declaration and initialization
    int* ptr1 = &number;      // Points to 'number'
    int* ptr2 = nullptr;      // Null pointer (safe initialization)
    int* ptr3;                // Uninitialized (dangerous!)
    
    // Alternative syntax (same meaning)
    int *ptr4 = &number;      // Space after type
    int* ptr5, ptr6;          // ptr5 is pointer, ptr6 is int!
    int *ptr7, *ptr8;         // Both are pointers
    
    std::cout << "number = " << number << std::endl;
    std::cout << "&number = " << &number << std::endl;
    std::cout << "ptr1 = " << ptr1 << std::endl;
    std::cout << "*ptr1 = " << *ptr1 << std::endl;
    
    return 0;
}
```

### Key Operators

- **`&`** - Address-of operator: gets the address of a variable
- **`*`** - Dereference operator: gets the value at an address
- **`->`** - Arrow operator: accesses members through a pointer

```cpp
#include <iostream>

int main() {
    int value = 75;
    int* ptr = &value;
    
    std::cout << "Original operations:" << std::endl;
    std::cout << "value = " << value << std::endl;      // Direct access
    std::cout << "&value = " << &value << std::endl;    // Address of value
    std::cout << "ptr = " << ptr << std::endl;          // Pointer value (address)
    std::cout << "*ptr = " << *ptr << std::endl;        // Dereference pointer
    
    // Modify through pointer
    *ptr = 200;
    std::cout << "\nAfter *ptr = 200:" << std::endl;
    std::cout << "value = " << value << std::endl;      // value changed!
    std::cout << "*ptr = " << *ptr << std::endl;
    
    return 0;
}
```

## Pointer Types

Different data types have different pointer types:

```cpp
#include <iostream>

int main() {
    // Different types, different pointers
    int integer = 42;
    double decimal = 3.14;
    char character = 'A';
    bool flag = true;
    
    int* intPtr = &integer;
    double* doublePtr = &decimal;
    char* charPtr = &character;
    bool* boolPtr = &flag;
    
    std::cout << "Integer pointer: " << intPtr << " -> " << *intPtr << std::endl;
    std::cout << "Double pointer: " << doublePtr << " -> " << *doublePtr << std::endl;
    std::cout << "Char pointer: " << static_cast<void*>(charPtr) << " -> " << *charPtr << std::endl;
    std::cout << "Bool pointer: " << boolPtr << " -> " << std::boolalpha << *boolPtr << std::endl;
    
    // Pointer sizes (usually same regardless of pointed-to type)
    std::cout << "\nPointer sizes:" << std::endl;
    std::cout << "sizeof(int*): " << sizeof(int*) << std::endl;
    std::cout << "sizeof(double*): " << sizeof(double*) << std::endl;
    std::cout << "sizeof(char*): " << sizeof(char*) << std::endl;
    
    return 0;
}
```

## Null Pointers

Always initialize pointers to avoid undefined behavior:

```cpp
#include <iostream>

int main() {
    // Safe pointer initialization
    int* safePtr = nullptr;        // C++11 preferred
    int* oldStyleNull = NULL;      // C-style (still works)
    int* zeroPtr = 0;             // Also works but less clear
    
    // Check before dereferencing
    if (safePtr != nullptr) {
        std::cout << "Safe to dereference: " << *safePtr << std::endl;
    } else {
        std::cout << "Pointer is null, cannot dereference" << std::endl;
    }
    
    // Assign valid address
    int value = 100;
    safePtr = &value;
    
    if (safePtr != nullptr) {
        std::cout << "Now safe to dereference: " << *safePtr << std::endl;
    }
    
    return 0;
}
```

### Null Pointer Checks

```cpp
#include <iostream>

void safeProcess(int* ptr) {
    // Always check for null before using
    if (ptr == nullptr) {
        std::cout << "Error: Null pointer passed to safeProcess" << std::endl;
        return;
    }
    
    std::cout << "Processing value: " << *ptr << std::endl;
    *ptr *= 2;  // Double the value
    std::cout << "New value: " << *ptr << std::endl;
}

int main() {
    int number = 50;
    int* validPtr = &number;
    int* nullPtr = nullptr;
    
    safeProcess(validPtr);  // Works fine
    safeProcess(nullPtr);   // Safely handles null
    
    return 0;
}
```

## Pointers and Arrays

Arrays and pointers have a close relationship in C++:

### Array Name as Pointer

```cpp
#include <iostream>

int main() {
    int numbers[] = {10, 20, 30, 40, 50};
    int* ptr = numbers;  // Array name converts to pointer to first element
    
    std::cout << "Array access vs pointer access:" << std::endl;
    for (int i = 0; i < 5; i++) {
        std::cout << "numbers[" << i << "] = " << numbers[i];
        std::cout << ", ptr[" << i << "] = " << ptr[i];
        std::cout << ", *(ptr + " << i << ") = " << *(ptr + i) << std::endl;
    }
    
    // Array name and pointer are almost equivalent
    std::cout << "\nAddresses:" << std::endl;
    std::cout << "numbers = " << numbers << std::endl;
    std::cout << "&numbers[0] = " << &numbers[0] << std::endl;
    std::cout << "ptr = " << ptr << std::endl;
    
    return 0;
}
```

### Pointer Arithmetic

```cpp
#include <iostream>

int main() {
    int array[] = {100, 200, 300, 400, 500};
    int* ptr = array;
    
    std::cout << "Pointer arithmetic:" << std::endl;
    std::cout << "ptr points to: " << *ptr << std::endl;        // 100
    
    ptr++;  // Move to next element
    std::cout << "After ptr++: " << *ptr << std::endl;         // 200
    
    ptr += 2;  // Move forward 2 elements
    std::cout << "After ptr += 2: " << *ptr << std::endl;      // 400
    
    ptr--;  // Move back 1 element
    std::cout << "After ptr--: " << *ptr << std::endl;         // 300
    
    // Calculate distance between pointers
    int* start = array;
    int* end = &array[4];
    std::cout << "Distance: " << end - start << " elements" << std::endl;  // 4
    
    return 0;
}
```

### Traversing Arrays with Pointers

```cpp
#include <iostream>

void printArray(int* arr, int size) {
    std::cout << "Array contents: ";
    for (int i = 0; i < size; i++) {
        std::cout << arr[i] << " ";
    }
    std::cout << std::endl;
}

void printArrayPointer(int* start, int* end) {
    std::cout << "Array via pointers: ";
    for (int* ptr = start; ptr < end; ptr++) {
        std::cout << *ptr << " ";
    }
    std::cout << std::endl;
}

int main() {
    int numbers[] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    int size = sizeof(numbers) / sizeof(numbers[0]);
    
    printArray(numbers, size);
    printArrayPointer(numbers, numbers + size);
    
    return 0;
}
```

## Pointers to Pointers

Pointers can point to other pointers:

```cpp
#include <iostream>

int main() {
    int value = 42;
    int* ptr = &value;      // Pointer to int
    int** ptrToPtr = &ptr;  // Pointer to pointer to int
    
    std::cout << "Multi-level indirection:" << std::endl;
    std::cout << "value = " << value << std::endl;
    std::cout << "*ptr = " << *ptr << std::endl;
    std::cout << "**ptrToPtr = " << **ptrToPtr << std::endl;
    
    // Modify value through double pointer
    **ptrToPtr = 100;
    std::cout << "After **ptrToPtr = 100:" << std::endl;
    std::cout << "value = " << value << std::endl;
    
    // Addresses at each level
    std::cout << "\nAddress breakdown:" << std::endl;
    std::cout << "&value = " << &value << std::endl;
    std::cout << "ptr = " << ptr << std::endl;
    std::cout << "&ptr = " << &ptr << std::endl;
    std::cout << "ptrToPtr = " << ptrToPtr << std::endl;
    
    return 0;
}
```

### 2D Arrays and Pointers

```cpp
#include <iostream>

void print2DArray(int** matrix, int rows, int cols) {
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            std::cout << matrix[i][j] << " ";
        }
        std::cout << std::endl;
    }
}

int main() {
    const int ROWS = 3;
    const int COLS = 4;
    
    // Create 2D array using pointers
    int** matrix = new int*[ROWS];
    for (int i = 0; i < ROWS; i++) {
        matrix[i] = new int[COLS];
    }
    
    // Initialize with values
    int value = 1;
    for (int i = 0; i < ROWS; i++) {
        for (int j = 0; j < COLS; j++) {
            matrix[i][j] = value++;
        }
    }
    
    print2DArray(matrix, ROWS, COLS);
    
    // Clean up memory
    for (int i = 0; i < ROWS; i++) {
        delete[] matrix[i];
    }
    delete[] matrix;
    
    return 0;
}
```

## Function Pointers

Pointers can also point to functions:

```cpp
#include <iostream>

// Function prototypes
int add(int a, int b) {
    return a + b;
}

int multiply(int a, int b) {
    return a * b;
}

int subtract(int a, int b) {
    return a - b;
}

// Function that takes a function pointer
int calculate(int x, int y, int (*operation)(int, int)) {
    return operation(x, y);
}

int main() {
    // Function pointer declaration and assignment
    int (*mathOp)(int, int) = add;
    
    std::cout << "Using function pointer:" << std::endl;
    std::cout << "10 + 5 = " << mathOp(10, 5) << std::endl;
    
    // Change which function the pointer points to
    mathOp = multiply;
    std::cout << "10 * 5 = " << mathOp(10, 5) << std::endl;
    
    // Use with calculate function
    std::cout << "\nUsing function pointer as parameter:" << std::endl;
    std::cout << "add(7, 3) = " << calculate(7, 3, add) << std::endl;
    std::cout << "multiply(7, 3) = " << calculate(7, 3, multiply) << std::endl;
    std::cout << "subtract(7, 3) = " << calculate(7, 3, subtract) << std::endl;
    
    // Array of function pointers
    int (*operations[])(int, int) = {add, subtract, multiply};
    const char* names[] = {"add", "subtract", "multiply"};
    
    std::cout << "\nUsing array of function pointers:" << std::endl;
    for (int i = 0; i < 3; i++) {
        std::cout << names[i] << "(8, 4) = " << operations[i](8, 4) << std::endl;
    }
    
    return 0;
}
```

## Practical Pointer Examples

### Linked List Implementation

```cpp
#include <iostream>

struct Node {
    int data;
    Node* next;
    
    Node(int value) : data(value), next(nullptr) {}
};

class LinkedList {
private:
    Node* head;
    
public:
    LinkedList() : head(nullptr) {}
    
    ~LinkedList() {
        while (head != nullptr) {
            Node* temp = head;
            head = head->next;
            delete temp;
        }
    }
    
    void push_front(int value) {
        Node* newNode = new Node(value);
        newNode->next = head;
        head = newNode;
    }
    
    void print() const {
        Node* current = head;
        std::cout << "List: ";
        while (current != nullptr) {
            std::cout << current->data << " ";
            current = current->next;
        }
        std::cout << std::endl;
    }
    
    bool find(int value) const {
        Node* current = head;
        while (current != nullptr) {
            if (current->data == value) {
                return true;
            }
            current = current->next;
        }
        return false;
    }
};

int main() {
    LinkedList list;
    
    list.push_front(30);
    list.push_front(20);
    list.push_front(10);
    
    list.print();
    
    std::cout << "Find 20: " << std::boolalpha << list.find(20) << std::endl;
    std::cout << "Find 40: " << std::boolalpha << list.find(40) << std::endl;
    
    return 0;
}
```

### Pointer-Based String Operations

```cpp
#include <iostream>
#include <cstring>

int stringLength(const char* str) {
    int length = 0;
    while (*str != '\0') {
        length++;
        str++;
    }
    return length;
}

void stringCopy(char* dest, const char* src) {
    while (*src != '\0') {
        *dest = *src;
        dest++;
        src++;
    }
    *dest = '\0';  // Null terminator
}

bool stringEqual(const char* str1, const char* str2) {
    while (*str1 != '\0' && *str2 != '\0') {
        if (*str1 != *str2) {
            return false;
        }
        str1++;
        str2++;
    }
    return *str1 == *str2;  // Both should be '\0'
}

int main() {
    const char* original = "Hello, World!";
    char copy[100];
    
    std::cout << "Original: " << original << std::endl;
    std::cout << "Length: " << stringLength(original) << std::endl;
    
    stringCopy(copy, original);
    std::cout << "Copy: " << copy << std::endl;
    
    std::cout << "Are they equal? " << std::boolalpha 
              << stringEqual(original, copy) << std::endl;
    
    return 0;
}
```

## Common Pointer Mistakes

### Dangling Pointers

```cpp
#include <iostream>

int* createDanglingPointer() {
    int localVar = 42;
    return &localVar;  // ✘ Dangerous! localVar is destroyed when function ends
}

int* createValidPointer() {
    int* heapVar = new int(42);
    return heapVar;  // → OK, but caller must delete
}

int main() {
    // ✘ Dangling pointer
    int* dangerous = createDanglingPointer();
    // std::cout << *dangerous;  // Undefined behavior!
    
    // → Valid pointer (but requires cleanup)
    int* valid = createValidPointer();
    std::cout << "Valid value: " << *valid << std::endl;
    delete valid;  // Clean up
    
    return 0;
}
```

### Memory Leaks

```cpp
#include <iostream>

void memoryLeak() {
    int* ptr = new int(100);
    // ✘ Forgot to delete! Memory leaked
    
    if (true) {
        return;  // Early return without cleanup
    }
    
    delete ptr;  // Never reached
}

void properCleanup() {
    int* ptr = new int(100);
    
    // Use the pointer...
    std::cout << "Value: " << *ptr << std::endl;
    
    delete ptr;  // → Proper cleanup
    ptr = nullptr;  // → Good practice
}
```

### Buffer Overruns

```cpp
#include <iostream>

void bufferOverrun() {
    int array[5] = {1, 2, 3, 4, 5};
    int* ptr = array;
    
    // ✘ Dangerous: accessing beyond array bounds
    for (int i = 0; i < 10; i++) {  // Should be i < 5
        // std::cout << ptr[i] << " ";  // Undefined behavior when i >= 5
    }
    
    // → Safe version
    for (int i = 0; i < 5; i++) {
        std::cout << ptr[i] << " ";
    }
    std::cout << std::endl;
}
```

## Best Practices

### Safe Pointer Usage

```cpp
#include <iostream>

class SafePointerExample {
private:
    int* data;
    size_t size;
    
public:
    SafePointerExample(size_t s) : size(s) {
        data = new int[size]();  // Initialize to zero
    }
    
    ~SafePointerExample() {
        delete[] data;
        data = nullptr;  // Good practice
    }
    
    // Prevent accidental copying
    SafePointerExample(const SafePointerExample&) = delete;
    SafePointerExample& operator=(const SafePointerExample&) = delete;
    
    int& at(size_t index) {
        if (index >= size) {
            throw std::out_of_range("Index out of bounds");
        }
        return data[index];
    }
    
    size_t getSize() const { return size; }
};

int main() {
    SafePointerExample container(10);
    
    for (size_t i = 0; i < container.getSize(); i++) {
        container.at(i) = i * i;
    }
    
    for (size_t i = 0; i < container.getSize(); i++) {
        std::cout << container.at(i) << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

## Exercises

### Exercise 1: Pointer Basics

Write functions that:

- Swap two integers using pointers
- Find the maximum element in an array using pointers
- Reverse an array in place using pointers

### Exercise 2: Dynamic Arrays

Implement a dynamic array class that:

- Grows automatically when capacity is exceeded
- Provides bounds checking
- Properly manages memory

### Exercise 3: Text Processing

Create pointer-based functions for:

- Counting words in a C-string
- Finding and replacing substrings
- Converting strings to uppercase/lowercase

### Exercise 4: Data Structures

Implement using pointers:

- A simple stack
- A binary tree with insertion and traversal
- A hash table with collision handling

## Key Takeaways

<i class="fa-solid fa-arrow-right"></i> **Always initialize pointers** (preferably to `nullptr`)  
<i class="fa-solid fa-arrow-right"></i> **Check for null before dereferencing**  
<i class="fa-solid fa-arrow-right"></i> **Match every `new` with `delete`** and `new[]` with `delete[]`  
<i class="fa-solid fa-arrow-right"></i> **Set pointers to `nullptr` after deletion**  
<i class="fa-solid fa-arrow-right"></i> **Be careful with pointer arithmetic** and array bounds  
<i class="fa-solid fa-arrow-right"></i> **Understand the relationship between arrays and pointers**

---

**Next**: Learn about references as a safer alternative to pointers // →  [References](references.md)
