# Strings and Text Processing

`std::string` is a specialized container for text data, providing a rich set of operations for manipulating character sequences. Understanding strings is essential for text processing, user interfaces, file I/O, and many other programming tasks.

## Introduction to `std::string`

`std::string` is actually a typedef for `std::basic_string<char>`, a template class that can work with different character types:

```cpp
#include <iostream>
#include <string>

int main() {
    // Different ways to create strings
    std::string empty;                          // Empty string
    std::string greeting = "Hello, World!";    // From string literal
    std::string copy(greeting);                 // Copy constructor
    std::string substr(greeting, 0, 5);        // Substring: "Hello"
    std::string repeated(10, 'A');             // 10 'A' characters: "AAAAAAAAAA"
    std::string fromCString = std::string("C-style string");
    
    std::cout << "Empty: '" << empty << "'" << std::endl;
    std::cout << "Greeting: '" << greeting << "'" << std::endl;
    std::cout << "Copy: '" << copy << "'" << std::endl;
    std::cout << "Substring: '" << substr << "'" << std::endl;
    std::cout << "Repeated: '" << repeated << "'" << std::endl;
    std::cout << "From C-string: '" << fromCString << "'" << std::endl;
    
    // String properties
    std::cout << "\nString properties:" << std::endl;
    std::cout << "Length: " << greeting.length() << std::endl;
    std::cout << "Size: " << greeting.size() << std::endl;        // Same as length()
    std::cout << "Capacity: " << greeting.capacity() << std::endl;
    std::cout << "Empty?: " << std::boolalpha << greeting.empty() << std::endl;
    std::cout << "Max size: " << greeting.max_size() << std::endl;
    
    return 0;
}
```

## String Access and Modification

### Character Access

```cpp
#include <iostream>
#include <string>

int main() {
    std::string text = "Hello, World!";
    
    std::cout << "Original string: " << text << std::endl;
    
    // Different ways to access characters
    std::cout << "Character access:" << std::endl;
    std::cout << "text[0]: " << text[0] << std::endl;           // 'H' (no bounds check)
    std::cout << "text.at(1): " << text.at(1) << std::endl;     // 'e' (bounds checked)
    std::cout << "text.front(): " << text.front() << std::endl; // 'H' (first character)
    std::cout << "text.back(): " << text.back() << std::endl;   // '!' (last character)
    
    // Modify characters
    text[0] = 'h';              // Change first character
    text.at(7) = 'w';           // Change character at index 7
    
    std::cout << "After modification: " << text << std::endl;
    
    // Safe access with bounds checking
    try {
        std::cout << "Trying to access text.at(100)..." << std::endl;
        char c = text.at(100);  // Throws std::out_of_range
        std::cout << "Character at 100: " << c << std::endl;
    } catch (const std::out_of_range& e) {
        std::cout << "Exception: " << e.what() << std::endl;
    }
    
    // Iterate through string
    std::cout << "Character by character: ";
    for (size_t i = 0; i < text.size(); i++) {
        std::cout << text[i] << " ";
    }
    std::cout << std::endl;
    
    // Range-based for loop
    std::cout << "Using range-for: ";
    for (char c : text) {
        std::cout << c << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

### String Concatenation

```cpp
#include <iostream>
#include <string>

int main() {
    std::string first = "Hello";
    std::string second = "World";
    
    // Different ways to concatenate
    std::string result1 = first + ", " + second + "!";
    std::cout << "Using +: " << result1 << std::endl;
    
    std::string result2 = first;
    result2 += ", ";
    result2 += second;
    result2 += "!";
    std::cout << "Using +=: " << result2 << std::endl;
    
    std::string result3 = first;
    result3.append(", ");
    result3.append(second);
    result3.append("!");
    std::cout << "Using append(): " << result3 << std::endl;
    
    // Append with repetition
    std::string exclamation = "!";
    result3.append(3, '!');  // Append 3 '!' characters
    std::cout << "With repeated chars: " << result3 << std::endl;
    
    // Append substring
    std::string source = "Beautiful Day";
    std::string result4 = "What a ";
    result4.append(source, 0, 9);  // Append "Beautiful"
    std::cout << "Appending substring: " << result4 << std::endl;
    
    return 0;
}
```

## String Operations

### Insertion and Deletion

```cpp
#include <iostream>
#include <string>

