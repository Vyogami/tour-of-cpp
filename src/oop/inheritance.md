# Inheritance

Inheritance is a fundamental feature of object-oriented programming that allows you to create new classes based on existing ones. It enables code reuse, establishes relationships between classes, and supports polymorphism by allowing derived classes to extend or modify the behavior of their base classes.

## What is Inheritance?

Inheritance creates an "is-a" relationship between classes:

- **Base class** (parent/superclass): The class being inherited from
- **Derived class** (child/subclass): The class that inherits from the base class
- Derived classes automatically get all members of the base class
- Derived classes can add new members or override existing ones

```cpp
#include <iostream>
#include <string>

// Base class
class Animal {
protected:
    std::string name;
    int age;
    
public:
    Animal(const std::string& animalName, int animalAge) 
        : name(animalName), age(animalAge) {
        std::cout << "Animal " << name << " created" << std::endl;
    }
    
    void eat() {
        std::cout << name << " is eating." << std::endl;
    }
    
    void sleep() {
        std::cout << name << " is sleeping." << std::endl;
    }
    
    void displayInfo() {
        std::cout << "Name: " << name << ", Age: " << age << std::endl;
    }
};

// Derived class
class Dog : public Animal {
private:
    std::string breed;
    
public:
    Dog(const std::string& dogName, int dogAge, const std::string& dogBreed) 
        : Animal(dogName, dogAge), breed(dogBreed) {  // Call base constructor
        std::cout << "Dog " << name << " (" << breed << ") created" << std::endl;
    }
    
    void bark() {
        std::cout << name << " barks: Woof! Woof!" << std::endl;
    }
    
    void wagTail() {
        std::cout << name << " is wagging tail happily." << std::endl;
    }
    
    void displayBreed() {
        std::cout << name << " is a " << breed << std::endl;
    }
};

int main() {
    // Create base class object
    Animal genericAnimal("Generic", 5);
    genericAnimal.eat();
    genericAnimal.sleep();
    genericAnimal.displayInfo();
    
    std::cout << std::endl;
    
    // Create derived class object
    Dog buddy("Buddy", 3, "Golden Retriever");
    
    // Can use inherited methods
    buddy.eat();           // From Animal
    buddy.sleep();         // From Animal
    buddy.displayInfo();   // From Animal
    
    // Can use own methods
    buddy.bark();          // From Dog
    buddy.wagTail();       // From Dog
    buddy.displayBreed();  // From Dog
    
    return 0;
}
```

## Access Levels in Inheritance

### Public Inheritance

Most common form - "is-a" relationship:

```cpp
#include <iostream>
#include <string>

class Vehicle {
protected:
    std::string brand;
    int year;
    double maxSpeed;
    
public:
    Vehicle(const std::string& vehicleBrand, int vehicleYear, double speed)
        : brand(vehicleBrand), year(vehicleYear), maxSpeed(speed) {}
    
    void start() {
        std::cout << brand << " is starting..." << std::endl;
    }
    
    void stop() {
        std::cout << brand << " has stopped." << std::endl;
    }
    
    void displaySpecs() {
        std::cout << "Brand: " << brand << ", Year: " << year 
                  << ", Max Speed: " << maxSpeed << " km/h" << std::endl;
    }
};

class Car : public Vehicle {  // Public inheritance
private:
    int numDoors;
    std::string fuelType;
    
public:
    Car(const std::string& carBrand, int carYear, double speed, int doors, const std::string& fuel)
        : Vehicle(carBrand, carYear, speed), numDoors(doors), fuelType(fuel) {}
    
    void honk() {
        std::cout << brand << " honks: Beep beep!" << std::endl;
    }
    
    void openTrunk() {
        std::cout << "Opening " << brand << "'s trunk." << std::endl;
    }
    
    void displayCarInfo() {
        displaySpecs();  // Can call public base method
        std::cout << "Doors: " << numDoors << ", Fuel: " << fuelType << std::endl;
    }
};

class Motorcycle : public Vehicle {  // Public inheritance
private:
    bool hasSidecar;
    
public:
    Motorcycle(const std::string& bikeBrand, int bikeYear, double speed, bool sidecar)
        : Vehicle(bikeBrand, bikeYear, speed), hasSidecar(sidecar) {}
    
    void wheelie() {
        std::cout << brand << " is doing a wheelie!" << std::endl;
    }
    
    void displayBikeInfo() {
        displaySpecs();
        std::cout << "Sidecar: " << (hasSidecar ? "Yes" : "No") << std::endl;
    }
};

int main() {
    Car sedan("Toyota", 2023, 180.0, 4, "Gasoline");
    Motorcycle sportsBike("Yamaha", 2024, 200.0, false);
    
    // Both can use Vehicle methods
    sedan.start();
    sedan.displayCarInfo();
    sedan.honk();
    sedan.stop();
    
    std::cout << std::endl;
    
    sportsBike.start();
    sportsBike.displayBikeInfo();
    sportsBike.wheelie();
    sportsBike.stop();
    
    return 0;
}
```

