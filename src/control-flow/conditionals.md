# Conditional Statements

Conditional statements allow your program to make decisions and execute different code paths based on whether conditions are true or false. They're fundamental to creating interactive and responsive programs.

## Basic `if` Statement

The `if` statement executes code only when a condition is true:

```cpp
#include <iostream>

int main() {
    int temperature = 25;
    
    if (temperature > 20) {
        std::cout << "It's a warm day!" << std::endl;
    }
    
    std::cout << "Program continues..." << std::endl;
    
    return 0;
}
```

**Output:**

```
It's a warm day!
Program continues...
```

### Syntax and Structure

```cpp
if (condition) {
    // Code to execute if condition is true
}
```

- **Condition**: Must evaluate to `true` or `false`
- **Braces**: `{}` define the code block (always use them for clarity)
- **Indentation**: Makes code readable (usually 4 spaces or 1 tab)

## `if-else` Statement

Execute one block of code if condition is true, another if false:

```cpp
#include <iostream>

int main() {
    int age;
    
    std::cout << "Enter your age: ";
    std::cin >> age;
    
    if (age >= 18) {
        std::cout << "You are an adult." << std::endl;
    } else {
        std::cout << "You are a minor." << std::endl;
    }
    
    return 0;
}
```

### Syntax

```cpp
if (condition) {
    // Code if condition is true
} else {
    // Code if condition is false
}
```

## `else if` for Multiple Conditions

Handle multiple exclusive conditions with `else if`:

```cpp
#include <iostream>

int main() {
    int score;
    
    std::cout << "Enter your test score (0-100): ";
    std::cin >> score;
    
    if (score >= 90) {
        std::cout << "Grade: A (Excellent!)" << std::endl;
    } else if (score >= 80) {
        std::cout << "Grade: B (Good!)" << std::endl;
    } else if (score >= 70) {
        std::cout << "Grade: C (Average)" << std::endl;
    } else if (score >= 60) {
        std::cout << "Grade: D (Below Average)" << std::endl;
    } else {
        std::cout << "Grade: F (Failing)" << std::endl;
    }
    
    return 0;
}
```

### How `else if` Works

- Conditions are checked **in order** from top to bottom
- Only the **first true condition** executes its block
- Remaining conditions are **skipped**
- If no condition is true, the `else` block executes (if present)

## Complex Conditions

### Logical Operators in Conditions

```cpp
#include <iostream>

int main() {
    int age;
    bool hasLicense;
    
    std::cout << "Enter your age: ";
    std::cin >> age;
    
    std::cout << "Do you have a driver's license? (1 for yes, 0 for no): ";
    std::cin >> hasLicense;
    
    // AND operator: both conditions must be true
    if (age >= 16 && hasLicense) {
        std::cout << "You can drive!" << std::endl;
    } else if (age >= 16 && !hasLicense) {
        std::cout << "You're old enough but need a license." << std::endl;
    } else {
        std::cout << "You're too young to drive." << std::endl;
    }
    
    return 0;
}
```

### Range Checking

```cpp
#include <iostream>

int main() {
    int number;
    
    std::cout << "Enter a number: ";
    std::cin >> number;
    
    // Check if number is in range [10, 20]
    if (number >= 10 && number <= 20) {
        std::cout << "Number is between 10 and 20 (inclusive)" << std::endl;
    } else {
        std::cout << "Number is outside the range [10, 20]" << std::endl;
    }
    
    // Check if number is outside range
    if (number < 0 || number > 100) {
        std::cout << "Number is outside normal range!" << std::endl;
    }
    
    return 0;
}
```

## Nested Conditionals

You can place `if` statements inside other `if` statements:

```cpp
#include <iostream>

int main() {
    bool isWeekend;
    bool isRaining;
    
    std::cout << "Is it weekend? (1 for yes, 0 for no): ";
    std::cin >> isWeekend;
    
    std::cout << "Is it raining? (1 for yes, 0 for no): ";
    std::cin >> isRaining;
    
    if (isWeekend) {
        std::cout << "It's weekend! ";
        
        if (isRaining) {
            std::cout << "Perfect day for indoor activities." << std::endl;
        } else {
            std::cout << "Great day for outdoor activities!" << std::endl;
        }
    } else {
        std::cout << "It's a weekday. ";
        
        if (isRaining) {
            std::cout << "Don't forget your umbrella for work!" << std::endl;
        } else {
            std::cout << "Nice weather for commuting." << std::endl;
        }
    }
    
    return 0;
}
```