int main() {
    std::string text = "Hello World";
    std::cout << "Original: " << text << std::endl;
    
    // Insert at position
    text.insert(5, ", beautiful");
    std::cout << "After insert: " << text << std::endl;
    
    // Insert single character
    text.insert(0, 1, '*');  // Insert '*' at beginning
    std::cout << "After char insert: " << text << std::endl;
    
    // Insert using iterator
    text.insert(text.end(), '!');
    std::cout << "After iterator insert: " << text << std::endl;
    
    // Erase characters
    text.erase(0, 1);  // Remove first character
    std::cout << "After erase first: " << text << std::endl;
    
    text.erase(5, 12);  // Remove ", beautiful"
    std::cout << "After erase substring: " << text << std::endl;
    
    // Erase single character
    text.erase(text.end() - 1);  // Remove last character
    std::cout << "After erase last: " << text << std::endl;
    
    // Replace substring
    text.replace(6, 5, "C++");  // Replace "World" with "C++"
    std::cout << "After replace: " << text << std::endl;
    
    // Clear entire string
    std::string temp = text;
    temp.clear();
    std::cout << "After clear: '" << temp << "' (empty: " << std::boolalpha << temp.empty() << ")" << std::endl;
    
    return 0;
}
```

### Substring Operations

```cpp
#include <iostream>
#include <string>

int main() {
    std::string text = "The quick brown fox jumps over the lazy dog";
    std::cout << "Original: " << text << std::endl;
    
    // Extract substrings
    std::string word1 = text.substr(0, 3);      // "The"
    std::string word2 = text.substr(4, 5);      // "quick"
    std::string from_pos = text.substr(10);      // From position 10 to end
    
    std::cout << "First word: " << word1 << std::endl;
    std::cout << "Second word: " << word2 << std::endl;
    std::cout << "From position 10: " << from_pos << std::endl;
    
    // Find operations
    size_t pos = text.find("fox");
    if (pos != std::string::npos) {
        std::cout << "Found 'fox' at position: " << pos << std::endl;
        std::string around_fox = text.substr(pos - 6, 15);
        std::cout << "Context around 'fox': " << around_fox << std::endl;
    }
    
    // Find last occurrence
    pos = text.rfind("the");
    if (pos != std::string::npos) {
        std::cout << "Last 'the' at position: " << pos << std::endl;
    }
    
    // Find any character from a set
    pos = text.find_first_of("aeiou");
    if (pos != std::string::npos) {
        std::cout << "First vowel '" << text[pos] << "' at position: " << pos << std::endl;
    }
    
    // Find character not in set
    pos = text.find_first_not_of("The ");
    if (pos != std::string::npos) {
        std::cout << "First non-'The ' character '" << text[pos] << "' at position: " << pos << std::endl;
    }
    
    return 0;
}
```

## String Comparison

### Comparison Operations

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

int main() {
    std::string str1 = "apple";
    std::string str2 = "banana";
    std::string str3 = "apple";
    std::string str4 = "Apple";
    
    std::cout << "Comparison results:" << std::endl;
    
    // Basic comparisons
    std::cout << "str1 == str2: " << std::boolalpha << (str1 == str2) << std::endl;  // false
    std::cout << "str1 == str3: " << std::boolalpha << (str1 == str3) << std::endl;  // true
    std::cout << "str1 != str2: " << std::boolalpha << (str1 != str2) << std::endl;  // true
    
    // Lexicographical comparisons
    std::cout << "str1 < str2: " << std::boolalpha << (str1 < str2) << std::endl;    // true
    std::cout << "str1 > str4: " << std::boolalpha << (str1 > str4) << std::endl;    // true (lowercase > uppercase)
    
    // Compare method (returns <0, 0, or >0)
    int cmp = str1.compare(str2);
    std::cout << "str1.compare(str2): " << cmp << " (negative means str1 < str2)" << std::endl;
    
    // Case-insensitive comparison (manual implementation)
    auto toLower = [](std::string s) {
        std::transform(s.begin(), s.end(), s.begin(), ::tolower);
        return s;
    };
    
    bool caseInsensitiveEqual = (toLower(str1) == toLower(str4));
    std::cout << "Case-insensitive str1 == str4: " << std::boolalpha << caseInsensitiveEqual << std::endl;
    
    // Sorting strings
    std::vector<std::string> fruits = {"banana", "apple", "cherry", "date", "elderberry"};
    std::cout << "\nOriginal order: ";
    for (const auto& fruit : fruits) {
        std::cout << fruit << " ";
    }
    std::cout << std::endl;
    
    std::sort(fruits.begin(), fruits.end());
    std::cout << "Sorted order: ";
    for (const auto& fruit : fruits) {
        std::cout << fruit << " ";
    }
    std::cout << std::endl;
    
    // Custom sorting (by length)
    std::sort(fruits.begin(), fruits.end(), [](const std::string& a, const std::string& b) {
        return a.length() < b.length();
    });
    std::cout << "Sorted by length: ";
    for (const auto& fruit : fruits) {
        std::cout << fruit << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

## String Search and Replace

### Advanced Search Operations

```cpp
#include <iostream>
#include <string>
#include <vector>

