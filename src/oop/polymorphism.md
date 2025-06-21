# Polymorphism and Virtual Functions

Polymorphism is one of the most powerful features of object-oriented programming. It allows objects of different types to be treated as objects of a common base type, while still calling the appropriate methods for their actual type. This enables writing flexible, extensible code that can work with objects that don't even exist yet.

## What is Polymorphism?

Polymorphism means "many forms" - the same interface can have different implementations:

- **Compile-time polymorphism**: Function overloading, operator overloading, templates
- **Runtime polymorphism**: Virtual functions, dynamic dispatch

```cpp
#include <iostream>
#include <vector>
#include <memory>

// Base class with virtual function
class Instrument {
protected:
    std::string name;
    
public:
    Instrument(const std::string& instrumentName) : name(instrumentName) {}
    virtual ~Instrument() = default;
    
    // Virtual function - can be overridden
    virtual void play() const {
        std::cout << name << " makes a sound" << std::endl;
    }
    
    // Virtual function with different implementations
    virtual void tune() const {
        std::cout << "Tuning " << name << std::endl;
    }
    
    std::string getName() const { return name; }
};

class Piano : public Instrument {
public:
    Piano() : Instrument("Piano") {}
    
    void play() const override {
        std::cout << "Playing beautiful piano melodies ♪" << std::endl;
    }
    
    void tune() const override {
        std::cout << "Piano technician tuning 88 keys" << std::endl;
    }
};

class Guitar : public Instrument {
public:
    Guitar() : Instrument("Guitar") {}
    
    void play() const override {
        std::cout << "Strumming guitar chords ♫" << std::endl;
    }
    
    void tune() const override {
        std::cout << "Adjusting 6 guitar strings" << std::endl;
    }
};

class Drums : public Instrument {
public:
    Drums() : Instrument("Drums") {}
    
    void play() const override {
        std::cout << "Beating powerful drum rhythms ♪♫" << std::endl;
    }
    
    void tune() const override {
        std::cout << "Tightening drum heads for perfect pitch" << std::endl;
    }
};

// Polymorphic function - works with any Instrument
void performSong(const Instrument& instrument) {
    std::cout << "Preparing " << instrument.getName() << "..." << std::endl;
    instrument.tune();  // Polymorphic call
    instrument.play();  // Polymorphic call
    std::cout << std::endl;
}

int main() {
    // Create different instruments
    Piano piano;
    Guitar guitar;
    Drums drums;
    
    // Same function works with different types
    performSong(piano);
    performSong(guitar);
    performSong(drums);
    
    // Polymorphic container
    std::vector<std::unique_ptr<Instrument>> orchestra;
    orchestra.push_back(std::make_unique<Piano>());
    orchestra.push_back(std::make_unique<Guitar>());
    orchestra.push_back(std::make_unique<Drums>());
    
    std::cout << "Orchestra performance:" << std::endl;
    for (const auto& instrument : orchestra) {
        instrument->play();  // Polymorphic call through pointer
    }
    
    return 0;
}
```

## Virtual Functions Deep Dive

### How Virtual Functions Work

```cpp
#include <iostream>
#include <typeinfo>

class Base {
public:
    // Non-virtual function - static binding
    void regularFunction() const {
        std::cout << "Base::regularFunction()" << std::endl;
    }
    
    // Virtual function - dynamic binding
    virtual void virtualFunction() const {
        std::cout << "Base::virtualFunction()" << std::endl;
    }
    
    // Pure virtual function - must be implemented in derived class
    virtual void pureVirtualFunction() const = 0;
    
    virtual ~Base() = default;
};

class Derived : public Base {
public:
    void regularFunction() const {  // Hides base function
        std::cout << "Derived::regularFunction()" << std::endl;
    }
    
    void virtualFunction() const override {  // Overrides base function
        std::cout << "Derived::virtualFunction()" << std::endl;
    }
    
    void pureVirtualFunction() const override {
        std::cout << "Derived::pureVirtualFunction()" << std::endl;
    }
};

void demonstratePolymorphism() {
    Derived obj;
    Base* basePtr = &obj;
    Base& baseRef = obj;
    
    std::cout << "Direct object calls:" << std::endl;
    obj.regularFunction();        // Derived::regularFunction()
    obj.virtualFunction();        // Derived::virtualFunction()
    obj.pureVirtualFunction();    // Derived::pureVirtualFunction()
    
    std::cout << "\nThrough base pointer:" << std::endl;
    basePtr->regularFunction();        // Base::regularFunction() (static binding)
    basePtr->virtualFunction();        // Derived::virtualFunction() (dynamic binding)
    basePtr->pureVirtualFunction();    // Derived::pureVirtualFunction() (dynamic binding)
    
    std::cout << "\nThrough base reference:" << std::endl;
    baseRef.regularFunction();        // Base::regularFunction() (static binding)
    baseRef.virtualFunction();        // Derived::virtualFunction() (dynamic binding)
    baseRef.pureVirtualFunction();    // Derived::pureVirtualFunction() (dynamic binding)
}

int main() {
    demonstratePolymorphism();
    return 0;
}
```

