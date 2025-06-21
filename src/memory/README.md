# Memory Management

Memory management is one of C++'s most powerful and distinctive features. Unlike garbage-collected languages, C++ gives you direct control over how and when memory is allocated and freed. This control enables high performance but requires understanding of pointers, references, and memory lifecycle.

## What You'll Learn

- **Pointers and Addresses** - Direct memory access and pointer arithmetic
- **References** - Aliases for existing variables and safer alternatives to pointers
- **Dynamic Memory Allocation** - Creating objects at runtime with `new` and `delete`
- **Smart Pointers** - Modern C++ automatic memory management

## Why Memory Management Matters

Understanding memory is crucial for:

<i class="fa-solid fa-rocket"></i> **Performance**: Direct memory control enables optimization  
<i class="fa-solid fa-lock"></i> **Safety**: Preventing memory leaks and access violations  
<i class="fa-solid fa-bolt"></i> **Efficiency**: Minimizing allocation overhead  
<i class="fa-solid fa-bullseye"></i> **Control**: Precise resource management  

## Memory Layout in C++

C++ programs use different memory regions:

```cpp
#include <iostream>

// Global/Static memory - exists for entire program duration
int globalVariable = 42;
static int staticVariable = 100;

void demonstrateMemoryRegions() {
    // Stack memory - automatic allocation/deallocation
    int stackVariable = 10;
    char stackArray[100];
    
    // Heap memory - manual allocation/deallocation
    int* heapVariable = new int(20);
    int* heapArray = new int[50];
    
    std::cout << "Stack variable address: " << &stackVariable << std::endl;
    std::cout << "Heap variable address: " << heapVariable << std::endl;
    std::cout << "Global variable address: " << &globalVariable << std::endl;
    
    // Must manually free heap memory
    delete heapVariable;
    delete[] heapArray;
}
```

### Memory Regions Explained

| Region | Purpose | Lifetime | Management |
|--------|---------|----------|------------|
| **Stack** | Local variables, function calls | Automatic (scope-based) | Compiler |
| **Heap** | Dynamic allocation | Manual | Programmer |
| **Global/Static** | Global variables, static data | Program lifetime | Compiler |
| **Code** | Program instructions | Program lifetime | OS |

## Stack vs Heap

### Stack Memory (Fast, Automatic)

```cpp
#include <iostream>

void stackExample() {
    int localVar = 42;          // Stack allocation
    char buffer[1024];          // Stack allocation
    
    // Advantages:
    // → Very fast allocation/deallocation
    // → Automatic cleanup when scope ends
    // → No fragmentation
    // → Cache-friendly (sequential)
    
    // Limitations:
    // ✘ Limited size (typically 1-8 MB)
    // ✘ Size must be known at compile time
    // ✘ Cannot outlive function scope
    
    std::cout << "Stack variable: " << localVar << std::endl;
} // localVar and buffer automatically destroyed here
```

### Heap Memory (Flexible, Manual)

```cpp
#include <iostream>

void heapExample() {
    int* dynamicVar = new int(42);      // Heap allocation
    int* dynamicArray = new int[1000];  // Heap allocation
    
    // Advantages:
    // → Size can be determined at runtime
    // → Large allocations possible
    // → Can outlive function scope
    // → Flexible lifetime management
    
    // Responsibilities:
    // Must manually free memory
    // Risk of memory leaks
    // Risk of double deletion
    // Slower than stack allocation
    
    std::cout << "Heap variable: " << *dynamicVar << std::endl;
    
    // Manual cleanup required
    delete dynamicVar;
    delete[] dynamicArray;
}
```

## The Power and Responsibility

C++ memory management follows the principle: **"You get what you pay for"**

### Power: Direct Control

