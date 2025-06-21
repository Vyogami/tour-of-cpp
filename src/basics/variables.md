# Variables and Declarations

Variables are named storage locations for data in your program. Think of them as labeled boxes where you can store different types of information.

## What is a Variable?

A variable has three key properties:

- **Name**: How you refer to it in code
- **Type**: What kind of data it can hold
- **Value**: The actual data stored

```cpp
#include <iostream>

int main() {
    int age = 25;        // Name: age, Type: int, Value: 25
    double price = 9.99; // Name: price, Type: double, Value: 9.99
    char grade = 'A';    // Name: grade, Type: char, Value: 'A'
    
    std::cout << "Age: " << age << std::endl;
    std::cout << "Price: $" << price << std::endl;
    std::cout << "Grade: " << grade << std::endl;
    
    return 0;
}
```

**Output:**

```
Age: 25
Price: $9.99
Grade: A
```

## Variable Declaration Syntax

### Basic Declaration

```cpp
type variableName;           // Declaration only
type variableName = value;   // Declaration with initialization
```

### Examples

```cpp
// Declaration without initialization
int count;              // Contains garbage value!
double temperature;     // Undefined behavior to use before assignment

// Declaration with initialization (preferred)
int count = 0;          // Safe: initialized to 0
double temperature = 98.6;  // Safe: initialized to 98.6
```

<i class="fa-solid fa-triangle-exclamation"></i> **Important**: Always initialize variables before use to avoid undefined behavior.

## Multiple Variable Declarations

```cpp
// Declare multiple variables of the same type
int x, y, z;                    // All are int, but uninitialized
int a = 1, b = 2, c = 3;       // All initialized
int width = 800, height = 600; // Descriptive names

// Mixed initialization (be careful!)
int first = 10, second, third = 20;  // 'second' is uninitialized!
```

## Variable Naming Rules

### Legal Names

```cpp
int age;           // → Simple and clear
int studentAge;    // → camelCase convention
int student_age;   // → snake_case convention
int MAX_SIZE;      // → Often used for constants
int _internal;     // → Legal but avoid (reserved convention)
int value1;        // → Numbers allowed (not at start)
```

### Illegal Names

```cpp
int 1value;        // ✘ Cannot start with digit
int student-age;   // ✘ Hyphens not allowed
int class;         // ✘ Reserved keyword
int my variable;   // ✘ Spaces not allowed
```

### Reserved Keywords

These words have special meaning in C++ and cannot be used as variable names:

```cpp
// Some common reserved keywords:
int, double, char, bool, if, else, while, for, class, 
public, private, return, void, const, static, new, delete
```

## Variable Initialization Methods

### Copy Initialization

```cpp
int age = 25;           // Traditional C-style
double pi = 3.14159;    // Copy the value
```

### Direct Initialization  

```cpp
int age(25);            // Direct initialization
double pi(3.14159);     // Calls constructor-like syntax
```

### List Initialization (C++11+)

```cpp
int age{25};            // Uniform initialization
double pi{3.14159};     // Preferred in modern C++
int value{};            // Zero initialization
```

### Auto Type Deduction (C++11+)

```cpp
auto age = 25;          // Compiler deduces 'int'
auto pi = 3.14159;      // Compiler deduces 'double'
auto grade = 'A';       // Compiler deduces 'char'
auto isStudent = true;  // Compiler deduces 'bool'
```

<i class="fa-solid fa-lightbulb"></i> **Modern C++ Tip**: Use `auto` when the type is obvious from the initializer, and uniform initialization `{}` to prevent narrowing conversions.

## Variable Scope

Scope determines where a variable can be accessed in your program.

### Local Scope

```cpp
#include <iostream>

int main() {
    int localVar = 10;  // Local to main() function
    
    if (true) {
        int blockVar = 20;      // Local to this if-block
        std::cout << localVar;  // → Can access outer scope
        std::cout << blockVar;  // → Can access current scope
    }
    
    // std::cout << blockVar;  // ✘ Error: blockVar out of scope
    std::cout << localVar;     // → Still in scope
    
    return 0;
}
```

### Global Scope

```cpp
#include <iostream>

int globalVar = 100;  // Global variable (avoid when possible)

int main() {
    int localVar = 50;
    
    std::cout << "Global: " << globalVar << std::endl;  // → Accessible
    std::cout << "Local: " << localVar << std::endl;    // → Accessible
    
    return 0;
}
```

