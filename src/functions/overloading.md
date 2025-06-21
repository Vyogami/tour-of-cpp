# Function Overloading

Function overloading allows you to create multiple functions with the same name but different parameters. This provides a clean interface while handling different types or numbers of arguments, making your code more intuitive and flexible.

## What is Function Overloading?

Function overloading means having multiple functions with:

- **Same name**
- **Different parameter lists** (number, types, or order of parameters)
- **Potentially different return types**

```cpp
#include <iostream>

// Three overloaded functions named "print"
void print(int value) {
    std::cout << "Integer: " << value << std::endl;
}

void print(double value) {
    std::cout << "Double: " << value << std::endl;
}

void print(std::string value) {
    std::cout << "String: " << value << std::endl;
}

int main() {
    print(42);           // Calls print(int)
    print(3.14);         // Calls print(double)
    print("Hello");      // Calls print(string)
    
    return 0;
}
```

**Output:**

```
Integer: 42
Double: 3.14
String: Hello
```

## How Overload Resolution Works

The compiler chooses the best matching function based on the arguments:

### Exact Match Priority

```cpp
#include <iostream>

void display(int value) {
    std::cout << "int version: " << value << std::endl;
}

void display(double value) {
    std::cout << "double version: " << value << std::endl;
}

void display(char value) {
    std::cout << "char version: " << value << std::endl;
}

int main() {
    display(42);      // Exact match: int
    display(3.14);    // Exact match: double
    display('A');     // Exact match: char
    
    display(42.0f);   // float promotes to double
    display(true);    // bool converts to int
    
    return 0;
}
```

## Overloading by Number of Parameters

### Different Parameter Counts

```cpp
#include <iostream>

int add(int a, int b) {
    return a + b;
}

int add(int a, int b, int c) {
    return a + b + c;
}

int add(int a, int b, int c, int d) {
    return a + b + c + d;
}

double average(double a, double b) {
    return (a + b) / 2.0;
}

double average(double a, double b, double c) {
    return (a + b + c) / 3.0;
}

int main() {
    std::cout << "Sum of 2 numbers: " << add(10, 20) << std::endl;
    std::cout << "Sum of 3 numbers: " << add(10, 20, 30) << std::endl;
    std::cout << "Sum of 4 numbers: " << add(10, 20, 30, 40) << std::endl;
    
    std::cout << "Average of 2: " << average(10.0, 20.0) << std::endl;
    std::cout << "Average of 3: " << average(10.0, 20.0, 30.0) << std::endl;
    
    return 0;
}
```

### Practical Example: Drawing Functions

```cpp
#include <iostream>

void drawLine(int length) {
    for (int i = 0; i < length; i++) {
        std::cout << "-";
    }
    std::cout << std::endl;
}

void drawLine(int length, char character) {
    for (int i = 0; i < length; i++) {
        std::cout << character;
    }
    std::cout << std::endl;
}

void drawRectangle(int width, int height) {
    for (int i = 0; i < height; i++) {
        drawLine(width);
    }
}

void drawRectangle(int width, int height, char border, char fill) {
    for (int i = 0; i < height; i++) {
        for (int j = 0; j < width; j++) {
            if (i == 0 || i == height-1 || j == 0 || j == width-1) {
                std::cout << border;
            } else {
                std::cout << fill;
            }
        }
        std::cout << std::endl;
    }
}

int main() {
    std::cout << "Simple line:" << std::endl;
    drawLine(10);
    
    std::cout << "\nCustom character line:" << std::endl;
    drawLine(10, '*');
    
    std::cout << "\nSimple rectangle:" << std::endl;
    drawRectangle(5, 3);
    
    std::cout << "\nFancy rectangle:" << std::endl;
    drawRectangle(8, 4, '#', '.');
    
    return 0;
}
```

## Overloading by Parameter Types

### Different Types for Same Purpose

