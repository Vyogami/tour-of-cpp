# Generic Programming

Generic programming is a programming paradigm that enables writing code that works with any data type while maintaining type safety and performance. In C++, templates are the primary mechanism for generic programming, allowing you to write functions and classes that can operate on different types without sacrificing efficiency or type checking.

## What You'll Learn

- **Function Templates** - Generic functions that work with multiple types
- **Class Templates** - Generic classes like containers and data structures
- **Template Specialization** - Customizing behavior for specific types
- **Concepts** - Type constraints and requirements (C++20)

## Why Generic Programming Matters

Without templates, you'd need to write separate functions for each type:

```cpp
// ✘ Without templates - repetitive code for each type
#include <iostream>

int max_int(int a, int b) {
    return (a > b) ? a : b;
}

double max_double(double a, double b) {
    return (a > b) ? a : b;
}

std::string max_string(const std::string& a, const std::string& b) {
    return (a > b) ? a : b;
}

int main() {
    std::cout << "Max int: " << max_int(10, 20) << std::endl;
    std::cout << "Max double: " << max_double(3.14, 2.71) << std::endl;
    std::cout << "Max string: " << max_string("apple", "banana") << std::endl;
    
    return 0;
}
```

```cpp
// → With templates - one function works for all comparable types
#include <iostream>
#include <string>

template<typename T>
T max_value(const T& a, const T& b) {
    return (a > b) ? a : b;
}

int main() {
    std::cout << "Max int: " << max_value(10, 20) << std::endl;
    std::cout << "Max double: " << max_value(3.14, 2.71) << std::endl;
    std::cout << "Max string: " << max_value(std::string("apple"), std::string("banana")) << std::endl;
    
    // Works with any comparable type
    std::cout << "Max char: " << max_value('x', 'a') << std::endl;
    
    return 0;
}
```

## Core Template Concepts

### <i class="fa-solid fa-bullseye"></i> **Type Parameters**

Templates are parameterized by types:

```cpp
#include <iostream>
#include <vector>
#include <string>

// Function template with type parameter
template<typename T>
void print_value(const T& value) {
    std::cout << "Value: " << value << std::endl;
}

// Function template with multiple type parameters
template<typename T, typename U>
void print_pair(const T& first, const U& second) {
    std::cout << "First: " << first << ", Second: " << second << std::endl;
}

// Class template with type parameter
template<typename T>
class SimpleContainer {
private:
    std::vector<T> data;
    
public:
    void add(const T& item) {
        data.push_back(item);
    }
    
    T get(size_t index) const {
        return data.at(index);
    }
    
    size_t size() const {
        return data.size();
    }
    
    void display() const {
        std::cout << "Container contents: ";
        for (const auto& item : data) {
            std::cout << item << " ";
        }
        std::cout << std::endl;
    }
};

int main() {
    // Function template usage
    print_value(42);
    print_value(3.14);
    print_value(std::string("Hello"));
    
    print_pair(10, "ten");
    print_pair(3.14, 'π');
    
    // Class template usage
    SimpleContainer<int> intContainer;
    intContainer.add(1);
    intContainer.add(2);
    intContainer.add(3);
    intContainer.display();
    
    SimpleContainer<std::string> stringContainer;
    stringContainer.add("Hello");
    stringContainer.add("World");
    stringContainer.display();
    
    return 0;
}
```

### <i class="fa-solid fa-screwdriver-wrench"></i> **Template Instantiation**

The compiler generates specific versions for each type used:

```cpp
#include <iostream>

template<typename T>
T square(const T& value) {
    std::cout << "Computing square of type: " << typeid(T).name() << std::endl;
    return value * value;
}

int main() {
    // Each call creates a different instantiation
    auto int_result = square(5);        // square<int> instantiated
    auto double_result = square(3.14);  // square<double> instantiated
    auto float_result = square(2.5f);   // square<float> instantiated
    
    std::cout << "Results: " << int_result << ", " << double_result << ", " << float_result << std::endl;
    
    // Explicit template instantiation
    auto long_result = square<long>(10L);
    std::cout << "Long result: " << long_result << std::endl;
    
    return 0;
}
```

