# Hello, World

Every programming journey begins with "Hello, World!" - let's write your first C++ program and understand what's happening under the hood.

## Your First Program

Create a file called `hello.cpp`:

```cpp
#include <iostream>

int main() {
    std::cout << "Hello, World!" << std::endl;
    return 0;
}
```

## Compile and Run

```bash
# Compile the program
g++ hello.cpp -o hello

# Run it
./hello        # Linux/macOS
hello.exe      # Windows
```

**Output:**

```
Hello, World!
```

🎉 **Congratulations!** You've written and executed your first C++ program!

## Understanding the Code

Let's break down each part of this simple program:

### 1. Include Directive

```cpp
#include <iostream>
```

- **Purpose**: Includes the input/output stream library
- **What it does**: Gives us access to `std::cout` and `std::endl`
- **Preprocessor**: This line is processed before compilation

### 2. Main Function

```cpp
int main() {
    // Program code goes here
    return 0;
}
```

- **Entry Point**: Every C++ program starts executing from `main()`
- **Return Type**: `int` indicates the function returns an integer
- **Return Value**: `0` typically means "success" to the operating system

### 3. Output Statement

```cpp
std::cout << "Hello, World!" << std::endl;
```

- **`std::cout`**: Character output stream (console output)
- **`<<`**: Stream insertion operator (sends data to output)
- **`"Hello, World!"`**: String literal to be printed
- **`std::endl`**: End line (adds newline and flushes buffer)

### 4. Namespace

```cpp
std::cout  // std is the namespace, cout is the function
```

- **`std`**: Standard namespace containing C++ standard library
- **`::`**: Scope resolution operator

## Variations and Improvements

### Using Namespace Declaration

```cpp
#include <iostream>
using namespace std;  // Now we can omit 'std::'

int main() {
    cout << "Hello, World!" << endl;
    return 0;
}
```

<i class="fa-solid fa-triangle-exclamation"></i> **Warning**: While `using namespace std;` is common in tutorials, avoid it in larger programs to prevent naming conflicts.

### Modern C++ Style

```cpp
#include <iostream>
#include <string>

int main() {
    std::string message = "Hello, World!";
    std::cout << message << '\n';  // '\n' instead of std::endl
    return 0;
}
```

<i class="fa-solid fa-lightbulb"></i> **Tip**: Use `'\n'` instead of `std::endl` for better performance (no buffer flush).

## Common Variations

### Multiple Output Lines

```cpp
#include <iostream>

int main() {
    std::cout << "Hello, World!" << std::endl;
    std::cout << "Welcome to C++!" << std::endl;
    std::cout << "Let's start programming!" << std::endl;
    return 0;
}
```

### Interactive Hello World

```cpp
#include <iostream>
#include <string>

int main() {
    std::string name;
    
    std::cout << "What's your name? ";
    std::getline(std::cin, name);  // Read full line including spaces
    
    std::cout << "Hello, " << name << "!" << std::endl;
    return 0;
}
```

### Formatted Output

```cpp
#include <iostream>
#include <iomanip>

int main() {
    std::cout << std::setw(20) << std::setfill('=') << "" << std::endl;
    std::cout << "   Hello, World!   " << std::endl;
    std::cout << std::setw(20) << std::setfill('=') << "" << std::endl;
    return 0;
}
```

## Compilation Deep Dive

Understanding what happens when you compile:

### 1. Preprocessing

```bash
g++ -E hello.cpp  # See preprocessed output
```

- Includes header files
- Processes `#include`, `#define`, etc.
- Removes comments

### 2. Compilation

```bash
g++ -S hello.cpp  # Generate assembly code
```

- Translates C++ to assembly language
- Creates `hello.s` file

### 3. Assembly

```bash
g++ -c hello.cpp  # Create object file
```

- Converts assembly to machine code
- Creates `hello.o` (or `hello.obj` on Windows)

### 4. Linking

```bash
g++ hello.o -o hello  # Create executable
```

- Combines object files
- Links with standard libraries
- Creates final executable

## Common Beginner Mistakes

### <i class="fa-solid fa-square-xmark"></i> Missing Semicolon

```cpp
std::cout << "Hello, World!" << std::endl  // Missing semicolon
return 0;
```

**Error**: `expected ';' before 'return'`

### <i class="fa-solid fa-square-xmark"></i> Wrong Main Signature

```cpp
void main() {  // Should be 'int main()'
    std::cout << "Hello, World!" << std::endl;
}
```

### <i class="fa-solid fa-square-xmark"></i> Missing Include

```cpp
// #include <iostream>  // Forgot to include!
int main() {
    std::cout << "Hello, World!" << std::endl;
    return 0;
}
```

**Error**: `'cout' was not declared in this scope`

### <i class="fa-solid fa-square-xmark"></i> Case Sensitivity

```cpp
#include <iostream>
int main() {
    std::Cout << "Hello, World!" << std::endl;  // Wrong: should be 'cout'
    return 0;
}
```

## Exercises

Try these variations to solidify your understanding:

### Exercise 1: Personal Greeting

Create a program that asks for your name and age, then prints a personalized greeting.

### Exercise 2: ASCII Art

Use `std::cout` to print a simple ASCII art picture.

### Exercise 3: Calculator Welcome

Write a program that prints a welcome message for a calculator app, including usage instructions.

## Next Steps

Now that you can compile and run C++ programs, let's explore the building blocks of the language:

<i class="fa-solid fa-paperclip"></i> **Continue Learning**:

- [Variables and Declarations // → ](../basics/variables.md)
- [Fundamental Types // → ](../basics/types.md)

You're ready to dive into C++ fundamentals! The next section will teach you about variables, types, and how to store and manipulate data.
