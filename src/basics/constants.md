# Constants and Literals

Constants are values that cannot be modified during program execution. C++ provides several ways to define constants and literal values, making your code more readable, maintainable, and less error-prone.

## Why Use Constants?

Constants provide several benefits:

- <i class="fa-solid fa-lock"></i> **Immutability**: Values that shouldn't change are protected
- <i class="fa-solid fa-book"></i> **Readability**: Named constants make code self-documenting
- <i class="fa-solid fa-screwdriver-wrench"></i> **Maintainability**: Change a value in one place, not throughout code
- <i class="fa-solid fa-bolt"></i> **Performance**: Compiler optimizations for constant values
- <i class="fa-solid fa-magnifying-glass"></i> **Error Prevention**: Compiler catches attempts to modify constants

```cpp
#include <iostream>

int main() {
    const double PI = 3.14159;           // Named constant
    const int DAYS_IN_WEEK = 7;          // Descriptive constant
    
    double radius = 5.0;
    double area = PI * radius * radius;   // Use constant in calculation
    
    std::cout << "Area of circle: " << area << std::endl;
    
    // PI = 3.14;  // ✘ Error: cannot modify const variable
    
    return 0;
}
```

## The `const` Keyword

### Basic Const Variables

```cpp
#include <iostream>

int main() {
    const int MAX_SIZE = 100;            // Integer constant
    const double GRAVITY = 9.81;         // Floating-point constant
    const char GRADE = 'A';              // Character constant
    const bool DEBUG_MODE = true;        // Boolean constant
    
    std::cout << "Max size: " << MAX_SIZE << std::endl;
    std::cout << "Gravity: " << GRAVITY << " m/s²" << std::endl;
    
    // MAX_SIZE = 200;  // ✘ Compilation error
    
    return 0;
}
```

### Const Initialization

```cpp
#include <iostream>

int main() {
    // → Correct: initialized at declaration
    const int VALUE1 = 42;
    
    // → Correct: initialized with expression
    const int VALUE2 = VALUE1 * 2;
    
    // → Correct: initialized with function call
    const int VALUE3 = static_cast<int>(3.14);
    
    // ✘ Error: const must be initialized
    // const int VALUE4;  // Compilation error
    
    std::cout << "Values: " << VALUE1 << ", " << VALUE2 << ", " << VALUE3 << std::endl;
    
    return 0;
}
```

## Compile-Time Constants with `constexpr`

`constexpr` (C++11) indicates that a value can be computed at compile time:

```cpp
#include <iostream>

constexpr int factorial(int n) {
    return (n <= 1) ? 1 : n * factorial(n - 1);
}

int main() {
    constexpr int ARRAY_SIZE = 10;       // Compile-time constant
    constexpr double PI = 3.14159;       // Compile-time constant
    constexpr int FACT_5 = factorial(5); // Computed at compile time
    
    int array[ARRAY_SIZE];               // Size known at compile time
    
    std::cout << "Array size: " << ARRAY_SIZE << std::endl;
    std::cout << "5! = " << FACT_5 << std::endl;
    
    return 0;
}
```

### `const` vs `constexpr`

```cpp
#include <iostream>

int getValue() {
    return 42;
}

int main() {
    const int runtime_const = getValue();     // Runtime constant
    constexpr int compile_const = 100;        // Compile-time constant
    
    // constexpr int error = getValue();      // ✘ Error: not compile-time
    
    // Use in array size (needs compile-time constant)
    int array1[compile_const];               // → OK
    // int array2[runtime_const];            // ✘ Error (in some contexts)
    
    std::cout << "Runtime const: " << runtime_const << std::endl;
    std::cout << "Compile-time const: " << compile_const << std::endl;
    
    return 0;
}
```

## Literal Values

Literals are constant values written directly in code.

### Integer Literals

```cpp
#include <iostream>

int main() {
    // Decimal literals
    int decimal = 42;
    
    // Binary literals (C++14)
    int binary = 0b101010;               // 42 in binary
    
    // Octal literals
    int octal = 052;                     // 42 in octal
    
    // Hexadecimal literals
    int hex = 0x2A;                      // 42 in hexadecimal
    
    // Long literals
    long longValue = 42L;
    long long longlongValue = 42LL;
    
    // Unsigned literals
    unsigned int unsignedValue = 42u;
    
    std::cout << "Decimal: " << decimal << std::endl;
    std::cout << "Binary: " << binary << std::endl;
    std::cout << "Octal: " << octal << std::endl;
    std::cout << "Hex: " << hex << std::endl;
    
    return 0;
}
```