<i class="fa-solid fa-triangle-exclamation"></i> **Warning**: Avoid deep nesting (more than 2-3 levels) as it makes code hard to read.

## Guard Clauses (Early Returns)

Instead of deep nesting, use guard clauses for cleaner code:

```cpp
#include <iostream>

// ✘ Deeply nested (harder to read)
void processAge_Nested(int age) {
    if (age >= 0) {
        if (age <= 150) {
            if (age >= 18) {
                std::cout << "Adult: " << age << " years old" << std::endl;
            } else {
                std::cout << "Minor: " << age << " years old" << std::endl;
            }
        } else {
            std::cout << "Error: Age too high" << std::endl;
        }
    } else {
        std::cout << "Error: Age cannot be negative" << std::endl;
    }
}

// → Guard clauses (cleaner and easier to read)
void processAge_Guards(int age) {
    // Handle error cases first
    if (age < 0) {
        std::cout << "Error: Age cannot be negative" << std::endl;
        return;
    }
    
    if (age > 150) {
        std::cout << "Error: Age too high" << std::endl;
        return;
    }
    
    // Main logic for valid input
    if (age >= 18) {
        std::cout << "Adult: " << age << " years old" << std::endl;
    } else {
        std::cout << "Minor: " << age << " years old" << std::endl;
    }
}

int main() {
    processAge_Guards(25);   // Adult: 25 years old
    processAge_Guards(-5);   // Error: Age cannot be negative
    processAge_Guards(200);  // Error: Age too high
    
    return 0;
}
```

## Common Patterns and Examples

### Input Validation

```cpp
#include <iostream>

int main() {
    int choice;
    
    std::cout << "Menu:" << std::endl;
    std::cout << "1. Start Game" << std::endl;
    std::cout << "2. Options" << std::endl;
    std::cout << "3. Exit" << std::endl;
    std::cout << "Enter your choice (1-3): ";
    
    std::cin >> choice;
    
    if (choice >= 1 && choice <= 3) {
        if (choice == 1) {
            std::cout << "Starting game..." << std::endl;
        } else if (choice == 2) {
            std::cout << "Opening options..." << std::endl;
        } else {  // choice == 3
            std::cout << "Goodbye!" << std::endl;
        }
    } else {
        std::cout << "Invalid choice! Please enter 1, 2, or 3." << std::endl;
    }
    
    return 0;
}
```

### User Authentication

```cpp
#include <iostream>
#include <string>

int main() {
    std::string username;
    std::string password;
    
    std::cout << "Login System" << std::endl;
    std::cout << "Username: ";
    std::cin >> username;
    
    std::cout << "Password: ";
    std::cin >> password;
    
    // Simple authentication (in real systems, use proper security!)
    if (username == "admin" && password == "secret123") {
        std::cout << "Login successful! Welcome, " << username << "!" << std::endl;
    } else if (username == "admin") {
        std::cout << "Incorrect password for user: " << username << std::endl;
    } else {
        std::cout << "User not found: " << username << std::endl;
    }
    
    return 0;
}
```

### Number Classification

```cpp
#include <iostream>

int main() {
    int number;
    
    std::cout << "Enter an integer: ";
    std::cin >> number;
    
    // Multiple independent conditions
    std::cout << "Analysis of " << number << ":" << std::endl;
    
    if (number == 0) {
        std::cout << "- Zero" << std::endl;
    } else if (number > 0) {
        std::cout << "- Positive" << std::endl;
    } else {
        std::cout << "- Negative" << std::endl;
    }
    
    if (number % 2 == 0) {
        std::cout << "- Even" << std::endl;
    } else {
        std::cout << "- Odd" << std::endl;
    }
    
    if (number >= 10 && number <= 99) {
        std::cout << "- Two digits" << std::endl;
    } else if (number >= 100) {
        std::cout << "- Three or more digits" << std::endl;
    } else if (number >= 0) {
        std::cout << "- One digit" << std::endl;
    }
    
    return 0;
}
```

## Short-Circuit Evaluation

Logical operators use short-circuit evaluation for efficiency:

