# Function Templates

Function templates allow you to write generic functions that work with multiple types while maintaining type safety and performance. They are the foundation of generic programming in C++ and enable code reuse without sacrificing efficiency or type checking.

## Basic Function Template Syntax

### Simple Function Templates

```cpp
#include <iostream>
#include <string>

// Basic function template
template<typename T>
T maximum(T a, T b) {
    return (a > b) ? a : b;
}

// Function template with multiple parameters
template<typename T, typename U>
void print_pair(T first, U second) {
    std::cout << "First: " << first << ", Second: " << second << std::endl;
}

// Function template with return type different from parameters
template<typename T>
bool is_positive(T value) {
    return value > T{};  // T{} creates zero value of type T
}

int main() {
    // Automatic type deduction
    std::cout << "Max integers: " << maximum(10, 20) << std::endl;
    std::cout << "Max doubles: " << maximum(3.14, 2.71) << std::endl;
    std::cout << "Max strings: " << maximum(std::string("apple"), std::string("banana")) << std::endl;
    
    // Multiple type parameters
    print_pair(42, "hello");
    print_pair(3.14, 'c');
    print_pair(std::string("key"), 100);
    
    // Boolean return type
    std::cout << "Is 5 positive? " << is_positive(5) << std::endl;
    std::cout << "Is -3 positive? " << is_positive(-3) << std::endl;
    std::cout << "Is 0.0 positive? " << is_positive(0.0) << std::endl;
    
    return 0;
}
```

### Alternative Template Syntax

```cpp
#include <iostream>

// Using 'class' keyword (equivalent to 'typename')
template<class T>
T multiply(T a, T b) {
    return a * b;
}

// Multiple parameters with different keywords
template<class T, typename U>
auto add(T a, U b) -> decltype(a + b) {
    return a + b;
}

// C++20 abbreviated function template syntax
auto power(auto base, auto exponent) {
    auto result = base;
    for (auto i = 1; i < exponent; ++i) {
        result *= base;
    }
    return result;
}

int main() {
    std::cout << "Multiply: " << multiply(5, 6) << std::endl;
    std::cout << "Add different types: " << add(10, 3.5) << std::endl;
    std::cout << "Power: " << power(2, 8) << std::endl;
    std::cout << "Power with doubles: " << power(1.5, 3) << std::endl;
    
    return 0;
}
```

## Type Deduction

### Automatic Type Deduction

```cpp
#include <iostream>
#include <vector>
#include <string>

template<typename T>
void analyze_type(T value) {
    std::cout << "Value: " << value << std::endl;
    std::cout << "Type: " << typeid(T).name() << std::endl;
    std::cout << "Size: " << sizeof(T) << " bytes" << std::endl;
    std::cout << std::endl;
}

template<typename T>
void process_container(const T& container) {
    std::cout << "Container size: " << container.size() << std::endl;
    std::cout << "Elements: ";
    for (const auto& element : container) {
        std::cout << element << " ";
    }
    std::cout << std::endl;
}

// Template function that deduces element type from container
template<typename Container>
auto sum_elements(const Container& container) -> typename Container::value_type {
    typename Container::value_type sum{};
    for (const auto& element : container) {
        sum += element;
    }
    return sum;
}

int main() {
    // Type deduction from literals
    analyze_type(42);           // int
    analyze_type(3.14);         // double
    analyze_type('c');          // char
    analyze_type("hello");      // const char*
    
    // Type deduction from variables
    std::string text = "world";
    analyze_type(text);         // std::string
    
    // Container type deduction
    std::vector<int> numbers = {1, 2, 3, 4, 5};
    std::vector<std::string> words = {"hello", "world", "template"};
    
    process_container(numbers);
    process_container(words);
    
    // Element type deduction
    auto sum = sum_elements(numbers);
    std::cout << "Sum of numbers: " << sum << std::endl;
    
    return 0;
}
```

### Reference and Pointer Deduction