### Protected Inheritance

Less common - "implemented-in-terms-of" relationship:

```cpp
#include <iostream>
#include <vector>

class Stack : protected std::vector<int> {  // Protected inheritance
public:
    void push(int value) {
        push_back(value);  // Can use vector's push_back
        std::cout << "Pushed " << value << " onto stack" << std::endl;
    }
    
    void pop() {
        if (!empty()) {
            std::cout << "Popped " << back() << " from stack" << std::endl;
            pop_back();
        } else {
            std::cout << "Stack is empty!" << std::endl;
        }
    }
    
    int top() const {
        return empty() ? -1 : back();
    }
    
    bool isEmpty() const {
        return empty();
    }
    
    size_t getSize() const {
        return size();
    }
    
    void display() const {
        std::cout << "Stack contents (top to bottom): ";
        for (auto it = rbegin(); it != rend(); ++it) {
            std::cout << *it << " ";
        }
        std::cout << std::endl;
    }
};

int main() {
    Stack myStack;
    
    myStack.push(10);
    myStack.push(20);
    myStack.push(30);
    
    myStack.display();
    
    std::cout << "Top element: " << myStack.top() << std::endl;
    std::cout << "Stack size: " << myStack.getSize() << std::endl;
    
    myStack.pop();
    myStack.display();
    
    // myStack.push_back(40);  // ✘ Error: push_back is not accessible
    // Protected inheritance hides vector's interface
    
    return 0;
}
```

### Private Inheritance

Rare - "implemented-in-terms-of" with stricter access:

```cpp
#include <iostream>
#include <string>

class Timer {
private:
    int seconds;
    
public:
    Timer() : seconds(0) {}
    
    void tick() { seconds++; }
    int getTime() const { return seconds; }
    void reset() { seconds = 0; }
};

class Game : private Timer {  // Private inheritance
private:
    std::string playerName;
    int score;
    
public:
    Game(const std::string& name) : playerName(name), score(0) {}
    
    void startGame() {
        reset();  // Can use Timer's methods privately
        std::cout << "Game started for " << playerName << std::endl;
    }
    
    void updateGame() {
        tick();   // Use Timer's tick privately
        score += 10;
        std::cout << "Game updated. Time: " << getTime() << "s, Score: " << score << std::endl;
    }
    
    void endGame() {
        std::cout << "Game ended for " << playerName 
                  << ". Final time: " << getTime() << "s, Score: " << score << std::endl;
    }
};

int main() {
    Game tetris("Alice");
    
    tetris.startGame();
    tetris.updateGame();
    tetris.updateGame();
    tetris.updateGame();
    tetris.endGame();
    
    // tetris.tick();     // ✘ Error: tick() is not accessible
    // tetris.getTime();  // ✘ Error: getTime() is not accessible
    
    return 0;
}
```

## Multiple Inheritance

A class can inherit from multiple base classes:

```cpp
#include <iostream>
#include <string>

class Flyable {
protected:
    double maxAltitude;
    
public:
    Flyable(double altitude = 10000.0) : maxAltitude(altitude) {}
    
    virtual void fly() {
        std::cout << "Flying at maximum altitude: " << maxAltitude << " meters" << std::endl;
    }
    
    virtual void land() {
        std::cout << "Landing..." << std::endl;
    }
};

class Swimmable {
protected:
    double maxDepth;
    
public:
    Swimmable(double depth = 100.0) : maxDepth(depth) {}
    
    virtual void swim() {
        std::cout << "Swimming at maximum depth: " << maxDepth << " meters" << std::endl;
    }
    
    virtual void surface() {
        std::cout << "Surfacing..." << std::endl;
    }
};

class Duck : public Flyable, public Swimmable {
private:
    std::string name;
    
public:
    Duck(const std::string& duckName, double flyHeight = 1000.0, double swimDepth = 5.0)
        : name(duckName), Flyable(flyHeight), Swimmable(swimDepth) {
        std::cout << "Duck " << name << " created" << std::endl;
    }
    
    void quack() {
        std::cout << name << " says: Quack!" << std::endl;
    }
    
    // Override inherited methods
    void fly() override {
        std::cout << name << " is flying at " << maxAltitude << " meters" << std::endl;
    }
    
    void swim() override {
        std::cout << name << " is swimming at depth " << maxDepth << " meters" << std::endl;
    }
    
    void displayCapabilities() {
        std::cout << name << " can fly up to " << maxAltitude 
                  << "m and swim down to " << maxDepth << "m" << std::endl;
    }
};

class Submarine : public Swimmable {
private:
    std::string model;
    
public:
    Submarine(const std::string& subModel, double depth = 500.0)
        : model(subModel), Swimmable(depth) {
        std::cout << "Submarine " << model << " created" << std::endl;
    }
    
    void dive() {
        std::cout << model << " is diving..." << std::endl;
        swim();
    }
    
    void sonar() {
        std::cout << model << " using sonar..." << std::endl;
    }
};

int main() {
    Duck mallard("Mallard", 500.0, 10.0);
    Submarine nautilus("Nautilus", 1000.0);
    
    // Duck can do both
    mallard.quack();
    mallard.fly();
    mallard.land();
    mallard.swim();
    mallard.surface();
    mallard.displayCapabilities();
    
    std::cout << std::endl;
    
    // Submarine can only swim
    nautilus.dive();
    nautilus.sonar();
    nautilus.surface();
    
    return 0;
}
```

## Virtual Functions and Polymorphism

Virtual functions enable runtime polymorphism:

```cpp
#include <iostream>
#include <vector>
#include <memory>
#include <string>

class Shape {
protected:
    std::string color;
    
public:
    Shape(const std::string& shapeColor) : color(shapeColor) {}
    
    // Virtual destructor is important for proper cleanup
    virtual ~Shape() = default;
    
    // Pure virtual functions make this an abstract class
    virtual double getArea() const = 0;
    virtual double getPerimeter() const = 0;
    virtual void draw() const = 0;
    
    // Virtual function with default implementation
    virtual void displayInfo() const {
        std::cout << "Color: " << color << ", Area: " << getArea() 
                  << ", Perimeter: " << getPerimeter() << std::endl;
    }
    
    // Non-virtual function
    std::string getColor() const {
        return color;
    }
};

class Circle : public Shape {
private:
    double radius;
    
public:
    Circle(const std::string& color, double r) : Shape(color), radius(r) {}
    
    double getArea() const override {
        return 3.14159 * radius * radius;
    }
    
    double getPerimeter() const override {
        return 2 * 3.14159 * radius;
    }
    
    void draw() const override {
        std::cout << "Drawing a " << color << " circle with radius " << radius << std::endl;
    }
    
    double getRadius() const { return radius; }
};

class Rectangle : public Shape {
private:
    double width;
    double height;
    
public:
    Rectangle(const std::string& color, double w, double h) 
        : Shape(color), width(w), height(h) {}
    
    double getArea() const override {
        return width * height;
    }
    
    double getPerimeter() const override {
        return 2 * (width + height);
    }
    
    void draw() const override {
        std::cout << "Drawing a " << color << " rectangle " << width << "x" << height << std::endl;
    }
    
    // Override virtual function with different behavior
    void displayInfo() const override {
        Shape::displayInfo();  // Call base implementation
        std::cout << "Dimensions: " << width << " x " << height << std::endl;
    }
};

class Triangle : public Shape {
private:
    double side1, side2, side3;
    
public:
    Triangle(const std::string& color, double s1, double s2, double s3)
        : Shape(color), side1(s1), side2(s2), side3(s3) {}
    
    double getArea() const override {
        // Using Heron's formula
        double s = (side1 + side2 + side3) / 2;
        return std::sqrt(s * (s - side1) * (s - side2) * (s - side3));
    }
    
    double getPerimeter() const override {
        return side1 + side2 + side3;
    }
    
    void draw() const override {
        std::cout << "Drawing a " << color << " triangle with sides " 
                  << side1 << ", " << side2 << ", " << side3 << std::endl;
    }
};

// Function that works with any Shape
void processShape(const Shape& shape) {
    shape.draw();          // Polymorphic call
    shape.displayInfo();   // Polymorphic call
    std::cout << std::endl;
}

int main() {
    // Create different shapes
    Circle circle("red", 5.0);
    Rectangle rectangle("blue", 4.0, 6.0);
    Triangle triangle("green", 3.0, 4.0, 5.0);
    
    // Polymorphic behavior
    std::cout << "Processing individual shapes:" << std::endl;
    processShape(circle);
    processShape(rectangle);
    processShape(triangle);
    
    // Container of shapes using polymorphism
    std::vector<std::unique_ptr<Shape>> shapes;
    shapes.push_back(std::make_unique<Circle>("yellow", 3.0));
    shapes.push_back(std::make_unique<Rectangle>("purple", 2.0, 8.0));
    shapes.push_back(std::make_unique<Triangle>("orange", 6.0, 8.0, 10.0));
    
    std::cout << "Processing shapes from container:" << std::endl;
    double totalArea = 0;
    for (const auto& shape : shapes) {
        shape->draw();                    // Polymorphic call
        totalArea += shape->getArea();    // Polymorphic call
    }
    
    std::cout << "Total area of all shapes: " << totalArea << std::endl;
    
    return 0;
}
```

## Constructor and Destructor Call Order

Understanding the order of construction and destruction:

```cpp
#include <iostream>
#include <string>

class Base {
private:
    std::string name;
    
public:
    Base(const std::string& baseName) : name(baseName) {
        std::cout << "Base constructor called: " << name << std::endl;
    }
    
    virtual ~Base() {
        std::cout << "Base destructor called: " << name << std::endl;
    }
    
    std::string getName() const { return name; }
};

class Derived : public Base {
private:
    int value;
    
public:
    Derived(const std::string& derivedName, int val) : Base(derivedName), value(val) {
        std::cout << "Derived constructor called: " << getName() << ", value: " << value << std::endl;
    }
    
    ~Derived() {
        std::cout << "Derived destructor called: " << getName() << ", value: " << value << std::endl;
    }
    
    int getValue() const { return value; }
};

class GrandChild : public Derived {
private:
    double data;
    
public:
    GrandChild(const std::string& name, int val, double d) : Derived(name, val), data(d) {
        std::cout << "GrandChild constructor called: " << getName() 
                  << ", value: " << getValue() << ", data: " << data << std::endl;
    }
    
    ~GrandChild() {
        std::cout << "GrandChild destructor called: " << getName() 
                  << ", value: " << getValue() << ", data: " << data << std::endl;
    }
};

void demonstrateConstructorOrder() {
    std::cout << "Creating GrandChild object:" << std::endl;
    GrandChild obj("TestObject", 42, 3.14);
    std::cout << "\nObject created successfully!" << std::endl;
    std::cout << "\nDestroying object (leaving scope):" << std::endl;
}

int main() {
    demonstrateConstructorOrder();
    std::cout << "\nProgram ending..." << std::endl;
    
    return 0;
}
```

## Abstract Classes and Interfaces

Creating contracts for derived classes:

```cpp
#include <iostream>
#include <vector>
#include <memory>
#include <string>

// Abstract base class (interface)
class Drawable {
public:
    virtual ~Drawable() = default;
    virtual void draw() const = 0;          // Pure virtual
    virtual void move(int x, int y) = 0;    // Pure virtual
    virtual void resize(double factor) = 0;  // Pure virtual
};

// Another interface
class Colorable {
public:
    virtual ~Colorable() = default;
    virtual void setColor(const std::string& color) = 0;
    virtual std::string getColor() const = 0;
};

// Concrete class implementing multiple interfaces
class Button : public Drawable, public Colorable {
private:
    int x, y;
    int width, height;
    std::string color;
    std::string text;
    
public:
    Button(int posX, int posY, int w, int h, const std::string& btnText) 
        : x(posX), y(posY), width(w), height(h), color("gray"), text(btnText) {}
    
    // Implement Drawable interface
    void draw() const override {
        std::cout << "Drawing " << color << " button '" << text 
                  << "' at (" << x << "," << y << ") size " << width << "x" << height << std::endl;
    }
    
    void move(int newX, int newY) override {
        x = newX;
        y = newY;
        std::cout << "Button '" << text << "' moved to (" << x << "," << y << ")" << std::endl;
    }
    
    void resize(double factor) override {
        width = static_cast<int>(width * factor);
        height = static_cast<int>(height * factor);
        std::cout << "Button '" << text << "' resized to " << width << "x" << height << std::endl;
    }
    
    // Implement Colorable interface
    void setColor(const std::string& newColor) override {
        color = newColor;
        std::cout << "Button '" << text << "' color changed to " << color << std::endl;
    }
    
    std::string getColor() const override {
        return color;
    }
    
    void click() {
        std::cout << "Button '" << text << "' clicked!" << std::endl;
    }
};

class Circle : public Drawable, public Colorable {
private:
    int centerX, centerY;
    double radius;
    std::string color;
    
public:
    Circle(int x, int y, double r) : centerX(x), centerY(y), radius(r), color("black") {}
    
    void draw() const override {
        std::cout << "Drawing " << color << " circle at (" << centerX << "," << centerY 
                  << ") with radius " << radius << std::endl;
    }
    
    void move(int x, int y) override {
        centerX = x;
        centerY = y;
        std::cout << "Circle moved to (" << centerX << "," << centerY << ")" << std::endl;
    }
    
    void resize(double factor) override {
        radius *= factor;
        std::cout << "Circle resized to radius " << radius << std::endl;
    }
    
    void setColor(const std::string& newColor) override {
        color = newColor;
    }
    
    std::string getColor() const override {
        return color;
    }
};

// Function that works with any Drawable object
void renderObject(Drawable& obj) {
    obj.draw();
}

// Function that works with any Colorable object
void changeColor(Colorable& obj, const std::string& newColor) {
    obj.setColor(newColor);
    std::cout << "Object color changed to: " << obj.getColor() << std::endl;
}

int main() {
    Button okButton(10, 20, 80, 30, "OK");
    Button cancelButton(100, 20, 80, 30, "Cancel");
    Circle redCircle(50, 50, 25);
    
    // Use as Drawable objects
    std::vector<std::unique_ptr<Drawable>> drawables;
    drawables.push_back(std::make_unique<Button>(200, 100, 60, 25, "Submit"));
    drawables.push_back(std::make_unique<Circle>(150, 150, 30));
    
    std::cout << "Rendering all drawable objects:" << std::endl;
    for (auto& drawable : drawables) {
        drawable->draw();
        drawable->move(drawable->getColor() == "red" ? 200 : 250, 200);
        drawable->resize(1.2);
    }
    
    std::cout << "\nWorking with colorable objects:" << std::endl;
    changeColor(okButton, "green");
    changeColor(cancelButton, "red");
    changeColor(redCircle, "blue");
    
    std::cout << "\nFinal rendering:" << std::endl;
    renderObject(okButton);
    renderObject(cancelButton);
    renderObject(redCircle);
    
    return 0;
}
```

## Diamond Problem and Virtual Inheritance

Solving multiple inheritance ambiguity:

```cpp
#include <iostream>
#include <string>

class Animal {
protected:
    std::string name;
    
public:
    Animal(const std::string& animalName) : name(animalName) {
        std::cout << "Animal constructor: " << name << std::endl;
    }
    
    virtual ~Animal() {
        std::cout << "Animal destructor: " << name << std::endl;
    }
    
    virtual void speak() {
        std::cout << name << " makes a sound" << std::endl;
    }
    
    std::string getName() const { return name; }
};

// Without virtual inheritance - creates diamond problem
class Mammal : public virtual Animal {  // Virtual inheritance
public:
    Mammal(const std::string& name) : Animal(name) {
        std::cout << "Mammal constructor: " << name << std::endl;
    }
    
    ~Mammal() {
        std::cout << "Mammal destructor: " << name << std::endl;
    }
    
    void giveBirth() {
        std::cout << name << " gives birth to live young" << std::endl;
    }
};

class Bird : public virtual Animal {  // Virtual inheritance
public:
    Bird(const std::string& name) : Animal(name) {
        std::cout << "Bird constructor: " << name << std::endl;
    }
    
    ~Bird() {
        std::cout << "Bird destructor: " << name << std::endl;
    }
    
    void layEggs() {
        std::cout << name << " lays eggs" << std::endl;
    }
    
    void speak() override {
        std::cout << name << " chirps" << std::endl;
    }
};

// Bat inherits from both Mammal and Bird
class Bat : public Mammal, public Bird {
public:
    Bat(const std::string& name) : Animal(name), Mammal(name), Bird(name) {
        std::cout << "Bat constructor: " << name << std::endl;
    }
    
    ~Bat() {
        std::cout << "Bat destructor: " << name << std::endl;
    }
    
    void fly() {
        std::cout << name << " flies using wings" << std::endl;
    }
    
    void speak() override {
        std::cout << name << " makes ultrasonic sounds" << std::endl;
    }
    
    void useEcholocation() {
        std::cout << name << " uses echolocation to navigate" << std::endl;
    }
};

int main() {
    std::cout << "Creating a bat:" << std::endl;
    Bat vampire("Vampire");
    
    std::cout << "\nBat capabilities:" << std::endl;
    vampire.speak();           // Bat's implementation
    vampire.fly();             // Bat's method
    vampire.giveBirth();       // From Mammal
    vampire.useEcholocation(); // Bat's method
    
    // With virtual inheritance, there's only one Animal instance
    std::cout << "\nAnimal name: " << vampire.getName() << std::endl;
    
    std::cout << "\nDestroying bat:" << std::endl;
    
    return 0;
}
```

## Best Practices

### Prefer Composition over Inheritance

```cpp
#include <iostream>
#include <string>

// ✘ Inheritance for code reuse (not "is-a" relationship)
class Timer {
public:
    void start() { std::cout << "Timer started" << std::endl; }
    void stop() { std::cout << "Timer stopped" << std::endl; }
    int getElapsed() { return 0; }
};

class BadStopwatch : public Timer {  // Stopwatch "is-a" Timer? Not really.
public:
    void lap() { std::cout << "Lap recorded" << std::endl; }
};

// → Composition - Stopwatch "has-a" Timer
class GoodStopwatch {
private:
    Timer timer;  // Composition
    std::vector<int> lapTimes;
    
public:
    void start() { 
        timer.start(); 
        std::cout << "Stopwatch started" << std::endl;
    }
    
    void stop() { 
        timer.stop(); 
        std::cout << "Stopwatch stopped" << std::endl;
    }
    
    void lap() {
        int elapsed = timer.getElapsed();
        lapTimes.push_back(elapsed);
        std::cout << "Lap " << lapTimes.size() << " recorded: " << elapsed << "ms" << std::endl;
    }
    
    void displayLaps() {
        for (size_t i = 0; i < lapTimes.size(); ++i) {
            std::cout << "Lap " << (i + 1) << ": " << lapTimes[i] << "ms" << std::endl;
        }
    }
};
```

### Design for Inheritance

