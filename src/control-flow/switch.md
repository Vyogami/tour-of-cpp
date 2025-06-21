# Switch Statements

The `switch` statement provides a clean way to choose between multiple alternatives based on the value of a single expression. It's particularly useful when you have many possible values to check against, making code more readable than long chains of `if-else` statements.

## Basic Switch Statement

### Syntax

```cpp
switch (expression) {
    case value1:
        // Code for value1
        break;
    case value2:
        // Code for value2
        break;
    default:
        // Code when no case matches
        break;
}
```

### Simple Example

```cpp
#include <iostream>

int main() {
    int dayOfWeek;
    
    std::cout << "Enter day of week (1-7): ";
    std::cin >> dayOfWeek;
    
    switch (dayOfWeek) {
        case 1:
            std::cout << "Monday" << std::endl;
            break;
        case 2:
            std::cout << "Tuesday" << std::endl;
            break;
        case 3:
            std::cout << "Wednesday" << std::endl;
            break;
        case 4:
            std::cout << "Thursday" << std::endl;
            break;
        case 5:
            std::cout << "Friday" << std::endl;
            break;
        case 6:
            std::cout << "Saturday" << std::endl;
            break;
        case 7:
            std::cout << "Sunday" << std::endl;
            break;
        default:
            std::cout << "Invalid day! Enter 1-7." << std::endl;
            break;
    }
    
    return 0;
}
```

## Switch vs If-Else Comparison

### Using If-Else (verbose)

```cpp
#include <iostream>

int main() {
    char grade = 'B';
    
    if (grade == 'A') {
        std::cout << "Excellent! (90-100)" << std::endl;
    } else if (grade == 'B') {
        std::cout << "Good! (80-89)" << std::endl;
    } else if (grade == 'C') {
        std::cout << "Average (70-79)" << std::endl;
    } else if (grade == 'D') {
        std::cout << "Below Average (60-69)" << std::endl;
    } else if (grade == 'F') {
        std::cout << "Failing (0-59)" << std::endl;
    } else {
        std::cout << "Invalid grade!" << std::endl;
    }
    
    return 0;
}
```

### Using Switch (cleaner)

```cpp
#include <iostream>

int main() {
    char grade = 'B';
    
    switch (grade) {
        case 'A':
            std::cout << "Excellent! (90-100)" << std::endl;
            break;
        case 'B':
            std::cout << "Good! (80-89)" << std::endl;
            break;
        case 'C':
            std::cout << "Average (70-79)" << std::endl;
            break;
        case 'D':
            std::cout << "Below Average (60-69)" << std::endl;
            break;
        case 'F':
            std::cout << "Failing (0-59)" << std::endl;
            break;
        default:
            std::cout << "Invalid grade!" << std::endl;
            break;
    }
    
    return 0;
}
```

## The `break` Statement

### Why `break` is Important

Without `break`, execution "falls through" to the next case:

```cpp
#include <iostream>

int main() {
    int number = 2;
    
    std::cout << "Without break:" << std::endl;
    switch (number) {
        case 1:
            std::cout << "One" << std::endl;
        case 2:
            std::cout << "Two" << std::endl;  // This executes
        case 3:
            std::cout << "Three" << std::endl;  // This also executes!
        default:
            std::cout << "Other" << std::endl;  // This also executes!
    }
    
    std::cout << "\nWith break:" << std::endl;
    switch (number) {
        case 1:
            std::cout << "One" << std::endl;
            break;
        case 2:
            std::cout << "Two" << std::endl;  // Only this executes
            break;
        case 3:
            std::cout << "Three" << std::endl;
            break;
        default:
            std::cout << "Other" << std::endl;
            break;
    }
    
    return 0;
}
```

**Output:**

```
Without break:
Two
Three
Other

With break:
Two
```

## Fall-Through Behavior

Sometimes fall-through is intentional and useful:

### Grouping Cases