std::vector<size_t> findAllOccurrences(const std::string& text, const std::string& pattern) {
    std::vector<size_t> positions;
    size_t pos = 0;
    
    while ((pos = text.find(pattern, pos)) != std::string::npos) {
        positions.push_back(pos);
        pos += pattern.length();  // Move past this occurrence
    }
    
    return positions;
}

std::string replaceAll(std::string text, const std::string& from, const std::string& to) {
    size_t pos = 0;
    while ((pos = text.find(from, pos)) != std::string::npos) {
        text.replace(pos, from.length(), to);
        pos += to.length();  // Move past the replacement
    }
    return text;
}

bool startsWith(const std::string& text, const std::string& prefix) {
    return text.size() >= prefix.size() && 
           text.substr(0, prefix.size()) == prefix;
}

bool endsWith(const std::string& text, const std::string& suffix) {
    return text.size() >= suffix.size() && 
           text.substr(text.size() - suffix.size()) == suffix;
}

int main() {
    std::string text = "The quick brown fox jumps over the lazy dog. The fox is quick.";
    std::cout << "Text: " << text << std::endl;
    
    // Find all occurrences
    auto positions = findAllOccurrences(text, "the");
    std::cout << "\nFound 'the' at positions: ";
    for (size_t pos : positions) {
        std::cout << pos << " ";
    }
    std::cout << std::endl;
    
    // Case-sensitive vs case-insensitive search
    auto foxPositions = findAllOccurrences(text, "fox");
    auto FoxPositions = findAllOccurrences(text, "Fox");
    std::cout << "Found 'fox' at: ";
    for (size_t pos : foxPositions) std::cout << pos << " ";
    std::cout << std::endl;
    std::cout << "Found 'Fox' at: ";
    for (size_t pos : FoxPositions) std::cout << pos << " ";
    std::cout << std::endl;
    
    // Replace operations
    std::string modified = replaceAll(text, "fox", "cat");
    std::cout << "\nAfter replacing 'fox' with 'cat': " << modified << std::endl;
    
    // Multiple replacements
    modified = replaceAll(modified, "quick", "fast");
    modified = replaceAll(modified, "lazy", "sleepy");
    std::cout << "After multiple replacements: " << modified << std::endl;
    
    // Prefix and suffix checking
    std::string filename = "document.txt";
    std::cout << "\nFilename: " << filename << std::endl;
    std::cout << "Starts with 'doc': " << std::boolalpha << startsWith(filename, "doc") << std::endl;
    std::cout << "Ends with '.txt': " << std::boolalpha << endsWith(filename, ".txt") << std::endl;
    std::cout << "Ends with '.pdf': " << std::boolalpha << endsWith(filename, ".pdf") << std::endl;
    
    return 0;
}
```

### Pattern Matching and Validation

```cpp
#include <iostream>
#include <string>
#include <regex>
#include <cctype>

bool isValidEmail(const std::string& email) {
    // Simple email validation (not RFC compliant)
    size_t atPos = email.find('@');
    if (atPos == std::string::npos || atPos == 0 || atPos == email.length() - 1) {
        return false;
    }
    
    size_t dotPos = email.find('.', atPos);
    if (dotPos == std::string::npos || dotPos == email.length() - 1) {
        return false;
    }
    
    return true;
}

bool isNumeric(const std::string& str) {
    if (str.empty()) return false;
    
    size_t start = 0;
    if (str[0] == '+' || str[0] == '-') {
        start = 1;
        if (str.length() == 1) return false;
    }
    
    bool hasDecimal = false;
    for (size_t i = start; i < str.length(); i++) {
        if (str[i] == '.') {
            if (hasDecimal) return false;  // Multiple decimal points
            hasDecimal = true;
        } else if (!std::isdigit(str[i])) {
            return false;
        }
    }
    
    return true;
}