### <i class="fa-solid fa-bolt"></i> **Zero-Cost Abstraction**

Templates provide abstraction without runtime overhead:

```cpp
#include <iostream>
#include <chrono>
#include <vector>

// Template function - no runtime overhead
template<typename T>
T add_template(const T& a, const T& b) {
    return a + b;
}

// Function with runtime polymorphism - has overhead
class Number {
public:
    virtual ~Number() = default;
    virtual double getValue() const = 0;
    virtual Number* add(const Number& other) const = 0;
};

class DoubleNumber : public Number {
private:
    double value;
public:
    DoubleNumber(double v) : value(v) {}
    double getValue() const override { return value; }
    Number* add(const Number& other) const override {
        return new DoubleNumber(value + other.getValue());
    }
};

void compare_performance() {
    const int iterations = 10000000;
    
    // Template version - compiled to efficient machine code
    auto start = std::chrono::high_resolution_clock::now();
    double template_result = 0;
    for (int i = 0; i < iterations; ++i) {
        template_result = add_template(template_result, 1.0);
    }
    auto end = std::chrono::high_resolution_clock::now();
    auto template_time = std::chrono::duration_cast<std::chrono::microseconds>(end - start);
    
    // Virtual function version - has virtual call overhead
    start = std::chrono::high_resolution_clock::now();
    std::unique_ptr<Number> poly_result = std::make_unique<DoubleNumber>(0.0);
    for (int i = 0; i < iterations; ++i) {
        DoubleNumber one(1.0);
        auto new_result = std::unique_ptr<Number>(poly_result->add(one));
        poly_result = std::move(new_result);
    }
    end = std::chrono::high_resolution_clock::now();
    auto poly_time = std::chrono::duration_cast<std::chrono::microseconds>(end - start);
    
    std::cout << "Template time: " << template_time.count() << " microseconds" << std::endl;
    std::cout << "Polymorphic time: " << poly_time.count() << " microseconds" << std::endl;
    std::cout << "Results: " << template_result << " vs " << poly_result->getValue() << std::endl;
}

int main() {
    compare_performance();
    return 0;
}
```

## Template Applications

### <i class="fa-solid fa-code"></i> **Generic Algorithms**

Write algorithms that work with any compatible type:

```cpp
#include <iostream>
#include <vector>
#include <list>
#include <string>
#include <algorithm>

// Generic sorting algorithm
template<typename Iterator>
void bubble_sort(Iterator begin, Iterator end) {
    for (auto i = begin; i != end; ++i) {
        for (auto j = begin; j != end - 1; ++j) {
            if (*j > *(j + 1)) {
                std::swap(*j, *(j + 1));
            }
        }
    }
}

// Generic search algorithm
template<typename Iterator, typename T>
Iterator linear_search(Iterator begin, Iterator end, const T& value) {
    for (auto it = begin; it != end; ++it) {
        if (*it == value) {
            return it;
        }
    }
    return end;
}

// Generic accumulation algorithm
template<typename Iterator, typename T>
T accumulate(Iterator begin, Iterator end, T initial_value) {
    T result = initial_value;
    for (auto it = begin; it != end; ++it) {
        result += *it;
    }
    return result;
}

// Generic transformation algorithm
template<typename InputIterator, typename OutputIterator, typename UnaryFunction>
void transform(InputIterator input_begin, InputIterator input_end,
               OutputIterator output_begin, UnaryFunction func) {
    while (input_begin != input_end) {
        *output_begin = func(*input_begin);
        ++input_begin;
        ++output_begin;
    }
}

int main() {
    // Test with different container types
    std::vector<int> numbers = {64, 34, 25, 12, 22, 11, 90};
    std::vector<std::string> words = {"banana", "apple", "cherry", "date"};
    
    std::cout << "Original numbers: ";
    for (int n : numbers) std::cout << n << " ";
    std::cout << std::endl;
    
    // Generic sorting
    bubble_sort(numbers.begin(), numbers.end());
    std::cout << "Sorted numbers: ";
    for (int n : numbers) std::cout << n << " ";
    std::cout << std::endl;
    
    bubble_sort(words.begin(), words.end());
    std::cout << "Sorted words: ";
    for (const auto& word : words) std::cout << word << " ";
    std::cout << std::endl;
    
    // Generic searching
    auto found = linear_search(numbers.begin(), numbers.end(), 25);
    if (found != numbers.end()) {
        std::cout << "Found 25 at position " << (found - numbers.begin()) << std::endl;
    }
    
    // Generic accumulation
    int sum = accumulate(numbers.begin(), numbers.end(), 0);
    std::cout << "Sum of numbers: " << sum << std::endl;
    
    std::string concatenated = accumulate(words.begin(), words.end(), std::string(""));
    std::cout << "Concatenated words: " << concatenated << std::endl;
    
    // Generic transformation
    std::vector<int> squared(numbers.size());
    transform(numbers.begin(), numbers.end(), squared.begin(), 
              [](int x) { return x * x; });
    
    std::cout << "Squared numbers: ";
    for (int n : squared) std::cout << n << " ";
    std::cout << std::endl;
    
    return 0;
}
```

### <i class="fa-solid fa-box"></i> **Generic Containers**

Create containers that work with any type:

```cpp
#include <iostream>
#include <stdexcept>

template<typename T>
class DynamicArray {
private:
    T* data;
    size_t capacity;
    size_t current_size;
    
    void resize() {
        size_t new_capacity = capacity * 2;
        T* new_data = new T[new_capacity];
        
        // Copy existing elements
        for (size_t i = 0; i < current_size; ++i) {
            new_data[i] = data[i];
        }
        
        delete[] data;
        data = new_data;
        capacity = new_capacity;
    }
    
public:
    DynamicArray(size_t initial_capacity = 4) 
        : capacity(initial_capacity), current_size(0) {
        data = new T[capacity];
    }
    
    ~DynamicArray() {
        delete[] data;
    }
    
    // Copy constructor
    DynamicArray(const DynamicArray& other) 
        : capacity(other.capacity), current_size(other.current_size) {
        data = new T[capacity];
        for (size_t i = 0; i < current_size; ++i) {
            data[i] = other.data[i];
        }
    }
    
    // Assignment operator
    DynamicArray& operator=(const DynamicArray& other) {
        if (this != &other) {
            delete[] data;
            capacity = other.capacity;
            current_size = other.current_size;
            data = new T[capacity];
            for (size_t i = 0; i < current_size; ++i) {
                data[i] = other.data[i];
            }
        }
        return *this;
    }
    
    void push_back(const T& value) {
        if (current_size >= capacity) {
            resize();
        }
        data[current_size++] = value;
    }
    
    void pop_back() {
        if (current_size > 0) {
            --current_size;
        }
    }
    
    T& operator[](size_t index) {
        if (index >= current_size) {
            throw std::out_of_range("Index out of range");
        }
        return data[index];
    }
    
    const T& operator[](size_t index) const {
        if (index >= current_size) {
            throw std::out_of_range("Index out of range");
        }
        return data[index];
    }
    
    size_t size() const { return current_size; }
    size_t get_capacity() const { return capacity; }
    bool empty() const { return current_size == 0; }
    
    // Iterator support
    T* begin() { return data; }
    T* end() { return data + current_size; }
    const T* begin() const { return data; }
    const T* end() const { return data + current_size; }
    
    void display() const {
        std::cout << "Array contents: ";
        for (size_t i = 0; i < current_size; ++i) {
            std::cout << data[i] << " ";
        }
        std::cout << "(size: " << current_size << ", capacity: " << capacity << ")" << std::endl;
    }
};

int main() {
    // Generic container works with any type
    std::cout << "=== Integer Array ===" << std::endl;
    DynamicArray<int> intArray;
    for (int i = 1; i <= 10; ++i) {
        intArray.push_back(i * i);
    }
    intArray.display();
    
    std::cout << "=== String Array ===" << std::endl;
    DynamicArray<std::string> stringArray;
    stringArray.push_back("Hello");
    stringArray.push_back("Generic");
    stringArray.push_back("Programming");
    stringArray.display();
    
    std::cout << "=== Double Array ===" << std::endl;
    DynamicArray<double> doubleArray;
    doubleArray.push_back(3.14);
    doubleArray.push_back(2.71);
    doubleArray.push_back(1.41);
    doubleArray.display();
    
    // Use with range-based for loop
    std::cout << "Using iterators: ";
    for (const auto& value : intArray) {
        std::cout << value << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

### <i class="fa-solid fa-magnifying-glass"></i> **Type Traits and Metaprogramming**

Use templates for compile-time computation and type analysis:

```cpp
#include <iostream>
#include <type_traits>
#include <string>