```cpp
#include <iostream>

template<typename T>
void by_value(T param) {
    std::cout << "by_value - Type: " << typeid(T).name() << std::endl;
    param = T{};  // Can modify local copy
}

template<typename T>
void by_reference(T& param) {
    std::cout << "by_reference - Type: " << typeid(T).name() << std::endl;
    // param = T{};  // Would modify original
}

template<typename T>
void by_const_reference(const T& param) {
    std::cout << "by_const_reference - Type: " << typeid(T).name() << std::endl;
    // param = T{};  // Cannot modify
}

template<typename T>
void by_universal_reference(T&& param) {
    std::cout << "by_universal_reference - Type: " << typeid(T).name() << std::endl;
    // Perfect forwarding context
}

template<typename T>
void by_pointer(T* param) {
    std::cout << "by_pointer - Type: " << typeid(T).name() << std::endl;
    if (param) {
        std::cout << "Value: " << *param << std::endl;
    }
}

int main() {
    int x = 42;
    const int cx = x;
    int& rx = x;
    
    std::cout << "=== Value Parameter ===" << std::endl;
    by_value(x);    // T deduced as int
    by_value(cx);   // T deduced as int (const stripped)
    by_value(rx);   // T deduced as int (reference stripped)
    
    std::cout << "\n=== Reference Parameter ===" << std::endl;
    by_reference(x);    // T deduced as int
    by_reference(rx);   // T deduced as int
    // by_reference(cx);   // Error: cannot bind non-const ref to const
    
    std::cout << "\n=== Const Reference Parameter ===" << std::endl;
    by_const_reference(x);     // T deduced as int
    by_const_reference(cx);    // T deduced as int
    by_const_reference(rx);    // T deduced as int
    by_const_reference(42);    // T deduced as int (can bind to temporary)
    
    std::cout << "\n=== Universal Reference Parameter ===" << std::endl;
    by_universal_reference(x);     // T deduced as int&
    by_universal_reference(cx);    // T deduced as const int&
    by_universal_reference(42);    // T deduced as int
    
    std::cout << "\n=== Pointer Parameter ===" << std::endl;
    by_pointer(&x);     // T deduced as int
    by_pointer(&cx);    // T deduced as const int
    
    return 0;
}
```

## Explicit Template Instantiation

### Manual Type Specification

```cpp
#include <iostream>
#include <string>

template<typename T>
T convert_and_process(const std::string& str) {
    if constexpr (std::is_same_v<T, int>) {
        return std::stoi(str);
    }
    else if constexpr (std::is_same_v<T, double>) {
        return std::stod(str);
    }
    else if constexpr (std::is_same_v<T, float>) {
        return std::stof(str);
    }
    else {
        // Default: return default-constructed value
        return T{};
    }
}

template<typename T, typename U>
auto safe_divide(T numerator, U denominator) -> decltype(numerator / denominator) {
    if (denominator != U{}) {
        return numerator / denominator;
    }
    return T{};
}

// Template with non-type parameters
template<typename T, size_t N>
void print_array(const T (&arr)[N]) {
    std::cout << "Array of " << N << " elements: ";
    for (size_t i = 0; i < N; ++i) {
        std::cout << arr[i] << " ";
    }
    std::cout << std::endl;
}

int main() {
    // Explicit template instantiation
    std::string number_str = "123";
    std::string decimal_str = "45.67";
    
    int int_value = convert_and_process<int>(number_str);
    double double_value = convert_and_process<double>(decimal_str);
    float float_value = convert_and_process<float>(decimal_str);
    
    std::cout << "Converted int: " << int_value << std::endl;
    std::cout << "Converted double: " << double_value << std::endl;
    std::cout << "Converted float: " << float_value << std::endl;
    
    // Explicit instantiation when types can't be deduced
    auto result1 = safe_divide<double, int>(10.0, 3);
    auto result2 = safe_divide<int, double>(20, 4.0);
    
    std::cout << "Division results: " << result1 << ", " << result2 << std::endl;
    
    // Array template with non-type parameter
    int numbers[] = {1, 2, 3, 4, 5};
    double decimals[] = {1.1, 2.2, 3.3};
    
    print_array(numbers);   // N deduced as 5
    print_array(decimals);  // N deduced as 3
    
    return 0;
}
```

## Function Template Overloading

### Multiple Template Overloads