```cpp
#include <iostream>
#include <chrono>

class PerformanceCritical {
private:
    static constexpr size_t POOL_SIZE = 1000;
    static char memoryPool[POOL_SIZE];
    static size_t poolIndex;
    
public:
    // Custom memory allocation for performance
    static void* operator new(size_t size) {
        if (poolIndex + size > POOL_SIZE) {
            throw std::bad_alloc();
        }
        void* ptr = &memoryPool[poolIndex];
        poolIndex += size;
        return ptr;
    }
    
    static void operator delete(void* ptr) {
        // Custom deallocation logic
        // In this example, we don't actually free (simple pool)
    }
};

char PerformanceCritical::memoryPool[POOL_SIZE];
size_t PerformanceCritical::poolIndex = 0;
```

### Responsibility: Common Pitfalls

```cpp
#include <iostream>

void commonMistakes() {
    // ✘ Memory leak - forgot to delete
    int* leaked = new int(42);
    // Memory never freed!
    
    // ✘ Double deletion
    int* ptr = new int(100);
    delete ptr;
    // delete ptr;  // Undefined behavior!
    
    // ✘ Using after delete
    int* dangling = new int(200);
    delete dangling;
    // std::cout << *dangling;  // Undefined behavior!
    
    // ✘ Array vs single object deletion mismatch
    int* array = new int[10];
    // delete array;    // Wrong! Should be delete[]
    delete[] array;     // Correct
    
    // ✘ Stack overflow
    // char hugeMistake[1000000000];  // Too big for stack!
}
```

## Modern C++ Solutions

### RAII (Resource Acquisition Is Initialization)

```cpp
#include <iostream>
#include <memory>

class RAIIExample {
private:
    int* data;
    size_t size;
    
public:
    RAIIExample(size_t s) : size(s) {
        data = new int[size];
        std::cout << "Allocated " << size << " integers" << std::endl;
    }
    
    ~RAIIExample() {
        delete[] data;
        std::cout << "Freed " << size << " integers" << std::endl;
    }
    
    // Prevent copying for simplicity (or implement proper copy semantics)
    RAIIExample(const RAIIExample&) = delete;
    RAIIExample& operator=(const RAIIExample&) = delete;
    
    int& operator[](size_t index) {
        return data[index];
    }
};

void demonstrateRAII() {
    RAIIExample container(100);
    container[0] = 42;
    std::cout << "First element: " << container[0] << std::endl;
    
    // Automatic cleanup when container goes out of scope
    // No explicit delete needed!
}
```

### Smart Pointers (Modern Approach)

```cpp
#include <iostream>
#include <memory>

void modernMemoryManagement() {
    // → unique_ptr - single ownership
    std::unique_ptr<int> unique = std::make_unique<int>(42);
    std::cout << "Unique value: " << *unique << std::endl;
    
    // → shared_ptr - shared ownership
    std::shared_ptr<int> shared1 = std::make_shared<int>(100);
    std::shared_ptr<int> shared2 = shared1;  // Shared ownership
    std::cout << "Shared value: " << *shared1 << std::endl;
    std::cout << "Reference count: " << shared1.use_count() << std::endl;
    
    // → weak_ptr - non-owning observer
    std::weak_ptr<int> observer = shared1;
    if (auto locked = observer.lock()) {
        std::cout << "Observed value: " << *locked << std::endl;
    }
    
    // Automatic cleanup - no explicit delete needed!
}
```

## Memory Safety Guidelines

### The Rule of Three/Five/Zero

```cpp
#include <iostream>

// Rule of Zero: Let compiler handle everything
class RuleOfZero {
private:
    std::unique_ptr<int> data;
public:
    RuleOfZero(int value) : data(std::make_unique<int>(value)) {}
    // Compiler-generated destructor, copy, and move operations are fine
};

// Rule of Three: If you need one, you probably need all three
class RuleOfThree {
private:
    int* data;
    size_t size;
    
public:
    // Constructor
    RuleOfThree(size_t s) : size(s), data(new int[size]) {}
    
    // 1. Destructor
    ~RuleOfThree() {
        delete[] data;
    }
    
    // 2. Copy constructor
    RuleOfThree(const RuleOfThree& other) : size(other.size), data(new int[size]) {
        std::copy(other.data, other.data + size, data);
    }
    
    // 3. Copy assignment operator
    RuleOfThree& operator=(const RuleOfThree& other) {
        if (this != &other) {
            delete[] data;
            size = other.size;
            data = new int[size];
            std::copy(other.data, other.data + size, data);
        }
        return *this;
    }
};
```