### Virtual Function Performance

```cpp
#include <iostream>
#include <chrono>
#include <vector>
#include <memory>

class FastCalculator {
public:
    // Non-virtual function - direct call
    double calculate(double x) const {
        return x * x + 2 * x + 1;
    }
};

class Calculator {
public:
    virtual ~Calculator() = default;
    
    // Virtual function - involves vtable lookup
    virtual double calculate(double x) const = 0;
};

class SquareCalculator : public Calculator {
public:
    double calculate(double x) const override {
        return x * x + 2 * x + 1;
    }
};

class CubeCalculator : public Calculator {
public:
    double calculate(double x) const override {
        return x * x * x + 3 * x * x + 3 * x + 1;
    }
};

void demonstrateVirtualFunctionCost() {
    const int iterations = 10000000;
    
    // Direct function call
    FastCalculator fastCalc;
    auto start = std::chrono::high_resolution_clock::now();
    double result1 = 0;
    for (int i = 0; i < iterations; ++i) {
        result1 += fastCalc.calculate(i * 0.001);
    }
    auto end = std::chrono::high_resolution_clock::now();
    auto directTime = std::chrono::duration_cast<std::chrono::microseconds>(end - start);
    
    // Virtual function call
    std::unique_ptr<Calculator> calc = std::make_unique<SquareCalculator>();
    start = std::chrono::high_resolution_clock::now();
    double result2 = 0;
    for (int i = 0; i < iterations; ++i) {
        result2 += calc->calculate(i * 0.001);
    }
    end = std::chrono::high_resolution_clock::now();
    auto virtualTime = std::chrono::duration_cast<std::chrono::microseconds>(end - start);
    
    std::cout << "Direct call time: " << directTime.count() << " microseconds" << std::endl;
    std::cout << "Virtual call time: " << virtualTime.count() << " microseconds" << std::endl;
    std::cout << "Virtual overhead: " << (virtualTime.count() - directTime.count()) << " microseconds" << std::endl;
    std::cout << "Results (should be similar): " << result1 << " vs " << result2 << std::endl;
}
```

## Pure Virtual Functions and Abstract Classes

### Abstract Base Classes

