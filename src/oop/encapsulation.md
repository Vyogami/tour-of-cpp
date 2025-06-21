# Encapsulation and Access Control

Encapsulation is one of the fundamental principles of object-oriented programming. It involves bundling data and the methods that operate on that data together, while hiding the internal implementation details from the outside world. This promotes data integrity, security, and maintainable code design.

## What is Encapsulation?

Encapsulation achieves two main goals:

1. **Data Hiding**: Private implementation details are hidden from external code
2. **Interface Control**: Public methods provide controlled access to object functionality

```cpp
#include <iostream>
#include <string>
#include <stdexcept>

// ✘ Poor encapsulation - everything is public
class BadBankAccount {
public:
    std::string accountNumber;
    std::string ownerName;
    double balance;
    double interestRate;
    
    void addInterest() {
        balance += balance * interestRate;
    }
};

// → Good encapsulation - data is protected, interface is controlled
class GoodBankAccount {
private:
    std::string accountNumber;
    std::string ownerName;
    double balance;
    double interestRate;
    bool isActive;
    
    // Private helper methods
    bool isValidAmount(double amount) const {
        return amount > 0;
    }
    
    void logTransaction(const std::string& type, double amount) const {
        std::cout << "[LOG] " << type << ": $" << amount 
                  << " | Balance: $" << balance << std::endl;
    }
    
public:
    // Constructor ensures valid initial state
    GoodBankAccount(const std::string& accNum, const std::string& owner, double initialBalance)
        : accountNumber(accNum), ownerName(owner), interestRate(0.02), isActive(true) {
        if (initialBalance < 0) {
            throw std::invalid_argument("Initial balance cannot be negative");
        }
        balance = initialBalance;
    }
    
    // Controlled access methods
    bool deposit(double amount) {
        if (!isActive) {
            std::cout << "Account is closed!" << std::endl;
            return false;
        }
        
        if (!isValidAmount(amount)) {
            std::cout << "Invalid deposit amount!" << std::endl;
            return false;
        }
        
        balance += amount;
        logTransaction("DEPOSIT", amount);
        return true;
    }
    
    bool withdraw(double amount) {
        if (!isActive) {
            std::cout << "Account is closed!" << std::endl;
            return false;
        }
        
        if (!isValidAmount(amount)) {
            std::cout << "Invalid withdrawal amount!" << std::endl;
            return false;
        }
        
        if (amount > balance) {
            std::cout << "Insufficient funds!" << std::endl;
            return false;
        }
        
        balance -= amount;
        logTransaction("WITHDRAWAL", amount);
        return true;
    }
    
    // Read-only access to sensitive data
    double getBalance() const {
        return isActive ? balance : 0.0;
    }
    
    std::string getAccountNumber() const {
        return accountNumber;
    }
    
    std::string getOwnerName() const {
        return ownerName;
    }
    
    bool getIsActive() const {
        return isActive;
    }
    
    // Controlled modification
    void setInterestRate(double rate) {
        if (rate >= 0 && rate <= 0.1) {  // Max 10% interest
            interestRate = rate;
        } else {
            std::cout << "Invalid interest rate!" << std::endl;
        }
    }
    
    void closeAccount() {
        isActive = false;
        std::cout << "Account " << accountNumber << " closed." << std::endl;
    }
    
    void addInterest() {
        if (isActive) {
            double interest = balance * interestRate;
            balance += interest;
            logTransaction("INTEREST", interest);
        }
    }
};

void demonstrateEncapsulation() {
    // Bad example - direct access to data
    BadBankAccount badAccount;
    badAccount.accountNumber = "BAD123";
    badAccount.ownerName = "John Doe";
    badAccount.balance = 1000.0;
    badAccount.interestRate = 0.02;
    
    // Oops! Anyone can directly modify the balance
    badAccount.balance = -999999.0;  // This should not be allowed!
    std::cout << "Bad account balance: $" << badAccount.balance << std::endl;
    
    // Good example - controlled access
    GoodBankAccount goodAccount("GOOD456", "Jane Smith", 1000.0);
    
    std::cout << "\nGood account operations:" << std::endl;
    goodAccount.deposit(500.0);
    goodAccount.withdraw(200.0);
    goodAccount.addInterest();
    
    std::cout << "Final balance: $" << goodAccount.getBalance() << std::endl;
    
    // Cannot directly access private members
    // goodAccount.balance = -999999.0;  // ✘ Compilation error!
}

int main() {
    demonstrateEncapsulation();
    return 0;
}
```

## Access Specifiers in Detail

### Private Members

Only accessible within the same class:

