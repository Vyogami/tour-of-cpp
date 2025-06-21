# Operators and Expressions

Operators are symbols that perform operations on data. Combined with variables and literals, they form expressions that compute new values. Understanding operators is essential for performing calculations, comparisons, and logical operations in C++.

## What are Operators and Expressions?

- **Operator**: A symbol that performs an operation (+, -, *, etc.)
- **Operand**: The data the operator works on
- **Expression**: A combination of operators and operands that produces a value

```cpp
#include <iostream>

int main() {
    int a = 10;
    int b = 3;
    
    int sum = a + b;        // '+' is operator, 'a' and 'b' are operands
    int product = a * b;    // Entire line is an expression
    
    std::cout << "Sum: " << sum << std::endl;
    std::cout << "Product: " << product << std::endl;
    
    return 0;
}
```

## Arithmetic Operators

### Basic Arithmetic

```cpp
#include <iostream>

int main() {
    int a = 15;
    int b = 4;
    
    std::cout << "a = " << a << ", b = " << b << std::endl;
    
    // Basic arithmetic operators
    std::cout << "Addition: " << a + b << std::endl;       // 19
    std::cout << "Subtraction: " << a - b << std::endl;    // 11
    std::cout << "Multiplication: " << a * b << std::endl; // 60
    std::cout << "Division: " << a / b << std::endl;       // 3 (integer division!)
    std::cout << "Modulus: " << a % b << std::endl;        // 3 (remainder)
    
    // Floating-point division
    double result = static_cast<double>(a) / b;
    std::cout << "Float division: " << result << std::endl; // 3.75
    
    return 0;
}
```

### Integer vs Floating-Point Division

```cpp
#include <iostream>

int main() {
    // Integer division truncates
    int intResult = 7 / 3;                    // Result: 2
    
    // Floating-point division preserves decimals
    double doubleResult = 7.0 / 3.0;          // Result: 2.33333...
    
    // Mixed types promote to floating-point
    double mixedResult = 7 / 3.0;             // Result: 2.33333...
    
    std::cout << "Integer division: " << intResult << std::endl;
    std::cout << "Double division: " << doubleResult << std::endl;
    std::cout << "Mixed division: " << mixedResult << std::endl;
    
    return 0;
}
```

### Unary Operators

```cpp
#include <iostream>

int main() {
    int x = 5;
    
    std::cout << "Original x: " << x << std::endl;
    
    // Unary plus and minus
    int positive = +x;                        // +5
    int negative = -x;                        // -5
    
    std::cout << "Unary plus: " << positive << std::endl;
    std::cout << "Unary minus: " << negative << std::endl;
    
    return 0;
}
```

## Assignment Operators

### Basic Assignment

```cpp
#include <iostream>

int main() {
    int x;
    
    x = 10;                                   // Basic assignment
    std::cout << "x = " << x << std::endl;
    
    x = x + 5;                                // Using x in assignment
    std::cout << "x = " << x << std::endl;    // 15
    
    return 0;
}
```

### Compound Assignment Operators

```cpp
#include <iostream>

int main() {
    int x = 10;
    
    std::cout << "Starting x: " << x << std::endl;
    
    x += 5;        // Equivalent to: x = x + 5
    std::cout << "After +=5: " << x << std::endl;     // 15
    
    x -= 3;        // Equivalent to: x = x - 3
    std::cout << "After -=3: " << x << std::endl;     // 12
    
    x *= 2;        // Equivalent to: x = x * 2
    std::cout << "After *=2: " << x << std::endl;     // 24
    
    x /= 4;        // Equivalent to: x = x / 4
    std::cout << "After /=4: " << x << std::endl;     // 6
    
    x %= 4;        // Equivalent to: x = x % 4
    std::cout << "After %=4: " << x << std::endl;     // 2
    
    return 0;
}
```

## Increment and Decrement Operators

### Pre-increment vs Post-increment

