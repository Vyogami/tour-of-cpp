# Other Standard Containers

Beyond arrays, vectors, and strings, the C++ Standard Library provides a rich collection of specialized containers. Each container is optimized for specific use cases, offering different performance characteristics and interfaces. Understanding when and how to use these containers is crucial for writing efficient C++ programs.

## Container Categories Overview

The STL containers can be categorized into several groups:

```cpp
#include <iostream>
#include <vector>
#include <list>
#include <deque>
#include <set>
#include <map>
#include <unordered_set>
#include <unordered_map>
#include <stack>
#include <queue>

void containerOverview() {
    // Sequential Containers - maintain element order
    std::vector<int> vec = {1, 2, 3, 4, 5};           // Dynamic array
    std::list<int> lst = {1, 2, 3, 4, 5};             // Doubly-linked list
    std::deque<int> deq = {1, 2, 3, 4, 5};            // Double-ended queue
    
    // Associative Containers - sorted, tree-based
    std::set<int> s = {5, 2, 8, 1, 9};                // Unique sorted elements
    std::map<std::string, int> m = {{"one", 1}, {"two", 2}};  // Key-value pairs
    
    // Unordered Containers - hash-based, faster lookup
    std::unordered_set<int> us = {5, 2, 8, 1, 9};     // Hash set
    std::unordered_map<std::string, int> um = {{"one", 1}, {"two", 2}};  // Hash map
    
    // Container Adapters - specialized interfaces
    std::stack<int> stk;                               // LIFO stack
    std::queue<int> que;                               // FIFO queue
    std::priority_queue<int> pq;                       // Priority queue (heap)
    
    std::cout << "All containers created successfully!" << std::endl;
}
```

## Sequential Containers

### `std::list` - Doubly-Linked List

Perfect for frequent insertions and deletions in the middle:

```cpp
#include <iostream>
#include <list>
#include <algorithm>

int main() {
    std::list<int> numbers = {1, 2, 3, 4, 5};
    
    std::cout << "Original list: ";
    for (const auto& num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
    
    // Insert at beginning and end
    numbers.push_front(0);      // Add at beginning
    numbers.push_back(6);       // Add at end
    
    std::cout << "After push_front(0) and push_back(6): ";
    for (const auto& num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
    
    // Insert in middle
    auto it = std::find(numbers.begin(), numbers.end(), 3);
    if (it != numbers.end()) {
        numbers.insert(it, 2.5);  // Insert before 3
    }
    
    std::cout << "After inserting 2.5 before 3: ";
    for (const auto& num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
    
    // Remove elements
    numbers.remove(2.5);        // Remove all occurrences of 2.5
    numbers.pop_front();        // Remove first element
    numbers.pop_back();         // Remove last element
    
    std::cout << "After removals: ";
    for (const auto& num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
    
    // List-specific operations
    std::list<int> other = {10, 11, 12};
    
    // Splice: move elements from one list to another
    auto pos = std::next(numbers.begin(), 2);  // Position after 2nd element
    numbers.splice(pos, other);  // Move all elements from 'other'
    
    std::cout << "After splicing: ";
    for (const auto& num : numbers) {
        std::cout << num << " ";
    }
    std::cout << " (other is now empty: " << other.empty() << ")" << std::endl;
    
    // Sort and unique
    numbers.push_back(2);  // Add duplicate
    numbers.push_back(2);
    
    std::cout << "Before sort: ";
    for (const auto& num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
    
    numbers.sort();         // Sort in ascending order
    numbers.unique();       // Remove consecutive duplicates
    
    std::cout << "After sort and unique: ";
    for (const auto& num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

### `std::deque` - Double-Ended Queue

Efficient insertion/deletion at both ends:

```cpp
#include <iostream>
#include <deque>

int main() {
    std::deque<int> dq;
    
    // Add elements at both ends
    dq.push_back(3);     // [3]
    dq.push_front(2);    // [2, 3]
    dq.push_back(4);     // [2, 3, 4]
    dq.push_front(1);    // [1, 2, 3, 4]
    
    std::cout << "Deque contents: ";
    for (const auto& element : dq) {
        std::cout << element << " ";
    }
    std::cout << std::endl;
    
    // Random access (like vector)
    std::cout << "Element at index 2: " << dq[2] << std::endl;
    std::cout << "Element at index 1: " << dq.at(1) << std::endl;
    
    // Access front and back
    std::cout << "Front: " << dq.front() << ", Back: " << dq.back() << std::endl;
    
    // Remove from both ends
    dq.pop_front();      // Remove first: [2, 3, 4]
    dq.pop_back();       // Remove last: [2, 3]
    
    std::cout << "After pop operations: ";
    for (const auto& element : dq) {
        std::cout << element << " ";
    }
    std::cout << std::endl;
    
    // Insert in middle (less efficient than list)
    auto it = dq.begin() + 1;
    dq.insert(it, 99);   // [2, 99, 3]
    
    std::cout << "After inserting 99: ";
    for (const auto& element : dq) {
        std::cout << element << " ";
    }
    std::cout << std::endl;
    
    // Deque characteristics
    std::cout << "Size: " << dq.size() << std::endl;
    std::cout << "Empty: " << std::boolalpha << dq.empty() << std::endl;
    
    return 0;
}
```

## Associative Containers (Tree-Based)

### `std::set` - Unique Sorted Elements

Automatically maintains unique, sorted elements:

```cpp
#include <iostream>
#include <set>
#include <string>

