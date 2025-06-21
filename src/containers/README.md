# Containers and Arrays

Containers are fundamental data structures that store collections of objects. C++ provides both low-level arrays inherited from C and modern container classes from the Standard Template Library (STL). Understanding these containers is essential for effective data management and algorithm implementation.

## What You'll Learn

- **Arrays and C-style Arrays** - Fixed-size sequential containers and their limitations
- **Vectors and Dynamic Arrays** - Resizable arrays with automatic memory management
- **Strings and Text Processing** - Specialized containers for character data
- **Other Standard Containers** - Maps, sets, lists, and specialized containers

## Why Containers Matter

Containers provide structured ways to organize and access data:

<i class="fa-solid fa-box"></i> **Data Organization**: Group related items together  
<i class="fa-solid fa-bolt"></i> **Efficient Access**: Optimized algorithms for common operations  
<i class="fa-solid fa-lock"></i> **Memory Safety**: Automatic bounds checking and memory management  
<i class="fa-solid fa-recycle"></i> **Reusability**: Standard interfaces work with algorithms  
<i class="fa-solid fa-bolt"></i> **Abstraction**: Hide implementation details behind clean interfaces

## Container Categories

C++ containers fall into several categories:

### Sequential Containers

Store elements in linear order:

```cpp
std::vector<int> numbers = {1, 2, 3, 4, 5};       // Dynamic array
std::array<int, 5> fixedNumbers = {1, 2, 3, 4, 5}; // Fixed array
std::list<int> linkedList = {1, 2, 3, 4, 5};      // Doubly-linked list
std::deque<int> doubleEnded = {1, 2, 3, 4, 5};    // Double-ended queue
```

### Associative Containers

Store elements in sorted order:

```cpp
std::set<int> uniqueNumbers = {5, 2, 8, 2, 1};         // Sorted unique values
std::map<std::string, int> ages = {{"Alice", 25}, {"Bob", 30}}; // Key-value pairs
std::multiset<int> allowDuplicates = {5, 2, 8, 2, 1};  // Sorted with duplicates
```

### Unordered Containers

Hash-based containers for fast lookup:

```cpp
std::unordered_set<int> fastSet = {1, 2, 3, 4, 5};
std::unordered_map<std::string, int> fastMap = {{"key1", 100}, {"key2", 200}};
```

### Container Adapters

Provide specific interfaces:

```cpp
std::stack<int> lifo;          // Last-in, first-out
std::queue<int> fifo;          // First-in, first-out
std::priority_queue<int> heap; // Priority-based ordering
```

## Evolution from C to Modern C++

### C-Style Arrays (Legacy)

```cpp
#include <iostream>

void cStyleArrays() {
    // Fixed size, manual memory management
    int numbers[5] = {1, 2, 3, 4, 5};
    int* dynamicArray = new int[10];
    
    // Manual bounds checking required
    for (int i = 0; i < 5; i++) {
        std::cout << numbers[i] << " ";
    }
    
    // Manual cleanup required
    delete[] dynamicArray;
    
    // Limitations:
    // ✘ No size information
    // ✘ No bounds checking
    // ✘ Manual memory management
    // ✘ Difficult to resize
}
```

### Modern C++ Containers

```cpp
#include <iostream>
#include <vector>
#include <array>

void modernContainers() {
    // Fixed size, stack-allocated
    std::array<int, 5> modernArray = {1, 2, 3, 4, 5};
    
    // Dynamic size, automatic memory management
    std::vector<int> dynamicVector = {1, 2, 3, 4, 5};
    
    // Automatic bounds checking (in debug mode)
    for (size_t i = 0; i < modernArray.size(); i++) {
        std::cout << modernArray[i] << " ";
    }
    
    // Range-based for loop
    for (int value : dynamicVector) {
        std::cout << value << " ";
    }
    
    // Benefits:
    // → Size information included
    // → Bounds checking available
    // → Automatic memory management
    // → Easy to resize (vector)
    // → STL algorithm compatibility
}
```

## Container Interface Commonalities

Most STL containers share common operations:

### Universal Operations

