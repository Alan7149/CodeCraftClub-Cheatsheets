# 🐹 Go Cheatsheet

> Part of [CodeCraftClub-Cheatsheets](../README.md) · Go (Golang) quick reference.

---

## Hello World & Running

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}
```

```bash
go run main.go        # compile & run
go build              # build binary
go mod init myapp     # start a module
go get github.com/...  # add a dependency
```

## Variables & Types

```go
var name string = "Alan"
age := 21                 // short declaration (inferred)
const Pi = 3.14
var x, y = 1, 2
var flag bool             // zero value: false

// Basic types: int, int64, float64, string, bool, byte, rune
```

## Strings & Formatting

```go
import "strings"
s := "Code Craft Club"
len(s); s[0]                      // byte
strings.ToUpper(s); strings.Split(s, " ")
strings.Contains(s, "Craft"); strings.Replace(s, "Club", "Crew", 1)
fmt.Sprintf("%s is %d", name, age)
fmt.Printf("%v %T\n", age, age)   // value, type
```

## Control Flow

```go
if age >= 18 {
} else if age > 12 {
} else {
}

// if with init statement
if v := compute(); v > 0 {
    fmt.Println(v)
}

for i := 0; i < 5; i++ { }        // classic
for i < 10 { i++ }                 // while-style
for { break }                      // infinite
for idx, val := range slice { }    // range

switch day {
case "Sat", "Sun":
    fmt.Println("weekend")
default:
    fmt.Println("weekday")
}
```

## Functions

```go
func add(a, b int) int { return a + b }

func divmod(a, b int) (int, int) {     // multiple returns
    return a / b, a % b
}

func sum(nums ...int) int {            // variadic
    t := 0
    for _, n := range nums { t += n }
    return t
}

square := func(x int) int { return x * x }   // anonymous
```

## Collections

```go
// Array (fixed size)
var arr [3]int = [3]int{1, 2, 3}

// Slice (dynamic)
s := []int{1, 2, 3}
s = append(s, 4)
s = s[1:3]                  // slicing
len(s); cap(s)

// Map
m := map[string]int{"alan": 21}
m["bob"] = 30
v, ok := m["alan"]          // ok = exists?
delete(m, "bob")

for k, v := range m { }
```

## Structs & Methods

```go
type Animal struct {
    Name string
    Age  int
}

func (a Animal) Speak() string {           // method
    return a.Name + " makes a sound"
}

func (a *Animal) Birthday() {              // pointer receiver (mutates)
    a.Age++
}

rex := Animal{Name: "Rex", Age: 3}
rex.Speak(); rex.Birthday()
```

## Interfaces

```go
type Speaker interface {
    Speak() string
}

func describe(s Speaker) {
    fmt.Println(s.Speak())
}
// Animal satisfies Speaker implicitly (no "implements" keyword)
```

## Error Handling

```go
import "errors"

func read() (string, error) {
    return "", errors.New("not found")
}

data, err := read()
if err != nil {
    log.Fatal(err)
}

// defer runs at function exit
defer file.Close()
```

## Goroutines & Channels (Concurrency)

```go
go doWork()                 // run concurrently

ch := make(chan int)
go func() { ch <- 42 }()    // send
v := <-ch                   // receive

// WaitGroup
var wg sync.WaitGroup
wg.Add(1)
go func() { defer wg.Done(); work() }()
wg.Wait()
```

## Pointers

```go
x := 10
p := &x        // pointer
*p = 20        // dereference -> x = 20
```

---

[🔝 Back to README](../README.md)
