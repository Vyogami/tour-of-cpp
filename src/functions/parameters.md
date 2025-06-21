# Parameters and Arguments

Parameters and arguments are how functions communicate with the outside world. Understanding the different ways to pass data to functions is crucial for writing efficient and correct C++ programs.

## Parameters vs Arguments

- **Parameter**: Variable in the function definition
- **Argument**: Actual value passed when calling the function

```cpp
#include <iostream>

// 'a' and 'b' are parameters
int add(int a, int b) {
    return a + b;
}

int main() {
    int x = 5, y = 10;
    int result = add(x, y);  // 'x' and 'y' are arguments
    // Can also pass literals as arguments
    int result2 = add(15, 25);  // '15' and '25' are arguments
    
    return 0;
}
```

## Pass by Value (Default)

By default, C++ passes arguments by value, meaning a copy is made:

### Basic Pass by Value

```cpp
#include <iostream>

void modifyValue(int num) {
    std::cout << "Inside function, before: " << num << std::endl;
    num = 100;  // Only modifies the local copy
    std::cout << "Inside function, after: " << num << std::endl;
}

int main() {
    int original = 42;
    
    std::cout << "Before function call: " << original << std::endl;
    modifyValue(original);
    std::cout << "After function call: " << original << std::endl;
    
    return 0;
}
```

**Output:**

```
Before function call: 42
Inside function, before: 42
Inside function, after: 100
After function call: 42
```

### Why Pass by Value is Safe

```cpp
#include <iostream>

int square(int number) {
    number = number * number;  // Modify the copy
    return number;
}

int main() {
    int value = 5;
    int squared = square(value);
    
    std::cout << "Original value: " << value << std::endl;    // Still 5
    std::cout << "Squared value: " << squared << std::endl;   // 25
    
    return 0;
}
```

## Pass by Reference

Use references (`&`) when you want to modify the original variable:

### Basic Pass by Reference

```cpp
#include <iostream>

void modifyByReference(int& num) {  // Note the &
    std::cout << "Inside function, before: " << num << std::endl;
    num = 100;  // Modifies the original variable
    std::cout << "Inside function, after: " << num << std::endl;
}

int main() {
    int original = 42;
    
    std::cout << "Before function call: " << original << std::endl;
    modifyByReference(original);
    std::cout << "After function call: " << original << std::endl;
    
    return 0;
}
```

**Output:**

```
Before function call: 42
Inside function, before: 42
Inside function, after: 100
After function call: 100
```

### Multiple Return Values Using References

```cpp
#include <iostream>

void divideWithRemainder(int dividend, int divisor, int& quotient, int& remainder) {
    quotient = dividend / divisor;
    remainder = dividend % divisor;
}

void getMinMax(int a, int b, int c, int& min, int& max) {
    min = max = a;
    if (b < min) min = b;
    if (c < min) min = c;
    if (b > max) max = b;
    if (c > max) max = c;
}

int main() {
    int quot, rem;
    divideWithRemainder(17, 5, quot, rem);
    std::cout << "17 ÷ 5 = " << quot << " remainder " << rem << std::endl;
    
    int minimum, maximum;
    getMinMax(15, 8, 23, minimum, maximum);
    std::cout << "Min: " << minimum << ", Max: " << maximum << std::endl;
    
    return 0;
}
```

### Swapping Values

```cpp
#include <iostream>

void swap(int& a, int& b) {
    int temp = a;
    a = b;
    b = temp;
}

void bubbleSort(int arr[], int size) {
    for (int i = 0; i < size - 1; i++) {
        for (int j = 0; j < size - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {
                swap(arr[j], arr[j + 1]);
            }
        }
    }
}

int main() {
    int x = 10, y = 20;
    std::cout << "Before swap: x=" << x << ", y=" << y << std::endl;
    swap(x, y);
    std::cout << "After swap: x=" << x << ", y=" << y << std::endl;
    
    int numbers[] = {64, 34, 25, 12, 22, 11, 90};
    int size = sizeof(numbers) / sizeof(numbers[0]);
    
    std::cout << "Before sorting: ";
    for (int i = 0; i < size; i++) {
        std::cout << numbers[i] << " ";
    }
    std::cout << std::endl;
    
    bubbleSort(numbers, size);
    
    std::cout << "After sorting: ";
    for (int i = 0; i < size; i++) {
        std::cout << numbers[i] << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

## Const Parameters

Use `const` to prevent modification while avoiding unnecessary copies:

### Const Value Parameters

```cpp
#include <iostream>