// Simple type trait
template<typename T>
struct is_pointer {
    static constexpr bool value = false;
};

template<typename T>
struct is_pointer<T*> {
    static constexpr bool value = true;
};

// Helper variable template (C++17)
template<typename T>
constexpr bool is_pointer_v = is_pointer<T>::value;

// SFINAE (Substitution Failure Is Not An Error) example
template<typename T>
typename std::enable_if<std::is_arithmetic<T>::value, void>::type
print_info(const T& value) {
    std::cout << "Arithmetic value: " << value << std::endl;
    std::cout << "  Size: " << sizeof(T) << " bytes" << std::endl;
    std::cout << "  Is integral: " << std::is_integral<T>::value << std::endl;
    std::cout << "  Is floating point: " << std::is_floating_point<T>::value << std::endl;
}

template<typename T>
typename std::enable_if<!std::is_arithmetic<T>::value, void>::type
print_info(const T& value) {
    std::cout << "Non-arithmetic value: " << value << std::endl;
    std::cout << "  Size: " << sizeof(T) << " bytes" << std::endl;
    std::cout << "  Is pointer: " << std::is_pointer<T>::value << std::endl;
}

// Conditional compilation based on type properties
template<typename T>
class SmartContainer {
private:
    T data;
    
public:
    SmartContainer(const T& value) : data(value) {}
    
    void process() {
        if constexpr (std::is_arithmetic_v<T>) {
            std::cout << "Processing arithmetic value: " << data * 2 << std::endl;
        }
        else if constexpr (std::is_same_v<T, std::string>) {
            std::cout << "Processing string: " << data + "!" << std::endl;
        }
        else {
            std::cout << "Processing unknown type" << std::endl;
        }
    }
    
    auto get_value() {
        if constexpr (std::is_pointer_v<T>) {
            return *data;  // Dereference if pointer
        } else {
            return data;   // Return value directly
        }
    }
};

// Compile-time factorial using templates
template<int N>
struct Factorial {
    static constexpr int value = N * Factorial<N-1>::value;
};

template<>
struct Factorial<0> {
    static constexpr int value = 1;
};