std::string extractDomain(const std::string& email) {
    size_t atPos = email.find('@');
    if (atPos != std::string::npos && atPos < email.length() - 1) {
        return email.substr(atPos + 1);
    }
    return "";
}

int main() {
    // Email validation
    std::vector<std::string> emails = {
        "user@example.com",
        "invalid.email",
        "@example.com",
        "user@",
        "test@domain.co.uk",
        "user@example."
    };
    
    std::cout << "Email validation:" << std::endl;
    for (const auto& email : emails) {
        bool valid = isValidEmail(email);
        std::cout << email << ": " << (valid ? "valid" : "invalid");
        if (valid) {
            std::cout << " (domain: " << extractDomain(email) << ")";
        }
        std::cout << std::endl;
    }
    
    // Numeric validation
    std::vector<std::string> numbers = {
        "123", "-456", "+789", "12.34", "-5.67", 
        "abc", "12.34.56", "12a", "", "."
    };
    
    std::cout << "\nNumeric validation:" << std::endl;
    for (const auto& num : numbers) {
        std::cout << "'" << num << "': " << (isNumeric(num) ? "numeric" : "not numeric") << std::endl;
    }
    
    // Using regex for more complex patterns (C++11)
    std::cout << "\nRegex validation:" << std::endl;
    std::regex phonePattern(R"(\d{3}-\d{3}-\d{4})");  // XXX-XXX-XXXX format
    
    std::vector<std::string> phones = {
        "123-456-7890", "555-0123", "123-45-6789", "abc-def-ghij"
    };
    
    for (const auto& phone : phones) {
        bool matches = std::regex_match(phone, phonePattern);
        std::cout << phone << ": " << (matches ? "valid phone" : "invalid phone") << std::endl;
    }
    
    return 0;
}
```

## String Formatting and Parsing

### Number Conversion

```cpp
#include <iostream>
#include <string>
#include <sstream>
#include <iomanip>

int main() {
    // String to number conversion
    std::string numStr = "12345";
    std::string floatStr = "123.456";
    std::string invalidStr = "12a34";
    
    try {
        int intVal = std::stoi(numStr);
        double doubleVal = std::stod(floatStr);
        
        std::cout << "Converted integer: " << intVal << std::endl;
        std::cout << "Converted double: " << doubleVal << std::endl;
        
        // This will throw an exception
        int invalidVal = std::stoi(invalidStr);
        std::cout << "This won't print: " << invalidVal << std::endl;
        
    } catch (const std::invalid_argument& e) {
        std::cout << "Invalid argument: " << e.what() << std::endl;
    } catch (const std::out_of_range& e) {
        std::cout << "Out of range: " << e.what() << std::endl;
    }
    
    // Number to string conversion
    int number = 42;
    double pi = 3.14159;
    
    std::string intStr = std::to_string(number);
    std::string doubleStr = std::to_string(pi);
    
    std::cout << "Integer as string: " << intStr << std::endl;
    std::cout << "Double as string: " << doubleStr << std::endl;
    
    // Custom formatting with stringstream
    std::ostringstream oss;
    oss << "Formatted number: " << std::fixed << std::setprecision(2) << pi;
    std::string formatted = oss.str();
    std::cout << formatted << std::endl;
    
    // Parsing multiple values from string
    std::string data = "42 3.14 hello 100";
    std::istringstream iss(data);
    
    int intPart;
    double doublePart;
    std::string stringPart;
    int lastInt;
    
    iss >> intPart >> doublePart >> stringPart >> lastInt;
    
    std::cout << "Parsed values: " << intPart << ", " << doublePart 
              << ", " << stringPart << ", " << lastInt << std::endl;
    
    return 0;
}
```

### CSV Parsing Example

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <sstream>

std::vector<std::string> splitString(const std::string& str, char delimiter) {
    std::vector<std::string> tokens;
    std::stringstream ss(str);
    std::string token;
    
    while (std::getline(ss, token, delimiter)) {
        tokens.push_back(token);
    }
    
    return tokens;
}

std::string trim(const std::string& str) {
    size_t start = str.find_first_not_of(" \t\n\r");
    if (start == std::string::npos) return "";
    
    size_t end = str.find_last_not_of(" \t\n\r");
    return str.substr(start, end - start + 1);
}

struct Person {
    std::string name;
    int age;
    std::string city;
    
    void print() const {
        std::cout << "Name: " << name << ", Age: " << age << ", City: " << city << std::endl;
    }
};

std::vector<Person> parseCSV(const std::vector<std::string>& lines) {
    std::vector<Person> people;
    
    for (size_t i = 1; i < lines.size(); i++) {  // Skip header line
        auto fields = splitString(lines[i], ',');
        if (fields.size() >= 3) {
            Person person;
            person.name = trim(fields[0]);
            person.age = std::stoi(trim(fields[1]));
            person.city = trim(fields[2]);
            people.push_back(person);
        }
    }
    
    return people;
}

int main() {
    // Sample CSV data
    std::vector<std::string> csvLines = {
        "Name,Age,City",
        "Alice Johnson,25,New York",
        "Bob Smith,30,Los Angeles",
        "Charlie Brown,22,Chicago",
        "Diana Prince,28,Seattle"
    };
    
    std::cout << "CSV Data:" << std::endl;
    for (const auto& line : csvLines) {
        std::cout << line << std::endl;
    }
    
    // Parse CSV
    auto people = parseCSV(csvLines);
    
    std::cout << "\nParsed Data:" << std::endl;
    for (const auto& person : people) {
        person.print();
    }
    
    // String manipulation examples
    std::cout << "\nString manipulation examples:" << std::endl;
    
    std::string sample = "  Hello, World!  ";
    std::cout << "Original: '" << sample << "'" << std::endl;
    std::cout << "Trimmed: '" << trim(sample) << "'" << std::endl;
    
    std::string sentence = "The quick brown fox";
    auto words = splitString(sentence, ' ');
    std::cout << "Words in '" << sentence << "':" << std::endl;
    for (size_t i = 0; i < words.size(); i++) {
        std::cout << "  " << i << ": '" << words[i] << "'" << std::endl;
    }
    
    return 0;
}
```

