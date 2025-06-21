# Smart Pointers

Smart pointers are modern C++ objects that automatically manage memory, providing the benefits of dynamic allocation while eliminating common memory management errors. They implement RAII (Resource Acquisition Is Initialization) and make memory management safer and more convenient.

## Why Smart Pointers?

Traditional raw pointers have several problems that smart pointers solve:

```cpp
#include <iostream>
#include <memory>

// ✘ Problems with raw pointers
void rawPointerProblems() {
    int* ptr = new int(42);
    
    // Problem 1: Easy to forget delete
    // return;  // Memory leak!
    
    // Problem 2: Exception safety issues
    // if (condition) throw exception;  // Memory leak!
    
    // Problem 3: Double deletion
    delete ptr;
    // delete ptr;  // Undefined behavior!
    
    // Problem 4: Use after delete
    // std::cout << *ptr;  // Undefined behavior!
    
    // Problem 5: Unclear ownership
    // Who is responsible for deleting this pointer?
}

// → Smart pointers solve these problems
void smartPointerSolution() {
    std::unique_ptr<int> ptr = std::make_unique<int>(42);
    
    // → Automatic cleanup - no explicit delete needed
    // → Exception safe - destructor called during stack unwinding
    // → Cannot double-delete - destructor handles it
    // → Cannot use after destruction - object manages lifetime
    // → Clear ownership semantics
    
    std::cout << "Value: " << *ptr << std::endl;
    
    // Automatic cleanup when ptr goes out of scope
}

int main() {
    smartPointerSolution();
    return 0;
}
```

## `std::unique_ptr` - Unique Ownership

`std::unique_ptr` represents exclusive ownership of a resource:

### Basic Usage

```cpp
#include <iostream>
#include <memory>

class Resource {
private:
    std::string name;
    
public:
    Resource(const std::string& n) : name(n) {
        std::cout << "Resource " << name << " created" << std::endl;
    }
    
    ~Resource() {
        std::cout << "Resource " << name << " destroyed" << std::endl;
    }
    
    void doWork() {
        std::cout << "Resource " << name << " is working" << std::endl;
    }
    
    const std::string& getName() const { return name; }
};

int main() {
    std::cout << "=== unique_ptr basic usage ===" << std::endl;
    
    {
        // Create unique_ptr
        std::unique_ptr<Resource> ptr1 = std::make_unique<Resource>("A");
        
        // Use like a regular pointer
        ptr1->doWork();
        std::cout << "Name: " << ptr1->getName() << std::endl;
        std::cout << "Value through dereference: " << (*ptr1).getName() << std::endl;
        
        // Check if it contains an object
        if (ptr1) {
            std::cout << "ptr1 is not null" << std::endl;
        }
        
        // Get raw pointer (use carefully)
        Resource* rawPtr = ptr1.get();
        std::cout << "Raw pointer: " << rawPtr << std::endl;
        
    }  // Resource automatically destroyed here
    
    std::cout << "=== After scope ===" << std::endl;
    
    return 0;
}
```

### Move Semantics with unique_ptr

```cpp
#include <iostream>
#include <memory>
#include <vector>

std::unique_ptr<Resource> createResource(const std::string& name) {
    return std::make_unique<Resource>(name);
}

void useResource(std::unique_ptr<Resource> resource) {
    if (resource) {
        resource->doWork();
    }
    // resource is automatically destroyed when function ends
}

int main() {
    std::cout << "=== unique_ptr move semantics ===" << std::endl;
    
    // Create and return unique_ptr
    auto resource1 = createResource("Factory");
    
    // Move construction
    std::unique_ptr<Resource> resource2 = std::move(resource1);
    
    // resource1 is now null, resource2 owns the object
    std::cout << "resource1 is null: " << (resource1 == nullptr) << std::endl;
    std::cout << "resource2 is not null: " << (resource2 != nullptr) << std::endl;
    
    // Move to function (transfers ownership)
    useResource(std::move(resource2));
    
    // resource2 is now null
    std::cout << "resource2 is null: " << (resource2 == nullptr) << std::endl;
    
    // Store in container
    std::vector<std::unique_ptr<Resource>> resources;
    resources.push_back(std::make_unique<Resource>("Container1"));
    resources.push_back(std::make_unique<Resource>("Container2"));
    
    std::cout << "Resources in container: " << resources.size() << std::endl;
    
    return 0;
}
```

### Custom Deleters

