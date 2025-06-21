# Classes and Objects

Classes are the foundation of object-oriented programming in C++. They allow you to create custom data types that combine data (member variables) and functions (member functions) into a single, cohesive unit. This chapter explores how to create, use, and design effective classes.

## What is a Class?

A class is a user-defined data type that serves as a blueprint for creating objects. It defines:

- **Data members** (variables that store the object's state)
- **Member functions** (methods that define what the object can do)
- **Access control** (who can access what parts of the object)

```cpp
#include <iostream>
#include <string>

// Class definition
class Book {
public:
    // Data members
    std::string title;
    std::string author;
    int pages;
    double price;
    
    // Member functions
    void displayInfo() {
        std::cout << "Title: " << title << std::endl;
        std::cout << "Author: " << author << std::endl;
        std::cout << "Pages: " << pages << std::endl;
        std::cout << "Price: $" << price << std::endl;
    }
    
    double calculateShippingCost() {
        return pages * 0.01;  // 1 cent per page
    }
};

int main() {
    // Creating objects (instances) of the Book class
    Book book1;
    book1.title = "The C++ Programming Language";
    book1.author = "Bjarne Stroustrup";
    book1.pages = 1376;
    book1.price = 89.99;
    
    Book book2;
    book2.title = "Effective C++";
    book2.author = "Scott Meyers";
    book2.pages = 320;
    book2.price = 54.99;
    
    book1.displayInfo();
    std::cout << "Shipping: $" << book1.calculateShippingCost() << std::endl;
    std::cout << std::endl;
    
    book2.displayInfo();
    std::cout << "Shipping: $" << book2.calculateShippingCost() << std::endl;
    
    return 0;
}
```

## Class vs Struct

In C++, `class` and `struct` are nearly identical, with one key difference:

```cpp
#include <iostream>

// struct members are public by default
struct Point2D {
    double x, y;  // public by default
    
    void print() {  // public by default
        std::cout << "(" << x << ", " << y << ")" << std::endl;
    }
};

// class members are private by default
class Point3D {
    double x, y, z;  // private by default
    
public:  // Must explicitly specify public
    void setCoordinates(double newX, double newY, double newZ) {
        x = newX;
        y = newY;
        z = newZ;
    }
    
    void print() {
        std::cout << "(" << x << ", " << y << ", " << z << ")" << std::endl;
    }
};

int main() {
    Point2D point2d;
    point2d.x = 5.0;  // → Accessible (public by default)
    point2d.y = 3.0;
    point2d.print();
    
    Point3D point3d;
    // point3d.x = 1.0;  // ✘ Error: x is private
    point3d.setCoordinates(1.0, 2.0, 3.0);  // → Use public method
    point3d.print();
    
    return 0;
}
```

## Access Specifiers

Control who can access class members:

### Public, Private, and Protected

```cpp
#include <iostream>
#include <string>

class Student {
private:
    // Only accessible within this class
    std::string studentId;
    double gpa;
    
protected:
    // Accessible within this class and derived classes
    std::string name;
    int age;
    
public:
    // Accessible from anywhere
    std::string major;
    
    // Public methods to access private data
    void setStudentInfo(const std::string& id, const std::string& studentName, int studentAge) {
        studentId = id;
        name = studentName;
        age = studentAge;
        gpa = 0.0;
    }
    
    void setGPA(double newGPA) {
        if (newGPA >= 0.0 && newGPA <= 4.0) {
            gpa = newGPA;
        } else {
            std::cout << "Invalid GPA. Must be between 0.0 and 4.0" << std::endl;
        }
    }
    
    double getGPA() const {
        return gpa;
    }
    
    std::string getStudentId() const {
        return studentId;
    }
    
    void displayInfo() const {
        std::cout << "ID: " << studentId << std::endl;
        std::cout << "Name: " << name << std::endl;
        std::cout << "Age: " << age << std::endl;
        std::cout << "Major: " << major << std::endl;
        std::cout << "GPA: " << gpa << std::endl;
    }
};

int main() {
    Student alice;
    
    alice.setStudentInfo("STU001", "Alice Johnson", 20);
    alice.major = "Computer Science";  // Public member
    alice.setGPA(3.8);
    
    alice.displayInfo();
    
    // alice.gpa = 5.0;  // ✘ Error: gpa is private
    // alice.name = "Bob";  // ✘ Error: name is protected
    
    return 0;
}
```

### Encapsulation Benefits

```cpp
#include <iostream>
#include <string>

class BankAccount {
private:
    std::string accountNumber;
    std::string ownerName;
    double balance;
    bool isActive;
    
    // Private helper method
    bool validateAmount(double amount) const {
        return amount > 0;
    }
    
public:
    BankAccount(const std::string& accNum, const std::string& owner, double initialBalance) {
        accountNumber = accNum;
        ownerName = owner;
        balance = (initialBalance >= 0) ? initialBalance : 0;
        isActive = true;
    }
    
    bool deposit(double amount) {
        if (!isActive) {
            std::cout << "Account is closed!" << std::endl;
            return false;
        }
        
        if (!validateAmount(amount)) {
            std::cout << "Invalid deposit amount!" << std::endl;
            return false;
        }
        
        balance += amount;
        std::cout << "Deposited $" << amount << ". New balance: $" << balance << std::endl;
        return true;
    }
    
    bool withdraw(double amount) {
        if (!isActive) {
            std::cout << "Account is closed!" << std::endl;
            return false;
        }
        
        if (!validateAmount(amount)) {
            std::cout << "Invalid withdrawal amount!" << std::endl;
            return false;
        }
        
        if (amount > balance) {
            std::cout << "Insufficient funds!" << std::endl;
            return false;
        }
        
        balance -= amount;
        std::cout << "Withdrew $" << amount << ". New balance: $" << balance << std::endl;
        return true;
    }
    
    double getBalance() const {
        return isActive ? balance : 0;
    }
    
    std::string getAccountInfo() const {
        return "Account: " + accountNumber + ", Owner: " + ownerName;
    }
    
    void closeAccount() {
        isActive = false;
        std::cout << "Account closed." << std::endl;
    }
};

int main() {
    BankAccount account("ACC123", "John Doe", 1000.0);
    
    std::cout << account.getAccountInfo() << std::endl;
    std::cout << "Initial balance: $" << account.getBalance() << std::endl;
    
    account.deposit(500.0);
    account.withdraw(200.0);
    account.withdraw(2000.0);  // Should fail
    
    std::cout << "Final balance: $" << account.getBalance() << std::endl;
    
    return 0;
}
```

## Member Functions

### Methods vs Functions

```cpp
#include <iostream>
#include <cmath>

class Circle {
private:
    double radius;
    
public:
    void setRadius(double r) {
        if (r > 0) {
            radius = r;
        }
    }
    
    double getRadius() const {
        return radius;
    }
    
    // Member function - has access to object's data
    double getArea() const {
        return M_PI * radius * radius;
    }
    
    double getCircumference() const {
        return 2 * M_PI * radius;
    }
    
    // Member function can call other member functions
    void displayInfo() const {
        std::cout << "Circle with radius: " << radius << std::endl;
        std::cout << "Area: " << getArea() << std::endl;
        std::cout << "Circumference: " << getCircumference() << std::endl;
    }
    
    // Member function can modify object state
    void scale(double factor) {
        if (factor > 0) {
            radius *= factor;
        }
    }
};

// Free function - doesn't belong to any class
double calculateDistance(const Circle& c1, const Circle& c2, double centerDistance) {
    double c1Radius = c1.getRadius();
    double c2Radius = c2.getRadius();
    return centerDistance - c1Radius - c2Radius;
}

int main() {
    Circle circle1, circle2;
    circle1.setRadius(5.0);
    circle2.setRadius(3.0);
    
    circle1.displayInfo();
    circle2.displayInfo();
    
    circle1.scale(2.0);
    std::cout << "After scaling circle1:" << std::endl;
    circle1.displayInfo();
    
    double distance = calculateDistance(circle1, circle2, 20.0);
    std::cout << "Distance between circles: " << distance << std::endl;
    
    return 0;
}
```

### Const Member Functions

```cpp
#include <iostream>
#include <string>

class Temperature {
private:
    double celsius;
    
public:
    Temperature(double temp = 0.0) : celsius(temp) {}
    
    // Const member functions - promise not to modify the object
    double getCelsius() const {
        return celsius;
    }
    
    double getFahrenheit() const {
        return (celsius * 9.0 / 5.0) + 32.0;
    }
    
    double getKelvin() const {
        return celsius + 273.15;
    }
    
    std::string getDescription() const {
        if (celsius < 0) return "Freezing";
        if (celsius < 10) return "Cold";
        if (celsius < 25) return "Cool";
        if (celsius < 35) return "Warm";
        return "Hot";
    }
    
    // Non-const member functions - can modify the object
    void setCelsius(double temp) {
        celsius = temp;
    }
    
    void setFahrenheit(double temp) {
        celsius = (temp - 32.0) * 5.0 / 9.0;
    }
    
    void adjustBy(double delta) {
        celsius += delta;
    }
};

// Function that takes const reference
void displayTemperature(const Temperature& temp) {
    std::cout << "Temperature: " << temp.getCelsius() << "°C" << std::endl;
    std::cout << "In Fahrenheit: " << temp.getFahrenheit() << "°F" << std::endl;
    std::cout << "Description: " << temp.getDescription() << std::endl;
    
    // temp.setCelsius(25);  // ✘ Error: can't call non-const method on const object
}

int main() {
    Temperature roomTemp(22.0);
    const Temperature freezingPoint(0.0);
    
    displayTemperature(roomTemp);
    displayTemperature(freezingPoint);
    
    roomTemp.adjustBy(5.0);  // → OK: roomTemp is not const
    // freezingPoint.adjustBy(1.0);  // ✘ Error: freezingPoint is const
    
    return 0;
}
```

## Static Members

### Static Data Members

```cpp
#include <iostream>
#include <string>

class Car {
private:
    std::string brand;
    std::string model;
    int year;
    
    // Static data member - shared by all instances
    static int totalCars;
    
public:
    Car(const std::string& b, const std::string& m, int y) 
        : brand(b), model(m), year(y) {
        totalCars++;  // Increment when new car is created
    }
    
    ~Car() {
        totalCars--;  // Decrement when car is destroyed
    }
    
    void displayInfo() const {
        std::cout << year << " " << brand << " " << model << std::endl;
    }
    
    // Static member function - can only access static members
    static int getTotalCars() {
        return totalCars;
    }
    
    static void displayTotalCars() {
        std::cout << "Total cars created: " << totalCars << std::endl;
    }
};

// Static member definition (required outside class)
int Car::totalCars = 0;

int main() {
    std::cout << "Initial cars: " << Car::getTotalCars() << std::endl;
    
    Car car1("Toyota", "Camry", 2023);
    Car car2("Honda", "Civic", 2022);
    
    Car::displayTotalCars();
    
    {
        Car car3("Ford", "Mustang", 2024);
        Car::displayTotalCars();
    }  // car3 destroyed here
    
    Car::displayTotalCars();
    
    return 0;
}
```

### Static vs Instance Members

```cpp
#include <iostream>
#include <string>

class Counter {
private:
    int instanceCount;           // Each object has its own copy
    static int globalCount;      // Shared by all objects
    static const int maxCount;   // Shared constant
    
public:
    Counter() : instanceCount(0) {
        globalCount++;
    }
    
    void increment() {
        instanceCount++;
        globalCount++;
    }
    
    void displayCounts() const {
        std::cout << "Instance count: " << instanceCount << std::endl;
        std::cout << "Global count: " << globalCount << std::endl;
        std::cout << "Max allowed: " << maxCount << std::endl;
    }
    
    // Static function can't access instance members
    static void displayGlobalCount() {
        std::cout << "Global count: " << globalCount << std::endl;
        // std::cout << instanceCount;  // ✘ Error: no access to instance members
    }
    
    // Static function to reset global count
    static void resetGlobalCount() {
        globalCount = 0;
    }
};

// Static member definitions
int Counter::globalCount = 0;
const int Counter::maxCount = 1000;

int main() {
    Counter counter1, counter2;
    
    counter1.increment();
    counter1.increment();
    
    counter2.increment();
    
    std::cout << "Counter 1:" << std::endl;
    counter1.displayCounts();
    
    std::cout << "\nCounter 2:" << std::endl;
    counter2.displayCounts();
    
    std::cout << "\nGlobal count from static function: ";
    Counter::displayGlobalCount();
    
    return 0;
}
```

## Friend Functions and Classes

### Friend Functions

```cpp
#include <iostream>
#include <cmath>

class Vector2D {
private:
    double x, y;
    
public:
    Vector2D(double xVal = 0, double yVal = 0) : x(xVal), y(yVal) {}
    
    void display() const {
        std::cout << "(" << x << ", " << y << ")" << std::endl;
    }
    
    // Friend function declaration
    friend double distance(const Vector2D& v1, const Vector2D& v2);
    friend Vector2D operator+(const Vector2D& v1, const Vector2D& v2);
    friend std::ostream& operator<<(std::ostream& os, const Vector2D& v);
};

// Friend function definition - can access private members
double distance(const Vector2D& v1, const Vector2D& v2) {
    double dx = v1.x - v2.x;  // Direct access to private members
    double dy = v1.y - v2.y;
    return std::sqrt(dx * dx + dy * dy);
}

Vector2D operator+(const Vector2D& v1, const Vector2D& v2) {
    return Vector2D(v1.x + v2.x, v1.y + v2.y);
}

std::ostream& operator<<(std::ostream& os, const Vector2D& v) {
    os << "(" << v.x << ", " << v.y << ")";
    return os;
}

int main() {
    Vector2D point1(3, 4);
    Vector2D point2(6, 8);
    
    std::cout << "Point 1: " << point1 << std::endl;
    std::cout << "Point 2: " << point2 << std::endl;
    
    double dist = distance(point1, point2);
    std::cout << "Distance: " << dist << std::endl;
    
    Vector2D sum = point1 + point2;
    std::cout << "Sum: " << sum << std::endl;
    
    return 0;
}
```

### Friend Classes

```cpp
#include <iostream>
#include <string>

class Engine {
private:
    int horsepower;
    std::string type;
    
public:
    Engine(int hp, const std::string& engineType) 
        : horsepower(hp), type(engineType) {}
    
    // Grant Car class access to private members
    friend class Car;
};

class Car {
private:
    std::string brand;
    Engine engine;
    
public:
    Car(const std::string& carBrand, int hp, const std::string& engineType)
        : brand(carBrand), engine(hp, engineType) {}
    
    void displaySpecs() const {
        std::cout << "Car: " << brand << std::endl;
        // Can access Engine's private members because Car is a friend
        std::cout << "Engine: " << engine.horsepower << " HP, " 
                  << engine.type << std::endl;
    }
    
    void tuneEngine(int additionalHP) {
        engine.horsepower += additionalHP;  // Direct access to private member
        std::cout << "Engine tuned! New power: " << engine.horsepower << " HP" << std::endl;
    }
};

int main() {
    Car sportsCar("Ferrari", 500, "V8");
    sportsCar.displaySpecs();
    sportsCar.tuneEngine(50);
    sportsCar.displaySpecs();
    
    return 0;
}
```

## Class Design Best Practices

### Single Responsibility Principle

```cpp
// ✘ Class doing too many things
class UserManagerEmailerLoggerValidator {
    // User data
    // Email functionality  
    // Logging functionality
    // Validation functionality
    // This violates single responsibility
};

// → Each class has one responsibility
class User {
private:
    std::string username;
    std::string email;
    
public:
    User(const std::string& name, const std::string& userEmail) 
        : username(name), email(userEmail) {}
    
    std::string getUsername() const { return username; }
    std::string getEmail() const { return email; }
};

class EmailValidator {
public:
    static bool isValidEmail(const std::string& email) {
        return email.find('@') != std::string::npos;
    }
};

class UserManager {
private:
    std::vector<User> users;
    
public:
    bool addUser(const User& user) {
        if (EmailValidator::isValidEmail(user.getEmail())) {
            users.push_back(user);
            return true;
        }
        return false;
    }
    
    void listUsers() const {
        for (const auto& user : users) {
            std::cout << user.getUsername() << " - " << user.getEmail() << std::endl;
        }
    }
};
```

### Proper Encapsulation

```cpp
#include <iostream>
#include <string>

class Rectangle {
private:
    double width;
    double height;
    
    // Private helper method
    void validateDimensions(double w, double h) {
        if (w <= 0 || h <= 0) {
            throw std::invalid_argument("Dimensions must be positive");
        }
    }
    
public:
    Rectangle(double w, double h) {
        validateDimensions(w, h);
        width = w;
        height = h;
    }
    
    // Getters
    double getWidth() const { return width; }
    double getHeight() const { return height; }
    
    // Setters with validation
    void setWidth(double w) {
        validateDimensions(w, height);
        width = w;
    }
    
    void setHeight(double h) {
        validateDimensions(width, h);
        height = h;
    }
    
    void setDimensions(double w, double h) {
        validateDimensions(w, h);
        width = w;
        height = h;
    }
    
    // Computed properties
    double getArea() const {
        return width * height;
    }
    
    double getPerimeter() const {
        return 2 * (width + height);
    }
    
    bool isSquare() const {
        return width == height;
    }
};

int main() {
    try {
        Rectangle rect(5.0, 3.0);
        
        std::cout << "Width: " << rect.getWidth() << std::endl;
        std::cout << "Height: " << rect.getHeight() << std::endl;
        std::cout << "Area: " << rect.getArea() << std::endl;
        std::cout << "Perimeter: " << rect.getPerimeter() << std::endl;
        std::cout << "Is square? " << std::boolalpha << rect.isSquare() << std::endl;
        
        rect.setDimensions(4.0, 4.0);
        std::cout << "After making it square: " << rect.isSquare() << std::endl;
        
        // This will throw an exception
        rect.setWidth(-1.0);
        
    } catch (const std::exception& e) {
        std::cout << "Error: " << e.what() << std::endl;
    }
    
    return 0;
}
```

## Common Class Design Patterns

### Data Class

```cpp
#include <iostream>
#include <string>

class Person {
private:
    std::string firstName;
    std::string lastName;
    int age;
    std::string email;
    
public:
    Person(const std::string& first, const std::string& last, int personAge, const std::string& personEmail)
        : firstName(first), lastName(last), age(personAge), email(personEmail) {}
    
    // Getters
    std::string getFirstName() const { return firstName; }
    std::string getLastName() const { return lastName; }
    std::string getFullName() const { return firstName + " " + lastName; }
    int getAge() const { return age; }
    std::string getEmail() const { return email; }
    
    // Setters
    void setFirstName(const std::string& name) { firstName = name; }
    void setLastName(const std::string& name) { lastName = name; }
    void setAge(int newAge) { if (newAge >= 0) age = newAge; }
    void setEmail(const std::string& newEmail) { email = newEmail; }
    
    // Utility methods
    void display() const {
        std::cout << "Name: " << getFullName() << std::endl;
        std::cout << "Age: " << age << std::endl;
        std::cout << "Email: " << email << std::endl;
    }
    
    bool isAdult() const {
        return age >= 18;
    }
};
```

### Service Class

```cpp
#include <iostream>
#include <vector>
#include <string>

class MathService {
public:
    static double calculateMean(const std::vector<double>& values) {
        if (values.empty()) return 0.0;
        
        double sum = 0.0;
        for (double value : values) {
            sum += value;
        }
        return sum / values.size();
    }
    
    static double calculateStandardDeviation(const std::vector<double>& values) {
        if (values.size() <= 1) return 0.0;
        
        double mean = calculateMean(values);
        double sumSquaredDifferences = 0.0;
        
        for (double value : values) {
            double difference = value - mean;
            sumSquaredDifferences += difference * difference;
        }
        
        return std::sqrt(sumSquaredDifferences / (values.size() - 1));
    }
    
    static std::vector<double> normalize(const std::vector<double>& values) {
        std::vector<double> normalized;
        double mean = calculateMean(values);
        double stdDev = calculateStandardDeviation(values);
        
        if (stdDev == 0.0) return values;  // Can't normalize
        
        for (double value : values) {
            normalized.push_back((value - mean) / stdDev);
        }
        
        return normalized;
    }
};
```

## Memory Management in Classes

### Resource Management

```cpp
#include <iostream>
#include <string>

class FileHandler {
private:
    std::string filename;
    bool isOpen;
    
public:
    FileHandler(const std::string& file) : filename(file), isOpen(false) {
        std::cout << "FileHandler created for: " << filename << std::endl;
    }
    
    ~FileHandler() {
        if (isOpen) {
            close();
        }
        std::cout << "FileHandler destroyed for: " << filename << std::endl;
    }
    
    bool open() {
        if (!isOpen) {
            std::cout << "Opening file: " << filename << std::endl;
            isOpen = true;
            return true;
        }
        return false;
    }
    
    void write(const std::string& data) {
        if (isOpen) {
            std::cout << "Writing to " << filename << ": " << data << std::endl;
        } else {
            std::cout << "File not open!" << std::endl;
        }
    }
    
    void close() {
        if (isOpen) {
            std::cout << "Closing file: " << filename << std::endl;
            isOpen = false;
        }
    }
    
    bool getIsOpen() const { return isOpen; }
};

int main() {
    {
        FileHandler file("data.txt");
        file.open();
        file.write("Hello, World!");
        // File automatically closed when destructor is called
    }  // file goes out of scope here
    
    std::cout << "File handler destroyed automatically" << std::endl;
    
    return 0;
}
```

## Exercises

### Exercise 1: Shopping Cart

Create a `Product` class and a `ShoppingCart` class:

- `Product`: name, price, category
- `ShoppingCart`: add/remove products, calculate total, apply discounts

### Exercise 2: Library System

Design classes for a library:

- `Book`: title, author, ISBN, availability
- `Member`: name, ID, borrowed books
- `Library`: manage books and members

### Exercise 3: Game Character

Create a role-playing game character system:

- `Character`: name, health, level, experience
- `Warrior`, `Mage`, `Archer`: different character types
- `Inventory`: manage items and equipment

### Exercise 4: Banking System

Build a banking application:

- `Account`: account number, balance, transaction history
- `SavingsAccount`, `CheckingAccount`: different account types
- `Bank`: manage multiple accounts

## Key Takeaways

<i class="fa-solid fa-arrow-right"></i> **Classes combine data and functions** into cohesive units  
<i class="fa-solid fa-arrow-right"></i> **Use access specifiers** to control visibility and maintain encapsulation  
<i class="fa-solid fa-arrow-right"></i> **Const member functions** promise not to modify object state  
<i class="fa-solid fa-arrow-right"></i> **Static members** are shared across all instances of a class  
<i class="fa-solid fa-arrow-right"></i> **Friend functions/classes** provide controlled access to private members  
<i class="fa-solid fa-arrow-right"></i> **Follow single responsibility principle** - each class should do one thing well  
<i class="fa-solid fa-arrow-right"></i> **Use proper encapsulation** with getters/setters and validation  
<i class="fa-solid fa-arrow-right"></i> **Design for resource management** with proper constructors and destructors

---

**Next**: Learn about object initialization and cleanup // →  [Constructors and Destructors](constructors.md)