## String Performance Optimization

### Efficient String Operations

```cpp
#include <iostream>
#include <string>
#include <chrono>
#include <sstream>

class Timer {
private:
    std::chrono::high_resolution_clock::time_point start;
    std::string name;
    
public:
    Timer(const std::string& n) : name(n) {
        start = std::chrono::high_resolution_clock::now();
    }
    
    ~Timer() {
        auto end = std::chrono::high_resolution_clock::now();
        auto duration = std::chrono::duration_cast<std::chrono::microseconds>(end - start);
        std::cout << name << " took: " << duration.count() << " microseconds" << std::endl;
    }
};

void stringConcatenationPerformance() {
    const int COUNT = 10000;
    
    std::cout << "=== String Concatenation Performance ===" << std::endl;
    
    // Method 1: Repeated concatenation (inefficient)
    {
        Timer timer("Repeated += (inefficient)");
        std::string result;
        for (int i = 0; i < COUNT; i++) {
            result += "Hello";
        }
    }
    
    // Method 2: Reserve capacity
    {
        Timer timer("Reserve + += (better)");
        std::string result;
        result.reserve(COUNT * 5);  // Pre-allocate space
        for (int i = 0; i < COUNT; i++) {
            result += "Hello";
        }
    }
    
    // Method 3: Using stringstream
    {
        Timer timer("stringstream (good for complex)");
        std::ostringstream oss;
        for (int i = 0; i < COUNT; i++) {
            oss << "Hello";
        }
        std::string result = oss.str();
    }
    
    // Method 4: Pre-calculate size
    {
        Timer timer("Pre-calculated size (fastest)");
        std::string result(COUNT * 5, 'x');  // Pre-allocate exact size
        size_t pos = 0;
        for (int i = 0; i < COUNT; i++) {
            result.replace(pos, 5, "Hello");
            pos += 5;
        }
    }
}

void stringSearchPerformance() {
    std::cout << "\n=== String Search Performance ===" << std::endl;
    
    // Create a large string
    std::string largeText;
    largeText.reserve(1000000);
    for (int i = 0; i < 100000; i++) {
        largeText += "This is a sample text for searching. ";
    }
    largeText += "FIND_ME";  // Target at the end
    
    // Method 1: find() method
    {
        Timer timer("std::string::find()");
        size_t pos = largeText.find("FIND_ME");
        if (pos != std::string::npos) {
            std::cout << "Found at position: " << pos << std::endl;
        }
    }
    
    // Method 2: Manual search (usually slower)
    {
        Timer timer("Manual search loop");
        std::string target = "FIND_ME";
        bool found = false;
        for (size_t i = 0; i <= largeText.length() - target.length(); i++) {
            if (largeText.substr(i, target.length()) == target) {
                std::cout << "Found manually at position: " << i << std::endl;
                found = true;
                break;
            }
        }
    }
}

int main() {
    stringConcatenationPerformance();
    stringSearchPerformance();
    
    std::cout << "\n=== Memory Usage Tips ===" << std::endl;
    
    std::string str = "Small string";
    std::cout << "Small string - Size: " << str.size() << ", Capacity: " << str.capacity() << std::endl;
    
    str.reserve(1000);
    std::cout << "After reserve(1000) - Size: " << str.size() << ", Capacity: " << str.capacity() << std::endl;
    
    str = "Still small";
    std::cout << "After reassignment - Size: " << str.size() << ", Capacity: " << str.capacity() << std::endl;
    
    str.shrink_to_fit();
    std::cout << "After shrink_to_fit() - Size: " << str.size() << ", Capacity: " << str.capacity() << std::endl;
    
    return 0;
}
```