```cpp
#include <iostream>
#include <string>

// Absolute value for different types
int abs(int value) {
    return (value < 0) ? -value : value;
}

double abs(double value) {
    return (value < 0.0) ? -value : value;
}

float abs(float value) {
    return (value < 0.0f) ? -value : value;
}

// Maximum value for different types
int max(int a, int b) {
    return (a > b) ? a : b;
}

double max(double a, double b) {
    return (a > b) ? a : b;
}

std::string max(const std::string& a, const std::string& b) {
    return (a > b) ? a : b;  // Lexicographical comparison
}

int main() {
    std::cout << "abs(-5): " << abs(-5) << std::endl;
    std::cout << "abs(-3.14): " << abs(-3.14) << std::endl;
    std::cout << "abs(-2.5f): " << abs(-2.5f) << std::endl;
    
    std::cout << "max(10, 20): " << max(10, 20) << std::endl;
    std::cout << "max(3.14, 2.71): " << max(3.14, 2.71) << std::endl;
    std::cout << "max(\"apple\", \"banana\"): " << max(std::string("apple"), std::string("banana")) << std::endl;
    
    return 0;
}
```

### Search Functions for Different Data Types

```cpp
#include <iostream>
#include <string>

// Linear search in integer array
int search(const int arr[], int size, int target) {
    for (int i = 0; i < size; i++) {
        if (arr[i] == target) {
            return i;  // Return index
        }
    }
    return -1;  // Not found
}

// Linear search in double array
int search(const double arr[], int size, double target) {
    for (int i = 0; i < size; i++) {
        if (arr[i] == target) {
            return i;
        }
    }
    return -1;
}

// Search for substring in string
int search(const std::string& text, const std::string& pattern) {
    size_t pos = text.find(pattern);
    return (pos != std::string::npos) ? static_cast<int>(pos) : -1;
}

int main() {
    // Search in integer array
    int numbers[] = {10, 20, 30, 40, 50};
    int numSize = sizeof(numbers) / sizeof(numbers[0]);
    int pos1 = search(numbers, numSize, 30);
    std::cout << "30 found at index: " << pos1 << std::endl;
    
    // Search in double array
    double prices[] = {10.99, 25.50, 15.75, 30.25};
    int priceSize = sizeof(prices) / sizeof(prices[0]);
    int pos2 = search(prices, priceSize, 15.75);
    std::cout << "15.75 found at index: " << pos2 << std::endl;
    
    // Search in string
    std::string text = "Hello, World!";
    int pos3 = search(text, "World");
    std::cout << "\"World\" found at position: " << pos3 << std::endl;
    
    return 0;
}
```

## Overloading with Mixed Parameters

### Combining Different Types and Counts

```cpp
#include <iostream>
#include <string>

// Format and print different data types
void print(int value, const std::string& label = "Value") {
    std::cout << label << ": " << value << std::endl;
}

void print(double value, int precision, const std::string& label = "Value") {
    std::cout << std::fixed << std::setprecision(precision);
    std::cout << label << ": " << value << std::endl;
    std::cout << std::defaultfloat;  // Reset formatting
}

void print(const std::string& text, bool uppercase = false) {
    if (uppercase) {
        for (char c : text) {
            std::cout << static_cast<char>(std::toupper(c));
        }
    } else {
        std::cout << text;
    }
    std::cout << std::endl;
}

void print(const int arr[], int size, const std::string& separator = ", ") {
    for (int i = 0; i < size; i++) {
        std::cout << arr[i];
        if (i < size - 1) {
            std::cout << separator;
        }
    }
    std::cout << std::endl;
}

int main() {
    print(42);                           // int with default label
    print(100, "Score");                 // int with custom label
    
    print(3.14159, 2);                   // double with precision
    print(2.71828, 4, "Euler's number"); // double with precision and label
    
    print("hello world");                // string, normal case
    print("hello world", true);          // string, uppercase
    
    int numbers[] = {1, 2, 3, 4, 5};
    print(numbers, 5);                   // array with default separator
    print(numbers, 5, " | ");           // array with custom separator
    
    return 0;
}
```

## Overloading with Different Return Types

Note: Return type alone is NOT sufficient for overloading!

