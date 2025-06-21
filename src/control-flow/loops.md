# Loops and Iteration

Loops allow you to repeat code multiple times without writing the same statements over and over. They're essential for processing collections of data, implementing algorithms, and creating interactive programs.

## Why Use Loops?

Without loops, repetitive tasks would require massive amounts of code:

```cpp
// ✘ Without loops - repetitive and inflexible
#include <iostream>
int main() {
    std::cout << "1" << std::endl;
    std::cout << "2" << std::endl;
    std::cout << "3" << std::endl;
    std::cout << "4" << std::endl;
    std::cout << "5" << std::endl;
    // What if we want to count to 100? Or 1000?
    return 0;
}
```

```cpp
// → With loops - concise and flexible
#include <iostream>
int main() {
    for (int i = 1; i <= 5; i++) {
        std::cout << i << std::endl;
    }
    // Easy to change the range!
    return 0;
}
```

## Types of Loops in C++

C++ provides three main types of loops:

1. **`for` loop**: When you know how many iterations you need
2. **`while` loop**: When you repeat based on a condition
3. **`do-while` loop**: When you need to execute at least once

## The `for` Loop

Perfect when you know the number of iterations in advance:

### Basic `for` Loop Syntax

```cpp
for (initialization; condition; increment/decrement) {
    // Code to repeat
}
```

### Simple Counting Example

```cpp
#include <iostream>

int main() {
    // Count from 1 to 10
    for (int i = 1; i <= 10; i++) {
        std::cout << i << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

**Output:**

```
1 2 3 4 5 6 7 8 9 10
```

### How `for` Loop Works

1. **Initialization**: `int i = 1` - executed once at the start
2. **Condition**: `i <= 10` - checked before each iteration
3. **Loop Body**: `std::cout << i << " "` - executed if condition is true
4. **Increment**: `i++` - executed after each iteration
5. **Repeat**: Go back to step 2

### Common `for` Loop Patterns

```cpp
#include <iostream>