```cpp
#include <iostream>
#include <vector>
#include <memory>
#include <string>

// Abstract base class - cannot be instantiated
class DatabaseConnection {
protected:
    std::string connectionString;
    bool connected;
    
public:
    DatabaseConnection(const std::string& connStr) : connectionString(connStr), connected(false) {}
    virtual ~DatabaseConnection() = default;
    
    // Pure virtual functions - must be implemented by derived classes
    virtual bool connect() = 0;
    virtual void disconnect() = 0;
    virtual bool executeQuery(const std::string& query) = 0;
    virtual std::vector<std::string> getResults() = 0;
    
    // Virtual function with default implementation
    virtual std::string getConnectionString() const {
        return connectionString;
    }
    
    // Non-virtual function
    bool isConnected() const {
        return connected;
    }
};

class MySQLConnection : public DatabaseConnection {
private:
    std::string username;
    std::string password;
    
public:
    MySQLConnection(const std::string& host, const std::string& user, const std::string& pass)
        : DatabaseConnection("mysql://" + host), username(user), password(pass) {}
    
    bool connect() override {
        std::cout << "Connecting to MySQL server: " << connectionString << std::endl;
        std::cout << "Using credentials: " << username << "/***" << std::endl;
        connected = true;
        return true;
    }
    
    void disconnect() override {
        if (connected) {
            std::cout << "Disconnecting from MySQL server" << std::endl;
            connected = false;
        }
    }
    
    bool executeQuery(const std::string& query) override {
        if (!connected) {
            std::cout << "Error: Not connected to MySQL" << std::endl;
            return false;
        }
        
        std::cout << "Executing MySQL query: " << query << std::endl;
        return true;
    }
    
    std::vector<std::string> getResults() override {
        return {"row1_col1", "row1_col2", "row2_col1", "row2_col2"};
    }
};

class PostgreSQLConnection : public DatabaseConnection {
private:
    int port;
    
public:
    PostgreSQLConnection(const std::string& host, int dbPort) 
        : DatabaseConnection("postgresql://" + host + ":" + std::to_string(dbPort)), port(dbPort) {}
    
    bool connect() override {
        std::cout << "Establishing PostgreSQL connection to " << connectionString << std::endl;
        connected = true;
        return true;
    }
    
    void disconnect() override {
        if (connected) {
            std::cout << "Closing PostgreSQL connection" << std::endl;
            connected = false;
        }
    }
    
    bool executeQuery(const std::string& query) override {
        if (!connected) {
            std::cout << "Error: PostgreSQL connection not established" << std::endl;
            return false;
        }
        
        std::cout << "Running PostgreSQL query: " << query << std::endl;
        return true;
    }
    
    std::vector<std::string> getResults() override {
        return {"pg_row1", "pg_row2", "pg_row3"};
    }
};

// Database manager that works with any database type
class DatabaseManager {
private:
    std::unique_ptr<DatabaseConnection> connection;
    
public:
    DatabaseManager(std::unique_ptr<DatabaseConnection> dbConn) 
        : connection(std::move(dbConn)) {}
    
    bool initialize() {
        if (connection) {
            return connection->connect();
        }
        return false;
    }
    
    void shutdown() {
        if (connection) {
            connection->disconnect();
        }
    }
    
    bool runQuery(const std::string& query) {
        if (connection) {
            return connection->executeQuery(query);
        }
        return false;
    }
    
    void displayResults() {
        if (connection) {
            auto results = connection->getResults();
            std::cout << "Query results:" << std::endl;
            for (const auto& result : results) {
                std::cout << "  " << result << std::endl;
            }
        }
    }
};

int main() {
    // Cannot instantiate abstract class
    // DatabaseConnection* conn = new DatabaseConnection("test");  // ✘ Error
    
    // Create different database connections
    auto mysqlConn = std::make_unique<MySQLConnection>("localhost", "user", "password");
    auto postgresConn = std::make_unique<PostgreSQLConnection>("localhost", 5432);
    
    // Use them polymorphically
    DatabaseManager mysqlManager(std::move(mysqlConn));
    DatabaseManager postgresManager(std::move(postgresConn));
    
    std::cout << "=== MySQL Database ===" << std::endl;
    mysqlManager.initialize();
    mysqlManager.runQuery("SELECT * FROM users");
    mysqlManager.displayResults();
    mysqlManager.shutdown();
    
    std::cout << "\n=== PostgreSQL Database ===" << std::endl;
    postgresManager.initialize();
    postgresManager.runQuery("SELECT * FROM customers");
    postgresManager.displayResults();
    postgresManager.shutdown();
    
    return 0;
}
```

## Virtual Destructors

### Why Virtual Destructors Matter

```cpp
#include <iostream>
#include <memory>

class Base {
public:
    Base() {
        std::cout << "Base constructor" << std::endl;
    }
    
    // ✘ Non-virtual destructor can cause problems
    ~Base() {
        std::cout << "Base destructor" << std::endl;
    }
    
    virtual void doSomething() {
        std::cout << "Base::doSomething()" << std::endl;
    }
};

class Derived : public Base {
private:
    int* data;
    
public:
    Derived() : data(new int[1000]) {
        std::cout << "Derived constructor - allocated memory" << std::endl;
    }
    
    ~Derived() {
        delete[] data;
        std::cout << "Derived destructor - freed memory" << std::endl;
    }
    
    void doSomething() override {
        std::cout << "Derived::doSomething()" << std::endl;
    }
};

void demonstrateDestructorProblem() {
    std::cout << "Creating Derived object through Base pointer:" << std::endl;
    Base* obj = new Derived();  // Derived constructor called
    
    obj->doSomething();  // Works fine due to virtual function
    
    std::cout << "Deleting object:" << std::endl;
    delete obj;  // ✘ Only Base destructor called! Memory leak!
    
    std::cout << "Object deleted\n" << std::endl;
}

// Fixed version with virtual destructor
class SafeBase {
public:
    SafeBase() {
        std::cout << "SafeBase constructor" << std::endl;
    }
    
    // → Virtual destructor ensures proper cleanup
    virtual ~SafeBase() {
        std::cout << "SafeBase destructor" << std::endl;
    }
    
    virtual void doSomething() {
        std::cout << "SafeBase::doSomething()" << std::endl;
    }
};

class SafeDerived : public SafeBase {
private:
    int* data;
    
public:
    SafeDerived() : data(new int[1000]) {
        std::cout << "SafeDerived constructor - allocated memory" << std::endl;
    }
    
    ~SafeDerived() {
        delete[] data;
        std::cout << "SafeDerived destructor - freed memory" << std::endl;
    }
    
    void doSomething() override {
        std::cout << "SafeDerived::doSomething()" << std::endl;
    }
};

void demonstrateVirtualDestructor() {
    std::cout << "Creating SafeDerived object through SafeBase pointer:" << std::endl;
    SafeBase* obj = new SafeDerived();  // SafeDerived constructor called
    
    obj->doSomething();  // Works fine
    
    std::cout << "Deleting object:" << std::endl;
    delete obj;  // → Both destructors called in correct order!
    
    std::cout << "Object properly deleted\n" << std::endl;
}

int main() {
    demonstrateDestructorProblem();
    demonstrateVirtualDestructor();
    
    // Modern C++ with smart pointers handles this automatically
    std::cout << "Using smart pointers:" << std::endl;
    {
        std::unique_ptr<SafeBase> smartObj = std::make_unique<SafeDerived>();
        smartObj->doSomething();
        // Automatic proper destruction when smartObj goes out of scope
    }
    std::cout << "Smart pointer automatically cleaned up" << std::endl;
    
    return 0;
}
```

