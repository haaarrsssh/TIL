## JavaScript Basics

1. What is JavaScript?

JavaScript (JS) is the programming language that runs in every web browser — it's what makes websites interactive (clicks, animations, form validation, live updates). With Node.js, it also runs outside the browser, on servers.

HTML is the skeleton, CSS is the skin, JavaScript is the nervous system — it makes things actually react.

1. Why Does It Matter?

JavaScript is the only language that runs natively in every browser — no exceptions. It powers:

Every interactive website (forms, dropdowns, live search, animations)
Full backend servers via Node.js (Express, used by huge companies)
Mobile apps (React Native) and desktop apps (Electron — VS Code itself is built with it)

If SQL is how you talk to databases and Python is how you script/automate, JavaScript is how you make things people actually click on come alive.

1. The 20% That Covers 80% of Real Work

Variables

javascriptlet age = 22;            // can be reassigned
const name = "Harsh";    // cannot be reassigned
var old = "avoid this";  // old syntax — don't use in new code

age = 23;                // ✅ works
// name = "Riya";        // ❌ error — const cannot change

Data types

javascriptlet str = "hello";          // string
let num = 42;                 // number (no separate int/float)
let isTrue = true;            // boolean
let nothing = null;           // intentional empty value
let notSet;                   // undefined — declared, no value yet
let arr = [1, 2, 3];           // array
let obj = { name: "Harsh" };  // object

console.log(typeof num);       // "number"

Template literals — like Python's f-strings

javascriptconst name = "Harsh";
const day = 31;

console.log(`Day ${day} — written by ${name}`);
// Day 31 — written by Harsh

// Multi-line strings, easy with backticks
const message = `Hello ${name},
Welcome to Day ${day}.`;

Arrays — Python list equivalent

javascriptlet nums = [1, 2, 3];
nums.push(4);              // add to end → [1, 2, 3, 4]
nums.pop();                 // remove last → [1, 2, 3]
nums[0] = 99;                // change item → [99, 2, 3]

console.log(nums.length);    // 3

// map, filter, reduce — JS's version of Python comprehensions
let squared = nums.map(n => n * n);            // [9801, 4, 9]
let evens = nums.filter(n => n % 2 === 0);      // [2]
let sum = nums.reduce((acc, n) => acc + n, 0);  // sum starting from 0

Objects — Python dict equivalent

javascriptconst user = {
  name: "Harsh",
  age: 22,
  isActive: true
};

console.log(user.name);          // dot notation
console.log(user["age"]);        // bracket notation

user.city = "Delhi";              // add new key
delete user.isActive;             // remove key

// Destructuring — pull values out directly
const { name, age } = user;
console.log(name, age);           // Harsh 22

Conditionals

javascriptlet age = 20;

if (age >= 18) {
  console.log("Adult");
} else if (age >= 13) {
  console.log("Teenager");
} else {
  console.log("Child");
}

// Ternary
const status = age >= 18 ? "Adult" : "Minor";

// === vs ==  — ALWAYS use === (strict equality)
console.log(5 === "5");    // false — different types
console.log(5 == "5");     // true  — converts type first (avoid this)

Loops

javascript// for loop
for (let i = 0; i < 5; i++) {
  console.log(i);
}

// for...of — iterate over array values
for (const num of [10, 20, 30]) {
  console.log(num);
}

// while loop
let count = 0;
while (count < 5) {
  console.log(count);
  count++;
}

Functions — 3 ways to write them

javascript// Regular function
function greet(name) {
  return `Hello, ${name}!`;
}

// Function expression
const greet2 = function(name) {
  return `Hello, ${name}!`;
};

// Arrow function — most common in modern JS
const greet3 = (name) => {
  return `Hello, ${name}!`;
};

// Arrow function shorthand — single expression, implicit return
const greet4 = name => `Hello, ${name}!`;

console.log(greet4("Harsh"));   // Hello, Harsh!

Working with the DOM (browser only)

javascript// Select an element
const button = document.querySelector("#myButton");

// React to a click
button.addEventListener("click", () => {
  console.log("Button clicked!");
});

// Change content
document.querySelector("h1").textContent = "Updated!";

1. Real-Life Mental Model

JS ConceptPython Equivalentlet / constregular variable assignmentArray []List []Object {}Dict {}=> arrow functionlambda (for short ones).map() / .filter()list comprehensionTemplate literal `f-string f""===== in Python (Python doesn't have JS's loose ==)null / undefinedNone

The one rule that prevents 90% of beginner bugs:

javascript// ALWAYS use const by default, let only when reassigning, never var
const PI = 3.14;     // won't change → const
let score = 0;        // will change → let
score = score + 10;    // fine

// ALWAYS use === and !==, never == or !=
if (value === null) { }   // ✅
if (value == null) { }    // ❌ avoid — type coercion surprises

Key Takeaway

JavaScript's core toolkit mirrors Python's closely: variables, arrays/objects instead of lists/dicts, and arrow functions instead of lambdas — but it's the language the browser actually understands. Default to const, use === always, and map/filter/reduce are your array superpowers, just like list comprehensions in Python.
