# ⚙️ C++ Cheatsheet

> Part of [CodeCraftClub-Cheatsheets](../README.md) · Modern C++ (C++11/17) quick reference.

---

## Hello World & Compiling

```cpp
#include <iostream>
int main() {
    std::cout << "Hello, World!" << std::endl;
    return 0;
}
```

```bash
g++ -std=c++17 main.cpp -o main && ./main
```

## Basic Types

```cpp
int n = 42;
long long big = 10'000'000'000;
double pi = 3.14159;
char c = 'A';
bool flag = true;
auto x = 3.5;            // type deduced -> double
const int MAX = 100;
```

## Input / Output

```cpp
int age;
std::cin >> age;                       // read one value
std::string line;
std::getline(std::cin, line);          // read full line
std::cout << "Age: " << age << "\n";
printf("%d %.2f\n", age, pi);          // C-style
```

## Strings

```cpp
#include <string>
std::string s = "Code Craft";
s.length(); s.substr(0, 4);            // "Code"
s += " Club"; s.find("Craft");         // index or npos
s[0]; s.append("!"); std::stoi("42"); std::to_string(42);
```

## Control Flow

```cpp
if (age >= 18) { } else if (age > 12) { } else { }

for (int i = 0; i < 5; i++) { }
for (int x : {1, 2, 3}) { }            // range-based
while (age < 30) age++;
do { } while (cond);

switch (day) {
    case 1: break;
    default: break;
}
```

## Functions

```cpp
int add(int a, int b = 0) { return a + b; }   // default arg
void swap(int& a, int& b) {                    // pass by reference
    int t = a; a = b; b = t;
}
auto square = [](int x) { return x * x; };     // lambda
```

## Pointers & References

```cpp
int x = 10;
int* p = &x;        // pointer holds address
*p = 20;            // dereference -> x is now 20
int& ref = x;       // reference (alias)
int* np = nullptr;  // null pointer
```

## STL Containers

```cpp
#include <vector>
#include <map>
#include <set>
#include <unordered_map>

std::vector<int> v = {1, 2, 3};
v.push_back(4); v.size(); v[0]; v.pop_back();

std::map<std::string, int> m;          // sorted by key
m["alan"] = 21; m.count("alan");

std::set<int> st = {3, 1, 2};          // sorted, unique
st.insert(5); st.find(3) != st.end();

std::unordered_map<std::string, int> um;  // hash map, O(1)
```

## Iterators & Algorithms

```cpp
#include <algorithm>
for (auto it = v.begin(); it != v.end(); ++it) { *it; }

std::sort(v.begin(), v.end());
std::sort(v.begin(), v.end(), std::greater<int>());  // desc
std::reverse(v.begin(), v.end());
std::max_element(v.begin(), v.end());
std::find(v.begin(), v.end(), 3);
int total = std::accumulate(v.begin(), v.end(), 0);  // <numeric>
```

## Structs & Classes

```cpp
struct Point { int x, y; };            // members public by default

class Animal {
private:
    std::string name;
public:
    Animal(std::string n) : name(n) {}      // constructor
    virtual std::string speak() {            // virtual = polymorphic
        return name + " makes a sound";
    }
};

class Dog : public Animal {            // inheritance
public:
    Dog(std::string n) : Animal(n) {}
    std::string speak() override { return "Woof"; }
};
```

## Smart Pointers (Modern Memory)

```cpp
#include <memory>
auto p = std::make_unique<int>(5);     // unique ownership
auto sp = std::make_shared<int>(10);   // shared, ref-counted
// no manual delete needed
```

## Templates

```cpp
template <typename T>
T maxOf(T a, T b) { return a > b ? a : b; }
maxOf(3, 7); maxOf(2.5, 1.0);
```

---

[🔝 Back to README](../README.md)