```cpp
#include <iostream>
#include <vector>
#include <string>

class SecureVault {
private:
    std::string masterPassword;
    std::vector<std::string> secrets;
    int failedAttempts;
    bool isLocked;
    
    // Private helper methods
    bool authenticatePassword(const std::string& password) const {
        return password == masterPassword;
    }
    
    void lockVault() {
        isLocked = true;
        std::cout << "Vault locked due to security breach!" << std::endl;
    }
    
    void resetFailedAttempts() {
        failedAttempts = 0;
    }
    
public:
    SecureVault(const std::string& password) 
        : masterPassword(password), failedAttempts(0), isLocked(false) {
        std::cout << "🏦 Secure vault created" << std::endl;
    }
    
    bool addSecret(const std::string& password, const std::string& secret) {
        if (isLocked) {
            std::cout << "Vault is locked!" << std::endl;
            return false;
        }
        
        if (authenticatePassword(password)) {
            secrets.push_back(secret);
            resetFailedAttempts();
            std::cout << " Secret added successfully" << std::endl;
            return true;
        } else {
            failedAttempts++;
            std::cout << "Authentication failed. Attempts: " << failedAttempts << std::endl;
            
            if (failedAttempts >= 3) {
                lockVault();
            }
            return false;
        }
    }
    
    std::vector<std::string> getSecrets(const std::string& password) {
        if (isLocked) {
            std::cout << "Vault is locked!" << std::endl;
            return {};
        }
        
        if (authenticatePassword(password)) {
            resetFailedAttempts();
            std::cout << " Access granted" << std::endl;
            return secrets;
        } else {
            failedAttempts++;
            std::cout << "Authentication failed. Attempts: " << failedAttempts << std::endl;
            
            if (failedAttempts >= 3) {
                lockVault();
            }
            return {};
        }
    }
    
    bool changePassword(const std::string& oldPassword, const std::string& newPassword) {
        if (isLocked) {
            std::cout << "Vault is locked!" << std::endl;
            return false;
        }
        
        if (authenticatePassword(oldPassword)) {
            masterPassword = newPassword;
            resetFailedAttempts();
            std::cout << " Password changed successfully" << std::endl;
            return true;
        } else {
            failedAttempts++;
            std::cout << "Current password incorrect" << std::endl;
            
            if (failedAttempts >= 3) {
                lockVault();
            }
            return false;
        }
    }
    
    int getSecretCount() const {
        return secrets.size();
    }
    
    bool getIsLocked() const {
        return isLocked;
    }
};

int main() {
    SecureVault vault("super_secret_123");
    
    // Successful operations
    vault.addSecret("super_secret_123", "My diary is under the bed");
    vault.addSecret("super_secret_123", "The treasure map is in the attic");
    
    auto secrets = vault.getSecrets("super_secret_123");
    std::cout << "\n📋 Retrieved secrets:" << std::endl;
    for (const auto& secret : secrets) {
        std::cout << "  🤐 " << secret << std::endl;
    }
    
    // Failed attempts
    std::cout << "\n Trying wrong passwords..." << std::endl;
    vault.addSecret("wrong_password", "Should not work");
    vault.addSecret("another_wrong", "Still wrong");
    vault.addSecret("third_attempt", "This locks the vault");
    
    // Vault is now locked
    vault.addSecret("super_secret_123", "Cannot add after lock");
    
    return 0;
}
```

### Protected Members

Accessible within the class and its derived classes:

```cpp
#include <iostream>
#include <string>
#include <vector>

class Employee {
protected:
    std::string name;
    int employeeId;
    double baseSalary;
    std::vector<std::string> performanceNotes;
    
    // Protected helper methods for derived classes
    void addPerformanceNote(const std::string& note) {
        performanceNotes.push_back(note);
    }
    
    double calculateBasePay() const {
        return baseSalary;
    }
    
    virtual void validateSalary(double salary) {
        if (salary < 0) {
            throw std::invalid_argument("Salary cannot be negative");
        }
    }
    
public:
    Employee(const std::string& empName, int empId, double salary)
        : name(empName), employeeId(empId) {
        validateSalary(salary);
        baseSalary = salary;
    }
    
    virtual ~Employee() = default;
    
    // Public interface
    virtual double calculateTotalPay() const = 0;
    virtual void displayInfo() const {
        std::cout << "Employee: " << name << " (ID: " << employeeId << ")" << std::endl;
        std::cout << "Base Salary: $" << baseSalary << std::endl;
    }
    
    std::string getName() const { return name; }
    int getId() const { return employeeId; }
    
    void addNote(const std::string& note) {
        addPerformanceNote("Manager note: " + note);
    }
};

class SalariedEmployee : public Employee {
private:
    double annualBonus;
    
public:
    SalariedEmployee(const std::string& name, int id, double salary, double bonus)
        : Employee(name, id, salary), annualBonus(bonus) {}
    
    double calculateTotalPay() const override {
        // Can access protected members from base class
        double total = calculateBasePay() + annualBonus;
        return total;
    }
    
    void displayInfo() const override {
        Employee::displayInfo();  // Call base implementation
        std::cout << "Annual Bonus: $" << annualBonus << std::endl;
        std::cout << "Total Annual Pay: $" << calculateTotalPay() << std::endl;
    }
    
    void performAnnualReview() {
        // Can access protected method
        addPerformanceNote("Annual review completed");
        std::cout << "Annual review completed for " << name << std::endl;  // Can access protected data
    }
    
    void setBonus(double bonus) {
        if (bonus >= 0) {
            annualBonus = bonus;
            addPerformanceNote("Bonus adjusted to $" + std::to_string(bonus));
        }
    }
};

class HourlyEmployee : public Employee {
private:
    double hourlyRate;
    int hoursWorked;
    
protected:
    void validateSalary(double rate) override {
        Employee::validateSalary(rate);
        if (rate < 15.0) {  // Minimum wage check
            throw std::invalid_argument("Hourly rate must be at least $15.00");
        }
    }
    
public:
    HourlyEmployee(const std::string& name, int id, double rate, int hours)
        : Employee(name, id, rate), hourlyRate(rate), hoursWorked(hours) {}
    
    double calculateTotalPay() const override {
        // Can access protected base salary for benefits calculation
        double hourlyPay = hourlyRate * hoursWorked;
        double overtimePay = (hoursWorked > 40) ? (hoursWorked - 40) * hourlyRate * 0.5 : 0;
        return hourlyPay + overtimePay;
    }
    
    void displayInfo() const override {
        std::cout << "Employee: " << name << " (ID: " << employeeId << ")" << std::endl;  // Protected access
        std::cout << "Hourly Rate: $" << hourlyRate << std::endl;
        std::cout << "Hours Worked: " << hoursWorked << std::endl;
        std::cout << "Total Pay: $" << calculateTotalPay() << std::endl;
    }
    
    void logWorkHours(int hours) {
        hoursWorked = hours;
        // Can access protected method
        addPerformanceNote("Logged " + std::to_string(hours) + " hours");
    }
    
    void printPerformanceNotes() const {
        std::cout << "Performance notes for " << name << ":" << std::endl;
        for (const auto& note : performanceNotes) {  // Can access protected data
            std::cout << "  - " << note << std::endl;
        }
    }
};

int main() {
    SalariedEmployee manager("Alice Johnson", 1001, 80000, 15000);
    HourlyEmployee worker("Bob Smith", 1002, 25.0, 45);
    
    std::cout << "=== Manager Information ===" << std::endl;
    manager.displayInfo();
    manager.performAnnualReview();
    manager.addNote("Excellent leadership skills");
    
    std::cout << "\n=== Worker Information ===" << std::endl;
    worker.displayInfo();
    worker.logWorkHours(50);  // Includes overtime
    worker.addNote("Reliable and punctual");
    worker.printPerformanceNotes();
    
    // Cannot access protected members directly from outside the class hierarchy
    // std::cout << manager.name;  // ✘ Compilation error
    // manager.addPerformanceNote("Direct note");  // ✘ Compilation error
    
    return 0;
}
```