## Runtime Type Information (RTTI)

### Dynamic Cast

```cpp
#include <iostream>
#include <vector>
#include <memory>
#include <typeinfo>

class Employee {
protected:
    std::string name;
    double baseSalary;
    
public:
    Employee(const std::string& empName, double salary) : name(empName), baseSalary(salary) {}
    virtual ~Employee() = default;
    
    virtual double calculatePay() const = 0;
    virtual void displayInfo() const {
        std::cout << "Employee: " << name << ", Base Salary: $" << baseSalary << std::endl;
    }
    
    std::string getName() const { return name; }
};

class Manager : public Employee {
private:
    double bonus;
    int teamSize;
    
public:
    Manager(const std::string& name, double salary, double mgmtBonus, int team) 
        : Employee(name, salary), bonus(mgmtBonus), teamSize(team) {}
    
    double calculatePay() const override {
        return baseSalary + bonus;
    }
    
    void displayInfo() const override {
        Employee::displayInfo();
        std::cout << "  Role: Manager, Bonus: $" << bonus 
                  << ", Team Size: " << teamSize << std::endl;
    }
    
    void conductMeeting() {
        std::cout << name << " is conducting a team meeting with " << teamSize << " members" << std::endl;
    }
    
    int getTeamSize() const { return teamSize; }
};

class Developer : public Employee {
private:
    std::string programmingLanguage;
    int projectCount;
    
public:
    Developer(const std::string& name, double salary, const std::string& language, int projects)
        : Employee(name, salary), programmingLanguage(language), projectCount(projects) {}
    
    double calculatePay() const override {
        return baseSalary + (projectCount * 1000);  // Bonus per project
    }
    
    void displayInfo() const override {
        Employee::displayInfo();
        std::cout << "  Role: Developer, Language: " << programmingLanguage 
                  << ", Projects: " << projectCount << std::endl;
    }
    
    void writeCode() {
        std::cout << name << " is writing " << programmingLanguage << " code" << std::endl;
    }
    
    std::string getLanguage() const { return programmingLanguage; }
};

class Salesperson : public Employee {
private:
    double commissionRate;
    double salesAmount;
    
public:
    Salesperson(const std::string& name, double salary, double commission, double sales)
        : Employee(name, salary), commissionRate(commission), salesAmount(sales) {}
    
    double calculatePay() const override {
        return baseSalary + (salesAmount * commissionRate);
    }
    
    void displayInfo() const override {
        Employee::displayInfo();
        std::cout << "  Role: Salesperson, Commission: " << (commissionRate * 100) 
                  << "%, Sales: $" << salesAmount << std::endl;
    }
    
    void makeSale(double amount) {
        salesAmount += amount;
        std::cout << name << " made a sale of $" << amount << std::endl;
    }
    
    double getSalesAmount() const { return salesAmount; }
};

void processEmployee(Employee* emp) {
    // Basic polymorphic operations
    emp->displayInfo();
    std::cout << "Pay: $" << emp->calculatePay() << std::endl;
    
    // Use dynamic_cast to check specific types
    if (Manager* mgr = dynamic_cast<Manager*>(emp)) {
        std::cout << "Special manager operation:" << std::endl;
        mgr->conductMeeting();
        std::cout << "Managing " << mgr->getTeamSize() << " team members" << std::endl;
    }
    else if (Developer* dev = dynamic_cast<Developer*>(emp)) {
        std::cout << "Special developer operation:" << std::endl;
        dev->writeCode();
        std::cout << "Specializes in " << dev->getLanguage() << std::endl;
    }
    else if (Salesperson* sales = dynamic_cast<Salesperson*>(emp)) {
        std::cout << "Special salesperson operation:" << std::endl;
        sales->makeSale(5000);
        std::cout << "Total sales: $" << sales->getSalesAmount() << std::endl;
    }
    
    std::cout << std::endl;
}

int main() {
    std::vector<std::unique_ptr<Employee>> employees;
    
    employees.push_back(std::make_unique<Manager>("Alice", 80000, 20000, 5));
    employees.push_back(std::make_unique<Developer>("Bob", 75000, "C++", 3));
    employees.push_back(std::make_unique<Salesperson>("Charlie", 50000, 0.05, 100000));
    
    std::cout << "Processing all employees:" << std::endl;
    for (auto& emp : employees) {
        processEmployee(emp.get());
    }
    
    // Type information at runtime
    std::cout << "Type information:" << std::endl;
    for (auto& emp : employees) {
        std::cout << "Employee " << emp->getName() 
                  << " is of type: " << typeid(*emp).name() << std::endl;
    }
    
    return 0;
}
```

