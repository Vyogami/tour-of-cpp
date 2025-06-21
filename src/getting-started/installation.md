# Installation and Setup

Before writing C++ code, you need a compiler and development environment. This guide covers the most popular options across different operating systems.

## Choose Your Compiler

### <i class="fa-brands fa-microsoft"></i> Windows

**Option 1: Microsoft Visual Studio (Recommended)**

```bash
# Download Visual Studio Community (free)
# https://visualstudio.microsoft.com/vs/community/
# Includes MSVC compiler, debugger, and IntelliSense
# Full IDE with project management
```

**Option 2: MinGW-w64 with Code::Blocks**

```bash
# Download from: http://www.codeblocks.org/
# Lightweight and beginner-friendly
# Cross-platform IDE
```

**Option 3: MSYS2 with GCC**

```bash
# Install MSYS2: https://www.msys2.org/
pacman -S mingw-w64-x86_64-gcc
pacman -S mingw-w64-x86_64-gdb
```

### <i class="fa-brands fa-linux"></i> Linux (Ubuntu/Debian)

```bash
# Install GCC compiler and development tools
sudo apt update
sudo apt install build-essential gdb

# Verify installation
gcc --version
g++ --version

# Optional: Install additional tools
sudo apt install cmake make git
```

### <i class="fa-brands fa-apple"></i> macOS

**Option 1: Xcode Command Line Tools**

```bash
# Install Xcode tools (includes Clang)
xcode-select --install

# Verify installation
clang++ --version
```

**Option 2: Homebrew with GCC**

```bash
# Install Homebrew: https://brew.sh/
brew install gcc

# Use GCC specifically
g++-11 --version  # Version number may vary
```

## Choose Your Editor/IDE

### Full IDEs

- **Visual Studio** (Windows) - Complete development environment
- **CLion** (Cross-platform) - JetBrains IDE with excellent debugging
- **Code::Blocks** (Cross-platform) - Free and lightweight
- **Dev-C++** (Windows) - Simple IDE for beginners

### Text Editors with C++ Support

- **VS Code** with C++ extensions - Lightweight and powerful
- **Sublime Text** with C++ packages
- **Vim/Neovim** with C++ plugins
- **Emacs** with C++ mode

## Setting Up VS Code (Recommended for Beginners)

If you choose VS Code, install these essential extensions:

```json
// Essential C++ extensions
{
  "recommendations": [
    "ms-vscode.cpptools",           // IntelliSense, debugging
    "ms-vscode.cmake-tools",        // CMake support
    "ms-vscode.cpptools-extension-pack"  // Complete C++ bundle
  ]
}
```

### VS Code Configuration

Create `.vscode/c_cpp_properties.json`:

```json
{
    "configurations": [
        {
            "name": "Linux/macOS",
            "includePath": [
                "${workspaceFolder}/**"
            ],
            "defines": [],
            "compilerPath": "/usr/bin/g++",
            "cStandard": "c17",
            "cppStandard": "c++20",
            "intelliSenseMode": "linux-gcc-x64"
        }
    ],
    "version": 4
}
```

## Verify Your Setup

Create a test file `test.cpp`:

```cpp
#include <iostream>

int main() {
    std::cout << "C++ setup successful!" << std::endl;
    return 0;
}
```

### Compile and Run

**Command Line:**

```bash
# Compile
g++ -o test test.cpp

# Run (Linux/macOS)
./test

# Run (Windows)
test.exe
```

**Expected Output:**

```
C++ setup successful!
```

## Common Compiler Flags

Learn these essential compiler options:

```bash
# Basic compilation
g++ program.cpp -o program

# Enable all warnings
g++ -Wall -Wextra program.cpp -o program

# Debug information
g++ -g program.cpp -o program

# Optimization
g++ -O2 program.cpp -o program

# Specify C++ standard
g++ -std=c++20 program.cpp -o program

# Combine common flags
g++ -std=c++20 -Wall -Wextra -g program.cpp -o program
```

## Troubleshooting

### <i class="fa-solid fa-square-xmark"></i> "g++ command not found"

- **Linux/macOS**: Install build tools as shown above
- **Windows**: Add compiler to PATH or use full path

### <i class="fa-solid fa-square-xmark"></i> "No such file or directory"

- Check file names and paths
- Ensure you're in the correct directory
- Verify file extensions (`.cpp`, not `.c`)

### <i class="fa-solid fa-square-xmark"></i> Permission denied

- **Linux/macOS**: Use `chmod +x program` to make executable
- **Windows**: Check antivirus settings

<i class="fa-solid fa-lightbulb"></i> **Tip**: Start with a simple IDE like Code::Blocks or VS Code if you're new to C++. You can always switch to more advanced tools later.

---

**Ready to code?** Let's write your first C++ program! // →  [Hello, World!](hello-world.md)
