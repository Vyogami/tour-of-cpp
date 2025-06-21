# Class Templates

Class templates allow you to create generic classes that can work with different types while maintaining type safety and performance. They are essential for building reusable data structures, containers, and utility classes that form the backbone of modern C++ programming.

## Basic Class Template Syntax

### Simple Class Templates

```cpp
#include <iostream>
#include <string>
#include <stdexcept>

// Basic class template
template<typename T>
class Container {
private:
    T* data;
    size_t capacity;
    size_t size;
    
public:
    Container(size_t initial_capacity = 10) : capacity(initial_capacity), size(0) {
        data = new T[capacity];
        std::cout << "Container created with capacity " << capacity << std::endl;
    }
    
    ~Container() {
        delete[] data;
        std::cout << "Container destroyed" << std::endl;
    }
    
    void add(const T& item) {
        if (size >= capacity) {
            throw std::runtime_error("Container is full");
        }
        data[size++] = item;
    }
    
    T get(size_t index) const {
        if (index >= size) {
            throw std::out_of_range("Index out of range");
        }
        return data[index];
    }
    
    size_t getSize() const { return size; }
    size_t getCapacity() const { return capacity; }
    
    void display() const {
        std::cout << "Container contents: ";
        for (size_t i = 0; i < size; ++i) {
            std::cout << data[i] << " ";
        }
        std::cout << "(size: " << size << "/" << capacity << ")" << std::endl;
    }
};

// Class template with multiple type parameters
template<typename Key, typename Value>
class KeyValuePair {
private:
    Key key;
    Value value;
    
public:
    KeyValuePair(const Key& k, const Value& v) : key(k), value(v) {}
    
    Key getKey() const { return key; }
    Value getValue() const { return value; }
    
    void setKey(const Key& k) { key = k; }
    void setValue(const Value& v) { value = v; }
    
    void display() const {
        std::cout << "Key: " << key << ", Value: " << value << std::endl;
    }
};

int main() {
    // Instantiate templates with different types
    Container<int> intContainer(5);
    Container<std::string> stringContainer(3);
    
    // Use int container
    intContainer.add(10);
    intContainer.add(20);
    intContainer.add(30);
    intContainer.display();
    
    // Use string container
    stringContainer.add("Hello");
    stringContainer.add("World");
    stringContainer.display();
    
    // Key-value pairs
    KeyValuePair<std::string, int> nameAge("Alice", 30);
    KeyValuePair<int, double> idScore(101, 95.5);
    
    nameAge.display();
    idScore.display();
    
    return 0;
}
```

### Class Template with Non-Type Parameters

```cpp
#include <iostream>
#include <array>

// Template with type and non-type parameters
template<typename T, size_t Size>
class FixedArray {
private:
    T data[Size];
    size_t current_size;
    
public:
    FixedArray() : current_size(0) {
        std::cout << "FixedArray<" << typeid(T).name() 
                  << ", " << Size << "> created" << std::endl;
    }
    
    void add(const T& item) {
        if (current_size >= Size) {
            throw std::runtime_error("Array is full");
        }
        data[current_size++] = item;
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
    
    constexpr size_t capacity() const { return Size; }
    size_t size() const { return current_size; }
    bool empty() const { return current_size == 0; }
    bool full() const { return current_size >= Size; }
    
    // Iterator support
    T* begin() { return data; }
    T* end() { return data + current_size; }
    const T* begin() const { return data; }
    const T* end() const { return data + current_size; }
    
    void display() const {
        std::cout << "FixedArray contents: ";
        for (size_t i = 0; i < current_size; ++i) {
            std::cout << data[i] << " ";
        }
        std::cout << "(size: " << current_size << "/" << Size << ")" << std::endl;
    }
};

// Template with default non-type parameter
template<typename T, size_t BufferSize = 1024>
class Buffer {
private:
    T buffer[BufferSize];
    size_t head, tail;
    
public:
    Buffer() : head(0), tail(0) {}
    
    bool enqueue(const T& item) {
        size_t next_tail = (tail + 1) % BufferSize;
        if (next_tail == head) {
            return false; // Buffer full
        }
        buffer[tail] = item;
        tail = next_tail;
        return true;
    }
    
    bool dequeue(T& item) {
        if (head == tail) {
            return false; // Buffer empty
        }
        item = buffer[head];
        head = (head + 1) % BufferSize;
        return true;
    }
    
    bool empty() const { return head == tail; }
    bool full() const { return (tail + 1) % BufferSize == head; }
    
    constexpr size_t getBufferSize() const { return BufferSize; }
    
    void status() const {
        std::cout << "Buffer status - Head: " << head 
                  << ", Tail: " << tail 
                  << ", Size: " << BufferSize 
                  << ", Empty: " << empty() 
                  << ", Full: " << full() << std::endl;
    }
};

int main() {
    // Fixed size arrays
    FixedArray<int, 5> intArray;
    FixedArray<std::string, 3> stringArray;
    
    // Fill arrays
    for (int i = 1; i <= 4; ++i) {
        intArray.add(i * 10);
    }
    
    stringArray.add("First");
    stringArray.add("Second");
    
    intArray.display();
    stringArray.display();
    
    // Range-based for loop
    std::cout << "Using range-based for: ";
    for (const auto& value : intArray) {
        std::cout << value << " ";
    }
    std::cout << std::endl;
    
    // Circular buffer
    Buffer<int, 4> buffer;      // Custom size
    Buffer<char> charBuffer;    // Default size (1024)
    
    buffer.status();
    
    // Fill buffer
    for (int i = 1; i <= 3; ++i) {
        buffer.enqueue(i * 100);
        buffer.status();
    }
    
    // Dequeue items
    int item;
    while (buffer.dequeue(item)) {
        std::cout << "Dequeued: " << item << std::endl;
        buffer.status();
    }
    
    return 0;
}
```