### Public Members

Accessible from anywhere:

```cpp
#include <iostream>
#include <string>
#include <vector>

class Library {
private:
    std::vector<std::string> books;
    std::string libraryName;
    int maxBooks;
    
public:
    // Public constructor
    Library(const std::string& name, int capacity = 1000) 
        : libraryName(name), maxBooks(capacity) {
        std::cout << "" << libraryName << " library created (capacity: " << maxBooks << ")" << std::endl;
    }
    
    // Public methods - the interface of the class
    bool addBook(const std::string& title) {
        if (books.size() >= maxBooks) {
            std::cout << "Library is full!" << std::endl;
            return false;
        }
        
        books.push_back(title);
        std::cout << " Added book: " << title << std::endl;
        return true;
    }
    
    bool removeBook(const std::string& title) {
        auto it = std::find(books.begin(), books.end(), title);
        if (it != books.end()) {
            books.erase(it);
            std::cout << " Removed book: " << title << std::endl;
            return true;
        }
        
        std::cout << "Book not found: " << title << std::endl;
        return false;
    }
    
    bool hasBook(const std::string& title) const {
        return std::find(books.begin(), books.end(), title) != books.end();
    }
    
    void listBooks() const {
        std::cout << "\nBooks in " << libraryName << ":" << std::endl;
        for (size_t i = 0; i < books.size(); ++i) {
            std::cout << "  " << (i + 1) << ". " << books[i] << std::endl;
        }
        std::cout << "Total: " << books.size() << "/" << maxBooks << std::endl;
    }
    
    std::string getName() const {
        return libraryName;
    }
    
    int getBookCount() const {
        return books.size();
    }
    
    int getCapacity() const {
        return maxBooks;
    }
    
    bool isFull() const {
        return books.size() >= maxBooks;
    }
    
    bool isEmpty() const {
        return books.empty();
    }
};

// Free function that uses the public interface
void demonstrateLibraryUsage(Library& lib) {
    std::cout << "\n Demonstrating " << lib.getName() << " usage:" << std::endl;
    
    // All these methods are publicly accessible
    lib.addBook("The C++ Programming Language");
    lib.addBook("Effective C++");
    lib.addBook("Design Patterns");
    
    std::cout << "Library has " << lib.getBookCount() << " books" << std::endl;
    
    if (lib.hasBook("Effective C++")) {
        std::cout << " Found 'Effective C++'" << std::endl;
    }
    
    lib.listBooks();
    
    lib.removeBook("Design Patterns");
    std::cout << "After removal: " << lib.getBookCount() << " books" << std::endl;
}

int main() {
    Library cityLibrary("City Central", 5);
    
    demonstrateLibraryUsage(cityLibrary);
    
    // Direct access to public members
    cityLibrary.addBook("Clean Code");
    cityLibrary.addBook("Refactoring");
    
    if (cityLibrary.isFull()) {
        std::cout << "\n Library is at capacity!" << std::endl;
    }
    
    cityLibrary.listBooks();
    
    return 0;
}
```

## Getters and Setters

### Basic Getters and Setters