void printNumber(const int num) {
    std::cout << "Number: " << num << std::endl;
    // num = 50;  // ✘ Error: cannot modify const parameter
}

int calculateArea(const int length, const int width) {
    // length = 10;  // ✘ Error: cannot modify const parameters
    return length * width;
}

int main() {
    printNumber(42);
    int area = calculateArea(5, 8);
    std::cout << "Area: " << area << std::endl;
    
    return 0;
}
```

### Const Reference Parameters

```cpp
#include <iostream>
#include <string>

// Efficient: no copy made, but cannot modify the string
void printMessage(const std::string& message) {
    std::cout << "Message: " << message << std::endl;
    // message += "!";  // ✘ Error: cannot modify const reference
}

// Calculate without modifying the original values
double calculateDistance(const double& x1, const double& y1, 
                        const double& x2, const double& y2) {
    double dx = x2 - x1;
    double dy = y2 - y1;
    return std::sqrt(dx * dx + dy * dy);
}

int main() {
    std::string greeting = "Hello, World!";
    printMessage(greeting);
    
    double distance = calculateDistance(0.0, 0.0, 3.0, 4.0);
    std::cout << "Distance: " << distance << std::endl;
    
    return 0;
}
```

## Arrays as Parameters

Arrays are passed by reference automatically (they decay to pointers):

### Basic Array Parameters

```cpp
#include <iostream>

// Arrays are automatically passed by reference
void printArray(int arr[], int size) {
    for (int i = 0; i < size; i++) {
        std::cout << arr[i] << " ";
    }
    std::cout << std::endl;
}

void modifyArray(int arr[], int size) {
    for (int i = 0; i < size; i++) {
        arr[i] *= 2;  // Modifies the original array
    }
}

int sumArray(const int arr[], int size) {  // const prevents modification
    int sum = 0;
    for (int i = 0; i < size; i++) {
        sum += arr[i];
        // arr[i] = 0;  // ✘ Error: cannot modify const array
    }
    return sum;
}

int main() {
    int numbers[] = {1, 2, 3, 4, 5};
    int size = sizeof(numbers) / sizeof(numbers[0]);
    
    std::cout << "Original array: ";
    printArray(numbers, size);
    
    std::cout << "Sum: " << sumArray(numbers, size) << std::endl;
    
    modifyArray(numbers, size);
    std::cout << "Modified array: ";
    printArray(numbers, size);
    
    return 0;
}
```

### 2D Arrays as Parameters

```cpp
#include <iostream>

// For 2D arrays, you must specify all dimensions except the first
void print2DArray(int arr[][3], int rows) {
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < 3; j++) {
            std::cout << arr[i][j] << " ";
        }
        std::cout << std::endl;
    }
}

int sum2DArray(const int arr[][3], int rows) {
    int sum = 0;
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < 3; j++) {
            sum += arr[i][j];
        }
    }
    return sum;
}

int main() {
    int matrix[2][3] = {
        {1, 2, 3},
        {4, 5, 6}
    };
    
    std::cout << "Matrix:" << std::endl;
    print2DArray(matrix, 2);
    
    std::cout << "Sum: " << sum2DArray(matrix, 2) << std::endl;
    
    return 0;
}
```

## Default Parameters

Provide default values for parameters:

### Basic Default Parameters

```cpp
#include <iostream>

// Default parameters must be at the end
void greetUser(std::string name, std::string greeting = "Hello") {
    std::cout << greeting << ", " << name << "!" << std::endl;
}

double calculateArea(double length, double width = 1.0) {
    return length * width;
}

void printNumber(int num, bool showLabel = true, std::string label = "Number") {
    if (showLabel) {
        std::cout << label << ": ";
    }
    std::cout << num << std::endl;
}