```cpp
#include <iostream>
#include <vector>
#include <string>

// Overload 1: Single value
template<typename T>
void print(const T& value) {
    std::cout << "Single value: " << value << std::endl;
}

// Overload 2: Two values
template<typename T, typename U>
void print(const T& first, const U& second) {
    std::cout << "Two values: " << first << ", " << second << std::endl;
}

// Overload 3: Container
template<typename Container>
void print(const Container& container) {
    std::cout << "Container: ";
    for (const auto& element : container) {
        std::cout << element << " ";
    }
    std::cout << std::endl;
}

// Overload 4: Array
template<typename T, size_t N>
void print(const T (&array)[N]) {
    std::cout << "Array of " << N << ": ";
    for (size_t i = 0; i < N; ++i) {
        std::cout << array[i] << " ";
    }
    std::cout << std::endl;
}

// Specialized behavior for specific types
template<typename T>
T absolute_value(T value) {
    return (value < T{}) ? -value : value;
}

// Specialization for unsigned types (no need to check sign)
template<>
unsigned int absolute_value<unsigned int>(unsigned int value) {
    std::cout << "(unsigned - no conversion needed) ";
    return value;
}

int main() {
    // Function overload resolution
    print(42);                              // Single value
    print("hello", 123);                    // Two values
    print(std::vector<int>{1, 2, 3, 4});   // Container
    
    int array[] = {10, 20, 30};
    print(array);                           // Array
    
    // Template specialization
    std::cout << "abs(-5): " << absolute_value(-5) << std::endl;
    std::cout << "abs(5u): " << absolute_value(5u) << std::endl;
    std::cout << "abs(-3.14): " << absolute_value(-3.14) << std::endl;
    
    return 0;
}
```

### Template vs Non-Template Overloads

```cpp
#include <iostream>
#include <string>

// Non-template function
void process(int value) {
    std::cout << "Non-template process(int): " << value << std::endl;
}

// Template function
template<typename T>
void process(T value) {
    std::cout << "Template process<T>: " << value << std::endl;
}

// Specific template instantiation
template<>
void process<double>(double value) {
    std::cout << "Specialized process<double>: " << value * 2 << std::endl;
}

// More specific template
template<typename T>
void process(T* pointer) {
    std::cout << "Template process<T*>: " << *pointer << std::endl;
}

void demonstrate_overload_resolution() {
    std::cout << "=== Overload Resolution Priority ===" << std::endl;
    
    process(42);        // Exact match: non-template preferred
    process(3.14);      // Template specialization
    process('c');       // Template instantiation
    process("hello");   // Template instantiation
    
    int x = 100;
    process(&x);        // More specific template (T*)
    
    // Force template instantiation
    process<int>(42);   // Explicitly call template version
}

int main() {
    demonstrate_overload_resolution();
    return 0;
}
```

## Advanced Function Templates

### SFINAE (Substitution Failure Is Not An Error)

```cpp
#include <iostream>
#include <type_traits>
#include <vector>
#include <string>

// SFINAE with std::enable_if
template<typename T>
typename std::enable_if<std::is_arithmetic<T>::value, void>::type
print_value(T value) {
    std::cout << "Arithmetic value: " << value << std::endl;
}

template<typename T>
typename std::enable_if<!std::is_arithmetic<T>::value, void>::type
print_value(T value) {
    std::cout << "Non-arithmetic value: " << value << std::endl;
}

// SFINAE with return type
template<typename Container>
auto get_size(const Container& container) -> decltype(container.size()) {
    return container.size();
}

// Alternative for types without size() method
template<typename T>
auto get_size(const T& value) -> typename std::enable_if<!std::is_class<T>::value, size_t>::type {
    return sizeof(T);
}

// Modern C++17 approach with if constexpr
template<typename T>
void modern_print(T value) {
    if constexpr (std::is_arithmetic_v<T>) {
        std::cout << "Number: " << value << " (size: " << sizeof(T) << " bytes)" << std::endl;
    }
    else if constexpr (std::is_same_v<T, std::string>) {
        std::cout << "String: \"" << value << "\" (length: " << value.length() << ")" << std::endl;
    }
    else {
        std::cout << "Other type: " << value << std::endl;
    }
}

// SFINAE for method detection
template<typename T>
class has_size_method {
private:
    template<typename U>
    static auto test(int) -> decltype(std::declval<U>().size(), std::true_type{});
    
    template<typename>
    static std::false_type test(...);
    
public:
    static constexpr bool value = decltype(test<T>(0))::value;
};

template<typename T>
void conditional_size_print(const T& obj) {
    if constexpr (has_size_method<T>::value) {
        std::cout << "Object has size: " << obj.size() << std::endl;
    } else {
        std::cout << "Object doesn't have size method" << std::endl;
    }
}

int main() {
    // SFINAE examples
    print_value(42);           // Arithmetic
    print_value(3.14);         // Arithmetic  
    print_value(std::string("hello"));  // Non-arithmetic
    
    // Size detection
    std::vector<int> vec = {1, 2, 3};
    std::string str = "hello";
    int number = 42;
    
    std::cout << "Vector size: " << get_size(vec) << std::endl;
    std::cout << "String size: " << get_size(str) << std::endl;
    std::cout << "Int size: " << get_size(number) << std::endl;
    
    // Modern approach
    modern_print(100);
    modern_print(2.5);
    modern_print(std::string("modern"));
    
    // Method detection
    conditional_size_print(vec);
    conditional_size_print(str);
    conditional_size_print(42);
    
    return 0;
}
```