### Floating-Point Literals

```cpp
#include <iostream>

int main() {
    // Basic floating-point literals
    double d1 = 3.14159;
    double d2 = 0.5;
    double d3 = .5;                      // Leading zero optional
    double d4 = 5.;                      // Trailing zero optional
    
    // Scientific notation
    double large = 1.23e6;               // 1.23 × 10^6
    double small = 4.56e-3;              // 4.56 × 10^-3
    
    // Type suffixes
    float f = 3.14f;                     // float literal
    double d = 3.14;                     // double literal (default)
    long double ld = 3.14L;              // long double literal
    
    std::cout << "Pi as float: " << f << std::endl;
    std::cout << "Pi as double: " << d << std::endl;
    std::cout << "Large number: " << large << std::endl;
    
    return 0;
}
```

### Character Literals

```cpp
#include <iostream>

int main() {
    // Basic character literals
    char letter = 'A';
    char digit = '9';
    
    // Escape sequences
    char newline = '\n';
    char tab = '\t';
    char backslash = '\\';
    char quote = '\'';
    char doubleQuote = '\"';
    
    // Unicode literals
    char16_t unicode16 = u'A';           // UTF-16
    char32_t unicode32 = U'A';           // UTF-32
    wchar_t wide = L'A';                 // Wide character
    
    std::cout << "Letter: " << letter << std::endl;
    std::cout << "Tab:" << tab << "After tab" << std::endl;
    
    return 0;
}
```

### String Literals

```cpp
#include <iostream>
#include <string>

int main() {
    // Basic string literals
    const char* cString = "Hello, World!";
    std::string cppString = "Hello, C++!";
    
    // Multi-line strings (C++11 raw strings)
    std::string multiline = R"(
        This is a multi-line string
        with "quotes" and \backslashes
        that don't need escaping!
    )";
    
    // String literal prefixes
    std::string utf8 = u8"UTF-8 string";
    std::u16string utf16 = u"UTF-16 string";
    std::u32string utf32 = U"UTF-32 string";
    std::wstring wide = L"Wide string";
    
    std::cout << cppString << std::endl;
    std::cout << "Raw string:" << multiline << std::endl;
    
    return 0;
}
```

## Enumeration Constants

Enumerations create named constant values:

### Basic Enums (C-style)

```cpp
#include <iostream>

enum Color {
    RED,       // 0
    GREEN,     // 1
    BLUE       // 2
};

enum Status {
    FAILED = -1,
    PENDING = 0,
    SUCCESS = 1
};

int main() {
    Color favoriteColor = BLUE;
    Status taskStatus = SUCCESS;
    
    std::cout << "Favorite color: " << favoriteColor << std::endl;  // Prints: 2
    std::cout << "Task status: " << taskStatus << std::endl;        // Prints: 1
    
    return 0;
}
```

### Scoped Enumerations (C++11)

```cpp
#include <iostream>

enum class Color {
    Red,
    Green,
    Blue
};

enum class Size {
    Small,
    Medium,
    Large
};

int main() {
    Color favoriteColor = Color::Blue;
    Size shirtSize = Size::Medium;
    
    // Color::Red != Size::Small  // Different types, can't compare
    
    // Convert to int if needed
    int colorValue = static_cast<int>(favoriteColor);
    
    std::cout << "Color value: " << colorValue << std::endl;
    
    return 0;
}
```

## Preprocessor Constants

### `#define` Macros (Avoid in Modern C++)

```cpp
#include <iostream>

#define OLD_STYLE_CONSTANT 100          // ✘ Avoid: no type safety
#define OLD_PI 3.14159                  // ✘ Avoid: no scope

int main() {
    const int MODERN_CONSTANT = 100;    // → Prefer: type-safe, scoped
    constexpr double MODERN_PI = 3.14159; // → Prefer: compile-time
    
    std::cout << "Old style: " << OLD_STYLE_CONSTANT << std::endl;
    std::cout << "Modern style: " << MODERN_CONSTANT << std::endl;
    
    return 0;
}
```

## Constant Best Practices

### Naming Conventions