```cpp
#include <iostream>
#include <vector>
#include <list>
#include <set>

template<typename Container>
void demonstrateCommonInterface(Container& container) {
    // Size operations
    std::cout << "Size: " << container.size() << std::endl;
    std::cout << "Empty: " << std::boolalpha << container.empty() << std::endl;
    
    // Iterator operations
    std::cout << "Contents: ";
    for (auto it = container.begin(); it != container.end(); ++it) {
        std::cout << *it << " ";
    }
    std::cout << std::endl;
    
    // Range-based for loop (works with all containers)
    std::cout << "Range-for: ";
    for (const auto& element : container) {
        std::cout << element << " ";
    }
    std::cout << std::endl;
}

void demonstrateUniformInterface() {
    std::vector<int> vec = {1, 2, 3, 4, 5};
    std::list<int> lst = {1, 2, 3, 4, 5};
    std::set<int> st = {1, 2, 3, 4, 5};
    
    std::cout << "Vector:" << std::endl;
    demonstrateCommonInterface(vec);
    
    std::cout << "List:" << std::endl;
    demonstrateCommonInterface(lst);
    
    std::cout << "Set:" << std::endl;
    demonstrateCommonInterface(st);
}
```

## Performance Characteristics

Different containers have different performance trade-offs:

| Operation | Array/Vector | List | Set/Map | Unordered Set/Map |
|-----------|-------------|------|---------|------------------|
| **Random Access** | O(1) | O(n) | O(log n) | O(1) average |
| **Insert at End** | O(1) amortized | O(1) | O(log n) | O(1) average |
| **Insert at Beginning** | O(n) | O(1) | O(log n) | O(1) average |
| **Search** | O(n) | O(n) | O(log n) | O(1) average |
| **Memory Overhead** | Low | Medium | Medium | Medium |

### Choosing the Right Container

```cpp
#include <iostream>
#include <vector>
#include <list>
#include <set>
#include <unordered_set>

void containerChoiceExamples() {
    // → Use vector for: random access, cache-friendly iteration
    std::vector<int> scores;  // Game scores, frequently accessed by index
    
    // → Use list for: frequent insertion/deletion in middle
    std::list<std::string> todoList;  // Todo items, frequently reorganized
    
    // → Use set for: sorted unique elements, range queries
    std::set<std::string> sortedNames;  // Alphabetically sorted names
    
    // → Use unordered_set for: fast lookup, no ordering required
    std::unordered_set<int> visitedPages;  // Fast "have I seen this?" checks
    
    // → Use map for: key-value associations, sorted keys
    std::map<std::string, int> wordCount;  // Word frequency, alphabetical
    
    // → Use unordered_map for: fast key-value lookup
    std::unordered_map<int, std::string> userCache;  // Fast user ID -> name lookup
}
```

## Container Algorithms Integration

Containers work seamlessly with STL algorithms:

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <numeric>

void algorithmsWithContainers() {
    std::vector<int> numbers = {5, 2, 8, 1, 9, 3};
    
    // Sorting
    std::sort(numbers.begin(), numbers.end());
    
    // Searching
    auto it = std::find(numbers.begin(), numbers.end(), 8);
    if (it != numbers.end()) {
        std::cout << "Found 8 at position: " << std::distance(numbers.begin(), it) << std::endl;
    }
    
    // Transforming
    std::transform(numbers.begin(), numbers.end(), numbers.begin(),
                   [](int n) { return n * n; });
    
    // Accumulating
    int sum = std::accumulate(numbers.begin(), numbers.end(), 0);
    std::cout << "Sum of squares: " << sum << std::endl;
    
    // Filtering
    auto evenEnd = std::remove_if(numbers.begin(), numbers.end(),
                                  [](int n) { return n % 2 != 0; });
    numbers.erase(evenEnd, numbers.end());
    
    std::cout << "Even squares: ";
    for (int n : numbers) {
        std::cout << n << " ";
    }
    std::cout << std::endl;
}
```

## Memory Layout and Cache Efficiency

Understanding how containers store data affects performance:

### Contiguous vs Non-Contiguous Storage

```cpp
#include <iostream>
#include <vector>
#include <list>
#include <chrono>

