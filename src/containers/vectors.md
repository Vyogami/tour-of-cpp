# Vectors and Dynamic Arrays

`std::vector` is the most important and frequently used container in C++. It provides a dynamic array that can grow and shrink at runtime while maintaining excellent performance characteristics. Vectors combine the efficiency of arrays with the flexibility of dynamic memory management.

## Introduction to `std::vector`

Vector is a sequence container that encapsulates dynamic size arrays:

```cpp
#include <iostream>
#include <vector>

int main() {
    // Declaration and initialization
    std::vector<int> numbers;                          // Empty vector
    std::vector<int> scores = {85, 92, 78, 96, 88};   // Initializer list
    std::vector<int> zeros(10);                       // 10 elements, all zero
    std::vector<int> fives(5, 42);                    // 5 elements, all 42
    std::vector<int> copy(scores);                    // Copy constructor
    
    std::cout << "Vector sizes:" << std::endl;
    std::cout << "numbers: " << numbers.size() << std::endl;
    std::cout << "scores: " << scores.size() << std::endl;
    std::cout << "zeros: " << zeros.size() << std::endl;
    std::cout << "fives: " << fives.size() << std::endl;
    std::cout << "copy: " << copy.size() << std::endl;
    
    // Basic operations
    std::cout << "\nScores: ";
    for (const auto& score : scores) {
        std::cout << score << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

## Basic Vector Operations

### Element Access

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> data = {10, 20, 30, 40, 50};
    
    // Different ways to access elements
    std::cout << "Element access methods:" << std::endl;
    std::cout << "data[2]: " << data[2] << std::endl;           // Subscript (no bounds check)
    std::cout << "data.at(2): " << data.at(2) << std::endl;     // at() method (bounds checked)
    std::cout << "data.front(): " << data.front() << std::endl; // First element
    std::cout << "data.back(): " << data.back() << std::endl;   // Last element
    
    // Modify elements
    data[1] = 99;
    data.at(3) = 88;
    
    std::cout << "\nAfter modification: ";
    for (size_t i = 0; i < data.size(); i++) {
        std::cout << data[i] << " ";
    }
    std::cout << std::endl;
    
    // Bounds checking demonstration
    try {
        std::cout << "Trying to access data.at(10)..." << std::endl;
        std::cout << data.at(10) << std::endl;  // Throws std::out_of_range
    } catch (const std::out_of_range& e) {
        std::cout << "Exception: " << e.what() << std::endl;
    }
    
    // data[10] would be undefined behavior - no exception thrown!
    
    return 0;
}
```

### Adding and Removing Elements

```cpp
#include <iostream>
#include <vector>

void printVector(const std::vector<int>& vec, const std::string& label) {
    std::cout << label << ": ";
    for (const auto& element : vec) {
        std::cout << element << " ";
    }
    std::cout << "(size: " << vec.size() << ")" << std::endl;
}

int main() {
    std::vector<int> numbers;
    
    // Adding elements
    numbers.push_back(10);        // Add to end
    numbers.push_back(20);
    numbers.push_back(30);
    printVector(numbers, "After push_back");
    
    // Insert at specific position
    numbers.insert(numbers.begin() + 1, 15);  // Insert 15 at index 1
    printVector(numbers, "After insert");
    
    // Insert multiple elements
    numbers.insert(numbers.end(), {40, 50, 60});  // Insert at end
    printVector(numbers, "After multiple insert");
    
    // Removing elements
    numbers.pop_back();           // Remove last element
    printVector(numbers, "After pop_back");
    
    // Erase specific element
    numbers.erase(numbers.begin() + 2);  // Remove element at index 2
    printVector(numbers, "After erase");
    
    // Erase range of elements
    numbers.erase(numbers.begin() + 1, numbers.begin() + 3);  // Remove elements [1, 3)
    printVector(numbers, "After range erase");
    
    // Clear all elements
    numbers.clear();
    printVector(numbers, "After clear");
    std::cout << "Is empty: " << std::boolalpha << numbers.empty() << std::endl;
    
    return 0;
}
```

## Vector Capacity and Memory Management

### Understanding Capacity vs Size