### Perfect Forwarding

```cpp
#include <iostream>
#include <utility>
#include <string>

// Function that we want to forward arguments to
void target_function(int& lvalue_ref) {
    std::cout << "Called with lvalue reference: " << lvalue_ref << std::endl;
    lvalue_ref += 10;
}

void target_function(const int& const_lvalue_ref) {
    std::cout << "Called with const lvalue reference: " << const_lvalue_ref << std::endl;
}

void target_function(int&& rvalue_ref) {
    std::cout << "Called with rvalue reference: " << rvalue_ref << std::endl;
}

// Perfect forwarding wrapper
template<typename T>
void perfect_forwarder(T&& arg) {
    std::cout << "Forwarding argument..." << std::endl;
    target_function(std::forward<T>(arg));
}

// Factory function with perfect forwarding
template<typename T, typename... Args>
T create_object(Args&&... args) {
    std::cout << "Creating object with " << sizeof...(args) << " arguments" << std::endl;
    return T(std::forward<Args>(args)...);
}

class ComplexObject {
private:
    std::string data;
    int value;
    
public:
    ComplexObject(const std::string& str, int val) : data(str), value(val) {
        std::cout << "ComplexObject constructed with: " << data << ", " << value << std::endl;
    }
    
    ComplexObject(std::string&& str, int val) : data(std::move(str)), value(val) {
        std::cout << "ComplexObject constructed with moved string and: " << value << std::endl;
    }
    
    void display() const {
        std::cout << "ComplexObject: " << data << ", " << value << std::endl;
    }
};

int main() {
    std::cout << "=== Perfect Forwarding ===" << std::endl;
    
    int x = 5;
    const int cx = 10;
    
    perfect_forwarder(x);       // lvalue
    perfect_forwarder(cx);      // const lvalue
    perfect_forwarder(15);      // rvalue
    perfect_forwarder(std::move(x));  // rvalue (x moved)
    
    std::cout << "x after move: " << x << std::endl;
    
    std::cout << "\n=== Factory with Perfect Forwarding ===" << std::endl;
    
    // Create objects with different argument types
    std::string str = "temporary";
    auto obj1 = create_object<ComplexObject>(str, 42);              // lvalue string
    auto obj2 = create_object<ComplexObject>(std::move(str), 100);  // rvalue string
    auto obj3 = create_object<ComplexObject>("literal", 200);       // string literal
    
    obj1.display();
    obj2.display();
    obj3.display();
    
    return 0;
}
```

## Variadic Function Templates

### Basic Variadic Templates

```cpp
#include <iostream>
#include <string>

// Recursive variadic template (C++11 style)
template<typename T>
void print_recursive(T&& value) {
    std::cout << value << std::endl;
}

template<typename T, typename... Args>
void print_recursive(T&& first, Args&&... rest) {
    std::cout << first << " ";
    print_recursive(rest...);
}

// C++17 fold expression style
template<typename... Args>
void print_fold(Args&&... args) {
    ((std::cout << args << " "), ...);
    std::cout << std::endl;
}

// Variadic template with type information
template<typename... Args>
void print_types(Args&&... args) {
    std::cout << "Function called with " << sizeof...(args) << " arguments:" << std::endl;
    ((std::cout << "  " << typeid(args).name() << ": " << args << std::endl), ...);
}

// Sum function using fold expression
template<typename... Args>
auto sum(Args... args) {
    return (args + ...);
}

// Variadic template for creating tuples
template<typename... Args>
auto make_tuple_and_print(Args&&... args) {
    auto tuple = std::make_tuple(std::forward<Args>(args)...);
    print_fold("Created tuple with:", args...);
    return tuple;
}

int main() {
    std::cout << "=== Recursive Variadic Template ===" << std::endl;
    print_recursive(1, 2.5, "hello", 'c', true);
    
    std::cout << "\n=== Fold Expression ===" << std::endl;
    print_fold(10, 20, "world", 3.14);
    
    std::cout << "\n=== Type Information ===" << std::endl;
    print_types(42, std::string("text"), 3.14f, 'x');
    
    std::cout << "\n=== Sum Function ===" << std::endl;
    std::cout << "Sum of integers: " << sum(1, 2, 3, 4, 5) << std::endl;
    std::cout << "Sum of doubles: " << sum(1.1, 2.2, 3.3) << std::endl;
    
    std::cout << "\n=== Tuple Creation ===" << std::endl;
    auto my_tuple = make_tuple_and_print(100, "tuple", 2.71);
    
    return 0;
}
```