int main() {
    greetUser("Alice");                    // Uses default greeting
    greetUser("Bob", "Hi");               // Uses custom greeting
    
    std::cout << "Square area: " << calculateArea(5) << std::endl;      // width = 1.0
    std::cout << "Rectangle area: " << calculateArea(5, 3) << std::endl; // width = 3
    
    printNumber(42);                      // All defaults
    printNumber(100, false);              // No label
    printNumber(200, true, "Value");      // Custom label
    
    return 0;
}
```

### Default Parameters in Declarations

```cpp
#include <iostream>

// Default values specified in declaration only
void displayInfo(std::string name, int age = 0, bool showAge = true);

int main() {
    displayInfo("Alice");
    displayInfo("Bob", 25);
    displayInfo("Charlie", 30, false);
    
    return 0;
}

// Implementation doesn't repeat default values
void displayInfo(std::string name, int age, bool showAge) {
    std::cout << "Name: " << name;
    if (showAge && age > 0) {
        std::cout << ", Age: " << age;
    }
    std::cout << std::endl;
}
```

## Parameter Validation

Always validate parameters to ensure function correctness:

### Input Validation Examples

```cpp
#include <iostream>
#include <string>

double safeDivide(double numerator, double denominator) {
    if (denominator == 0.0) {
        std::cout << "Error: Division by zero!" << std::endl;
        return 0.0;  // or throw an exception
    }
    return numerator / denominator;
}

int factorial(int n) {
    if (n < 0) {
        std::cout << "Error: Factorial not defined for negative numbers!" << std::endl;
        return -1;  // Error indicator
    }
    if (n == 0 || n == 1) {
        return 1;
    }
    
    int result = 1;
    for (int i = 2; i <= n; i++) {
        result *= i;
    }
    return result;
}

void printSubstring(const std::string& text, int start, int length) {
    if (start < 0 || start >= text.length()) {
        std::cout << "Error: Invalid start position!" << std::endl;
        return;
    }
    
    if (length <= 0) {
        std::cout << "Error: Length must be positive!" << std::endl;
        return;
    }
    
    // Adjust length if it goes beyond string end
    if (start + length > text.length()) {
        length = text.length() - start;
    }
    
    std::cout << text.substr(start, length) << std::endl;
}

int main() {
    std::cout << "10 / 3 = " << safeDivide(10, 3) << std::endl;
    std::cout << "10 / 0 = " << safeDivide(10, 0) << std::endl;
    
    std::cout << "5! = " << factorial(5) << std::endl;
    std::cout << "(-3)! = " << factorial(-3) << std::endl;
    
    std::string text = "Hello, World!";
    printSubstring(text, 7, 5);   // "World"
    printSubstring(text, -1, 5);  // Error
    printSubstring(text, 7, 20);  // "World!" (adjusted length)
    
    return 0;
}
```

## Pass by Pointer

An alternative to references for modifying variables:

### Basic Pointer Parameters

```cpp
#include <iostream>

void modifyByPointer(int* num) {
    if (num != nullptr) {  // Always check for null pointer
        *num = 100;
    }
}

void swap(int* a, int* b) {
    if (a != nullptr && b != nullptr) {
        int temp = *a;
        *a = *b;
        *b = temp;
    }
}

// Function that may fail - returns success/failure
bool divide(int a, int b, int* result) {
    if (b == 0 || result == nullptr) {
        return false;  // Division by zero or invalid pointer
    }
    *result = a / b;
    return true;
}

int main() {
    int value = 42;
    std::cout << "Before: " << value << std::endl;
    modifyByPointer(&value);  // Pass address of variable
    std::cout << "After: " << value << std::endl;
    
    int x = 10, y = 20;
    std::cout << "Before swap: x=" << x << ", y=" << y << std::endl;
    swap(&x, &y);
    std::cout << "After swap: x=" << x << ", y=" << y << std::endl;
    
    int result;
    if (divide(10, 3, &result)) {
        std::cout << "10 / 3 = " << result << std::endl;
    } else {
        std::cout << "Division failed!" << std::endl;
    }
    
    return 0;
}
```

## When to Use Which Parameter Type

### Decision Guide

```cpp
#include <iostream>
#include <string>