## Advanced String Features

### String Views (C++17)

```cpp
#include <iostream>
#include <string>
#include <string_view>

void processStringView(std::string_view sv) {
    std::cout << "Processing: '" << sv << "' (length: " << sv.length() << ")" << std::endl;
}

void processString(const std::string& s) {
    std::cout << "Processing string: '" << s << "' (length: " << s.length() << ")" << std::endl;
}

int main() {
    std::cout << "=== String View Example (C++17) ===" << std::endl;
    
    std::string str = "Hello, World!";
    const char* cstr = "C-style string";
    
    // string_view can reference different string types without copying
    processStringView(str);           // From std::string
    processStringView(cstr);          // From C-string
    processStringView("String literal");  // From string literal
    
    // Substring without copying
    std::string_view substr = std::string_view(str).substr(0, 5);
    processStringView(substr);        // "Hello"
    
    // Performance comparison
    std::cout << "\nPerformance benefit: no copying when passing substrings" << std::endl;
    
    // string_view is just a view - original string must remain valid
    std::string_view dangling;
    {
        std::string temp = "Temporary string";
        dangling = temp;  // Dangerous: temp will be destroyed
    }
    // std::cout << dangling;  // ✘ Undefined behavior - temp is destroyed
    
    return 0;
}
```

### Unicode and Wide Strings

```cpp
#include <iostream>
#include <string>
#include <locale>
#include <codecvt>

int main() {
    // Different string types
    std::string utf8String = u8"Hello, 世界!";           // UTF-8
    std::wstring wideString = L"Hello, 世界!";           // Wide string
    std::u16string utf16String = u"Hello, 世界!";        // UTF-16
    std::u32string utf32String = U"Hello, 世界!";        // UTF-32
    
    std::cout << "UTF-8 string: " << utf8String << std::endl;
    std::wcout << L"Wide string: " << wideString << std::endl;
    
    std::cout << "String lengths:" << std::endl;
    std::cout << "UTF-8 length: " << utf8String.length() << " bytes" << std::endl;
    std::cout << "Wide length: " << wideString.length() << " wide chars" << std::endl;
    std::cout << "UTF-16 length: " << utf16String.length() << " 16-bit units" << std::endl;
    std::cout << "UTF-32 length: " << utf32String.length() << " 32-bit units" << std::endl;
    
    // Note: Actual Unicode support depends on system locale and compiler
    // For full Unicode support, consider libraries like ICU
    
    return 0;
}
```

## Practical String Applications

### Text File Processing

