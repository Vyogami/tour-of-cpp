# Arrays and C-style Arrays

Arrays are the most fundamental container type, providing sequential storage for elements of the same type. C++ supports both traditional C-style arrays and modern `std::array` containers. Understanding arrays is essential as they form the foundation for more complex data structures.

## C-Style Arrays

C-style arrays are inherited from the C language and provide basic sequential storage:

### Declaration and Initialization

```cpp
#include <iostream>

int main() {
    // Declaration with size
    int numbers[5];                    // Uninitialized array of 5 ints
    
    // Declaration with initialization
    int scores[5] = {85, 92, 78, 96, 88};
    
    // Partial initialization (remaining elements are zero)
    int grades[5] = {90, 85};          // {90, 85, 0, 0, 0}
    
    // Zero initialization
    int zeros[5] = {};                 // All elements are 0
    int moreZeros[5] = {0};           // Also all elements are 0
    
    // Size deduction from initializer
    int data[] = {1, 2, 3, 4, 5};     // Size automatically set to 5
    
    // Character arrays (strings)
    char name[10] = "Alice";           // {'A', 'l', 'i', 'c', 'e', '\0', 0, 0, 0, 0}
    char greeting[] = "Hello";         // Size automatically set to 6 (including '\0')
    
    std::cout << "Array contents:" << std::endl;
    for (int i = 0; i < 5; i++) {
        std::cout << "scores[" << i << "] = " << scores[i] << std::endl;
    }
    
    return 0;
}
```

### Array Access and Modification

```cpp
#include <iostream>

int main() {
    int values[5] = {10, 20, 30, 40, 50};
    
    // Access elements using subscript operator
    std::cout << "First element: " << values[0] << std::endl;
    std::cout << "Last element: " << values[4] << std::endl;
    
    // Modify elements
    values[2] = 99;
    std::cout << "Modified third element: " << values[2] << std::endl;
    
    // No bounds checking - dangerous!
    // values[10] = 123;  // Undefined behavior - writes outside array bounds
    // int bad = values[10]; // Undefined behavior - reads garbage
    
    // Safe iteration with known size
    const int SIZE = 5;
    for (int i = 0; i < SIZE; i++) {
        values[i] *= 2;  // Double each element
    }
    
    std::cout << "After doubling:" << std::endl;
    for (int i = 0; i < SIZE; i++) {
        std::cout << values[i] << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

## Array Size and Limitations

### The `sizeof` Trick

```cpp
#include <iostream>

void printArrayInfo(int arr[]) {
    // ✘ This doesn't work - arr is actually a pointer parameter
    // int size = sizeof(arr) / sizeof(arr[0]);  // Wrong! sizeof(arr) is sizeof(int*)
    std::cout << "Inside function, sizeof(arr): " << sizeof(arr) << std::endl;
}

int main() {
    int numbers[10] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    
    // → This works in the same scope where array is declared
    int size = sizeof(numbers) / sizeof(numbers[0]);
    std::cout << "Array size: " << size << std::endl;
    std::cout << "sizeof(numbers): " << sizeof(numbers) << std::endl;
    std::cout << "sizeof(numbers[0]): " << sizeof(numbers[0]) << std::endl;
    
    printArrayInfo(numbers);  // Array decays to pointer when passed
    
    return 0;
}
```

### Passing Arrays to Functions

```cpp
#include <iostream>

// Method 1: Array parameter (actually a pointer)
void printArray1(int arr[], int size) {
    for (int i = 0; i < size; i++) {
        std::cout << arr[i] << " ";
    }
    std::cout << std::endl;
}

// Method 2: Pointer parameter (equivalent to above)
void printArray2(int* arr, int size) {
    for (int i = 0; i < size; i++) {
        std::cout << arr[i] << " ";
    }
    std::cout << std::endl;
}

// Method 3: Reference to array (preserves size information)
template<size_t N>
void printArray3(int (&arr)[N]) {
    std::cout << "Array size in function: " << N << std::endl;
    for (size_t i = 0; i < N; i++) {
        std::cout << arr[i] << " ";
    }
    std::cout << std::endl;
}

// Method 4: Using const for read-only access
void printArrayConst(const int arr[], int size) {
    for (int i = 0; i < size; i++) {
        std::cout << arr[i] << " ";
        // arr[i] = 0;  // ✘ Error: cannot modify const array
    }
    std::cout << std::endl;
}