// → Pass by value: small types, no modification needed
int add(int a, int b) {
    return a + b;
}

// → Pass by const reference: large types, no modification needed
void printString(const std::string& text) {
    std::cout << text << std::endl;
}

// → Pass by reference: need to modify the original
void increment(int& value) {
    value++;
}

// → Pass by pointer: optional parameter or array
void printOptional(int* value) {
    if (value) {
        std::cout << *value << std::endl;
    } else {
        std::cout << "No value provided" << std::endl;
    }
}

int main() {
    // Small types - pass by value
    int sum = add(5, 3);
    
    // Large types - pass by const reference
    std::string message = "Hello, World!";
    printString(message);
    
    // Need to modify - pass by reference
    int counter = 0;
    increment(counter);
    std::cout << "Counter: " << counter << std::endl;
    
    // Optional parameter - pass by pointer
    int number = 42;
    printOptional(&number);
    printOptional(nullptr);
    
    return 0;
}
```

## Common Mistakes and Best Practices

### <i class="fa-solid fa-square-xmark"></i> Dangling References

```cpp
// ✘ Don't return reference to local variable
int& badFunction() {
    int local = 42;
    return local;  // Dangerous! local is destroyed when function ends
}

// → Return by value instead
int goodFunction() {
    int local = 42;
    return local;  // Safe: value is copied
}
```

### <i class="fa-solid fa-square-xmark"></i> Forgetting to Check Pointers

```cpp
#include <iostream>

// ✘ Dangerous: doesn't check for null pointer
void badPrint(int* value) {
    std::cout << *value << std::endl;  // Crash if value is nullptr!
}

// → Safe: checks for null pointer
void goodPrint(int* value) {
    if (value != nullptr) {
        std::cout << *value << std::endl;
    } else {
        std::cout << "Null pointer!" << std::endl;
    }
}
```

### Parameter Guidelines

```cpp
#include <iostream>
#include <string>
#include <vector>

// → Use const& for expensive-to-copy types
void processData(const std::vector<int>& data, const std::string& filename) {
    // Process without copying large objects
}

// → Use & when you need to modify
void updateScore(int& score, int points) {
    score += points;
}

// → Use default parameters for optional behavior
void log(const std::string& message, bool timestamp = true) {
    if (timestamp) {
        std::cout << "[LOG] ";
    }
    std::cout << message << std::endl;
}
```

## Exercises

### Exercise 1: Mathematical Functions

Create functions that demonstrate different parameter types:

- `calculateCompoundInterest()` - pass by value
- `convertTemperature()` - pass by reference for multiple outputs
- `findRoots()` - find quadratic equation roots

### Exercise 2: Array Processing

Write functions to:

- Find minimum and maximum values in an array
- Reverse an array in place
- Calculate statistics (mean, median, mode)

### Exercise 3: String Utilities

Build functions with various parameter types:

- `countWords()` - pass by const reference
- `capitalize()` - pass by reference to modify
- `findAndReplace()` - multiple parameters with defaults

### Exercise 4: Data Validation

Create validation functions:

- `validateEmail()` - pass by const reference
- `sanitizeInput()` - pass by reference to clean data
- `parseNumber()` - return success/failure via pointer

## Key Takeaways

<i class="fa-solid fa-arrow-right"></i> **Use pass by value** for small types that don't need modification  
<i class="fa-solid fa-arrow-right"></i> **Use pass by const reference** for large types you only read  
<i class="fa-solid fa-arrow-right"></i> **Use pass by reference** when you need to modify the original  
<i class="fa-solid fa-arrow-right"></i> **Always validate parameters** especially pointers and array indices  
<i class="fa-solid fa-arrow-right"></i> **Use default parameters** to make functions more flexible  
<i class="fa-solid fa-arrow-right"></i> **Check for null pointers** before dereferencing

---

**Next**: Learn about function overloading and multiple function versions // →  [Function Overloading](overloading.md)