```cpp
#include <iostream>
#include <memory>
#include <cstdio>

// Custom deleter for FILE*
struct FileDeleter {
    void operator()(FILE* file) {
        if (file) {
            std::cout << "Closing file" << std::endl;
            std::fclose(file);
        }
    }
};

// Custom deleter for arrays
struct ArrayDeleter {
    void operator()(int* ptr) {
        std::cout << "Deleting array" << std::endl;
        delete[] ptr;
    }
};

int main() {
    std::cout << "=== Custom deleters ===" << std::endl;
    
    // File with custom deleter
    {
        std::unique_ptr<FILE, FileDeleter> file(std::fopen("temp.txt", "w"));
        if (file) {
            std::fputs("Hello, World!", file.get());
        }
        // File automatically closed when going out of scope
    }
    
    // Array with custom deleter
    {
        std::unique_ptr<int[], ArrayDeleter> array(new int[10]);
        for (int i = 0; i < 10; i++) {
            array[i] = i;
        }
        // Array automatically deleted when going out of scope
    }
    
    // Lambda deleter
    {
        auto customDeleter = [](Resource* ptr) {
            std::cout << "Custom lambda deleter called" << std::endl;
            delete ptr;
        };
        
        std::unique_ptr<Resource, decltype(customDeleter)> ptr(
            new Resource("Lambda"), customDeleter);
        
        ptr->doWork();
    }
    
    return 0;
}
```

## `std::shared_ptr` - Shared Ownership

`std::shared_ptr` allows multiple pointers to share ownership of the same resource:

### Basic Shared Ownership

```cpp
#include <iostream>
#include <memory>

int main() {
    std::cout << "=== shared_ptr basic usage ===" << std::endl;
    
    std::shared_ptr<Resource> ptr1 = std::make_shared<Resource>("Shared");
    std::cout << "Reference count: " << ptr1.use_count() << std::endl;
    
    {
        std::shared_ptr<Resource> ptr2 = ptr1;  // Share ownership
        std::cout << "Reference count after sharing: " << ptr1.use_count() << std::endl;
        
        std::shared_ptr<Resource> ptr3 = ptr1;  // Another share
        std::cout << "Reference count with 3 owners: " << ptr1.use_count() << std::endl;
        
        ptr2->doWork();
        ptr3->doWork();
        
    }  // ptr2 and ptr3 destroyed, but resource still alive
    
    std::cout << "Reference count after scope: " << ptr1.use_count() << std::endl;
    ptr1->doWork();
    
    return 0;
}  // Resource finally destroyed when last shared_ptr is destroyed
```

### Shared Ownership Scenarios

```cpp
#include <iostream>
#include <memory>
#include <vector>

class Team {
private:
    std::string name;
    std::vector<std::shared_ptr<Resource>> members;
    
public:
    Team(const std::string& n) : name(n) {
        std::cout << "Team " << name << " created" << std::endl;
    }
    
    ~Team() {
        std::cout << "Team " << name << " destroyed" << std::endl;
    }
    
    void addMember(std::shared_ptr<Resource> member) {
        members.push_back(member);
        std::cout << "Added member to team " << name 
                  << " (ref count: " << member.use_count() << ")" << std::endl;
    }
    
    void workTogether() {
        std::cout << "Team " << name << " working together:" << std::endl;
        for (auto& member : members) {
            member->doWork();
        }
    }
};

int main() {
    std::cout << "=== Shared ownership scenario ===" << std::endl;
    
    // Create shared resources
    auto alice = std::make_shared<Resource>("Alice");
    auto bob = std::make_shared<Resource>("Bob");
    
    // Create teams that share members
    Team projectA("Project A");
    Team projectB("Project B");
    
    // Alice works on both projects
    projectA.addMember(alice);
    projectB.addMember(alice);
    
    // Bob only works on project A
    projectA.addMember(bob);
    
    std::cout << "\nAlice reference count: " << alice.use_count() << std::endl;
    std::cout << "Bob reference count: " << bob.use_count() << std::endl;
    
    projectA.workTogether();
    projectB.workTogether();
    
    return 0;
}  // All objects destroyed in proper order
```

### Converting Between Smart Pointers