## Performance Considerations

### Memory Access Patterns

```cpp
#include <iostream>
#include <vector>
#include <chrono>

void cacheEfficiency() {
    const size_t SIZE = 1000000;
    std::vector<int> data(SIZE);
    
    // → Cache-friendly: sequential access
    auto start = std::chrono::high_resolution_clock::now();
    for (size_t i = 0; i < SIZE; ++i) {
        data[i] = i;
    }
    auto end = std::chrono::high_resolution_clock::now();
    
    std::cout << "Sequential access time: " 
              << std::chrono::duration_cast<std::chrono::microseconds>(end - start).count() 
              << " microseconds" << std::endl;
    
    // ✘ Cache-unfriendly: random access pattern would be slower
    // (Example omitted for brevity)
}
```

### Memory Alignment

```cpp
#include <iostream>
#include <cstddef>

struct Unaligned {
    char c;     // 1 byte
    int i;      // 4 bytes
    char c2;    // 1 byte
}; // Total: potentially 12 bytes due to padding

struct Aligned {
    int i;      // 4 bytes
    char c;     // 1 byte
    char c2;    // 1 byte
    // 2 bytes padding
}; // Total: 8 bytes

void checkSizes() {
    std::cout << "Unaligned struct size: " << sizeof(Unaligned) << std::endl;
    std::cout << "Aligned struct size: " << sizeof(Aligned) << std::endl;
    
    std::cout << "int alignment: " << alignof(int) << std::endl;
    std::cout << "char alignment: " << alignof(char) << std::endl;
}
```

## Memory Debugging and Tools

### Debug Techniques

```cpp
#include <iostream>

class DebugMemory {
private:
    static int allocationCount;
    static int totalAllocated;
    
public:
    static void* operator new(size_t size) {
        allocationCount++;
        totalAllocated += size;
        std::cout << "Allocating " << size << " bytes (total: " << totalAllocated << ")" << std::endl;
        return std::malloc(size);
    }
    
    static void operator delete(void* ptr, size_t size) {
        allocationCount--;
        totalAllocated -= size;
        std::cout << "Deallocating " << size << " bytes (total: " << totalAllocated << ")" << std::endl;
        std::free(ptr);
    }
    
    static void printStats() {
        std::cout << "Active allocations: " << allocationCount << std::endl;
        std::cout << "Total memory: " << totalAllocated << " bytes" << std::endl;
    }
};

int DebugMemory::allocationCount = 0;
int DebugMemory::totalAllocated = 0;
```

## What's Coming Next

In this section, we'll explore:

### Memory Fundamentals

- How addresses and pointers work
- Pointer arithmetic and array relationships
- Reference semantics and when to use them

### Dynamic Memory

- Manual allocation with `new` and `delete`
- Array allocation and proper cleanup
- Memory leak prevention

### Modern C++ Memory Management

- Smart pointers for automatic cleanup
- RAII principles and best practices
- Move semantics for efficient transfers

### Advanced Topics

- Custom allocators for performance
- Memory pools and object recycling
- Debugging memory issues

Understanding memory management is essential for writing efficient, safe C++ code. Let's start with the foundation: pointers and how they provide direct access to memory!

---

**Sections in This Chapter:**

- [Pointers and Addresses // → ](pointers.md)
- [References // → ](references.md)
- [Dynamic Memory Allocation // → ](dynamic-memory.md)
- [Smart Pointers // → ](smart-pointers.md)
