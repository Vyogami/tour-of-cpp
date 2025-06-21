# Dynamic Memory Allocation

Dynamic memory allocation allows you to request memory at runtime, providing flexibility to create objects whose size or lifetime isn't known at compile time. This is essential for data structures that grow, shrink, or need to persist beyond their original scope.

## Stack vs Heap Allocation

### Stack Allocation (Automatic)

```cpp
#include <iostream>

void stackAllocation() {
    // Stack allocation - automatic management
    int stackArray[100];           // Fixed size, fast allocation
    char buffer[1024];             // Size must be known at compile time
    
    // Automatically destroyed when function ends
    std::cout << "Stack allocated array size: " << sizeof(stackArray) << std::endl;
    
    // ✘ Cannot do this - size must be compile-time constant
    // int size = getUserInput();
    // int dynamicArray[size];  // Error in older C++ standards
}
```

### Heap Allocation (Dynamic)

```cpp
#include <iostream>

void heapAllocation() {
    // Heap allocation - manual management required
    int* heapArray = new int[100];     // Size can be determined at runtime
    char* dynamicBuffer = new char[1024];
    
    // Use the allocated memory
    for (int i = 0; i < 100; i++) {
        heapArray[i] = i * i;
    }
    
    std::cout << "First few squares: ";
    for (int i = 0; i < 5; i++) {
        std::cout << heapArray[i] << " ";
    }
    std::cout << std::endl;
    
    // Must manually free the memory
    delete[] heapArray;
    delete[] dynamicBuffer;
}

int main() {
    stackAllocation();
    heapAllocation();
    return 0;
}
```

## The `new` and `delete` Operators

### Basic Syntax

```cpp
#include <iostream>

int main() {
    // Single object allocation
    int* singleInt = new int;              // Uninitialized
    int* initializedInt = new int(42);     // Initialized to 42
    int* zeroInt = new int();              // Zero-initialized
    
    // Array allocation
    int* intArray = new int[10];           // Array of 10 uninitialized ints
    int* zeroArray = new int[10]();        // Array of 10 zero-initialized ints
    
    std::cout << "Single int: " << *initializedInt << std::endl;
    std::cout << "Zero int: " << *zeroInt << std::endl;
    
    // Use array
    for (int i = 0; i < 10; i++) {
        intArray[i] = i + 1;
    }
    
    std::cout << "Array contents: ";
    for (int i = 0; i < 10; i++) {
        std::cout << intArray[i] << " ";
    }
    std::cout << std::endl;
    
    // Cleanup - CRITICAL!
    delete singleInt;
    delete initializedInt;
    delete zeroInt;
    delete[] intArray;      // Note: delete[] for arrays
    delete[] zeroArray;
    
    return 0;
}
```

### Important Rules

- **`new`** allocates memory and calls constructor
- **`delete`** calls destructor and frees memory
- **`new[]`** allocates array and calls constructors for each element
- **`delete[]`** calls destructors for each element and frees array memory
- **Always match**: `new` with `delete`, `new[]` with `delete[]`

## Dynamic Objects and Classes

### Creating Objects Dynamically

```cpp
#include <iostream>
#include <string>

class Person {
private:
    std::string name;
    int age;
    
public:
    Person() : name("Unknown"), age(0) {
        std::cout << "Person default constructor called" << std::endl;
    }
    
    Person(const std::string& n, int a) : name(n), age(a) {
        std::cout << "Person constructor called for " << name << std::endl;
    }
    
    ~Person() {
        std::cout << "Person destructor called for " << name << std::endl;
    }
    
    void introduce() const {
        std::cout << "Hi, I'm " << name << ", age " << age << std::endl;
    }
    
    void setAge(int newAge) { age = newAge; }
    const std::string& getName() const { return name; }
};

int main() {
    std::cout << "=== Creating objects on stack ===" << std::endl;
    {
        Person stackPerson("Alice", 25);
        stackPerson.introduce();
    }  // Destructor called automatically here
    
    std::cout << "\n=== Creating objects on heap ===" << std::endl;
    Person* heapPerson1 = new Person();               // Default constructor
    Person* heapPerson2 = new Person("Bob", 30);      // Parameterized constructor
    
    heapPerson1->introduce();
    heapPerson2->introduce();
    
    // Modify objects
    heapPerson1->setAge(28);
    heapPerson1->introduce();
    
    // Manual cleanup required
    delete heapPerson1;
    delete heapPerson2;
    
    std::cout << "\n=== Creating arrays of objects ===" << std::endl;
    Person* personArray = new Person[3];  // Calls default constructor 3 times
    
    // Note: Cannot pass parameters to constructors in array new
    // Must use default constructor, then modify
    
    delete[] personArray;  // Calls destructor 3 times
    
    return 0;
}
```