```cpp
#include <iostream>
#include <string>
#include <fstream>
#include <vector>
#include <map>
#include <sstream>
#include <algorithm>
#include <cctype>

class TextAnalyzer {
private:
    std::string text;
    
public:
    void loadFromFile(const std::string& filename) {
        std::ifstream file(filename);
        if (!file.is_open()) {
            throw std::runtime_error("Cannot open file: " + filename);
        }
        
        std::string line;
        text.clear();
        while (std::getline(file, line)) {
            text += line + "\n";
        }
    }
    
    void loadFromString(const std::string& input) {
        text = input;
    }
    
    int countWords() const {
        std::istringstream iss(text);
        std::string word;
        int count = 0;
        while (iss >> word) {
            count++;
        }
        return count;
    }
    
    int countLines() const {
        return std::count(text.begin(), text.end(), '\n');
    }
    
    int countCharacters() const {
        return text.length();
    }
    
    std::map<std::string, int> getWordFrequency() const {
        std::map<std::string, int> frequency;
        std::istringstream iss(text);
        std::string word;
        
        while (iss >> word) {
            // Remove punctuation and convert to lowercase
            word.erase(std::remove_if(word.begin(), word.end(), ::ispunct), word.end());
            std::transform(word.begin(), word.end(), word.begin(), ::tolower);
            
            if (!word.empty()) {
                frequency[word]++;
            }
        }
        
        return frequency;
    }
    
    std::vector<std::pair<std::string, int>> getMostFrequentWords(int count) const {
        auto frequency = getWordFrequency();
        
        std::vector<std::pair<std::string, int>> words(frequency.begin(), frequency.end());
        
        std::sort(words.begin(), words.end(), 
                  [](const auto& a, const auto& b) {
                      return a.second > b.second;
                  });
        
        if (words.size() > static_cast<size_t>(count)) {
            words.resize(count);
        }
        
        return words;
    }
    
    void printStatistics() const {
        std::cout << "Text Statistics:" << std::endl;
        std::cout << "Characters: " << countCharacters() << std::endl;
        std::cout << "Lines: " << countLines() << std::endl;
        std::cout << "Words: " << countWords() << std::endl;
        
        auto topWords = getMostFrequentWords(5);
        std::cout << "Top 5 most frequent words:" << std::endl;
        for (const auto& pair : topWords) {
            std::cout << "  " << pair.first << ": " << pair.second << std::endl;
        }
    }
};

int main() {
    TextAnalyzer analyzer;
    
    // Sample text for analysis
    std::string sampleText = R"(
        The quick brown fox jumps over the lazy dog.
        The dog was sleeping in the sun.
        Quick movements of the fox startled the dog.
        The brown fox and the lazy dog became friends.
        In the end, the fox and dog played together in the sun.
    )";
    
    analyzer.loadFromString(sampleText);
    analyzer.printStatistics();
    
    return 0;
}
```

### Configuration File Parser

```cpp
#include <iostream>
#include <string>
#include <map>
#include <fstream>
#include <sstream>

class ConfigParser {
private:
    std::map<std::string, std::string> settings;
    
    std::string trim(const std::string& str) {
        size_t start = str.find_first_not_of(" \t");
        if (start == std::string::npos) return "";
        
        size_t end = str.find_last_not_of(" \t");
        return str.substr(start, end - start + 1);
    }
    
public:
    void parseString(const std::string& configText) {
        std::istringstream iss(configText);
        std::string line;
        
        while (std::getline(iss, line)) {
            line = trim(line);
            
            // Skip empty lines and comments
            if (line.empty() || line[0] == '#' || line[0] == ';') {
                continue;
            }
            
            // Find the '=' separator
            size_t pos = line.find('=');
            if (pos != std::string::npos) {
                std::string key = trim(line.substr(0, pos));
                std::string value = trim(line.substr(pos + 1));
                
                // Remove quotes from value if present
                if (value.length() >= 2 && 
                    ((value.front() == '"' && value.back() == '"') ||
                     (value.front() == '\'' && value.back() == '\''))) {
                    value = value.substr(1, value.length() - 2);
                }
                
                settings[key] = value;
            }
        }
    }
    
    std::string getString(const std::string& key, const std::string& defaultValue = "") const {
        auto it = settings.find(key);
        return (it != settings.end()) ? it->second : defaultValue;
    }
    
    int getInt(const std::string& key, int defaultValue = 0) const {
        auto it = settings.find(key);
        if (it != settings.end()) {
            try {
                return std::stoi(it->second);
            } catch (...) {
                return defaultValue;
            }
        }
        return defaultValue;
    }
    
    bool getBool(const std::string& key, bool defaultValue = false) const {
        auto it = settings.find(key);
        if (it != settings.end()) {
            std::string value = it->second;
            std::transform(value.begin(), value.end(), value.begin(), ::tolower);
            return value == "true" || value == "yes" || value == "1" || value == "on";
        }
        return defaultValue;
    }
    
    void printAll() const {
        std::cout << "Configuration settings:" << std::endl;
        for (const auto& pair : settings) {
            std::cout << "  " << pair.first << " = " << pair.second << std::endl;
        }
    }
};

int main() {
    std::string configData = R"(
        # Server configuration
        server_name = "My Web Server"
        port = 8080
        debug_mode = true
        
        ; Database settings
        db_host = localhost
        db_port = 5432
        db_name = 'myapp_db'
        
        # Performance settings
        max_connections = 100
        timeout = 30
        enable_cache = yes
    )";
    
    ConfigParser config;
    config.parseString(configData);
    
    config.printAll();
    
    std::cout << "\nAccessing specific settings:" << std::endl;
    std::cout << "Server name: " << config.getString("server_name") << std::endl;
    std::cout << "Port: " << config.getInt("port") << std::endl;
    std::cout << "Debug mode: " << std::boolalpha << config.getBool("debug_mode") << std::endl;
    std::cout << "Max connections: " << config.getInt("max_connections") << std::endl;
    std::cout << "Enable cache: " << std::boolalpha << config.getBool("enable_cache") << std::endl;
    
    // Test default values
    std::cout << "Non-existent key: " << config.getString("missing_key", "default_value") << std::endl;
    
    return 0;
}
```

