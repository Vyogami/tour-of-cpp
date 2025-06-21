# Lambda Expressions

Lambda expressions, introduced in C++11, allow you to create anonymous functions inline. They're particularly useful for short functions, callbacks, and functional programming patterns. Lambdas make code more concise and expressive, especially when working with algorithms and event handling.

## What are Lambda Expressions?

A lambda is an anonymous function that you can define at the point of use:

```cpp
#include <iostream>

int main() {
    // Traditional function approach
    auto traditionalAdd = [](int a, int b) {
        return a + b;
    };
    
    // Lambda expression breakdown:
    // [] - capture clause
    // (int a, int b) - parameter list
    // { return a + b; } - function body
    
    std::cout << "5 + 3 = " << traditionalAdd(5, 3) << std::endl;
    
    // Even simpler lambda
    auto sayHello = []() {
        std::cout << "Hello from lambda!" << std::endl;
    };
    
    sayHello();
    
    return 0;
}
```

## Basic Lambda Syntax

### Complete Syntax

```cpp
[capture clause](parameter list) -> return type { function body }
```

### Simplified Forms

```cpp
#include <iostream>

int main() {
    // Full syntax with explicit return type
    auto multiply = [](int a, int b) -> int {
        return a * b;
    };
    
    // Return type deduced automatically
    auto divide = [](double a, double b) {
        return a / b;  // Compiler deduces return type as double
    };
    
    // No parameters
    auto getRandomNumber = []() {
        return 42;  // The most random number
    };
    
    // No parameters (parentheses optional)
    auto printMessage = [] {
        std::cout << "Lambda without parentheses!" << std::endl;
    };
    
    std::cout << "10 * 5 = " << multiply(10, 5) << std::endl;
    std::cout << "10.0 / 3.0 = " << divide(10.0, 3.0) << std::endl;
    std::cout << "Random number: " << getRandomNumber() << std::endl;
    printMessage();
    
    return 0;
}
```

## Capture Clauses

The capture clause `[]` determines how variables from the surrounding scope are accessed:

### Capture by Value

```cpp
#include <iostream>

int main() {
    int x = 10;
    int y = 20;
    
    // Capture x and y by value (copy)
    auto lambda1 = [x, y]() {
        std::cout << "Captured by value: x=" << x << ", y=" << y << std::endl;
        // x = 100;  // ✘ Error: cannot modify captured-by-value variables
    };
    
    // Capture all local variables by value
    auto lambda2 = [=]() {
        std::cout << "All by value: x=" << x << ", y=" << y << std::endl;
    };
    
    lambda1();
    lambda2();
    
    // Original variables unchanged
    x = 50;
    y = 60;
    std::cout << "After modification: x=" << x << ", y=" << y << std::endl;
    
    // Lambda still has original values
    lambda1();
    
    return 0;
}
```

### Capture by Reference

```cpp
#include <iostream>

int main() {
    int counter = 0;
    
    // Capture counter by reference
    auto increment = [&counter]() {
        counter++;
        std::cout << "Counter: " << counter << std::endl;
    };
    
    // Capture all local variables by reference
    auto reset = [&]() {
        counter = 0;
        std::cout << "Counter reset to: " << counter << std::endl;
    };
    
    increment();  // Counter: 1
    increment();  // Counter: 2
    increment();  // Counter: 3
    
    reset();      // Counter reset to: 0
    
    std::cout << "Final counter value: " << counter << std::endl;
    
    return 0;
}
```

### Mixed Capture

```cpp
#include <iostream>

int main() {
    int value = 100;
    int multiplier = 5;
    int offset = 10;
    
    // Mixed capture: value by copy, multiplier by reference, offset explicitly by value
    auto calculate = [value, &multiplier, offset](int input) {
        // value and offset are copies (cannot be modified)
        // multiplier is a reference (can be modified)
        multiplier = 2;  // This modifies the original multiplier
        return (input + value + offset) * multiplier;
    };
    
    std::cout << "Before lambda: multiplier = " << multiplier << std::endl;
    int result = calculate(20);
    std::cout << "Result: " << result << std::endl;
    std::cout << "After lambda: multiplier = " << multiplier << std::endl;
    
    return 0;
}
```

### Mutable Lambdas

```cpp
#include <iostream>

int main() {
    int original = 42;
    
    // Mutable lambda can modify captured-by-value variables
    auto modifyCapture = [original](int increment) mutable {
        original += increment;  // Modifies the copy, not the original
        std::cout << "Inside lambda: " << original << std::endl;
        return original;
    };
    
    std::cout << "Original before: " << original << std::endl;
    int result1 = modifyCapture(10);
    int result2 = modifyCapture(20);
    std::cout << "Original after: " << original << std::endl;
    std::cout << "Results: " << result1 << ", " << result2 << std::endl;
    
    return 0;
}
```

