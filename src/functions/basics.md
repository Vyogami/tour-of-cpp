# Function Basics

Functions are named blocks of code that perform specific tasks. They're the fundamental building blocks for organizing and reusing code in C++. This chapter covers the essential concepts of creating, calling, and working with functions.

## What is a Function?

A function is a self-contained block of code that:

- Has a **name** for identification
- May accept **input** (parameters)
- Performs a **specific task**
- May return **output** (return value)

```cpp
#include <iostream>

// A simple function that greets the user
void sayHello() {
    std::cout << "Hello, World!" << std::endl;
}

int main() {
    sayHello();  // Function call
    return 0;
}
```

## Function Syntax

### Basic Structure

```cpp
returnType functionName(parameters) {
    // Function body
    return value;  // Optional, depends on return type
}
```

### Parts of a Function

```cpp
#include <iostream>

// Return type: int (function returns an integer)
// Function name: addNumbers
// Parameters: int a, int b (two integer inputs)
int addNumbers(int a, int b) {
    int sum = a + b;    // Function body
    return sum;         // Return statement
}

int main() {
    // Function call with arguments 5 and 3
    int result = addNumbers(5, 3);
    std::cout << "Result: " << result << std::endl;
    
    return 0;
}
```

## Return Types

### Functions That Return Values

```cpp
#include <iostream>

// Returns an integer
int multiply(int x, int y) {
    return x * y;
}

// Returns a double
double calculateAverage(double a, double b) {
    return (a + b) / 2.0;
}

// Returns a boolean
bool isEven(int number) {
    return number % 2 == 0;
}

// Returns a character
char getGrade(int score) {
    if (score >= 90) return 'A';
    if (score >= 80) return 'B';
    if (score >= 70) return 'C';
    if (score >= 60) return 'D';
    return 'F';
}

int main() {
    std::cout << "5 * 6 = " << multiply(5, 6) << std::endl;
    std::cout << "Average of 10 and 20: " << calculateAverage(10, 20) << std::endl;
    std::cout << "Is 8 even? " << std::boolalpha << isEven(8) << std::endl;
    std::cout << "Grade for 85: " << getGrade(85) << std::endl;
    
    return 0;
}
```

### Functions That Don't Return Values (`void`)

```cpp
#include <iostream>

// void means "no return value"
void printHeader() {
    std::cout << "=== Welcome to My Program ===" << std::endl;
    std::cout << "==============================" << std::endl;
}

void printSeparator() {
    std::cout << "------------------------------" << std::endl;
}

void displayNumber(int num) {
    std::cout << "The number is: " << num << std::endl;
}

int main() {
    printHeader();
    displayNumber(42);
    printSeparator();
    displayNumber(100);
    
    return 0;
}
```

## Function Parameters

### No Parameters

```cpp
#include <iostream>

int getCurrentYear() {
    return 2024;  // In a real program, this might get the actual current year
}

void clearScreen() {
    // In a real program, this might clear the console
    std::cout << "\n\n\n\n\n" << std::endl;
}

int main() {
    std::cout << "Current year: " << getCurrentYear() << std::endl;
    clearScreen();
    std::cout << "Screen cleared!" << std::endl;
    
    return 0;
}
```

### Single Parameter

```cpp
#include <iostream>
#include <cmath>

double calculateSquare(double number) {
    return number * number;
}

double calculateSquareRoot(double number) {
    if (number < 0) {
        std::cout << "Error: Cannot calculate square root of negative number" << std::endl;
        return -1;  // Error indicator
    }
    return std::sqrt(number);
}

void printCountdown(int start) {
    for (int i = start; i >= 1; i--) {
        std::cout << i << "... ";
    }
    std::cout << "Go!" << std::endl;
}

int main() {
    std::cout << "Square of 5: " << calculateSquare(5) << std::endl;
    std::cout << "Square root of 16: " << calculateSquareRoot(16) << std::endl;
    
    printCountdown(5);
    
    return 0;
}
```

### Multiple Parameters

```cpp
#include <iostream>

double calculateRectangleArea(double length, double width) {
    return length * width;
}

int findMaximum(int a, int b, int c) {
    int max = a;
    if (b > max) max = b;
    if (c > max) max = c;
    return max;
}

void printPersonInfo(std::string name, int age, double height) {
    std::cout << "Name: " << name << std::endl;
    std::cout << "Age: " << age << " years" << std::endl;
    std::cout << "Height: " << height << " meters" << std::endl;
}

bool isInRange(int value, int min, int max) {
    return value >= min && value <= max;
}

int main() {
    double area = calculateRectangleArea(5.5, 3.2);
    std::cout << "Rectangle area: " << area << std::endl;
    
    int maximum = findMaximum(10, 25, 18);
    std::cout << "Maximum: " << maximum << std::endl;
    
    printPersonInfo("Alice", 28, 1.65);
    
    bool inRange = isInRange(75, 0, 100);
    std::cout << "Is 75 in range [0,100]? " << std::boolalpha << inRange << std::endl;
    
    return 0;
}
```

## Function Calls

### Direct Function Calls