### Advanced Variadic Examples

```cpp
#include <iostream>
#include <vector>
#include <memory>
#include <functional>

// Variadic template for function composition
template<typename F>
auto compose(F&& f) {
    return std::forward<F>(f);
}

template<typename F, typename... Fs>
auto compose(F&& f, Fs&&... fs) {
    return [=](auto&&... args) {
        return f(compose(fs...)(std::forward<decltype(args)>(args)...));
    };
}

// Variadic template for applying function to multiple arguments
template<typename F, typename... Args>
void apply_to_each(F&& func, Args&&... args) {
    (func(std::forward<Args>(args)), ...);
}

// Variadic template for creating and initializing container
template<typename Container, typename... Args>
Container make_container(Args&&... args) {
    Container container;
    (container.push_back(std::forward<Args>(args)), ...);
    return container;
}

// Variadic template for visitor pattern
template<typename... Visitors>
struct visitor : Visitors... {
    using Visitors::operator()...;
};

// Deduction guide for C++17
template<typename... Visitors>
visitor(Visitors...) -> visitor<Visitors...>;

int main() {
    std::cout << "=== Function Composition ===" << std::endl;
    auto add_one = [](int x) { return x + 1; };
    auto multiply_two = [](int x) { return x * 2; };
    auto square = [](int x) { return x * x; };
    
    auto composed = compose(square, multiply_two, add_one);
    std::cout << "Composed function result: " << composed(5) << std::endl;  // ((5+1)*2)^2 = 144
    
    std::cout << "\n=== Apply to Each ===" << std::endl;
    auto print_with_type = [](auto&& value) {
        std::cout << typeid(value).name() << ": " << value << std::endl;
    };
    
    apply_to_each(print_with_type, 42, 3.14, "hello", 'c');
    
    std::cout << "\n=== Container Creation ===" << std::endl;
    auto int_vector = make_container<std::vector<int>>(1, 2, 3, 4, 5);
    auto string_vector = make_container<std::vector<std::string>>("a", "b", "c");
    
    std::cout << "Int vector: ";
    for (const auto& item : int_vector) std::cout << item << " ";
    std::cout << std::endl;
    
    std::cout << "String vector: ";
    for (const auto& item : string_vector) std::cout << item << " ";
    std::cout << std::endl;
    
    std::cout << "\n=== Visitor Pattern ===" << std::endl;
    auto multi_visitor = visitor {
        [](int i) { std::cout << "Integer: " << i << std::endl; },
        [](double d) { std::cout << "Double: " << d << std::endl; },
        [](const std::string& s) { std::cout << "String: " << s << std::endl; }
    };
    
    apply_to_each(multi_visitor, 42, 3.14, std::string("hello"));
    
    return 0;
}
```

## Practical Examples

### Generic Algorithms

