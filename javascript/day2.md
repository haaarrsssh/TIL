## DOM Manipulation & Events

1. What is the DOM?

The DOM (Document Object Model) is how JavaScript "sees" an HTML page — every tag becomes a JavaScript object you can select, read, and change. Events are actions the user takes (click, type, scroll) that your JS code can listen for and react to.

The DOM is the browser's live, editable map of your HTML. JavaScript is the hand that redraws it.

1. Why Does It Matter?

This is literally what makes websites interactive instead of static:

Click a button → something happens (DOM manipulation + events)
Type in a search box → results filter live
Submit a form → validate before sending
Like button → count updates instantly without reloading the page

Every modern framework (React, Vue) is ultimately just a smarter way of doing DOM manipulation. Understanding raw DOM/events first means frameworks will make sense, not feel like magic.

1. The 20% That Covers 80% of Real Work

Selecting elements

javascript// Modern way — use these
document.querySelector("#myId");          // selects by ID
document.querySelector(".myClass");       // selects FIRST match by class
document.querySelectorAll(".myClass");    // selects ALL matches (returns a list-like NodeList)
document.querySelector("button");         // selects first <button>

// Older ways — still seen in legacy code
document.getElementById("myId");
document.getElementsByClassName("myClass");

Reading and changing content

javascriptconst heading = document.querySelector("h1");

heading.textContent = "New Title";        // plain text — safe, preferred
heading.innerHTML = "<b>Bold Title</b>";  // can include HTML tags — use carefully

const input = document.querySelector("#nameInput");
console.log(input.value);                  // read what user typed
input.value = "Harsh";                       // set the input's value

Changing styles and classes

javascriptconst box = document.querySelector(".box");

box.style.color = "blue";              // inline style — quick but not ideal for big changes
box.style.backgroundColor = "yellow";

box.classList.add("active");            // add a CSS class
box.classList.remove("hidden");          // remove a CSS class
box.classList.toggle("dark-mode");       // add if missing, remove if present
box.classList.contains("active");         // check if class exists → true/false

Creating and removing elements

javascript// Create a new element
const newItem = document.createElement("li");
newItem.textContent = "New task";

// Add it to the page
const list = document.querySelector("#taskList");
list.appendChild(newItem);          // add to the end
list.prepend(newItem);               // add to the beginning

// Remove an element
newItem.remove();

Events — the core pattern

javascriptconst button = document.querySelector("#myButton");

button.addEventListener("click", function() {
  console.log("Button was clicked!");
});

// Arrow function version (more common in modern JS)
button.addEventListener("click", () => {
  console.log("Clicked!");
});

// The event object — gives info about what happened
button.addEventListener("click", (event) => {
  console.log(event.target);       // the element that triggered the event
});

Common event types

javascript// Click
button.addEventListener("click", () => console.log("clicked"));

// Typing in an input — fires on every keystroke
input.addEventListener("input", (event) => {
  console.log(event.target.value);
});

// Form submission — MUST preventDefault to stop page reload
form.addEventListener("submit", (event) => {
  event.preventDefault();
  console.log("Form submitted without reloading the page");
});

// Mouse hover
box.addEventListener("mouseenter", () => box.classList.add("highlight"));
box.addEventListener("mouseleave", () => box.classList.remove("highlight"));

// Page fully loaded
document.addEventListener("DOMContentLoaded", () => {
  console.log("Page is ready, DOM fully loaded");
});

Real-world pattern — a simple to-do list

javascriptconst input = document.querySelector("#taskInput");
const addButton = document.querySelector("#addButton");
const list = document.querySelector("#taskList");

addButton.addEventListener("click", () => {
  if (input.value.trim() === "") return;   // ignore empty input

  const li = document.createElement("li");
  li.textContent = input.value;

  // Click a task to remove it
  li.addEventListener("click", () => li.remove());

  list.appendChild(li);
  input.value = "";    // clear input after adding
});

1. Real-Life Mental Model

ConceptReal EquivalentquerySelectorPointing at a specific item in a roomtextContent / innerHTMLChanging the label on somethingclassList.add/remove/toggleChanging an outfit on/offcreateElement + appendChildBuilding something new and placing it on a shelfaddEventListener"Watch for this action, then do something"event.preventDefault()"Stop the default thing from happening automatically"event.target"Which exact thing triggered this?"

The pattern behind almost every interactive feature:

1. SELECT the element        → querySelector
2. LISTEN for an event        → addEventListener
3. REACT by changing the DOM  → textContent / classList / createElement

Every button click, form, toggle, or live update you've ever seen on a website follows this exact 3-step loop.

Key Takeaway

The DOM is JavaScript's live view of your HTML — querySelector finds elements, textContent/classList/createElement change them, and addEventListener reacts to what users do. The select → listen → react loop is the foundation every interactive website (and every JS framework underneath) is built on.