```cpp
#include <iostream>

int add(int a, int b) {
    return a + b;
}

void printMessage(std::string msg) {
    std::cout << msg << std::endl;
}

int main() {
    // Direct call and use return value immediately
    std::cout << "Sum: " << add(10, 20) << std::endl;
    
    // Direct call to void function
    printMessage("Hello from function!");
    
    return 0;
}
```

### Storing Return Values

```cpp
#include <iostream>

double calculateCircleArea(double radius) {
    return 3.14159 * radius * radius;
}

bool isPasswordStrong(std::string password) {
    return password.length() >= 8 && 
           password.find_first_of("0123456789") != std::string::npos;
}

int main() {
    // Store return value in variable
    double area = calculateCircleArea(5.0);
    std::cout << "Circle area: " << area << std::endl;
    
    // Use return value in conditional
    std::string userPassword = "mypass123";
    if (isPasswordStrong(userPassword)) {
        std::cout << "Strong password!" << std::endl;
    } else {
        std::cout << "Weak password." << std::endl;
    }
    
    return 0;
}
```

### Chaining Function Calls

```cpp
#include <iostream>
#include <cmath>

double absolute(double value) {
    return (value < 0) ? -value : value;
}

double roundToTwoDecimals(double value) {
    return std::round(value * 100.0) / 100.0;
}

int main() {
    double number = -3.14567;
    
    // Chain function calls
    double result = roundToTwoDecimals(absolute(number));
    std::cout << "Original: " << number << std::endl;
    std::cout << "Absolute and rounded: " << result << std::endl;
    
    return 0;
}
```

## Local Variables and Scope

### Variable Scope in Functions

```cpp
#include <iostream>

int globalVariable = 100;  // Global scope

void demonstrateScope() {
    int localVariable = 50;  // Local to this function
    
    std::cout << "Inside function:" << std::endl;
    std::cout << "Local variable: " << localVariable << std::endl;
    std::cout << "Global variable: " << globalVariable << std::endl;
    
    // Modify global variable
    globalVariable = 200;
}

int main() {
    std::cout << "Before function call:" << std::endl;
    std::cout << "Global variable: " << globalVariable << std::endl;
    
    demonstrateScope();
    
    std::cout << "After function call:" << std::endl;
    std::cout << "Global variable: " << globalVariable << std::endl;
    
    // std::cout << localVariable;  // ✘ Error: localVariable not accessible here
    
    return 0;
}
```

### Variable Lifetime

```cpp
#include <iostream>

int createNumber() {
    int localNum = 42;      // Created when function is called
    std::cout << "Created local number: " << localNum << std::endl;
    return localNum;        // Value is copied to caller
}  // localNum is destroyed here

int main() {
    int result = createNumber();  // Gets copy of the value
    std::cout << "Returned value: " << result << std::endl;
    
    return 0;
}
```

## Function Examples

### Mathematical Functions

```cpp
#include <iostream>
#include <cmath>

double power(double base, int exponent) {
    double result = 1.0;
    for (int i = 0; i < exponent; i++) {
        result *= base;
    }
    return result;
}

int factorial(int n) {
    if (n <= 1) return 1;
    
    int result = 1;
    for (int i = 2; i <= n; i++) {
        result *= i;
    }
    return result;
}

bool isPrime(int number) {
    if (number < 2) return false;
    if (number == 2) return true;
    if (number % 2 == 0) return false;
    
    for (int i = 3; i * i <= number; i += 2) {
        if (number % i == 0) return false;
    }
    return true;
}

int main() {
    std::cout << "2^8 = " << power(2, 8) << std::endl;
    std::cout << "5! = " << factorial(5) << std::endl;
    std::cout << "Is 17 prime? " << std::boolalpha << isPrime(17) << std::endl;
    std::cout << "Is 15 prime? " << std::boolalpha << isPrime(15) << std::endl;
    
    return 0;
}
```

### String Processing Functions

```cpp
#include <iostream>
#include <string>

int countVowels(std::string text) {
    int count = 0;
    for (char c : text) {
        char lower = std::tolower(c);
        if (lower == 'a' || lower == 'e' || lower == 'i' || 
            lower == 'o' || lower == 'u') {
            count++;
        }
    }
    return count;
}

std::string reverseString(std::string text) {
    std::string reversed;
    for (int i = text.length() - 1; i >= 0; i--) {
        reversed += text[i];
    }
    return reversed;
}

bool isPalindrome(std::string text) {
    std::string reversed = reverseString(text);
    return text == reversed;
}

int main() {
    std::string word = "programming";
    
    std::cout << "Word: " << word << std::endl;
    std::cout << "Vowels: " << countVowels(word) << std::endl;
    std::cout << "Reversed: " << reverseString(word) << std::endl;
    
    std::string palindrome = "racecar";
    std::cout << palindrome << " is palindrome? " 
              << std::boolalpha << isPalindrome(palindrome) << std::endl;
    
    return 0;
}
```

### Input/Output Functions

