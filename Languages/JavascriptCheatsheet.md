# 🟨 JavaScript Cheatsheet

> Part of [CodeCraftClub-Cheatsheets](../README.md) · Modern (ES6+) JavaScript quick reference.

---

## Variables

```js
const PI = 3.14;     // block-scoped, can't reassign
let count = 0;        // block-scoped, reassignable
var old = "avoid";   // function-scoped (legacy)
```

## Types & Checks

```js
typeof 42            // "number"
typeof "hi"          // "string"
typeof true          // "boolean"
typeof undefined     // "undefined"
typeof null          // "object" (historic quirk)
Array.isArray([])    // true
Number("3"); String(3); Boolean(0); parseInt("42px"); parseFloat("3.5");
```

## Strings (Template Literals)

```js
const name = "Alan";
`Hello ${name}, ${1 + 1}`        // interpolation
"abc".toUpperCase().includes("A");
"a,b,c".split(",");               // ['a','b','c']
"  hi  ".trim(); "abc".slice(0, 2); "abc".replace("a", "x");
"abc".padStart(5, "0");           // "00abc"
```

## Numbers & Math

```js
Math.max(1, 9); Math.min(1, 9); Math.round(2.6); Math.floor(2.9);
Math.ceil(2.1); Math.abs(-3); Math.sqrt(16); Math.random();
(255).toString(16);               // "ff"
(1234.567).toFixed(2);            // "1234.57"
```

## Arrays

```js
const a = [1, 2, 3];
a.push(4); a.pop(); a.shift(); a.unshift(0);
a.map(x => x * 2);                // [2,4,6]
a.filter(x => x > 1);             // [2,3]
a.reduce((sum, x) => sum + x, 0); // 6
a.find(x => x === 2); a.includes(2); a.indexOf(2);
a.sort((x, y) => y - x);          // descending
[...a, ...[5, 6]];                // spread / concat
a.forEach(x => console.log(x));
```

## Objects

```js
const user = { name: "Alan", age: 21 };
user.name; user["age"];
const { name, age } = user;       // destructuring
const copy = { ...user, age: 22 };// spread + override
Object.keys(user); Object.values(user); Object.entries(user);
user.email ??= "n/a";             // assign if null/undefined
```

## Control Flow

```js
if (age >= 18) { /* ... */ } else if (age > 12) {} else {}

const status = age >= 18 ? "adult" : "minor";   // ternary

switch (day) {
  case "Sat":
  case "Sun": return "weekend";
  default: return "weekday";
}

for (let i = 0; i < 3; i++) {}
for (const x of [1, 2, 3]) {}      // values
for (const k in user) {}           // keys
let i = 0; while (i < 3) i++;
```

## Functions

```js
function add(a, b = 0) { return a + b; }   // default param
const mul = (a, b) => a * b;                // arrow function
const log = msg => console.log(msg);        // single param
const sum = (...nums) => nums.reduce((s, n) => s + n, 0);  // rest

// IIFE
(() => console.log("runs now"))();
```

## Truthy / Falsy

```js
// Falsy: false, 0, "", null, undefined, NaN
value || "default";    // fallback if falsy
value ?? "default";    // fallback only if null/undefined
obj?.prop?.method();   // optional chaining
```

## Classes

```js
class Animal {
  constructor(name) { this.name = name; }
  speak() { return `${this.name} makes a sound`; }
  static create(n) { return new Animal(n); }
}

class Dog extends Animal {
  speak() { return `${this.name} barks`; }
}
const d = new Dog("Rex");
d instanceof Animal;   // true
```

## Async (Promises & async/await)

```js
fetch("/api/data")
  .then(res => res.json())
  .then(data => console.log(data))
  .catch(err => console.error(err));

async function load() {
  try {
    const res = await fetch("/api/data");
    const data = await res.json();
    return data;
  } catch (e) {
    console.error(e);
  }
}

await Promise.all([p1, p2]);   // wait for all
```

## DOM (Browser)

```js
document.querySelector(".btn");
document.querySelectorAll("li");
el.addEventListener("click", e => { /* ... */ });
el.textContent = "Hi"; el.classList.add("active");
el.setAttribute("data-id", "5");
const node = document.createElement("div");
parent.appendChild(node);
```

## Modules (ES6)

```js
// math.js
export const add = (a, b) => a + b;
export default function () {}

// main.js
import defaultFn, { add } from "./math.js";
```

---

[🔝 Back to README](../README.md)
