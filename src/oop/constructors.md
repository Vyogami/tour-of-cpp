# Constructors and Destructors

Constructors and destructors are special member functions that control object creation and destruction. They ensure objects are properly initialized when created and cleaned up when destroyed, making C++ programs safer and more reliable.

## What are Constructors?

A constructor is a special member function that:

- **Has the same name as the class**
- **Has no return type** (not even `void`)
- **Is automatically called when an object is created**
- **Initializes the object's data members**

```cpp
#include <iostream>
#include <string>

class Student {
private:
    std::string name;
    int age;
    double gpa;
    
public:
    // Constructor
    Student(const std::string& studentName, int studentAge, double studentGPA) {
        name = studentName;
        age = studentAge;
        gpa = studentGPA;
        std::cout << "Student " << name << " created!" << std::endl;
    }
    
    void displayInfo() const {
        std::cout << "Name: " << name << ", Age: " << age << ", GPA: " << gpa << std::endl;
    }
};

int main() {
    // Constructor is called automatically when object is created
    Student alice("Alice Johnson", 20, 3.8);
    Student bob("Bob Smith", 19, 3.5);
    
    alice.displayInfo();
    bob.displayInfo();
    
    return 0;
}
```

## Types of Constructors

### Default Constructor

A constructor that takes no parameters:

```cpp
#include <iostream>
#include <string>

class Rectangle {
private:
    double width;
    double height;
    
public:
    // Default constructor
    Rectangle() {
        width = 1.0;
        height = 1.0;
        std::cout << "Default rectangle created (1x1)" << std::endl;
    }
    
    // Parameterized constructor
    Rectangle(double w, double h) {
        width = w;
        height = h;
        std::cout << "Rectangle created (" << w << "x" << h << ")" << std::endl;
    }
    
    double getArea() const {
        return width * height;
    }
    
    void display() const {
        std::cout << "Rectangle: " << width << " x " << height 
                  << " (Area: " << getArea() << ")" << std::endl;
    }
};

int main() {
    Rectangle defaultRect;           // Calls default constructor
    Rectangle customRect(5.0, 3.0); // Calls parameterized constructor
    
    defaultRect.display();
    customRect.display();
    
    return 0;
}
```

### Parameterized Constructors

Constructors that accept arguments for initialization:

```cpp
#include <iostream>
#include <string>

class BankAccount {
private:
    std::string accountNumber;
    std::string ownerName;
    double balance;
    double interestRate;
    
public:
    // Constructor with validation
    BankAccount(const std::string& accNum, const std::string& owner, 
                double initialBalance, double rate = 0.01) {
        accountNumber = accNum;
        ownerName = owner;
        
        // Validate initial balance
        if (initialBalance >= 0) {
            balance = initialBalance;
        } else {
            std::cout << "Warning: Negative balance not allowed. Setting to 0." << std::endl;
            balance = 0.0;
        }
        
        // Validate interest rate
        if (rate >= 0 && rate <= 1.0) {
            interestRate = rate;
        } else {
            std::cout << "Warning: Invalid interest rate. Setting to 1%." << std::endl;
            interestRate = 0.01;
        }
        
        std::cout << "Account " << accountNumber << " created for " << ownerName << std::endl;
    }
    
    void displayAccount() const {
        std::cout << "Account: " << accountNumber << std::endl;
        std::cout << "Owner: " << ownerName << std::endl;
        std::cout << "Balance: $" << balance << std::endl;
        std::cout << "Interest Rate: " << (interestRate * 100) << "%" << std::endl;
    }
    
    void addInterest() {
        balance += balance * interestRate;
    }
};

int main() {
    BankAccount account1("ACC001", "John Doe", 1000.0);          // Default interest rate
    BankAccount account2("ACC002", "Jane Smith", 2500.0, 0.025); // Custom interest rate
    BankAccount account3("ACC003", "Bob Johnson", -100.0);       // Invalid balance
    
    account1.displayAccount();
    std::cout << std::endl;
    
    account2.displayAccount();
    account2.addInterest();
    std::cout << "After adding interest:" << std::endl;
    account2.displayAccount();
    std::cout << std::endl;
    
    account3.displayAccount();
    
    return 0;
}
```