```cpp
#include <iostream>

int main() {
    char ch = 'A';
    
    switch (ch) {
        case 'a':
        case 'e':
        case 'i':
        case 'o':
        case 'u':
        case 'A':
        case 'E':
        case 'I':
        case 'O':
        case 'U':
            std::cout << ch << " is a vowel" << std::endl;
            break;
        default:
            std::cout << ch << " is a consonant" << std::endl;
            break;
    }
    
    return 0;
}
```

### Cascading Operations

```cpp
#include <iostream>

int main() {
    int securityLevel = 3;
    
    std::cout << "Security level " << securityLevel << " permissions:" << std::endl;
    
    switch (securityLevel) {
        case 5:
            std::cout << "- System administration" << std::endl;
            // Fall through to include lower level permissions
        case 4:
            std::cout << "- User management" << std::endl;
            // Fall through
        case 3:
            std::cout << "- File modification" << std::endl;
            // Fall through
        case 2:
            std::cout << "- File reading" << std::endl;
            // Fall through
        case 1:
            std::cout << "- Basic access" << std::endl;
            break;
        case 0:
            std::cout << "- No access" << std::endl;
            break;
        default:
            std::cout << "- Invalid security level" << std::endl;
            break;
    }
    
    return 0;
}
```

**Output:**

```
Security level 3 permissions:
- File modification
- File reading
- Basic access
```

## Valid Switch Expression Types

Switch works with integral types and enums:

### Integer Types

```cpp
#include <iostream>

int main() {
    short s = 1;
    int i = 2;
    long l = 3;
    
    switch (s) {  // → short
        case 1: std::cout << "Short value" << std::endl; break;
    }
    
    switch (i) {  // → int
        case 2: std::cout << "Int value" << std::endl; break;
    }
    
    switch (l) {  // → long
        case 3: std::cout << "Long value" << std::endl; break;
    }
    
    return 0;
}
```

### Character Types

```cpp
#include <iostream>

int main() {
    char operation = '+';
    
    switch (operation) {  // → char
        case '+':
            std::cout << "Addition" << std::endl;
            break;
        case '-':
            std::cout << "Subtraction" << std::endl;
            break;
        case '*':
            std::cout << "Multiplication" << std::endl;
            break;
        case '/':
            std::cout << "Division" << std::endl;
            break;
        default:
            std::cout << "Unknown operation" << std::endl;
            break;
    }
    
    return 0;
}
```

### Enumerations

```cpp
#include <iostream>

enum class Color {
    Red,
    Green,
    Blue,
    Yellow
};

int main() {
    Color favoriteColor = Color::Blue;
    
    switch (favoriteColor) {  // → enum
        case Color::Red:
            std::cout << "Red like fire!" << std::endl;
            break;
        case Color::Green:
            std::cout << "Green like nature!" << std::endl;
            break;
        case Color::Blue:
            std::cout << "Blue like the sky!" << std::endl;
            break;
        case Color::Yellow:
            std::cout << "Yellow like the sun!" << std::endl;
            break;
    }
    
    return 0;
}
```

### Invalid Types for Switch

```cpp
// ✘ These types cannot be used with switch:

// switch (3.14) { }           // double
// switch ("hello") { }        // string literal
// switch (std::string) { }    // std::string
// switch (array) { }          // arrays
// switch (ptr) { }           // pointers (unless checking against specific addresses)
```

## Practical Examples

### Simple Calculator

```cpp
#include <iostream>

int main() {
    double num1, num2;
    char operation;
    
    std::cout << "Enter first number: ";
    std::cin >> num1;
    
    std::cout << "Enter operation (+, -, *, /): ";
    std::cin >> operation;
    
    std::cout << "Enter second number: ";
    std::cin >> num2;
    
    switch (operation) {
        case '+':
            std::cout << "Result: " << num1 + num2 << std::endl;
            break;
        case '-':
            std::cout << "Result: " << num1 - num2 << std::endl;
            break;
        case '*':
            std::cout << "Result: " << num1 * num2 << std::endl;
            break;
        case '/':
            if (num2 != 0) {
                std::cout << "Result: " << num1 / num2 << std::endl;
            } else {
                std::cout << "Error: Division by zero!" << std::endl;
            }
            break;
        default:
            std::cout << "Error: Invalid operation!" << std::endl;
            break;
    }
    
    return 0;
}
```

