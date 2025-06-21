
# Object-Oriented Programming

Object-Oriented Programming (OOP) is a programming paradigm that organizes code around objects rather than functions. It provides a powerful way to model real-world entities and relationships in your programs, making code more modular, reusable, and easier to maintain.

## What You'll Learn

- **Classes and Objects** - Creating custom data types and instances
- **Constructors and Destructors** - Object initialization and cleanup
- **Inheritance** - Building relationships between classes
- **Polymorphism** - Objects behaving differently based on their type
- **Encapsulation** - Controlling access to data and methods

## Why Object-Oriented Programming Matters

Without OOP, complex programs become difficult to manage:

```cpp
// ✘ Without OOP - procedural approach with global variables
#include <iostream>
#include <string>

// Global variables for a bank account
std::string accountNumber = "12345";
std::string ownerName = "John Doe";
double balance = 1000.0;

void deposit(double amount) {
    balance += amount;
    std::cout << "Deposited $" << amount << std::endl;
}

void withdraw(double amount) {
    if (amount <= balance) {
        balance -= amount;
        std::cout << "Withdrew $" << amount << std::endl;
    } else {
        std::cout << "Insufficient funds!" << std::endl;
    }
}

void displayAccount() {
    std::cout << "Account: " << accountNumber 
              << " Owner: " << ownerName 
              << " Balance: $" << balance << std::endl;
}

int main() {
    displayAccount();
    deposit(500);
    withdraw(200);
    displayAccount();
    return 0;
}
```

```cpp
// → With OOP - encapsulated, reusable, and organized
#include <iostream>
#include <string>

class BankAccount {
private:
    std::string accountNumber;
    std::string ownerName;
    double balance;

public:
    BankAccount(const std::string& accNum, const std::string& owner, double initialBalance) 
        : accountNumber(accNum), ownerName(owner), balance(initialBalance) {}
    
    void deposit(double amount) {
        balance += amount;
        std::cout << "Deposited $" << amount << std::endl;
    }
    
    void withdraw(double amount) {
        if (amount <= balance) {
            balance -= amount;
            std::cout << "Withdrew $" << amount << std::endl;
        } else {
            std::cout << "Insufficient funds!" << std::endl;
        }
    }
    
    void display() const {
        std::cout << "Account: " << accountNumber 
                  << " Owner: " << ownerName 
                  << " Balance: $" << balance << std::endl;
    }
};

int main() {
    BankAccount account1("12345", "John Doe", 1000.0);
    BankAccount account2("67890", "Jane Smith", 2500.0);
    
    account1.display();
    account1.deposit(500);
    account1.withdraw(200);
    account1.display();
    
    account2.display();
    
    return 0;
}
```

## Core OOP Concepts

### <i class="fa-solid fa-bolt"></i> **Classes and Objects**

Classes are blueprints; objects are instances:

```cpp
#include <iostream>
#include <string>

class Car {
public:
    std::string brand;
    std::string model;
    int year;
    
    void start() {
        std::cout << "The " << year << " " << brand << " " << model << " is starting!" << std::endl;
    }
    
    void honk() {
        std::cout << "Beep beep!" << std::endl;
    }
};

int main() {
    // Creating objects (instances) of the Car class
    Car myCar;
    myCar.brand = "Toyota";
    myCar.model = "Camry";
    myCar.year = 2023;
    
    Car friendsCar;
    friendsCar.brand = "Honda";
    friendsCar.model = "Civic";
    friendsCar.year = 2022;
    
    myCar.start();
    friendsCar.honk();
    
    return 0;
}
```

### <i class="fa-solid fa-lock"></i> **Encapsulation**

Hiding internal details and controlling access:

```cpp
#include <iostream>

class Rectangle {
private:
    double width;
    double height;
    
public:
    // Constructor
    Rectangle(double w, double h) : width(w), height(h) {}
    
    // Getter methods
    double getWidth() const { return width; }
    double getHeight() const { return height; }
    
    // Setter methods with validation
    void setWidth(double w) {
        if (w > 0) width = w;
    }
    
    void setHeight(double h) {
        if (h > 0) height = h;
    }
    
    double getArea() const {
        return width * height;
    }
    
    double getPerimeter() const {
        return 2 * (width + height);
    }
};

int main() {
    Rectangle rect(5.0, 3.0);
    
    std::cout << "Area: " << rect.getArea() << std::endl;
    std::cout << "Perimeter: " << rect.getPerimeter() << std::endl;
    
    rect.setWidth(7.0);
    std::cout << "New area: " << rect.getArea() << std::endl;
    
    // rect.width = -5;  // ✘ Error: can't access private member directly
    
    return 0;
}
```

### <i class="fa-solid fa-code-branch"></i> **Inheritance**

Creating new classes based on existing ones:

```cpp
#include <iostream>
#include <string>

// Base class
class Animal {
protected:
    std::string name;
    int age;
    
public:
    Animal(const std::string& n, int a) : name(n), age(a) {}
    
    void sleep() {
        std::cout << name << " is sleeping." << std::endl;
    }
    
    void eat() {
        std::cout << name << " is eating." << std::endl;
    }
    
    virtual void makeSound() {  // Virtual for polymorphism
        std::cout << name << " makes a sound." << std::endl;
    }
};

// Derived class
class Dog : public Animal {
public:
    Dog(const std::string& n, int a) : Animal(n, a) {}
    
    void makeSound() override {
        std::cout << name << " barks: Woof!" << std::endl;
    }
    
    void wagTail() {
        std::cout << name << " is wagging tail." << std::endl;
    }
};

// Another derived class
class Cat : public Animal {
public:
    Cat(const std::string& n, int a) : Animal(n, a) {}
    
    void makeSound() override {
        std::cout << name << " meows: Meow!" << std::endl;
    }
    
    void purr() {
        std::cout << name << " is purring." << std::endl;
    }
};

int main() {
    Dog buddy("Buddy", 3);
    Cat whiskers("Whiskers", 2);
    
    buddy.eat();
    buddy.makeSound();
    buddy.wagTail();
    
    whiskers.sleep();
    whiskers.makeSound();
    whiskers.purr();
    
    return 0;
}
```

### <i class="fa-solid fa-border-all"></i> **Polymorphism**

Same interface, different behaviors:

```cpp
#include <iostream>
#include <vector>
#include <memory>

class Shape {
public:
    virtual double getArea() const = 0;  // Pure virtual function
    virtual void draw() const = 0;
    virtual ~Shape() = default;  // Virtual destructor
};

class Circle : public Shape {
private:
    double radius;
    
public:
    Circle(double r) : radius(r) {}
    
    double getArea() const override {
        return 3.14159 * radius * radius;
    }
    
    void draw() const override {
        std::cout << "Drawing a circle with radius " << radius << std::endl;
    }
};

class Square : public Shape {
private:
    double side;
    
public:
    Square(double s) : side(s) {}
    
    double getArea() const override {
        return side * side;
    }
    
    void draw() const override {
        std::cout << "Drawing a square with side " << side << std::endl;
    }
};

int main() {
    std::vector<std::unique_ptr<Shape>> shapes;
    shapes.push_back(std::make_unique<Circle>(5.0));
    shapes.push_back(std::make_unique<Square>(4.0));
    shapes.push_back(std::make_unique<Circle>(3.0));
    
    double totalArea = 0;
    for (const auto& shape : shapes) {
        shape->draw();  // Polymorphic call
        totalArea += shape->getArea();  // Polymorphic call
    }
    
    std::cout << "Total area: " << totalArea << std::endl;
    
    return 0;
}
```

## Benefits of Object-Oriented Programming

### <i class="fa-solid fa-recycle"></i> **Code Reusability**

Create once, use many times:

```cpp
class DatabaseConnection {
private:
    std::string connectionString;
    bool connected;
    
public:
    DatabaseConnection(const std::string& connStr) 
        : connectionString(connStr), connected(false) {}
    
    void connect() { /* connection logic */ }
    void disconnect() { /* disconnection logic */ }
    void executeQuery(const std::string& query) { /* query logic */ }
};

// Reuse the same class for different databases
DatabaseConnection userDB("user_database");
DatabaseConnection productDB("product_database");
DatabaseConnection orderDB("order_database");
```

### <i class="fa-solid fa-box"></i> **Modularity**

Separate concerns into distinct classes:

```cpp
// Each class has a specific responsibility
class User {
    // Handle user data and validation
};

class EmailService {
    // Handle email sending
};

class Logger {
    // Handle logging
};

class PaymentProcessor {
    // Handle payment processing
};
```

### <i class="fa-solid fa-shield-halved"></i> **Data Protection**

Control access to sensitive data:

```cpp
class SecureAccount {
private:
    std::string password;  // Hidden from outside access
    double balance;        // Protected from direct manipulation
    
public:
    bool authenticate(const std::string& inputPassword) {
        return password == inputPassword;
    }
    
    void withdraw(double amount, const std::string& inputPassword) {
        if (authenticate(inputPassword) && amount <= balance) {
            balance -= amount;
        }
    }
};
```

### <i class="fa-solid fa-vial"></i> **Easier Testing**

Test individual components in isolation:

```cpp
class Calculator {
public:
    virtual double add(double a, double b) { return a + b; }
    virtual double multiply(double a, double b) { return a * b; }
};

// Easy to create mock objects for testing
class MockCalculator : public Calculator {
public:
    double add(double a, double b) override {
        // Test-specific implementation
        return 999;  // Predictable result for testing
    }
};
```

## Real-World Modeling

OOP allows you to model complex systems naturally:

```cpp
class Employee {
protected:
    std::string name;
    double salary;
    
public:
    Employee(const std::string& n, double s) : name(n), salary(s) {}
    virtual double calculatePay() const = 0;
    virtual void displayInfo() const = 0;
};

class FullTimeEmployee : public Employee {
public:
    FullTimeEmployee(const std::string& n, double s) : Employee(n, s) {}
    
    double calculatePay() const override {
        return salary / 12;  // Monthly salary
    }
    
    void displayInfo() const override {
        std::cout << "Full-time employee: " << name 
                  << ", Monthly pay: $" << calculatePay() << std::endl;
    }
};

class ContractEmployee : public Employee {
private:
    int hoursWorked;
    
public:
    ContractEmployee(const std::string& n, double hourlyRate, int hours) 
        : Employee(n, hourlyRate), hoursWorked(hours) {}
    
    double calculatePay() const override {
        return salary * hoursWorked;  // Hourly rate * hours
    }
    
    void displayInfo() const override {
        std::cout << "Contract employee: " << name 
                  << ", Total pay: $" << calculatePay() << std::endl;
    }
};
```

## OOP in Modern C++

### Smart Pointers and Memory Management

```cpp
#include <memory>

class Resource {
public:
    Resource() { std::cout << "Resource created" << std::endl; }
    ~Resource() { std::cout << "Resource destroyed" << std::endl; }
    void use() { std::cout << "Using resource" << std::endl; }
};

int main() {
    // Automatic memory management
    auto resource = std::make_unique<Resource>();
    resource->use();
    
    // Resource automatically destroyed when resource goes out of scope
    return 0;
}
```

### Move Semantics

```cpp
class LargeObject {
private:
    std::vector<int> data;
    
public:
    LargeObject(int size) : data(size, 42) {}
    
    // Move constructor
    LargeObject(LargeObject&& other) noexcept : data(std::move(other.data)) {
        std::cout << "Object moved" << std::endl;
    }
    
    // Move assignment operator
    LargeObject& operator=(LargeObject&& other) noexcept {
        if (this != &other) {
            data = std::move(other.data);
        }
        return *this;
    }
};
```

## Design Patterns Preview

OOP enables powerful design patterns:

```cpp
// Singleton Pattern - Only one instance allowed
class Logger {
private:
    static Logger* instance;
    Logger() {}  // Private constructor
    
public:
    static Logger* getInstance() {
        if (instance == nullptr) {
            instance = new Logger();
        }
        return instance;
    }
    
    void log(const std::string& message) {
        std::cout << "[LOG] " << message << std::endl;
    }
};

Logger* Logger::instance = nullptr;

// Factory Pattern - Create objects without specifying exact class
class ShapeFactory {
public:
    static std::unique_ptr<Shape> createShape(const std::string& type) {
        if (type == "circle") return std::make_unique<Circle>(1.0);
        if (type == "square") return std::make_unique<Square>(1.0);
        return nullptr;
    }
};
```

## Common OOP Mistakes to Avoid

### <i class="fa-solid fa-square-xmark"></i> **God Objects**

```cpp
// ✘ Class doing too many things
class UserManagerEmailSenderDatabaseLoggerPaymentProcessor {
    // Hundreds of methods handling everything
};

// → Separate responsibilities
class User {};
class UserManager {};
class EmailSender {};
class Logger {};
class PaymentProcessor {};
```

### <i class="fa-solid fa-square-xmark"></i> **Inappropriate Inheritance**

```cpp
// ✘ Using inheritance for code reuse when composition is better
class Rectangle {
    // Rectangle implementation
};

class Window : public Rectangle {  // Window "is-a" Rectangle?
    // This doesn't make semantic sense
};

// → Use composition instead
class Window {
private:
    Rectangle bounds;  // Window "has-a" Rectangle
public:
    // Window-specific functionality
};
```

## Getting Started with OOP

Start with simple, well-defined classes:

```cpp
class Point {
private:
    double x, y;
    
public:
    Point(double xPos, double yPos) : x(xPos), y(yPos) {}
    
    double getX() const { return x; }
    double getY() const { return y; }
    
    void setX(double xPos) { x = xPos; }
    void setY(double yPos) { y = yPos; }
    
    double distanceTo(const Point& other) const {
        double dx = x - other.x;
        double dy = y - other.y;
        return std::sqrt(dx * dx + dy * dy);
    }
};
```

## What's Next

As we explore OOP in C++, we'll cover:

- **Class Design**: Creating effective class hierarchies
- **Memory Management**: Constructors, destructors, and RAII
- **Advanced Inheritance**: Virtual functions and abstract classes
- **Operator Overloading**: Making objects work with built-in operators
- **Exception Safety**: Writing robust object-oriented code
- **Modern C++ Features**: Smart pointers, move semantics, and more

Object-oriented programming transforms how you think about and structure your programs. Instead of thinking in terms of functions that manipulate data, you'll think in terms of objects that have both data and behavior, making your code more intuitive and maintainable.

---

**Sections in This Chapter:**

- [Classes and Objects // → ](classes.md)
- [Constructors and Destructors // → ](constructors.md)
- [Inheritance // → ](inheritance.md)
- [Polymorphism // → ](polymorphism.md)
- [Encapsulation // → ](encapsulation.md)