### Copy Constructor

Creates a new object as a copy of an existing object:

```cpp
#include <iostream>
#include <string>

class Book {
private:
    std::string title;
    std::string author;
    int pages;
    
public:
    // Regular constructor
    Book(const std::string& bookTitle, const std::string& bookAuthor, int numPages) 
        : title(bookTitle), author(bookAuthor), pages(numPages) {
        std::cout << "Book '" << title << "' created" << std::endl;
    }
    
    // Copy constructor
    Book(const Book& other) {
        title = other.title;
        author = other.author;
        pages = other.pages;
        std::cout << "Book '" << title << "' copied" << std::endl;
    }
    
    void display() const {
        std::cout << "Title: " << title << ", Author: " << author 
                  << ", Pages: " << pages << std::endl;
    }
    
    void setTitle(const std::string& newTitle) {
        title = newTitle;
    }
};

int main() {
    Book original("1984", "George Orwell", 328);
    
    // Copy constructor is called
    Book copy1(original);           // Direct copy construction
    Book copy2 = original;          // Also calls copy constructor
    
    std::cout << "\nOriginal book:" << std::endl;
    original.display();
    
    std::cout << "\nCopied books:" << std::endl;
    copy1.display();
    copy2.display();
    
    // Modify copy to show they're independent
    copy1.setTitle("Animal Farm");
    
    std::cout << "\nAfter modifying copy1:" << std::endl;
    original.display();
    copy1.display();
    
    return 0;
}
```

## Member Initializer Lists

More efficient way to initialize members:

```cpp
#include <iostream>
#include <string>

class Person {
private:
    const std::string name;  // const member must be initialized
    int age;
    std::string email;
    
public:
    // ✘ This won't work for const members
    /*
    Person(const std::string& personName, int personAge, const std::string& personEmail) {
        name = personName;  // Error: can't assign to const member
        age = personAge;
        email = personEmail;
    }
    */
    
    // → Member initializer list
    Person(const std::string& personName, int personAge, const std::string& personEmail) 
        : name(personName), age(personAge), email(personEmail) {
        std::cout << "Person " << name << " initialized" << std::endl;
    }
    
    void display() const {
        std::cout << "Name: " << name << ", Age: " << age << ", Email: " << email << std::endl;
    }
};

class Complex {
private:
    double real;
    double imaginary;
    
public:
    // Multiple ways to initialize
    Complex() : real(0.0), imaginary(0.0) {
        std::cout << "Default complex number created: " << real << " + " << imaginary << "i" << std::endl;
    }
    
    Complex(double r) : real(r), imaginary(0.0) {
        std::cout << "Real complex number created: " << real << " + " << imaginary << "i" << std::endl;
    }
    
    Complex(double r, double i) : real(r), imaginary(i) {
        std::cout << "Complex number created: " << real << " + " << imaginary << "i" << std::endl;
    }
    
    void display() const {
        std::cout << real << " + " << imaginary << "i" << std::endl;
    }
};

int main() {
    Person alice("Alice Johnson", 25, "alice@email.com");
    alice.display();
    
    Complex c1;           // Default constructor
    Complex c2(5.0);      // Single parameter
    Complex c3(3.0, 4.0); // Two parameters
    
    c1.display();
    c2.display();
    c3.display();
    
    return 0;
}
```

### Initialization vs Assignment