### Type ID and Type Information

```cpp
#include <iostream>
#include <typeinfo>
#include <vector>
#include <memory>

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
    Circle(double r) : radius(r) {}
    
    void draw() const override {
        std::cout << "Drawing circle with radius " << radius << std::endl;
    }
    
    double getArea() const override {
        return 3.14159 * radius * radius;
    }
    
    double getRadius() const { return radius; }
};

class Rectangle : public Shape {
private:
    double width, height;
    
public:
    Rectangle(double w, double h) : width(w), height(h) {}
    
    void draw() const override {
        std::cout << "Drawing rectangle " << width << "x" << height << std::endl;
    }
    
    double getArea() const override {
        return width * height;
    }
    
    double getWidth() const { return width; }
    double getHeight() const { return height; }
};

void analyzeShape(const Shape& shape) {
    std::cout << "Shape analysis:" << std::endl;
    std::cout << "Type: " << typeid(shape).name() << std::endl;
    std::cout << "Area: " << shape.getArea() << std::endl;
    
    // Check if it's a specific type
    if (typeid(shape) == typeid(Circle)) {
        std::cout << "This is a Circle!" << std::endl;
        // Need dynamic_cast to actually use it as Circle
        const Circle* circle = dynamic_cast<const Circle*>(&shape);
        if (circle) {
            std::cout << "Radius: " << circle->getRadius() << std::endl;
        }
    }
    else if (typeid(shape) == typeid(Rectangle)) {
        std::cout << "This is a Rectangle!" << std::endl;
        const Rectangle* rect = dynamic_cast<const Rectangle*>(&shape);
        if (rect) {
            std::cout << "Dimensions: " << rect->getWidth() << "x" << rect->getHeight() << std::endl;
        }
    }
    
    shape.draw();
    std::cout << std::endl;
}

int main() {
    std::vector<std::unique_ptr<Shape>> shapes;
    shapes.push_back(std::make_unique<Circle>(5.0));
    shapes.push_back(std::make_unique<Rectangle>(4.0, 6.0));
    shapes.push_back(std::make_unique<Circle>(3.0));
    
    for (const auto& shape : shapes) {
        analyzeShape(*shape);
    }
    
    // Compare types
    Circle c1(1.0);
    Circle c2(2.0);
    Rectangle r1(1.0, 2.0);
    
    std::cout << "Type comparisons:" << std::endl;
    std::cout << "c1 and c2 same type? " << (typeid(c1) == typeid(c2)) << std::endl;
    std::cout << "c1 and r1 same type? " << (typeid(c1) == typeid(r1)) << std::endl;
    
    return 0;
}
```

## Operator Overloading

### Basic Operator Overloading