```cpp
#include <iostream>
#include <string>
#include <stdexcept>

class Temperature {
private:
    double celsius;
    std::string unit;
    
    void validateTemperature(double temp) const {
        if (temp < -273.15) {
            throw std::invalid_argument("Temperature cannot be below absolute zero");
        }
    }
    
public:
    Temperature(double temp = 0.0, const std::string& tempUnit = "C") : unit(tempUnit) {
        validateTemperature(temp);
        celsius = temp;
    }
    
    // Getter methods (const - don't modify object)
    double getCelsius() const {
        return celsius;
    }
    
    double getFahrenheit() const {
        return (celsius * 9.0 / 5.0) + 32.0;
    }
    
    double getKelvin() const {
        return celsius + 273.15;
    }
    
    std::string getUnit() const {
        return unit;
    }
    
    double getValue() const {
        if (unit == "F") return getFahrenheit();
        if (unit == "K") return getKelvin();
        return celsius;
    }
    
    // Setter methods (with validation)
    void setCelsius(double temp) {
        validateTemperature(temp);
        celsius = temp;
        std::cout << "Temperature set to " << celsius << "°C" << std::endl;
    }
    
    void setFahrenheit(double temp) {
        double celsiusValue = (temp - 32.0) * 5.0 / 9.0;
        validateTemperature(celsiusValue);
        celsius = celsiusValue;
        std::cout << "Temperature set to " << temp << "°F (" << celsius << "°C)" << std::endl;
    }
    
    void setKelvin(double temp) {
        double celsiusValue = temp - 273.15;
        validateTemperature(celsiusValue);
        celsius = celsiusValue;
        std::cout << "Temperature set to " << temp << "K (" << celsius << "°C)" << std::endl;
    }
    
    void setUnit(const std::string& newUnit) {
        if (newUnit == "C" || newUnit == "F" || newUnit == "K") {
            unit = newUnit;
            std::cout << "Display unit changed to " << unit << std::endl;
        } else {
            std::cout << "Invalid unit. Use C, F, or K" << std::endl;
        }
    }
    
    // Convenience methods
    void display() const {
        std::cout << "Temperature: " << getValue() << "°" << unit << std::endl;
    }
    
    bool isFreezingPoint() const {
        return std::abs(celsius - 0.0) < 0.01;
    }
    
    bool isBoilingPoint() const {
        return std::abs(celsius - 100.0) < 0.01;
    }
};

int main() {
    try {
        Temperature temp(25.0);
        
        std::cout << "Initial temperature:" << std::endl;
        temp.display();
        
        std::cout << "\nConversions:" << std::endl;
        std::cout << "Celsius: " << temp.getCelsius() << "°C" << std::endl;
        std::cout << "Fahrenheit: " << temp.getFahrenheit() << "°F" << std::endl;
        std::cout << "Kelvin: " << temp.getKelvin() << "K" << std::endl;
        
        std::cout << "\nChanging temperature using setters:" << std::endl;
        temp.setFahrenheit(98.6);  // Body temperature
        temp.display();
        
        temp.setKelvin(273.15);    // Freezing point
        temp.display();
        std::cout << "Is freezing point? " << (temp.isFreezingPoint() ? "Yes" : "No") << std::endl;
        
        std::cout << "\nChanging display unit:" << std::endl;
        temp.setUnit("F");
        temp.display();
        
        temp.setUnit("K");
        temp.display();
        
        // This will throw an exception
        std::cout << "\nTrying invalid temperature:" << std::endl;
        temp.setCelsius(-300.0);
        
    } catch (const std::exception& e) {
        std::cout << "Error: " << e.what() << std::endl;
    }
    
    return 0;
}
```

### Smart Getters and Setters