```cpp
#include <iostream>

int main() {
    int a = 5;
    int b = 5;
    
    // Pre-increment: increment first, then use value
    int pre_result = ++a;
    std::cout << "Pre-increment: a=" << a << ", result=" << pre_result << std::endl;
    
    // Post-increment: use value first, then increment
    int post_result = b++;
    std::cout << "Post-increment: b=" << b << ", result=" << post_result << std::endl;
    
    // Same behavior for decrement
    int c = 10;
    int d = 10;
    
    int pre_dec = --c;      // c becomes 9, pre_dec is 9
    int post_dec = d--;     // post_dec is 10, d becomes 9
    
    std::cout << "Pre-decrement: c=" << c << ", result=" << pre_dec << std::endl;
    std::cout << "Post-decrement: d=" << d << ", result=" << post_dec << std::endl;
    
    return 0;
}
```

## Comparison Operators

### Equality and Relational Operators

```cpp
#include <iostream>

int main() {
    int a = 10;
    int b = 20;
    int c = 10;
    
    std::cout << std::boolalpha;  // Print true/false instead of 1/0
    
    // Equality operators
    std::cout << "a == b: " << (a == b) << std::endl;  // false
    std::cout << "a == c: " << (a == c) << std::endl;  // true
    std::cout << "a != b: " << (a != b) << std::endl;  // true
    
    // Relational operators
    std::cout << "a < b: " << (a < b) << std::endl;    // true
    std::cout << "a > b: " << (a > b) << std::endl;    // false
    std::cout << "a <= c: " << (a <= c) << std::endl;  // true
    std::cout << "b >= a: " << (b >= a) << std::endl;  // true
    
    return 0;
}
```

### Floating-Point Comparison Pitfalls

```cpp
#include <iostream>
#include <cmath>

int main() {
    double a = 0.1 + 0.2;
    double b = 0.3;
    
    std::cout << std::fixed;
    std::cout << "a = " << a << std::endl;
    std::cout << "b = " << b << std::endl;
    
    // ✘ Dangerous: floating-point comparison
    if (a == b) {
        std::cout << "Equal (direct comparison)" << std::endl;
    } else {
        std::cout << "Not equal (direct comparison)" << std::endl;
    }
    
    // → Safe: compare with tolerance
    double epsilon = 1e-9;
    if (std::abs(a - b) < epsilon) {
        std::cout << "Equal (with tolerance)" << std::endl;
    } else {
        std::cout << "Not equal (with tolerance)" << std::endl;
    }
    
    return 0;
}
```

## Logical Operators

### Boolean Logic

```cpp
#include <iostream>

int main() {
    bool a = true;
    bool b = false;
    
    std::cout << std::boolalpha;
    
    // Logical AND (&&)
    std::cout << "a && b: " << (a && b) << std::endl;  // false
    std::cout << "a && a: " << (a && a) << std::endl;  // true
    
    // Logical OR (||)
    std::cout << "a || b: " << (a || b) << std::endl;  // true
    std::cout << "b || b: " << (b || b) << std::endl;  // false
    
    // Logical NOT (!)
    std::cout << "!a: " << (!a) << std::endl;          // false
    std::cout << "!b: " << (!b) << std::endl;          // true
    
    return 0;
}
```

### Short-Circuit Evaluation

```cpp
#include <iostream>

bool expensive_function() {
    std::cout << "Expensive function called!" << std::endl;
    return true;
}

int main() {
    bool condition = false;
    
    std::cout << "Testing short-circuit AND:" << std::endl;
    if (condition && expensive_function()) {
        // expensive_function() is NOT called because condition is false
        std::cout << "Both conditions true" << std::endl;
    }
    
    std::cout << "Testing short-circuit OR:" << std::endl;
    condition = true;
    if (condition || expensive_function()) {
        // expensive_function() is NOT called because condition is true
        std::cout << "At least one condition true" << std::endl;
    }
    
    return 0;
}
```

## Bitwise Operators

### Basic Bitwise Operations