### Menu System

```cpp
#include <iostream>

int main() {
    int choice;
    bool running = true;
    
    while (running) {
        std::cout << "\n=== GAME MENU ===" << std::endl;
        std::cout << "1. New Game" << std::endl;
        std::cout << "2. Load Game" << std::endl;
        std::cout << "3. Settings" << std::endl;
        std::cout << "4. High Scores" << std::endl;
        std::cout << "5. Exit" << std::endl;
        std::cout << "Enter your choice (1-5): ";
        
        std::cin >> choice;
        
        switch (choice) {
            case 1:
                std::cout << "Starting new game..." << std::endl;
                // Game logic would go here
                break;
            case 2:
                std::cout << "Loading saved game..." << std::endl;
                // Load logic would go here
                break;
            case 3:
                std::cout << "Opening settings..." << std::endl;
                // Settings logic would go here
                break;
            case 4:
                std::cout << "Displaying high scores..." << std::endl;
                // High score logic would go here
                break;
            case 5:
                std::cout << "Thank you for playing! Goodbye!" << std::endl;
                running = false;
                break;
            default:
                std::cout << "Invalid choice! Please enter 1-5." << std::endl;
                break;
        }
    }
    
    return 0;
}
```

### Month Days Calculator

```cpp
#include <iostream>

int main() {
    int month, year;
    
    std::cout << "Enter month (1-12): ";
    std::cin >> month;
    
    std::cout << "Enter year: ";
    std::cin >> year;
    
    int days;
    
    switch (month) {
        case 1:   // January
        case 3:   // March
        case 5:   // May
        case 7:   // July
        case 8:   // August
        case 10:  // October
        case 12:  // December
            days = 31;
            break;
            
        case 4:   // April
        case 6:   // June
        case 9:   // September
        case 11:  // November
            days = 30;
            break;
            
        case 2:   // February
            // Check for leap year
            if ((year % 4 == 0 && year % 100 != 0) || (year % 400 == 0)) {
                days = 29;  // Leap year
            } else {
                days = 28;  // Non-leap year
            }
            break;
            
        default:
            std::cout << "Invalid month! Enter 1-12." << std::endl;
            return 1;
    }
    
    std::cout << "Month " << month << " in year " << year 
              << " has " << days << " days." << std::endl;
    
    return 0;
}
```

## Switch with Functions

### Organizing Code with Functions

```cpp
#include <iostream>

void printWelcome() {
    std::cout << "Welcome to the application!" << std::endl;
}

void printHelp() {
    std::cout << "Available commands:" << std::endl;
    std::cout << "1 - Start process" << std::endl;
    std::cout << "2 - Show status" << std::endl;
    std::cout << "3 - Help" << std::endl;
    std::cout << "4 - Exit" << std::endl;
}

void startProcess() {
    std::cout << "Process started successfully!" << std::endl;
}

void showStatus() {
    std::cout << "System status: All systems operational" << std::endl;
}

int main() {
    int command;
    
    printWelcome();
    
    do {
        std::cout << "\nEnter command (1-4): ";
        std::cin >> command;
        
        switch (command) {
            case 1:
                startProcess();
                break;
            case 2:
                showStatus();
                break;
            case 3:
                printHelp();
                break;
            case 4:
                std::cout << "Exiting..." << std::endl;
                break;
            default:
                std::cout << "Invalid command!" << std::endl;
                printHelp();
                break;
        }
    } while (command != 4);
    
    return 0;
}
```