```cpp
#include <iostream>
#include <memory>

int main() {
    std::cout << "=== Converting between smart pointers ===" << std::endl;
    
    // unique_ptr to shared_ptr
    std::unique_ptr<Resource> uniquePtr = std::make_unique<Resource>("Convert");
    std::shared_ptr<Resource> sharedPtr = std::move(uniquePtr);
    
    std::cout << "After move: unique_ptr is null: " << (uniquePtr == nullptr) << std::endl;
    std::cout << "shared_ptr count: " << sharedPtr.use_count() << std::endl;
    
    // Cannot convert shared_ptr to unique_ptr directly
    // std::unique_ptr<Resource> backToUnique = sharedPtr;  // ✘ Error
    
    // But can release from shared_ptr if you're the only owner
    if (sharedPtr.use_count() == 1) {
        Resource* rawPtr = sharedPtr.get();
        sharedPtr.reset();  // Release ownership
        std::unique_ptr<Resource> newUnique(rawPtr);
        std::cout << "Converted back to unique_ptr" << std::endl;
    }
    
    return 0;
}
```

## `std::weak_ptr` - Non-owning Observer

`std::weak_ptr` provides a non-owning "weak" reference to an object managed by `std::shared_ptr`:

### Breaking Circular References

```cpp
#include <iostream>
#include <memory>

class Child;

class Parent {
private:
    std::string name;
    std::vector<std::shared_ptr<Child>> children;
    
public:
    Parent(const std::string& n) : name(n) {
        std::cout << "Parent " << name << " created" << std::endl;
    }
    
    ~Parent() {
        std::cout << "Parent " << name << " destroyed" << std::endl;
    }
    
    void addChild(std::shared_ptr<Child> child) {
        children.push_back(child);
    }
    
    const std::string& getName() const { return name; }
};

class Child {
private:
    std::string name;
    std::weak_ptr<Parent> parent;  // → Use weak_ptr to break cycle
    
public:
    Child(const std::string& n) : name(n) {
        std::cout << "Child " << name << " created" << std::endl;
    }
    
    ~Child() {
        std::cout << "Child " << name << " destroyed" << std::endl;
    }
    
    void setParent(std::shared_ptr<Parent> p) {
        parent = p;
    }
    
    void visitParent() {
        // Convert weak_ptr to shared_ptr for safe access
        if (auto parentPtr = parent.lock()) {
            std::cout << "Child " << name << " visiting parent " 
                      << parentPtr->getName() << std::endl;
        } else {
            std::cout << "Child " << name << ": parent no longer exists" << std::endl;
        }
    }
    
    const std::string& getName() const { return name; }
};

int main() {
    std::cout << "=== weak_ptr breaking circular references ===" << std::endl;
    
    {
        auto parent = std::make_shared<Parent>("John");
        auto child1 = std::make_shared<Child>("Alice");
        auto child2 = std::make_shared<Child>("Bob");
        
        // Establish relationships
        parent->addChild(child1);
        parent->addChild(child2);
        child1->setParent(parent);
        child2->setParent(parent);
        
        child1->visitParent();
        child2->visitParent();
        
        std::cout << "Parent ref count: " << parent.use_count() << std::endl;  // 1
        
    }  // Parent and children properly destroyed
    
    std::cout << "=== Demonstrating expired weak_ptr ===" << std::endl;
    
    std::weak_ptr<Resource> weakPtr;
    
    {
        auto resource = std::make_shared<Resource>("Temporary");
        weakPtr = resource;
        
        std::cout << "weak_ptr expired: " << weakPtr.expired() << std::endl;
        
        if (auto locked = weakPtr.lock()) {
            locked->doWork();
        }
    }  // resource destroyed
    
    std::cout << "weak_ptr expired: " << weakPtr.expired() << std::endl;
    
    if (auto locked = weakPtr.lock()) {
        locked->doWork();
    } else {
        std::cout << "Cannot access expired resource" << std::endl;
    }
    
    return 0;
}
```

### Observer Pattern with weak_ptr

