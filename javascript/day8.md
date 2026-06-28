## Connecting Express to a Database

1. What is This?

Day 37's API stored users in a plain JavaScript array — the moment you restart the server, all that data vanishes. Today fixes that by connecting Express to a real database (MongoDB), so data survives restarts, crashes, and deployments.

An array in memory is a sticky note. A database is a filing cabinet that's still there tomorrow.

1. Why Does It Matter?

Every real app needs data to persist:

User accounts must still exist after you restart the server
Orders, posts, comments — all need permanent storage
This is the final piece connecting your full stack: React (UI) → Express (server) → Database (storage)

We're using MongoDB here because it stores data as JSON-like documents — which fits naturally with JavaScript objects, making it the most common pairing with Node/Express (this combo is often called the "MERN stack": MongoDB, Express, React, Node).

1. The 20% That Covers 80% of Real Work

Setup — install MongoDB driver tools

bashnpm install mongoose

Mongoose is a library that makes working with MongoDB from Node much easier — it adds structure (schemas) on top of MongoDB's flexible documents.

Connecting to MongoDB

javascript// db.js
const mongoose = require("mongoose");

mongoose.connect("mongodb://localhost:27017/myapp")
  .then(() => console.log("MongoDB connected"))
  .catch((err) => console.log("Connection error:", err));

For local development, install MongoDB Community Edition on Mint (sudo apt install mongodb) — or use a free cloud database at MongoDB Atlas and connect with a URL they give you instead of localhost.

Defining a Schema and Model

javascript// models/User.js
const mongoose = require("mongoose");

const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  age: { type: Number, default: 18 },
  createdAt: { type: Date, default: Date.now }
});

const User = mongoose.model("User", userSchema);
module.exports = User;

The schema defines the shape of your data. The model is what you actually use to create, read, update, and delete documents.

CRUD operations with Mongoose

javascriptconst User = require("./models/User");

// CREATE
const newUser = new User({ name: "Harsh", email: "<harsh@example.com>" });
await newUser.save();

// or shorthand:
await User.create({ name: "Harsh", email: "<harsh@example.com>" });

// READ — all users
const users = await User.find();

// READ — with a filter
const adults = await User.find({ age: { $gte: 18 } });

// READ — one specific user
const user = await User.findById(userId);
const userByEmail = await User.findOne({ email: "<harsh@example.com>" });

// UPDATE
await User.findByIdAndUpdate(userId, { age: 23 });

// DELETE
await User.findByIdAndDelete(userId);

Rewriting Day 37's API with a real database

javascriptconst express = require("express");
const mongoose = require("mongoose");
const User = require("./models/User");

const app = express();
app.use(express.json());

mongoose.connect("mongodb://localhost:27017/myapp")
  .then(() => console.log("MongoDB connected"));

// GET all users — now persists across restarts
app.get("/users", async (req, res) => {
  const users = await User.find();
  res.json(users);
});

// GET one user
app.get("/users/:id", async (req, res) => {
  const user = await User.findById(req.params.id);
  if (!user) return res.status(404).json({ error: "Not found" });
  res.json(user);
});

// POST — create a new user
app.post("/users", async (req, res) => {
  try {
    const newUser = await User.create(req.body);
    res.status(201).json(newUser);
  } catch (err) {
    res.status(400).json({ error: err.message });
  }
});

// DELETE
app.delete("/users/:id", async (req, res) => {
  await User.findByIdAndDelete(req.params.id);
  res.status(204).send();
});

app.listen(3000, () => console.log("Server running on port 3000"));

Notice every route handler is now async — database calls take time, just like the fetch calls from Day 33. Same pattern, different direction (server → database instead of browser → server).

Error handling — don't let bad input crash your server

javascriptapp.post("/users", async (req, res) => {
  try {
    const newUser = await User.create(req.body);
    res.status(201).json(newUser);
  } catch (err) {
    // Mongoose validation errors land here (e.g. missing required field)
    res.status(400).json({ error: err.message });
  }
});

Always wrap database calls in try/catch — a missing required field or duplicate unique value will throw, and an uncaught error crashes the whole server.

1. Real-Life Mental Model

ConceptReal EquivalentDatabaseA permanent filing cabinet, survives server restartsSchemaA form template — defines what fields a record must haveModelThe actual filing cabinet you open to add/find/remove records.find()"Show me everything" or "show me everything matching this".findById()"Show me record number X specifically".create()"Add a new record to the cabinet".findByIdAndUpdate()"Find record X, change these fields".findByIdAndDelete()"Find record X, remove it permanently"

The full stack, now complete:

React (Day 35)  →  fetch()  →  Express routes (Day 37)  →  Mongoose  →  MongoDB (Day 38)
   UI/State                      API layer                  ORM           Storage

Data flows down to be saved, and back up to be displayed — this is the complete loop every full-stack app runs on.

Key Takeaway

A database turns your app's data from temporary (gone on restart) to permanent. Mongoose gives you a schema (the shape of your data) and a model (the tool to interact with it) — .find(), .create(), .findByIdAndUpdate(), and .findByIdAndDelete() cover the vast majority of real database operations. Every database call is async, just like fetch — same await + try/catch pattern you already know from Day 33, just facing the other direction.
