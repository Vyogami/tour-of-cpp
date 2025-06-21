# Fundamental Types

C++ provides several built-in data types to represent different kinds of information. Understanding these types is crucial for writing efficient and correct programs.

## Overview of Fundamental Types

C++ fundamental types can be categorized into:

- **Integral Types**: Whole numbers (`int`, `char`, `bool`)
- **Floating-Point Types**: Decimal numbers (`float`, `double`)
- **Void Type**: Represents "no value" (`void`)

```cpp
#include <iostream>
#include <climits>  // For type limits

int main() {
    // Integral types
    int age = 25;
    char grade = 'A';
    bool isStudent = true;
    
    // Floating-point types
    float pi = 3.14f;
    double precision = 3.141592653589793;
    
    std::cout << "Age: " << age << std::endl;
    std::cout << "Grade: " << grade << std::endl;
    std::cout << "Student: " << isStudent << std::endl;
    std::cout << "Pi (float): " << pi << std::endl;
    std::cout << "Pi (double): " << precision << std::endl;
    
    return 0;
}
```

## Integer Types

### Basic Integer Types

```cpp
#include <iostream>
#include <climits>

int main() {
    // Standard integer types
    short smallNum = 32767;           // At least 16 bits
    int regularNum = 2147483647;      // At least 16 bits (usually 32)
    long bigNum = 2147483647L;        // At least 32 bits
    long long hugeNum = 9223372036854775807LL;  // At least 64 bits
    
    // Display sizes and ranges
    std::cout << "short: " << sizeof(short) << " bytes, "
              << "range: " << SHRT_MIN << " to " << SHRT_MAX << std::endl;
    
    std::cout << "int: " << sizeof(int) << " bytes, "
              << "range: " << INT_MIN << " to " << INT_MAX << std::endl;
    
    std::cout << "long: " << sizeof(long) << " bytes, "
              << "range: " << LONG_MIN << " to " << LONG_MAX << std::endl;
    
    return 0;
}
```

### Signed vs Unsigned

```cpp
#include <iostream>

int main() {
    // Signed integers (can be positive or negative)
    int signedInt = -42;
    
    // Unsigned integers (only positive values)
    unsigned int unsignedInt = 42u;
    
    std::cout << "Signed: " << signedInt << std::endl;
    std::cout << "Unsigned: " << unsignedInt << std::endl;
    
    // Unsigned types can store larger positive values
    unsigned int maxUnsigned = 4294967295u;  // 2^32 - 1 on 32-bit systems
    
    // Be careful with unsigned arithmetic!
    unsigned int danger = 0;
    // danger = danger - 1;  // Wraps around to large positive number!
    
    return 0;
}
```

<i class="fa-solid fa-triangle-exclamation"></i> **Warning**: Mixing signed and unsigned types can lead to unexpected behavior. Prefer signed types unless you specifically need the extended positive range.

### Fixed-Width Integer Types (C++11)

```cpp
#include <iostream>
#include <cstdint>

int main() {
    // Exact width types (guaranteed size)
    std::int8_t  tiny = 127;        // Exactly 8 bits
    std::int16_t small = 32767;     // Exactly 16 bits  
    std::int32_t medium = 2147483647;   // Exactly 32 bits
    std::int64_t large = 9223372036854775807;  // Exactly 64 bits
    
    // Unsigned versions
    std::uint8_t  utiny = 255;      // 0 to 255
    std::uint16_t usmall = 65535;   // 0 to 65,535
    std::uint32_t umedium = 4294967295;  // 0 to 4,294,967,295
    
    std::cout << "int8_t: " << static_cast<int>(tiny) << std::endl;
    std::cout << "int32_t: " << medium << std::endl;
    std::cout << "uint16_t: " << usmall << std::endl;
    
    return 0;
}
```

<i class="fa-solid fa-lightbulb"></i> **Best Practice**: Use fixed-width types when exact size matters (file formats, network protocols, etc.).

## Character Types

### Basic Character Type

```cpp
#include <iostream>

int main() {
    char letter = 'A';              // Single character
    char digit = '7';               // Character, not number 7
    char newline = '\n';            // Escape sequence
    char tab = '\t';                // Tab character
    
    std::cout << "Letter: " << letter << std::endl;
    std::cout << "Digit: " << digit << std::endl;
    std::cout << "ASCII value of A: " << static_cast<int>(letter) << std::endl;
    
    // Characters are small integers
    char nextLetter = letter + 1;   // 'B'
    std::cout << "Next letter: " << nextLetter << std::endl;
    
    return 0;
}
```

