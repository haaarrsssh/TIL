## React Basics

1. What is React?

React is a JavaScript library for building user interfaces out of reusable components. Instead of manually selecting and updating DOM elements (Day 32's approach), you describe what the UI should look like for a given state, and React handles updating the actual DOM for you.

Plain JS: "Find this element, change its text." React: "Here's what the UI looks like — you figure out what changed."

1. Why Does It Matter?

Manually managing the DOM (Day 32 style) gets messy fast once an app has many moving pieces — buttons, lists, forms, all updating each other. React solves this by:

Breaking UI into small, reusable components (a button, a card, a whole page)
Automatically re-rendering only what changed when data updates
Being the most in-demand frontend skill in the industry — used by Instagram, Netflix, Airbnb, and most modern web apps

It's also exactly what powers Claude.ai's own interface, and what you'd use for any artifact-style interactive UI.

1. The 20% That Covers 80% of Real Work

A component — just a function that returns UI

jsxfunction Greeting() {
  return <h1>Hello, Harsh!</h1>;
}

// Using it elsewhere
function App() {
  return (
    <div>
      <Greeting />
      <Greeting />
    </div>
  );
}

JSX (<h1>Hello</h1> inside JS) looks like HTML but is actually JavaScript — it gets compiled into regular function calls.

Props — passing data into a component

jsxfunction Greeting(props) {
  return <h1>Hello, {props.name}!</h1>;
}

function App() {
  return (
    <div>
      <Greeting name="Harsh" />
      <Greeting name="Riya" />
    </div>
  );
}

jsx// Destructuring props — the common style
function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
}

Props are read-only — a component never changes its own props, only the parent passing them in.

State — data that can change (useState)

jsximport { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);   // [currentValue, setterFunction]

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Add</button>
      <button onClick={() => setCount(count - 1)}>Subtract</button>
    </div>
  );
}

Calling setCount updates the state AND tells React to re-render the component with the new value. Never change state directly (count = count + 1 ❌) — always use the setter.

Rendering lists

jsxfunction TaskList() {
  const tasks = ["Learn React", "Build a project", "Push to GitHub"];

  return (
    <ul>
      {tasks.map((task, index) => (
        <li key={index}>{task}</li>
      ))}
    </ul>
  );
}

key is required when rendering lists — it helps React track which item is which when the list changes. Use a stable unique ID when you have one (not just the index) — index is fine for static lists.

Conditional rendering

jsxfunction Greeting({ isLoggedIn }) {
  if (isLoggedIn) {
    return <h1>Welcome back!</h1>;
  }
  return <h1>Please log in.</h1>;
}

// Inline conditional with ternary — common in JSX
function Status({ isOnline }) {
  return <p>{isOnline ? "🟢 Online" : "🔴 Offline"}</p>;
}

// && for "render only if true"
function Notification({ hasNewMessage }) {
  return (
    <div>
      {hasNewMessage && <span>You have a new message!</span>}
    </div>
  );
}

Handling events and forms

jsxfunction NameForm() {
  const [name, setName] = useState("");

  const handleSubmit = (event) => {
    event.preventDefault();
    console.log("Submitted:", name);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        value={name}
        onChange={(event) => setName(event.target.value)}
      />
      <button type="submit">Submit</button>
    </form>
  );
}

This is a "controlled input" — the input's value always matches state, and every keystroke updates state via onChange.

useEffect — running code on render / data fetching

jsximport { useState, useEffect } from "react";

function UserProfile() {
  const [user, setUser] = useState(null);

  useEffect(() => {
    // Runs once when the component first renders (empty dependency array)
    fetch("<https://api.github.com/users/octocat>")
      .then((res) => res.json())
      .then((data) => setUser(data));
  }, []);   // <- empty array = run only once

  if (!user) return <p>Loading...</p>;

  return <h2>{user.login}</h2>;
}

The dependency array [] controls when useEffect re-runs: empty = once on mount, [someValue] = re-run when someValue changes, omitted entirely = runs after every render (rarely what you want).

1. Real-Life Mental Model

ConceptReal EquivalentComponentA reusable LEGO brick — same shape, different detailsPropsInstructions handed to the brick from outsideStateThe brick's own memory — changes trigger a repaintuseStateA labeled box: read the current value, call setter to change itJSXHTML-looking syntax that's secretly JavaScriptkey in listsA nametag so React doesn't confuse one item for anotheruseEffect"After this renders, also do this side task (fetch, timer, etc.)"

The mental shift from Day 32's plain JS:

javascript// Plain JS (Day 32 style) — YOU manually update the DOM
button.addEventListener("click", () => {
  count++;
  document.querySelector("#count").textContent = count;
});

// React — YOU just update STATE, React updates the DOM for you
const [count, setCount] = useState(0);
<button onClick={() => setCount(count + 1)}>{count}</button>

Key Takeaway

React components are functions that return UI, props pass data down, and useState holds data that changes over time — every state update triggers React to re-render automatically. The mental shift from Day 32: stop thinking "which element do I update," start thinking "what does my state look like, and let React handle the DOM." useEffect is your hook for anything that needs to happen outside the render itself, like data fetching.