## Dynamic Arrays

### Runtime-Sized Arrays

```cpp
#include <iostream>

int* createArray(int size, int initialValue = 0) {
    int* array = new int[size];
    
    for (int i = 0; i < size; i++) {
        array[i] = initialValue;
    }
    
    return array;
}

void printArray(int* array, int size) {
    for (int i = 0; i < size; i++) {
        std::cout << array[i] << " ";
    }
    std::cout << std::endl;
}

void resizeArray(int*& array, int oldSize, int newSize) {
    // Create new array
    int* newArray = new int[newSize];
    
    // Copy old data
    int copySize = (oldSize < newSize) ? oldSize : newSize;
    for (int i = 0; i < copySize; i++) {
        newArray[i] = array[i];
    }
    
    // Initialize new elements to zero
    for (int i = oldSize; i < newSize; i++) {
        newArray[i] = 0;
    }
    
    // Clean up old array
    delete[] array;
    
    // Update pointer
    array = newArray;
}

int main() {
    int size;
    std::cout << "Enter array size: ";
    std::cin >> size;
    
    // Create dynamic array
    int* numbers = createArray(size, 1);
    
    std::cout << "Initial array: ";
    printArray(numbers, size);
    
    // Modify some elements
    for (int i = 0; i < size; i++) {
        numbers[i] = i * i;
    }
    
    std::cout << "Modified array: ";
    printArray(numbers, size);
    
    // Resize array
    int newSize = size * 2;
    resizeArray(numbers, size, newSize);
    
    std::cout << "Resized array: ";
    printArray(numbers, newSize);
    
    // Cleanup
    delete[] numbers;
    
    return 0;
}
```

### 2D Dynamic Arrays

```cpp
#include <iostream>

// Method 1: Array of pointers
int** create2DArray(int rows, int cols) {
    int** array = new int*[rows];
    
    for (int i = 0; i < rows; i++) {
        array[i] = new int[cols];
    }
    
    return array;
}

void delete2DArray(int** array, int rows) {
    for (int i = 0; i < rows; i++) {
        delete[] array[i];
    }
    delete[] array;
}

// Method 2: Single allocation with indexing
int* create2DArrayFlat(int rows, int cols) {
    return new int[rows * cols];
}

int& getElement(int* array, int row, int col, int cols) {
    return array[row * cols + col];
}

void fill2DArray(int** array, int rows, int cols) {
    int value = 1;
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            array[i][j] = value++;
        }
    }
}

void print2DArray(int** array, int rows, int cols) {
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            std::cout << array[i][j] << "\t";
        }
        std::cout << std::endl;
    }
}

int main() {
    const int ROWS = 3;
    const int COLS = 4;
    
    // Method 1: Array of pointers
    std::cout << "2D Array (array of pointers):" << std::endl;
    int** matrix1 = create2DArray(ROWS, COLS);
    fill2DArray(matrix1, ROWS, COLS);
    print2DArray(matrix1, ROWS, COLS);
    delete2DArray(matrix1, ROWS);
    
    // Method 2: Flat array with manual indexing
    std::cout << "\n2D Array (flat allocation):" << std::endl;
    int* matrix2 = create2DArrayFlat(ROWS, COLS);
    
    int value = 1;
    for (int i = 0; i < ROWS; i++) {
        for (int j = 0; j < COLS; j++) {
            getElement(matrix2, i, j, COLS) = value++;
        }
    }
    
    for (int i = 0; i < ROWS; i++) {
        for (int j = 0; j < COLS; j++) {
            std::cout << getElement(matrix2, i, j, COLS) << "\t";
        }
        std::cout << std::endl;
    }
    
    delete[] matrix2;
    
    return 0;
}
```

