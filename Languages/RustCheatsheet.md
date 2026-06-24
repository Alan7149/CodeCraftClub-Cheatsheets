# 🦀 Rust Cheatsheet

> Part of [CodeCraftClub-Cheatsheets](../README.md) · Rust quick reference.

---

## Hello World & Running

```rust
fn main() {
    println!("Hello, World!");
}
```

```bash
cargo new myapp       # new project
cargo run             # build & run
cargo build --release # optimized build
cargo add serde       # add a dependency
```

## Variables & Types

```rust
let x = 5;                 // immutable by default
let mut y = 10;            // mutable
const MAX: u32 = 100;      // constant (type required)
let z: f64 = 3.14;         // explicit type

// Types: i32, u32, i64, f64, bool, char, &str, String, usize
let inferred = 42;         // i32 by default
```

## Strings

```rust
let s: &str = "slice";              // string slice (borrowed)
let mut owned = String::from("Code");
owned.push_str(" Craft");
owned.len(); owned.to_uppercase();
owned.contains("Craft"); owned.replace("Code", "Rust");
let parts: Vec<&str> = "a,b,c".split(',').collect();
format!("{} is {}", "Alan", 21);
let n: i32 = "42".parse().unwrap();
```

## Control Flow

```rust
if age >= 18 { } else if age > 12 { } else { }

let status = if age >= 18 { "adult" } else { "minor" };  // expression

for i in 0..5 { }                    // 0,1,2,3,4
for i in 0..=5 { }                   // inclusive
for item in &vec { }
while cond { }
loop { break; }                      // infinite

match day {
    "Sat" | "Sun" => println!("weekend"),
    _ => println!("weekday"),
}
```

## Functions

```rust
fn add(a: i32, b: i32) -> i32 {
    a + b                            // no semicolon = return
}

fn greet(name: &str) {
    println!("Hi, {}", name);
}

let square = |x: i32| x * x;         // closure
```

## Ownership & Borrowing (core concept)

```rust
let s1 = String::from("hello");
let s2 = s1;                  // s1 MOVED, no longer valid
let s3 = s2.clone();          // deep copy

fn print(s: &String) {}       // borrow (reference)
fn modify(s: &mut String) { s.push('!'); }  // mutable borrow

print(&s3);
let mut m = String::from("x");
modify(&mut m);
```

## Collections

```rust
// Vector
let mut v: Vec<i32> = vec![1, 2, 3];
v.push(4); v.pop(); v[0]; v.len();
let doubled: Vec<i32> = v.iter().map(|x| x * 2).collect();
let evens: Vec<&i32> = v.iter().filter(|x| **x % 2 == 0).collect();

// HashMap
use std::collections::HashMap;
let mut map = HashMap::new();
map.insert("alan", 21);
map.get("alan");                     // Option<&i32>
```

## Structs & Enums

```rust
struct Animal {
    name: String,
    age: u32,
}

impl Animal {
    fn new(name: &str) -> Self {
        Animal { name: name.to_string(), age: 0 }
    }
    fn speak(&self) -> String {
        format!("{} makes a sound", self.name)
    }
}

enum Direction { North, South, East, West }

enum Shape {
    Circle(f64),
    Rect(f64, f64),
}
```

## Option & Result (no null, no exceptions)

```rust
let maybe: Option<i32> = Some(5);
match maybe {
    Some(v) => println!("{}", v),
    None => println!("nothing"),
}
maybe.unwrap_or(0);

fn divide(a: i32, b: i32) -> Result<i32, String> {
    if b == 0 { Err("divide by zero".into()) }
    else { Ok(a / b) }
}

let r = divide(10, 2)?;              // ? propagates errors
```

## Traits (interfaces)

```rust
trait Speak {
    fn speak(&self) -> String;
}

impl Speak for Animal {
    fn speak(&self) -> String {
        format!("{} barks", self.name)
    }
}
```

---

[🔝 Back to README](../README.md)