## Lambdas with Standard Algorithms

Lambdas are extensively used with STL algorithms:

### Sorting and Searching

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

int main() {
    std::vector<int> numbers = {5, 2, 8, 1, 9, 3};
    
    std::cout << "Original: ";
    for (int n : numbers) std::cout << n << " ";
    std::cout << std::endl;
    
    // Sort in ascending order
    std::sort(numbers.begin(), numbers.end(), [](int a, int b) {
        return a < b;
    });
    
    std::cout << "Ascending: ";
    for (int n : numbers) std::cout << n << " ";
    std::cout << std::endl;
    
    // Sort in descending order
    std::sort(numbers.begin(), numbers.end(), [](int a, int b) {
        return a > b;
    });
    
    std::cout << "Descending: ";
    for (int n : numbers) std::cout << n << " ";
    std::cout << std::endl;
    
    // Find first number greater than 5
    auto it = std::find_if(numbers.begin(), numbers.end(), [](int n) {
        return n > 5;
    });
    
    if (it != numbers.end()) {
        std::cout << "First number > 5: " << *it << std::endl;
    }
    
    return 0;
}
```

### Transformation and Filtering

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <numeric>

int main() {
    std::vector<int> numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    std::vector<int> squares(numbers.size());
    std::vector<int> evens;
    
    // Transform: square each number
    std::transform(numbers.begin(), numbers.end(), squares.begin(), [](int n) {
        return n * n;
    });
    
    std::cout << "Squares: ";
    for (int n : squares) std::cout << n << " ";
    std::cout << std::endl;
    
    // Filter: copy only even numbers
    std::copy_if(numbers.begin(), numbers.end(), std::back_inserter(evens), [](int n) {
        return n % 2 == 0;
    });
    
    std::cout << "Even numbers: ";
    for (int n : evens) std::cout << n << " ";
    std::cout << std::endl;
    
    // Count numbers greater than 5
    int count = std::count_if(numbers.begin(), numbers.end(), [](int n) {
        return n > 5;
    });
    
    std::cout << "Numbers > 5: " << count << std::endl;
    
    // Calculate sum using accumulate with lambda
    int sum = std::accumulate(numbers.begin(), numbers.end(), 0, [](int total, int n) {
        return total + n;
    });
    
    std::cout << "Sum: " << sum << std::endl;
    
    return 0;
}
```

## Practical Lambda Examples

### Event Handling Simulation

```cpp
#include <iostream>
#include <vector>
#include <functional>
#include <string>

class Button {
private:
    std::string name;
    std::function<void()> clickHandler;
    
public:
    Button(const std::string& buttonName) : name(buttonName) {}
    
    void setOnClick(std::function<void()> handler) {
        clickHandler = handler;
    }
    
    void click() {
        std::cout << name << " button clicked!" << std::endl;
        if (clickHandler) {
            clickHandler();
        }
    }
};

int main() {
    Button saveButton("Save");
    Button exitButton("Exit");
    Button calculateButton("Calculate");
    
    // Simple lambda for save button
    saveButton.setOnClick([]() {
        std::cout << "Saving document..." << std::endl;
    });
    
    // Lambda with capture for exit button
    bool confirmExit = true;
    exitButton.setOnClick([&confirmExit]() {
        if (confirmExit) {
            std::cout << "Exiting application..." << std::endl;
        } else {
            std::cout << "Exit cancelled." << std::endl;
        }
    });
    
    // Lambda with more complex logic
    int operand1 = 10, operand2 = 5;
    calculateButton.setOnClick([operand1, operand2]() {
        int sum = operand1 + operand2;
        int product = operand1 * operand2;
        std::cout << "Calculation results:" << std::endl;
        std::cout << operand1 << " + " << operand2 << " = " << sum << std::endl;
        std::cout << operand1 << " * " << operand2 << " = " << product << std::endl;
    });
    
    // Simulate button clicks
    saveButton.click();
    calculateButton.click();
    exitButton.click();
    
    return 0;
}
```

### Custom Comparators

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <string>

struct Person {
    std::string name;
    int age;
    double salary;
    
    Person(const std::string& n, int a, double s) : name(n), age(a), salary(s) {}
};

void printPeople(const std::vector<Person>& people, const std::string& title) {
    std::cout << "\n" << title << ":" << std::endl;
    for (const auto& person : people) {
        std::cout << person.name << " (age: " << person.age 
                  << ", salary: $" << person.salary << ")" << std::endl;
    }
}