<i class="fa-solid fa-triangle-exclamation"></i> **Best Practice**: Minimize use of global variables. Prefer local variables and function parameters.

## Variable Lifetime

When variables are created and destroyed:

```cpp
#include <iostream>

int main() {
    std::cout << "Starting main()" << std::endl;
    
    int x = 10;  // x is created here
    
    {
        int y = 20;  // y is created here
        std::cout << "x: " << x << ", y: " << y << std::endl;
    }  // y is destroyed here
    
    // std::cout << y;  // ✘ Error: y no longer exists
    std::cout << "x: " << x << std::endl;  // → x still exists
    
    return 0;
}  // x is destroyed here
```

## Common Patterns and Examples

### Swap Variables

```cpp
#include <iostream>

int main() {
    int a = 10;
    int b = 20;
    
    std::cout << "Before swap: a = " << a << ", b = " << b << std::endl;
    
    // Swap using temporary variable
    int temp = a;
    a = b;
    b = temp;
    
    std::cout << "After swap: a = " << a << ", b = " << b << std::endl;
    
    return 0;
}
```

### Counter Pattern

```cpp
#include <iostream>

int main() {
    int count = 0;  // Initialize counter
    
    // Simulate counting something
    count = count + 1;  // Increment
    count += 1;         // Shorthand increment
    count++;            // Post-increment
    ++count;            // Pre-increment
    
    std::cout << "Final count: " << count << std::endl;  // Output: 4
    
    return 0;
}
```

### Accumulator Pattern

```cpp
#include <iostream>

int main() {
    double total = 0.0;     // Initialize accumulator
    double price1 = 15.99;
    double price2 = 8.50;
    double price3 = 22.25;
    
    total += price1;        // Add to total
    total += price2;
    total += price3;
    
    std::cout << "Total: $" << total << std::endl;  // Output: $46.74
    
    return 0;
}
```

## Common Beginner Mistakes

### <i class="fa-solid fa-square-xmark"></i> Uninitialized Variables

```cpp
int count;                    // Uninitialized!
count = count + 1;           // Undefined behavior
std::cout << count;          // Could print anything
```

**<i class="fa-solid fa-arrow-right"></i> Fix:**

```cpp
int count = 0;               // Always initialize
count = count + 1;           // Now well-defined
std::cout << count;          // Prints: 1
```

### <i class="fa-solid fa-square-xmark"></i> Variable Shadowing

```cpp
int value = 10;              // Outer variable

if (true) {
    int value = 20;          // Shadows outer variable
    std::cout << value;      // Prints: 20 (inner variable)
}

std::cout << value;          // Prints: 10 (outer variable)
```

### <i class="fa-solid fa-square-xmark"></i> Unused Variables

```cpp
int unusedVar = 42;          // Compiler warning
int result = 10 + 20;        // Used variable
```

## Exercises

### Exercise 1: Personal Information

Create variables to store your name, age, height, and whether you're a student. Print them in a formatted way.

### Exercise 2: Temperature Converter

Create variables for Fahrenheit and Celsius temperatures. Calculate the conversion and display both values.

### Exercise 3: Shopping Cart

Create variables for item quantities and prices. Calculate and display the total cost.

## Modern C++ Features

### Structured Bindings (C++17)

```cpp
#include <iostream>
#include <tuple>

int main() {
    auto person = std::make_tuple("Alice", 25, 5.6);
    auto [name, age, height] = person;  // Structured binding
    
    std::cout << "Name: " << name << std::endl;
    std::cout << "Age: " << age << std::endl;
    std::cout << "Height: " << height << std::endl;
    
    return 0;
}
```

## Key Takeaways

<i class="fa-solid fa-arrow-right"></i> **Always initialize variables** before use  
<i class="fa-solid fa-arrow-right"></i> **Use meaningful names** that describe the variable's purpose  
<i class="fa-solid fa-arrow-right"></i> **Prefer local scope** over global scope  
<i class="fa-solid fa-arrow-right"></i> **Use `auto`** when type is obvious from context  
<i class="fa-solid fa-arrow-right"></i> **Use uniform initialization** `{}` in modern C++

---

**Next**: Learn about the different types of data you can store in variables // →  [Fundamental Types](types.md)