## Template Member Functions

### Member Function Templates

```cpp
#include <iostream>
#include <vector>
#include <type_traits>

template<typename T>
class SmartContainer {
private:
    std::vector<T> data;
    
public:
    // Regular member functions
    void add(const T& item) {
        data.push_back(item);
    }
    
    size_t size() const { return data.size(); }
    
    // Template member function for conversion
    template<typename U>
    void addConverted(const U& item) {
        if constexpr (std::is_convertible_v<U, T>) {
            data.push_back(static_cast<T>(item));
        } else {
            std::cout << "Cannot convert type to container type" << std::endl;
        }
    }
    
    // Template member function for copying from other containers
    template<typename Iterator>
    void addRange(Iterator begin, Iterator end) {
        while (begin != end) {
            if constexpr (std::is_convertible_v<decltype(*begin), T>) {
                data.push_back(static_cast<T>(*begin));
            }
            ++begin;
        }
    }
    
    // Template member function for transformation
    template<typename Function>
    auto transform(Function func) const {
        using ResultType = decltype(func(std::declval<T>()));
        SmartContainer<ResultType> result;
        
        for (const auto& item : data) {
            result.add(func(item));
        }
        
        return result;
    }
    
    // Template member function for filtering
    template<typename Predicate>
    SmartContainer<T> filter(Predicate pred) const {
        SmartContainer<T> result;
        for (const auto& item : data) {
            if (pred(item)) {
                result.add(item);
            }
        }
        return result;
    }
    
    // Template member function for type checking
    template<typename U>
    bool contains() const {
        if constexpr (std::is_same_v<T, U>) {
            return !data.empty();
        } else {
            return false;
        }
    }
    
    void display() const {
        std::cout << "Container contents: ";
        for (const auto& item : data) {
            std::cout << item << " ";
        }
        std::cout << std::endl;
    }
    
    // Iterator access
    auto begin() { return data.begin(); }
    auto end() { return data.end(); }
    auto begin() const { return data.begin(); }
    auto end() const { return data.end(); }
};

int main() {
    SmartContainer<double> container;
    
    // Regular addition
    container.add(3.14);
    container.add(2.71);
    
    // Converted addition
    container.addConverted(42);      // int to double
    container.addConverted(5.5f);    // float to double
    
    container.display();
    
    // Add range from other container
    std::vector<int> integers = {10, 20, 30};
    container.addRange(integers.begin(), integers.end());
    container.display();
    
    // Transform to string container
    auto stringContainer = container.transform([](double d) {
        return std::to_string(d);
    });
    
    std::cout << "Transformed to strings: ";
    stringContainer.display();
    
    // Filter values greater than 10
    auto filtered = container.filter([](double d) {
        return d > 10.0;
    });
    
    std::cout << "Filtered (>10): ";
    filtered.display();
    
    // Type checking
    std::cout << "Contains doubles? " << container.contains<double>() << std::endl;
    std::cout << "Contains ints? " << container.contains<int>() << std::endl;
    
    return 0;
}
```

### Static Members in Class Templates