```cpp
#include <iostream>
#include <memory>
#include <vector>
#include <algorithm>

class Observer {
public:
    virtual ~Observer() = default;
    virtual void notify(const std::string& message) = 0;
    virtual std::string getName() const = 0;
};

class ConcreteObserver : public Observer {
private:
    std::string name;
    
public:
    ConcreteObserver(const std::string& n) : name(n) {
        std::cout << "Observer " << name << " created" << std::endl;
    }
    
    ~ConcreteObserver() {
        std::cout << "Observer " << name << " destroyed" << std::endl;
    }
    
    void notify(const std::string& message) override {
        std::cout << "Observer " << name << " received: " << message << std::endl;
    }
    
    std::string getName() const override { return name; }
};

class Subject {
private:
    std::vector<std::weak_ptr<Observer>> observers;
    
public:
    void addObserver(std::shared_ptr<Observer> observer) {
        observers.push_back(observer);
        std::cout << "Added observer: " << observer->getName() << std::endl;
    }
    
    void removeExpiredObservers() {
        auto it = std::remove_if(observers.begin(), observers.end(),
            [](const std::weak_ptr<Observer>& weak) {
                return weak.expired();
            });
        observers.erase(it, observers.end());
    }
    
    void notifyAll(const std::string& message) {
        std::cout << "Notifying all observers: " << message << std::endl;
        
        for (auto& weakObs : observers) {
            if (auto obs = weakObs.lock()) {
                obs->notify(message);
            }
        }
        
        // Clean up expired observers
        removeExpiredObservers();
    }
    
    size_t getObserverCount() const {
        return std::count_if(observers.begin(), observers.end(),
            [](const std::weak_ptr<Observer>& weak) {
                return !weak.expired();
            });
    }
};

int main() {
    std::cout << "=== Observer pattern with weak_ptr ===" << std::endl;
    
    Subject subject;
    
    {
        auto obs1 = std::make_shared<ConcreteObserver>("Observer1");
        auto obs2 = std::make_shared<ConcreteObserver>("Observer2");
        
        subject.addObserver(obs1);
        subject.addObserver(obs2);
        
        subject.notifyAll("First message");
        std::cout << "Active observers: " << subject.getObserverCount() << std::endl;
        
    }  // obs1 and obs2 destroyed
    
    subject.notifyAll("Second message");
    std::cout << "Active observers: " << subject.getObserverCount() << std::endl;
    
    return 0;
}
```

## Smart Pointer Best Practices

### Factory Functions and make_shared/make_unique

```cpp
#include <iostream>
#include <memory>

// → Preferred: Use make_shared
std::shared_ptr<Resource> createSharedResource(const std::string& name) {
    return std::make_shared<Resource>(name);  // Single allocation, exception safe
}

// ✘ Avoid: Direct new with shared_ptr constructor
std::shared_ptr<Resource> createSharedResourceBad(const std::string& name) {
    return std::shared_ptr<Resource>(new Resource(name));  // Two allocations
}

// → Preferred: Use make_unique
std::unique_ptr<Resource> createUniqueResource(const std::string& name) {
    return std::make_unique<Resource>(name);  // Exception safe
}

// Exception safety demonstration
void exceptionSafetyDemo() {
    try {
        // ✘ Potential memory leak if processResources throws
        // processResources(std::shared_ptr<Resource>(new Resource("A")),
        //                  std::shared_ptr<Resource>(new Resource("B")));
        
        // → Exception safe
        auto resA = std::make_shared<Resource>("A");
        auto resB = std::make_shared<Resource>("B");
        // processResources(resA, resB);
        
    } catch (...) {
        std::cout << "Exception caught, but no memory leaks!" << std::endl;
    }
}

int main() {
    exceptionSafetyDemo();
    return 0;
}
```

### Performance Considerations

```cpp
#include <iostream>
#include <memory>
#include <chrono>
#include <vector>

void performanceComparison() {
    const int COUNT = 1000000;
    
    // Raw pointer performance
    auto start = std::chrono::high_resolution_clock::now();
    {
        std::vector<int*> rawPtrs;
        for (int i = 0; i < COUNT; ++i) {
            rawPtrs.push_back(new int(i));
        }
        for (auto ptr : rawPtrs) {
            delete ptr;
        }
    }
    auto end = std::chrono::high_resolution_clock::now();
    auto rawTime = std::chrono::duration_cast<std::chrono::milliseconds>(end - start);
    
    // unique_ptr performance
    start = std::chrono::high_resolution_clock::now();
    {
        std::vector<std::unique_ptr<int>> uniquePtrs;
        for (int i = 0; i < COUNT; ++i) {
            uniquePtrs.push_back(std::make_unique<int>(i));
        }
        // Automatic cleanup
    }
    end = std::chrono::high_resolution_clock::now();
    auto uniqueTime = std::chrono::duration_cast<std::chrono::milliseconds>(end - start);
    
    // shared_ptr performance
    start = std::chrono::high_resolution_clock::now();
    {
        std::vector<std::shared_ptr<int>> sharedPtrs;
        for (int i = 0; i < COUNT; ++i) {
            sharedPtrs.push_back(std::make_shared<int>(i));
        }
        // Automatic cleanup
    }
    end = std::chrono::high_resolution_clock::now();
    auto sharedTime = std::chrono::duration_cast<std::chrono::milliseconds>(end - start);
    
    std::cout << "Performance comparison (" << COUNT << " allocations):" << std::endl;
    std::cout << "Raw pointers: " << rawTime.count() << "ms" << std::endl;
    std::cout << "unique_ptr: " << uniqueTime.count() << "ms" << std::endl;
    std::cout << "shared_ptr: " << sharedTime.count() << "ms" << std::endl;
}
```