int main() {
    int numbers[5] = {1, 2, 3, 4, 5};
    
    std::cout << "Method 1 (array parameter): ";
    printArray1(numbers, 5);
    
    std::cout << "Method 2 (pointer parameter): ";
    printArray2(numbers, 5);
    
    std::cout << "Method 3 (reference to array): ";
    printArray3(numbers);  // Size automatically deduced
    
    std::cout << "Method 4 (const array): ";
    printArrayConst(numbers, 5);
    
    return 0;
}
```

## Multi-dimensional Arrays

### 2D Arrays

```cpp
#include <iostream>

int main() {
    // 2D array declaration and initialization
    int matrix[3][4] = {
        {1, 2, 3, 4},
        {5, 6, 7, 8},
        {9, 10, 11, 12}
    };
    
    // Alternative initialization styles
    int matrix2[3][4] = {{1, 2}, {5, 6, 7}, {9}};  // Partial initialization
    int matrix3[3][4] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12};  // Linear
    
    // Access elements
    std::cout << "Element at [1][2]: " << matrix[1][2] << std::endl;  // 7
    
    // Iterate through 2D array
    std::cout << "Matrix contents:" << std::endl;
    for (int row = 0; row < 3; row++) {
        for (int col = 0; col < 4; col++) {
            std::cout << matrix[row][col] << "\t";
        }
        std::cout << std::endl;
    }
    
    // Modify elements
    matrix[1][1] = 99;
    std::cout << "After modification: matrix[1][1] = " << matrix[1][1] << std::endl;
    
    return 0;
}
```

### Passing 2D Arrays to Functions

```cpp
#include <iostream>

// Method 1: Specify all dimensions except the first
void print2DArray1(int arr[][4], int rows) {
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < 4; j++) {
            std::cout << arr[i][j] << " ";
        }
        std::cout << std::endl;
    }
}

// Method 2: Use template for both dimensions
template<int ROWS, int COLS>
void print2DArray2(int (&arr)[ROWS][COLS]) {
    std::cout << "Array dimensions: " << ROWS << "x" << COLS << std::endl;
    for (int i = 0; i < ROWS; i++) {
        for (int j = 0; j < COLS; j++) {
            std::cout << arr[i][j] << " ";
        }
        std::cout << std::endl;
    }
}

// Method 3: Treat as 1D array with manual indexing
void print2DArrayFlat(int* arr, int rows, int cols) {
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            std::cout << arr[i * cols + j] << " ";  // Manual 2D to 1D conversion
        }
        std::cout << std::endl;
    }
}

int main() {
    int matrix[3][4] = {
        {1, 2, 3, 4},
        {5, 6, 7, 8},
        {9, 10, 11, 12}
    };
    
    std::cout << "Method 1:" << std::endl;
    print2DArray1(matrix, 3);
    
    std::cout << "Method 2:" << std::endl;
    print2DArray2(matrix);
    
    std::cout << "Method 3:" << std::endl;
    print2DArrayFlat(&matrix[0][0], 3, 4);
    
    return 0;
}
```

## Array and Pointer Relationship

### Array Name as Pointer

```cpp
#include <iostream>

int main() {
    int numbers[5] = {10, 20, 30, 40, 50};
    
    // Array name behaves like a pointer to the first element
    std::cout << "numbers: " << numbers << std::endl;           // Address of first element
    std::cout << "&numbers[0]: " << &numbers[0] << std::endl;   // Same address
    
    // Pointer arithmetic with array names
    std::cout << "*numbers: " << *numbers << std::endl;        // 10 (first element)
    std::cout << "*(numbers + 1): " << *(numbers + 1) << std::endl;  // 20 (second element)
    std::cout << "*(numbers + 4): " << *(numbers + 4) << std::endl;  // 50 (last element)
    
    // Array subscript is equivalent to pointer arithmetic
    std::cout << "numbers[2]: " << numbers[2] << std::endl;          // 30
    std::cout << "*(numbers + 2): " << *(numbers + 2) << std::endl;  // 30 (same thing)
    
    // You can use pointers to iterate through arrays
    int* ptr = numbers;
    std::cout << "Iterating with pointer: ";
    for (int i = 0; i < 5; i++) {
        std::cout << *ptr << " ";
        ptr++;  // Move to next element
    }
    std::cout << std::endl;
    
    return 0;
}
```

### Pointer Arithmetic

```cpp
#include <iostream>