```cpp
#include <iostream>
#include <string>
#include <chrono>
#include <ctime>

class UserProfile {
private:
    std::string username;
    std::string email;
    int age;
    std::string password;
    std::chrono::system_clock::time_point lastLogin;
    int loginAttempts;
    bool isActive;
    
    bool isValidEmail(const std::string& email) const {
        return email.find('@') != std::string::npos && 
               email.find('.') != std::string::npos;
    }
    
    bool isStrongPassword(const std::string& pass) const {
        if (pass.length() < 8) return false;
        
        bool hasDigit = false, hasLower = false, hasUpper = false;
        for (char c : pass) {
            if (std::isdigit(c)) hasDigit = true;
            if (std::islower(c)) hasLower = true;
            if (std::isupper(c)) hasUpper = true;
        }
        
        return hasDigit && hasLower && hasUpper;
    }
    
    void updateLastLogin() {
        lastLogin = std::chrono::system_clock::now();
    }
    
public:
    UserProfile(const std::string& user, const std::string& userEmail, int userAge)
        : username(user), email(userEmail), age(userAge), loginAttempts(0), isActive(true) {
        if (!isValidEmail(email)) {
            throw std::invalid_argument("Invalid email format");
        }
        if (age < 13) {
            throw std::invalid_argument("User must be at least 13 years old");
        }
        updateLastLogin();
    }
    
    // Getters with computed values
    std::string getUsername() const {
        return username;
    }
    
    std::string getEmail() const {
        // Return masked email for privacy
        size_t atPos = email.find('@');
        if (atPos > 2) {
            return email.substr(0, 2) + "***" + email.substr(atPos);
        }
        return email;
    }
    
    std::string getFullEmail() const {
        // Only for internal use or authorized access
        return email;
    }
    
    int getAge() const {
        return age;
    }
    
    std::string getAccountStatus() const {
        if (!isActive) return "Suspended";
        if (loginAttempts >= 3) return "Locked";
        return "Active";
    }
    
    int getDaysSinceLastLogin() const {
        auto now = std::chrono::system_clock::now();
        auto duration = std::chrono::duration_cast<std::chrono::hours>(now - lastLogin);
        return duration.count() / 24;
    }
    
    // Setters with validation and side effects
    bool setEmail(const std::string& newEmail) {
        if (!isValidEmail(newEmail)) {
            std::cout << "Invalid email format" << std::endl;
            return false;
        }
        
        std::cout << "📧 Email changed from " << getEmail() << " to ";
        email = newEmail;
        std::cout << getEmail() << std::endl;
        return true;
    }
    
    bool setAge(int newAge) {
        if (newAge < age) {
            std::cout << "Age cannot decrease" << std::endl;
            return false;
        }
        
        if (newAge > age + 1) {
            std::cout << "Age increase too large" << std::endl;
            return false;
        }
        
        age = newAge;
        std::cout << "🎂 Age updated to " << age << std::endl;
        return true;
    }
    
    bool setPassword(const std::string& newPassword) {
        if (!isStrongPassword(newPassword)) {
            std::cout << "Password must be at least 8 characters with uppercase, lowercase, and digits" << std::endl;
            return false;
        }
        
        password = newPassword;  // In real code, hash this!
        std::cout << "Password updated successfully" << std::endl;
        return true;
    }
    
    // Methods that change internal state
    bool login(const std::string& pass) {
        if (!isActive) {
            std::cout << "Account suspended" << std::endl;
            return false;
        }
        
        if (loginAttempts >= 3) {
            std::cout << "Account locked due to too many failed attempts" << std::endl;
            return false;
        }
        
        if (password == pass) {
            std::cout << " Login successful" << std::endl;
            loginAttempts = 0;
            updateLastLogin();
            return true;
        } else {
            loginAttempts++;
            std::cout << "Invalid password. Attempts: " << loginAttempts << "/3" << std::endl;
            return false;
        }
    }
    
    void resetLoginAttempts() {
        loginAttempts = 0;
        std::cout << "🔓 Login attempts reset" << std::endl;
    }
    
    void suspendAccount() {
        isActive = false;
        std::cout << "⛔ Account suspended" << std::endl;
    }
    
    void reactivateAccount() {
        isActive = true;
        loginAttempts = 0;
        std::cout << " Account reactivated" << std::endl;
    }
    
    void displayProfile() const {
        std::cout << "\n👤 User Profile:" << std::endl;
        std::cout << "Username: " << username << std::endl;
        std::cout << "Email: " << getEmail() << std::endl;
        std::cout << "Age: " << age << std::endl;
        std::cout << "Status: " << getAccountStatus() << std::endl;
        std::cout << "Days since last login: " << getDaysSinceLastLogin() << std::endl;
    }
};

int main() {
    try {
        UserProfile user("john_doe", "john.doe@example.com", 25);
        
        user.displayProfile();
        
        // Test setters
        std::cout << "\n Testing setters:" << std::endl;
        user.setPassword("MySecure123");
        user.setEmail("john.newemail@example.com");
        user.setAge(26);
        
        // Test login system
        std::cout << "\n🔑 Testing login:" << std::endl;
        user.login("wrong_password");
        user.login("another_wrong");
        user.login("MySecure123");  // Correct password
        
        user.displayProfile();
        
        // Test invalid operations
        std::cout << "\n Testing invalid operations:" << std::endl;
        user.setAge(20);  // Age cannot decrease
        user.setEmail("invalid_email");  // Invalid format
        user.setPassword("weak");  // Weak password
        
    } catch (const std::exception& e) {
        std::cout << "Error: " << e.what() << std::endl;
    }
    
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
    
    // Regular member function
    double magnitude() const {
        return std::sqrt(x * x + y * y);
    }
    
    void display() const {
        std::cout << "(" << x << ", " << y << ")";
    }
    
    // Friend function declarations
    friend double distance(const Vector2D& v1, const Vector2D& v2);
    friend Vector2D operator+(const Vector2D& v1, const Vector2D& v2);
    friend Vector2D operator*(const Vector2D& v, double scalar);
    friend Vector2D operator*(double scalar, const Vector2D& v);
    friend std::ostream& operator<<(std::ostream& os, const Vector2D& v);
    friend std::istream& operator>>(std::istream& is, Vector2D& v);
    
    // Friend class declaration
    friend class VectorAnalyzer;
};

// Friend function implementations
double distance(const Vector2D& v1, const Vector2D& v2) {
    // Can access private members directly
    double dx = v1.x - v2.x;
    double dy = v1.y - v2.y;
    return std::sqrt(dx * dx + dy * dy);
}

Vector2D operator+(const Vector2D& v1, const Vector2D& v2) {
    // Direct access to private members
    return Vector2D(v1.x + v2.x, v1.y + v2.y);
}

Vector2D operator*(const Vector2D& v, double scalar) {
    return Vector2D(v.x * scalar, v.y * scalar);
}

Vector2D operator*(double scalar, const Vector2D& v) {
    return v * scalar;  // Reuse the above operator
}

std::ostream& operator<<(std::ostream& os, const Vector2D& v) {
    os << "(" << v.x << ", " << v.y << ")";
    return os;
}

std::istream& operator>>(std::istream& is, Vector2D& v) {
    std::cout << "Enter x coordinate: ";
    is >> v.x;
    std::cout << "Enter y coordinate: ";
    is >> v.y;
    return is;
}

// Friend class - has access to all private members of Vector2D
class VectorAnalyzer {
public:
    static void analyzeVector(const Vector2D& v) {
        std::cout << "\n Vector Analysis:" << std::endl;
        std::cout << "Vector: " << v << std::endl;
        std::cout << "X component: " << v.x << std::endl;  // Direct private access
        std::cout << "Y component: " << v.y << std::endl;  // Direct private access
        std::cout << "Magnitude: " << v.magnitude() << std::endl;
        
        // Determine quadrant
        if (v.x > 0 && v.y > 0) std::cout << "Quadrant: I" << std::endl;
        else if (v.x < 0 && v.y > 0) std::cout << "Quadrant: II" << std::endl;
        else if (v.x < 0 && v.y < 0) std::cout << "Quadrant: III" << std::endl;
        else if (v.x > 0 && v.y < 0) std::cout << "Quadrant: IV" << std::endl;
        else std::cout << "On axis" << std::endl;
    }
    
    static Vector2D normalize(const Vector2D& v) {
        double mag = v.magnitude();
        if (mag == 0) return Vector2D(0, 0);
        
        // Create normalized vector using private member access
        return Vector2D(v.x / mag, v.y / mag);
    }
    
    static double dotProduct(const Vector2D& v1, const Vector2D& v2) {
        // Direct access to private members for efficient calculation
        return v1.x * v2.x + v1.y * v2.y;
    }
    
    static double crossProduct(const Vector2D& v1, const Vector2D& v2) {
        // 2D cross product returns scalar (z component of 3D cross product)
        return v1.x * v2.y - v1.y * v2.x;
    }
};

int main() {
    Vector2D v1(3, 4);
    Vector2D v2(1, 2);
    
    std::cout << "Vector 1: " << v1 << std::endl;
    std::cout << "Vector 2: " << v2 << std::endl;
    
    // Using friend functions
    std::cout << "\n➕ Vector Addition: " << v1 + v2 << std::endl;
    std::cout << "📏 Distance between vectors: " << distance(v1, v2) << std::endl;
    std::cout << "✖️ Vector scaling: " << v1 * 2.5 << std::endl;
    std::cout << "✖️ Scalar multiplication: " << 3.0 * v2 << std::endl;
    
    // Using friend class
    VectorAnalyzer::analyzeVector(v1);
    VectorAnalyzer::analyzeVector(v2);
    
    Vector2D normalized = VectorAnalyzer::normalize(v1);
    std::cout << "\n Normalized v1: " << normalized << std::endl;
    
    double dot = VectorAnalyzer::dotProduct(v1, v2);
    double cross = VectorAnalyzer::crossProduct(v1, v2);
    std::cout << "Dot product: " << dot << std::endl;
    std::cout << "Cross product: " << cross << std::endl;
    
    // Input using friend function
    std::cout << "\n📝 Enter a new vector:" << std::endl;
    Vector2D userVector;
    // std::cin >> userVector;  // Uncomment for interactive input
    // VectorAnalyzer::analyzeVector(userVector);
    
    return 0;
}
```