```cpp
#include <iostream>
#include <vector>
#include <list>
#include <algorithm>
#include <functional>

// Generic binary search
template<typename Iterator, typename T>
bool binary_search_generic(Iterator begin, Iterator end, const T& value) {
    Iterator left = begin;
    Iterator right = end;
    
    while (left < right) {
        Iterator mid = left + std::distance(left, right) / 2;
        
        if (*mid == value) {
            return true;
        } else if (*mid < value) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    
    return false;
}

// Generic quicksort
template<typename Iterator>
void quicksort(Iterator begin, Iterator end) {
    if (std::distance(begin, end) <= 1) return;
    
    auto pivot = std::partition(begin, end - 1, 
        [pivot_val = *(end - 1)](const auto& element) {
            return element < pivot_val;
        });
    
    std::swap(*pivot, *(end - 1));
    quicksort(begin, pivot);
    quicksort(pivot + 1, end);
}

// Generic filter function
template<typename Container, typename Predicate>
auto filter(const Container& container, Predicate pred) {
    Container result;
    for (const auto& item : container) {
        if (pred(item)) {
            result.push_back(item);
        }
    }
    return result;
}

// Generic map function
template<typename Container, typename Function>
auto map(const Container& container, Function func) {
    using InputType = typename Container::value_type;
    using OutputType = decltype(func(std::declval<InputType>()));
    
    std::vector<OutputType> result;
    result.reserve(container.size());
    
    for (const auto& item : container) {
        result.push_back(func(item));
    }
    
    return result;
}

// Generic reduce function
template<typename Container, typename T, typename BinaryOp>
T reduce(const Container& container, T initial, BinaryOp op) {
    T result = initial;
    for (const auto& item : container) {
        result = op(result, item);
    }
    return result;
}

int main() {
    std::vector<int> numbers = {1, 3, 5, 7, 9, 11, 13, 15, 17, 19};
    
    std::cout << "=== Binary Search ===" << std::endl;
    std::cout << "Found 7: " << binary_search_generic(numbers.begin(), numbers.end(), 7) << std::endl;
    std::cout << "Found 8: " << binary_search_generic(numbers.begin(), numbers.end(), 8) << std::endl;
    
    std::cout << "\n=== Quicksort ===" << std::endl;
    std::vector<int> unsorted = {64, 34, 25, 12, 22, 11, 90, 5};
    std::cout << "Before sort: ";
    for (int n : unsorted) std::cout << n << " ";
    std::cout << std::endl;
    
    quicksort(unsorted.begin(), unsorted.end());
    std::cout << "After sort: ";
    for (int n : unsorted) std::cout << n << " ";
    std::cout << std::endl;
    
    std::cout << "\n=== Filter, Map, Reduce ===" << std::endl;
    
    // Filter even numbers
    auto evens = filter(numbers, [](int x) { return x % 2 == 0; });
    std::cout << "Even numbers: ";
    for (int n : evens) std::cout << n << " ";
    std::cout << std::endl;
    
    // Map to squares
    auto squares = map(numbers, [](int x) { return x * x; });
    std::cout << "Squares: ";
    for (int n : squares) std::cout << n << " ";
    std::cout << std::endl;
    
    // Reduce to sum
    int sum = reduce(numbers, 0, [](int a, int b) { return a + b; });
    std::cout << "Sum: " << sum << std::endl;
    
    // Chain operations
    auto result = reduce(
        map(
            filter(numbers, [](int x) { return x > 10; }),
            [](int x) { return x * 2; }
        ),
        0,
        std::plus<int>{}
    );
    
    std::cout << "Chained result (filter>10, map*2, sum): " << result << std::endl;
    
    return 0;
}
```

### Template Utilities