```cpp
#include <iostream>
#include <string>
#include <map>

template<typename T>
class ObjectCounter {
private:
    static size_t count;              // Static member declaration
    static std::map<std::string, size_t> type_counts; // Track different instantiations
    
    T data;
    
public:
    ObjectCounter(const T& value) : data(value) {
        ++count;
        ++type_counts[typeid(T).name()];
        std::cout << "Created object #" << count << " of type " << typeid(T).name() << std::endl;
    }
    
    ObjectCounter(const ObjectCounter& other) : data(other.data) {
        ++count;
        ++type_counts[typeid(T).name()];
        std::cout << "Copied object #" << count << " of type " << typeid(T).name() << std::endl;
    }
    
    ~ObjectCounter() {
        --count;
        --type_counts[typeid(T).name()];
        std::cout << "Destroyed object of type " << typeid(T).name() 
                  << " (remaining: " << count << ")" << std::endl;
    }
    
    // Static member functions
    static size_t getCount() { return count; }
    
    static void displayTypeCounts() {
        std::cout << "Object counts by type:" << std::endl;
        for (const auto& [type_name, type_count] : type_counts) {
            std::cout << "  " << type_name << ": " << type_count << std::endl;
        }
    }
    
    // Static template member function
    template<typename U>
    static size_t getCountForType() {
        auto it = type_counts.find(typeid(U).name());
        return (it != type_counts.end()) ? it->second : 0;
    }
    
    T getData() const { return data; }
    void setData(const T& value) { data = value; }
};

// Static member definitions (required for each instantiation)
template<typename T>
size_t ObjectCounter<T>::count = 0;

template<typename T>
std::map<std::string, size_t> ObjectCounter<T>::type_counts;

// Singleton pattern with class template
template<typename T>
class Singleton {
private:
    static T* instance;
    
    Singleton() = default;
    
public:
    static T* getInstance() {
        if (instance == nullptr) {
            instance = new T();
        }
        return instance;
    }
    
    static void destroyInstance() {
        delete instance;
        instance = nullptr;
    }
    
    // Prevent copying
    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;
};

template<typename T>
T* Singleton<T>::instance = nullptr;

// Example classes for singleton
class DatabaseConnection {
public:
    void connect() { std::cout << "Database connected" << std::endl; }
    void disconnect() { std::cout << "Database disconnected" << std::endl; }
};

class Logger {
public:
    void log(const std::string& message) {
        std::cout << "[LOG] " << message << std::endl;
    }
};

int main() {
    std::cout << "=== Object Counter Example ===" << std::endl;
    
    {
        ObjectCounter<int> int_obj1(42);
        ObjectCounter<int> int_obj2(100);
        ObjectCounter<std::string> str_obj1("hello");
        
        std::cout << "\nCurrent counts:" << std::endl;
        std::cout << "Total int objects: " << ObjectCounter<int>::getCount() << std::endl;
        std::cout << "Total string objects: " << ObjectCounter<std::string>::getCount() << std::endl;
        
        ObjectCounter<int>::displayTypeCounts();
        
        {
            ObjectCounter<double> double_obj(3.14);
            ObjectCounter<int> int_obj3(200);
            
            std::cout << "\nAfter creating more objects:" << std::endl;
            ObjectCounter<int>::displayTypeCounts();
        }
        
        std::cout << "\nAfter inner scope:" << std::endl;
        ObjectCounter<int>::displayTypeCounts();
    }
    
    std::cout << "\nAfter all objects destroyed:" << std::endl;
    ObjectCounter<int>::displayTypeCounts();
    
    std::cout << "\n=== Singleton Example ===" << std::endl;
    
    // Get singleton instances
    auto* db1 = Singleton<DatabaseConnection>::getInstance();
    auto* db2 = Singleton<DatabaseConnection>::getInstance();
    
    std::cout << "Same database instance? " << (db1 == db2) << std::endl;
    
    db1->connect();
    db2->disconnect();  // Same object
    
    auto* logger1 = Singleton<Logger>::getInstance();
    auto* logger2 = Singleton<Logger>::getInstance();
    
    std::cout << "Same logger instance? " << (logger1 == logger2) << std::endl;
    
    logger1->log("Message from logger1");
    logger2->log("Message from logger2");  // Same object
    
    // Cleanup
    Singleton<DatabaseConnection>::destroyInstance();
    Singleton<Logger>::destroyInstance();
    
    return 0;
}
```

## Template Inheritance

### Inheriting from Template Classes

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>

// Base template class
template<typename T>
class Container {
protected:
    std::vector<T> data;
    
public:
    virtual ~Container() = default;
    
    void add(const T& item) {
        data.push_back(item);
    }
    
    size_t size() const { return data.size(); }
    bool empty() const { return data.empty(); }
    
    virtual void display() const {
        std::cout << "Container: ";
        for (const auto& item : data) {
            std::cout << item << " ";
        }
        std::cout << std::endl;
    }
    
    // Iterator access
    typename std::vector<T>::iterator begin() { return data.begin(); }
    typename std::vector<T>::iterator end() { return data.end(); }
    typename std::vector<T>::const_iterator begin() const { return data.begin(); }
    typename std::vector<T>::const_iterator end() const { return data.end(); }
};

// Derived template class
template<typename T>
class SortedContainer : public Container<T> {
public:
    void add(const T& item) override {
        // Insert in sorted position
        auto pos = std::upper_bound(this->data.begin(), this->data.end(), item);
        this->data.insert(pos, item);
    }
    
    void display() const override {
        std::cout << "SortedContainer: ";
        for (const auto& item : this->data) {
            std::cout << item << " ";
        }
        std::cout << std::endl;
    }
    