int main() {
    int data[6] = {100, 200, 300, 400, 500, 600};
    int* start = data;
    int* end = data + 6;  // Points one past the last element
    
    std::cout << "Forward iteration:" << std::endl;
    for (int* ptr = start; ptr < end; ptr++) {
        std::cout << *ptr << " ";
    }
    std::cout << std::endl;
    
    std::cout << "Backward iteration:" << std::endl;
    for (int* ptr = end - 1; ptr >= start; ptr--) {
        std::cout << *ptr << " ";
    }
    std::cout << std::endl;
    
    // Calculate distance between pointers
    std::cout << "Distance from start to end: " << (end - start) << " elements" << std::endl;
    
    // Find specific element using pointers
    int* found = nullptr;
    for (int* ptr = start; ptr < end; ptr++) {
        if (*ptr == 400) {
            found = ptr;
            break;
        }
    }
    
    if (found) {
        std::cout << "Found 400 at index: " << (found - start) << std::endl;
    }
    
    return 0;
}
```

## Modern Alternative: `std::array`

C++11 introduced `std::array` as a safer alternative to C-style arrays:

### Basic `std::array` Usage

```cpp
#include <iostream>
#include <array>

int main() {
    // Declaration and initialization
    std::array<int, 5> numbers = {10, 20, 30, 40, 50};
    
    // Alternative initialization
    std::array<int, 5> zeros = {};           // All elements zero
    std::array<int, 5> filled;
    filled.fill(42);                         // Fill all elements with 42
    
    // Size information is preserved
    std::cout << "Array size: " << numbers.size() << std::endl;
    std::cout << "Max size: " << numbers.max_size() << std::endl;
    std::cout << "Is empty: " << std::boolalpha << numbers.empty() << std::endl;
    
    // Safe element access
    std::cout << "First element: " << numbers.front() << std::endl;
    std::cout << "Last element: " << numbers.back() << std::endl;
    std::cout << "Element at index 2: " << numbers.at(2) << std::endl;  // Bounds checking
    
    // Unsafe but fast access (like C-style arrays)
    std::cout << "Element at index 3: " << numbers[3] << std::endl;
    
    // Iterator support
    std::cout << "Using iterators: ";
    for (auto it = numbers.begin(); it != numbers.end(); ++it) {
        std::cout << *it << " ";
    }
    std::cout << std::endl;
    
    // Range-based for loop
    std::cout << "Range-based for: ";
    for (const auto& element : numbers) {
        std::cout << element << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

### `std::array` vs C-style Arrays

```cpp
#include <iostream>
#include <array>
#include <algorithm>

// Function accepting std::array by reference
void printStdArray(const std::array<int, 5>& arr) {
    std::cout << "std::array size: " << arr.size() << std::endl;
    for (const auto& element : arr) {
        std::cout << element << " ";
    }
    std::cout << std::endl;
}

// Function accepting C-style array
void printCArray(const int arr[], int size) {
    std::cout << "C-array (size passed separately): " << size << std::endl;
    for (int i = 0; i < size; i++) {
        std::cout << arr[i] << " ";
    }
    std::cout << std::endl;
}

int main() {
    // C-style array
    int cArray[5] = {1, 2, 3, 4, 5};
    
    // std::array
    std::array<int, 5> stdArray = {1, 2, 3, 4, 5};
    
    std::cout << "=== Comparison ===" << std::endl;
    
    // Size information
    // int cSize = sizeof(cArray) / sizeof(cArray[0]);  // Only works in same scope
    std::cout << "C-array 'size': " << 5 << " (hardcoded)" << std::endl;
    std::cout << "std::array size: " << stdArray.size() << std::endl;
    
    // Function calls
    printCArray(cArray, 5);
    printStdArray(stdArray);
    
    // STL algorithm compatibility
    std::cout << "=== STL Algorithms ===" << std::endl;
    
    // std::array works seamlessly with STL algorithms
    std::sort(stdArray.begin(), stdArray.end(), std::greater<int>());
    std::cout << "Sorted std::array (descending): ";
    for (const auto& element : stdArray) {
        std::cout << element << " ";
    }
    std::cout << std::endl;
    
    // C-array also works, but requires manual size management
    std::sort(cArray, cArray + 5, std::greater<int>());
    std::cout << "Sorted C-array (descending): ";
    for (int i = 0; i < 5; i++) {
        std::cout << cArray[i] << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

## Practical Array Examples

### Array-based Stack Implementation

```cpp
#include <iostream>
#include <stdexcept>

template<typename T, size_t N>
class ArrayStack {
private:
    std::array<T, N> data;
    size_t topIndex;
    
public:
    ArrayStack() : topIndex(0) {}
    
    void push(const T& value) {
        if (topIndex >= N) {
            throw std::overflow_error("Stack overflow");
        }
        data[topIndex++] = value;
    }
    
    T pop() {
        if (topIndex == 0) {
            throw std::underflow_error("Stack underflow");
        }
        return data[--topIndex];
    }
    
    const T& top() const {
        if (topIndex == 0) {
            throw std::underflow_error("Stack is empty");
        }
        return data[topIndex - 1];
    }
    
    bool empty() const {
        return topIndex == 0;
    }
    
    size_t size() const {
        return topIndex;
    }
    
    size_t capacity() const {
        return N;
    }
    
    void print() const {
        std::cout << "Stack (bottom to top): ";
        for (size_t i = 0; i < topIndex; i++) {
            std::cout << data[i] << " ";
        }
        std::cout << std::endl;
    }
};

int main() {
    ArrayStack<int, 10> stack;
    
    // Push some values
    for (int i = 1; i <= 5; i++) {
        stack.push(i * 10);
    }
    
    stack.print();
    std::cout << "Stack size: " << stack.size() << "/" << stack.capacity() << std::endl;
    std::cout << "Top element: " << stack.top() << std::endl;
    
    // Pop some values
    std::cout << "Popping: ";
    while (!stack.empty()) {
        std::cout << stack.pop() << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

### Matrix Operations with 2D Arrays

```cpp
#include <iostream>
#include <array>
#include <iomanip>

template<size_t ROWS, size_t COLS>
class Matrix {
private:
    std::array<std::array<double, COLS>, ROWS> data;
    
public:
    Matrix() {
        // Initialize to zero
        for (auto& row : data) {
            row.fill(0.0);
        }
    }
    
    // Element access
    std::array<double, COLS>& operator[](size_t row) {
        return data[row];
    }
    
    const std::array<double, COLS>& operator[](size_t row) const {
        return data[row];
    }
    
    // Matrix addition
    Matrix operator+(const Matrix& other) const {
        Matrix result;
        for (size_t i = 0; i < ROWS; i++) {
            for (size_t j = 0; j < COLS; j++) {
                result[i][j] = data[i][j] + other[i][j];
            }
        }
        return result;
    }
    
    // Scalar multiplication
    Matrix operator*(double scalar) const {
        Matrix result;
        for (size_t i = 0; i < ROWS; i++) {
            for (size_t j = 0; j < COLS; j++) {
                result[i][j] = data[i][j] * scalar;
            }
        }
        return result;
    }
    
    void print() const {
        for (size_t i = 0; i < ROWS; i++) {
            for (size_t j = 0; j < COLS; j++) {
                std::cout << std::setw(8) << std::fixed << std::setprecision(2) 
                         << data[i][j] << " ";
            }
            std::cout << std::endl;
        }
    }
    
    constexpr size_t rows() const { return ROWS; }
    constexpr size_t cols() const { return COLS; }
};

int main() {
    Matrix<3, 3> matrix1;
    Matrix<3, 3> matrix2;
    
    // Initialize matrices
    for (size_t i = 0; i < 3; i++) {
        for (size_t j = 0; j < 3; j++) {
            matrix1[i][j] = i * 3 + j + 1;
            matrix2[i][j] = (i + j) * 0.5;
        }
    }
    
    std::cout << "Matrix 1:" << std::endl;
    matrix1.print();
    
    std::cout << "\nMatrix 2:" << std::endl;
    matrix2.print();
    
    std::cout << "\nMatrix 1 + Matrix 2:" << std::endl;
    (matrix1 + matrix2).print();
    
    std::cout << "\nMatrix 1 * 2.5:" << std::endl;
    (matrix1 * 2.5).print();
    
    return 0;
}
```

## Common Array Pitfalls

### Array Decay

```cpp
#include <iostream>

void arrayDecayDemo(int arr[]) {
    // arr is actually int* here, not int[]
    std::cout << "In function, sizeof(arr): " << sizeof(arr) << std::endl;  // Size of pointer
}

template<size_t N>
void noArrayDecay(int (&arr)[N]) {
    // Array reference prevents decay
    std::cout << "With reference, array size: " << N << std::endl;
    std::cout << "sizeof(arr): " << sizeof(arr) << std::endl;  // Size of actual array
}

int main() {
    int numbers[10] = {0};
    
    std::cout << "In main, sizeof(numbers): " << sizeof(numbers) << std::endl;
    
    arrayDecayDemo(numbers);    // Array decays to pointer
    noArrayDecay(numbers);      // Array reference preserves type
    
    return 0;
}
```

### Bounds Checking

```cpp
#include <iostream>
#include <array>

void boundsCheckingDemo() {
    int cArray[5] = {1, 2, 3, 4, 5};
    std::array<int, 5> stdArray = {1, 2, 3, 4, 5};
    
    std::cout << "=== C-style array ===" << std::endl;
    // No bounds checking - undefined behavior
    // std::cout << cArray[10] << std::endl;  // Dangerous!
    
    std::cout << "=== std::array ===" << std::endl;
    try {
        // Bounds checking with at()
        std::cout << "stdArray.at(4): " << stdArray.at(4) << std::endl;  // OK
        std::cout << "stdArray.at(10): " << stdArray.at(10) << std::endl;  // Throws exception
    } catch (const std::out_of_range& e) {
        std::cout << "Exception caught: " << e.what() << std::endl;
    }
    
    // operator[] still has no bounds checking for performance
    // std::cout << stdArray[10] << std::endl;  // Still dangerous!
}
```

## Performance Considerations

### Cache Efficiency

```cpp
#include <iostream>
#include <chrono>
#include <array>

void cacheEfficiencyDemo() {
    constexpr size_t SIZE = 1000;
    std::array<std::array<int, SIZE>, SIZE> matrix;
    
    // Initialize matrix
    for (size_t i = 0; i < SIZE; i++) {
        for (size_t j = 0; j < SIZE; j++) {
            matrix[i][j] = i * SIZE + j;
        }
    }
    
    // Row-major access (cache-friendly)
    auto start = std::chrono::high_resolution_clock::now();
    long long sum1 = 0;
    for (size_t i = 0; i < SIZE; i++) {
        for (size_t j = 0; j < SIZE; j++) {
            sum1 += matrix[i][j];
        }
    }
    auto rowTime = std::chrono::high_resolution_clock::now() - start;
    
    // Column-major access (cache-unfriendly)
    start = std::chrono::high_resolution_clock::now();
    long long sum2 = 0;
    for (size_t j = 0; j < SIZE; j++) {
        for (size_t i = 0; i < SIZE; i++) {
            sum2 += matrix[i][j];
        }
    }
    auto colTime = std::chrono::high_resolution_clock::now() - start;
    
    std::cout << "Row-major sum: " << sum1 
              << " Time: " << std::chrono::duration_cast<std::chrono::milliseconds>(rowTime).count() << "ms" << std::endl;
    std::cout << "Column-major sum: " << sum2 
              << " Time: " << std::chrono::duration_cast<std::chrono::milliseconds>(colTime).count() << "ms" << std::endl;
}
```

## Exercises

### Exercise 1: Array Utilities

Implement utility functions for arrays:

- Find minimum and maximum elements
- Reverse an array in-place
- Check if an array is sorted
- Rotate array elements

### Exercise 2: 2D Array Games

Create simple games using 2D arrays:

- Tic-tac-toe board
- Conway's Game of Life
- Simple maze representation

### Exercise 3: Array-based Data Structures

Implement data structures using arrays:

- Circular buffer/queue
- Priority queue with binary heap
- Hash table with linear probing

### Exercise 4: Performance Analysis

Compare performance of:

- C-style arrays vs std::array
- Different access patterns in multi-dimensional arrays
- Array-based vs pointer-based implementations

## Key Takeaways

<i class="fa-solid fa-arrow-right"></i> **C-style arrays are fast but dangerous** - no bounds checking, decay to pointers  
<i class="fa-solid fa-arrow-right"></i> **`std::array` provides safety** - size information preserved, bounds checking available  
<i class="fa-solid fa-arrow-right"></i> **Arrays provide contiguous memory** - excellent cache performance  
<i class="fa-solid fa-arrow-right"></i> **Array name decays to pointer** when passed to functions  
<i class="fa-solid fa-arrow-right"></i> **Multi-dimensional arrays** are arrays of arrays  
<i class="fa-solid fa-arrow-right"></i> **Prefer `std::array` over C-style arrays** in modern C++

---

**Next**: Learn about dynamic arrays with automatic memory management // →  [Vectors and Dynamic Arrays](vectors.md)
