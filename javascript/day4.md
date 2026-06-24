## JavaScript Recap & Practice Problems

1. What Is This?

Three days of JavaScript — distilled into one reference sheet, followed by 10 practice problems mixing everything together. Same active-recall format as Days 20 and 30.

Syntax fades. Patterns stick. Today locks in the patterns.

1. The Full JavaScript Mental Map (Days 31–33)

DayTopicThe One Thing to Remember31JS Basicsconst by default, === always, arrays/objects mirror Python lists/dicts32DOM & Eventsselect → listen → react: querySelector → addEventListener → change DOM33Async JSawait pauses inside async functions; fetch doesn't reject on 404 — check .ok

1. 10 Practice Problems — Write the Code

Easy

P1. Write a function isEven(n) that returns true if n is even.

P2. Given an array of numbers, return a new array with only the even numbers (use .filter()).

P3. Write a function reverseString(s) that reverses a string without using .reverse() directly on a string (strings aren't arrays in JS).

P4. Select all <li> elements on a page and log their text content.

Medium

P5. Write a function capitalize(str) that capitalizes the first letter of each word in a sentence.

P6. Given an array of objects [{name: "Harsh", age: 22}, ...], return just the names of people older than 20, sorted alphabetically.

P7. Add a click event listener to a button that toggles a "dark-mode" class on the <body>.

P8. Write an async function getPost(id) that fetches <https://jsonplaceholder.typicode.com/posts/{id}> and returns just the title.

Hard

P9. Write a function findDuplicates(arr) that returns an array of numbers that appear more than once (use a Set or object to track seen values).

P10. Write an async function getMultiplePosts(ids) that fetches several post IDs in parallel using Promise.all and returns an array of their titles.

1. Solutions

<details>
<summary>Click to reveal — try first!</summary>
javascript// P1
function isEven(n) {
  return n % 2 === 0;
}

// P2
function evensOnly(nums) {
  return nums.filter(n => n % 2 === 0);
}

// P3
function reverseString(s) {
  let result = "";
  for (const char of s) {
    result = char + result;
  }
  return result;
}

// P4
const items = document.querySelectorAll("li");
items.forEach(item => console.log(item.textContent));

// P5
function capitalize(str) {
  return str
    .split(" ")
    .map(word => word.charAt(0).toUpperCase() + word.slice(1))
    .join(" ");
}

// P6
function namesAbove20(people) {
  return people
    .filter(p => p.age > 20)
    .map(p => p.name)
    .sort();
}

// P7
const toggleButton = document.querySelector("#themeToggle");
toggleButton.addEventListener("click", () => {
  document.body.classList.toggle("dark-mode");
});

// P8
async function getPost(id) {
  const response = await fetch(`https://jsonplaceholder.typicode.com/posts/${id}`);
  const data = await response.json();
  return data.title;
}

// P9
function findDuplicates(arr) {
  const seen = new Set();
  const duplicates = new Set();

  for (const num of arr) {
    if (seen.has(num)) {
      duplicates.add(num);
    }
    seen.add(num);
  }
  return [...duplicates];   // spread Set into an array
}

// P10
async function getMultiplePosts(ids) {
  const requests = ids.map(id =>
    fetch(`https://jsonplaceholder.typicode.com/posts/${id}`).then(r => r.json())
  );
  const posts = await Promise.all(requests);
  return posts.map(post => post.title);
}

</details>

1. Pattern → Problem Map (The Real 80/20)

If the problem says...Reach for..."filter items matching a condition".filter()"transform every item".map()"combine into a single value (sum, total)".reduce()"sort by some rule".sort((a, b) => ...)"track duplicates / things already seen"new Set()"react to a user action"addEventListener"get data from a server"fetch + async/await"do several independent fetches at once"Promise.all()"this object knows about itself" (rare in plain JS)arrow functions for callbacks, regular function for methods needing this

1. JS vs Python — Side by Side Cheat Sheet

TaskPythonJavaScriptVariablename = "Harsh"const name = "Harsh";List/Array filter[x for x in nums if x > 0]nums.filter(x => x > 0)List/Array transform[x**2 for x in nums]nums.map(x => x** 2)Dict/Object lookupuser["name"]user.name or user["name"]String formattingf"Hello {name}"`Hello ${name}`Functiondef greet(name): return ...const greet = (name) => ...

Key Takeaway

The JS array methods (map, filter, reduce, sort) and async pattern (async/await + fetch + Promise.all) cover the vast majority of real frontend work. The DOM trio — select, listen, react — is the loop behind every interactive feature. With Python already in your toolkit, JS mostly means learning new syntax for concepts you already understand.