```cpp
#include <iostream>
#include <string>

class Timer {
private:
    std::string name;
    int milliseconds;
    
public:
    // ✘ Assignment in constructor body (less efficient)
    Timer(const std::string& timerName, int ms) {
        name = timerName;        // Assignment (creates temporary, then assigns)
        milliseconds = ms;       // Assignment
        std::cout << "Timer " << name << " created (assignment)" << std::endl;
    }
    
    // → Member initializer list (more efficient)
    /*
    Timer(const std::string& timerName, int ms) 
        : name(timerName), milliseconds(ms) {  // Direct initialization
        std::cout << "Timer " << name << " created (initialization)" << std::endl;
    }
    */
};

class Expensive {
private:
    std::string data;
    
public:
    Expensive(const std::string& str) {
        std::cout << "Expensive object created with: " << str << std::endl;
        data = str;
    }
    
    Expensive(const Expensive& other) {
        std::cout << "Expensive object copied!" << std::endl;
        data = other.data;
    }
    
    Expensive& operator=(const Expensive& other) {
        std::cout << "Expensive object assigned!" << std::endl;
        if (this != &other) {
            data = other.data;
        }
        return *this;
    }
};

class Container {
private:
    Expensive member;
    
public:
    // This creates a temporary Expensive object, then assigns it
    Container(const std::string& str) {
        member = Expensive(str);  // Creates temporary, then assigns
    }
    
    // This directly constructs the member in place
    /*
    Container(const std::string& str) : member(str) {
        // Direct construction, no temporary objects
    }
    */
};
```

## Destructors

Destructors clean up resources when objects are destroyed:

```cpp
#include <iostream>
#include <string>

class FileManager {
private:
    std::string filename;
    bool isOpen;
    
public:
    // Constructor
    FileManager(const std::string& file) : filename(file), isOpen(false) {
        std::cout << "FileManager created for: " << filename << std::endl;
    }
    
    // Destructor - called automatically when object is destroyed
    ~FileManager() {
        if (isOpen) {
            close();
        }
        std::cout << "FileManager destroyed for: " << filename << std::endl;
    }
    
    void open() {
        if (!isOpen) {
            std::cout << "Opening file: " << filename << std::endl;
            isOpen = true;
        }
    }
    
    void close() {
        if (isOpen) {
            std::cout << "Closing file: " << filename << std::endl;
            isOpen = false;
        }
    }
    
    void write(const std::string& data) {
        if (isOpen) {
            std::cout << "Writing to " << filename << ": " << data << std::endl;
        } else {
            std::cout << "Error: File not open!" << std::endl;
        }
    }
};

void demonstrateFileManager() {
    FileManager file("data.txt");
    file.open();
    file.write("Hello, World!");
    // Destructor called automatically when function ends
}

int main() {
    std::cout << "Starting file operations..." << std::endl;
    demonstrateFileManager();
    std::cout << "File operations completed." << std::endl;
    
    {
        FileManager tempFile("temp.txt");
        tempFile.open();
        tempFile.write("Temporary data");
        // Destructor called when scope ends
    }
    
    std::cout << "Program ending..." << std::endl;
    
    return 0;
}
```

## Constructor Overloading

Multiple constructors with different parameter lists:

```cpp
#include <iostream>
#include <string>

class Date {
private:
    int day;
    int month;
    int year;
    
    bool isValidDate(int d, int m, int y) const {
        if (y < 1900 || y > 2100) return false;
        if (m < 1 || m > 12) return false;
        if (d < 1 || d > 31) return false;
        
        // Simple validation (doesn't account for leap years, etc.)
        int daysInMonth[] = {31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
        if (d > daysInMonth[m - 1]) return false;
        
        return true;
    }
    
public:
    // Default constructor - current date
    Date() : day(1), month(1), year(2024) {
        std::cout << "Default date created: ";
        display();
    }
    
    // Constructor with day, month, year
    Date(int d, int m, int y) {
        if (isValidDate(d, m, y)) {
            day = d;
            month = m;
            year = y;
        } else {
            std::cout << "Invalid date! Using default." << std::endl;
            day = 1;
            month = 1;
            year = 2024;
        }
        std::cout << "Date created: ";
        display();
    }
    
    // Constructor with formatted string "DD/MM/YYYY"
    Date(const std::string& dateStr) {
        // Simple parsing (in real code, use proper parsing)
        if (dateStr.length() == 10 && dateStr[2] == '/' && dateStr[5] == '/') {
            day = std::stoi(dateStr.substr(0, 2));
            month = std::stoi(dateStr.substr(3, 2));
            year = std::stoi(dateStr.substr(6, 4));
            
            if (!isValidDate(day, month, year)) {
                day = 1; month = 1; year = 2024;
                std::cout << "Invalid date string! Using default." << std::endl;
            }
        } else {
            day = 1; month = 1; year = 2024;
            std::cout << "Invalid date format! Using default." << std::endl;
        }
        std::cout << "Date from string created: ";
        display();
    }
    
    // Copy constructor
    Date(const Date& other) : day(other.day), month(other.month), year(other.year) {
        std::cout << "Date copied: ";
        display();
    }
    
    void display() const {
        std::cout << (day < 10 ? "0" : "") << day << "/"
                  << (month < 10 ? "0" : "") << month << "/"
                  << year << std::endl;
    }
    
    void setDate(int d, int m, int y) {
        if (isValidDate(d, m, y)) {
            day = d;
            month = m;
            year = y;
        }
    }
};

int main() {
    Date defaultDate;                    // Default constructor
    Date specificDate(25, 12, 2024);     // Parameterized constructor
    Date stringDate("15/06/2023");       // String constructor
    Date copiedDate(specificDate);       // Copy constructor
    
    std::cout << "\nAll dates:" << std::endl;
    std::cout << "Default: "; defaultDate.display();
    std::cout << "Specific: "; specificDate.display();
    std::cout << "From string: "; stringDate.display();
    std::cout << "Copied: "; copiedDate.display();
    
    // Test invalid dates
    Date invalidDate(32, 13, 2025);      // Invalid
    Date invalidString("not-a-date");    // Invalid format
    
    return 0;
}
```

## Constructor Delegation (C++11)

Constructors can call other constructors:

```cpp
#include <iostream>
#include <string>

class Employee {
private:
    std::string name;
    int id;
    std::string department;
    double salary;
    
public:
    // Master constructor
    Employee(const std::string& empName, int empId, const std::string& dept, double empSalary)
        : name(empName), id(empId), department(dept), salary(empSalary) {
        std::cout << "Full employee record created for " << name << std::endl;
    }
    
    // Delegating constructors
    Employee(const std::string& empName, int empId) 
        : Employee(empName, empId, "General", 50000.0) {  // Delegate to main constructor
        std::cout << "Employee with default department and salary" << std::endl;
    }
    
    Employee(const std::string& empName) 
        : Employee(empName, 0) {  // Delegate to previous constructor
        std::cout << "Employee with default ID, department, and salary" << std::endl;
    }
    
    Employee() 
        : Employee("Unknown Employee") {  // Delegate to previous constructor
        std::cout << "Default employee created" << std::endl;
    }
    
    void display() const {
        std::cout << "ID: " << id << ", Name: " << name 
                  << ", Dept: " << department << ", Salary: $" << salary << std::endl;
    }
};

int main() {
    Employee emp1("Alice Johnson", 1001, "Engineering", 75000.0);
    Employee emp2("Bob Smith", 1002);
    Employee emp3("Charlie Brown");
    Employee emp4;
    
    std::cout << "\nEmployee Details:" << std::endl;
    emp1.display();
    emp2.display();
    emp3.display();
    emp4.display();
    
    return 0;
}
```

## RAII (Resource Acquisition Is Initialization)

A fundamental C++ idiom using constructors and destructors:

```cpp
#include <iostream>
#include <string>
#include <stdexcept>

class DatabaseConnection {
private:
    std::string connectionString;
    bool connected;
    
public:
    DatabaseConnection(const std::string& connStr) : connectionString(connStr), connected(false) {
        std::cout << "Attempting to connect to database..." << std::endl;
        
        // Simulate connection
        if (connStr.empty()) {
            throw std::runtime_error("Invalid connection string");
        }
        
        connected = true;
        std::cout << "Database connection established: " << connectionString << std::endl;
    }
    
    ~DatabaseConnection() {
        if (connected) {
            std::cout << "Closing database connection: " << connectionString << std::endl;
            connected = false;
        }
    }
    
    void executeQuery(const std::string& query) {
        if (!connected) {
            throw std::runtime_error("Not connected to database");
        }
        std::cout << "Executing query: " << query << std::endl;
    }
    
    bool isConnected() const {
        return connected;
    }
    
    // Prevent copying (we'll learn about this in detail later)
    DatabaseConnection(const DatabaseConnection&) = delete;
    DatabaseConnection& operator=(const DatabaseConnection&) = delete;
};

void performDatabaseOperations() {
    try {
        DatabaseConnection db("server=localhost;database=myapp");
        
        db.executeQuery("SELECT * FROM users");
        db.executeQuery("UPDATE users SET active=1 WHERE id=123");
        
        // Connection automatically closed when db goes out of scope
    } catch (const std::exception& e) {
        std::cout << "Database error: " << e.what() << std::endl;
    }
}

int main() {
    std::cout << "Starting database operations..." << std::endl;
    performDatabaseOperations();
    std::cout << "Database operations completed." << std::endl;
    
    // Demonstrate exception safety
    try {
        DatabaseConnection badDb("");  // This will throw
    } catch (const std::exception& e) {
        std::cout << "Error creating connection: " << e.what() << std::endl;
    }
    
    return 0;
}
```

## Common Constructor/Destructor Patterns

### Singleton Pattern

```cpp
#include <iostream>

class Logger {
private:
    static Logger* instance;
    std::string logFile;
    
    // Private constructor prevents direct instantiation
    Logger(const std::string& filename = "app.log") : logFile(filename) {
        std::cout << "Logger initialized with file: " << logFile << std::endl;
    }
    
    // Prevent copying
    Logger(const Logger&) = delete;
    Logger& operator=(const Logger&) = delete;
    
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
    
    ~Logger() {
        std::cout << "Logger shutting down" << std::endl;
    }
    
    static void cleanup() {
        delete instance;
        instance = nullptr;
    }
};

// Static member definition
Logger* Logger::instance = nullptr;

int main() {
    Logger* logger1 = Logger::getInstance();
    Logger* logger2 = Logger::getInstance();
    
    // Both pointers point to the same instance
    std::cout << "Same instance? " << (logger1 == logger2) << std::endl;
    
    logger1->log("Application started");
    logger2->log("Processing data...");
    
    Logger::cleanup();
    
    return 0;
}
```

### Factory Pattern

```cpp
#include <iostream>
#include <memory>
#include <string>

class Shape {
public:
    virtual ~Shape() = default;
    virtual void draw() const = 0;
    virtual double getArea() const = 0;
};

class Circle : public Shape {
private:
    double radius;
    
public:
    Circle(double r) : radius(r) {
        std::cout << "Circle created with radius: " << radius << std::endl;
    }
    
    void draw() const override {
        std::cout << "Drawing circle with radius: " << radius << std::endl;
    }
    
    double getArea() const override {
        return 3.14159 * radius * radius;
    }
};

class Rectangle : public Shape {
private:
    double width, height;
    
public:
    Rectangle(double w, double h) : width(w), height(h) {
        std::cout << "Rectangle created: " << width << "x" << height << std::endl;
    }
    
    void draw() const override {
        std::cout << "Drawing rectangle: " << width << "x" << height << std::endl;
    }
    
    double getArea() const override {
        return width * height;
    }
};

class ShapeFactory {
public:
    static std::unique_ptr<Shape> createShape(const std::string& type, double param1, double param2 = 0) {
        if (type == "circle") {
            return std::make_unique<Circle>(param1);
        } else if (type == "rectangle") {
            return std::make_unique<Rectangle>(param1, param2);
        }
        return nullptr;
    }
};

int main() {
    auto circle = ShapeFactory::createShape("circle", 5.0);
    auto rectangle = ShapeFactory::createShape("rectangle", 4.0, 6.0);
    
    if (circle) {
        circle->draw();
        std::cout << "Area: " << circle->getArea() << std::endl;
    }
    
    if (rectangle) {
        rectangle->draw();
        std::cout << "Area: " << rectangle->getArea() << std::endl;
    }
    
    return 0;
}
```

