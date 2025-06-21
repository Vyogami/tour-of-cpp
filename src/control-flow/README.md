# Control Flow

Control flow statements determine the order in which your program executes code. Instead of running line by line from top to bottom, you can make decisions, repeat code, and jump to different sections based on conditions.

## What You'll Learn

- **Conditional Statements** - Making decisions with `if`, `else if`, and `else`
- **Loops and Iteration** - Repeating code with `for`, `while`, and `do-while`
- **Switch Statements** - Multi-way branching for cleaner code
- **Loop Control** - Using `break`, `continue`, and `goto`

## Types of Control Flow

Every programming language provides these essential control structures:

**Sequential**: Code executes line by line (default behavior)  
**Conditional**: Code executes based on true/false conditions  
**Iterative**: Code repeats until a condition is met  
**Jump**: Code jumps to a different location (functions, break, continue)

## Why Control Flow Matters

Without control flow, programs would be linear and limited:

```cpp
// ✘ Without control flow - always the same output
#include <iostream>
int main() {
    std::cout << "Hello" << std::endl;
    std::cout << "World" << std::endl;
    return 0;
}
```

```cpp
// → With control flow - dynamic and interactive
#include <iostream>
int main() {
    int hour;
    std::cout << "What hour is it (0-23)? ";
    std::cin >> hour;
    
    if (hour < 12) {
        std::cout << "Good morning!" << std::endl;
    } else if (hour < 18) {
        std::cout << "Good afternoon!" << std::endl;
    } else {
        std::cout << "Good evening!" << std::endl;
    }
    
    return 0;
}
```

## Control Flow Categories

### Decision Making

```cpp
// Simple decisions
if (temperature > 30) {
    std::cout << "It's hot!" << std::endl;
}

// Multiple choices
if (grade >= 90) {
    std::cout << "A" << std::endl;
} else if (grade >= 80) {
    std::cout << "B" << std::endl;
} else {
    std::cout << "C or below" << std::endl;
}
```

### Repetition (Loops)

```cpp
// Count from 1 to 5
for (int i = 1; i <= 5; i++) {
    std::cout << i << " ";
}

// Repeat while condition is true
int count = 0;
while (count < 3) {
    std::cout << "Hello" << std::endl;
    count++;
}
```

### Multi-way Branching

```cpp
switch (dayOfWeek) {
    case 1:
        std::cout << "Monday" << std::endl;
        break;
    case 2:
        std::cout << "Tuesday" << std::endl;
        break;
    // ... more cases
    default:
        std::cout << "Invalid day" << std::endl;
}
```

## Program Flow Visualization

Here's how control flow changes program execution:

```cpp
#include <iostream>

int main() {
    std::cout << "1. Start of program" << std::endl;
    
    bool condition = true;
    if (condition) {                          // Decision point
        std::cout << "2. Inside if block" << std::endl;
    } else {
        std::cout << "2. Inside else block" << std::endl;  // Not executed
    }
    
    for (int i = 0; i < 3; i++) {             // Loop begins
        std::cout << "3. Loop iteration " << i << std::endl;
    }                                         // Loop ends
    
    std::cout << "4. End of program" << std::endl;
    
    return 0;
}
```

**Output:**

```
1. Start of program
2. Inside if block
3. Loop iteration 0
3. Loop iteration 1
3. Loop iteration 2
4. End of program
```

## Common Control Flow Patterns

### Guard Clauses (Early Returns)

```cpp
#include <iostream>

bool isValidAge(int age) {
    // → Guard clauses handle special cases first
    if (age < 0) {
        std::cout << "Age cannot be negative" << std::endl;
        return false;
    }
    
    if (age > 150) {
        std::cout << "Age seems unrealistic" << std::endl;
        return false;
    }
    
    // Main logic when input is valid
    std::cout << "Valid age: " << age << std::endl;
    return true;
}
```

### Input Validation Loops

```cpp
#include <iostream>

int main() {
    int number;
    
    // Keep asking until valid input
    while (true) {
        std::cout << "Enter a number between 1 and 10: ";
        std::cin >> number;
        
        if (number >= 1 && number <= 10) {
            break;  // Valid input, exit loop
        }
        
        std::cout << "Invalid! Try again." << std::endl;
    }
    
    std::cout << "You entered: " << number << std::endl;
    return 0;
}
```

### State Machines

```cpp
#include <iostream>

enum class State {
    Idle,
    Running,
    Paused,
    Stopped
};

int main() {
    State currentState = State::Idle;
    char command;
    
    while (true) {
        std::cout << "Enter command (s=start, p=pause, q=quit): ";
        std::cin >> command;
        
        switch (currentState) {
            case State::Idle:
                if (command == 's') {
                    currentState = State::Running;
                    std::cout << "Started!" << std::endl;
                }
                break;
                
            case State::Running:
                if (command == 'p') {
                    currentState = State::Paused;
                    std::cout << "Paused!" << std::endl;
                }
                break;
                
            case State::Paused:
                if (command == 's') {
                    currentState = State::Running;
                    std::cout << "Resumed!" << std::endl;
                }
                break;
        }
        
        if (command == 'q') {
            std::cout << "Goodbye!" << std::endl;
            break;
        }
    }
    
    return 0;
}
```

## Best Practices Preview

As we explore control flow, we'll emphasize:

- **Clear Conditions**: Write readable boolean expressions
- **Avoid Deep Nesting**: Use guard clauses and early returns
- **Consistent Style**: Follow consistent indentation and bracing
- **Loop Safety**: Ensure loops will eventually terminate
- **Error Handling**: Plan for unexpected input or conditions

## Performance Considerations

Control flow affects program performance:

- **Branch Prediction**: Modern CPUs predict which branch will be taken
- **Loop Optimization**: Compilers can optimize well-structured loops
- **Short-Circuit Evaluation**: Logical operators can skip unnecessary evaluations

```cpp
// → Efficient: most common case first
if (commonCondition) {
    // Handle 90% of cases
} else if (lessCommonCondition) {
    // Handle 9% of cases
} else {
    // Handle 1% of cases
}
```

Let's dive into the specific control flow constructs and see how they work in practice!

---

**Sections in This Chapter:**

- [Conditional Statements // → ](conditionals.md)
- [Loops and Iteration // → ](loops.md)
- [Switch Statements // → ](switch.md)
