# API Authentication

## Definition

API Authentication is the process of proving your identity to a server before it gives you access to protected data.
Every time you make a request to a protected API endpoint, you must send a credential — something that tells the server who you are and that you're allowed to be here.

## Why It Matters

Without authentication, any person or program could call your API and read, modify, or delete any data. Authentication is the first line of defense for every real-world application.
In the CS field it answers a fundamental question every system must solve:

"How does the server know this request is coming from a trusted source?"

## Real Life Example

Think of it like a hotel key card.
When you check in, the hotel gives you a key card (your token). Every time you want to enter your room, you tap the card (send the token in your request header). The door doesn't know your name or face — it just checks if the card is valid.
If your card expires or you check out, the card stops working — same way tokens expire and API keys get revoked.

## The 3 Most Common Authentication Methods

1. API Key
A simple secret string you attach to every request.
GET /api/data
Authorization: ApiKey abc123xyz
or as a query param (less secure):
GET /api/data?api_key=abc123xyz
Used by: weather APIs, Google Maps, Stripe.

2. Bearer Token (JWT)
A token generated after login, sent with every request.
POST /api/login
{ "email": "<user@example.com>", "password": "secret" }

→ Response: { "token": "eyJhbGciOiJIUzI1NiJ9..." }

GET /api/profile
Authorization: Bearer eyJhbGciOiJIUzI1NiJ9...

1. Basic Auth
Sends username:password encoded in Base64.
Authorization: Basic dXNlcjpwYXNzd29yZA==
Simple but less secure — only use over HTTPS, mostly for internal tools.

## Quick Reference Card

API Key:      Authorization: ApiKey <your-key>
Bearer Token: Authorization: Bearer <your-token>
Basic Auth:   Authorization: Basic <base64(user:pass)>

Login flow:

1. POST /api/login with credentials → get token
2. Store token
3. Send token in every request header:
   Authorization: Bearer <token>

## Key Takeaway

Authentication = proving who you are to the server.
In modern APIs, you login once, get a token, and send that token with every request.
Always send credentials in the Authorization header, never in the URL.
Part of my daily TIL (Today I Learned) series — learning HTTP & APIs the 80/20 way.