    bool isSorted() const {
        return std::is_sorted(this->data.begin(), this->data.end());
    }
    
    T median() const {
        if (this->data.empty()) {
            throw std::runtime_error("Container is empty");
        }
        
        size_t middle = this->data.size() / 2;
        if (this->data.size() % 2 == 0) {
            return (this->data[middle - 1] + this->data[middle]) / 2;
        } else {
            return this->data[middle];
        }
    }
};

// Template class inheriting from non-template base
class BaseCounter {
protected:
    static size_t total_count;
    
public:
    BaseCounter() { ++total_count; }
    virtual ~BaseCounter() { --total_count; }
    
    static size_t getTotalCount() { return total_count; }
};

size_t BaseCounter::total_count = 0;

template<typename T>
class TypedCounter : public BaseCounter {
private:
    static size_t type_count;
    T value;
    
public:
    TypedCounter(const T& val) : BaseCounter(), value(val) {
        ++type_count;
        std::cout << "Created TypedCounter<" << typeid(T).name() 
                  << "> #" << type_count << std::endl;
    }
    
    ~TypedCounter() {
        --type_count;
        std::cout << "Destroyed TypedCounter<" << typeid(T).name() 
                  << "> (remaining: " << type_count << ")" << std::endl;
    }
    
    static size_t getTypeCount() { return type_count; }
    
    T getValue() const { return value; }
    void setValue(const T& val) { value = val; }
};

template<typename T>
size_t TypedCounter<T>::type_count = 0;

// Template specialization for inheritance
template<typename T>
class Stack : public Container<T> {
public:
    void push(const T& item) {
        this->add(item);
    }
    
    T pop() {
        if (this->empty()) {
            throw std::runtime_error("Stack is empty");
        }
        T item = this->data.back();
        this->data.pop_back();
        return item;
    }
    
    T& top() {
        if (this->empty()) {
            throw std::runtime_error("Stack is empty");
        }
        return this->data.back();
    }
    
    void display() const override {
        std::cout << "Stack (top to bottom): ";
        for (auto it = this->data.rbegin(); it != this->data.rend(); ++it) {
            std::cout << *it << " ";
        }
        std::cout << std::endl;
    }
};

int main() {
    std::cout << "=== Template Inheritance ===" << std::endl;
    
    // Polymorphic usage
    std::vector<std::unique_ptr<Container<int>>> containers;
    containers.push_back(std::make_unique<Container<int>>());
    containers.push_back(std::make_unique<SortedContainer<int>>());
    containers.push_back(std::make_unique<Stack<int>>());
    
    // Add data to each container
    for (auto& container : containers) {
        container->add(30);
        container->add(10);
        container->add(20);
        container->display();
    }
    
    std::cout << "\n=== Sorted Container Specific Features ===" << std::endl;
    SortedContainer<double> sorted;
    sorted.add(3.14);
    sorted.add(1.41);
    sorted.add(2.71);
    sorted.add(1.73);
    
    sorted.display();
    std::cout << "Is sorted? " << sorted.isSorted() << std::endl;
    std::cout << "Median: " << sorted.median() << std::endl;
    
    std::cout << "\n=== Stack Operations ===" << std::endl;
    Stack<std::string> stringStack;
    stringStack.push("First");
    stringStack.push("Second");
    stringStack.push("Third");
    
    stringStack.display();
    
    std::cout << "Top: " << stringStack.top() << std::endl;
    std::cout << "Popped: " << stringStack.pop() << std::endl;
    stringStack.display();
    
    std::cout << "\n=== Inheritance from Non-Template Base ===" << std::endl;
    {
        TypedCounter<int> intCounter1(42);
        TypedCounter<int> intCounter2(100);
        TypedCounter<std::string> stringCounter("hello");
        
        std::cout << "Total objects: " << BaseCounter::getTotalCount() << std::endl;
        std::cout << "Int counters: " << TypedCounter<int>::getTypeCount() << std::endl;
        std::cout << "String counters: " << TypedCounter<std::string>::getTypeCount() << std::endl;
    }
    
    std::cout << "After destruction - Total: " << BaseCounter::getTotalCount() << std::endl;
    
    return 0;
}
```

## Template Friends

### Friend Functions and Classes in Templates

```cpp
#include <iostream>
#include <vector>

// Forward declaration for friend
template<typename T> class Matrix;
template<typename T> Matrix<T> operator+(const Matrix<T>& a, const Matrix<T>& b);
template<typename T> std::ostream& operator<<(std::ostream& os, const Matrix<T>& matrix);

template<typename T>
class Matrix {
private:
    std::vector<std::vector<T>> data;
    size_t rows, cols;
    
public:
    Matrix(size_t r, size_t c, T initial_value = T{}) : rows(r), cols(c) {
        data.resize(rows, std::vector<T>(cols, initial_value));
    }
    