### When to Use Which Smart Pointer

```cpp
#include <iostream>
#include <memory>

// → Use unique_ptr for exclusive ownership
class FileManager {
private:
    std::unique_ptr<Resource> configFile;
    
public:
    FileManager() : configFile(std::make_unique<Resource>("config.txt")) {}
    
    // Move-only semantics
    FileManager(FileManager&&) = default;
    FileManager& operator=(FileManager&&) = default;
    
    // Disable copying
    FileManager(const FileManager&) = delete;
    FileManager& operator=(const FileManager&) = delete;
};

// → Use shared_ptr for shared ownership
class Cache {
private:
    std::map<std::string, std::shared_ptr<Resource>> cachedResources;
    
public:
    std::shared_ptr<Resource> getResource(const std::string& name) {
        auto it = cachedResources.find(name);
        if (it != cachedResources.end()) {
            return it->second;  // Return shared ownership
        }
        
        auto resource = std::make_shared<Resource>(name);
        cachedResources[name] = resource;
        return resource;
    }
};

// → Use weak_ptr for observers and breaking cycles
class EventSystem {
private:
    std::vector<std::weak_ptr<Observer>> listeners;
    
public:
    void addListener(std::shared_ptr<Observer> listener) {
        listeners.push_back(listener);
    }
    
    void notifyListeners(const std::string& event) {
        for (auto& weak : listeners) {
            if (auto listener = weak.lock()) {
                listener->notify(event);
            }
        }
    }
};
```

## Common Smart Pointer Mistakes

### Mixing Raw and Smart Pointers

```cpp
#include <iostream>
#include <memory>

void badMixing() {
    Resource* raw = new Resource("Mixed");
    
    // ✘ Don't do this - double deletion possible
    std::unique_ptr<Resource> smart1(raw);
    std::unique_ptr<Resource> smart2(raw);  // Double deletion!
}

void goodPractice() {
    // → Start with smart pointer
    auto smart = std::make_unique<Resource>("Good");
    
    // → Get raw pointer only when needed for APIs
    Resource* raw = smart.get();
    
    // → Don't delete raw pointer - smart pointer owns it
    // delete raw;  // ✘ Don't do this!
}
```

### Circular References with shared_ptr

```cpp
#include <iostream>
#include <memory>

class BadNode {
public:
    std::shared_ptr<BadNode> next;
    std::shared_ptr<BadNode> prev;  // ✘ Creates circular reference
    
    ~BadNode() {
        std::cout << "BadNode destroyed" << std::endl;  // Never called!
    }
};

class GoodNode {
public:
    std::shared_ptr<GoodNode> next;
    std::weak_ptr<GoodNode> prev;   // → Use weak_ptr to break cycle
    
    ~GoodNode() {
        std::cout << "GoodNode destroyed" << std::endl;  // Properly called
    }
};

void demonstrateCircularReference() {
    std::cout << "=== Bad circular reference ===" << std::endl;
    {
        auto bad1 = std::make_shared<BadNode>();
        auto bad2 = std::make_shared<BadNode>();
        bad1->next = bad2;
        bad2->prev = bad1;  // Circular reference - memory leak!
    }  // Nodes not destroyed due to circular reference
    
    std::cout << "=== Good practice ===" << std::endl;
    {
        auto good1 = std::make_shared<GoodNode>();
        auto good2 = std::make_shared<GoodNode>();
        good1->next = good2;
        good2->prev = good1;  // weak_ptr breaks the cycle
    }  // Nodes properly destroyed
}
```

## Practical Examples

### RAII File Manager