### When to Use Friend Functions

```cpp
#include <iostream>
#include <string>

class Matrix; // Forward declaration

class Complex {
private:
    double real, imaginary;
    
public:
    Complex(double r = 0, double i = 0) : real(r), imaginary(i) {}
    
    // Some operations are better as member functions
    Complex& operator+=(const Complex& other) {
        real += other.real;
        imaginary += other.imaginary;
        return *this;
    }
    
    Complex operator-() const {
        return Complex(-real, -imaginary);
    }
    
    // Some operations are better as friend functions
    friend Complex operator+(const Complex& a, const Complex& b);
    friend Complex operator*(const Complex& a, const Complex& b);
    friend bool operator==(const Complex& a, const Complex& b);
    friend std::ostream& operator<<(std::ostream& os, const Complex& c);
    
    // Sometimes we need friends for efficiency
    friend class ComplexCalculator;
    friend Matrix complexToMatrix(const Complex& c);
    
    double getReal() const { return real; }
    double getImaginary() const { return imaginary; }
};

// Friend function implementations
Complex operator+(const Complex& a, const Complex& b) {
    return Complex(a.real + b.real, a.imaginary + b.imaginary);
}

Complex operator*(const Complex& a, const Complex& b) {
    return Complex(
        a.real * b.real - a.imaginary * b.imaginary,
        a.real * b.imaginary + a.imaginary * b.real
    );
}

bool operator==(const Complex& a, const Complex& b) {
    return (a.real == b.real) && (a.imaginary == b.imaginary);
}

std::ostream& operator<<(std::ostream& os, const Complex& c) {
    os << c.real;
    if (c.imaginary >= 0) os << " + " << c.imaginary << "i";
    else os << " - " << (-c.imaginary) << "i";
    return os;
}

// Friend class for complex calculations
class ComplexCalculator {
public:
    static double magnitude(const Complex& c) {
        // Direct access to private members for efficiency
        return std::sqrt(c.real * c.real + c.imaginary * c.imaginary);
    }
    
    static double phase(const Complex& c) {
        return std::atan2(c.imaginary, c.real);
    }
    
    static Complex fromPolar(double magnitude, double phase) {
        return Complex(magnitude * std::cos(phase), magnitude * std::sin(phase));
    }
    
    static Complex conjugate(const Complex& c) {
        return Complex(c.real, -c.imaginary);
    }
    
    static void performAdvancedAnalysis(const Complex& c) {
        std::cout << "\n🧮 Advanced Complex Analysis:" << std::endl;
        std::cout << "Number: " << c << std::endl;
        std::cout << "Real part: " << c.real << std::endl;
        std::cout << "Imaginary part: " << c.imaginary << std::endl;
        std::cout << "Magnitude: " << magnitude(c) << std::endl;
        std::cout << "Phase: " << phase(c) << " radians" << std::endl;
        std::cout << "Conjugate: " << conjugate(c) << std::endl;
    }
};

class Matrix {
private:
    double a, b, c, d;  // 2x2 matrix
    
public:
    Matrix(double a11, double a12, double a21, double a22) : a(a11), b(a12), c(a21), d(a22) {}
    
    friend std::ostream& operator<<(std::ostream& os, const Matrix& m) {
        os << "[" << m.a << " " << m.b << "]" << std::endl;
        os << "[" << m.c << " " << m.d << "]";
        return os;
    }
    
    friend Matrix complexToMatrix(const Complex& comp);
};

// Friend function that converts Complex to Matrix representation
Matrix complexToMatrix(const Complex& comp) {
    // Can access private members of Complex
    return Matrix(comp.real, -comp.imaginary, comp.imaginary, comp.real);
}

int main() {
    Complex c1(3, 4);
    Complex c2(1, 2);
    
    std::cout << "Complex numbers:" << std::endl;
    std::cout << "c1 = " << c1 << std::endl;
    std::cout << "c2 = " << c2 << std::endl;
    
    // Using friend functions
    Complex sum = c1 + c2;
    Complex product = c1 * c2;
    
    std::cout << "\nOperations:" << std::endl;
    std::cout << "c1 + c2 = " << sum << std::endl;
    std::cout << "c1 * c2 = " << product << std::endl;
    std::cout << "c1 == c2? " << (c1 == c2) << std::endl;
    
    // Using friend class
    ComplexCalculator::performAdvancedAnalysis(c1);
    ComplexCalculator::performAdvancedAnalysis(product);
    
    // Using friend function with different class
    Matrix m = complexToMatrix(c1);
    std::cout << "\nMatrix representation of " << c1 << ":" << std::endl;
    std::cout << m << std::endl;
    
    return 0;
}
```