int main() {
    std::vector<Person> employees = {
        {"Alice", 30, 75000},
        {"Bob", 25, 65000},
        {"Charlie", 35, 85000},
        {"Diana", 28, 70000}
    };
    
    printPeople(employees, "Original order");
    
    // Sort by age (ascending)
    std::sort(employees.begin(), employees.end(), [](const Person& a, const Person& b) {
        return a.age < b.age;
    });
    printPeople(employees, "Sorted by age");
    
    // Sort by salary (descending)
    std::sort(employees.begin(), employees.end(), [](const Person& a, const Person& b) {
        return a.salary > b.salary;
    });
    printPeople(employees, "Sorted by salary (highest first)");
    
    // Sort by name length, then alphabetically
    std::sort(employees.begin(), employees.end(), [](const Person& a, const Person& b) {
        if (a.name.length() != b.name.length()) {
            return a.name.length() < b.name.length();
        }
        return a.name < b.name;
    });
    printPeople(employees, "Sorted by name length, then alphabetically");
    
    // Find highest paid employee under 30
    auto it = std::max_element(employees.begin(), employees.end(), 
        [](const Person& a, const Person& b) {
            // Only compare if both are under 30
            if (a.age >= 30 && b.age >= 30) return false;
            if (a.age >= 30) return true;   // a is not valid, b is better
            if (b.age >= 30) return false;  // b is not valid, a is better
            return a.salary < b.salary;     // Both valid, compare salaries
        });
    
    std::cout << "\nHighest paid employee under 30:" << std::endl;
    if (it != employees.end() && it->age < 30) {
        std::cout << it->name << " with salary $" << it->salary << std::endl;
    }
    
    return 0;
}
```

### Function Factories

```cpp
#include <iostream>
#include <functional>

// Function that returns a lambda
std::function<int(int)> makeMultiplier(int factor) {
    return [factor](int value) {
        return value * factor;
    };
}

// Function that returns a validator lambda
std::function<bool(int)> makeRangeValidator(int min, int max) {
    return [min, max](int value) {
        return value >= min && value <= max;
    };
}

// Function that returns a configurable formatter lambda
std::function<std::string(double)> makeCurrencyFormatter(const std::string& symbol, int decimals) {
    return [symbol, decimals](double amount) {
        std::string result = symbol;
        result += std::to_string(amount);
        // Simple decimal formatting (in real code, use proper formatting)
        size_t dot = result.find('.');
        if (dot != std::string::npos) {
            result = result.substr(0, dot + decimals + 1);
        }
        return result;
    };
}

int main() {
    // Create specialized multiplier functions
    auto double_it = makeMultiplier(2);
    auto triple_it = makeMultiplier(3);
    auto times_ten = makeMultiplier(10);
    
    std::cout << "Multipliers:" << std::endl;
    std::cout << "5 * 2 = " << double_it(5) << std::endl;
    std::cout << "7 * 3 = " << triple_it(7) << std::endl;
    std::cout << "4 * 10 = " << times_ten(4) << std::endl;
    
    // Create validators for different ranges
    auto isValidAge = makeRangeValidator(0, 150);
    auto isValidPercent = makeRangeValidator(0, 100);
    auto isValidGrade = makeRangeValidator(0, 4);
    
    std::cout << "\nValidators:" << std::endl;
    std::cout << "Age 25 valid? " << std::boolalpha << isValidAge(25) << std::endl;
    std::cout << "Age 200 valid? " << std::boolalpha << isValidAge(200) << std::endl;
    std::cout << "Percent 85 valid? " << std::boolalpha << isValidPercent(85) << std::endl;
    std::cout << "Grade 3.5 valid? " << std::boolalpha << isValidGrade(3) << std::endl;
    
    // Create formatters for different currencies
    auto formatUSD = makeCurrencyFormatter("$", 2);
    auto formatEUR = makeCurrencyFormatter("€", 2);
    auto formatYEN = makeCurrencyFormatter("¥", 0);
    
    std::cout << "\nFormatters:" << std::endl;
    std::cout << "Price in USD: " << formatUSD(19.99) << std::endl;
    std::cout << "Price in EUR: " << formatEUR(16.75) << std::endl;
    std::cout << "Price in YEN: " << formatYEN(2100.0) << std::endl;
    
    return 0;
}
```

## Advanced Lambda Features

### Generic Lambdas (C++14)

```cpp
#include <iostream>
#include <string>
#include <vector>