int main() {
    // Count up by 1
    std::cout << "Counting up: ";
    for (int i = 0; i < 5; i++) {
        std::cout << i << " ";
    }
    std::cout << std::endl;
    
    // Count down
    std::cout << "Counting down: ";
    for (int i = 5; i > 0; i--) {
        std::cout << i << " ";
    }
    std::cout << std::endl;
    
    // Count by 2s
    std::cout << "Even numbers: ";
    for (int i = 0; i <= 10; i += 2) {
        std::cout << i << " ";
    }
    std::cout << std::endl;
    
    // Count with different step
    std::cout << "Multiples of 5: ";
    for (int i = 5; i <= 25; i += 5) {
        std::cout << i << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

**Output:**

```
Counting up: 0 1 2 3 4 
Counting down: 5 4 3 2 1 
Even numbers: 0 2 4 6 8 10 
Multiples of 5: 5 10 15 20 25
```

## The `while` Loop

Use `while` when you don't know exactly how many iterations you need:

### Basic `while` Loop Syntax

```cpp
while (condition) {
    // Code to repeat
    // Make sure to modify the condition variable!
}
```

### Simple Example

```cpp
#include <iostream>

int main() {
    int count = 1;
    
    while (count <= 5) {
        std::cout << "Count: " << count << std::endl;
        count++;  // Don't forget to increment!
    }
    
    std::cout << "Loop finished, count is now: " << count << std::endl;
    
    return 0;
}
```

**Output:**

```
Count: 1
Count: 2
Count: 3
Count: 4
Count: 5
Loop finished, count is now: 6
```

### Input Validation with `while`

```cpp
#include <iostream>

int main() {
    int number;
    
    std::cout << "Enter a number between 1 and 10: ";
    std::cin >> number;
    
    // Keep asking until valid input
    while (number < 1 || number > 10) {
        std::cout << "Invalid! Enter a number between 1 and 10: ";
        std::cin >> number;
    }
    
    std::cout << "Thank you! You entered: " << number << std::endl;
    
    return 0;
}
```

### Sentinel-Controlled Loop

```cpp
#include <iostream>

int main() {
    int sum = 0;
    int number;
    
    std::cout << "Enter numbers to sum (enter 0 to stop):" << std::endl;
    
    std::cin >> number;
    while (number != 0) {
        sum += number;
        std::cout << "Running total: " << sum << std::endl;
        std::cout << "Enter next number (0 to stop): ";
        std::cin >> number;
    }
    
    std::cout << "Final sum: " << sum << std::endl;
    
    return 0;
}
```

## The `do-while` Loop

Executes the loop body at least once, then checks the condition:

### Basic `do-while` Syntax

```cpp
do {
    // Code to repeat (executes at least once)
} while (condition);
```

### Menu System Example

```cpp
#include <iostream>

int main() {
    int choice;
    
    do {
        // Display menu
        std::cout << "\n=== MENU ===" << std::endl;
        std::cout << "1. Play Game" << std::endl;
        std::cout << "2. View Scores" << std::endl;
        std::cout << "3. Settings" << std::endl;
        std::cout << "4. Exit" << std::endl;
        std::cout << "Enter your choice (1-4): ";
        
        std::cin >> choice;
        
        // Process choice
        switch (choice) {
            case 1:
                std::cout << "Starting game..." << std::endl;
                break;
            case 2:
                std::cout << "Displaying scores..." << std::endl;
                break;
            case 3:
                std::cout << "Opening settings..." << std::endl;
                break;
            case 4:
                std::cout << "Goodbye!" << std::endl;
                break;
            default:
                std::cout << "Invalid choice! Try again." << std::endl;
        }
        
    } while (choice != 4);
    
    return 0;
}
```

### When to Use `do-while`

- Menu systems (always show menu at least once)
- Input validation (always ask for input at least once)
- Game loops (always play at least one round)

## Nested Loops

Loops inside other loops for multi-dimensional problems:

### Multiplication Table

```cpp
#include <iostream>
#include <iomanip>

int main() {
    std::cout << "Multiplication Table:" << std::endl;
    
    // Outer loop for rows
    for (int row = 1; row <= 10; row++) {
        // Inner loop for columns
        for (int col = 1; col <= 10; col++) {
            std::cout << std::setw(4) << (row * col);
        }
        std::cout << std::endl;  // New line after each row
    }
    
    return 0;
}
```

### Pattern Printing

```cpp
#include <iostream>

int main() {
    int size = 5;
    
    std::cout << "Triangle pattern:" << std::endl;
    for (int row = 1; row <= size; row++) {
        for (int col = 1; col <= row; col++) {
            std::cout << "*";
        }
        std::cout << std::endl;
    }
    
    std::cout << "\nSquare pattern:" << std::endl;
    for (int row = 1; row <= size; row++) {
        for (int col = 1; col <= size; col++) {
            if (row == 1 || row == size || col == 1 || col == size) {
                std::cout << "*";
            } else {
                std::cout << " ";
            }
        }
        std::cout << std::endl;
    }
    
    return 0;
}
```

**Output:**

```
Triangle pattern:
*
**
***
****
*****

Square pattern:
*****
*   *
*   *
*   *
*****
```

## Loop Control Statements

### `break` Statement

Exits the loop immediately:

```cpp
#include <iostream>

int main() {
    std::cout << "Looking for number 7:" << std::endl;
    
    for (int i = 1; i <= 10; i++) {
        if (i == 7) {
            std::cout << "Found 7! Breaking out of loop." << std::endl;
            break;  // Exit the loop immediately
        }
        std::cout << i << " ";
    }
    
    std::cout << "\nLoop finished." << std::endl;
    
    return 0;
}
```

**Output:**

```
Looking for number 7:
1 2 3 4 5 6 Found 7! Breaking out of loop.
Loop finished.
```

### `continue` Statement

Skips the rest of the current iteration:

```cpp
#include <iostream>

int main() {
    std::cout << "Odd numbers from 1 to 10:" << std::endl;
    
    for (int i = 1; i <= 10; i++) {
        if (i % 2 == 0) {
            continue;  // Skip even numbers
        }
        std::cout << i << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

**Output:**

```
Odd numbers from 1 to 10:
1 3 5 7 9
```

### `break` and `continue` in Nested Loops

```cpp
#include <iostream>

int main() {
    std::cout << "Breaking out of nested loops:" << std::endl;
    
    for (int i = 1; i <= 3; i++) {
        std::cout << "Outer loop: " << i << std::endl;
        
        for (int j = 1; j <= 5; j++) {
            if (j == 3) {
                std::cout << "  Breaking inner loop at j=" << j << std::endl;
                break;  // Only breaks inner loop
            }
            std::cout << "  Inner loop: " << j << std::endl;
        }
    }
    
    return 0;
}
```

**Output:**

```
Breaking out of nested loops:
Outer loop: 1
  Inner loop: 1
  Inner loop: 2
  Breaking inner loop at j=3
Outer loop: 2
  Inner loop: 1
  Inner loop: 2
  Breaking inner loop at j=3
Outer loop: 3
  Inner loop: 1
  Inner loop: 2
  Breaking inner loop at j=3
```

## Infinite Loops and How to Avoid Them

### <i class="fa-solid fa-square-xmark"></i> Infinite Loop Examples

```cpp
// ✘ Forgot to increment
int i = 0;
while (i < 10) {
    std::cout << i << std::endl;
    // Missing: i++;  This creates infinite loop!
}

// ✘ Wrong condition
for (int i = 0; i >= 0; i++) {  // i will always be >= 0
    std::cout << i << std::endl;
}

// ✘ Modifying loop variable incorrectly
for (int i = 0; i < 10; i++) {
    if (i == 5) {
        i = 0;  // Resets counter, loop never ends!
    }
    std::cout << i << std::endl;
}
```

### How to Avoid Infinite Loops

1. **Always modify the loop variable** inside the loop
2. **Check loop conditions carefully**
3. **Use debugger or print statements** to trace execution
4. **Add safety counters** for complex conditions

```cpp
#include <iostream>

int main() {
    int attempts = 0;
    const int MAX_ATTEMPTS = 5;
    bool success = false;
    
    while (!success && attempts < MAX_ATTEMPTS) {
        attempts++;
        
        std::cout << "Attempt " << attempts << ": ";
        // Simulate some operation that might succeed
        if (attempts >= 3) {  // Succeed on 3rd attempt
            success = true;
            std::cout << "Success!" << std::endl;
        } else {
            std::cout << "Failed, trying again..." << std::endl;
        }
    }
    
    if (!success) {
        std::cout << "Max attempts reached, giving up." << std::endl;
    }
    
    return 0;
}
```

## Common Loop Patterns

### Accumulator Pattern

```cpp
#include <iostream>

int main() {
    // Sum of numbers 1 to 100
    int sum = 0;
    for (int i = 1; i <= 100; i++) {
        sum += i;
    }
    std::cout << "Sum of 1 to 100: " << sum << std::endl;
    
    // Product of numbers 1 to 10 (factorial)
    long long factorial = 1;
    for (int i = 1; i <= 10; i++) {
        factorial *= i;
    }
    std::cout << "10! = " << factorial << std::endl;
    
    return 0;
}
```

### Counter Pattern

```cpp
#include <iostream>

int main() {
    int positiveCount = 0;
    int negativeCount = 0;
    int zeroCount = 0;
    
    int numbers[] = {5, -3, 0, 8, -1, 0, 12, -7, 0, 4};
    int size = sizeof(numbers) / sizeof(numbers[0]);
    
    for (int i = 0; i < size; i++) {
        if (numbers[i] > 0) {
            positiveCount++;
        } else if (numbers[i] < 0) {
            negativeCount++;
        } else {
            zeroCount++;
        }
    }
    
    std::cout << "Positive numbers: " << positiveCount << std::endl;
    std::cout << "Negative numbers: " << negativeCount << std::endl;
    std::cout << "Zeros: " << zeroCount << std::endl;
    
    return 0;
}
```

### Search Pattern

```cpp
#include <iostream>

int main() {
    int numbers[] = {10, 25, 7, 42, 18, 33, 9};
    int size = sizeof(numbers) / sizeof(numbers[0]);
    int target = 42;
    bool found = false;
    int position = -1;
    
    // Linear search
    for (int i = 0; i < size; i++) {
        if (numbers[i] == target) {
            found = true;
            position = i;
            break;  // Stop searching once found
        }
    }
    
    if (found) {
        std::cout << "Found " << target << " at position " << position << std::endl;
    } else {
        std::cout << target << " not found in the array." << std::endl;
    }
    
    return 0;
}
```

## Choosing the Right Loop

### Use `for` when

- You know the exact number of iterations
- Working with arrays or containers
- Counting or iterating through a range

### Use `while` when

- The number of iterations depends on a condition
- Reading input until a sentinel value
- Implementing algorithms with unknown iteration count

### Use `do-while` when

- You need to execute the loop body at least once
- Menu systems
- Input validation

## Performance Considerations

### Loop Optimization Tips

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> data = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    
    // → Good: calculate size once
    int size = data.size();
    for (int i = 0; i < size; i++) {
        std::cout << data[i] << " ";
    }
    
    // ✘ Less efficient: calculates size every iteration
    // for (int i = 0; i < data.size(); i++) {
    //     std::cout << data[i] << " ";
    // }
    
    return 0;
}
```

## Exercises

### Exercise 1: Number Guessing Game

Create a number guessing game where:

- Computer picks a random number 1-100
- User guesses until correct
- Provide "too high" or "too low" hints
- Count the number of attempts

### Exercise 2: Prime Number Checker

Write a program that finds all prime numbers between 1 and 100.

### Exercise 3: Simple Calculator

Create a calculator that:

- Shows a menu of operations
- Performs calculations in a loop
- Exits when user chooses to quit

### Exercise 4: Pattern Generator

Write a program that generates various patterns using nested loops (triangles, diamonds, etc.).

## Key Takeaways

<i class="fa-solid fa-arrow-right"></i> **Choose the right loop type** for your specific need  
<i class="fa-solid fa-arrow-right"></i> **Always ensure loop termination** to avoid infinite loops  
<i class="fa-solid fa-arrow-right"></i> **Use meaningful variable names** for loop counters  
<i class="fa-solid fa-arrow-right"></i> **Consider performance** in nested loops  
<i class="fa-solid fa-arrow-right"></i> **Use `break` and `continue`** appropriately for control  
<i class="fa-solid fa-arrow-right"></i> **Test edge cases** (empty ranges, single iterations)

---

**Next**: Learn about multi-way branching // →  [Switch Statements](switch.md)