## Common String Pitfalls

### Buffer Overflows and Memory Issues

```cpp
#include <iostream>
#include <string>
#include <cstring>

void cStringProblems() {
    std::cout << "=== C-String Problems ===" << std::endl;
    
    // ✘ Buffer overflow with C-strings
    char buffer[10];
    // strcpy(buffer, "This string is too long");  // Buffer overflow!
    
    // → Safe with std::string
    std::string safeString = "This string can be as long as needed without buffer overflow";
    std::cout << "Safe string length: " << safeString.length() << std::endl;
    
    // ✘ Null terminator issues
    char notTerminated[5] = {'H', 'e', 'l', 'l', 'o'};  // No '\0'
    // std::cout << notTerminated;  // Undefined behavior!
    
    // → std::string handles this automatically
    std::string properString(notTerminated, 5);  // Specify length
    std::cout << "Proper string: " << properString << std::endl;
}

void performancePitfalls() {
    std::cout << "\n=== Performance Pitfalls ===" << std::endl;
    
    // ✘ Inefficient string concatenation in loop
    std::string result;
    for (int i = 0; i < 1000; i++) {
        result += "x";  // Potentially many reallocations
    }
    
    // → Better: reserve space
    std::string betterResult;
    betterResult.reserve(1000);
    for (int i = 0; i < 1000; i++) {
        betterResult += "x";
    }
    
    // ✘ Unnecessary string copies
    auto badFunction = [](std::string s) {  // Copies the string
        return s.length();
    };
    
    // → Better: use const reference
    auto goodFunction = [](const std::string& s) {  // No copy
        return s.length();
    };
    
    std::string test = "test string";
    std::cout << "Length: " << goodFunction(test) << std::endl;
}

int main() {
    cStringProblems();
    performancePitfalls();
    return 0;
}
```

## Exercises

### Exercise 1: String Utilities

Implement a string utility library with functions for:

- Case conversion (upper, lower, title case)
- String trimming (left, right, both)
- Word wrapping for console output
- String padding and alignment

### Exercise 2: Text Processing Tools

Create tools for text processing:

- Word frequency analyzer
- Text statistics calculator
- Simple search and replace tool
- Text formatter (remove extra spaces, fix punctuation)

### Exercise 3: Data Parsing

Implement parsers for different formats:

- JSON-like key-value parser
- Simple XML tag extractor
- Command-line argument parser
- URL component extractor

### Exercise 4: String Algorithms

Implement classic string algorithms:

- String matching (KMP or Boyer-Moore)
- Edit distance calculation
- Anagram detector
- Palindrome checker with optimizations

## Key Takeaways

<i class="fa-solid fa-arrow-right"></i> **`std::string` provides safe, convenient text handling** compared to C-strings  
<i class="fa-solid fa-arrow-right"></i> **Use `reserve()` for better performance** when string size is predictable  
<i class="fa-solid fa-arrow-right"></i> **Prefer `const std::string&` parameters** to avoid unnecessary copies  
<i class="fa-solid fa-arrow-right"></i> **String search and replace operations** are powerful for text processing  
<i class="fa-solid fa-arrow-right"></i> **Be aware of encoding issues** with international text  
<i class="fa-solid fa-arrow-right"></i> **Consider `std::string_view`** (C++17) for read-only string operations

---

**Next**: Explore other standard containers for specialized use cases // →  [Other Standard Containers](other-containers.md)