## Memory Management Patterns

### RAII (Resource Acquisition Is Initialization)

```cpp
#include <iostream>

class DynamicArray {
private:
    int* data;
    size_t size;
    
public:
    // Constructor acquires resource
    DynamicArray(size_t s) : size(s) {
        data = new int[size]();  // Zero-initialize
        std::cout << "Allocated array of size " << size << std::endl;
    }
    
    // Destructor releases resource
    ~DynamicArray() {
        delete[] data;
        std::cout << "Deallocated array of size " << size << std::endl;
    }
    
    // Copy constructor (deep copy)
    DynamicArray(const DynamicArray& other) : size(other.size) {
        data = new int[size];
        for (size_t i = 0; i < size; i++) {
            data[i] = other.data[i];
        }
        std::cout << "Copied array of size " << size << std::endl;
    }
    
    // Assignment operator (deep copy)
    DynamicArray& operator=(const DynamicArray& other) {
        if (this != &other) {
            delete[] data;  // Release old resource
            
            size = other.size;
            data = new int[size];  // Acquire new resource
            for (size_t i = 0; i < size; i++) {
                data[i] = other.data[i];
            }
            std::cout << "Assigned array of size " << size << std::endl;
        }
        return *this;
    }
    
    // Access operators
    int& operator[](size_t index) {
        return data[index];
    }
    
    const int& operator[](size_t index) const {
        return data[index];
    }
    
    size_t getSize() const { return size; }
};

void demonstrateRAII() {
    std::cout << "=== RAII Demonstration ===" << std::endl;
    
    {
        DynamicArray arr1(5);
        for (size_t i = 0; i < arr1.getSize(); i++) {
            arr1[i] = i * 10;
        }
        
        DynamicArray arr2 = arr1;  // Copy constructor
        DynamicArray arr3(3);
        arr3 = arr1;               // Assignment operator
        
    }  // All destructors called automatically here
    
    std::cout << "All arrays automatically cleaned up!" << std::endl;
}

int main() {
    demonstrateRAII();
    return 0;
}
```

### Factory Pattern for Dynamic Objects

```cpp
#include <iostream>
#include <string>

class Shape {
public:
    virtual ~Shape() = default;
    virtual void draw() const = 0;
    virtual double area() const = 0;
};

class Circle : public Shape {
private:
    double radius;
    
public:
    Circle(double r) : radius(r) {}
    
    void draw() const override {
        std::cout << "Drawing circle with radius " << radius << std::endl;
    }
    
    double area() const override {
        return 3.14159 * radius * radius;
    }
};

class Rectangle : public Shape {
private:
    double width, height;
    
public:
    Rectangle(double w, double h) : width(w), height(h) {}
    
    void draw() const override {
        std::cout << "Drawing rectangle " << width << "x" << height << std::endl;
    }
    
    double area() const override {
        return width * height;
    }
};

class ShapeFactory {
public:
    static Shape* createShape(const std::string& type, double param1, double param2 = 0) {
        if (type == "circle") {
            return new Circle(param1);
        } else if (type == "rectangle") {
            return new Rectangle(param1, param2);
        } else {
            return nullptr;
        }
    }
};

int main() {
    // Create shapes dynamically
    Shape* shapes[] = {
        ShapeFactory::createShape("circle", 5.0),
        ShapeFactory::createShape("rectangle", 4.0, 6.0),
        ShapeFactory::createShape("circle", 3.0)
    };
    
    const int numShapes = sizeof(shapes) / sizeof(shapes[0]);
    
    // Use shapes
    for (int i = 0; i < numShapes; i++) {
        if (shapes[i]) {
            shapes[i]->draw();
            std::cout << "Area: " << shapes[i]->area() << std::endl;
        }
    }
    
    // Cleanup
    for (int i = 0; i < numShapes; i++) {
        delete shapes[i];
    }
    
    return 0;
}
```

