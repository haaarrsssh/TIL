What are HTTP Methods?
HTTP Methods (also called HTTP Verbs) tell the server what action you want to perform on a resource.
When you make an HTTP request, the method is always the first word:

GET    /api/users        → fetch users
POST   /api/users        → create a user
PUT    /api/users/1      → update user 1
DELETE /api/users/1      → delete user 1

The 5 Core Methods (The 80%)

1. GET — Read / Fetch Data
GET /api/users
GET /api/users/1
GET /api/products?category=shoes

Retrieves data from the server
Never changes anything on the server
No body in the request
Safe to call multiple times — same result every time

Real life analogy: Looking something up in a book. You read it, you don't change it.

1. POST — Create Something New
POST /api/users
Body: { "name": "John", "email": "<john@example.com>" }

Sends data to the server to create a new resource
Has a body with the data you're sending
Creates something new each time you call it
Server usually responds with the created item + its new ID

Real life analogy: Filling out and submitting a form.

1. PUT — Replace / Full Update
PUT /api/users/1
Body: { "name": "John Updated", "email": "<john@new.com>" }

Replaces the entire resource with what you send
You must send ALL fields, not just the ones you want to change
If you leave a field out → it gets overwritten with empty/null

Real life analogy: Replacing an entire document with a new version.

1. PATCH — Partial Update
PATCH /api/users/1
Body: { "email": "<newemail@example.com>" }

Updates only the fields you send
Everything else stays the same
More efficient than PUT when changing one or two fields

Real life analogy: Editing just one sentence in a document, not rewriting the whole thing.

1. DELETE — Remove a Resource
DELETE /api/users/1

Deletes the specified resource
Usually no body needed
Server responds with success confirmation or 204 No Content

Real life analogy: Throwing a document in the bin.

PUT vs PATCH — The Key Difference
This trips up a lot of developers:
json// Current user in database:
{ "name": "John", "email": "<john@old.com>", "age": 25 }

// PUT request — send full object
PUT /api/users/1
{ "name": "John", "email": "<john@new.com>", "age": 25 }
→ Result: { "name": "John", "email": "<john@new.com>", "age": 25 }

// PATCH request — send only what changed
PATCH /api/users/1
{ "email": "<john@new.com>" }
→ Result: { "name": "John", "email": "<john@new.com>", "age": 25 }
Both update email. But with PUT you must resend everything, with PATCH just send what changed.