## Best Practices

### Always Initialize Members

```cpp
// ✘ Uninitialized members
class BadExample {
private:
    int value;        // Uninitialized!
    std::string name; // Default constructed, but unclear intent
    double* data;     // Uninitialized pointer!
    
public:
    BadExample() {
        // Members already constructed, this is assignment
    }
};

// → Properly initialized members
class GoodExample {
private:
    int value;
    std::string name;
    double* data;
    
public:
    GoodExample() : value(0), name("default"), data(nullptr) {
        // Clear initialization
    }
    
    GoodExample(int v, const std::string& n) : value(v), name(n), data(nullptr) {
        // Parameterized initialization
    }
    
    ~GoodExample() {
        delete[] data;  // Clean up if allocated
    }
};
```

### Exception Safety

```cpp
#include <iostream>
#include <stdexcept>

class SafeContainer {
private:
    int* data;
    size_t size;
    
public:
    SafeContainer(size_t count) : data(nullptr), size(0) {
        if (count == 0) {
            throw std::invalid_argument("Size must be greater than 0");
        }
        
        data = new int[count];  // Might throw std::bad_alloc
        size = count;           // Only set size if allocation succeeded
        
        std::cout << "SafeContainer created with size: " << size << std::endl;
    }
    
    ~SafeContainer() {
        delete[] data;
        std::cout << "SafeContainer destroyed" << std::endl;
    }
    
    // Prevent copying for now (we'll cover this properly later)
    SafeContainer(const SafeContainer&) = delete;
    SafeContainer& operator=(const SafeContainer&) = delete;
};

int main() {
    try {
        SafeContainer container(10);
        // Use container...
    } catch (const std::exception& e) {
        std::cout << "Error: " << e.what() << std::endl;
    }
    
    try {
        SafeContainer badContainer(0);  // This will throw
    } catch (const std::exception& e) {
        std::cout << "Error: " << e.what() << std::endl;
    }
    
    return 0;
}
```

## Exercises

### Exercise 1: Bank Account System

Create a `BankAccount` class with:

- Multiple constructors (default, with initial balance, with all details)
- Proper validation in constructors
- Destructor that logs account closure
- Member initializer lists

### Exercise 2: Dynamic Array

Build a `DynamicArray` class that:

- Allocates memory in constructor
- Releases memory in destructor
- Has copy constructor for deep copying
- Validates parameters in all constructors

### Exercise 3: Student Management

Design a `Student` class with:

- Constructor overloading for different initialization scenarios
- Delegation between constructors
- Proper resource management
- Exception safety

### Exercise 4: Resource Manager

Create a generic resource management class that:

- Acquires resources in constructor
- Releases resources in destructor
- Demonstrates RAII principles
- Handles exceptions properly

## Key Takeaways

<i class="fa-solid fa-arrow-right"></i> **Constructors initialize objects** automatically when they're created  
<i class="fa-solid fa-arrow-right"></i> **Use member initializer lists** for efficient initialization  
<i class="fa-solid fa-arrow-right"></i> **Destructors clean up resources** automatically when objects are destroyed  
<i class="fa-solid fa-arrow-right"></i> **Constructor overloading** provides flexible object creation  
<i class="fa-solid fa-arrow-right"></i> **Copy constructors** create new objects from existing ones  
<i class="fa-solid fa-arrow-right"></i> **RAII ensures resource safety** through automatic cleanup  
<i class="fa-solid fa-arrow-right"></i> **Always initialize all members** to avoid undefined behavior  
<i class="fa-solid fa-arrow-right"></i> **Handle exceptions in constructors** to maintain object integrity

---

**Next**: Learn about building class relationships // →  [Inheritance](inheritance.md)
