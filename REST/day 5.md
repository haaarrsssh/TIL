## TypeScript Basics

1. What is TypeScript?

TypeScript is JavaScript with types added on top. You write .ts files instead of .js, declare what type each variable/parameter/return value should be, and the TypeScript compiler catches type errors before your code ever runs — then compiles down to plain JavaScript.

TypeScript is JavaScript with a spell-checker that catches bugs before the browser/Node ever sees your code.

1. Why Does It Matter?

JavaScript's biggest weakness is that it fails silently at runtime:

javascriptfunction add(a, b) { return a + b; }
add(5, "10")   // returns "510" — no error thrown, just wrong

TypeScript catches that at compile time instead. In the real world:

Every serious frontend codebase (React) uses TypeScript
Every serious backend codebase (Node/Express) uses TypeScript
It's the #1 skill gap between junior and mid-level JS developers
VS Code (which you likely use) was built with TypeScript and gives you type hints automatically

Since you already know JavaScript, TypeScript is mostly just adding annotations to code you already know how to write.

1. The 20% That Covers 80% of Real Work

Setup

bashnpm install -g typescript    # install compiler globally
tsc --version                 # verify it's installed

# In a project

npm install typescript --save-dev
npx tsc --init               # generates tsconfig.json

bash# Compile a TS file to JS
tsc script.ts               # creates script.js
tsc --watch                  # recompile on every save

Basic types

typescriptlet name: string = "Harsh";
let age: number = 22;
let isActive: boolean = true;
let anything: any = "avoid this";   // defeats the purpose — use sparingly

// Arrays
let nums: number[] = [1, 2, 3];
let names: string[] = ["Harsh", "Riya"];

// Tuple — fixed-length array with known types
let point: [number, number] = [3, 4];
let entry: [string, number] = ["Harsh", 22];

Type inference — TypeScript is smart

typescript// You DON'T need to annotate everything — TS infers obvious types
let name = "Harsh";       // TS infers: string
let age = 22;              // TS infers: number
let active = true;          // TS infers: boolean

// Only annotate when TS can't infer or when it matters for clarity
function greet(name: string): string {
  return `Hello, ${name}`;
}

Functions — annotate parameters and return type

typescriptfunction add(a: number, b: number): number {
  return a + b;
}

// Optional parameter with ?
function greet(name: string, greeting?: string): string {
  return `${greeting ?? "Hello"}, ${name}!`;
}

// Arrow function with types
const multiply = (a: number, b: number): number => a * b;

// void — function returns nothing
function logMessage(msg: string): void {
  console.log(msg);
}

Interfaces — defining the shape of objects

typescriptinterface User {
  id: number;
  name: string;
  email: string;
  age?: number;      // optional field
}

function printUser(user: User): void {
  console.log(`${user.name} — ${user.email}`);
}

const user: User = { id: 1, name: "Harsh", email: "<harsh@example.com>" };
printUser(user);   // ✅

// TS catches missing required fields immediately
const bad: User = { id: 2, name: "Riya" };   // ❌ Error: missing 'email'

Type aliases — reusable custom types

typescripttype ID = string | number;        // union type — can be either
type Status = "active" | "inactive" | "banned";   // literal union

let userId: ID = 42;
userId = "abc-123";    // ✅ both are valid

let userStatus: Status = "active";
userStatus = "deleted";    // ❌ Error: not one of the allowed values

Interfaces vs Type aliases

typescript// Interface — use for object shapes (preferred for objects)
interface Product {
  id: number;
  name: string;
  price: number;
}

// Type — use for unions, primitives, tuples, or when you need flexibility
type StringOrNumber = string | number;
type Coordinates = [number, number];

Rule of thumb: interface for objects, type for everything else.

TypeScript with classes (OOP from Day 29, upgraded)

typescriptclass BankAccount {
  owner: string;
  private balance: number;    // private — only accessible inside the class

  constructor(owner: string, balance: number) {
    this.owner = owner;
    this.balance = balance;
  }

  deposit(amount: number): void {
    this.balance += amount;
  }

  getBalance(): number {
    return this.balance;
  }
}

const account = new BankAccount("Harsh", 1000);
account.deposit(500);
console.log(account.getBalance());   // 1500
// account.balance  ❌ Error — balance is private

Generics — one function that works for any type

typescript// Without generics — only works for numbers
function firstItem(arr: number[]): number {
  return arr[0];
}

// With generics — works for any type
function firstItem<T>(arr: T[]): T {
  return arr[0];
}

firstItem([1, 2, 3]);          // returns number
firstItem(["a", "b", "c"]);    // returns string
firstItem([true, false]);        // returns boolean

1. Real-Life Mental Model

ConceptReal EquivalentType annotationA label on a box saying exactly what goes inside itType inferenceTS reading the label you didn't write because it's obviousinterfaceA contract — "this object must have these fields"typeA nickname for a type, including unions and combosprivateA locked drawer — only the class itself can open itGenerics <T>A wildcard — "works with any type, but consistently"Compile-time errorFinding a typo before you submit, not after it breaks prod

The JS → TS migration mindset:

typescript// JavaScript
function getUser(id) {
  return fetch(`/users/${id}`).then(res => res.json());
}

// TypeScript — same function, just honest about what goes in and comes out
interface User {
  id: number;
  name: string;
  email: string;
}

async function getUser(id: number): Promise<User> {
  const res = await fetch(`/users/${id}`);
  return res.json();
}

You're not rewriting the logic — you're just making the contracts explicit.

Key Takeaway

TypeScript is JavaScript that tells the truth about its types. The core toolkit: annotate function parameters and return types, use interface for object shapes, type for unions and custom aliases, and private to enforce encapsulation in classes. Type inference means you annotate less than you'd think — TypeScript fills in obvious types automatically. The payoff: the compiler catches mismatches before runtime, which means fewer bugs, better autocomplete, and code that's dramatically easier to read and maintain.