```cpp
#include <iostream>
#include <vector>

void printVectorInfo(const std::vector<int>& vec, const std::string& operation) {
    std::cout << operation << ":" << std::endl;
    std::cout << "  Size: " << vec.size() << std::endl;
    std::cout << "  Capacity: " << vec.capacity() << std::endl;
    std::cout << "  Empty: " << std::boolalpha << vec.empty() << std::endl;
    std::cout << std::endl;
}

int main() {
    std::vector<int> numbers;
    printVectorInfo(numbers, "Initial state");
    
    // Adding elements one by one
    for (int i = 1; i <= 10; i++) {
        numbers.push_back(i * 10);
        if (i <= 5 || i == 10) {  // Show first few and last
            printVectorInfo(numbers, "After push_back " + std::to_string(i));
        }
    }
    
    // Reserve capacity to avoid reallocations
    std::vector<int> reserved;
    reserved.reserve(20);  // Pre-allocate space for 20 elements
    printVectorInfo(reserved, "After reserve(20)");
    
    for (int i = 1; i <= 15; i++) {
        reserved.push_back(i);
    }
    printVectorInfo(reserved, "After adding 15 elements");
    
    // Shrink to fit
    reserved.shrink_to_fit();  // Reduce capacity to match size
    printVectorInfo(reserved, "After shrink_to_fit");
    
    // Resize vector
    reserved.resize(10);       // Reduce size to 10
    printVectorInfo(reserved, "After resize(10)");
    
    reserved.resize(15, 99);   // Increase size to 15, fill new elements with 99
    printVectorInfo(reserved, "After resize(15, 99)");
    
    std::cout << "Final contents: ";
    for (const auto& element : reserved) {
        std::cout << element << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

### Memory Reallocation Behavior

```cpp
#include <iostream>
#include <vector>

class TrackedInt {
private:
    int value;
    static int instances;
    
public:
    TrackedInt(int v = 0) : value(v) {
        ++instances;
        std::cout << "TrackedInt(" << value << ") created. Total: " << instances << std::endl;
    }
    
    TrackedInt(const TrackedInt& other) : value(other.value) {
        ++instances;
        std::cout << "TrackedInt(" << value << ") copied. Total: " << instances << std::endl;
    }
    
    TrackedInt(TrackedInt&& other) noexcept : value(other.value) {
        ++instances;
        std::cout << "TrackedInt(" << value << ") moved. Total: " << instances << std::endl;
    }
    
    ~TrackedInt() {
        --instances;
        std::cout << "TrackedInt(" << value << ") destroyed. Total: " << instances << std::endl;
    }
    
    TrackedInt& operator=(const TrackedInt& other) {
        value = other.value;
        std::cout << "TrackedInt(" << value << ") assigned" << std::endl;
        return *this;
    }
    
    int getValue() const { return value; }
    static int getInstanceCount() { return instances; }
};

int TrackedInt::instances = 0;

