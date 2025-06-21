# Functions

Functions are the building blocks of modular programming. They allow you to break complex problems into smaller, manageable pieces, reuse code, and create more organized and maintainable programs.

## What You'll Learn

- **Function Basics** - Creating and calling functions, return values
- **Parameters and Arguments** - Passing data to functions by value and reference
- **Function Overloading** - Multiple functions with the same name
- **Lambda Expressions** - Anonymous functions and modern C++ functional programming

## Why Functions Matter

Without functions, all code would be in one giant `main()` function:

```cpp
// ✘ Without functions - everything in main
#include <iostream>
int main() {
    // Calculate area of rectangle 1
    int length1 = 10, width1 = 5;
    int area1 = length1 * width1;
    std::cout << "Area 1: " << area1 << std::endl;
    
    // Calculate area of rectangle 2 (repeated code!)
    int length2 = 8, width2 = 3;
    int area2 = length2 * width2;
    std::cout << "Area 2: " << area2 << std::endl;
    
    // Calculate area of rectangle 3 (more repetition!)
    int length3 = 12, width3 = 7;
    int area3 = length3 * width3;
    std::cout << "Area 3: " << area3 << std::endl;
    
    return 0;
}
```

```cpp
// → With functions - clean and reusable
#include <iostream>

int calculateArea(int length, int width) {
    return length * width;
}

int main() {
    std::cout << "Area 1: " << calculateArea(10, 5) << std::endl;
    std::cout << "Area 2: " << calculateArea(8, 3) << std::endl;
    std::cout << "Area 3: " << calculateArea(12, 7) << std::endl;
    
    return 0;
}
```

## Benefits of Functions

### **Code Reusability**

Write once, use many times:

```cpp
double calculateCircleArea(double radius) {
    return 3.14159 * radius * radius;
}

// Use the same function multiple times
double garden = calculateCircleArea(5.0);
double pond = calculateCircleArea(2.5);
double fountain = calculateCircleArea(1.0);
```

### **Modularity**

Break complex problems into smaller pieces:

```cpp
// Each function has a single responsibility
bool isValidEmail(const std::string& email);
void sendEmail(const std::string& to, const std::string& message);
bool saveToDatabase(const UserData& data);
void logActivity(const std::string& action);
```

### **Testability**

Test individual components:

```cpp
// Easy to test individual functions
bool testCalculateArea() {
    return calculateArea(5, 4) == 20;  // Expected result
}
```

### **Readability**

Self-documenting code:

```cpp
// Clear intent through function names
if (isWeekend(today) && !isHoliday(today)) {
    scheduleBackup();
    sendWeeklyReport();
}
```

## Function Categories

### Input/Output Functions

```cpp
void displayWelcome() {                    // No input, no output (void)
    std::cout << "Welcome!" << std::endl;
}

int getUserInput() {                       // No input, returns output
    int value;
    std::cin >> value;
    return value;
}

void displayResult(int result) {           // Takes input, no output
    std::cout << "Result: " << result << std::endl;
}

int add(int a, int b) {                   // Takes input, returns output
    return a + b;
}
```

### Pure vs Impure Functions

```cpp
// → Pure function: same input always gives same output, no side effects
int multiply(int a, int b) {
    return a * b;
}

// ✘ Impure function: has side effects (prints to console)
int multiplyAndLog(int a, int b) {
    int result = a * b;
    std::cout << "Multiplying " << a << " and " << b << std::endl;  // Side effect
    return result;
}

// ✘ Impure function: depends on external state
int counter = 0;
int getNextNumber() {
    return ++counter;  // Modifies global state
}
```

## Function Anatomy

### Basic Structure

```cpp
returnType functionName(parameterType parameterName) {
    // Function body
    return value;  // Optional, depending on return type
}
```

### Example Breakdown

```cpp
#include <iostream>

// Return type: double
// Function name: calculateTax
// Parameters: double income, double rate
double calculateTax(double income, double rate) {
    double tax = income * (rate / 100.0);  // Function body
    return tax;                            // Return statement
}

int main() {
    double myIncome = 50000.0;
    double taxRate = 15.0;
    
    // Function call: passing arguments to parameters
    double myTax = calculateTax(myIncome, taxRate);
    
    std::cout << "Tax owed: $" << myTax << std::endl;
    
    return 0;
}
```

## Common Function Patterns

### Validation Functions

```cpp
#include <iostream>

bool isValidAge(int age) {
    return age >= 0 && age <= 150;
}

bool isValidGrade(char grade) {
    return grade == 'A' || grade == 'B' || grade == 'C' || 
           grade == 'D' || grade == 'F';
}

bool isPositive(double number) {
    return number > 0.0;
}

int main() {
    int userAge = 25;
    
    if (isValidAge(userAge)) {
        std::cout << "Valid age: " << userAge << std::endl;
    } else {
        std::cout << "Invalid age!" << std::endl;
    }
    
    return 0;
}
```

### Calculation Functions