## Common Memory Management Errors

### Memory Leaks

```cpp
#include <iostream>

void memoryLeak() {
    int* ptr = new int(42);
    
    if (true) {
        return;  // ✘ Early return without delete - memory leaked!
    }
    
    delete ptr;  // Never reached
}

void memoryLeakFixed() {
    int* ptr = new int(42);
    
    // Use the pointer...
    std::cout << "Value: " << *ptr << std::endl;
    
    if (true) {
        delete ptr;  // → Clean up before return
        return;
    }
    
    delete ptr;  // Also clean up here if needed
}

// → Better: Use RAII
void noMemoryLeak() {
    DynamicArray arr(10);  // Automatic cleanup in destructor
    
    if (true) {
        return;  // → Destructor called automatically
    }
}
```

### Double Deletion

```cpp
#include <iostream>

void doubleDeletion() {
    int* ptr = new int(42);
    
    delete ptr;
    // ptr = nullptr;  // → Good practice: set to null after delete
    
    delete ptr;  // ✘ Undefined behavior: double deletion
}

void safePattern() {
    int* ptr = new int(42);
    
    delete ptr;
    ptr = nullptr;  // → Safe: deleting nullptr is a no-op
    
    delete ptr;     // → Safe: this does nothing
}
```

### Using After Delete

```cpp
#include <iostream>

void useAfterDelete() {
    int* ptr = new int(42);
    
    delete ptr;
    
    std::cout << *ptr << std::endl;  // ✘ Undefined behavior: use after delete
    *ptr = 100;                      // ✘ Undefined behavior: modify after delete
}

void safeAccess() {
    int* ptr = new int(42);
    
    std::cout << *ptr << std::endl;  // → Use before delete
    
    delete ptr;
    ptr = nullptr;                   // → Clear pointer
    
    if (ptr != nullptr) {            // → Check before use
        std::cout << *ptr << std::endl;
    }
}
```

### Array/Single Object Mismatch

```cpp
#include <iostream>

void deleteMismatch() {
    int* single = new int(42);
    int* array = new int[10];
    
    delete array;     // ✘ Wrong: should be delete[]
    delete[] single;  // ✘ Wrong: should be delete
}

void correctDeletion() {
    int* single = new int(42);
    int* array = new int[10];
    
    delete single;    // → Correct for single object
    delete[] array;   // → Correct for array
}
```

## Memory Debugging Techniques

### Custom Memory Tracking

```cpp
#include <iostream>
#include <map>

class MemoryTracker {
private:
    static std::map<void*, size_t> allocations;
    static size_t totalAllocated;
    static size_t allocationCount;
    
public:
    static void* allocate(size_t size) {
        void* ptr = std::malloc(size);
        if (ptr) {
            allocations[ptr] = size;
            totalAllocated += size;
            allocationCount++;
            std::cout << "Allocated " << size << " bytes at " << ptr 
                      << " (total: " << totalAllocated << ")" << std::endl;
        }
        return ptr;
    }
    
    static void deallocate(void* ptr) {
        auto it = allocations.find(ptr);
        if (it != allocations.end()) {
            size_t size = it->second;
            totalAllocated -= size;
            allocationCount--;
            allocations.erase(it);
            std::cout << "Deallocated " << size << " bytes at " << ptr 
                      << " (total: " << totalAllocated << ")" << std::endl;
            std::free(ptr);
        } else {
            std::cout << "Warning: Attempting to delete untracked pointer " << ptr << std::endl;
        }
    }
    
    static void printStats() {
        std::cout << "Memory Statistics:" << std::endl;
        std::cout << "Active allocations: " << allocationCount << std::endl;
        std::cout << "Total allocated: " << totalAllocated << " bytes" << std::endl;
        
        if (!allocations.empty()) {
            std::cout << "Leaked memory:" << std::endl;
            for (const auto& alloc : allocations) {
                std::cout << "  " << alloc.first << ": " << alloc.second << " bytes" << std::endl;
            }
        }
    }
};

// Static member definitions
std::map<void*, size_t> MemoryTracker::allocations;
size_t MemoryTracker::totalAllocated = 0;
size_t MemoryTracker::allocationCount = 0;

// Custom new/delete operators for tracking
void* operator new(size_t size) {
    return MemoryTracker::allocate(size);
}

void operator delete(void* ptr) noexcept {
    MemoryTracker::deallocate(ptr);
}

void testMemoryTracking() {
    std::cout << "=== Memory Tracking Test ===" << std::endl;
    
    int* ptr1 = new int(42);
    int* ptr2 = new int[10];
    
    MemoryTracker::printStats();
    
    delete ptr1;
    // Intentionally forget to delete ptr2 to show leak detection
    
    MemoryTracker::printStats();
}
```

