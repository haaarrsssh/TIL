What are Status Codes?
Every HTTP response starts with a 3-digit number that instantly tells you whether the request worked — and if not, why.
HTTP/1.1 200 OK          ← everything worked
HTTP/1.1 404 Not Found   ← resource doesn't exist
HTTP/1.1 500 Internal Server Error  ← server crashed
You don't read the body first — you check the status code first.The 5 Categories
The first digit tells you the category:
RangeCategoryMeaning1xxInformationalRequest received, still processing2xx✅ SuccessRequest worked3xxRedirectGo somewhere else4xx❌ Client ErrorYou did something wrong5xx💥 Server ErrorServer did something wrong

Simple rule:

2xx = good
4xx = your fault
5xx = their fault

The Codes You'll See Every Day (The 80%)

✅ 2xx — Success
200 OK
The most common success code. Request worked, here's your data.
GET /api/users → 200 OK
{ "users": [...] }
201 Created
Resource was successfully created. Comes after a POST.
POST /api/users → 201 Created
{ "id": 42, "name": "John" }
204 No Content
Request worked but there's nothing to return. Common for DELETE.

DELETE /api/users/1 → 204 No Content
(empty body)

3xx — Redirects
301 Moved Permanently
Resource has a new URL forever. Browser updates its bookmark.
GET <http://example.com> → 301 → <https://example.com>
302 Found (Temporary Redirect)
Resource is temporarily at a different URL.

You rarely deal with these directly — browsers handle them automatically.

❌ 4xx — Client Errors (You Made a Mistake)
400 Bad Request
Your request was malformed — missing fields, wrong format, invalid data.
POST /api/users
{ "name": "" }   ← name is required but empty
→ 400 Bad Request
{ "error": "name is required" }
401 Unauthorized
You are not logged in / no credentials provided.
GET /api/profile  (no token)
→ 401 Unauthorized
{ "error": "please log in" }

❌ 4xx — Client Errors (You Made a Mistake)
400 Bad Request
Your request was malformed — missing fields, wrong format, invalid data.
POST /api/users
{ "name": "" }   ← name is required but empty
→ 400 Bad Request
{ "error": "name is required" }
401 Unauthorized
You are not logged in / no credentials provided.
GET /api/profile  (no token)
→ 401 Unauthorized
{ "error": "please log in" }

403 Forbidden
You are logged in but not allowed to access this.
DELETE /api/users/99  (you're not an admin)
→ 403 Forbidden
{ "error": "insufficient permissions" }
404 Not Found
The resource doesn't exist at that URL.
GET /api/users/9999  (user doesn't exist)
→ 404 Not Found
{ "error": "user not found" }
429 Too Many Requests
You've hit the API's rate limit. Slow down.
→ 429 Too Many Requests
{ "error": "rate limit exceeded, retry after 60s" }

💥 5xx — Server Errors (Their Mistake)
500 Internal Server Error
Something crashed on the server. Not your fault.
→ 500 Internal Server Error
{ "error": "something went wrong" }
502 Bad Gateway
Server got a bad response from another server upstream.
Common when a proxy or load balancer has issues.

503 Service Unavailable
Server is down — maintenance, overload, or crashed.
→ 503 Service Unavailable
{ "error": "service temporarily unavailable" }

401 vs 403 — The Confusing One
This trips everyone up:
401 Unauthorized403 ForbiddenAre you logged in?❌ No✅ YesDo you have access?Unknown❌ NoFixLog inGet higher permissionsReal lifeLocked door, no keyBouncer says "you're not on the list"

How to Use Status Codes When Building APIs
javascript// Good API response patterns:
GET    → 200 (found) or 404 (not found)
POST   → 201 (created) or 400 (bad data) or 409 (conflict)
PUT    → 200 (updated) or 404 (not found)
PATCH  → 200 (updated) or 404 (not found)
DELETE → 204 (deleted) or 404 (not found)

Quick Reference Card
200 OK                  → success, here's your data
201 Created             → resource created successfully
204 No Content          → success, nothing to return

301 Moved Permanently   → URL changed forever
302 Found               → temporary redirect

400 Bad Request         → malformed request
401 Unauthorized        → not logged in
403 Forbidden           → logged in but no permission
404 Not Found           → resource doesn't exist
409 Conflict            → duplicate / state conflict
422 Unprocessable       → validation failed
429 Too Many Requests   → rate limit hit

500 Internal Error      → server crashed
502 Bad Gateway         → upstream server issue
503 Unavailable         → server is down

Key Takeaway

2xx = worked, 4xx = your fault, 5xx = their fault.
The three you'll see most: 200, 404, 500.
The three that confuse most beginners: 401 vs 403, 400 vs 422, 201 vs 200.

Part of my daily TIL (Today I Learned) series — learning HTTP & APIs the 80/20 way.