```cpp
#include <iostream>
#include <cmath>

double calculateDistance(double x1, double y1, double x2, double y2) {
    double dx = x2 - x1;
    double dy = y2 - y1;
    return std::sqrt(dx * dx + dy * dy);
}

double calculateCompoundInterest(double principal, double rate, int years) {
    return principal * std::pow(1 + rate/100, years);
}

double celsiusToFahrenheit(double celsius) {
    return (celsius * 9.0/5.0) + 32.0;
}

int main() {
    // Calculate distance between two points
    double distance = calculateDistance(0, 0, 3, 4);
    std::cout << "Distance: " << distance << std::endl;  // 5.0
    
    // Calculate compound interest
    double amount = calculateCompoundInterest(1000, 5, 10);
    std::cout << "Amount after 10 years: $" << amount << std::endl;
    
    // Convert temperature
    double fahrenheit = celsiusToFahrenheit(25);
    std::cout << "25°C = " << fahrenheit << "°F" << std::endl;
    
    return 0;
}
```

### Utility Functions

```cpp
#include <iostream>
#include <algorithm>

int maximum(int a, int b) {
    return (a > b) ? a : b;
}

int minimum(int a, int b) {
    return (a < b) ? a : b;
}

void swap(int& a, int& b) {    // Note: parameters passed by reference
    int temp = a;
    a = b;
    b = temp;
}

int clamp(int value, int minVal, int maxVal) {
    return std::max(minVal, std::min(value, maxVal));
}

int main() {
    int x = 10, y = 20;
    
    std::cout << "Max: " << maximum(x, y) << std::endl;
    std::cout << "Min: " << minimum(x, y) << std::endl;
    
    std::cout << "Before swap: x=" << x << ", y=" << y << std::endl;
    swap(x, y);
    std::cout << "After swap: x=" << x << ", y=" << y << std::endl;
    
    int value = clamp(150, 0, 100);  // Limit value to range [0, 100]
    std::cout << "Clamped value: " << value << std::endl;  // 100
    
    return 0;
}
```

## Function Organization

### Header Comments

```cpp
/**
 * Calculates the area of a circle given its radius
 * @param radius The radius of the circle (must be positive)
 * @return The area of the circle, or -1 if radius is invalid
 */
double calculateCircleArea(double radius) {
    if (radius < 0) {
        return -1;  // Error indicator
    }
    return 3.14159 * radius * radius;
}

/**
 * Determines if a year is a leap year
 * @param year The year to check (Gregorian calendar)
 * @return true if the year is a leap year, false otherwise
 */
bool isLeapYear(int year) {
    return (year % 4 == 0 && year % 100 != 0) || (year % 400 == 0);
}
```

### Function Declaration vs Definition

```cpp
#include <iostream>

// Function declarations (prototypes) - usually in header files
int add(int a, int b);
double calculateAverage(int numbers[], int size);
void printHeader();

int main() {
    printHeader();
    
    int sum = add(10, 20);
    std::cout << "Sum: " << sum << std::endl;
    
    int data[] = {10, 20, 30, 40, 50};
    double avg = calculateAverage(data, 5);
    std::cout << "Average: " << avg << std::endl;
    
    return 0;
}

// Function definitions - the actual implementation
int add(int a, int b) {
    return a + b;
}

double calculateAverage(int numbers[], int size) {
    if (size == 0) return 0.0;
    
    int sum = 0;
    for (int i = 0; i < size; i++) {
        sum += numbers[i];
    }
    return static_cast<double>(sum) / size;
}

void printHeader() {
    std::cout << "=== Math Calculator ===" << std::endl;
}
```

## Best Practices Preview

As we explore functions, we'll emphasize:

- **Single Responsibility**: Each function should do one thing well
- **Meaningful Names**: Functions should clearly describe what they do
- **Parameter Validation**: Check inputs for validity
- **Consistent Style**: Use consistent naming and organization
- **Error Handling**: Plan for invalid inputs and edge cases

## Modern C++ Function Features

### Auto Return Type Deduction (C++14)

```cpp
auto add(int a, int b) {
    return a + b;  // Compiler deduces return type as int
}

auto calculateArea(double length, double width) {
    return length * width;  // Compiler deduces return type as double
}
```

### Constexpr Functions (C++11)

```cpp
constexpr int factorial(int n) {
    return (n <= 1) ? 1 : n * factorial(n - 1);
}

// Can be evaluated at compile time
constexpr int fact5 = factorial(5);  // Computed during compilation
```

### Function Templates Preview

```cpp
template<typename T>
T maximum(T a, T b) {
    return (a > b) ? a : b;
}

// Works with any comparable type
int maxInt = maximum(10, 20);        // int version
double maxDouble = maximum(3.14, 2.71);  // double version
```

Let's dive into the specifics of how functions work and how to use them effectively!

---

**Sections in This Chapter:**

- [Function Basics // → ](basics.md)
- [Parameters and Arguments // → ](parameters.md)
- [Function Overloading // → ](overloading.md)
- [Lambda Expressions // → ](lambda.md)