    // Constructor from initializer list
    Matrix(std::initializer_list<std::initializer_list<T>> init) {
        rows = init.size();
        cols = (rows > 0) ? init.begin()->size() : 0;
        
        data.reserve(rows);
        for (const auto& row : init) {
            if (row.size() != cols) {
                throw std::invalid_argument("All rows must have the same size");
            }
            data.emplace_back(row);
        }
    }
    
    // Element access
    std::vector<T>& operator[](size_t row) { return data[row]; }
    const std::vector<T>& operator[](size_t row) const { return data[row]; }
    
    size_t getRows() const { return rows; }
    size_t getCols() const { return cols; }
    
    // Friend function for addition (needs access to private data)
    friend Matrix<T> operator+<T>(const Matrix<T>& a, const Matrix<T>& b);
    
    // Friend function for output (needs access to private data)
    friend std::ostream& operator<<<T>(std::ostream& os, const Matrix<T>& matrix);
    
    // Friend class for matrix operations
    friend class MatrixCalculator;
    
    // Template friend function (befriends all instantiations)
    template<typename U>
    friend class MatrixAnalyzer;
};

// Friend function implementations
template<typename T>
Matrix<T> operator+(const Matrix<T>& a, const Matrix<T>& b) {
    if (a.rows != b.rows || a.cols != b.cols) {
        throw std::invalid_argument("Matrix dimensions must match");
    }
    
    Matrix<T> result(a.rows, a.cols);
    for (size_t i = 0; i < a.rows; ++i) {
        for (size_t j = 0; j < a.cols; ++j) {
            result.data[i][j] = a.data[i][j] + b.data[i][j];
        }
    }
    return result;
}

template<typename T>
std::ostream& operator<<(std::ostream& os, const Matrix<T>& matrix) {
    for (size_t i = 0; i < matrix.rows; ++i) {
        os << "[";
        for (size_t j = 0; j < matrix.cols; ++j) {
            os << matrix.data[i][j];
            if (j < matrix.cols - 1) os << " ";
        }
        os << "]" << std::endl;
    }
    return os;
}

// Friend class with access to all Matrix instantiations
class MatrixCalculator {
public:
    template<typename T>
    static Matrix<T> multiply(const Matrix<T>& a, const Matrix<T>& b) {
        if (a.cols != b.rows) {
            throw std::invalid_argument("Invalid dimensions for multiplication");
        }
        
        Matrix<T> result(a.rows, b.cols);
        for (size_t i = 0; i < a.rows; ++i) {
            for (size_t j = 0; j < b.cols; ++j) {
                for (size_t k = 0; k < a.cols; ++k) {
                    result.data[i][j] += a.data[i][k] * b.data[k][j];
                }
            }
        }
        return result;
    }
    
    template<typename T>
    static Matrix<T> transpose(const Matrix<T>& matrix) {
        Matrix<T> result(matrix.cols, matrix.rows);
        for (size_t i = 0; i < matrix.rows; ++i) {
            for (size_t j = 0; j < matrix.cols; ++j) {
                result.data[j][i] = matrix.data[i][j];
            }
        }
        return result;
    }
    
    template<typename T>
    static T trace(const Matrix<T>& matrix) {
        if (matrix.rows != matrix.cols) {
            throw std::invalid_argument("Matrix must be square");
        }
        
        T sum = T{};
        for (size_t i = 0; i < matrix.rows; ++i) {
            sum += matrix.data[i][i];
        }
        return sum;
    }
};

// Template friend class
template<typename T>
class MatrixAnalyzer {
public:
    static void analyzeMatrix(const Matrix<T>& matrix) {
        std::cout << "Matrix Analysis:" << std::endl;
        std::cout << "Dimensions: " << matrix.rows << "x" << matrix.cols << std::endl;
        std::cout << "Type: " << typeid(T).name() << std::endl;
        
        if (matrix.rows == matrix.cols) {
            std::cout << "Square matrix - Trace: " << MatrixCalculator::trace(matrix) << std::endl;
        }
        
        // Calculate sum of all elements (direct access to private data)
        T sum = T{};
        for (const auto& row : matrix.data) {
            for (const auto& element : row) {
                sum += element;
            }
        }
        std::cout << "Sum of elements: " << sum << std::endl;
        
        std::cout << "Matrix:" << std::endl << matrix << std::endl;
    }
};

