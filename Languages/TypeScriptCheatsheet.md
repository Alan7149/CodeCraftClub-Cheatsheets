# 🔷 TypeScript Cheatsheet

> Part of [CodeCraftClub-Cheatsheets](../README.md) · TypeScript quick reference.

---

## Setup

```bash
npm install -D typescript
npx tsc --init           # create tsconfig.json
npx tsc                  # compile to JS
npx tsc --watch          # recompile on change
npx ts-node app.ts       # run directly
```

## Basic Types

```typescript
let name: string = "Alan";
let age: number = 21;
let active: boolean = true;
let nothing: null = null;
let u: undefined = undefined;
let anything: any = "avoid";        // disables checking
let unknownVal: unknown;            // safer than any
let tuple: [string, number] = ["a", 1];
let list: number[] = [1, 2, 3];
let list2: Array<string> = ["a"];
```

## Functions

```typescript
function add(a: number, b: number): number {
  return a + b;
}

const greet = (name: string, greeting = "Hi"): string =>
  `${greeting}, ${name}`;

function log(msg: string): void {}          // returns nothing

function build(name: string, age?: number) {}  // optional param

function sum(...nums: number[]): number {       // rest
  return nums.reduce((a, b) => a + b, 0);
}
```

## Interfaces & Types

```typescript
interface User {
  name: string;
  age: number;
  email?: string;            // optional
  readonly id: number;       // can't reassign
}

type Point = { x: number; y: number };

// Union & intersection
type Status = "active" | "inactive" | "pending";
type Admin = User & { role: string };

const u: User = { name: "Alan", age: 21, id: 1 };
```

## Type Aliases & Literals

```typescript
type ID = string | number;
type Direction = "N" | "S" | "E" | "W";

let dir: Direction = "N";    // only these 4 allowed
let id: ID = 42;
```

## Generics

```typescript
function identity<T>(value: T): T {
  return value;
}
identity<string>("hi");
identity(42);                 // type inferred

interface Box<T> {
  value: T;
}
const b: Box<number> = { value: 5 };

function first<T>(arr: T[]): T | undefined {
  return arr[0];
}
```

## Classes

```typescript
class Animal {
  private name: string;
  protected age: number = 0;
  readonly species: string;

  constructor(name: string, species: string) {
    this.name = name;
    this.species = species;
  }

  speak(): string {
    return `${this.name} makes a sound`;
  }
}

class Dog extends Animal {
  constructor(name: string) {
    super(name, "dog");
  }
  speak(): string { return "Woof"; }
}

// Implementing an interface
interface Shape { area(): number; }
class Circle implements Shape {
  constructor(private r: number) {}
  area() { return Math.PI * this.r ** 2; }
}
```

## Enums

```typescript
enum Role { Admin, User, Guest }     // 0, 1, 2
enum Color { Red = "red", Green = "green" }
let r: Role = Role.Admin;
```

## Type Narrowing & Guards

```typescript
function process(x: string | number) {
  if (typeof x === "string") {
    return x.toUpperCase();    // x is string here
  }
  return x.toFixed(2);          // x is number here
}

// Non-null assertion
const el = document.querySelector(".btn")!;

// Type assertion
const input = el as HTMLInputElement;
```

## Utility Types

```typescript
Partial<User>      // all props optional
Required<User>     // all props required
Readonly<User>     // all props readonly
Pick<User, "name" | "age">     // subset
Omit<User, "id">               // exclude
Record<string, number>         // { [k: string]: number }
ReturnType<typeof add>         // infer return type
```

---

[🔝 Back to README](../README.md)
