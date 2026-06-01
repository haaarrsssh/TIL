## What are Headers?

Headers are key-value pairs that carry metadata about a request or response.
They travel alongside the body but they are NOT the data itself — they describe the data, the sender, the format, the auth, and more.
Content-Type: application/json
Authorization: Bearer abc123
Accept: application/json
Think of headers as the envelope — the body is the letter inside.

## What is the Body?

The body is the actual data being sent or received.

Request body → data you send to the server (e.g. a new user's details)
Response body → data the server sends back (e.g. the user that was created)

Not every request has a body:

GET and DELETE → usually no body
POST, PUT, PATCH → almost always have a body

## Request Headers (The Important Ones)

Content-Type
Tells the server what format the body is in.
Content-Type: application/json       ← sending JSON
Content-Type: application/x-www-form-urlencoded  ← sending form data
Content-Type: multipart/form-data    ← sending files/uploads
Content-Type: text/plain             ← sending plain text

If you send JSON but forget Content-Type: application/json → server may reject or misread your data.

## Accept

Tells the server what format you want the response in.
Accept: application/json    ← I want JSON back
Accept: text/html           ← I want HTML back
Accept: */*                 ← I'll take anything

Authorization
Sends your credentials so the server knows who you are.
Authorization: Bearer eyJhbGciOiJIUzI1NiJ9...   ← JWT token
Authorization: Basic dXNlcjpwYXNz              ← Base64 encoded user:pass
Authorization: ApiKey abc123xyz                ← API key

## This is how most APIs verify your identity. More on this on Day 13

User-Agent
Identifies what client is making the request.
User-Agent: Mozilla/5.0 (Windows NT 10.0)    ← browser
User-Agent: PostmanRuntime/7.32.0            ← Postman
User-Agent: my-app/1.0.0                     ← your custom app
Servers use this to detect bots, browsers, or mobile apps.

Accept-Language
Tells the server what language you prefer.

## Accept-Language: en-US

Accept-Language: hi-IN

Response Headers (The Important Ones)
Content-Type
Tells you what format the response body is in.
Content-Type: application/json       ← body is JSON
Content-Type: text/html              ← body is HTML
Content-Type: image/png              ← body is an image
Always check this before parsing the response.

Content-Length
Size of the response body in bytes.

## Content-Length: 348

Cache-Control
Tells the client how long to cache the response.
Cache-Control: no-cache           ← don't cache, always fetch fresh
Cache-Control: max-age=3600       ← cache for 1 hour
Cache-Control: no-store           ← don't store at all

Set-Cookie
Server sets a cookie on your browser.
Set-Cookie: session=abc123; HttpOnly; Secure; Max-Age=86400

## Location

Used with redirects — tells you where to go.
HTTP/1.1 301 Moved Permanently
Location: <https://new-url.com>
Also used after 201 Created to tell you where the new resource lives:
HTTP/1.1 201 Created
Location: /api/users/42

## CORS Headers

Controls which domains can access the API from a browser.
Access-Control-Allow-Origin: *                    ← anyone can call this
Access-Control-Allow-Origin: <https://myapp.com>    ← only myapp.com
Access-Control-Allow-Methods: GET, POST, PUT
CORS errors in the browser? This header is usually the culprit.

The Body — Formats
JSON (Most Common)
{
  "id": 1,
  "name": "John Doe",
  "email": "<john@example.com>"
}

Key Takeaway

Headers = envelope metadata — who you are, what format, how to handle it.
Body = the actual data being sent or received.
The two most important headers to always get right: Content-Type and Authorization.

Part of my daily TIL (Today I Learned) series — learning HTTP & APIs the 80/20 way.