int main() {
    std::cout << "=== Matrix Template with Friends ===" << std::endl;
    
    Matrix<int> m1 = {{1, 2, 3},
                      {4, 5, 6}};
    
    Matrix<int> m2 = {{7, 8, 9},
                      {1, 2, 3}};
    
    std::cout << "Matrix 1:" << std::endl << m1;
    std::cout << "Matrix 2:" << std::endl << m2;
    
    // Friend function usage
    Matrix<int> sum = m1 + m2;
    std::cout << "Sum:" << std::endl << sum;
    
    // Friend class usage
    Matrix<int> product = MatrixCalculator::multiply(m1, m2.transpose());
    std::cout << "Product with transpose:" << std::endl << product;
    
    Matrix<int> transposed = MatrixCalculator::transpose(m1);
    std::cout << "Transposed m1:" << std::endl << transposed;
    
    // Square matrix for trace
    Matrix<double> square = {{1.0, 2.0, 3.0},
                            {4.0, 5.0, 6.0},
                            {7.0, 8.0, 9.0}};
    
    std::cout << "Square matrix trace: " << MatrixCalculator::trace(square) << std::endl;
    
    // Template friend class usage
    std::cout << "\n=== Matrix Analysis ===" << std::endl;
    MatrixAnalyzer<int>::analyzeMatrix(m1);
    MatrixAnalyzer<double>::analyzeMatrix(square);
    
    return 0;
}
```

## Template Aliases and Nested Templates

### Template Aliases

```cpp
#include <iostream>
#include <vector>
#include <map>
#include <memory>
#include <functional>

// Basic template aliases
template<typename T>
using Ptr = std::shared_ptr<T>;

template<typename T>
using Vec = std::vector<T>;

template<typename Key, typename Value>
using Map = std::map<Key, Value>;

// More complex aliases
template<typename T>
using Matrix = std::vector<std::vector<T>>;

template<typename T>
using Callback = std::function<void(const T&)>;

template<typename T>
using Predicate = std::function<bool(const T&)>;

template<typename From, typename To>
using Converter = std::function<To(const From&)>;

// Aliases for nested templates
template<typename T>
class Container {
public:
    using value_type = T;
    using iterator = typename std::vector<T>::iterator;
    using const_iterator = typename std::vector<T>::const_iterator;
    using size_type = typename std::vector<T>::size_type;
    
private:
    std::vector<T> data;
    
public:
    void add(const T& item) { data.push_back(item); }
    size_type size() const { return data.size(); }
    
    iterator begin() { return data.begin(); }
    iterator end() { return data.end(); }
    const_iterator begin() const { return data.begin(); }
    const_iterator end() const { return data.end(); }
    
    void display() const {
        std::cout << "Container: ";
        for (const auto& item : data) {
            std::cout << item << " ";
        }
        std::cout << std::endl;
    }
};

// Convenience aliases for specific container types
using IntContainer = Container<int>;
using StringContainer = Container<std::string>;
using DoubleContainer = Container<double>;

// Generic processing function using aliases
template<typename T>
void processContainer(Container<T>& container, 
                     Callback<T> callback, 
                     Predicate<T> filter = [](const T&) { return true; }) {
    for (const auto& item : container) {
        if (filter(item)) {
            callback(item);
        }
    }
}

// Factory function using aliases
template<typename T>
Ptr<Container<T>> createContainer() {
    return std::make_shared<Container<T>>();
}

int main() {
    std::cout << "=== Template Aliases Example ===" << std::endl;
    
    // Using basic aliases
    Ptr<int> intPtr = std::make_shared<int>(42);
    Vec<std::string> stringVec = {"hello", "world", "template"};
    Map<std::string, int> stringToInt = {{"one", 1}, {"two", 2}, {"three", 3}};
    
    std::cout << "Shared pointer value: " << *intPtr << std::endl;
    
    std::cout << "String vector: ";
    for (const auto& str : stringVec) {
        std::cout << str << " ";
    }
    std::cout << std::endl;
    
    std::cout << "String to int map:" << std::endl;
    for (const auto& [key, value] : stringToInt) {
        std::cout << "  " << key << " -> " << value << std::endl;
    }
    
    // Using container aliases
    IntContainer intContainer;
    StringContainer stringContainer;
    
    intContainer.add(10);
    intContainer.add(20);
    intContainer.add(30);
    
    stringContainer.add("first");
    stringContainer.add("second");
    stringContainer.add("third");
    
    intContainer.display();
    stringContainer.display();
    
    // Using function type aliases
    Callback<int> printInt = [](const int& value) {
        std::cout << "Processing: " << value << std::endl;
    };
    
    Predicate<int> isEven = [](const int& value) {
        return value % 2 == 0;
    };
    
    std::cout << "\nProcessing even numbers only:" << std::endl;
    processContainer(intContainer, printInt, isEven);
    
    // Using factory with aliases
    auto containerPtr = createContainer<double>();
    containerPtr->add(3.14);
    containerPtr->add(2.71);
    containerPtr->add(1.41);
    
    std::cout << "\nFactory-created container:" << std::endl;
    containerPtr->display();
    
    // Matrix alias usage
    Matrix<int> matrix = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}};
    
    std::cout << "\nMatrix:" << std::endl;
    for (const auto& row : matrix) {
        for (const auto& element : row) {
            std::cout << element << " ";
        }
        std::cout << std::endl;
    }
    
    return 0;
}
```

### Nested Templates

```cpp
#include <iostream>
#include <vector>
#include <map>
#include <functional>