```cpp
#include <iostream>
#include <string>

class Document {
protected:
    std::string title;
    std::string content;
    
    // Protected helper methods for derived classes
    virtual void validateContent() {
        if (content.empty()) {
            throw std::invalid_argument("Content cannot be empty");
        }
    }
    
public:
    Document(const std::string& docTitle) : title(docTitle) {}
    
    // Virtual destructor for proper cleanup
    virtual ~Document() = default;
    
    // Template method pattern - defines algorithm, allows customization
    void save() {
        validateContent();
        performSave();
        onSaveComplete();
    }
    
    // Pure virtual - must be implemented by derived classes
    virtual void performSave() = 0;
    
    // Virtual with default implementation - can be overridden
    virtual void onSaveComplete() {
        std::cout << "Document '" << title << "' saved successfully" << std::endl;
    }
    
    // Non-virtual - consistent behavior across all documents
    std::string getTitle() const { return title; }
    
    void setContent(const std::string& newContent) {
        content = newContent;
    }
};

class PDFDocument : public Document {
public:
    PDFDocument(const std::string& title) : Document(title) {}
    
    void performSave() override {
        std::cout << "Saving PDF document: " << title << std::endl;
        std::cout << "Compressing content..." << std::endl;
        std::cout << "Writing to .pdf file" << std::endl;
    }
    
    void onSaveComplete() override {
        Document::onSaveComplete();  // Call base implementation
        std::cout << "PDF optimization complete" << std::endl;
    }
};

class HTMLDocument : public Document {
public:
    HTMLDocument(const std::string& title) : Document(title) {}
    
protected:
    void validateContent() override {
        Document::validateContent();  // Call base validation
        
        // Additional HTML-specific validation
        if (content.find("<html>") == std::string::npos) {
            std::cout << "Warning: No <html> tag found" << std::endl;
        }
    }
    
public:
    void performSave() override {
        std::cout << "Saving HTML document: " << title << std::endl;
        std::cout << "Validating HTML structure..." << std::endl;
        std::cout << "Writing to .html file" << std::endl;
    }
};
```

## Exercises

### Exercise 1: Employee Hierarchy

Create an inheritance hierarchy for employees:

- Base `Employee` class with name, ID, salary
- Derived classes: `Manager`, `Developer`, `Salesperson`
- Each with specific methods and salary calculations
- Use virtual functions for polymorphic behavior

### Exercise 2: Shape Library

Build a comprehensive shape library:

- Abstract `Shape` base class
- Concrete shapes: `Circle`, `Rectangle`, `Triangle`, `Polygon`
- Implement area, perimeter, and drawing methods
- Add transformations (translate, rotate, scale)

### Exercise 3: Vehicle System

Design a vehicle inheritance system:

- Base `Vehicle` class
- Intermediate classes: `LandVehicle`, `WaterVehicle`, `AirVehicle`
- Specific vehicles: `Car`, `Boat`, `Airplane`, `Amphibian`
- Use multiple inheritance where appropriate

### Exercise 4: Game Character System

Create a role-playing game character hierarchy:

- Base `Character` class with health, level, experience
- Classes: `Warrior`, `Mage`, `Archer`, `Paladin`
- Multiple inheritance for hybrid classes
- Virtual functions for combat and abilities

## Key Takeaways

<i class="fa-solid fa-arrow-right"></i> **Inheritance models "is-a" relationships** between classes  
<i class="fa-solid fa-arrow-right"></i> **Use public inheritance** for polymorphic behavior  
<i class="fa-solid fa-arrow-right"></i> **Virtual functions enable runtime polymorphism**  
<i class="fa-solid fa-arrow-right"></i> **Abstract classes define contracts** for derived classes  
<i class="fa-solid fa-arrow-right"></i> **Virtual destructors** are essential for proper cleanup  
<i class="fa-solid fa-arrow-right"></i> **Prefer composition over inheritance** when not modeling "is-a"  
<i class="fa-solid fa-arrow-right"></i> **Design base classes thoughtfully** with virtual functions  
<i class="fa-solid fa-arrow-right"></i> **Virtual inheritance solves** the diamond problem in multiple inheritance

---

**Next**: Explore dynamic behavior and runtime polymorphism // →  [Polymorphism](polymorphism.md)
