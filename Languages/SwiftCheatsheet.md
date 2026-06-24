# 🍎 Swift Cheatsheet

> Part of [CodeCraftClub-Cheatsheets](../README.md) · Swift quick reference (iOS / macOS).

---

## Hello World & Running

```swift
print("Hello, World!")
```

```bash
swift main.swift      # run a script
swift                 # REPL
swift build / swift run   # SwiftPM project
```

## Variables & Types

```swift
let name = "Alan"       // constant (immutable)
var age = 21             // variable
let pi: Double = 3.14    // explicit type

// Types: Int, Double, Float, Bool, String, Character
let big = 1_000_000      // underscores for readability
Int("42"); String(42); Double("3.5")
```

## Optionals (null safety)

```swift
var maybe: String? = nil          // optional
maybe = "hello"

if let value = maybe {            // optional binding
    print(value)
}
guard let value = maybe else { return }   // early exit

maybe?.count                       // optional chaining
maybe ?? "default"                 // nil-coalescing
maybe!                             // force unwrap (crashes if nil)
```

## Strings

```swift
let s = "Code Craft Club"
s.count; s.uppercased(); s.lowercased()
s.contains("Craft"); s.hasPrefix("Code"); s.hasSuffix("Club")
s.split(separator: " ")
"\(name) is \(age)"                // interpolation
let combined = "a" + "b"
```

## Control Flow

```swift
if age >= 18 { } else if age > 12 { } else { }

let status = age >= 18 ? "adult" : "minor"

switch day {
case "Sat", "Sun":
    print("weekend")
case let d where d.count > 5:
    print("long name")
default:
    print("weekday")
}

for i in 0..<5 { }                 // 0,1,2,3,4
for i in 0...5 { }                 // inclusive
for item in array { }
while cond { }
repeat { } while cond
```

## Functions

```swift
func add(_ a: Int, _ b: Int) -> Int { a + b }   // _ omits label
func greet(name: String, greeting: String = "Hi") -> String {
    "\(greeting), \(name)"
}
add(2, 3); greet(name: "Alan")

let square = { (x: Int) -> Int in x * x }        // closure
let double: (Int) -> Int = { $0 * 2 }            // shorthand $0
```

## Collections

```swift
// Array
var nums = [1, 2, 3]
nums.append(4); nums.removeFirst(); nums.count; nums[0]
nums.map { $0 * 2 }; nums.filter { $0 > 1 }
nums.reduce(0, +); nums.sorted(); nums.contains(2)

// Dictionary
var user = ["name": "Alan", "age": "21"]
user["name"]; user["email"] = "a@b.com"
user.keys; user.values

// Set
var s: Set = [1, 2, 2, 3]          // {1,2,3}
s.insert(4); s.contains(2)
```

## Structs, Classes & Enums

```swift
struct Point {                     // value type
    var x: Int
    var y: Int
    func distance() -> Double { Double(x*x + y*y).squareRoot() }
}

class Animal {                     // reference type
    var name: String
    init(name: String) { self.name = name }
    func speak() -> String { "\(name) makes a sound" }
}

class Dog: Animal {                // inheritance
    override func speak() -> String { "\(name) barks" }
}

enum Direction {
    case north, south, east, west
}

enum Shape {
    case circle(radius: Double)
    case rect(w: Double, h: Double)
}
```

## Protocols (interfaces)

```swift
protocol Shape {
    var area: Double { get }
    func describe() -> String
}

struct Circle: Shape {
    var radius: Double
    var area: Double { .pi * radius * radius }
    func describe() -> String { "circle" }
}
```

## Error Handling

```swift
enum MyError: Error { case notFound }

func load() throws -> String {
    throw MyError.notFound
}

do {
    let data = try load()
} catch MyError.notFound {
    print("not found")
} catch {
    print(error)
}

let safe = try? load()             // nil on error
```

---

[🔝 Back to README](../README.md)