int main() {
    std::cout << "=== Type Analysis ===" << std::endl;
    print_info(42);
    print_info(3.14);
    print_info(std::string("Hello"));
    
    int* ptr = nullptr;
    print_info(ptr);
    
    std::cout << "\n=== Custom Type Traits ===" << std::endl;
    std::cout << "int is pointer: " << is_pointer_v<int> << std::endl;
    std::cout << "int* is pointer: " << is_pointer_v<int*> << std::endl;
    std::cout << "double** is pointer: " << is_pointer_v<double**> << std::endl;
    
    std::cout << "\n=== Smart Container ===" << std::endl;
    SmartContainer<int> intContainer(42);
    SmartContainer<std::string> stringContainer("Hello");
    
    intContainer.process();
    stringContainer.process();
    
    std::cout << "\n=== Compile-time Computation ===" << std::endl;
    constexpr int fact5 = Factorial<5>::value;
    constexpr int fact10 = Factorial<10>::value;
    
    std::cout << "5! = " << fact5 << std::endl;
    std::cout << "10! = " << fact10 << std::endl;
    
    return 0;
}
```

## Template Advantages

### <i class="fa-solid fa-bolt"></i> **Performance**

No runtime overhead - everything resolved at compile time:

```cpp
#include <iostream>
#include <vector>
#include <chrono>

// Template version - zero runtime overhead
template<typename T>
T generic_max(const T& a, const T& b) {
    return (a > b) ? a : b;
}

// Runtime polymorphism version - virtual call overhead
class Comparable {
public:
    virtual ~Comparable() = default;
    virtual bool operator>(const Comparable& other) const = 0;
    virtual Comparable* clone() const = 0;
};

class IntComparable : public Comparable {
private:
    int value;
public:
    IntComparable(int v) : value(v) {}
    bool operator>(const Comparable& other) const override {
        const IntComparable& other_int = static_cast<const IntComparable&>(other);
        return value > other_int.value;
    }
    Comparable* clone() const override {
        return new IntComparable(value);
    }
    int getValue() const { return value; }
};

void performance_comparison() {
    const int iterations = 1000000;
    
    // Template version
    auto start = std::chrono::high_resolution_clock::now();
    int template_result = 0;
    for (int i = 0; i < iterations; ++i) {
        template_result = generic_max(template_result, i);
    }
    auto end = std::chrono::high_resolution_clock::now();
    auto template_time = std::chrono::duration_cast<std::chrono::microseconds>(end - start);
    
    // Virtual function version
    start = std::chrono::high_resolution_clock::now();
    std::unique_ptr<Comparable> poly_result = std::make_unique<IntComparable>(0);
    for (int i = 0; i < iterations; ++i) {
        IntComparable current(i);
        if (current > *poly_result) {
            poly_result.reset(current.clone());
        }
    }
    end = std::chrono::high_resolution_clock::now();
    auto poly_time = std::chrono::duration_cast<std::chrono::microseconds>(end - start);
    
    std::cout << "Template time: " << template_time.count() << " μs" << std::endl;
    std::cout << "Virtual time: " << poly_time.count() << " μs" << std::endl;
    std::cout << "Speedup: " << (double)poly_time.count() / template_time.count() << "x" << std::endl;
}

int main() {
    performance_comparison();
    return 0;
}
```

### <i class="fa-solid fa-shield-halved"></i> **Type Safety**

Compile-time type checking prevents runtime errors:

```cpp
#include <iostream>
#include <vector>
#include <string>

// Type-safe generic container
template<typename T>
class TypeSafeVector {
private:
    std::vector<T> data;
    
public:
    void add(const T& item) {
        data.push_back(item);
    }
    
    T get(size_t index) const {
        return data.at(index);
    }
    
    size_t size() const {
        return data.size();
    }
    
    // Type-safe iteration
    void for_each(std::function<void(const T&)> func) const {
        for (const auto& item : data) {
            func(item);
        }
    }
    
    // Type-safe filtering
    TypeSafeVector<T> filter(std::function<bool(const T&)> predicate) const {
        TypeSafeVector<T> result;
        for (const auto& item : data) {
            if (predicate(item)) {
                result.add(item);
            }
        }
        return result;
    }
};