### Wide Character Types

```cpp
#include <iostream>

int main() {
    wchar_t wide = L'A';            // Wide character
    char16_t utf16 = u'A';          // UTF-16 character (C++11)
    char32_t utf32 = U'A';          // UTF-32 character (C++11)
    char8_t utf8 = u8'A';           // UTF-8 character (C++20)
    
    std::cout << "Wide char size: " << sizeof(wchar_t) << " bytes" << std::endl;
    std::cout << "UTF-16 size: " << sizeof(char16_t) << " bytes" << std::endl;
    std::cout << "UTF-32 size: " << sizeof(char32_t) << " bytes" << std::endl;
    
    return 0;
}
```

## Boolean Type

```cpp
#include <iostream>

int main() {
    bool isTrue = true;
    bool isFalse = false;
    
    // Boolean expressions
    bool result1 = (5 > 3);         // true
    bool result2 = (10 == 5);       // false
    bool result3 = !isFalse;        // true (logical NOT)
    
    std::cout << "isTrue: " << isTrue << std::endl;       // Prints: 1
    std::cout << "isFalse: " << isFalse << std::endl;     // Prints: 0
    
    // Use std::boolalpha for true/false output
    std::cout << std::boolalpha;
    std::cout << "result1: " << result1 << std::endl;     // Prints: true
    std::cout << "result2: " << result2 << std::endl;     // Prints: false
    
    return 0;
}
```

## Floating-Point Types

### Float vs Double vs Long Double

```cpp
#include <iostream>
#include <iomanip>
#include <cfloat>

int main() {
    float singlePrecision = 3.14159f;          // ~7 decimal digits
    double doublePrecision = 3.141592653589793; // ~15 decimal digits  
    long double extendedPrecision = 3.141592653589793238L; // Platform dependent
    
    // Display precision and sizes
    std::cout << std::fixed << std::setprecision(15);
    
    std::cout << "float: " << singlePrecision 
              << " (" << sizeof(float) << " bytes)" << std::endl;
    
    std::cout << "double: " << doublePrecision 
              << " (" << sizeof(double) << " bytes)" << std::endl;
    
    std::cout << "long double: " << extendedPrecision 
              << " (" << sizeof(long double) << " bytes)" << std::endl;
    
    // Precision limits
    std::cout << "\nPrecision:" << std::endl;
    std::cout << "FLT_DIG: " << FLT_DIG << " decimal digits" << std::endl;
    std::cout << "DBL_DIG: " << DBL_DIG << " decimal digits" << std::endl;
    
    return 0;
}
```

### Scientific Notation

```cpp
#include <iostream>

int main() {
    double large = 1.23e6;          // 1.23 × 10^6 = 1,230,000
    double small = 4.56e-3;         // 4.56 × 10^-3 = 0.00456
    
    std::cout << "Large number: " << large << std::endl;
    std::cout << "Small number: " << small << std::endl;
    
    // Scientific notation output
    std::cout << std::scientific;
    std::cout << "Large (scientific): " << large << std::endl;
    std::cout << "Small (scientific): " << small << std::endl;
    
    return 0;
}
```

## Type Properties and Limits

### Using `sizeof` Operator

```cpp
#include <iostream>

int main() {
    std::cout << "Type sizes on this system:" << std::endl;
    std::cout << "char: " << sizeof(char) << " byte(s)" << std::endl;
    std::cout << "short: " << sizeof(short) << " byte(s)" << std::endl;
    std::cout << "int: " << sizeof(int) << " byte(s)" << std::endl;
    std::cout << "long: " << sizeof(long) << " byte(s)" << std::endl;
    std::cout << "long long: " << sizeof(long long) << " byte(s)" << std::endl;
    std::cout << "float: " << sizeof(float) << " byte(s)" << std::endl;
    std::cout << "double: " << sizeof(double) << " byte(s)" << std::endl;
    std::cout << "bool: " << sizeof(bool) << " byte(s)" << std::endl;
    
    return 0;
}
```