## Encapsulation Best Practices

### Data Validation and Invariants

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <stdexcept>
#include <algorithm>

class Rectangle {
private:
    double width;
    double height;
    
    // Private method to maintain class invariants
    void validateDimensions(double w, double h) const {
        if (w <= 0 || h <= 0) {
            throw std::invalid_argument("Width and height must be positive");
        }
        if (w > 1000 || h > 1000) {
            throw std::invalid_argument("Dimensions cannot exceed 1000 units");
        }
    }
    
public:
    Rectangle(double w, double h) {
        validateDimensions(w, h);
        width = w;
        height = h;
    }
    
    // Controlled access with validation
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
    
    // Read-only access
    double getWidth() const { return width; }
    double getHeight() const { return height; }
    
    // Computed properties
    double getArea() const { return width * height; }
    double getPerimeter() const { return 2 * (width + height); }
    
    bool isSquare() const { return width == height; }
    
    void display() const {
        std::cout << "Rectangle: " << width << " x " << height 
                  << " (Area: " << getArea() << ", Perimeter: " << getPerimeter() << ")" << std::endl;
    }
};

class StudentGrades {
private:
    std::string studentName;
    std::vector<double> grades;
    double maxGrade;
    
    void validateGrade(double grade) const {
        if (grade < 0 || grade > maxGrade) {
            throw std::invalid_argument("Grade must be between 0 and " + std::to_string(maxGrade));
        }
    }
    
public:
    StudentGrades(const std::string& name, double maxGradeValue = 100.0) 
        : studentName(name), maxGrade(maxGradeValue) {
        if (maxGrade <= 0) {
            throw std::invalid_argument("Maximum grade must be positive");
        }
    }
    
    void addGrade(double grade) {
        validateGrade(grade);
        grades.push_back(grade);
        std::cout << "Added grade " << grade << " for " << studentName << std::endl;
    }
    
    bool updateGrade(size_t index, double newGrade) {
        if (index >= grades.size()) {
            std::cout << "Invalid grade index" << std::endl;
            return false;
        }
        
        try {
            validateGrade(newGrade);
            double oldGrade = grades[index];
            grades[index] = newGrade;
            std::cout << "Updated grade from " << oldGrade << " to " << newGrade << std::endl;
            return true;
        } catch (const std::exception& e) {
            std::cout << "Cannot update grade: " << e.what() << std::endl;
            return false;
        }
    }
    
    bool removeGrade(size_t index) {
        if (index >= grades.size()) {
            std::cout << "Invalid grade index" << std::endl;
            return false;
        }
        
        double removedGrade = grades[index];
        grades.erase(grades.begin() + index);
        std::cout << "Removed grade " << removedGrade << std::endl;
        return true;
    }
    
