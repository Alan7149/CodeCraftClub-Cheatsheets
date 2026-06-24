# ☕ Java Cheatsheet

> Part of [CodeCraftClub-Cheatsheets](../README.md) · Core Java quick reference.

---

## Hello World & Running

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

```bash
javac Main.java && java Main      # compile + run
java Main.java                    # run directly (JDK 11+)
```

## Variables & Types

```java
int n = 42;
long big = 10000000000L;
double pi = 3.14;
char c = 'A';
boolean flag = true;
String s = "hello";
final int MAX = 100;        // constant
var x = 3.5;                // inferred (Java 10+)
```

## Strings

```java
String s = "Code Craft Club";
s.length(); s.charAt(0); s.substring(0, 4);
s.toUpperCase(); s.trim(); s.replace("Club", "Crew");
s.split(" "); s.contains("Craft"); s.indexOf("C");
String.format("%s is %d", "Alan", 21);
String joined = String.join("-", "a", "b");   // "a-b"
Integer.parseInt("42"); String.valueOf(42);
// StringBuilder for heavy concatenation
StringBuilder sb = new StringBuilder();
sb.append("a").append("b"); sb.toString();
```

## Control Flow

```java
if (age >= 18) { } else if (age > 12) { } else { }

String r = age >= 18 ? "adult" : "minor";    // ternary

for (int i = 0; i < 5; i++) { }
for (int x : new int[]{1, 2, 3}) { }          // for-each
while (cond) { }
do { } while (cond);

switch (day) {
    case "Sat", "Sun" -> System.out.println("weekend");
    default -> System.out.println("weekday");
}
```

## Arrays & Collections

```java
int[] arr = {1, 2, 3};
arr.length; arr[0];
import java.util.*;

List<Integer> list = new ArrayList<>();
list.add(1); list.get(0); list.size(); list.remove(0);

Map<String, Integer> map = new HashMap<>();
map.put("alan", 21); map.get("alan"); map.containsKey("alan");

Set<Integer> set = new HashSet<>();
set.add(1); set.contains(1);

Collections.sort(list);
list.sort(Comparator.reverseOrder());
```

## Methods

```java
public static int add(int a, int b) { return a + b; }
public int square(int x) { return x * x; }
// varargs
public static int sum(int... nums) {
    int t = 0;
    for (int n : nums) t += n;
    return t;
}
```

## Classes & OOP

```java
public class Animal {
    private String name;                    // encapsulation
    public Animal(String name) { this.name = name; }
    public String getName() { return name; }
    public String speak() { return name + " makes a sound"; }
}

public class Dog extends Animal {           // inheritance
    public Dog(String name) { super(name); }
    @Override
    public String speak() { return getName() + " barks"; }
}

// Interface
interface Shape { double area(); }
class Circle implements Shape {
    double r;
    public double area() { return Math.PI * r * r; }
}
```

## Exceptions

```java
try {
    int x = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println(e.getMessage());
} catch (Exception e) {
    e.printStackTrace();
} finally {
    System.out.println("cleanup");
}

throw new IllegalArgumentException("bad input");
```

## Streams (Java 8+)

```java
List<Integer> nums = List.of(1, 2, 3, 4);
nums.stream()
    .filter(x -> x % 2 == 0)
    .map(x -> x * x)
    .collect(Collectors.toList());          // [4, 16]

nums.stream().reduce(0, Integer::sum);      // 10
nums.forEach(System.out::println);
```

## Generics

```java
class Box<T> {
    private T value;
    public void set(T v) { value = v; }
    public T get() { return value; }
}
Box<String> b = new Box<>();
```

---

[🔝 Back to README](../README.md)
