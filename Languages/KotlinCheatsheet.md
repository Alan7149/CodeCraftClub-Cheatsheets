# 🟪 Kotlin Cheatsheet

> Part of [CodeCraftClub-Cheatsheets](../README.md) · Kotlin quick reference (JVM / Android).

---

## Hello World & Running

```kotlin
fun main() {
    println("Hello, World!")
}
```

```bash
kotlinc main.kt -include-runtime -d main.jar && java -jar main.jar
```

## Variables & Types

```kotlin
val name = "Alan"       // immutable (read-only)
var age = 21             // mutable
const val MAX = 100      // compile-time constant
val pi: Double = 3.14    // explicit type

// Types: Int, Long, Double, Float, Boolean, Char, String
```

## Null Safety (signature feature)

```kotlin
var nullable: String? = null     // ? = can be null
var nonNull: String = "hi"        // cannot be null

nullable?.length                  // safe call -> null if null
nullable?.length ?: 0             // Elvis operator (default)
nullable!!.length                 // force unwrap (throws if null)
nullable?.let { println(it) }     // run only if non-null
```

## Strings

```kotlin
val s = "Code Craft Club"
s.length; s.uppercase(); s.lowercase()
s.split(" "); s.contains("Craft"); s.replace("Club", "Crew")
s[0]; s.substring(0, 4)
"$name is $age"                    // template
"Sum: ${1 + 1}"                    // expression in template
"42".toInt(); 42.toString()
```

## Control Flow

```kotlin
if (age >= 18) { } else if (age > 12) { } else { }

val status = if (age >= 18) "adult" else "minor"   // expression

when (day) {
    "Sat", "Sun" -> println("weekend")
    in listOf("Mon", "Tue") -> println("early week")
    else -> println("weekday")
}

for (i in 0..4) { }                // inclusive 0..4
for (i in 0 until 5) { }           // exclusive
for (i in 10 downTo 1 step 2) { }
for (item in list) { }
while (cond) { }
```

## Functions

```kotlin
fun add(a: Int, b: Int): Int = a + b           // expression body
fun greet(name: String, greeting: String = "Hi") = "$greeting, $name"
fun sum(vararg nums: Int) = nums.sum()          // varargs

val square = { x: Int -> x * x }                // lambda
greet(name = "Alan")                            // named argument
```

## Collections

```kotlin
val list = listOf(1, 2, 3)            // immutable
val mutable = mutableListOf(1, 2, 3)
mutable.add(4); mutable.removeAt(0)

list.map { it * 2 }                    // it = implicit param
list.filter { it > 1 }
list.reduce { acc, x -> acc + x }
list.first(); list.last(); list.sorted()

val map = mapOf("alan" to 21)
val mmap = mutableMapOf("a" to 1)
mmap["b"] = 2
map["alan"]; map.getOrDefault("x", 0)

val set = setOf(1, 2, 2, 3)            // {1,2,3}
```

## Classes & OOP

```kotlin
class Animal(val name: String, var age: Int = 0) {  // primary constructor
    fun speak() = "$name makes a sound"
}

open class Base(name: String)          // open = inheritable
class Dog(name: String) : Animal(name) {
    fun bark() = "$name barks"
}

// Data class (auto equals/hashCode/toString/copy)
data class User(val name: String, val age: Int)
val u = User("Alan", 21)
val older = u.copy(age = 22)

// Interface
interface Shape { fun area(): Double }
```

## Scope Functions

```kotlin
val len = "hello".let { it.length }            // transform
user.apply { age = 22 }                         // configure, returns receiver
user.also { println(it) }                       // side-effect
with(user) { println(name) }                    // group calls
val result = run { compute() }
```

---

[🔝 Back to README](../README.md)
