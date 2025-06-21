# Basic Concepts

Now that you can write and run C++ programs, let's explore the fundamental building blocks of the language. This section covers the essential concepts you'll use in every C++ program.

## What You'll Learn

- **Variables and Declarations** - How to store and name data
- **Fundamental Types** - Built-in data types and their characteristics  
- **Constants and Literals** - Immutable values and literal syntax
- **Operators and Expressions** - Performing operations on data

## Core Programming Concepts

Every programming language has these essential elements:

<i class="fa-solid fa-folder"></i> **Data Storage**: Variables to hold information  
<i class="fa-solid fa-tag"></i> **Data Types**: Different kinds of data (numbers, text, etc.)  
<i class="fa-solid fa-lock"></i> **Constants**: Values that don't change  
<i class="fa-solid fa-bolt"></i> **Operations**: Ways to manipulate and compute with data

## The C++ Type System

C++ is a **statically typed** language, which means:

- <i class="fa-solid fa-arrow-right"></i> **Type Safety**: Variables have specific types known at compile time
- <i class="fa-solid fa-arrow-right"></i> **Performance**: No runtime type checking overhead
- <i class="fa-solid fa-arrow-right"></i> **Clear Intent**: Code explicitly states what kind of data it uses
- <i class="fa-solid fa-arrow-right"></i> **Early Error Detection**: Type mismatches caught during compilation

### Type Categories

```cpp
// Fundamental types (built into the language)
int age = 25;
double price = 19.99;
char grade = 'A';
bool isStudent = true;

// Compound types (built from fundamental types)
int numbers[5];        // Array
int* pointer;          // Pointer  
int& reference = age;  // Reference
```

## Memory and Performance

Understanding how C++ manages data in memory:

```cpp
// Stack allocation (automatic storage)
int stackVar = 42;          // Cleaned up automatically
char buffer[1024];          // Fixed size, fast access

// Heap allocation (dynamic storage)  
int* heapVar = new int(42); // Manual memory management
delete heapVar;             // Must explicitly free
```

<i class="fa-solid fa-lightbulb"></i> **Key Insight**: C++ gives you control over where and how data is stored, enabling high-performance applications.

## Best Practices Preview

As we explore these concepts, we'll emphasize:

- **Meaningful Names**: `studentCount` instead of `n`
- **Initialization**: Always initialize variables
- **Type Safety**: Use appropriate types for your data
- **Const Correctness**: Mark unchanging data as `const`

Let's start with the foundation: variables and how to declare them!

---

**Sections in This Chapter:**

- [Variables and Declarations // → ](variables.md)
- [Fundamental Types // → ](types.md)  
- [Constants and Literals // → ](constants.md)
- [Operators and Expressions // → ](operators.md)