```cpp
#include <iostream>
#include <string>

int getPositiveInteger(std::string prompt) {
    int value;
    do {
        std::cout << prompt;
        std::cin >> value;
        if (value <= 0) {
            std::cout << "Please enter a positive number!" << std::endl;
        }
    } while (value <= 0);
    return value;
}

char getYesOrNo(std::string question) {
    char answer;
    do {
        std::cout << question << " (y/n): ";
        std::cin >> answer;
        answer = std::tolower(answer);
        if (answer != 'y' && answer != 'n') {
            std::cout << "Please enter 'y' or 'n'!" << std::endl;
        }
    } while (answer != 'y' && answer != 'n');
    return answer;
}

void displayMenu() {
    std::cout << "\n=== MENU ===" << std::endl;
    std::cout << "1. Option 1" << std::endl;
    std::cout << "2. Option 2" << std::endl;
    std::cout << "3. Exit" << std::endl;
    std::cout << "Choice: ";
}

int main() {
    int age = getPositiveInteger("Enter your age: ");
    std::cout << "You are " << age << " years old." << std::endl;
    
    char continue_program = getYesOrNo("Do you want to continue?");
    if (continue_program == 'y') {
        displayMenu();
        // Menu handling code would go here
    }
    
    return 0;
}
```

## Common Mistakes and Best Practices

### <i class="fa-solid fa-square-xmark"></i> Missing Return Statement

```cpp
// ✘ Function promises to return int but doesn't always do so
int divide(int a, int b) {
    if (b != 0) {
        return a / b;
    }
    // Missing return for b == 0 case!
}

// → All paths return a value
int divide_fixed(int a, int b) {
    if (b != 0) {
        return a / b;
    }
    return 0;  // or throw an exception
}
```

### <i class="fa-solid fa-square-xmark"></i> Unused Return Values

```cpp
#include <iostream>

int calculate(int x) {
    return x * 2 + 1;
}

int main() {
    calculate(5);  // ✘ Return value ignored
    
    int result = calculate(5);  // → Return value used
    std::cout << result << std::endl;
    
    return 0;
}
```

### Function Naming Best Practices

```cpp
// → Good function names (descriptive verbs)
bool isValidEmail(std::string email);
void calculateTotal();
int findMaximum(int a, int b);
void displayResults();

// ✘ Poor function names
bool check(std::string email);  // Check what?
void calc();                    // Calculate what?
int func(int a, int b);        // What does this function do?
void show();                   // Show what?
```

### Single Responsibility Principle

```cpp
// ✘ Function doing too many things
void processUserDataBad(std::string name, int age) {
    // Validate input
    if (name.empty() || age < 0) return;
    
    // Format data
    std::string formatted = "Name: " + name + ", Age: " + std::to_string(age);
    
    // Display data
    std::cout << formatted << std::endl;
    
    // Save to file
    // file saving code...
    
    // Send email notification
    // email sending code...
}

// → Each function has single responsibility
bool isValidUserData(std::string name, int age) {
    return !name.empty() && age >= 0;
}

std::string formatUserData(std::string name, int age) {
    return "Name: " + name + ", Age: " + std::to_string(age);
}

void displayUserData(std::string formattedData) {
    std::cout << formattedData << std::endl;
}
```

## Advanced Function Concepts Preview

### Function Prototypes

```cpp
#include <iostream>

// Function prototypes (declarations)
int add(int a, int b);
void printResult(int result);

int main() {
    int sum = add(10, 20);
    printResult(sum);
    return 0;
}

// Function definitions (can be after main)
int add(int a, int b) {
    return a + b;
}

void printResult(int result) {
    std::cout << "Result: " << result << std::endl;
}
```

### Recursive Functions

```cpp
#include <iostream>

int factorial_recursive(int n) {
    if (n <= 1) {
        return 1;           // Base case
    }
    return n * factorial_recursive(n - 1);  // Recursive case
}

int main() {
    std::cout << "5! = " << factorial_recursive(5) << std::endl;
    return 0;
}
```

## Exercises

### Exercise 1: Temperature Converter

Create functions to convert between Celsius, Fahrenheit, and Kelvin temperatures.

### Exercise 2: Grade Calculator

Write functions to:

- Calculate letter grade from numeric score
- Calculate GPA from multiple grades
- Determine if student is passing

### Exercise 3: Number Utilities

Create utility functions for:

- Finding the greatest common divisor (GCD)
- Checking if a number is perfect
- Generating Fibonacci sequence

### Exercise 4: Text Processor

Build functions to:

- Count words in a sentence
- Capitalize first letter of each word
- Remove extra spaces

## Key Takeaways

<i class="fa-solid fa-arrow-right"></i> **Functions promote code reuse** and organization  
<i class="fa-solid fa-arrow-right"></i> **Choose descriptive names** that indicate what the function does  
<i class="fa-solid fa-arrow-right"></i> **Each function should have a single responsibility**  
<i class="fa-solid fa-arrow-right"></i> **Always handle the case where functions might fail**  
<i class="fa-solid fa-arrow-right"></i> **Use appropriate return types** for your data  
<i class="fa-solid fa-arrow-right"></i> **Validate input parameters** when necessary

---

**Next**: Learn how to pass data to functions effectively // →  [Parameters and Arguments](parameters.md)