```cpp
#include <iostream>
#include <string>

class Complex {
private:
    double real;
    double imaginary;
    
public:
    Complex(double r = 0.0, double i = 0.0) : real(r), imaginary(i) {}
    
    // Copy constructor
    Complex(const Complex& other) : real(other.real), imaginary(other.imaginary) {}
    
    // Assignment operator
    Complex& operator=(const Complex& other) {
        if (this != &other) {
            real = other.real;
            imaginary = other.imaginary;
        }
        return *this;
    }
    
    // Arithmetic operators
    Complex operator+(const Complex& other) const {
        return Complex(real + other.real, imaginary + other.imaginary);
    }
    
    Complex operator-(const Complex& other) const {
        return Complex(real - other.real, imaginary - other.imaginary);
    }
    
    Complex operator*(const Complex& other) const {
        return Complex(
            real * other.real - imaginary * other.imaginary,
            real * other.imaginary + imaginary * other.real
        );
    }
    
    // Compound assignment operators
    Complex& operator+=(const Complex& other) {
        real += other.real;
        imaginary += other.imaginary;
        return *this;
    }
    
    Complex& operator-=(const Complex& other) {
        real -= other.real;
        imaginary -= other.imaginary;
        return *this;
    }
    
    // Comparison operators
    bool operator==(const Complex& other) const {
        return (real == other.real) && (imaginary == other.imaginary);
    }
    
    bool operator!=(const Complex& other) const {
        return !(*this == other);
    }
    
    // Unary operators
    Complex operator-() const {
        return Complex(-real, -imaginary);
    }
    
    Complex& operator++() {  // Prefix increment
        ++real;
        return *this;
    }
    
    Complex operator++(int) {  // Postfix increment
        Complex temp(*this);
        ++real;
        return temp;
    }
    
    // Stream operators (as friend functions)
    friend std::ostream& operator<<(std::ostream& os, const Complex& c);
    friend std::istream& operator>>(std::istream& is, Complex& c);
    
    // Getters
    double getReal() const { return real; }
    double getImaginary() const { return imaginary; }
    
    double magnitude() const {
        return std::sqrt(real * real + imaginary * imaginary);
    }
};

// Stream operators implementation
std::ostream& operator<<(std::ostream& os, const Complex& c) {
    os << c.real;
    if (c.imaginary >= 0) {
        os << " + " << c.imaginary << "i";
    } else {
        os << " - " << (-c.imaginary) << "i";
    }
    return os;
}

std::istream& operator>>(std::istream& is, Complex& c) {
    std::cout << "Enter real part: ";
    is >> c.real;
    std::cout << "Enter imaginary part: ";
    is >> c.imaginary;
    return is;
}

int main() {
    Complex c1(3.0, 4.0);
    Complex c2(1.0, 2.0);
    
    std::cout << "c1 = " << c1 << std::endl;
    std::cout << "c2 = " << c2 << std::endl;
    
    // Arithmetic operations
    Complex sum = c1 + c2;
    Complex diff = c1 - c2;
    Complex product = c1 * c2;
    
    std::cout << "c1 + c2 = " << sum << std::endl;
    std::cout << "c1 - c2 = " << diff << std::endl;
    std::cout << "c1 * c2 = " << product << std::endl;
    
    // Compound assignment
    Complex c3 = c1;
    c3 += c2;
    std::cout << "c3 (c1 += c2) = " << c3 << std::endl;
    
    // Comparison
    std::cout << "c1 == c2? " << (c1 == c2) << std::endl;
    std::cout << "c1 != c2? " << (c1 != c2) << std::endl;
    
    // Unary operators
    Complex neg = -c1;
    std::cout << "-c1 = " << neg << std::endl;
    
    Complex c4 = c1;
    std::cout << "c4++ = " << c4++ << ", c4 is now " << c4 << std::endl;
    std::cout << "++c4 = " << ++c4 << std::endl;
    
    return 0;
}
```

### Advanced Operator Overloading