template<typename T>
class DataProcessor {
public:
    // Nested class template
    template<typename U>
    class Converter {
    private:
        std::function<U(const T&)> convert_func;
        
    public:
        Converter(std::function<U(const T&)> func) : convert_func(func) {}
        
        U convert(const T& input) const {
            return convert_func(input);
        }
        
        std::vector<U> convertMany(const std::vector<T>& inputs) const {
            std::vector<U> results;
            results.reserve(inputs.size());
            for (const auto& input : inputs) {
                results.push_back(convert(input));
            }
            return results;
        }
    };
    
    // Nested template struct
    template<typename FilterType>
    struct Filter {
        std::function<bool(const T&)> predicate;
        
        Filter(std::function<bool(const T&)> pred) : predicate(pred) {}
        
        std::vector<T> apply(const std::vector<T>& data) const {
            std::vector<T> result;
            for (const auto& item : data) {
                if (predicate(item)) {
                    result.push_back(item);
                }
            }
            return result;
        }
    };
    
    // Nested template for aggregation
    template<typename ResultType>
    class Aggregator {
    private:
        std::function<ResultType(const std::vector<T>&)> agg_func;
        
    public:
        Aggregator(std::function<ResultType(const std::vector<T>&)> func) : agg_func(func) {}
        
        ResultType aggregate(const std::vector<T>& data) const {
            return agg_func(data);
        }
        
        // Template member function within nested template
        template<typename Predicate>
        ResultType aggregateFiltered(const std::vector<T>& data, Predicate pred) const {
            std::vector<T> filtered;
            for (const auto& item : data) {
                if (pred(item)) {
                    filtered.push_back(item);
                }
            }
            return agg_func(filtered);
        }
    };
    
private:
    std::vector<T> data;
    
public:
    void addData(const T& item) {
        data.push_back(item);
    }
    
    const std::vector<T>& getData() const { return data; }
    
    // Factory methods for nested templates
    template<typename U>
    Converter<U> createConverter(std::function<U(const T&)> func) const {
        return Converter<U>(func);
    }
    
    template<typename FilterType = bool>
    Filter<FilterType> createFilter(std::function<bool(const T&)> pred) const {
        return Filter<FilterType>(pred);
    }
    
    template<typename ResultType>
    Aggregator<ResultType> createAggregator(std::function<ResultType(const std::vector<T>&)> func) const {
        return Aggregator<ResultType>(func);
    }
    
    void display() const {
        std::cout << "Data: ";
        for (const auto& item : data) {
            std::cout << item << " ";
        }
        std::cout << std::endl;
    }
};

// Specialized nested template
template<typename T>
class Graph {
public:
    // Nested template for nodes
    template<typename NodeData>
    class Node {
    public:
        NodeData data;
        std::vector<Node<NodeData>*> neighbors;
        
        Node(const NodeData& nodeData) : data(nodeData) {}
        
        void addNeighbor(Node<NodeData>* neighbor) {
            neighbors.push_back(neighbor);
        }
        
        template<typename Visitor>
        void visit(Visitor visitor) const {
            visitor(data);
        }
        
        template<typename Predicate>
        std::vector<Node<NodeData>*> findNeighbors(Predicate pred) const {
            std::vector<Node<NodeData>*> result;
            for (auto* neighbor : neighbors) {
                if (pred(neighbor->data)) {
                    result.push_back(neighbor);
                }
            }
            return result;
        }
    };
    
    // Nested template for edges
    template<typename EdgeData>
    struct Edge {
        Node<T>* from;
        Node<T>* to;
        EdgeData weight;
        
        Edge(Node<T>* f, Node<T>* t, const EdgeData& w) : from(f), to(t), weight(w) {}
    };
    
private:
    std::vector<std::unique_ptr<Node<T>>> nodes;
    
public:
    Node<T>* addNode(const T& data) {
        auto node = std::make_unique<Node<T>>(data);
        Node<T>* nodePtr = node.get();
        nodes.push_back(std::move(node));
        return nodePtr;
    }
    
    template<typename Visitor>
    void visitAllNodes(Visitor visitor) const {
        for (const auto& node : nodes) {
            node->visit(visitor);
        }
    }
    
    size_t getNodeCount() const { return nodes.size(); }
};

