## Definition

Calling an API in real code means writing actual code or commands that send an HTTP request to a server and handle the response it sends back.
It's how your code talks to the outside world — fetching data, sending data, authenticating, and reacting to what the server returns.

## Why It Matters

Every real application calls APIs — fetching weather, processing payments, sending emails, logging in users. Knowing how to make API calls in code is one of the most immediately practical skills in software development.
It's where everything you've learned this series — methods, status codes, headers, auth — comes together into working code.

## Real Life Example

Think of it like ordering food on the phone.
You dial the restaurant (make a request), tell them what you want with your address (send method + body + headers), they confirm your order and read it back (the response). Your job is to call correctly and handle whatever they say back — including if they say "we're closed" (error handling).

## Two Ways to Call APIs

1. curl — From the Terminal
curl is a command line tool to send HTTP requests. Great for testing APIs quickly without writing any code.
GET request — fetch data:
bashcurl <https://jsonplaceholder.typicode.com/posts/1>

POST request — send data:
bashcurl -X POST <https://jsonplaceholder.typicode.com/posts> \
  -H "Content-Type: application/json" \
  -d '{"title": "My Post", "body": "Hello world", "userId": 1}'
With Authorization header:
bashcurl <https://api.example.com/profile> \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiJ9..."

## Flags to know

-X        → specify method (POST, PUT, DELETE)
-H        → add a header
-d        → send body data
-i        → show response headers too

1. fetch — In JavaScript
fetch is the built-in browser and Node.js function to make API calls in JavaScript.
GET request:
javascriptconst response = await fetch('<https://jsonplaceholder.typicode.com/posts/1>');
const data = await response.json();
console.log(data);

## POST request

javascriptconst response = await fetch('<https://jsonplaceholder.typicode.com/posts>', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    title: 'My Post',
    body: 'Hello world',
    userId: 1
  })
});

const data = await response.json();
console.log(data);

## With Authentication

javascriptconst response = await fetch('<https://api.example.com/profile>', {
  method: 'GET',
  headers: {
    'Authorization': 'Bearer eyJhbGciOiJIUzI1NiJ9...'
  }
});

const data = await response.json();
console.log(data);
Always handle errors:
const response = await fetch('<https://api.example.com/users/1>');

if (!response.ok) {
  console.error('Request failed:', response.status);
  return;
}

const data = await response.json();
console.log(data);

## Quick Reference Card

bash# curl
curl <url>                          → GET request
curl -X POST <url>                  → POST request
curl -H "Key: Value" <url>          → add header
curl -d '{"key":"value"}' <url>     → add body
curl -i <url>                       → show response headers

Key Takeaway

Calling an API = send a request with the right method + headers + body, then handle the response.
Use curl to test APIs fast from the terminal.
Use fetch to call APIs inside JavaScript code.
Always check response.ok before trying to use the data.

🎉 HTTP & APIs 80/20 Series — Complete!
