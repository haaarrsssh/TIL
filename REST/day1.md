## REST API Design Best Practices

1. What is REST?

REST (Representational State Transfer) is a set of conventions for designing APIs so they're predictable and consistent. It's not a library or framework — it's a style of organizing URLs and HTTP methods so anyone familiar with REST can guess how your API works without reading docs.

REST isn't a technology — it's a shared etiquette. Follow it, and other developers (including future-you) instantly understand your API.

1. Why Does It Matter?

Days 37–38 built a working API, but "working" and "well-designed" aren't the same thing. Bad API design causes:

Confusing endpoints (/getUser, /user-delete, /fetchAllUsersList — no consistency)
Wrong status codes (returning 200 OK even when something failed)
Frontend devs (or you, in 3 months) having to guess how things behave

REST conventions are checked in nearly every backend interview, and following them makes your API instantly usable by anyone — including the React frontend you built in Days 35–36.

1. The 20% That Covers 80% of Real Work

Resource-based URLs — nouns, not verbs

❌ Bad (verb-based, inconsistent)
GET  /getAllUsers
POST /createNewUser
GET  /deleteUser?id=5

✅ Good (resource-based, the HTTP method IS the verb)
GET    /users          → get all users
GET    /users/5         → get user 5
POST   /users            → create a new user
PUT    /users/5           → update user 5 fully
PATCH  /users/5            → update user 5 partially
DELETE /users/5             → delete user 5

The URL names the thing (users). The HTTP method names the action. Never repeat the verb in the URL.

Nesting related resources

GET  /users/5/orders        → all orders belonging to user 5
GET  /users/5/orders/12      → order 12, belonging to user 5
POST /users/5/orders          → create a new order for user 5

Use nesting to express "belongs to" relationships — but don't nest more than 2 levels deep, it gets unreadable fast.

Status codes — tell the truth about what happened

javascript// 2xx — Success
res.status(200).json(data);        // OK — general success (GET, PUT)
res.status(201).json(newItem);      // Created — successful POST
res.status(204).send();              // No Content — successful DELETE, nothing to return

// 4xx — Client made a mistake
res.status(400).json({ error: "Invalid input" });     // Bad Request
res.status(401).json({ error: "Not logged in" });       // Unauthorized
res.status(403).json({ error: "Access denied" });        // Forbidden (logged in, but not allowed)
res.status(404).json({ error: "Not found" });              // Not Found

// 5xx — Server made a mistake
res.status(500).json({ error: "Internal server error" });   // Something broke on YOUR end

The #1 mistake beginners make: always returning 200 even when something failed, forcing the frontend to parse error messages instead of checking the status code.

Consistent response shape

javascript// ✅ Good — predictable shape every time
// Success:
{ "data": { "id": 5, "name": "Harsh" } }

// Error:
{ "error": "User not found" }

// List:
{ "data": [ { "id": 1, "name": "Harsh" }, { "id": 2, "name": "Riya" } ] }

javascript// Express example
app.get("/users/:id", async (req, res) => {
  const user = await User.findById(req.params.id);
  if (!user) {
    return res.status(404).json({ error: "User not found" });
  }
  res.status(200).json({ data: user });
});

Filtering, sorting, pagination via query params

GET /users?age=25                     → filter by age
GET /users?sort=name&order=asc          → sort results
GET /users?page=2&limit=10               → pagination — page 2, 10 per page
GET /users?search=harsh                   → search/filter by keyword

javascriptapp.get("/users", async (req, res) => {
  const { page = 1, limit = 10 } = req.query;
  const skip = (page - 1) * limit;

  const users = await User.find()
    .skip(skip)
    .limit(parseInt(limit));

  res.json({ data: users, page: parseInt(page) });
});

Versioning your API

/api/v1/users
/api/v2/users     ← when you make breaking changes, bump the version
                     instead of breaking everyone using v1

Validate input before touching the database

javascriptapp.post("/users", async (req, res) => {
  const { name, email } = req.body;

  if (!name || !email) {
    return res.status(400).json({ error: "Name and email are required" });
  }

  try {
    const newUser = await User.create({ name, email });
    res.status(201).json({ data: newUser });
  } catch (err) {
    res.status(400).json({ error: err.message });
  }
});

Never trust input from the client. Validate before saving — empty fields, wrong types, and duplicate values should be caught early with clear error messages.

1. Real-Life Mental Model

Bad PracticeGood Practice/getAllUsersGET /users/deleteUser?id=5DELETE /users/5Always returning 200Matching status code to what actually happenedInconsistent response shapesAlways { data: ... } or { error: ... }No input validationCheck required fields before hitting the databaseOne giant /users endpoint returning everythingQuery params for filtering, sorting, pagination

The REST mental checklist before shipping any endpoint:

1. Is the URL a noun (resource), not a verb?
2. Does the HTTP method match the action (GET/POST/PUT/PATCH/DELETE)?
3. Does the status code reflect what actually happened?
4. Is the response shape consistent with my other endpoints?
5. Did I validate the input before touching the database?

Key Takeaway

REST is etiquette, not magic: name URLs after resources (/users), let HTTP methods carry the action, use accurate status codes instead of always returning 200, keep response shapes consistent, and validate input before it reaches your database. Following these conventions means anyone — a teammate, a frontend dev, future-you — can predict how your API behaves without reading a single line of documentation.