```cpp
#include <iostream>

int main() {
    // → Good: ALL_CAPS for constants
    const int MAX_BUFFER_SIZE = 1024;
    const double EARTH_GRAVITY = 9.81;
    
    // → Alternative: camelCase (less common)
    const int maxBufferSize = 1024;
    
    // → Good: Descriptive names
    const int SECONDS_PER_MINUTE = 60;
    const int MINUTES_PER_HOUR = 60;
    const int HOURS_PER_DAY = 24;
    
    // ✘ Bad: Magic numbers in code
    // int totalSeconds = days * 24 * 60 * 60;  // What do these mean?
    
    // → Good: Use named constants
    int totalSeconds = days * HOURS_PER_DAY * MINUTES_PER_HOUR * SECONDS_PER_MINUTE;
    
    return 0;
}
```

### Grouping Related Constants

```cpp
#include <iostream>

namespace MathConstants {
    constexpr double PI = 3.14159265358979323846;
    constexpr double E = 2.71828182845904523536;
    constexpr double GOLDEN_RATIO = 1.61803398874989484820;
}

namespace PhysicsConstants {
    constexpr double LIGHT_SPEED = 299792458.0;      // m/s
    constexpr double GRAVITY = 9.80665;              // m/s²
    constexpr double PLANCK = 6.62607015e-34;        // J⋅s
}

int main() {
    double circumference = 2 * MathConstants::PI * radius;
    double energy = mass * PhysicsConstants::LIGHT_SPEED * PhysicsConstants::LIGHT_SPEED;
    
    return 0;
}
```

## Constant Expressions in Practice

### Configuration Constants

```cpp
#include <iostream>

namespace Config {
    constexpr int WINDOW_WIDTH = 800;
    constexpr int WINDOW_HEIGHT = 600;
    constexpr const char* WINDOW_TITLE = "My Application";
    constexpr double FRAME_RATE = 60.0;
    constexpr bool DEBUG_MODE = true;
}

int main() {
    if (Config::DEBUG_MODE) {
        std::cout << "Debug mode enabled" << std::endl;
    }
    
    std::cout << "Window: " << Config::WINDOW_WIDTH 
              << "x" << Config::WINDOW_HEIGHT << std::endl;
    
    return 0;
}
```

### Mathematical Constants

```cpp
#include <iostream>
#include <cmath>

namespace Math {
    constexpr double PI = 3.14159265358979323846;
    constexpr double TAU = 2.0 * PI;              // Full circle in radians
    constexpr double DEG_TO_RAD = PI / 180.0;
    constexpr double RAD_TO_DEG = 180.0 / PI;
}

double degreeToRadian(double degrees) {
    return degrees * Math::DEG_TO_RAD;
}

int main() {
    double angle = 45.0;  // degrees
    double radians = degreeToRadian(angle);
    
    std::cout << angle << " degrees = " << radians << " radians" << std::endl;
    
    return 0;
}
```

## Common Mistakes with Constants

### <i class="fa-solid fa-square-xmark"></i> Forgetting to Initialize

```cpp
// const int VALUE;  // ✘ Error: must initialize const variables
```

### <i class="fa-solid fa-square-xmark"></i> Trying to Modify Constants

```cpp
const int SIZE = 100;
// SIZE = 200;  // ✘ Error: cannot modify const variable
```

### <i class="fa-solid fa-square-xmark"></i> Using Magic Numbers

```cpp
// ✘ Bad: What does 86400 mean?
int totalSeconds = days * 86400;

// → Good: Self-documenting
const int SECONDS_PER_DAY = 86400;
int totalSeconds = days * SECONDS_PER_DAY;
```

## Exercises

### Exercise 1: Constants Practice

Create a program that calculates the area and perimeter of various shapes using named mathematical constants.

### Exercise 2: Configuration System

Design a simple configuration system using constants for a game (screen size, player speed, etc.).

### Exercise 3: Unit Conversion

Write a program that converts between different units (temperature, distance, etc.) using conversion constants.

## Key Takeaways

<i class="fa-solid fa-arrow-right"></i> **Use `const`** for values that shouldn't change  
<i class="fa-solid fa-arrow-right"></i> **Use `constexpr`** for compile-time constants  
<i class="fa-solid fa-arrow-right"></i> **Prefer named constants** over magic numbers  
<i class="fa-solid fa-arrow-right"></i> **Use descriptive names** for constants  
<i class="fa-solid fa-arrow-right"></i> **Group related constants** in namespaces  
<i class="fa-solid fa-arrow-right"></i> **Avoid `#define`** in favor of `const`/`constexpr`

---

**Next**: Learn how to perform operations on data // →  [Operators and Expressions](operators.md)