int main() {
    std::cout << "=== Nested Templates Example ===" << std::endl;
    
    // DataProcessor with nested templates
    DataProcessor<int> intProcessor;
    intProcessor.addData(1);
    intProcessor.addData(2);
    intProcessor.addData(3);
    intProcessor.addData(4);
    intProcessor.addData(5);
    
    intProcessor.display();
    
    // Using nested Converter
    auto doubleConverter = intProcessor.createConverter<double>(
        [](const int& x) { return static_cast<double>(x) * 1.5; });
    
    auto stringConverter = intProcessor.createConverter<std::string>(
        [](const int& x) { return "Number_" + std::to_string(x); });
    
    auto doubleResults = doubleConverter.convertMany(intProcessor.getData());
    auto stringResults = stringConverter.convertMany(intProcessor.getData());
    
    std::cout << "Converted to doubles: ";
    for (const auto& d : doubleResults) {
        std::cout << d << " ";
    }
    std::cout << std::endl;
    
    std::cout << "Converted to strings: ";
    for (const auto& s : stringResults) {
        std::cout << s << " ";
    }
    std::cout << std::endl;
    
    // Using nested Filter
    auto evenFilter = intProcessor.createFilter(
        [](const int& x) { return x % 2 == 0; });
    
    auto evenNumbers = evenFilter.apply(intProcessor.getData());
    std::cout << "Even numbers: ";
    for (const auto& n : evenNumbers) {
        std::cout << n << " ";
    }
    std::cout << std::endl;
    
    // Using nested Aggregator
    auto sumAggregator = intProcessor.createAggregator<int>(
        [](const std::vector<int>& data) {
            int sum = 0;
            for (const auto& x : data) sum += x;
            return sum;
        });
    
    int total = sumAggregator.aggregate(intProcessor.getData());
    int evenSum = sumAggregator.aggregateFiltered(intProcessor.getData(),
        [](const int& x) { return x % 2 == 0; });
    
    std::cout << "Total sum: " << total << std::endl;
    std::cout << "Even sum: " << evenSum << std::endl;
    
    std::cout << "\n=== Graph with Nested Templates ===" << std::endl;
    
    Graph<std::string> stringGraph;
    
    auto* node1 = stringGraph.addNode("Node1");
    auto* node2 = stringGraph.addNode("Node2");
    auto* node3 = stringGraph.addNode("Node3");
    
    node1->addNeighbor(node2);
    node1->addNeighbor(node3);
    node2->addNeighbor(node3);
    
    std::cout << "Graph nodes: ";
    stringGraph.visitAllNodes([](const std::string& data) {
        std::cout << data << " ";
    });
    std::cout << std::endl;
    
    std::cout << "Node1 neighbors: ";
    node1->visit([](const std::string& data) {
        std::cout << data << "'s neighbors: ";
    });
    
    auto neighbors = node1->findNeighbors([](const std::string& data) {
        return data.find("Node") != std::string::npos;
    });
    
    for (const auto* neighbor : neighbors) {
        std::cout << neighbor->data << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

## Exercises

### Exercise 1: Generic Smart Pointer

Create a generic smart pointer class template:

- `UniquePtr<T>` with move semantics and RAII
- `SharedPtr<T>` with reference counting
- `WeakPtr<T>` to break circular references
- Custom deleters and array support

### Exercise 2: Template Container Library

Build a comprehensive container library:

- `Vector<T>` - dynamic array with growth strategies
- `List<T>` - doubly-linked list with iterators
- `Stack<T>` and `Queue<T>` - LIFO and FIFO containers
- `HashTable<K,V>` - hash-based associative container

### Exercise 3: Template-Based Observer Pattern

Design an observer pattern using templates:

- `Subject<T>` - observable object with typed events
- `Observer<T>` - base class for event handlers
- `EventManager<T>` - central event dispatcher
- Type-safe event subscription and notification

### Exercise 4: Generic Algorithm Library

Create a generic algorithm library:

- Sorting algorithms (quicksort, mergesort, heapsort)
- Search algorithms (binary search, interpolation search)
- Graph algorithms (DFS, BFS, shortest path)
- Mathematical algorithms (FFT, matrix operations)

## Key Takeaways

<i class="fa-solid fa-arrow-right"></i> **Class templates enable generic data structures** that work with any type  
<i class="fa-solid fa-arrow-right"></i> **Template member functions provide flexible operations** within template classes  
<i class="fa-solid fa-arrow-right"></i> **Static members in templates are per-instantiation** - each type gets its own  
<i class="fa-solid fa-arrow-right"></i> **Template inheritance enables hierarchies** of generic classes  
<i class="fa-solid fa-arrow-right"></i> **Friend functions and classes provide controlled access** to template internals  
<i class="fa-solid fa-arrow-right"></i> **Template aliases simplify complex type names** and improve readability  
<i class="fa-solid fa-arrow-right"></i> **Nested templates enable sophisticated designs** but should be used judiciously  
<i class="fa-solid fa-arrow-right"></i> **Follow the Rule of Zero/Three/Five** for template classes with resources

---

**Next**: Learn about customizing templates for specific types // →  [Template Specialization](specialization.md)
