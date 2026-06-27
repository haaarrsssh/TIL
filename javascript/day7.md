## Node.js & Express Basics

1. What Are They?

Node.js lets you run JavaScript outside the browser — on your own machine or a server. Express is a minimal framework built on top of Node that makes it easy to build web servers and APIs without writing raw networking code.

Browser JS reacts to clicks. Node.js + Express JS reacts to requests — from anyone, anywhere, hitting your server.

1. Why Does It Matter?

Up to now, your JS and React skills have only handled the frontend — what users see and click. Node + Express is the missing half: the backend — where data actually lives, gets validated, stored, and served.

With this, you can finally:

Build the API that your React app fetches data from (closing the loop from Day 33's fetch calls)
Use one language (JavaScript) for your entire app — frontend and backend
Understand what's actually running when you deploy a "full-stack" project

This is the single biggest unlock for going from "I can build a UI" to "I can build a full app."

1. The 20% That Covers 80% of Real Work

Running JavaScript with Node (outside the browser)

bashnode --version          # confirm it's installed
node script.js          # run a JS file directly from the terminal
node                    # opens an interactive JS shell (like Python's REPL)

javascript// script.js — plain Node, no Express yet
console.log("Hello from Node!");

// Node gives you things the browser doesn't have, like file access
const fs = require("fs");
fs.writeFileSync("notes.txt", "Day 37 of #100DaysOfCode");

Setting up a project

bashmkdir my-api && cd my-api
npm init -y              # creates package.json
npm install express       # installs Express, adds to package.json

json// package.json — what gets created
{
  "name": "my-api",
  "dependencies": {
    "express": "^4.19.2"
  }
}

Your first Express server

javascript// server.js
const express = require("express");
const app = express();
const PORT = 3000;

app.get("/", (req, res) => {
  res.send("Hello, Harsh!");
});

app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});

bashnode server.js

# Visit <http://localhost:3000> in your browser → see "Hello, Harsh!"

The core pattern — routes

javascriptapp.get("/", (req, res) => {
  res.send("Welcome!");
});

app.get("/users", (req, res) => {
  res.json([{ name: "Harsh" }, { name: "Riya" }]);   // send JSON, not text
});

// Route with a URL parameter
app.get("/users/:id", (req, res) => {
  const id = req.params.id;          // grabs the value from the URL
  res.json({ id, name: "Harsh" });
});

app.post("/users", (req, res) => {
  console.log(req.body);              // the data sent in the request
  res.status(201).json({ message: "User created" });
});

HTTP MethodUsed ForGETReading dataPOSTCreating new dataPUTUpdating existing dataDELETERemoving data

Middleware — code that runs between request and response

javascript// REQUIRED to read JSON from incoming requests (like POST bodies)
app.use(express.json());

// Custom middleware — runs on EVERY request, in order
app.use((req, res, next) => {
  console.log(`${req.method} ${req.url}`);
  next();    // MUST call next() or the request hangs forever
});

// Middleware only on a specific route
app.get("/admin", checkAuth, (req, res) => {
  res.send("Welcome, admin");
});

function checkAuth(req, res, next) {
  if (req.headers.authorization === "secret123") {
    next();                  // allowed — continue to the route
  } else {
    res.status(401).send("Unauthorized");    // blocked — stop here
  }
}

A small but complete CRUD API

javascriptconst express = require("express");
const app = express();
app.use(express.json());

let users = [{ id: 1, name: "Harsh" }];

app.get("/users", (req, res) => {
  res.json(users);
});

app.get("/users/:id", (req, res) => {
  const user = users.find(u => u.id === parseInt(req.params.id));
  if (!user) return res.status(404).json({ error: "Not found" });
  res.json(user);
});

app.post("/users", (req, res) => {
  const newUser = { id: users.length + 1, name: req.body.name };
  users.push(newUser);
  res.status(201).json(newUser);
});

app.delete("/users/:id", (req, res) => {
  users = users.filter(u => u.id !== parseInt(req.params.id));
  res.status(204).send();
});

app.listen(3000, () => console.log("Server running on port 3000"));

Connecting this to your React knowledge (Day 33 closes the loop)

javascript// In your React app — fetching from YOUR OWN Express server now
async function loadUsers() {
  const response = await fetch("<http://localhost:3000/users>");
  const users = await response.json();
  console.log(users);
}

This is the exact same fetch pattern from Day 33 — except now you control both ends. The frontend (React) talks to the backend (Express) you just built.

1. Real-Life Mental Model

ConceptReal EquivalentNode.jsJavaScript with permission to leave the browserExpressA receptionist that routes visitors to the right roomapp.get("/users", ...)"When someone asks for /users, do THIS"reqThe incoming request — what they're asking forresYour reply — what you send backMiddlewareA security checkpoint everyone passes through firstnext()"Let them through to the next checkpoint/destination"

The request/response cycle:

Client (React/browser) → sends request → Express route matches it
   → middleware runs (if any) → route handler runs → res.send()/res.json()
   → Client receives the response

Key Takeaway

Node.js runs JavaScript outside the browser; Express turns that into a web server with minimal code. The core loop is: define a route (app.get/app.post/etc.) → read the request (req.params, req.body) → send a response (res.json, res.status). Middleware like express.json() and custom auth checks run before your route logic. This is the backend your Day 33 fetch calls and Day 35 React app were always meant to talk to — you can now build both halves yourself.