    // Read-only access to data
    std::string getName() const { return studentName; }
    size_t getGradeCount() const { return grades.size(); }
    double getMaxGrade() const { return maxGrade; }
    
    double getGrade(size_t index) const {
        if (index >= grades.size()) {
            throw std::out_of_range("Grade index out of range");
        }
        return grades[index];
    }
    
    // Computed statistics
    double getAverage() const {
        if (grades.empty()) return 0.0;
        
        double sum = 0;
        for (double grade : grades) {
            sum += grade;
        }
        return sum / grades.size();
    }
    
    double getHighestGrade() const {
        if (grades.empty()) return 0.0;
        return *std::max_element(grades.begin(), grades.end());
    }
    
    double getLowestGrade() const {
        if (grades.empty()) return 0.0;
        return *std::min_element(grades.begin(), grades.end());
    }
    
    char getLetterGrade() const {
        double avg = getAverage();
        double percentage = (avg / maxGrade) * 100;
        
        if (percentage >= 90) return 'A';
        if (percentage >= 80) return 'B';
        if (percentage >= 70) return 'C';
        if (percentage >= 60) return 'D';
        return 'F';
    }
    
    void displayReport() const {
        std::cout << "\n📊 Grade Report for " << studentName << ":" << std::endl;
        std::cout << "Number of grades: " << grades.size() << std::endl;
        
        if (!grades.empty()) {
            std::cout << "Grades: ";
            for (size_t i = 0; i < grades.size(); ++i) {
                std::cout << grades[i];
                if (i < grades.size() - 1) std::cout << ", ";
            }
            std::cout << std::endl;
            
            std::cout << "Average: " << getAverage() << "/" << maxGrade << std::endl;
            std::cout << "Highest: " << getHighestGrade() << std::endl;
            std::cout << "Lowest: " << getLowestGrade() << std::endl;
            std::cout << "Letter Grade: " << getLetterGrade() << std::endl;
        } else {
            std::cout << "No grades recorded" << std::endl;
        }
    }
};

int main() {
    try {
        // Rectangle with validated dimensions
        std::cout << "=== Rectangle Example ===" << std::endl;
        Rectangle rect(5, 3);
        rect.display();
        
        rect.setWidth(8);
        rect.display();
        
        std::cout << "Is square? " << (rect.isSquare() ? "Yes" : "No") << std::endl;
        
        // This will throw an exception
        // rect.setWidth(-5);
        
        // Student grades with validation
        std::cout << "\n=== Student Grades Example ===" << std::endl;
        StudentGrades alice("Alice Johnson");
        
        alice.addGrade(85.5);
        alice.addGrade(92.0);
        alice.addGrade(78.5);
        alice.addGrade(88.0);
        
        alice.displayReport();
        
        alice.updateGrade(1, 95.0);  // Update second grade
        alice.displayReport();
        
        // Try invalid operations
        std::cout << "\n Testing invalid operations:" << std::endl;
        alice.addGrade(105.0);  // Should fail
        alice.updateGrade(10, 90.0);  // Invalid index
        
    } catch (const std::exception& e) {
        std::cout << "Error: " << e.what() << std::endl;
    }
    
    return 0;
}
```

## Exercises

### Exercise 1: Bank Account System

Create a comprehensive bank account system:

- `BankAccount` base class with proper encapsulation
- Derived classes: `SavingsAccount`, `CheckingAccount`, `CreditAccount`
- Implement transaction history, interest calculation, and overdraft protection
- Use getters/setters with validation
- Add friend functions for account transfers

### Exercise 2: Student Management System

Design a student management system:

- `Student` class with private data members
- `Course` and `Grade` classes with proper encapsulation
- Implement enrollment, grading, and GPA calculation
- Use friend classes for administrative access
- Add data validation and business rules

### Exercise 3: Game Character System

Build a game character with encapsulation:

- `Character` class with private stats (health, mana, experience)
- `Inventory` class for item management
- Implement level progression and skill systems
- Use controlled access for game balance
- Add friend functions for character interactions

### Exercise 4: Library Management System

Create a library management system:

- `Book`, `Member`, and `Library` classes
- Implement borrowing, returning, and reservation systems
- Use encapsulation for data integrity
- Add validation for library policies
- Include friend functions for administrative operations

## Key Takeaways

<i class="fa-solid fa-arrow-right"></i> **Encapsulation protects data integrity** by controlling access to class members  
<i class="fa-solid fa-arrow-right"></i> **Use private members for implementation details** that shouldn't be accessed externally  
<i class="fa-solid fa-arrow-right"></i> **Protected members enable inheritance** while maintaining encapsulation  
<i class="fa-solid fa-arrow-right"></i> **Public members form the class interface** - design them carefully  
<i class="fa-solid fa-arrow-right"></i> **Getters and setters provide controlled access** with validation and computed values  
<i class="fa-solid fa-arrow-right"></i> **Friend functions/classes break encapsulation** - use them sparingly and with purpose  
<i class="fa-solid fa-arrow-right"></i> **Always validate data** in constructors and setters to maintain class invariants  
<i class="fa-solid fa-arrow-right"></i> **Design the public interface first** - it's harder to change later

---

**Next**: Explore generic programming and code reuse // →  [Templates Overview](../templates/README.md)
