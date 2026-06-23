## Async JS: Promises, Fetch & Async/Await

1. What is Async JavaScript?

JavaScript runs one thing at a time (single-threaded), but some tasks take time — fetching data from an API, reading a file, waiting on a timer. Asynchronous code lets JS start a slow task, keep running other code while it waits, and handle the result whenever it's ready — without freezing the page.

Sync code: stand in line and wait. Async code: take a number, go sit down, get called when it's ready.

1. Why Does It Matter?

Almost every real web app talks to a server:

Loading a user's profile from an API
Submitting a form and waiting for a response
Fetching live data (weather, stock prices, search results)

Without async handling, the entire page would freeze while waiting for the network. This is one of THE most important JS concepts for any real-world app — and a very common interview topic.

1. The 20% That Covers 80% of Real Work

Callbacks — the old way (know it, don't write it)

javascriptfunction getUser(id, callback) {
  setTimeout(() => {
    callback({ id, name: "Harsh" });
  }, 1000);
}

getUser(1, (user) => {
  console.log(user);   // runs after 1 second
});
// Nesting callbacks gets messy fast — "callback hell" — Promises fixed this

Promises — represents a future value

javascriptconst promise = new Promise((resolve, reject) => {
  const success = true;

  setTimeout(() => {
    if (success) {
      resolve("Data loaded!");      // task succeeded
    } else {
      reject("Something went wrong"); // task failed
    }
  }, 1000);
});

promise
  .then((result) => console.log(result))     // runs on resolve
  .catch((error) => console.log(error))       // runs on reject
  .finally(() => console.log("Done either way"));

A promise has 3 states:

pending    → still waiting
fulfilled  → resolve() was called, .then() runs
rejected   → reject() was called, .catch() runs

fetch() — making real API calls

javascriptfetch("<https://api.github.com/users/octocat>")
  .then((response) => response.json())   // parse the JSON body
  .then((data) => console.log(data))       // use the actual data
  .catch((error) => console.log("Error:", error));

javascript// POST request with fetch
fetch("<https://api.example.com/users>", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ name: "Harsh", age: 22 })
})
  .then((response) => response.json())
  .then((data) => console.log(data));

async/await — the modern, readable way

javascript// Same fetch as above, written with async/await
async function getUser() {
  const response = await fetch("<https://api.github.com/users/octocat>");
  const data = await response.json();
  console.log(data);
}

getUser();

async marks a function as asynchronous. await pauses execution until the promise resolves — but only inside an async function. This reads almost exactly like synchronous code.

Error handling with async/await — try/catch

javascriptasync function getUser() {
  try {
    const response = await fetch("<https://api.github.com/users/octocat>");
    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.log("Something went wrong:", error.message);
  } finally {
    console.log("Request finished");
  }
}

Important: fetch doesn't reject on 404/500 errors — only on network failure. You must check response.ok yourself.

Running multiple async tasks together

javascript// Sequential — each waits for the previous (slower if unrelated)
async function sequential() {
  const user = await fetch("/api/user").then(r => r.json());
  const posts = await fetch("/api/posts").then(r => r.json());
  console.log(user, posts);
}

// Parallel — both start at once (faster, use when tasks don't depend on each other)
async function parallel() {
  const [user, posts] = await Promise.all([
    fetch("/api/user").then(r => r.json()),
    fetch("/api/posts").then(r => r.json())
  ]);
  console.log(user, posts);
}

Real-world pattern — loading data into the DOM

javascriptasync function loadUsers() {
  const list = document.querySelector("#userList");
  list.textContent = "Loading...";

  try {
    const response = await fetch("<https://api.github.com/users>");
    const users = await response.json();

    list.innerHTML = "";   // clear "Loading..."
    users.forEach((user) => {
      const li = document.createElement("li");
      li.textContent = user.login;
      list.appendChild(li);
    });
  } catch (error) {
    list.textContent = "Failed to load users.";
  }
}

loadUsers();

1. Real-Life Mental Model

ConceptReal EquivalentSynchronous codeStanding in line, one task at a timePromiseA receipt for food you ordered — not ready yet, but coming.then()"When it's ready, do this".catch()"If it goes wrong, do this instead"async functionA function that's allowed to "wait" without freezing everythingawait"Pause here until this specific thing finishes"Promise.all()Ordering 3 dishes at once instead of one at a time

The translation cheat sheet — old vs new:

javascript// Promise chain
fetch(url).then(res => res.json()).then(data => console.log(data));

// Same thing, async/await (always prefer this for readability)
async function load() {
  const res = await fetch(url);
  const data = await res.json();
  console.log(data);
}

Key Takeaway

Async JS exists because network requests take time and JS can't afford to freeze while waiting. Promises represent "a value that will exist eventually" with 3 states (pending/fulfilled/rejected). async/await is syntactic sugar over promises that reads like normal synchronous code — use it by default, wrap it in try/catch for errors, and reach for Promise.all() when running independent requests in parallel.