int main() {
    std::cout << "=== Demonstrating vector reallocation ===" << std::endl;
    
    std::vector<TrackedInt> vec;
    std::cout << "Initial capacity: " << vec.capacity() << std::endl;
    
    // Add elements and watch reallocations
    for (int i = 1; i <= 8; i++) {
        std::cout << "\n--- Adding element " << i << " ---" << std::endl;
        vec.push_back(TrackedInt(i * 10));
        std::cout << "Size: " << vec.size() << ", Capacity: " << vec.capacity() << std::endl;
    }
    
    std::cout << "\n=== Using reserve to avoid reallocations ===" << std::endl;
    
    std::vector<TrackedInt> reservedVec;
    reservedVec.reserve(8);  // Pre-allocate capacity
    std::cout << "Reserved capacity: " << reservedVec.capacity() << std::endl;
    
    for (int i = 1; i <= 8; i++) {
        std::cout << "\n--- Adding element " << i << " to reserved vector ---" << std::endl;
        reservedVec.push_back(TrackedInt(i * 100));
        std::cout << "Size: " << reservedVec.size() << ", Capacity: " << reservedVec.capacity() << std::endl;
    }
    
    std::cout << "\n=== End of program - destructors will be called ===" << std::endl;
    
    return 0;
}
```

## Iterators and Vector

### Iterator Types and Usage

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

int main() {
    std::vector<int> numbers = {5, 2, 8, 1, 9, 3};
    
    std::cout << "Original vector: ";
    for (const auto& num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
    
    // Different iterator types
    std::cout << "\n=== Iterator types ===" << std::endl;
    
    // Forward iteration
    std::cout << "Forward iteration: ";
    for (auto it = numbers.begin(); it != numbers.end(); ++it) {
        std::cout << *it << " ";
    }
    std::cout << std::endl;
    
    // Reverse iteration
    std::cout << "Reverse iteration: ";
    for (auto it = numbers.rbegin(); it != numbers.rend(); ++it) {
        std::cout << *it << " ";
    }
    std::cout << std::endl;
    
    // Const iterators (read-only)
    std::cout << "Const iteration: ";
    for (auto it = numbers.cbegin(); it != numbers.cend(); ++it) {
        std::cout << *it << " ";
        // *it = 100;  // Error: cannot modify through const iterator
    }
    std::cout << std::endl;
    
    // Modify elements using iterators
    std::cout << "\n=== Modifying through iterators ===" << std::endl;
    for (auto it = numbers.begin(); it != numbers.end(); ++it) {
        *it *= 2;  // Double each element
    }
    
    std::cout << "After doubling: ";
    for (const auto& num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
    
    // Random access with iterators
    std::cout << "\n=== Random access ===" << std::endl;
    auto it = numbers.begin();
    std::cout << "First element: " << *it << std::endl;
    std::cout << "Third element: " << *(it + 2) << std::endl;
    std::cout << "Last element: " << *(numbers.end() - 1) << std::endl;
    
    // Distance between iterators
    std::cout << "Distance from begin to end: " << std::distance(numbers.begin(), numbers.end()) << std::endl;
    
    return 0;
}
```

### STL Algorithms with Vectors

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <numeric>
#include <functional>