int main() {
    // Generic lambda with auto parameters
    auto genericPrint = [](const auto& value) {
        std::cout << "Value: " << value << std::endl;
    };
    
    genericPrint(42);
    genericPrint(3.14);
    genericPrint("Hello");
    genericPrint(std::string("World"));
    
    // Generic lambda for comparison
    auto isGreater = [](const auto& a, const auto& b) {
        return a > b;
    };
    
    std::cout << "5 > 3: " << std::boolalpha << isGreater(5, 3) << std::endl;
    std::cout << "3.14 > 2.71: " << std::boolalpha << isGreater(3.14, 2.71) << std::endl;
    std::cout << "\"apple\" > \"banana\": " << std::boolalpha << isGreater(std::string("apple"), std::string("banana")) << std::endl;
    
    return 0;
}
```

### Recursive Lambdas

```cpp
#include <iostream>
#include <functional>

int main() {
    // Recursive lambda for factorial
    std::function<int(int)> factorial = [&factorial](int n) -> int {
        return (n <= 1) ? 1 : n * factorial(n - 1);
    };
    
    std::cout << "5! = " << factorial(5) << std::endl;
    
    // Recursive lambda for Fibonacci
    std::function<int(int)> fibonacci = [&fibonacci](int n) -> int {
        return (n <= 1) ? n : fibonacci(n - 1) + fibonacci(n - 2);
    };
    
    std::cout << "Fibonacci sequence: ";
    for (int i = 0; i < 10; i++) {
        std::cout << fibonacci(i) << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

## When to Use Lambdas

### Good Use Cases

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

int main() {
    std::vector<int> numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    
    // → Short, one-time use functions
    auto evens = std::count_if(numbers.begin(), numbers.end(), [](int n) {
        return n % 2 == 0;
    });
    
    // → Custom comparators
    std::sort(numbers.begin(), numbers.end(), [](int a, int b) {
        return std::abs(a - 5) < std::abs(b - 5);  // Sort by distance from 5
    });
    
    // → Event handlers and callbacks
    auto processData = [](const std::vector<int>& data, std::function<void(int)> callback) {
        for (int value : data) {
            callback(value * 2);
        }
    };
    
    processData(numbers, [](int value) {
        std::cout << value << " ";
    });
    std::cout << std::endl;
    
    return 0;
}
```

### <i class="fa-solid fa-square-xmark"></i> When NOT to Use Lambdas

```cpp
// ✘ Complex logic (use regular functions instead)
auto complexCalculation = [](double a, double b, double c, double d) {
    // 20+ lines of complex mathematical calculations
    // Better as a named function with documentation
};

// ✘ Reused in multiple places (define as function)
auto validateEmail = [](const std::string& email) {
    // Email validation logic
    // If used in multiple places, make it a proper function
};

// ✘ Very simple operations where built-in functions exist
std::sort(numbers.begin(), numbers.end(), [](int a, int b) {
    return a < b;  // Just use std::sort without lambda for default ordering
});
```

## Exercises

### Exercise 1: Lambda Utilities

Create lambdas for:

- String processing (trim, capitalize, word count)
- Number operations (prime check, perfect square)
- Collection manipulation (filter, map, reduce)

### Exercise 2: Configurable Sorters

Build lambda factories that create custom sorting functions:

- Multi-field sorting for structs
- Case-insensitive string sorting
- Numerical sorting with different criteria

### Exercise 3: Event System

Implement a simple event system using lambdas:

- Event registration with lambda handlers
- Event triggering with parameter passing
- Lambda-based filtering and transformation

### Exercise 4: Functional Programming

Practice functional programming patterns:

- Function composition using lambdas
- Currying and partial application
- Pipeline processing with lambda chains

## Key Takeaways

<i class="fa-solid fa-arrow-right"></i> **Lambdas are perfect for short, one-time functions**  
<i class="fa-solid fa-arrow-right"></i> **Use capture clauses appropriately** - by value for safety, by reference when needed  
<i class="fa-solid fa-arrow-right"></i> **Lambdas excel with STL algorithms** and functional programming  
<i class="fa-solid fa-arrow-right"></i> **Consider regular functions for complex or reused logic**  
<i class="fa-solid fa-arrow-right"></i> **Generic lambdas provide type flexibility**  
<i class="fa-solid fa-arrow-right"></i> **Function factories can create specialized behavior**

---

**Congratulations!** You've completed the Functions section. Functions are fundamental to C++ programming, and mastering them enables you to write more organized, reusable, and maintainable code. Next, let's explore memory management // →  [Memory Management](../memory/README.md)