```cpp
#include <iostream>
#include <bitset>

int main() {
    unsigned int a = 12;    // Binary: 1100
    unsigned int b = 10;    // Binary: 1010
    
    std::cout << "a = " << std::bitset<8>(a) << " (" << a << ")" << std::endl;
    std::cout << "b = " << std::bitset<8>(b) << " (" << b << ")" << std::endl;
    
    // Bitwise AND
    unsigned int and_result = a & b;    // 1000 = 8
    std::cout << "a & b = " << std::bitset<8>(and_result) << " (" << and_result << ")" << std::endl;
    
    // Bitwise OR
    unsigned int or_result = a | b;     // 1110 = 14
    std::cout << "a | b = " << std::bitset<8>(or_result) << " (" << or_result << ")" << std::endl;
    
    // Bitwise XOR
    unsigned int xor_result = a ^ b;    // 0110 = 6
    std::cout << "a ^ b = " << std::bitset<8>(xor_result) << " (" << xor_result << ")" << std::endl;
    
    // Bitwise NOT
    unsigned int not_result = ~a;       // Flips all bits
    std::cout << "~a = " << std::bitset<8>(not_result) << " (" << not_result << ")" << std::endl;
    
    return 0;
}
```

### Bit Shift Operators

```cpp
#include <iostream>
#include <bitset>

int main() {
    unsigned int value = 5;  // Binary: 101
    
    std::cout << "Original: " << std::bitset<8>(value) << " (" << value << ")" << std::endl;
    
    // Left shift (multiply by 2^n)
    unsigned int left_shift = value << 2;  // 10100 = 20
    std::cout << "Left shift by 2: " << std::bitset<8>(left_shift) << " (" << left_shift << ")" << std::endl;
    
    // Right shift (divide by 2^n)
    unsigned int right_shift = value >> 1;  // 10 = 2
    std::cout << "Right shift by 1: " << std::bitset<8>(right_shift) << " (" << right_shift << ")" << std::endl;
    
    return 0;
}
```

## Conditional (Ternary) Operator

### Basic Ternary Operator

```cpp
#include <iostream>

int main() {
    int a = 10;
    int b = 20;
    
    // Syntax: condition ? value_if_true : value_if_false
    int max = (a > b) ? a : b;
    
    std::cout << "Maximum of " << a << " and " << b << " is: " << max << std::endl;
    
    // Equivalent if-else statement
    int max_traditional;
    if (a > b) {
        max_traditional = a;
    } else {
        max_traditional = b;
    }
    
    // Can be used in expressions
    std::cout << "The larger number is " << ((a > b) ? "a" : "b") << std::endl;
    
    return 0;
}
```

### Nested Ternary (Use Sparingly)

```cpp
#include <iostream>

int main() {
    int score = 85;
    
    // ✘ Hard to read when nested
    char grade = (score >= 90) ? 'A' : (score >= 80) ? 'B' : (score >= 70) ? 'C' : 'F';
    
    std::cout << "Score: " << score << ", Grade: " << grade << std::endl;
    
    // → Better: use if-else for complex conditions
    char better_grade;
    if (score >= 90) {
        better_grade = 'A';
    } else if (score >= 80) {
        better_grade = 'B';
    } else if (score >= 70) {
        better_grade = 'C';
    } else {
        better_grade = 'F';
    }
    
    return 0;
}
```

## Operator Precedence and Associativity

### Precedence Examples

```cpp
#include <iostream>

int main() {
    int result;
    
    // Multiplication has higher precedence than addition
    result = 2 + 3 * 4;                    // Result: 14 (not 20)
    std::cout << "2 + 3 * 4 = " << result << std::endl;
    
    // Use parentheses to override precedence
    result = (2 + 3) * 4;                  // Result: 20
    std::cout << "(2 + 3) * 4 = " << result << std::endl;
    
    // More complex expression
    result = 2 * 3 + 4 * 5;                // Result: 26 (6 + 20)
    std::cout << "2 * 3 + 4 * 5 = " << result << std::endl;
    
    return 0;
}
```

### Associativity Examples