```cpp
#include <iostream>
#include <type_traits>
#include <utility>
#include <functional>
#include <chrono>

// Timer utility template
template<typename TimeUnit = std::chrono::milliseconds>
class Timer {
private:
    std::chrono::high_resolution_clock::time_point start_time;
    
public:
    Timer() : start_time(std::chrono::high_resolution_clock::now()) {}
    
    auto elapsed() const {
        auto end_time = std::chrono::high_resolution_clock::now();
        return std::chrono::duration_cast<TimeUnit>(end_time - start_time).count();
    }
    
    void reset() {
        start_time = std::chrono::high_resolution_clock::now();
    }
};

// Benchmark template function
template<typename Func, typename... Args>
auto benchmark(Func&& func, Args&&... args) {
    Timer<std::chrono::microseconds> timer;
    
    if constexpr (std::is_void_v<std::invoke_result_t<Func, Args...>>) {
        std::invoke(std::forward<Func>(func), std::forward<Args>(args)...);
        return std::make_pair(timer.elapsed(), 0);  // No return value
    } else {
        auto result = std::invoke(std::forward<Func>(func), std::forward<Args>(args)...);
        return std::make_pair(timer.elapsed(), result);
    }
}

// Memoization template
template<typename Func>
class Memoized {
private:
    mutable std::map<std::string, std::any> cache;
    Func func;
    
public:
    Memoized(Func f) : func(f) {}
    
    template<typename... Args>
    auto operator()(Args&&... args) const {
        // Simple string-based key (in practice, you'd want a better hash)
        std::string key = std::to_string(sizeof...(args));
        ((key += std::to_string(args)), ...);
        
        if (auto it = cache.find(key); it != cache.end()) {
            return std::any_cast<std::invoke_result_t<Func, Args...>>(it->second);
        }
        
        auto result = func(std::forward<Args>(args)...);
        cache[key] = result;
        return result;
    }
};

// Make memoized helper
template<typename Func>
auto make_memoized(Func&& func) {
    return Memoized<std::decay_t<Func>>(std::forward<Func>(func));
}

// Optional-like template
template<typename T>
class Optional {
private:
    alignas(T) char storage[sizeof(T)];
    bool has_value_flag;
    
public:
    Optional() : has_value_flag(false) {}
    
    Optional(const T& value) : has_value_flag(true) {
        new(storage) T(value);
    }
    
    Optional(T&& value) : has_value_flag(true) {
        new(storage) T(std::move(value));
    }
    
    ~Optional() {
        if (has_value_flag) {
            reinterpret_cast<T*>(storage)->~T();
        }
    }
    
    bool has_value() const { return has_value_flag; }
    
    T& value() {
        if (!has_value_flag) throw std::runtime_error("No value");
        return *reinterpret_cast<T*>(storage);
    }
    
    const T& value() const {
        if (!has_value_flag) throw std::runtime_error("No value");
        return *reinterpret_cast<const T*>(storage);
    }
    
    T value_or(const T& default_value) const {
        return has_value_flag ? value() : default_value;
    }
};

// Test functions
int fibonacci(int n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
}

void expensive_operation() {
    std::this_thread::sleep_for(std::chrono::milliseconds(100));
}

int main() {
    std::cout << "=== Timer Utility ===" << std::endl;
    Timer<std::chrono::milliseconds> ms_timer;
    Timer<std::chrono::microseconds> us_timer;
    
    expensive_operation();
    
    std::cout << "Elapsed time: " << ms_timer.elapsed() << " ms" << std::endl;
    std::cout << "Elapsed time: " << us_timer.elapsed() << " μs" << std::endl;
    
    std::cout << "\n=== Benchmark Function ===" << std::endl;
    auto [time1, result1] = benchmark(fibonacci, 30);
    std::cout << "Fibonacci(30) = " << result1 << ", Time: " << time1 << " μs" << std::endl;
    
    auto [time2, _] = benchmark(expensive_operation);
    std::cout << "Expensive operation time: " << time2 << " μs" << std::endl;
    
    std::cout << "\n=== Memoization ===" << std::endl;
    auto memoized_fib = make_memoized([](int n) -> int {
        std::cout << "Computing fib(" << n << ")" << std::endl;
        if (n <= 1) return n;
        return fibonacci(n - 1) + fibonacci(n - 2);
    });
    
    std::cout << "First call: " << memoized_fib(10) << std::endl;
    std::cout << "Second call: " << memoized_fib(10) << std::endl;  // Should use cache
    
    std::cout << "\n=== Optional Template ===" << std::endl;
    Optional<int> opt1(42);
    Optional<int> opt2;
    
    std::cout << "opt1 has value: " << opt1.has_value() << ", value: " << opt1.value() << std::endl;
    std::cout << "opt2 has value: " << opt2.has_value() << ", value_or(0): " << opt2.value_or(0) << std::endl;
    
    return 0;
}
```

## Best Practices

### Template Design Guidelines