## Common Mistakes and Best Practices

### <i class="fa-solid fa-square-xmark"></i> Forgetting `break`

```cpp
// ✘ Dangerous: missing break causes fall-through
switch (value) {
    case 1:
        doSomething();
        // Missing break! Falls through to case 2
    case 2:
        doSomethingElse();  // This executes for both case 1 and 2!
        break;
}
```

### <i class="fa-solid fa-square-xmark"></i> Using Non-Integral Types

```cpp
double price = 19.99;
// switch (price) { }  // ✘ Error: can't switch on double

std::string name = "John";
// switch (name) { }   // ✘ Error: can't switch on string
```

### Best Practices

```cpp
#include <iostream>

int main() {
    int choice = 2;
    
    switch (choice) {
        case 1:
            std::cout << "Option 1" << std::endl;
            break;
            
        case 2:
            std::cout << "Option 2" << std::endl;
            break;
            
        case 3:
            std::cout << "Option 3" << std::endl;
            break;
            
        default:  // → Always include default case
            std::cout << "Invalid option" << std::endl;
            break;  // → Include break in default too
    }
    
    return 0;
}
```

## When to Use Switch vs If-Else

### Use Switch When

- <i class="fa-solid fa-arrow-right"></i> Comparing a single variable against multiple constant values
- <i class="fa-solid fa-arrow-right"></i> Values are integral types (int, char, enum)
- <i class="fa-solid fa-arrow-right"></i> You have many possible values (3+ cases)
- <i class="fa-solid fa-arrow-right"></i> Values are known at compile time

### Use If-Else When

- <i class="fa-solid fa-arrow-right"></i> Comparing different variables
- <i class="fa-solid fa-arrow-right"></i> Using complex conditions (ranges, logical operators)
- <i class="fa-solid fa-arrow-right"></i> Working with floating-point numbers or strings
- <i class="fa-solid fa-arrow-right"></i> Need different comparison operators (>, <, !=)

```cpp
#include <iostream>

int main() {
    int age = 25;
    double salary = 50000.0;
    
    // → Use if-else for complex conditions
    if (age >= 18 && age <= 65 && salary > 30000) {
        std::cout << "Eligible for loan" << std::endl;
    }
    
    // → Use switch for simple value matching
    int dayType;
    switch (age / 10) {  // Age groups
        case 0:
        case 1:
            dayType = 1;  // Child
            break;
        case 2:
        case 3:
        case 4:
        case 5:
            dayType = 2;  // Adult
            break;
        default:
            dayType = 3;  // Senior
            break;
    }
    
    return 0;
}
```

## Exercises

### Exercise 1: Grade Calculator

Create a program that converts numerical scores to letter grades using switch:

- 90-100: A
- 80-89: B  
- 70-79: C
- 60-69: D
- Below 60: F

### Exercise 2: Roman Numeral Converter

Write a program that converts single digits (1-9) to Roman numerals using switch.

### Exercise 3: Simple Text Adventure

Create a text-based adventure game using switch statements for different room choices and actions.

### Exercise 4: Calendar Information

Build a program that provides information about months (days, season, holidays) using switch.

## Key Takeaways

<i class="fa-solid fa-arrow-right"></i> **Use switch for multiple value comparisons** of integral types  
<i class="fa-solid fa-arrow-right"></i> **Always include `break`** unless fall-through is intentional  
<i class="fa-solid fa-arrow-right"></i> **Always include a `default` case** for unexpected values  
<i class="fa-solid fa-arrow-right"></i> **Group related cases** using fall-through when appropriate  
<i class="fa-solid fa-arrow-right"></i> **Consider if-else for complex conditions** and non-integral types  
<i class="fa-solid fa-arrow-right"></i> **Use meaningful case labels** and organize logically

---

**Congratulations!** You've completed the Control Flow section. Next, let's explore how to organize code into reusable blocks // →  [Functions](../functions/README.md)