void cacheEfficiencyDemo() {
    const size_t SIZE = 1000000;
    
    // Contiguous storage (cache-friendly)
    std::vector<int> vec(SIZE);
    std::iota(vec.begin(), vec.end(), 1);
    
    // Non-contiguous storage (cache-unfriendly)
    std::list<int> lst;
    for (size_t i = 1; i <= SIZE; ++i) {
        lst.push_back(i);
    }
    
    // Time vector iteration
    auto start = std::chrono::high_resolution_clock::now();
    long long vecSum = 0;
    for (int value : vec) {
        vecSum += value;
    }
    auto vecTime = std::chrono::high_resolution_clock::now() - start;
    
    // Time list iteration
    start = std::chrono::high_resolution_clock::now();
    long long lstSum = 0;
    for (int value : lst) {
        lstSum += value;
    }
    auto lstTime = std::chrono::high_resolution_clock::now() - start;
    
    std::cout << "Vector sum: " << vecSum 
              << " Time: " << std::chrono::duration_cast<std::chrono::milliseconds>(vecTime).count() << "ms" << std::endl;
    std::cout << "List sum: " << lstSum 
              << " Time: " << std::chrono::duration_cast<std::chrono::milliseconds>(lstTime).count() << "ms" << std::endl;
}
```

## Iterator Categories

Different containers provide different iterator capabilities:

```cpp
#include <iostream>
#include <vector>
#include <list>
#include <forward_list>

void iteratorCategories() {
    std::vector<int> vec = {1, 2, 3, 4, 5};
    std::list<int> lst = {1, 2, 3, 4, 5};
    std::forward_list<int> flst = {1, 2, 3, 4, 5};
    
    // Random access iterators (vector, array, deque)
    auto vecIt = vec.begin();
    vecIt += 3;                    // → Can jump by arbitrary amounts
    std::cout << "vec[3]: " << vecIt[0] << std::endl;  // → Subscript operator
    
    // Bidirectional iterators (list, set, map)
    auto lstIt = lst.begin();
    std::advance(lstIt, 3);        // → Can advance, but not jump
    ++lstIt;                       // → Can increment
    --lstIt;                       // → Can decrement
    
    // Forward iterators (forward_list, unordered containers)
    auto flstIt = flst.begin();
    ++flstIt;                      // → Can increment
    // --flstIt;                   // ✘ Cannot decrement
    // flstIt += 2;                // ✘ Cannot jump
}
```

## Container Safety and Best Practices

### Exception Safety

```cpp
#include <iostream>
#include <vector>
#include <stdexcept>

class ThrowingClass {
private:
    static int instances;
    int id;
    
public:
    ThrowingClass() : id(++instances) {
        if (instances > 3) {
            throw std::runtime_error("Too many instances");
        }
        std::cout << "ThrowingClass " << id << " created" << std::endl;
    }
    
    ~ThrowingClass() {
        std::cout << "ThrowingClass " << id << " destroyed" << std::endl;
        --instances;
    }
    
    // Copy constructor that might throw
    ThrowingClass(const ThrowingClass& other) : id(++instances) {
        if (instances > 3) {
            throw std::runtime_error("Too many instances in copy");
        }
        std::cout << "ThrowingClass " << id << " copied from " << other.id << std::endl;
    }
};

int ThrowingClass::instances = 0;

void exceptionSafetyDemo() {
    try {
        std::vector<ThrowingClass> vec;
        
        // This will create 3 objects successfully
        vec.resize(3);
        std::cout << "Successfully created 3 objects" << std::endl;
        
        // This will throw, but vector handles cleanup properly
        vec.resize(5);  // Throws during construction of 4th object
        
    } catch (const std::exception& e) {
        std::cout << "Exception: " << e.what() << std::endl;
        std::cout << "All objects properly cleaned up" << std::endl;
    }
}
```

## What's Coming Next

In this section, we'll explore:

### Array Fundamentals

- C-style arrays and their limitations
- Array decay and pointer relationships
- Multi-dimensional arrays

### Modern Array Containers

- `std::array` for fixed-size arrays
- `std::vector` for dynamic arrays
- Performance characteristics and when to use each

### String Processing

- `std::string` as a specialized container
- String operations and algorithms
- Text processing patterns

### Advanced Containers

- Associative containers (map, set)
- Unordered containers for hash-based lookup
- Container adapters (stack, queue, priority_queue)

Understanding containers is crucial for effective C++ programming. They provide the foundation for data structure design and algorithm implementation!

---

**Sections in This Chapter:**

- [Arrays and C-style Arrays // → ](arrays.md)
- [Vectors and Dynamic Arrays // → ](vectors.md)
- [Strings and Text Processing // → ](strings.md)
- [Other Standard Containers // → ](other-containers.md)