```cpp
#include <iostream>

bool expensiveCheck() {
    std::cout << "Expensive check called!" << std::endl;
    return true;
}

int main() {
    bool quickCheck = false;
    
    std::cout << "Testing AND short-circuit:" << std::endl;
    // expensiveCheck() is NOT called because quickCheck is false
    if (quickCheck && expensiveCheck()) {
        std::cout << "Both conditions true" << std::endl;
    }
    
    quickCheck = true;
    std::cout << "\nTesting OR short-circuit:" << std::endl;
    // expensiveCheck() is NOT called because quickCheck is true
    if (quickCheck || expensiveCheck()) {
        std::cout << "At least one condition true" << std::endl;
    }
    
    return 0;
}
```

**Output:**

```
Testing AND short-circuit:

Testing OR short-circuit:
At least one condition true
```

## Common Mistakes and Best Practices

### <i class="fa-solid fa-square-xmark"></i> Assignment vs Equality

```cpp
int x = 5;

// ✘ Wrong: assigns 10 to x (always true)
if (x = 10) {
    std::cout << "This always executes!" << std::endl;
}

// → Correct: compares x with 10
if (x == 10) {
    std::cout << "x equals 10" << std::endl;
}
```

### <i class="fa-solid fa-square-xmark"></i> Floating-Point Comparison

```cpp
double a = 0.1 + 0.2;
double b = 0.3;

// ✘ Dangerous: may not work due to floating-point precision
if (a == b) {
    std::cout << "Equal" << std::endl;
}

// → Safe: use tolerance for floating-point comparison
double epsilon = 1e-9;
if (std::abs(a - b) < epsilon) {
    std::cout << "Equal (within tolerance)" << std::endl;
}
```

### <i class="fa-solid fa-square-xmark"></i> Always Use Braces

```cpp
// ✘ Dangerous: without braces, only first line is in if
if (condition)
    std::cout << "Line 1" << std::endl;
    std::cout << "Line 2" << std::endl;  // Always executes!

// → Safe: always use braces
if (condition) {
    std::cout << "Line 1" << std::endl;
    std::cout << "Line 2" << std::endl;  // Both lines in if block
}
```

### Positive Conditions

```cpp
// ✘ Harder to read (negative condition)
if (!isNotReady) {
    startProcess();
}

// → Easier to read (positive condition)
if (isReady) {
    startProcess();
}
```

## Advanced Conditional Patterns

### Ternary Operator for Simple Cases

```cpp
#include <iostream>

int main() {
    int a = 10, b = 20;
    
    // Simple conditional assignment
    int max = (a > b) ? a : b;
    std::cout << "Maximum: " << max << std::endl;
    
    // Conditional output
    std::cout << "Number is " << ((a % 2 == 0) ? "even" : "odd") << std::endl;
    
    return 0;
}
```

### Condition Variables

```cpp
#include <iostream>

int main() {
    int score = 85;
    
    // Calculate conditions once
    bool isExcellent = score >= 90;
    bool isGood = score >= 80;
    bool isPassing = score >= 60;
    
    if (isExcellent) {
        std::cout << "Outstanding performance!" << std::endl;
    } else if (isGood) {
        std::cout << "Good job!" << std::endl;
    } else if (isPassing) {
        std::cout << "You passed." << std::endl;
    } else {
        std::cout << "Need improvement." << std::endl;
    }
    
    return 0;
}
```

## Exercises

### Exercise 1: Grade Calculator

Write a program that:

- Takes a numerical score (0-100)
- Outputs the letter grade (A, B, C, D, F)
- Handles invalid input gracefully

### Exercise 2: Triangle Classifier

Create a program that takes three side lengths and determines:

- If they can form a valid triangle
- What type of triangle it is (equilateral, isosceles, scalene)

### Exercise 3: Leap Year Calculator

Write a program that determines if a given year is a leap year using these rules:

- Divisible by 4 // →  leap year
- BUT if divisible by 100 // →  not a leap year  
- BUT if divisible by 400 // →  leap year

## Key Takeaways

<i class="fa-solid fa-arrow-right"></i> **Always use braces** `{}` even for single statements  
<i class="fa-solid fa-arrow-right"></i> **Use positive conditions** when possible for clarity  
<i class="fa-solid fa-arrow-right"></i> **Prefer guard clauses** over deep nesting  
<i class="fa-solid fa-arrow-right"></i> **Be careful with assignment vs equality** (`=` vs `==`)  
<i class="fa-solid fa-arrow-right"></i> **Handle edge cases** and invalid input  
<i class="fa-solid fa-arrow-right"></i> **Use meaningful variable names** for conditions

---

**Next**: Learn how to repeat code with loops // →  [Loops and Iteration](loops.md)