int main() {
    // Type safety at compile time
    TypeSafeVector<int> intVector;
    TypeSafeVector<std::string> stringVector;
    
    intVector.add(1);
    intVector.add(2);
    intVector.add(3);
    
    stringVector.add("Hello");
    stringVector.add("World");
    
    // This would cause a compile error:
    // intVector.add("string");  // ✘ Type mismatch!
    // stringVector.add(42);     // ✘ Type mismatch!
    
    std::cout << "Integer vector:" << std::endl;
    intVector.for_each([](const int& value) {
        std::cout << value << " ";
    });
    std::cout << std::endl;
    
    std::cout << "String vector:" << std::endl;
    stringVector.for_each([](const std::string& value) {
        std::cout << value << " ";
    });
    std::cout << std::endl;
    
    // Type-safe filtering
    auto evenNumbers = intVector.filter([](const int& x) {
        return x % 2 == 0;
    });
    
    std::cout << "Even numbers:" << std::endl;
    evenNumbers.for_each([](const int& value) {
        std::cout << value << " ";
    });
    std::cout << std::endl;
    
    return 0;
}
```

### <i class="fa-solid fa-recycle"></i> **Code Reusability**

Write once, use with many types:

```cpp
#include <iostream>
#include <vector>
#include <list>
#include <deque>
#include <string>

// Generic algorithm works with any container
template<typename Container>
void print_container(const Container& container, const std::string& name) {
    std::cout << name << ": ";
    for (const auto& item : container) {
        std::cout << item << " ";
    }
    std::cout << std::endl;
}

// Generic statistics function
template<typename Container>
auto calculate_statistics(const Container& container) {
    using ValueType = typename Container::value_type;
    
    if (container.empty()) {
        return std::make_tuple(ValueType{}, ValueType{}, ValueType{});
    }
    
    auto min_val = *container.begin();
    auto max_val = *container.begin();
    ValueType sum{};
    
    for (const auto& item : container) {
        if (item < min_val) min_val = item;
        if (item > max_val) max_val = item;
        sum += item;
    }
    
    auto average = sum / static_cast<ValueType>(container.size());
    return std::make_tuple(min_val, max_val, average);
}

// Generic sorting check
template<typename Container>
bool is_sorted(const Container& container) {
    auto it = container.begin();
    if (it == container.end()) return true;
    
    auto prev = it++;
    while (it != container.end()) {
        if (*prev > *it) return false;
        prev = it++;
    }
    return true;
}

int main() {
    // Same generic functions work with different containers
    std::vector<int> vec = {5, 2, 8, 1, 9, 3};
    std::list<double> lst = {3.14, 2.71, 1.41, 0.57};
    std::deque<int> deq = {10, 20, 30, 40, 50};
    
    print_container(vec, "Vector");
    print_container(lst, "List");
    print_container(deq, "Deque");
    
    // Calculate statistics for different containers
    auto [vec_min, vec_max, vec_avg] = calculate_statistics(vec);
    std::cout << "Vector stats - Min: " << vec_min 
              << ", Max: " << vec_max << ", Avg: " << vec_avg << std::endl;
    
    auto [lst_min, lst_max, lst_avg] = calculate_statistics(lst);
    std::cout << "List stats - Min: " << lst_min 
              << ", Max: " << lst_max << ", Avg: " << lst_avg << std::endl;
    
    // Check if containers are sorted
    std::cout << "Vector sorted? " << is_sorted(vec) << std::endl;
    std::cout << "Deque sorted? " << is_sorted(deq) << std::endl;
    
    // Works with string containers too
    std::vector<std::string> words = {"apple", "banana", "cherry"};
    print_container(words, "Words");
    std::cout << "Words sorted? " << is_sorted(words) << std::endl;
    
    return 0;
}
```

## Modern Template Features

### C++11 and Beyond

```cpp
#include <iostream>
#include <type_traits>
#include <utility>

// Variadic templates (C++11)
template<typename... Args>
void print_values(Args... args) {
    ((std::cout << args << " "), ...);  // C++17 fold expression
    std::cout << std::endl;
}