```cpp
#include <iostream>
#include <string>

// ✘ This would cause a compilation error
// int getValue() { return 42; }
// double getValue() { return 3.14; }  // Error: same signature

// → Different parameters allow different return types
int getNumber(int dummy) {
    return 42;
}

double getNumber(double dummy) {
    return 3.14;
}

std::string getNumber(const std::string& dummy) {
    return "forty-two";
}

// Better approach: Use different parameter types meaningfully
template<typename T>
T convert(const std::string& str);  // Template approach (more advanced)

// Or use clear function names
int parseInteger(const std::string& str) {
    return std::stoi(str);
}

double parseDouble(const std::string& str) {
    return std::stod(str);
}

int main() {
    std::cout << getNumber(0) << std::endl;        // int version
    std::cout << getNumber(0.0) << std::endl;      // double version
    std::cout << getNumber(std::string("")) << std::endl;  // string version
    
    std::cout << parseInteger("123") << std::endl;
    std::cout << parseDouble("3.14") << std::endl;
    
    return 0;
}
```

## Real-World Example: Calculator Functions

```cpp
#include <iostream>
#include <string>
#include <vector>

class Calculator {
public:
    // Basic arithmetic with two operands
    static double calculate(double a, double b, char operation) {
        switch (operation) {
            case '+': return a + b;
            case '-': return a - b;
            case '*': return a * b;
            case '/': return (b != 0) ? a / b : 0;
            default: return 0;
        }
    }
    
    // Extended arithmetic with operation name
    static double calculate(double a, double b, const std::string& operation) {
        if (operation == "add") return a + b;
        if (operation == "subtract") return a - b;
        if (operation == "multiply") return a * b;
        if (operation == "divide") return (b != 0) ? a / b : 0;
        if (operation == "power") return std::pow(a, b);
        if (operation == "modulo") return std::fmod(a, b);
        return 0;
    }
    
    // Single operand functions
    static double calculate(double value, const std::string& function) {
        if (function == "sqrt") return std::sqrt(value);
        if (function == "square") return value * value;
        if (function == "cube") return value * value * value;
        if (function == "reciprocal") return (value != 0) ? 1.0 / value : 0;
        if (function == "abs") return std::abs(value);
        return value;
    }
    
    // Array operations
    static double calculate(const double arr[], int size, const std::string& operation) {
        if (size <= 0) return 0;
        
        if (operation == "sum") {
            double sum = 0;
            for (int i = 0; i < size; i++) sum += arr[i];
            return sum;
        }
        
        if (operation == "average") {
            double sum = calculate(arr, size, "sum");
            return sum / size;
        }
        
        if (operation == "max") {
            double max = arr[0];
            for (int i = 1; i < size; i++) {
                if (arr[i] > max) max = arr[i];
            }
            return max;
        }
        
        if (operation == "min") {
            double min = arr[0];
            for (int i = 1; i < size; i++) {
                if (arr[i] < min) min = arr[i];
            }
            return min;
        }
        
        return 0;
    }
};

int main() {
    // Two operand calculations
    std::cout << "Basic operations:" << std::endl;
    std::cout << "5 + 3 = " << Calculator::calculate(5, 3, '+') << std::endl;
    std::cout << "5 * 3 = " << Calculator::calculate(5, 3, '*') << std::endl;
    
    // Extended operations
    std::cout << "\nExtended operations:" << std::endl;
    std::cout << "2^8 = " << Calculator::calculate(2, 8, "power") << std::endl;
    std::cout << "10 mod 3 = " << Calculator::calculate(10, 3, "modulo") << std::endl;
    
    // Single operand functions
    std::cout << "\nSingle operand functions:" << std::endl;
    std::cout << "sqrt(16) = " << Calculator::calculate(16, "sqrt") << std::endl;
    std::cout << "5^2 = " << Calculator::calculate(5, "square") << std::endl;
    
    // Array operations
    double numbers[] = {1.5, 2.5, 3.5, 4.5, 5.5};
    int size = sizeof(numbers) / sizeof(numbers[0]);
    
    std::cout << "\nArray operations:" << std::endl;
    std::cout << "Sum = " << Calculator::calculate(numbers, size, "sum") << std::endl;
    std::cout << "Average = " << Calculator::calculate(numbers, size, "average") << std::endl;
    std::cout << "Max = " << Calculator::calculate(numbers, size, "max") << std::endl;
    std::cout << "Min = " << Calculator::calculate(numbers, size, "min") << std::endl;
    
    return 0;
}
```

## Overloading Best Practices

### Good Overloading

