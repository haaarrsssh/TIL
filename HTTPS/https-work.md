What is HTTP?
HTTP stands for HyperText Transfer Protocol.
It is the set of rules that governs how data is sent and received between a client (your browser or app) and a server (where the data lives).
Every time you open a website, watch a video, or use an app — HTTP is working underneath.

The Big Picture: Request → Response
HTTP is always a two-step conversation:
Client                        Server
  |                              |
  |  ── HTTP Request ──────────► |
  |                              |  (server processes)
  |  ◄── HTTP Response ──────── |
  |                              |

Client sends a Request → "Give me this page / data"
Server sends a Response → "Here it is" (or "Not found" etc.)

That's it. Every API call, every webpage load, follows this exact pattern.

A Real Example — What Happens When You Visit google.com

1. You type google.com in browser
2. Browser sends HTTP Request to Google's server:
   "GET / HTTP/1.1
    Host: google.com"

3. Google's server receives it, finds the homepage
4. Server sends HTTP Response:
   "HTTP/1.1 200 OK
    Content-Type: text/html
    ...the HTML of the page..."

5. Browser renders the HTML → you see Google

All of this happens in milliseconds.

Anatomy of an HTTP Request
Every HTTP request has 4 parts:
METHOD  /path  HTTP/version
Headers

Body (optional)
Real example:
POST /api/login HTTP/1.1
Host: myapp.com
Content-Type: application/json
Authorization: Bearer abc123

{
  "email": "<user@example.com>",
  "password": "secret"
}

PartWhat it isPOSTThe method — what action you want/api/loginThe path — which resource you wantHTTP/1.1The version of HTTPHost, Content-TypeHeaders — metadata about the request{ "email": ... }Body — the data you're sending

Anatomy of an HTTP Response
Every HTTP response also has 4 parts:
HTTP/version  STATUS_CODE  STATUS_MESSAGE
Headers

Body
Real example:
HTTP/1.1 200 OK
Content-Type: application/json
Date: Thu, 29 May 2026 10:00:00 GMT

{
  "token": "xyz789",
  "user": "John"
}

PartWhat it is200The status code — did it work?OKHuman-readable status messageContent-TypeHeaders — metadata about the response{ "token": ... }Body — the data the server returned

HTTP vs HTTPS
HTTPHTTPSData sent as plain textData is encryptedAnyone can read itOnly client & server can read ithttp://<https://Not> safe for passwords/cards✅ Safe for sensitive data
The S in HTTPS stands for Secure — it uses TLS encryption.
Always use HTTPS in real applications.

HTTP Versions (Quick Overview)
VersionYearKey FeatureHTTP/1.11997Still widely used, one request at a timeHTTP/22015Multiple requests at once, fasterHTTP/32022Even faster, uses UDP instead of TCP

Key Terms to Know
TermMeaningClientThe one making the request (browser, app, code)ServerThe one responding (where data lives)RequestMessage from client to serverResponseMessage from server back to clientEndpointA specific URL that accepts requests (/api/users)PayloadThe data sent in the body of a request/responseLatencyTime it takes for request to reach server and back

## Quick Reference Card

HTTP  = rules for client-server communication
HTTPS = HTTP + encryption (always use this)

Request  = client → server (method + path + headers + body)
Response = server → client (status + headers + body)

Every API call = one request + one response

## Key Takeaway

HTTP is just a conversation — client asks, server answers.
Every API you ever call follows this exact request → response pattern.
Understanding this is the foundation of everything in web development.
