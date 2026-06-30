## Backend Recap & Practice

1. What Is This?

Four days of backend development — Node, Express, MongoDB, and REST design — distilled into one reference sheet, followed by a hands-on build-from-scratch exercise. Same active-recall format as Days 20, 30, and 34. This also marks Day 40 of #100DaysOfCode — 40% of the way there.

The first 39 days built the pieces. Today proves you can assemble them without a guide.

1. The Full Backend Mental Map (Days 37–39)

DayTopicThe One Thing to Remember37Node.js & Expressapp.get/post/put/delete + req/res is the entire routing pattern38Express + MongoDBSchema = shape of data, Model = tool to interact with it; every DB call is async39REST API DesignURLs are nouns, HTTP methods are verbs, status codes must tell the truth

1. The Build — A Complete "Notes" API From Scratch

Instead of 10 isolated problems, today is one cumulative build — write a full REST API for a notes app, applying everything from this week.

Requirements

A Note model with fields: title (required), content (required), pinned (boolean, default false), createdAt (default to now)
GET /api/v1/notes — return all notes, support ?pinned=true filtering
GET /api/v1/notes/:id — return one note, 404 if not found
POST /api/v1/notes — create a note, validate title and content are present, 400 if missing
PATCH /api/v1/notes/:id — update any subset of fields on a note
DELETE /api/v1/notes/:id — delete a note, return 204
All responses follow { data: ... } or { error: ... } shape

1. Solution

<details>
<summary>Click to reveal — try building it yourself first!</summary>
javascript// models/Note.js
const mongoose = require("mongoose");

const noteSchema = new mongoose.Schema({
  title: { type: String, required: true },
  content: { type: String, required: true },
  pinned: { type: Boolean, default: false },
  createdAt: { type: Date, default: Date.now }
});

module.exports = mongoose.model("Note", noteSchema);

javascript// server.js
const express = require("express");
const mongoose = require("mongoose");
const Note = require("./models/Note");

const app = express();
app.use(express.json());

mongoose.connect("mongodb://localhost:27017/notesapp")
  .then(() => console.log("MongoDB connected"))
  .catch((err) => console.log("Connection error:", err));

// GET all notes, optional ?pinned=true filter
app.get("/api/v1/notes", async (req, res) => {
  const filter = {};
  if (req.query.pinned !== undefined) {
    filter.pinned = req.query.pinned === "true";
  }
  const notes = await Note.find(filter);
  res.status(200).json({ data: notes });
});

// GET one note
app.get("/api/v1/notes/:id", async (req, res) => {
  const note = await Note.findById(req.params.id);
  if (!note) return res.status(404).json({ error: "Note not found" });
  res.status(200).json({ data: note });
});

// POST — create a note
app.post("/api/v1/notes", async (req, res) => {
  const { title, content } = req.body;
  if (!title || !content) {
    return res.status(400).json({ error: "Title and content are required" });
  }
  try {
    const note = await Note.create(req.body);
    res.status(201).json({ data: note });
  } catch (err) {
    res.status(400).json({ error: err.message });
  }
});

// PATCH — update a note
app.patch("/api/v1/notes/:id", async (req, res) => {
  const note = await Note.findByIdAndUpdate(req.params.id, req.body, { new: true });
  if (!note) return res.status(404).json({ error: "Note not found" });
  res.status(200).json({ data: note });
});

// DELETE
app.delete("/api/v1/notes/:id", async (req, res) => {
  const note = await Note.findByIdAndDelete(req.params.id);
  if (!note) return res.status(404).json({ error: "Note not found" });
  res.status(204).send();
});

app.listen(3000, () => console.log("Server running on port 3000"));

</details>

1. Self-Check — Did You Apply the Whole Week?

CheckDay It Comes FromRoutes are nouns (/notes) not verbs (/getNotes)Day 39Status codes are accurate (201 on create, 404 on missing, 204 on delete)Day 39Response shape is consistent ({ data } / { error })Day 39Input validated before hitting the databaseDay 39Schema defines required fields and defaultsDay 38Every database call uses async/await with error handlingDay 38, Day 33express.json() middleware is set up before routes that read req.bodyDay 37

1. Pattern → Problem Map (The Real 80/20)

If the task says...Reach for..."get one specific record by ID"Model.findById(req.params.id)"get records matching a condition"Model.find({ field: value })"create a new record"Model.create(req.body) inside try/catch"update part of a record"Model.findByIdAndUpdate(id, updates, { new: true })"delete a record"Model.findByIdAndDelete(id)"record not found"404 + { error: "..." }"missing required input"400 + { error: "..." } BEFORE touching the database"successful creation"201 + the created object"successful deletion"204 + empty response

1. 40-Day Checkpoint

You've now built four complete skill stacks:

Days 14–20: SQL — basics through window functions and indexes
Days 21–26: Linux — commands, permissions, scripting, cron, networking
Days 27–30: Python — basics, functional tools, OOP
Days 31–40: JavaScript full stack — JS basics → DOM/async → React → Node/Express → MongoDB → REST design

This is no longer scattered learning — it's a coherent, deployable skillset: query and design a database (SQL + MongoDB), operate the machine it runs on (Linux), write the logic in two languages (Python + JS), and build both the interface (React) and the server (Express) that connects everything together.

Key Takeaway

The Notes API above isn't a toy exercise — it's the same shape as 90% of real backend projects: a schema, a handful of REST routes, validation, and correct status codes. If you can rebuild this from memory, you have a genuine, demonstrable full-stack skill — not just a checklist of topics you've read about.