```cpp
#include <iostream>

int main() {
    int result;
    
    // Left-to-right associativity for same precedence
    result = 20 - 5 - 3;                   // (20 - 5) - 3 = 12
    std::cout << "20 - 5 - 3 = " << result << std::endl;
    
    // Assignment is right-to-left associative
    int a, b, c;
    a = b = c = 10;                        // c = 10, b = c, a = b
    std::cout << "a = " << a << ", b = " << b << ", c = " << c << std::endl;
    
    return 0;
}
```

## Common Operator Mistakes

### <i class="fa-solid fa-square-xmark"></i> Assignment vs Equality

```cpp
#include <iostream>

int main() {
    int x = 5;
    
    // ✘ Wrong: assignment instead of comparison
    if (x = 10) {  // Assigns 10 to x, then checks if 10 is true (it is)
        std::cout << "This will always execute, x is now: " << x << std::endl;
    }
    
    // → Correct: comparison
    x = 5;  // Reset x
    if (x == 10) {
        std::cout << "This won't execute" << std::endl;
    } else {
        std::cout << "x is not 10, it's: " << x << std::endl;
    }
    
    return 0;
}
```

### <i class="fa-solid fa-square-xmark"></i> Undefined Behavior with Increment

```cpp
#include <iostream>

int main() {
    int x = 5;
    
    // ✘ Undefined behavior: modifying x multiple times between sequence points
    // int result = ++x + x++;  // Don't do this!
    
    // → Clear and defined behavior
    int result = ++x;
    result += x++;
    
    std::cout << "Final result: " << result << std::endl;
    std::cout << "Final x: " << x << std::endl;
    
    return 0;
}
```

## Practical Examples

### Expression Evaluation

```cpp
#include <iostream>

int main() {
    // Complex expression with mixed operators
    int a = 10, b = 20, c = 30;
    bool flag = true;
    
    // Evaluate step by step
    bool result = (a < b) && (b < c) || !flag;
    //              true  &&  true   ||  false
    //                   true        ||  false
    //                            true
    
    std::cout << "Result: " << std::boolalpha << result << std::endl;
    
    return 0;
}
```

### Calculator Example

```cpp
#include <iostream>

int main() {
    double num1, num2;
    char operation;
    
    std::cout << "Enter first number: ";
    std::cin >> num1;
    
    std::cout << "Enter operator (+, -, *, /): ";
    std::cin >> operation;
    
    std::cout << "Enter second number: ";
    std::cin >> num2;
    
    double result;
    bool valid = true;
    
    switch (operation) {
        case '+':
            result = num1 + num2;
            break;
        case '-':
            result = num1 - num2;
            break;
        case '*':
            result = num1 * num2;
            break;
        case '/':
            if (num2 != 0) {
                result = num1 / num2;
            } else {
                std::cout << "Error: Division by zero!" << std::endl;
                valid = false;
            }
            break;
        default:
            std::cout << "Error: Invalid operator!" << std::endl;
            valid = false;
    }
    
    if (valid) {
        std::cout << num1 << " " << operation << " " << num2 << " = " << result << std::endl;
    }
    
    return 0;
}
```

## Exercises

### Exercise 1: Arithmetic Practice

Write a program that performs various arithmetic operations on user input and displays the results.

### Exercise 2: Boolean Logic

Create a program that evaluates complex boolean expressions and explains the step-by-step evaluation.

### Exercise 3: Bitwise Operations

Write a program that demonstrates bitwise operations by manipulating individual bits in integers.

## Key Takeaways

<i class="fa-solid fa-arrow-right"></i> **Understand operator precedence** - use parentheses when in doubt  
<i class="fa-solid fa-arrow-right"></i> **Be careful with floating-point comparisons** - use tolerance  
<i class="fa-solid fa-arrow-right"></i> **Prefer compound assignment operators** (+=, -=, etc.) for clarity  
<i class="fa-solid fa-arrow-right"></i> **Watch out for assignment vs equality** (= vs ==)  
<i class="fa-solid fa-arrow-right"></i> **Use bitwise operators** for low-level programming and optimizations  
<i class="fa-solid fa-arrow-right"></i> **Keep expressions readable** - break complex ones into simpler parts

---

**Next**: Learn how to control program flow // →  [Control Flow](../control-flow/README.md)