```cpp
#include <iostream>
#include <memory>
#include <fstream>

class FileHandle {
private:
    std::unique_ptr<std::fstream> file;
    std::string filename;
    
public:
    FileHandle(const std::string& name, std::ios::openmode mode) 
        : filename(name) {
        file = std::make_unique<std::fstream>(name, mode);
        if (!file->is_open()) {
            throw std::runtime_error("Cannot open file: " + name);
        }
        std::cout << "File " << filename << " opened" << std::endl;
    }
    
    ~FileHandle() {
        if (file && file->is_open()) {
            file->close();
            std::cout << "File " << filename << " closed" << std::endl;
        }
    }
    
    // Move-only semantics
    FileHandle(FileHandle&&) = default;
    FileHandle& operator=(FileHandle&&) = default;
    FileHandle(const FileHandle&) = delete;
    FileHandle& operator=(const FileHandle&) = delete;
    
    std::fstream& get() { return *file; }
    
    template<typename T>
    FileHandle& write(const T& data) {
        *file << data;
        return *this;
    }
    
    template<typename T>
    FileHandle& read(T& data) {
        *file >> data;
        return *this;
    }
};

void demonstrateFileManager() {
    try {
        FileHandle file("test.txt", std::ios::out);
        file.write("Hello, ").write("World!").write("\n").write(42);
        
        // File automatically closed when going out of scope
    } catch (const std::exception& e) {
        std::cout << "Error: " << e.what() << std::endl;
    }
}
```

### Smart Pointer Factory

```cpp
#include <iostream>
#include <memory>
#include <map>
#include <functional>

class Widget {
protected:
    std::string type;
    
public:
    Widget(const std::string& t) : type(t) {}
    virtual ~Widget() = default;
    virtual void render() const = 0;
    const std::string& getType() const { return type; }
};

class Button : public Widget {
public:
    Button() : Widget("Button") {}
    void render() const override {
        std::cout << "Rendering Button" << std::endl;
    }
};

class TextBox : public Widget {
public:
    TextBox() : Widget("TextBox") {}
    void render() const override {
        std::cout << "Rendering TextBox" << std::endl;
    }
};

class WidgetFactory {
private:
    using CreatorFunc = std::function<std::unique_ptr<Widget>()>;
    std::map<std::string, CreatorFunc> creators;
    
public:
    WidgetFactory() {
        // Register widget creators
        creators["Button"] = []() { return std::make_unique<Button>(); };
        creators["TextBox"] = []() { return std::make_unique<TextBox>(); };
    }
    
    std::unique_ptr<Widget> create(const std::string& type) {
        auto it = creators.find(type);
        if (it != creators.end()) {
            return it->second();
        }
        return nullptr;
    }
    
    std::vector<std::string> getAvailableTypes() const {
        std::vector<std::string> types;
        for (const auto& pair : creators) {
            types.push_back(pair.first);
        }
        return types;
    }
};

void demonstrateFactory() {
    WidgetFactory factory;
    
    auto widgets = std::vector<std::unique_ptr<Widget>>{};
    widgets.push_back(factory.create("Button"));
    widgets.push_back(factory.create("TextBox"));
    widgets.push_back(factory.create("Button"));
    
    for (const auto& widget : widgets) {
        if (widget) {
            widget->render();
        }
    }
}
```

## Exercises

### Exercise 1: Smart Pointer Conversions

Practice converting between different smart pointer types:

- Convert unique_ptr to shared_ptr
- Use weak_ptr to safely observe shared_ptr
- Implement move-only and shared resource classes

### Exercise 2: Memory-Safe Data Structures

Implement data structures using smart pointers:

- Linked list with unique_ptr
- Binary tree with shared_ptr
- Graph with weak_ptr to avoid cycles

### Exercise 3: RAII Resource Manager

Create managers for different resources:

- Database connection manager
- Thread pool manager
- Graphics resource manager

### Exercise 4: Observer Pattern

Implement a complete observer system:

- Subject class with weak_ptr observers
- Automatic cleanup of expired observers
- Type-safe event notification system

## Key Takeaways

<i class="fa-solid fa-arrow-right"></i> **Use `std::unique_ptr` for exclusive ownership** and move semantics  
<i class="fa-solid fa-arrow-right"></i> **Use `std::shared_ptr` for shared ownership** when multiple objects need the same resource  
<i class="fa-solid fa-arrow-right"></i> **Use `std::weak_ptr` to break circular references** and as non-owning observers  
<i class="fa-solid fa-arrow-right"></i> **Prefer `make_unique` and `make_shared`** for exception safety and performance  
<i class="fa-solid fa-arrow-right"></i> **Smart pointers eliminate most memory management errors** automatically  
<i class="fa-solid fa-arrow-right"></i> **Follow RAII principles** for automatic resource management

---

**Congratulations!** You've completed the Memory Management section. Smart pointers represent modern C++'s approach to safe, automatic memory management. Next, let's explore data containers // →  [Containers and Arrays](../containers/README.md)