```cpp
#include <iostream>
#include <type_traits>
#include <concepts>

// → Good: Clear naming and documentation
template<typename InputIterator, typename OutputIterator, typename UnaryFunction>
OutputIterator transform_copy(InputIterator first, InputIterator last, 
                             OutputIterator result, UnaryFunction func) {
    while (first != last) {
        *result = func(*first);
        ++first;
        ++result;
    }
    return result;
}

// → Good: Use concepts for requirements (C++20)
template<typename T>
concept Comparable = requires(T a, T b) {
    { a < b } -> std::convertible_to<bool>;
    { a > b } -> std::convertible_to<bool>;
    { a == b } -> std::convertible_to<bool>;
};

template<Comparable T>
T clamp(T value, T min_val, T max_val) {
    if (value < min_val) return min_val;
    if (value > max_val) return max_val;
    return value;
}

// → Good: SFINAE for backwards compatibility
template<typename T>
std::enable_if_t<std::is_arithmetic_v<T>, T>
safe_clamp(T value, T min_val, T max_val) {
    return clamp(value, min_val, max_val);
}

// → Good: Default template parameters
template<typename T, typename Compare = std::less<T>>
bool is_sorted_with_comparator(const std::vector<T>& vec, Compare comp = Compare{}) {
    for (size_t i = 1; i < vec.size(); ++i) {
        if (comp(vec[i], vec[i-1])) {
            return false;
        }
    }
    return true;
}

// ✘ Avoid: Overly complex template metaprogramming
template<bool B, typename T, typename F>
using conditional_t = typename std::conditional<B, T, F>::type;

template<typename T>
using complex_type = conditional_t<
    std::is_arithmetic_v<T>,
    conditional_t<std::is_integral_v<T>, long long, long double>,
    std::string
>;

// → Better: Simple, readable alternatives
template<typename T>
auto make_wider_type(T value) {
    if constexpr (std::is_integral_v<T>) {
        return static_cast<long long>(value);
    } else if constexpr (std::is_floating_point_v<T>) {
        return static_cast<long double>(value);
    } else {
        return std::to_string(value);
    }
}

int main() {
    std::cout << "=== Template Best Practices ===" << std::endl;
    
    // Clear, well-named templates
    std::vector<int> source = {1, 2, 3, 4, 5};
    std::vector<int> destination(source.size());
    
    transform_copy(source.begin(), source.end(), destination.begin(), 
                  [](int x) { return x * x; });
    
    std::cout << "Transformed: ";
    for (int n : destination) std::cout << n << " ";
    std::cout << std::endl;
    
    // Concepts usage
    std::cout << "Clamped value: " << clamp(15, 10, 20) << std::endl;
    std::cout << "Clamped value: " << clamp(5, 10, 20) << std::endl;
    
    // Default template parameters
    std::vector<int> sorted_vec = {1, 2, 3, 4, 5};
    std::vector<int> reverse_sorted = {5, 4, 3, 2, 1};
    
    std::cout << "Is sorted (ascending): " << is_sorted_with_comparator(sorted_vec) << std::endl;
    std::cout << "Is sorted (descending): " << is_sorted_with_comparator(reverse_sorted, std::greater<int>{}) << std::endl;
    
    // Simple alternatives to complex metaprogramming
    auto wider_int = make_wider_type(42);
    auto wider_double = make_wider_type(3.14);
    auto wider_string = make_wider_type(std::string("hello"));
    
    std::cout << "Wider types: " << wider_int << ", " << wider_double << ", " << wider_string << std::endl;
    
    return 0;
}
```

## Exercises

### Exercise 1: Generic Container Operations

Create a set of function templates for container operations:

- `find_if()` - find first element matching predicate
- `count_if()` - count elements matching predicate  
- `partition()` - partition container based on predicate
- `unique()` - remove consecutive duplicates

### Exercise 2: Mathematical Function Templates

Build mathematical function templates:

- `gcd()` and `lcm()` for greatest common divisor and least common multiple
- `power()` with integer and floating-point exponents
- `factorial()` and `fibonacci()` with memoization
- `integrate()` for numerical integration

### Exercise 3: String Processing Templates

Create generic string processing functions:

- `split()` - split string by delimiter
- `join()` - join container elements with separator
- `replace_all()` - replace all occurrences of substring
- `trim()` - remove whitespace from both ends

### Exercise 4: Advanced Template Utilities

Design advanced template utilities:

- `any_of()`, `all_of()`, `none_of()` predicates
- `zip()` - combine multiple containers
- `enumerate()` - iterate with index
- `cached_function()` - automatic memoization wrapper

## Key Takeaways

<i class="fa-solid fa-arrow-right"></i> **Function templates enable generic programming** while maintaining type safety  
<i class="fa-solid fa-arrow-right"></i> **Type deduction works automatically** in most cases, but can be explicitly specified  
<i class="fa-solid fa-arrow-right"></i> **Template overloading follows specific resolution rules** - exact matches preferred  
<i class="fa-solid fa-arrow-right"></i> **SFINAE enables conditional compilation** based on type properties  
<i class="fa-solid fa-arrow-right"></i> **Perfect forwarding preserves value categories** in wrapper functions  
<i class="fa-solid fa-arrow-right"></i> **Variadic templates handle variable argument counts** efficiently  
<i class="fa-solid fa-arrow-right"></i> **Use concepts (C++20) or SFINAE** to constrain template parameters  
<i class="fa-solid fa-arrow-right"></i> **Keep templates simple and readable** - avoid over-engineering

---

**Next**: Learn about creating generic classes and data structures // →  [Class Templates](class-templates.md)