```cpp
#include <iostream>
#include <string>

// → Related functionality with same conceptual operation
void save(const std::string& filename) {
    std::cout << "Saving to file: " << filename << std::endl;
}

void save(const std::string& filename, const std::string& data) {
    std::cout << "Saving data to file: " << filename << std::endl;
}

void save(const std::string& filename, const std::string& data, bool backup) {
    if (backup) {
        std::cout << "Creating backup before saving" << std::endl;
    }
    save(filename, data);
}

// → Intuitive parameter differences
void resize(int newSize) {
    std::cout << "Resizing to: " << newSize << std::endl;
}

void resize(int width, int height) {
    std::cout << "Resizing to: " << width << "x" << height << std::endl;
}

void resize(double scale) {
    std::cout << "Scaling by factor: " << scale << std::endl;
}
```

### <i class="fa-solid fa-square-xmark"></i> Poor Overloading

```cpp
// ✘ Confusing: same name for unrelated operations
void process(int value) {
    // Sorts an array
}

void process(const std::string& text) {
    // Sends an email
}

// ✘ Ambiguous parameter differences
void set(int value) { }
void set(unsigned int value) { }  // Can cause ambiguity

// ✘ Too many overloads make the interface complex
void draw(int x, int y);
void draw(int x, int y, int color);
void draw(int x, int y, int color, int size);
void draw(int x, int y, int color, int size, bool filled);
void draw(int x, int y, int color, int size, bool filled, double rotation);
// Better: Use a struct or class to group parameters
```

## Common Pitfalls

### Ambiguous Function Calls

```cpp
#include <iostream>

void ambiguous(int value) {
    std::cout << "int version" << std::endl;
}

void ambiguous(double value) {
    std::cout << "double version" << std::endl;
}

int main() {
    ambiguous(42);      // → Clear: int
    ambiguous(3.14);    // → Clear: double
    
    // ambiguous(42.0f);   // ✘ Ambiguous: float could go to either int or double
    
    // Resolve ambiguity with explicit casting
    ambiguous(static_cast<int>(42.0f));     // Explicitly call int version
    ambiguous(static_cast<double>(42.0f));  // Explicitly call double version
    
    return 0;
}
```

### Default Parameters and Overloading

```cpp
#include <iostream>

void func(int a) {
    std::cout << "One parameter: " << a << std::endl;
}

void func(int a, int b = 10) {
    std::cout << "Two parameters: " << a << ", " << b << std::endl;
}

int main() {
    func(5, 15);    // → Clear: calls two-parameter version
    // func(5);     // ✘ Ambiguous: could call either version!
    
    return 0;
}
```

## Exercises

### Exercise 1: Math Library

Create overloaded functions for:

- `pow()` - integer power, double power, complex power
- `round()` - different rounding modes and precisions
- `convert()` - temperature, distance, weight conversions

### Exercise 2: Text Processing

Build overloaded functions for:

- `format()` - format numbers, dates, currency
- `validate()` - validate emails, phone numbers, credit cards
- `extract()` - extract different data types from strings

### Exercise 3: Graphics Drawing

Create a drawing system with overloaded functions:

- `draw()` - points, lines, rectangles, circles
- `fill()` - different fill patterns and colors
- `transform()` - translate, rotate, scale shapes

### Exercise 4: Data Storage

Design overloaded save/load functions:

- Different file formats (text, binary, JSON)
- Different data types (arrays, objects, primitives)
- Different storage options (file, memory, database)

## Key Takeaways

<i class="fa-solid fa-arrow-right"></i> **Use overloading for related operations** that differ in input type or count  
<i class="fa-solid fa-arrow-right"></i> **Keep parameter differences clear** to avoid ambiguity  
<i class="fa-solid fa-arrow-right"></i> **Return types alone cannot distinguish overloads**  
<i class="fa-solid fa-arrow-right"></i> **Be careful with default parameters** and overloading interactions  
<i class="fa-solid fa-arrow-right"></i> **Use meaningful names** - not every similar function needs the same name  
<i class="fa-solid fa-arrow-right"></i> **Document overloaded functions well** to clarify their differences

---

**Next**: Explore modern C++ anonymous functions // →  [Lambda Expressions](lambda.md)