```cpp
#include <iostream>
#include <vector>
#include <stdexcept>

class Matrix {
private:
    std::vector<std::vector<double>> data;
    size_t rows;
    size_t cols;
    
public:
    Matrix(size_t r, size_t c, double initialValue = 0.0) 
        : rows(r), cols(c), data(r, std::vector<double>(c, initialValue)) {}
    
    Matrix(std::initializer_list<std::initializer_list<double>> init) {
        rows = init.size();
        cols = (rows > 0) ? init.begin()->size() : 0;
        
        data.reserve(rows);
        for (const auto& row : init) {
            if (row.size() != cols) {
                throw std::invalid_argument("All rows must have the same number of columns");
            }
            data.emplace_back(row);
        }
    }
    
    // Subscript operator for row access
    std::vector<double>& operator[](size_t row) {
        if (row >= rows) {
            throw std::out_of_range("Row index out of range");
        }
        return data[row];
    }
    
    const std::vector<double>& operator[](size_t row) const {
        if (row >= rows) {
            throw std::out_of_range("Row index out of range");
        }
        return data[row];
    }
    
    // Function call operator for element access
    double& operator()(size_t row, size_t col) {
        if (row >= rows || col >= cols) {
            throw std::out_of_range("Matrix index out of range");
        }
        return data[row][col];
    }
    
    const double& operator()(size_t row, size_t col) const {
        if (row >= rows || col >= cols) {
            throw std::out_of_range("Matrix index out of range");
        }
        return data[row][col];
    }
    
    // Matrix addition
    Matrix operator+(const Matrix& other) const {
        if (rows != other.rows || cols != other.cols) {
            throw std::invalid_argument("Matrix dimensions must match for addition");
        }
        
        Matrix result(rows, cols);
        for (size_t i = 0; i < rows; ++i) {
            for (size_t j = 0; j < cols; ++j) {
                result[i][j] = data[i][j] + other[i][j];
            }
        }
        return result;
    }
    
    // Matrix multiplication
    Matrix operator*(const Matrix& other) const {
        if (cols != other.rows) {
            throw std::invalid_argument("Invalid dimensions for matrix multiplication");
        }
        
        Matrix result(rows, other.cols);
        for (size_t i = 0; i < rows; ++i) {
            for (size_t j = 0; j < other.cols; ++j) {
                for (size_t k = 0; k < cols; ++k) {
                    result[i][j] += data[i][k] * other[k][j];
                }
            }
        }
        return result;
    }
    
    // Scalar multiplication
    Matrix operator*(double scalar) const {
        Matrix result(rows, cols);
        for (size_t i = 0; i < rows; ++i) {
            for (size_t j = 0; j < cols; ++j) {
                result[i][j] = data[i][j] * scalar;
            }
        }
        return result;
    }
    
    // Comparison operator
    bool operator==(const Matrix& other) const {
        if (rows != other.rows || cols != other.cols) {
            return false;
        }
        
        for (size_t i = 0; i < rows; ++i) {
            for (size_t j = 0; j < cols; ++j) {
                if (data[i][j] != other[i][j]) {
                    return false;
                }
            }
        }
        return true;
    }
    
    // Stream output
    friend std::ostream& operator<<(std::ostream& os, const Matrix& matrix) {
        for (size_t i = 0; i < matrix.rows; ++i) {
            os << "[";
            for (size_t j = 0; j < matrix.cols; ++j) {
                os << matrix[i][j];
                if (j < matrix.cols - 1) os << ", ";
            }
            os << "]" << std::endl;
        }
        return os;
    }
    
    size_t getRows() const { return rows; }
    size_t getCols() const { return cols; }
};

// Non-member operator for scalar * matrix
Matrix operator*(double scalar, const Matrix& matrix) {
    return matrix * scalar;
}

int main() {
    try {
        // Create matrices using initializer list
        Matrix m1 = {{1, 2, 3}, 
                     {4, 5, 6}};
        
        Matrix m2 = {{7, 8}, 
                     {9, 10}, 
                     {11, 12}};
        
        std::cout << "Matrix m1:" << std::endl << m1 << std::endl;
        std::cout << "Matrix m2:" << std::endl << m2 << std::endl;
        
        // Matrix multiplication
        Matrix product = m1 * m2;
        std::cout << "m1 * m2:" << std::endl << product << std::endl;
        
        // Scalar multiplication
        Matrix scaled = m1 * 2.0;
        std::cout << "m1 * 2:" << std::endl << scaled << std::endl;
        
        Matrix scaled2 = 3.0 * m1;
        std::cout << "3 * m1:" << std::endl << scaled2 << std::endl;
        
        // Element access
        std::cout << "m1[0][1] = " << m1[0][1] << std::endl;
        std::cout << "m1(1, 2) = " << m1(1, 2) << std::endl;
        
        // Modify element
        m1(0, 0) = 99;
        std::cout << "After m1(0,0) = 99:" << std::endl << m1 << std::endl;
        
        // Matrix addition (same dimensions)
        Matrix m3 = {{1, 2, 3}, 
                     {4, 5, 6}};
        
        Matrix sum = m1 + m3;
        std::cout << "m1 + m3:" << std::endl << sum << std::endl;
        
    } catch (const std::exception& e) {
        std::cout << "Error: " << e.what() << std::endl;
    }
    
    return 0;
}
```

## Function Objects and Callable Objects

### Function Objects (Functors)

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <functional>

// Function object class
class Multiplier {
private:
    int factor;
    
public:
    Multiplier(int f) : factor(f) {}
    
    // Overload function call operator
    int operator()(int value) const {
        return value * factor;
    }
    
    // Can have state and other methods
    void setFactor(int newFactor) {
        factor = newFactor;
    }
    
    int getFactor() const {
        return factor;
    }
};