int main() {
    std::vector<int> numbers = {5, 2, 8, 1, 9, 3, 7, 4, 6};
    
    std::cout << "Original: ";
    for (const auto& num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
    
    // Sorting
    std::sort(numbers.begin(), numbers.end());
    std::cout << "Sorted ascending: ";
    for (const auto& num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
    
    // Reverse sorting
    std::sort(numbers.begin(), numbers.end(), std::greater<int>());
    std::cout << "Sorted descending: ";
    for (const auto& num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
    
    // Searching
    auto it = std::find(numbers.begin(), numbers.end(), 5);
    if (it != numbers.end()) {
        std::cout << "Found 5 at position: " << std::distance(numbers.begin(), it) << std::endl;
    }
    
    // Binary search (requires sorted container)
    std::sort(numbers.begin(), numbers.end());
    bool found = std::binary_search(numbers.begin(), numbers.end(), 7);
    std::cout << "Binary search for 7: " << std::boolalpha << found << std::endl;
    
    // Counting
    int count = std::count_if(numbers.begin(), numbers.end(), [](int n) {
        return n > 5;
    });
    std::cout << "Count of numbers > 5: " << count << std::endl;
    
    // Transforming
    std::vector<int> squares(numbers.size());
    std::transform(numbers.begin(), numbers.end(), squares.begin(), [](int n) {
        return n * n;
    });
    
    std::cout << "Squares: ";
    for (const auto& square : squares) {
        std::cout << square << " ";
    }
    std::cout << std::endl;
    
    // Accumulating
    int sum = std::accumulate(numbers.begin(), numbers.end(), 0);
    std::cout << "Sum: " << sum << std::endl;
    
    int product = std::accumulate(numbers.begin(), numbers.end(), 1, std::multiplies<int>());
    std::cout << "Product: " << product << std::endl;
    
    // Filtering (remove elements)
    numbers = {5, 2, 8, 1, 9, 3, 7, 4, 6};  // Reset
    auto newEnd = std::remove_if(numbers.begin(), numbers.end(), [](int n) {
        return n % 2 == 0;  // Remove even numbers
    });
    numbers.erase(newEnd, numbers.end());  // Actually remove the elements
    
    std::cout << "After removing even numbers: ";
    for (const auto& num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

## Vector of Custom Objects

### Storing Custom Classes

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>

class Student {
private:
    std::string name;
    int age;
    double gpa;
    
public:
    Student(const std::string& n, int a, double g) : name(n), age(a), gpa(g) {}
    
    // Getters
    const std::string& getName() const { return name; }
    int getAge() const { return age; }
    double getGpa() const { return gpa; }
    
    // Setters
    void setGpa(double g) { gpa = g; }
    
    // Display
    void print() const {
        std::cout << name << " (age: " << age << ", GPA: " << gpa << ")";
    }
    
    // Comparison operators for sorting
    bool operator<(const Student& other) const {
        return gpa < other.gpa;  // Sort by GPA
    }
    
    bool operator>(const Student& other) const {
        return gpa > other.gpa;
    }
};

void printStudents(const std::vector<Student>& students, const std::string& title) {
    std::cout << title << ":" << std::endl;
    for (const auto& student : students) {
        std::cout << "  ";
        student.print();
        std::cout << std::endl;
    }
    std::cout << std::endl;
}

int main() {
    // Create vector of students
    std::vector<Student> students;
    
    // Add students using different methods
    students.emplace_back("Alice", 20, 3.8);    // Construct in-place
    students.emplace_back("Bob", 19, 3.2);
    students.push_back(Student("Charlie", 21, 3.9));  // Construct then copy/move
    students.emplace_back("Diana", 20, 3.5);
    
    printStudents(students, "Original list");
    
    // Sort by GPA (ascending)
    std::sort(students.begin(), students.end());
    printStudents(students, "Sorted by GPA (ascending)");
    
    // Sort by GPA (descending)
    std::sort(students.begin(), students.end(), std::greater<Student>());
    printStudents(students, "Sorted by GPA (descending)");
    
    // Custom sorting by name
    std::sort(students.begin(), students.end(), [](const Student& a, const Student& b) {
        return a.getName() < b.getName();
    });
    printStudents(students, "Sorted by name");
    
    // Find student with highest GPA
    auto maxGpaStudent = std::max_element(students.begin(), students.end(),
        [](const Student& a, const Student& b) {
            return a.getGpa() < b.getGpa();
        });
    
    std::cout << "Student with highest GPA: ";
    maxGpaStudent->print();
    std::cout << std::endl;
    
    // Count students with GPA > 3.5
    int highGpaCount = std::count_if(students.begin(), students.end(),
        [](const Student& s) {
            return s.getGpa() > 3.5;
        });
    
    std::cout << "Students with GPA > 3.5: " << highGpaCount << std::endl;
    
    // Find a specific student
    auto it = std::find_if(students.begin(), students.end(),
        [](const Student& s) {
            return s.getName() == "Charlie";
        });
    
    if (it != students.end()) {
        std::cout << "Found Charlie: ";
        it->print();
        std::cout << std::endl;
    }
    
    return 0;
}
```

## Vector Performance Optimization

### Performance Best Practices

```cpp
#include <iostream>
#include <vector>
#include <chrono>
#include <string>

class Timer {
private:
    std::chrono::high_resolution_clock::time_point start;
    std::string name;
    
public:
    Timer(const std::string& n) : name(n) {
        start = std::chrono::high_resolution_clock::now();
    }
    
    ~Timer() {
        auto end = std::chrono::high_resolution_clock::now();
        auto duration = std::chrono::duration_cast<std::chrono::microseconds>(end - start);
        std::cout << name << " took: " << duration.count() << " microseconds" << std::endl;
    }
};

void performanceComparison() {
    const int SIZE = 100000;
    
    std::cout << "=== Performance Comparison ===" << std::endl;
    
    // Test 1: push_back without reserve
    {
        Timer timer("push_back without reserve");
        std::vector<int> vec;
        for (int i = 0; i < SIZE; i++) {
            vec.push_back(i);
        }
    }
    
    // Test 2: push_back with reserve
    {
        Timer timer("push_back with reserve");
        std::vector<int> vec;
        vec.reserve(SIZE);
        for (int i = 0; i < SIZE; i++) {
            vec.push_back(i);
        }
    }
    
    // Test 3: resize then assign
    {
        Timer timer("resize then assign");
        std::vector<int> vec(SIZE);
        for (int i = 0; i < SIZE; i++) {
            vec[i] = i;
        }
    }
    
    // Test 4: emplace_back vs push_back
    std::cout << "\n=== emplace_back vs push_back ===" << std::endl;
    
    {
        Timer timer("push_back with string construction");
        std::vector<std::string> vec;
        vec.reserve(SIZE);
        for (int i = 0; i < SIZE; i++) {
            vec.push_back(std::string("String" + std::to_string(i)));
        }
    }
    
    {
        Timer timer("emplace_back with in-place construction");
        std::vector<std::string> vec;
        vec.reserve(SIZE);
        for (int i = 0; i < SIZE; i++) {
            vec.emplace_back("String" + std::to_string(i));
        }
    }
}

// Memory efficient vector operations
void memoryEfficiency() {
    std::cout << "\n=== Memory Efficiency ===" << std::endl;
    
    std::vector<int> vec;
    
    // Add many elements
    for (int i = 0; i < 1000; i++) {
        vec.push_back(i);
    }
    
    std::cout << "After adding 1000 elements:" << std::endl;
    std::cout << "Size: " << vec.size() << ", Capacity: " << vec.capacity() << std::endl;
    
    // Remove most elements
    vec.resize(10);
    std::cout << "After resizing to 10:" << std::endl;
    std::cout << "Size: " << vec.size() << ", Capacity: " << vec.capacity() << std::endl;
    
    // Shrink to fit to reclaim memory
    vec.shrink_to_fit();
    std::cout << "After shrink_to_fit:" << std::endl;
    std::cout << "Size: " << vec.size() << ", Capacity: " << vec.capacity() << std::endl;
    
    // Alternative: swap with temporary vector
    std::vector<int> vec2;
    for (int i = 0; i < 1000; i++) {
        vec2.push_back(i);
    }
    vec2.resize(10);
    
    std::cout << "\nBefore swap trick:" << std::endl;
    std::cout << "Size: " << vec2.size() << ", Capacity: " << vec2.capacity() << std::endl;
    
    std::vector<int>(vec2).swap(vec2);  // Swap with temporary vector
    std::cout << "After swap trick:" << std::endl;
    std::cout << "Size: " << vec2.size() << ", Capacity: " << vec2.capacity() << std::endl;
}

int main() {
    performanceComparison();
    memoryEfficiency();
    return 0;
}
```

## Advanced Vector Techniques

### 2D Vector (Vector of Vectors)

```cpp
#include <iostream>
#include <vector>
#include <iomanip>

class Matrix {
private:
    std::vector<std::vector<double>> data;
    size_t rows, cols;
    
public:
    Matrix(size_t r, size_t c, double initValue = 0.0) : rows(r), cols(c) {
        data.resize(rows);
        for (auto& row : data) {
            row.resize(cols, initValue);
        }
    }
    
    // Element access
    std::vector<double>& operator[](size_t row) {
        return data[row];
    }
    
    const std::vector<double>& operator[](size_t row) const {
        return data[row];
    }
    
    // Safe element access
    double& at(size_t row, size_t col) {
        return data.at(row).at(col);
    }
    
    const double& at(size_t row, size_t col) const {
        return data.at(row).at(col);
    }
    
    // Dimensions
    size_t getRows() const { return rows; }
    size_t getCols() const { return cols; }
    
    // Matrix operations
    Matrix operator+(const Matrix& other) const {
        if (rows != other.rows || cols != other.cols) {
            throw std::invalid_argument("Matrix dimensions must match");
        }
        
        Matrix result(rows, cols);
        for (size_t i = 0; i < rows; i++) {
            for (size_t j = 0; j < cols; j++) {
                result[i][j] = data[i][j] + other[i][j];
            }
        }
        return result;
    }
    
    Matrix operator*(double scalar) const {
        Matrix result(rows, cols);
        for (size_t i = 0; i < rows; i++) {
            for (size_t j = 0; j < cols; j++) {
                result[i][j] = data[i][j] * scalar;
            }
        }
        return result;
    }
    
    void print() const {
        for (size_t i = 0; i < rows; i++) {
            for (size_t j = 0; j < cols; j++) {
                std::cout << std::setw(8) << std::fixed << std::setprecision(2) 
                         << data[i][j] << " ";
            }
            std::cout << std::endl;
        }
    }
    
    // Resize matrix
    void resize(size_t newRows, size_t newCols, double fillValue = 0.0) {
        data.resize(newRows);
        for (auto& row : data) {
            row.resize(newCols, fillValue);
        }
        rows = newRows;
        cols = newCols;
    }
};

int main() {
    // Create and initialize matrices
    Matrix m1(3, 3);
    Matrix m2(3, 3, 1.0);
    
    // Fill m1 with values
    for (size_t i = 0; i < m1.getRows(); i++) {
        for (size_t j = 0; j < m1.getCols(); j++) {
            m1[i][j] = i * 3 + j + 1;
        }
    }
    
    std::cout << "Matrix 1:" << std::endl;
    m1.print();
    
    std::cout << "\nMatrix 2:" << std::endl;
    m2.print();
    
    // Matrix operations
    Matrix sum = m1 + m2;
    std::cout << "\nMatrix 1 + Matrix 2:" << std::endl;
    sum.print();
    
    Matrix scaled = m1 * 2.5;
    std::cout << "\nMatrix 1 * 2.5:" << std::endl;
    scaled.print();
    
    // Resize matrix
    m1.resize(4, 4, 99.0);
    std::cout << "\nMatrix 1 after resize to 4x4:" << std::endl;
    m1.print();
    
    return 0;
}
```

### Vector as a Buffer

```cpp
#include <iostream>
#include <vector>
#include <fstream>
#include <string>

class CircularBuffer {
private:
    std::vector<int> buffer;
    size_t head, tail, count, capacity;
    
public:
    CircularBuffer(size_t size) : buffer(size), head(0), tail(0), count(0), capacity(size) {}
    
    bool push(int value) {
        if (count == capacity) {
            return false;  // Buffer full
        }
        
        buffer[tail] = value;
        tail = (tail + 1) % capacity;
        count++;
        return true;
    }
    
    bool pop(int& value) {
        if (count == 0) {
            return false;  // Buffer empty
        }
        
        value = buffer[head];
        head = (head + 1) % capacity;
        count--;
        return true;
    }
    
    bool empty() const { return count == 0; }
    bool full() const { return count == capacity; }
    size_t size() const { return count; }
    
    void print() const {
        std::cout << "Buffer contents: ";
        size_t current = head;
        for (size_t i = 0; i < count; i++) {
            std::cout << buffer[current] << " ";
            current = (current + 1) % capacity;
        }
        std::cout << "(size: " << count << "/" << capacity << ")" << std::endl;
    }
};

// File I/O with vector buffer
void fileBufferExample() {
    const size_t BUFFER_SIZE = 4096;
    std::vector<char> buffer(BUFFER_SIZE);
    
    // Write data to file using vector as buffer
    {
        std::ofstream file("test.txt", std::ios::binary);
        std::string data = "This is test data for vector buffer example. ";
        
        // Repeat data to fill buffer
        std::string fullData;
        while (fullData.size() < BUFFER_SIZE) {
            fullData += data;
        }
        fullData.resize(BUFFER_SIZE);
        
        // Copy to buffer and write
        std::copy(fullData.begin(), fullData.end(), buffer.begin());
        file.write(buffer.data(), buffer.size());
    }
    
    // Read data from file using vector as buffer
    {
        std::ifstream file("test.txt", std::ios::binary);
        file.read(buffer.data(), buffer.size());
        
        std::streamsize bytesRead = file.gcount();
        std::cout << "Read " << bytesRead << " bytes from file" << std::endl;
        
        // Display first 100 characters
        std::string content(buffer.begin(), buffer.begin() + std::min(size_t(100), size_t(bytesRead)));
        std::cout << "Content preview: " << content << "..." << std::endl;
    }
}

int main() {
    std::cout << "=== Circular Buffer Example ===" << std::endl;
    
    CircularBuffer cb(5);
    
    // Fill buffer
    for (int i = 1; i <= 5; i++) {
        cb.push(i * 10);
    }
    cb.print();
    
    // Try to add when full
    if (!cb.push(60)) {
        std::cout << "Buffer is full, cannot add 60" << std::endl;
    }
    
    // Remove some elements
    int value;
    cb.pop(value);
    std::cout << "Popped: " << value << std::endl;
    cb.print();
    
    cb.pop(value);
    std::cout << "Popped: " << value << std::endl;
    cb.print();
    
    // Add new elements
    cb.push(60);
    cb.push(70);
    cb.print();
    
    std::cout << "\n=== File Buffer Example ===" << std::endl;
    fileBufferExample();
    
    return 0;
}
```

## Common Vector Mistakes

### Iterator Invalidation

```cpp
#include <iostream>
#include <vector>

void iteratorInvalidationDemo() {
    std::vector<int> vec = {1, 2, 3, 4, 5};
    
    std::cout << "=== Iterator Invalidation Example ===" << std::endl;
    
    // ✘ Dangerous: iterator invalidation
    std::cout << "Attempting to remove even numbers (dangerous way):" << std::endl;
    for (auto it = vec.begin(); it != vec.end(); ++it) {
        if (*it % 2 == 0) {
            vec.erase(it);  // ✘ This invalidates the iterator!
            // ++it would now access invalid memory
        }
    }
    
    // Reset vector
    vec = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    
    // → Safe way: erase returns next valid iterator
    std::cout << "Removing even numbers (safe way):" << std::endl;
    for (auto it = vec.begin(); it != vec.end(); ) {
        if (*it % 2 == 0) {
            it = vec.erase(it);  // → erase returns next valid iterator
        } else {
            ++it;
        }
    }
    
    std::cout << "Remaining elements: ";
    for (const auto& element : vec) {
        std::cout << element << " ";
    }
    std::cout << std::endl;
    
    // → Alternative: use remove_if algorithm
    vec = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    auto newEnd = std::remove_if(vec.begin(), vec.end(), [](int n) {
        return n % 2 == 0;
    });
    vec.erase(newEnd, vec.end());
    
    std::cout << "After remove_if: ";
    for (const auto& element : vec) {
        std::cout << element << " ";
    }
    std::cout << std::endl;
}
```

### Memory and Performance Pitfalls

```cpp
#include <iostream>
#include <vector>

void memoryPitfalls() {
    std::cout << "=== Memory Pitfalls ===" << std::endl;
    
    // Pitfall 1: Not reserving capacity
    std::vector<int> vec1;
    std::cout << "Adding elements without reserve:" << std::endl;
    for (int i = 0; i < 10; i++) {
        vec1.push_back(i);
        std::cout << "Size: " << vec1.size() << ", Capacity: " << vec1.capacity() << std::endl;
    }
    
    // Pitfall 2: Excessive capacity after clear
    std::vector<int> vec2;
    vec2.reserve(1000);
    for (int i = 0; i < 1000; i++) {
        vec2.push_back(i);
    }
    
    std::cout << "\nBefore clear - Size: " << vec2.size() << ", Capacity: " << vec2.capacity() << std::endl;
    vec2.clear();
    std::cout << "After clear - Size: " << vec2.size() << ", Capacity: " << vec2.capacity() << std::endl;
    
    // Solution: use shrink_to_fit or swap trick
    vec2.shrink_to_fit();
    std::cout << "After shrink_to_fit - Size: " << vec2.size() << ", Capacity: " << vec2.capacity() << std::endl;
}
```

## Exercises

### Exercise 1: Dynamic Array Implementation

Create your own simplified dynamic array class:

- Implement basic operations (push_back, pop_back, insert, erase)
- Add capacity management and automatic resizing
- Include iterator support

### Exercise 2: Vector Algorithms

Implement useful algorithms for vectors:

- Binary search in sorted vector
- Merge two sorted vectors
- Find all duplicates in a vector
- Implement quicksort using vectors

### Exercise 3: Data Processing

Use vectors for data processing tasks:

- Read CSV data into vector of structs
- Implement statistical functions (mean, median, mode)
- Create histogram from data
- Filter and transform data using STL algorithms

### Exercise 4: Game Development

Use vectors in game programming:

- Particle system with vector of particles
- Game object management
- Collision detection with spatial partitioning
- Level data representation

## Key Takeaways

<i class="fa-solid fa-arrow-right"></i> **`std::vector` is the default container choice** for sequential data  
<i class="fa-solid fa-arrow-right"></i> **Reserve capacity when size is known** to avoid reallocations  
<i class="fa-solid fa-arrow-right"></i> **Use `emplace_back` for in-place construction** when possible  
<i class="fa-solid fa-arrow-right"></i> **Be aware of iterator invalidation** during modifications  
<i class="fa-solid fa-arrow-right"></i> **Vectors provide excellent cache performance** due to contiguous memory  
<i class="fa-solid fa-arrow-right"></i> **Use algorithms instead of raw loops** for better performance and safety

---

**Next**: Learn about specialized text containers // →  [Strings and Text Processing](strings.md)