## Best Practices

### Modern C++ Alternatives

```cpp
#include <iostream>
#include <vector>
#include <memory>

void modernAlternatives() {
    // ✘ Old style: manual memory management
    int* oldArray = new int[100];
    // ... use array ...
    delete[] oldArray;
    
    // → Modern: use containers
    std::vector<int> modernArray(100);
    // Automatic cleanup, bounds checking, resizing, etc.
    
    // ✘ Old style: raw pointers for objects
    Person* oldPerson = new Person("Alice", 25);
    // ... use person ...
    delete oldPerson;
    
    // → Modern: smart pointers
    std::unique_ptr<Person> modernPerson = std::make_unique<Person>("Bob", 30);
    // Automatic cleanup, exception safety, etc.
}
```

### Exception Safety

```cpp
#include <iostream>
#include <stdexcept>

void unsafeFunction() {
    int* ptr1 = new int(10);
    int* ptr2 = new int(20);
    
    // If this throws, ptr1 and ptr2 are leaked!
    if (true) {
        throw std::runtime_error("Something went wrong");
    }
    
    delete ptr1;
    delete ptr2;
}

void saferFunction() {
    int* ptr1 = nullptr;
    int* ptr2 = nullptr;
    
    try {
        ptr1 = new int(10);
        ptr2 = new int(20);
        
        // Risky operation
        if (true) {
            throw std::runtime_error("Something went wrong");
        }
        
        delete ptr1;
        delete ptr2;
    } catch (...) {
        delete ptr1;
        delete ptr2;
        throw;  // Re-throw the exception
    }
}

void bestFunction() {
    // Use RAII - automatic cleanup even with exceptions
    DynamicArray arr1(10);
    DynamicArray arr2(20);
    
    // If this throws, destructors are called automatically
    if (true) {
        throw std::runtime_error("Something went wrong");
    }
}
```

## Exercises

### Exercise 1: Dynamic String Class

Implement a simple string class that:

- Dynamically allocates memory for character storage
- Supports concatenation, comparison, and substring operations
- Properly manages memory with copy constructor and assignment operator

### Exercise 2: Resizable Array

Create a dynamic array class that:

- Automatically grows when capacity is exceeded
- Shrinks when usage drops below a threshold
- Provides iterator-like access

### Exercise 3: Memory Pool

Implement a simple memory pool that:

- Pre-allocates a large block of memory
- Provides fast allocation/deallocation for fixed-size objects
- Tracks allocated and free blocks

### Exercise 4: Object Factory

Design a factory system that:

- Creates different types of objects dynamically
- Manages object lifetime
- Provides object pooling for performance

## Key Takeaways

<i class="fa-solid fa-arrow-right"></i> **Use `new`/`delete` for single objects, `new[]`/`delete[]` for arrays**  
<i class="fa-solid fa-arrow-right"></i> **Always match allocation and deallocation operations**  
<i class="fa-solid fa-arrow-right"></i> **Set pointers to `nullptr` after deletion**  
<i class="fa-solid fa-arrow-right"></i> **Prefer RAII and smart pointers over manual memory management**  
<i class="fa-solid fa-arrow-right"></i> **Handle exceptions properly to avoid memory leaks**  
<i class="fa-solid fa-arrow-right"></i> **Use tools and techniques to detect memory errors**

---

**Next**: Learn about modern C++ automatic memory management // →  [Smart Pointers](smart-pointers.md)
