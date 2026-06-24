# 🐘 PHP Cheatsheet

> Part of [CodeCraftClub-Cheatsheets](../README.md) · PHP quick reference.

---

## Hello World & Running

```php
<?php
echo "Hello, World!";
```

```bash
php script.php           # run a file
php -S localhost:8000     # built-in dev server
composer install          # install dependencies
```

## Variables & Types

```php
<?php
$name = "Alan";          // string
$age = 21;                // int
$pi = 3.14;               // float
$active = true;           // bool
$nothing = null;          // null
const MAX = 100;          // constant
define("SITE", "CCC");

gettype($age);            // "integer"
(int)"42"; (string)42; (float)"3.5"; (bool)0;
var_dump($age); is_array($x); isset($y);
```

## Strings

```php
$s = "Code Craft Club";
strlen($s); strtoupper($s); strtolower($s);
str_replace("Club", "Crew", $s);
explode(" ", $s);                 // ['Code','Craft','Club']
implode("-", ["a", "b"]);         // "a-b"
substr($s, 0, 4);                 // "Code"
strpos($s, "Craft"); trim("  x  ");
"Hello $name";                     // interpolation (double quotes)
"Sum: " . (1 + 1);                 // concatenation
sprintf("%s is %d", $name, $age);
```

## Arrays

```php
// Indexed
$a = [1, 2, 3];
$a[] = 4; array_push($a, 5); array_pop($a);
count($a); in_array(2, $a); sort($a);

// Associative
$user = ["name" => "Alan", "age" => 21];
$user["name"]; $user["email"] = "a@b.com";
array_keys($user); array_values($user);
isset($user["name"]); $user["x"] ?? "default";

// Functional
array_map(fn($x) => $x * 2, $a);
array_filter($a, fn($x) => $x > 1);
array_reduce($a, fn($c, $x) => $c + $x, 0);
```

## Control Flow

```php
if ($age >= 18) { } elseif ($age > 12) { } else { }

$status = $age >= 18 ? "adult" : "minor";
$email = $user["email"] ?? "n/a";     // null coalescing

switch ($day) {
    case "Sat":
    case "Sun": echo "weekend"; break;
    default: echo "weekday";
}

for ($i = 0; $i < 5; $i++) { }
foreach ($a as $value) { }
foreach ($user as $key => $value) { }
while ($cond) { }
```

## Functions

```php
function add($a, $b = 0) {
    return $a + $b;
}

function sum(...$nums) {               // variadic
    return array_sum($nums);
}

$square = fn($x) => $x * $x;           // arrow function
$double = function ($x) { return $x * 2; };

// Type declarations (PHP 7+)
function greet(string $name): string {
    return "Hi, $name";
}
```

## Classes & OOP

```php
class Animal {
    public string $name;
    private int $age = 0;

    public function __construct(string $name) {
        $this->name = $name;
    }

    public function speak(): string {
        return "{$this->name} makes a sound";
    }
}

class Dog extends Animal {              // inheritance
    public function speak(): string {
        return "{$this->name} barks";
    }
}

$d = new Dog("Rex");
echo $d->speak();
$d instanceof Animal;                   // true

interface Shape {
    public function area(): float;
}
```

## Error Handling

```php
try {
    throw new Exception("bad input");
} catch (Exception $e) {
    echo $e->getMessage();
} finally {
    echo "always runs";
}
```

## Superglobals (Web)

```php
$_GET["id"]; $_POST["name"]; $_SERVER["REQUEST_METHOD"];
$_SESSION["user"]; $_COOKIE["token"]; $_REQUEST;
header("Content-Type: application/json");
echo json_encode(["ok" => true]);
$data = json_decode($input, true);
```

---

[🔝 Back to README](../README.md)