class Accumulator {
private:
    int sum;
    
public:
    Accumulator() : sum(0) {}
    
    int operator()(int value) {
        sum += value;
        return sum;
    }
    
    int getSum() const { return sum; }
    void reset() { sum = 0; }
};

// Predicate function object
class GreaterThan {
private:
    int threshold;
    
public:
    GreaterThan(int t) : threshold(t) {}
    
    bool operator()(int value) const {
        return value > threshold;
    }
};

void demonstrateFunctors() {
    std::vector<int> numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    
    std::cout << "Original numbers: ";
    for (int n : numbers) {
        std::cout << n << " ";
    }
    std::cout << std::endl;
    
    // Use Multiplier functor
    Multiplier times3(3);
    std::cout << "Multiplied by 3: ";
    for (int n : numbers) {
        std::cout << times3(n) << " ";
    }
    std::cout << std::endl;
    
    // Use with algorithms
    std::vector<int> doubled(numbers.size());
    Multiplier times2(2);
    std::transform(numbers.begin(), numbers.end(), doubled.begin(), times2);
    
    std::cout << "Doubled using transform: ";
    for (int n : doubled) {
        std::cout << n << " ";
    }
    std::cout << std::endl;
    
    // Use predicate functor
    GreaterThan gt5(5);
    int count = std::count_if(numbers.begin(), numbers.end(), gt5);
    std::cout << "Numbers greater than 5: " << count << std::endl;
    
    // Accumulator with state
    Accumulator acc;
    std::cout << "Running sum: ";
    for (int n : numbers) {
        std::cout << acc(n) << " ";
    }
    std::cout << std::endl;
    std::cout << "Final sum: " << acc.getSum() << std::endl;
}

int main() {
    demonstrateFunctors();
    
    // Lambda expressions (modern alternative to functors)
    std::vector<int> numbers = {1, 2, 3, 4, 5};
    
    // Lambda with capture
    int multiplier = 4;
    auto multiply = [multiplier](int x) { return x * multiplier; };
    
    std::cout << "\nUsing lambda expressions:" << std::endl;
    std::cout << "Numbers multiplied by 4: ";
    for (int n : numbers) {
        std::cout << multiply(n) << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

## Exercises

### Exercise 1: Shape Hierarchy with Polymorphism

Create a comprehensive shape system:

- Abstract `Shape` base class with virtual functions
- Derived classes: `Circle`, `Rectangle`, `Triangle`, `Polygon`
- Implement area, perimeter, and drawing methods
- Use virtual destructors and demonstrate polymorphic behavior
- Add operator overloading for comparison and stream operations

### Exercise 2: Mathematical Expression System

Build a mathematical expression evaluator:

- Abstract `Expression` base class
- Concrete classes: `Number`, `Variable`, `BinaryOperation`
- Implement evaluation and string representation
- Use polymorphism for different operation types
- Overload operators for natural expression building

### Exercise 3: Container Class with Operators

Design a custom container class:

- Dynamic array or list implementation
- Overload operators: `[]`, `+`, `+=`, `==`, `<<`
- Implement iterator support
- Add comparison and arithmetic operations
- Ensure proper memory management with RAII

### Exercise 4: Game Entity System

Create a game entity hierarchy:

- Base `Entity` class with virtual methods
- Derived classes: `Player`, `Enemy`, `Projectile`, `PowerUp`
- Implement update, render, and collision methods
- Use RTTI for type-specific behavior
- Add function objects for game logic

## Key Takeaways

<i class="fa-solid fa-arrow-right"></i> **Polymorphism enables flexible, extensible code** that works with objects of different types  
<i class="fa-solid fa-arrow-right"></i> **Virtual functions provide runtime polymorphism** through dynamic dispatch  
<i class="fa-solid fa-arrow-right"></i> **Virtual destructors are essential** for proper cleanup in inheritance hierarchies  
<i class="fa-solid fa-arrow-right"></i> **Pure virtual functions create abstract interfaces** that derived classes must implement  
<i class="fa-solid fa-arrow-right"></i> **RTTI allows type checking and casting** at runtime when needed  
<i class="fa-solid fa-arrow-right"></i> **Operator overloading makes objects behave like built-in types** with natural syntax  
<i class="fa-solid fa-arrow-right"></i> **Function objects provide stateful, reusable behavior** for algorithms and callbacks  
<i class="fa-solid fa-arrow-right"></i> **Design for polymorphism from the beginning** - it's hard to add later

---

**Next**: Learn about data hiding and access control // →  [Encapsulation](encapsulation.md)