// Auto return type deduction (C++14)
template<typename T, typename U>
auto multiply(T a, U b) -> decltype(a * b) {
    return a * b;
}

// Perfect forwarding (C++11)
template<typename T>
void forward_to_function(T&& value) {
    some_function(std::forward<T>(value));
}

// Variable templates (C++14)
template<typename T>
constexpr T pi = T(3.1415926535897932385);

// Alias templates (C++11)
template<typename T>
using Vector = std::vector<T>;

template<typename Key, typename Value>
using Map = std::map<Key, Value>;

int main() {
    // Variadic templates
    print_values(1, 2.5, "hello", 'c');
    
    // Auto return type
    auto result = multiply(3, 4.5);
    std::cout << "Multiply result: " << result << std::endl;
    
    // Variable templates
    std::cout << "Pi as float: " << pi<float> << std::endl;
    std::cout << "Pi as double: " << pi<double> << std::endl;
    
    // Alias templates
    Vector<int> numbers = {1, 2, 3, 4, 5};
    Map<std::string, int> ages = {{"Alice", 30}, {"Bob", 25}};
    
    return 0;
}
```

## Getting Started with Templates

### Simple Template Patterns

```cpp
#include <iostream>
#include <string>

// 1. Function template for utilities
template<typename T>
void swap_values(T& a, T& b) {
    T temp = a;
    a = b;
    b = temp;
}

// 2. Class template for simple containers
template<typename T>
class Pair {
private:
    T first, second;
    
public:
    Pair(const T& a, const T& b) : first(a), second(b) {}
    
    T getFirst() const { return first; }
    T getSecond() const { return second; }
    
    void setFirst(const T& value) { first = value; }
    void setSecond(const T& value) { second = value; }
    
    void display() const {
        std::cout << "(" << first << ", " << second << ")" << std::endl;
    }
};

// 3. Template with default parameters
template<typename T = int>
class Counter {
private:
    T count;
    
public:
    Counter(T initial = T{}) : count(initial) {}
    
    void increment() { ++count; }
    void decrement() { --count; }
    T getValue() const { return count; }
};

int main() {
    // Function template usage
    int x = 10, y = 20;
    std::cout << "Before swap: x=" << x << ", y=" << y << std::endl;
    swap_values(x, y);
    std::cout << "After swap: x=" << x << ", y=" << y << std::endl;
    
    std::string s1 = "Hello", s2 = "World";
    swap_values(s1, s2);
    std::cout << "Swapped strings: " << s1 << ", " << s2 << std::endl;
    
    // Class template usage
    Pair<int> intPair(5, 10);
    Pair<std::string> stringPair("First", "Second");
    
    intPair.display();
    stringPair.display();
    
    // Template with defaults
    Counter<> defaultCounter;    // Uses int
    Counter<double> doubleCounter(3.14);
    
    defaultCounter.increment();
    doubleCounter.increment();
    
    std::cout << "Default counter: " << defaultCounter.getValue() << std::endl;
    std::cout << "Double counter: " << doubleCounter.getValue() << std::endl;
    
    return 0;
}
```

## What's Next

As we dive deeper into templates, we'll explore:

- **Function Templates**: Generic functions and type deduction
- **Class Templates**: Generic classes and member specialization  
- **Template Specialization**: Customizing behavior for specific types
- **Advanced Features**: SFINAE, concepts, metaprogramming
- **STL Integration**: How templates power the Standard Library
- **Best Practices**: Writing maintainable and efficient template code

Templates are one of C++'s most powerful features, enabling you to write highly reusable, efficient, and type-safe code. They're the foundation of the entire Standard Template Library and modern C++ programming practices.

---

**Sections in This Chapter:**

- [Function Templates // → ](function-templates.md)
- [Class Templates // → ](class-templates.md)
- [Template Specialization // → ](specialization.md)
- [Concepts // → ](concepts.md)