### Type Limits

```cpp
#include <iostream>
#include <limits>

int main() {
    std::cout << "Type limits:" << std::endl;
    
    std::cout << "int min: " << std::numeric_limits<int>::min() << std::endl;
    std::cout << "int max: " << std::numeric_limits<int>::max() << std::endl;
    
    std::cout << "float min: " << std::numeric_limits<float>::min() << std::endl;
    std::cout << "float max: " << std::numeric_limits<float>::max() << std::endl;
    
    std::cout << "double digits: " << std::numeric_limits<double>::digits10 << std::endl;
    
    return 0;
}
```

## Type Conversions

### Implicit Conversions

```cpp
#include <iostream>

int main() {
    int intValue = 42;
    double doubleValue = intValue;    // int // →  double (safe)
    
    double pi = 3.14159;
    int truncated = pi;               // double // →  int (loses decimal part)
    
    std::cout << "Original int: " << intValue << std::endl;
    std::cout << "As double: " << doubleValue << std::endl;
    std::cout << "Original double: " << pi << std::endl;
    std::cout << "Truncated int: " << truncated << std::endl;
    
    return 0;
}
```

### Explicit Conversions (Casts)

```cpp
#include <iostream>

int main() {
    double value = 3.8;
    
    // C-style cast (avoid in modern C++)
    int cStyleCast = (int)value;
    
    // C++ style casts (preferred)
    int staticCast = static_cast<int>(value);
    
    std::cout << "Original: " << value << std::endl;
    std::cout << "C-style cast: " << cStyleCast << std::endl;
    std::cout << "static_cast: " << staticCast << std::endl;
    
    return 0;
}
```

## Common Type-Related Mistakes

### <i class="fa-solid fa-square-xmark"></i> Integer Division

```cpp
int result = 5 / 2;              // Result is 2, not 2.5!
```

**<i class="fa-solid fa-arrow-right"></i> Fix:**

```cpp
double result = 5.0 / 2.0;       // Result is 2.5
// or
double result = static_cast<double>(5) / 2;
```

### <i class="fa-solid fa-square-xmark"></i> Floating-Point Comparison

```cpp
double a = 0.1 + 0.2;
if (a == 0.3) {                  // May be false due to precision!
    std::cout << "Equal" << std::endl;
}
```

**<i class="fa-solid fa-arrow-right"></i> Fix:**

```cpp
double a = 0.1 + 0.2;
double epsilon = 1e-9;
if (std::abs(a - 0.3) < epsilon) {
    std::cout << "Equal (within tolerance)" << std::endl;
}
```

### <i class="fa-solid fa-square-xmark"></i> Unsigned Underflow

```cpp
unsigned int value = 0;
value = value - 1;               // Wraps to maximum value!
std::cout << value;              // Prints large number
```

## Choosing the Right Type

### Guidelines

- **`int`**: Default choice for integer values
- **`double`**: Default choice for floating-point values
- **`char`**: For single characters only
- **`bool`**: For true/false values
- **`std::size_t`**: For sizes and indices
- **Fixed-width types**: When exact size is crucial

### Examples

```cpp
int age = 25;                    // Person's age
double price = 19.99;            // Product price
char grade = 'A';                // Letter grade
bool isValid = true;             // Flag/status
std::size_t count = items.size(); // Container size
std::uint32_t fileSize = 1024;   // File size in bytes
```

## Exercises

### Exercise 1: Type Explorer

Write a program that displays the size and range of all fundamental types on your system.

### Exercise 2: Type Conversions

Create variables of different types and experiment with conversions between them.

### Exercise 3: Precision Test

Compare the precision of `float` vs `double` by performing calculations with very large or very small numbers.

## Key Takeaways

<i class="fa-solid fa-arrow-right"></i> **Choose appropriate types** for your data  
<i class="fa-solid fa-arrow-right"></i> **Be aware of type limits** and potential overflow  
<i class="fa-solid fa-arrow-right"></i> **Use explicit casts** when type conversion is needed  
<i class="fa-solid fa-arrow-right"></i> **Prefer signed types** over unsigned unless necessary  
<i class="fa-solid fa-arrow-right"></i> **Use `double`** as default for floating-point values

---

**Next**: Learn about constants and literal values // →  [Constants and Literals](constants.md)