int main() {
    // Basic set operations
    std::set<int> numbers = {5, 2, 8, 2, 1, 9, 1};  // Duplicates ignored
    
    std::cout << "Set contents (automatically sorted): ";
    for (const auto& num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
    
    // Insert elements
    auto result = numbers.insert(3);
    if (result.second) {
        std::cout << "Successfully inserted 3" << std::endl;
    }
    
    result = numbers.insert(5);  // Already exists
    if (!result.second) {
        std::cout << "5 already exists in set" << std::endl;
    }
    
    // Search for elements
    auto it = numbers.find(8);
    if (it != numbers.end()) {
        std::cout << "Found 8 in the set" << std::endl;
    }
    
    // Count elements (always 0 or 1 for set)
    std::cout << "Count of 5: " << numbers.count(5) << std::endl;
    std::cout << "Count of 100: " << numbers.count(100) << std::endl;
    
    // Range operations
    auto lower = numbers.lower_bound(3);  // First element >= 3
    auto upper = numbers.upper_bound(7);  // First element > 7
    
    std::cout << "Elements in range [3, 7]: ";
    for (auto it = lower; it != upper; ++it) {
        std::cout << *it << " ";
    }
    std::cout << std::endl;
    
    // Remove elements
    numbers.erase(2);              // Remove by value
    numbers.erase(numbers.find(9)); // Remove by iterator
    
    std::cout << "After removals: ";
    for (const auto& num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
    
    // Custom comparison for strings (case-insensitive)
    auto caseInsensitiveCompare = [](const std::string& a, const std::string& b) {
        return std::lexicographical_compare(
            a.begin(), a.end(),
            b.begin(), b.end(),
            [](char c1, char c2) { return std::tolower(c1) < std::tolower(c2); }
        );
    };
    
    std::set<std::string, decltype(caseInsensitiveCompare)> words(caseInsensitiveCompare);
    words.insert("apple");
    words.insert("Banana");
    words.insert("cherry");
    words.insert("APPLE");  // Won't be inserted (case-insensitive duplicate)
    
    std::cout << "Case-insensitive string set: ";
    for (const auto& word : words) {
        std::cout << word << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

### `std::multiset` - Sorted Elements with Duplicates

```cpp
#include <iostream>
#include <set>

int main() {
    std::multiset<int> numbers = {5, 2, 8, 2, 1, 9, 1, 5, 5};
    
    std::cout << "Multiset contents: ";
    for (const auto& num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
    
    // Count duplicates
    std::cout << "Count of 5: " << numbers.count(5) << std::endl;
    std::cout << "Count of 2: " << numbers.count(2) << std::endl;
    
    // Find range of equal elements
    auto range = numbers.equal_range(5);
    std::cout << "All occurrences of 5: ";
    for (auto it = range.first; it != range.second; ++it) {
        std::cout << *it << " ";
    }
    std::cout << std::endl;
    
    // Remove one occurrence
    auto it = numbers.find(5);
    if (it != numbers.end()) {
        numbers.erase(it);  // Remove only one occurrence
    }
    
    std::cout << "After removing one 5: ";
    for (const auto& num : numbers) {
        std::cout << num << " ";
    }
    std::cout << " (count of 5: " << numbers.count(5) << ")" << std::endl;
    
    // Remove all occurrences
    numbers.erase(2);  // Remove all 2s
    
    std::cout << "After removing all 2s: ";
    for (const auto& num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

### `std::map` - Key-Value Pairs

Sorted key-value associations:

```cpp
#include <iostream>
#include <map>
#include <string>

int main() {
    // Create and initialize map
    std::map<std::string, int> ages = {
        {"Alice", 25},
        {"Bob", 30},
        {"Charlie", 22}
    };
    
    std::cout << "Initial ages:" << std::endl;
    for (const auto& pair : ages) {
        std::cout << pair.first << ": " << pair.second << std::endl;
    }
    
    // Insert elements
    ages["Diana"] = 28;                    // Using subscript operator
    ages.insert({"Eve", 26});             // Using insert
    ages.emplace("Frank", 32);            // Using emplace (construct in-place)
    
    // Access elements
    std::cout << "\nBob's age: " << ages["Bob"] << std::endl;
    std::cout << "Alice's age: " << ages.at("Alice") << std::endl;
    
    // Safe access with find
    auto it = ages.find("Grace");
    if (it != ages.end()) {
        std::cout << "Grace's age: " << it->second << std::endl;
    } else {
        std::cout << "Grace not found in map" << std::endl;
    }
    
    // Modify existing value
    ages["Bob"] = 31;  // Update Bob's age
    
    // Check if key exists
    if (ages.count("Charlie") > 0) {
        std::cout << "Charlie is in the map" << std::endl;
    }
    
    // Iterate and modify
    std::cout << "\nAll ages after updates:" << std::endl;
    for (auto& pair : ages) {  // Note: non-const reference to modify
        if (pair.second < 30) {
            pair.second += 1;  // Give everyone under 30 a birthday
        }
        std::cout << pair.first << ": " << pair.second << std::endl;
    }
    
    // Remove elements
    ages.erase("Eve");                     // Remove by key
    ages.erase(ages.find("Frank"));       // Remove by iterator
    
    std::cout << "\nAfter removing Eve and Frank:" << std::endl;
    for (const auto& pair : ages) {
        std::cout << pair.first << ": " << pair.second << std::endl;
    }
    
    // Map with custom key type
    struct Person {
        std::string name;
        int id;
        
        bool operator<(const Person& other) const {
            return id < other.id;  // Compare by ID
        }
    };
    
    std::map<Person, std::string> roles;
    roles[{"Alice", 101}] = "Manager";
    roles[{"Bob", 102}] = "Developer";
    roles[{"Charlie", 100}] = "Designer";
    
    std::cout << "\nEmployee roles (sorted by ID):" << std::endl;
    for (const auto& pair : roles) {
        std::cout << "ID " << pair.first.id << " (" << pair.first.name 
                  << "): " << pair.second << std::endl;
    }
    
    return 0;
}
```

### `std::multimap` - Multiple Values per Key

```cpp
#include <iostream>
#include <map>
#include <string>

int main() {
    std::multimap<std::string, std::string> contacts;
    
    // Add multiple phone numbers per person
    contacts.insert({"Alice", "123-456-7890"});
    contacts.insert({"Alice", "987-654-3210"});
    contacts.insert({"Bob", "555-123-4567"});
    contacts.insert({"Alice", "111-222-3333"});
    contacts.insert({"Charlie", "777-888-9999"});
    
    std::cout << "All contacts:" << std::endl;
    for (const auto& contact : contacts) {
        std::cout << contact.first << ": " << contact.second << std::endl;
    }
    
    // Find all phone numbers for a person
    std::cout << "\nAlice's phone numbers:" << std::endl;
    auto range = contacts.equal_range("Alice");
    for (auto it = range.first; it != range.second; ++it) {
        std::cout << "  " << it->second << std::endl;
    }
    
    // Count how many numbers each person has
    std::cout << "\nContact counts:" << std::endl;
    std::cout << "Alice: " << contacts.count("Alice") << " numbers" << std::endl;
    std::cout << "Bob: " << contacts.count("Bob") << " numbers" << std::endl;
    std::cout << "Charlie: " << contacts.count("Charlie") << " numbers" << std::endl;
    
    // Remove one specific entry
    auto it = contacts.find("Alice");
    if (it != contacts.end()) {
        contacts.erase(it);  // Remove first Alice entry
    }
    
    std::cout << "\nAfter removing one Alice entry:" << std::endl;
    std::cout << "Alice now has " << contacts.count("Alice") << " numbers" << std::endl;
    
    return 0;
}
```

## Unordered Containers (Hash-Based)

### `std::unordered_set` and `std::unordered_map`

Hash-based containers for fast average-case lookup:

```cpp
#include <iostream>
#include <unordered_set>
#include <unordered_map>
#include <string>
#include <chrono>

void performanceComparison() {
    const int SIZE = 100000;
    
    std::set<int> orderedSet;
    std::unordered_set<int> unorderedSet;
    
    // Insert elements
    auto start = std::chrono::high_resolution_clock::now();
    for (int i = 0; i < SIZE; i++) {
        orderedSet.insert(i);
    }
    auto orderedInsertTime = std::chrono::high_resolution_clock::now() - start;
    
    start = std::chrono::high_resolution_clock::now();
    for (int i = 0; i < SIZE; i++) {
        unorderedSet.insert(i);
    }
    auto unorderedInsertTime = std::chrono::high_resolution_clock::now() - start;
    
    // Search for elements
    start = std::chrono::high_resolution_clock::now();
    for (int i = 0; i < SIZE; i += 100) {
        orderedSet.find(i);
    }
    auto orderedSearchTime = std::chrono::high_resolution_clock::now() - start;
    
    start = std::chrono::high_resolution_clock::now();
    for (int i = 0; i < SIZE; i += 100) {
        unorderedSet.find(i);
    }
    auto unorderedSearchTime = std::chrono::high_resolution_clock::now() - start;
    
    std::cout << "Performance comparison (" << SIZE << " elements):" << std::endl;
    std::cout << "Insert - Ordered: " << std::chrono::duration_cast<std::chrono::milliseconds>(orderedInsertTime).count() << "ms" << std::endl;
    std::cout << "Insert - Unordered: " << std::chrono::duration_cast<std::chrono::milliseconds>(unorderedInsertTime).count() << "ms" << std::endl;
    std::cout << "Search - Ordered: " << std::chrono::duration_cast<std::chrono::microseconds>(orderedSearchTime).count() << "μs" << std::endl;
    std::cout << "Search - Unordered: " << std::chrono::duration_cast<std::chrono::microseconds>(unorderedSearchTime).count() << "μs" << std::endl;
}

int main() {
    // Basic unordered_set usage
    std::unordered_set<std::string> words = {"apple", "banana", "cherry"};
    
    words.insert("date");
    words.insert("apple");  // Duplicate, won't be added
    
    std::cout << "Unordered set contents (order not guaranteed): ";
    for (const auto& word : words) {
        std::cout << word << " ";
    }
    std::cout << std::endl;
    
    // Fast lookup
    if (words.find("banana") != words.end()) {
        std::cout << "Found 'banana'" << std::endl;
    }
    
    // Hash map example
    std::unordered_map<std::string, int> wordCount;
    std::string text = "the quick brown fox jumps over the lazy dog the fox is quick";
    
    // Count word frequencies
    std::stringstream ss(text);
    std::string word;
    while (ss >> word) {
        wordCount[word]++;
    }
    
    std::cout << "\nWord frequencies:" << std::endl;
    for (const auto& pair : wordCount) {
        std::cout << pair.first << ": " << pair.second << std::endl;
    }
    
    // Hash information
    std::cout << "\nHash table information:" << std::endl;
    std::cout << "Bucket count: " << wordCount.bucket_count() << std::endl;
    std::cout << "Load factor: " << wordCount.load_factor() << std::endl;
    std::cout << "Max load factor: " << wordCount.max_load_factor() << std::endl;
    
    performanceComparison();
    
    return 0;
}
```

### Custom Hash Functions

```cpp
#include <iostream>
#include <unordered_set>
#include <unordered_map>
#include <string>

struct Point {
    int x, y;
    
    bool operator==(const Point& other) const {
        return x == other.x && y == other.y;
    }
};

// Custom hash function for Point
struct PointHash {
    std::size_t operator()(const Point& p) const {
        // Combine hash values of x and y
        return std::hash<int>{}(p.x) ^ (std::hash<int>{}(p.y) << 1);
    }
};

int main() {
    // Using custom hash function
    std::unordered_set<Point, PointHash> points;
    
    points.insert({1, 2});
    points.insert({3, 4});
    points.insert({1, 2});  // Duplicate, won't be added
    
    std::cout << "Points in set: " << points.size() << std::endl;
    for (const auto& point : points) {
        std::cout << "(" << point.x << ", " << point.y << ")" << std::endl;
    }
    
    // Using lambda as hash function
    auto stringHash = [](const std::string& s) {
        std::size_t hash = 0;
        for (char c : s) {
            hash = hash * 31 + c;
        }
        return hash;
    };
    
    std::unordered_set<std::string, decltype(stringHash)> customStringSet(0, stringHash);
    customStringSet.insert("hello");
    customStringSet.insert("world");
    
    std::cout << "\nCustom string set size: " << customStringSet.size() << std::endl;
    
    return 0;
}
```

## Container Adapters

### `std::stack` - LIFO Container

```cpp
#include <iostream>
#include <stack>
#include <string>

bool isBalanced(const std::string& expression) {
    std::stack<char> brackets;
    
    for (char c : expression) {
        if (c == '(' || c == '[' || c == '{') {
            brackets.push(c);
        } else if (c == ')' || c == ']' || c == '}') {
            if (brackets.empty()) {
                return false;  // Closing bracket without opening
            }
            
            char top = brackets.top();
            brackets.pop();
            
            if ((c == ')' && top != '(') ||
                (c == ']' && top != '[') ||
                (c == '}' && top != '{')) {
                return false;  // Mismatched brackets
            }
        }
    }
    
    return brackets.empty();  // All brackets should be matched
}

int main() {
    std::stack<int> numbers;
    
    // Push elements
    for (int i = 1; i <= 5; i++) {
        numbers.push(i);
    }
    
    std::cout << "Stack size: " << numbers.size() << std::endl;
    std::cout << "Top element: " << numbers.top() << std::endl;
    
    // Pop and display elements
    std::cout << "Popping elements: ";
    while (!numbers.empty()) {
        std::cout << numbers.top() << " ";
        numbers.pop();
    }
    std::cout << std::endl;
    
    // Practical example: balanced parentheses checker
    std::vector<std::string> expressions = {
        "(())",
        "([{}])",
        "(()",
        "([)]",
        "{[()]}",
        "((()))"
    };
    
    std::cout << "\nBalanced parentheses check:" << std::endl;
    for (const auto& expr : expressions) {
        std::cout << expr << ": " << (isBalanced(expr) ? "Balanced" : "Not balanced") << std::endl;
    }
    
    return 0;
}
```

### `std::queue` - FIFO Container

```cpp
#include <iostream>
#include <queue>
#include <string>

class PrintJob {
public:
    std::string document;
    int pages;
    int priority;
    
    PrintJob(const std::string& doc, int p, int prio = 1) 
        : document(doc), pages(p), priority(prio) {}
    
    void print() const {
        std::cout << "Printing: " << document << " (" << pages << " pages, priority " << priority << ")" << std::endl;
    }
};

int main() {
    std::queue<int> numbers;
    
    // Add elements (enqueue)
    for (int i = 1; i <= 5; i++) {
        numbers.push(i);
    }
    
    std::cout << "Queue size: " << numbers.size() << std::endl;
    std::cout << "Front element: " << numbers.front() << std::endl;
    std::cout << "Back element: " << numbers.back() << std::endl;
    
    // Remove elements (dequeue)
    std::cout << "Processing queue: ";
    while (!numbers.empty()) {
        std::cout << numbers.front() << " ";
        numbers.pop();
    }
    std::cout << std::endl;
    
    // Practical example: print queue
    std::queue<PrintJob> printQueue;
    
    printQueue.push(PrintJob("Report.pdf", 10));
    printQueue.push(PrintJob("Presentation.pptx", 25));
    printQueue.push(PrintJob("Letter.docx", 2));
    
    std::cout << "\nProcessing print queue:" << std::endl;
    while (!printQueue.empty()) {
        printQueue.front().print();
        printQueue.pop();
    }
    
    return 0;
}
```

### `std::priority_queue` - Heap-Based Priority Queue

```cpp
#include <iostream>
#include <queue>
#include <vector>
#include <functional>

struct Task {
    std::string name;
    int priority;
    
    Task(const std::string& n, int p) : name(n), priority(p) {}
    
    // For max-heap (higher priority first)
    bool operator<(const Task& other) const {
        return priority < other.priority;  // Note: reversed for max-heap
    }
};

int main() {
    // Default priority queue (max-heap)
    std::priority_queue<int> maxHeap;
    
    maxHeap.push(3);
    maxHeap.push(1);
    maxHeap.push(4);
    maxHeap.push(1);
    maxHeap.push(5);
    
    std::cout << "Max heap (largest first): ";
    while (!maxHeap.empty()) {
        std::cout << maxHeap.top() << " ";
        maxHeap.pop();
    }
    std::cout << std::endl;
    
    // Min heap using greater comparator
    std::priority_queue<int, std::vector<int>, std::greater<int>> minHeap;
    
    minHeap.push(3);
    minHeap.push(1);
    minHeap.push(4);
    minHeap.push(1);
    minHeap.push(5);
    
    std::cout << "Min heap (smallest first): ";
    while (!minHeap.empty()) {
        std::cout << minHeap.top() << " ";
        minHeap.pop();
    }
    std::cout << std::endl;
    
    // Priority queue with custom objects
    std::priority_queue<Task> taskQueue;
    
    taskQueue.push(Task("Low priority task", 1));
    taskQueue.push(Task("High priority task", 5));
    taskQueue.push(Task("Medium priority task", 3));
    taskQueue.push(Task("Urgent task", 10));
    
    std::cout << "\nTask queue (by priority):" << std::endl;
    while (!taskQueue.empty()) {
        const Task& task = taskQueue.top();
        std::cout << "Priority " << task.priority << ": " << task.name << std::endl;
        taskQueue.pop();
    }
    
    // Lambda comparator for custom ordering
    auto customCompare = [](const std::pair<std::string, int>& a, const std::pair<std::string, int>& b) {
        return a.second > b.second;  // Min heap by second element
    };
    
    std::priority_queue<std::pair<std::string, int>, 
                       std::vector<std::pair<std::string, int>>, 
                       decltype(customCompare)> customQueue(customCompare);
    
    customQueue.push({"Alice", 85});
    customQueue.push({"Bob", 92});
    customQueue.push({"Charlie", 78});
    
    std::cout << "\nStudent scores (lowest first):" << std::endl;
    while (!customQueue.empty()) {
        auto student = customQueue.top();
        std::cout << student.first << ": " << student.second << std::endl;
        customQueue.pop();
    }
    
    return 0;
}
```

## Choosing the Right Container

### Performance Characteristics Comparison

```cpp
#include <iostream>
#include <vector>
#include <list>
#include <deque>
#include <set>
#include <unordered_set>
#include <chrono>

template<typename Container>
void measureInsertion(Container& container, const std::string& name, int count) {
    auto start = std::chrono::high_resolution_clock::now();
    
    for (int i = 0; i < count; i++) {
        container.insert(container.end(), i);
    }
    
    auto end = std::chrono::high_resolution_clock::now();
    auto duration = std::chrono::duration_cast<std::chrono::microseconds>(end - start);
    
    std::cout << name << " insertion (" << count << " elements): " 
              << duration.count() << " microseconds" << std::endl;
}

void containerChoiceGuide() {
    std::cout << "=== Container Choice Guide ===" << std::endl;
    
    std::cout << "\nUse std::vector when:" << std::endl;
    std::cout << "  ✓ You need random access to elements" << std::endl;
    std::cout << "  ✓ You mostly add/remove at the end" << std::endl;
    std::cout << "  ✓ You want cache-friendly iteration" << std::endl;
    std::cout << "  ✓ You need the best general-purpose container" << std::endl;
    
    std::cout << "\nUse std::list when:" << std::endl;
    std::cout << "  ✓ You frequently insert/remove in the middle" << std::endl;
    std::cout << "  ✓ You don't need random access" << std::endl;
    std::cout << "  ✓ Element addresses must remain stable" << std::endl;
    
    std::cout << "\nUse std::deque when:" << std::endl;
    std::cout << "  ✓ You need efficient insertion at both ends" << std::endl;
    std::cout << "  ✓ You need random access" << std::endl;
    std::cout << "  ✓ Implementing a queue or double-ended queue" << std::endl;
    
    std::cout << "\nUse std::set when:" << std::endl;
    std::cout << "  ✓ You need unique, automatically sorted elements" << std::endl;
    std::cout << "  ✓ You need fast lookup and range queries" << std::endl;
    std::cout << "  ✓ Order matters" << std::endl;
    
    std::cout << "\nUse std::unordered_set when:" << std::endl;
    std::cout << "  ✓ You need unique elements with fastest lookup" << std::endl;
    std::cout << "  ✓ Order doesn't matter" << std::endl;
    std::cout << "  ✓ You have a good hash function" << std::endl;
    
    std::cout << "\nUse std::map when:" << std::endl;
    std::cout << "  ✓ You need key-value associations" << std::endl;
    std::cout << "  ✓ You need sorted keys" << std::endl;
    std::cout << "  ✓ You need range queries on keys" << std::endl;
    
    std::cout << "\nUse std::unordered_map when:" << std::endl;
    std::cout << "  ✓ You need fastest key-value lookup" << std::endl;
    std::cout << "  ✓ Key order doesn't matter" << std::endl;
    std::cout << "  ✓ You have a good hash function for keys" << std::endl;
}

int main() {
    containerChoiceGuide();
    
    // Performance comparison for insertions
    const int COUNT = 10000;
    
    std::cout << "\n=== Performance Comparison ===" << std::endl;
    
    std::vector<int> vec;
    std::list<int> lst;
    std::deque<int> deq;
    std::set<int> st;
    std::unordered_set<int> ust;
    
    vec.reserve(COUNT);  // Fair comparison for vector
    
    measureInsertion(vec, "vector (at end)", COUNT);
    measureInsertion(lst, "list (at end)", COUNT);
    measureInsertion(deq, "deque (at end)", COUNT);
    
    // For set and unordered_set, we'll use their insert method
    auto start = std::chrono::high_resolution_clock::now();
    for (int i = 0; i < COUNT; i++) {
        st.insert(i);
    }
    auto end = std::chrono::high_resolution_clock::now();
    auto duration = std::chrono::duration_cast<std::chrono::microseconds>(end - start);
    std::cout << "set insertion (" << COUNT << " elements): " 
              << duration.count() << " microseconds" << std::endl;
    
    start = std::chrono::high_resolution_clock::now();
    for (int i = 0; i < COUNT; i++) {
        ust.insert(i);
    }
    end = std::chrono::high_resolution_clock::now();
    duration = std::chrono::duration_cast<std::chrono::microseconds>(end - start);
    std::cout << "unordered_set insertion (" << COUNT << " elements): " 
              << duration.count() << " microseconds" << std::endl;
    
    return 0;
}
```

## Practical Container Applications

### Word Frequency Counter

```cpp
#include <iostream>
#include <map>
#include <unordered_map>
#include <string>
#include <sstream>
#include <vector>
#include <algorithm>

class WordFrequencyAnalyzer {
private:
    std::unordered_map<std::string, int> frequencies;
    
    std::string cleanWord(const std::string& word) {
        std::string cleaned;
        for (char c : word) {
            if (std::isalnum(c)) {
                cleaned += std::tolower(c);
            }
        }
        return cleaned;
    }
    
public:
    void addText(const std::string& text) {
        std::istringstream iss(text);
        std::string word;
        
        while (iss >> word) {
            std::string cleanedWord = cleanWord(word);
            if (!cleanedWord.empty()) {
                frequencies[cleanedWord]++;
            }
        }
    }
    
    void printTopWords(int count) const {
        // Convert to vector for sorting
        std::vector<std::pair<std::string, int>> wordPairs(frequencies.begin(), frequencies.end());
        
        // Sort by frequency (descending)
        std::sort(wordPairs.begin(), wordPairs.end(),
                  [](const auto& a, const auto& b) {
                      return a.second > b.second;
                  });
        
        std::cout << "Top " << std::min(count, static_cast<int>(wordPairs.size())) << " words:" << std::endl;
        for (int i = 0; i < std::min(count, static_cast<int>(wordPairs.size())); i++) {
            std::cout << wordPairs[i].first << ": " << wordPairs[i].second << std::endl;
        }
    }
    
    int getWordCount(const std::string& word) const {
        std::string cleanedWord = cleanWord(word);
        auto it = frequencies.find(cleanedWord);
        return (it != frequencies.end()) ? it->second : 0;
    }
    
    size_t getTotalUniqueWords() const {
        return frequencies.size();
    }
    
    int getTotalWords() const {
        int total = 0;
        for (const auto& pair : frequencies) {
            total += pair.second;
        }
        return total;
    }
};

int main() {
    WordFrequencyAnalyzer analyzer;
    
    std::string text = R"(
        The quick brown fox jumps over the lazy dog.
        The dog was sleeping under the tree.
        Quick movements of the fox woke the dog up.
        The brown fox and the lazy dog became friends.
        Now the fox and the dog play together every day.
    )";
    
    analyzer.addText(text);
    
    std::cout << "Text analysis results:" << std::endl;
    std::cout << "Total words: " << analyzer.getTotalWords() << std::endl;
    std::cout << "Unique words: " << analyzer.getTotalUniqueWords() << std::endl;
    
    std::cout << std::endl;
    analyzer.printTopWords(10);
    
    std::cout << "\nSpecific word counts:" << std::endl;
    std::cout << "'the': " << analyzer.getWordCount("the") << std::endl;
    std::cout << "'fox': " << analyzer.getWordCount("fox") << std::endl;
    std::cout << "'dog': " << analyzer.getWordCount("dog") << std::endl;
    
    return 0;
}
```

### Graph Representation Using Containers

```cpp
#include <iostream>
#include <vector>
#include <unordered_map>
#include <unordered_set>
#include <queue>
#include <string>

class Graph {
private:
    std::unordered_map<std::string, std::unordered_set<std::string>> adjacencyList;
    
public:
    void addVertex(const std::string& vertex) {
        adjacencyList[vertex];  // Creates empty set if doesn't exist
    }
    
    void addEdge(const std::string& from, const std::string& to) {
        adjacencyList[from].insert(to);
        adjacencyList[to].insert(from);  // Undirected graph
    }
    
    void removeEdge(const std::string& from, const std::string& to) {
        adjacencyList[from].erase(to);
        adjacencyList[to].erase(from);
    }
    
    bool hasEdge(const std::string& from, const std::string& to) const {
        auto it = adjacencyList.find(from);
        if (it != adjacencyList.end()) {
            return it->second.count(to) > 0;
        }
        return false;
    }
    
    std::vector<std::string> getNeighbors(const std::string& vertex) const {
        std::vector<std::string> neighbors;
        auto it = adjacencyList.find(vertex);
        if (it != adjacencyList.end()) {
            neighbors.assign(it->second.begin(), it->second.end());
        }
        return neighbors;
    }
    
    void printGraph() const {
        std::cout << "Graph structure:" << std::endl;
        for (const auto& vertex : adjacencyList) {
            std::cout << vertex.first << " -> ";
            for (const auto& neighbor : vertex.second) {
                std::cout << neighbor << " ";
            }
            std::cout << std::endl;
        }
    }
    
    std::vector<std::string> breadthFirstSearch(const std::string& start) const {
        std::vector<std::string> result;
        std::unordered_set<std::string> visited;
        std::queue<std::string> toVisit;
        
        toVisit.push(start);
        visited.insert(start);
        
        while (!toVisit.empty()) {
            std::string current = toVisit.front();
            toVisit.pop();
            result.push_back(current);
            
            auto it = adjacencyList.find(current);
            if (it != adjacencyList.end()) {
                for (const std::string& neighbor : it->second) {
                    if (visited.find(neighbor) == visited.end()) {
                        visited.insert(neighbor);
                        toVisit.push(neighbor);
                    }
                }
            }
        }
        
        return result;
    }
};

int main() {
    Graph socialNetwork;
    
    // Add people (vertices)
    socialNetwork.addVertex("Alice");
    socialNetwork.addVertex("Bob");
    socialNetwork.addVertex("Charlie");
    socialNetwork.addVertex("Diana");
    socialNetwork.addVertex("Eve");
    
    // Add friendships (edges)
    socialNetwork.addEdge("Alice", "Bob");
    socialNetwork.addEdge("Alice", "Charlie");
    socialNetwork.addEdge("Bob", "Diana");
    socialNetwork.addEdge("Charlie", "Diana");
    socialNetwork.addEdge("Diana", "Eve");
    
    socialNetwork.printGraph();
    
    std::cout << "\nAlice's friends: ";
    auto aliceFriends = socialNetwork.getNeighbors("Alice");
    for (const auto& friend_name : aliceFriends) {
        std::cout << friend_name << " ";
    }
    std::cout << std::endl;
    
    std::cout << "\nBFS from Alice: ";
    auto bfsResult = socialNetwork.breadthFirstSearch("Alice");
    for (const auto& person : bfsResult) {
        std::cout << person << " ";
    }
    std::cout << std::endl;
    
    std::cout << "\nAre Alice and Eve connected? " 
              << (socialNetwork.hasEdge("Alice", "Eve") ? "Yes" : "No") << std::endl;
    std::cout << "Are Alice and Bob connected? " 
              << (socialNetwork.hasEdge("Alice", "Bob") ? "Yes" : "No") << std::endl;
    
    return 0;
}
```

## Advanced Container Techniques

### Container Composition

```cpp
#include <iostream>
#include <vector>
#include <map>
#include <set>
#include <string>

class StudentDatabase {
private:
    // Primary storage: student ID -> student info
    std::map<int, std::string> students;
    
    // Secondary indices for fast lookup
    std::map<std::string, std::set<int>> studentsByName;  // Name -> set of IDs
    std::map<char, std::set<int>> studentsByGrade;        // Grade -> set of IDs
    
    // Course enrollment
    std::map<int, std::set<std::string>> studentCourses;  // Student ID -> courses
    std::map<std::string, std::set<int>> courseStudents;  // Course -> student IDs
    
public:
    void addStudent(int id, const std::string& name, char grade) {
        students[id] = name;
        studentsByName[name].insert(id);
        studentsByGrade[grade].insert(id);
    }
    
    void enrollStudent(int studentId, const std::string& course) {
        if (students.find(studentId) != students.end()) {
            studentCourses[studentId].insert(course);
            courseStudents[course].insert(studentId);
        }
    }
    
    std::vector<int> getStudentsByName(const std::string& name) const {
        std::vector<int> result;
        auto it = studentsByName.find(name);
        if (it != studentsByName.end()) {
            result.assign(it->second.begin(), it->second.end());
        }
        return result;
    }
    
    std::vector<int> getStudentsByGrade(char grade) const {
        std::vector<int> result;
        auto it = studentsByGrade.find(grade);
        if (it != studentsByGrade.end()) {
            result.assign(it->second.begin(), it->second.end());
        }
        return result;
    }
    
    std::vector<std::string> getStudentCourses(int studentId) const {
        std::vector<std::string> result;
        auto it = studentCourses.find(studentId);
        if (it != studentCourses.end()) {
            result.assign(it->second.begin(), it->second.end());
        }
        return result;
    }
    
    std::vector<int> getCourseStudents(const std::string& course) const {
        std::vector<int> result;
        auto it = courseStudents.find(course);
        if (it != courseStudents.end()) {
            result.assign(it->second.begin(), it->second.end());
        }
        return result;
    }
    
    void printDatabase() const {
        std::cout << "Student Database:" << std::endl;
        for (const auto& student : students) {
            std::cout << "ID " << student.first << ": " << student.second << std::endl;
            
            auto courses = getStudentCourses(student.first);
            if (!courses.empty()) {
                std::cout << "  Courses: ";
                for (const auto& course : courses) {
                    std::cout << course << " ";
                }
                std::cout << std::endl;
            }
        }
    }
};

int main() {
    StudentDatabase db;
    
    // Add students
    db.addStudent(101, "Alice Johnson", 'A');
    db.addStudent(102, "Bob Smith", 'B');
    db.addStudent(103, "Alice Brown", 'A');
    db.addStudent(104, "Charlie Davis", 'C');
    
    // Enroll students in courses
    db.enrollStudent(101, "Math");
    db.enrollStudent(101, "Physics");
    db.enrollStudent(102, "Math");
    db.enrollStudent(102, "Chemistry");
    db.enrollStudent(103, "Physics");
    db.enrollStudent(103, "Chemistry");
    db.enrollStudent(104, "Math");
    
    db.printDatabase();
    
    // Query by name
    auto aliceStudents = db.getStudentsByName("Alice Brown");
    std::cout << "\nStudents named 'Alice Brown': ";
    for (int id : aliceStudents) {
        std::cout << id << " ";
    }
    std::cout << std::endl;
    
    // Query by grade
    auto gradeAStudents = db.getStudentsByGrade('A');
    std::cout << "Grade A students: ";
    for (int id : gradeAStudents) {
        std::cout << id << " ";
    }
    std::cout << std::endl;
    
    // Query course enrollment
    auto mathStudents = db.getCourseStudents("Math");
    std::cout << "Students in Math: ";
    for (int id : mathStudents) {
        std::cout << id << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

## Exercises

### Exercise 1: Container Performance Analysis

Compare performance of different containers for various operations:

- Insertion at different positions
- Search operations
- Iteration speed
- Memory usage patterns

### Exercise 2: Data Structure Implementation

Implement these data structures using STL containers:

- LRU (Least Recently Used) cache
- Trie (prefix tree) for string storage
- Disjoint set (union-find) data structure
- Simple database with multiple indices

### Exercise 3: Algorithm Implementation

Use containers to implement classic algorithms:

- Dijkstra's shortest path (using priority_queue)
- Topological sorting (using queue and map)
- Expression evaluation (using stack)
- Sliding window maximum (using deque)

### Exercise 4: Real-World Applications

Build practical applications using containers:

- Task scheduler with priorities
- Simple caching system
- Log file analyzer
- Configuration manager with nested settings

## Key Takeaways

<i class="fa-solid fa-arrow-right"></i> **Choose containers based on your access patterns** and performance requirements  
<i class="fa-solid fa-arrow-right"></i> **Associative containers provide sorted data** with O(log n) operations  
<i class="fa-solid fa-arrow-right"></i> **Unordered containers offer average O(1) lookup** but no ordering guarantees  
<i class="fa-solid fa-arrow-right"></i> **Container adapters provide specialized interfaces** for common patterns  
<i class="fa-solid fa-arrow-right"></i> **Consider combining multiple containers** for complex data relationships  
<i class="fa-solid fa-arrow-right"></i> **Hash-based containers require good hash functions** for optimal performance

---

**Congratulations!** You've completed the Containers and Arrays section. These containers form the foundation of most C++ programs and algorithms. Next, let's explore object-oriented programming // →  [Object-Oriented Programming](../oop/README.md)
