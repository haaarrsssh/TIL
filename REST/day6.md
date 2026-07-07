## TypeScript with Express

1. What is This?

Taking the Express API from Days 37–39 and rewriting it in TypeScript — adding types to routes, request/response objects, middleware, and models. The logic stays identical; TypeScript just makes every contract explicit and catches mismatches before the server ever starts.

Same Express API from Day 37. Same routes. Same logic. Now with a compiler that catches mistakes before your users do.

1. Why Does It Matter?

Plain JavaScript Express has a major blind spot — req.body, req.params, and req.query are all typed as any, meaning TypeScript can't help you catch bugs like accessing req.body.titel (typo) instead of req.body.title. With proper types:

Autocomplete works on request and response objects
Typos in field names are caught at compile time
Other developers immediately know what shape every request/response takes
Your backend codebase matches the quality standard of every serious production Node.js project

1. The 20% That Covers 80% of Real Work

Setup

bashnpm init -y
npm install express
npm install typescript ts-node @types/node @types/express --save-dev
npx tsc --init

json// tsconfig.json — key settings
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "commonjs",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true
  }
}

json// package.json — add these scripts
{
  "scripts": {
    "dev": "ts-node src/server.ts",
    "build": "tsc",
    "start": "node dist/server.js"
  }
}

Typing req and res — the core skill

typescriptimport express, { Request, Response, NextFunction } from "express";

const app = express();
app.use(express.json());

// Basic typed route
app.get("/", (req: Request, res: Response) => {
  res.status(200).json({ message: "Hello, TypeScript!" });
});

Typing request body, params, and query

typescript// Define the shapes with interfaces
interface CreateUserBody {
  name: string;
  email: string;
  age?: number;
}

interface UserParams {
  id: string;
}

interface UserQuery {
  page?: string;
  limit?: string;
}

// Use generics on Request to lock in the types
app.post(
  "/users",
  (req: Request<{}, {}, CreateUserBody>, res: Response) => {
    const { name, email, age } = req.body;   // TS knows the exact shape now
    if (!name || !email) {
      return res.status(400).json({ error: "Name and email are required" });
    }
    res.status(201).json({ data: { name, email, age } });
  }
);

app.get(
  "/users/:id",
  (req: Request<UserParams>, res: Response) => {
    const id = req.params.id;   // TS knows id is a string
    res.json({ data: { id } });
  }
);

app.get(
  "/users",
  (req: Request<{}, {}, {}, UserQuery>, res: Response) => {
    const page = parseInt(req.query.page ?? "1");
    const limit = parseInt(req.query.limit ?? "10");
    res.json({ data: { page, limit } });
  }
);

Request<Params, ResBody, ReqBody, Query> — four generics in order. Pass {} for ones you don't need.

Typed middleware

typescript// middleware/auth.ts
import { Request, Response, NextFunction } from "express";
import jwt from "jsonwebtoken";

const SECRET = process.env.JWT_SECRET || "changethis";

// Extend Express's Request type to include our custom "user" field
declare global {
  namespace Express {
    interface Request {
      user?: { id: string };
    }
  }
}

export function protect(req: Request, res: Response, next: NextFunction): void {
  const header = req.headers.authorization;

  if (!header?.startsWith("Bearer ")) {
    res.status(401).json({ error: "No token provided" });
    return;
  }

  const token = header.split[" "](1);

  try {
    const decoded = jwt.verify(token, SECRET) as { id: string };
    req.user = decoded;
    next();
  } catch {
    res.status(401).json({ error: "Invalid or expired token" });
  }
}

The declare global block tells TypeScript that req.user is a valid field — without it, req.user would throw a type error even though it works at runtime.

Typed response helpers — consistent shapes enforced by TS

typescript// types/api.ts
export interface ApiSuccess<T> {
  data: T;
}

export interface ApiError {
  error: string;
}

export type ApiResponse<T> = ApiSuccess<T> | ApiError;

typescript// Using them in a route
import { ApiSuccess, ApiError } from "./types/api";

interface User {
  id: string;
  name: string;
  email: string;
}

app.get(
  "/users/:id",
  (req: Request<{ id: string }>, res: Response<ApiSuccess<User> | ApiError>) => {
    const user: User = { id: req.params.id, name: "Harsh", email: "<harsh@example.com>" };
    res.status(200).json({ data: user });
    // res.status(200).json({ data: "wrong type" })  ← TS would catch this
  }
);

Full typed mini-server — putting it all together

typescript// src/server.ts
import express, { Request, Response } from "express";

const app = express();
app.use(express.json());

interface Task {
  id: number;
  title: string;
  done: boolean;
}

interface CreateTaskBody {
  title: string;
}

let tasks: Task[] = [];
let nextId = 1;

app.get("/tasks", (req: Request, res: Response<{ data: Task[] }>) => {
  res.json({ data: tasks });
});

app.post(
  "/tasks",
  (req: Request<{}, {}, CreateTaskBody>, res: Response) => {
    const { title } = req.body;
    if (!title) {
      return res.status(400).json({ error: "Title is required" });
    }
    const task: Task = { id: nextId++, title, done: false };
    tasks.push(task);
    res.status(201).json({ data: task });
  }
);

app.patch(
  "/tasks/:id",
  (req: Request<{ id: string }>, res: Response) => {
    const task = tasks.find(t => t.id === parseInt(req.params.id));
    if (!task) return res.status(404).json({ error: "Task not found" });
    task.done = !task.done;
    res.json({ data: task });
  }
);

app.listen(3000, () => console.log("Server running on <http://localhost:3000>"));

bashnpm run dev    # runs with ts-node — no manual compile step needed in dev

1. Real-Life Mental Model

ConceptWhat It SolvesRequest<Params, Res, Body, Query>Stops req.body.titel typos from reaching productionResponse<Shape>Guarantees you're sending the shape you promiseddeclare global for req.userTeaches TS about fields you added at runtimeinterface for body/params/queryDocuments and enforces what the API expectsApiSuccess<T> / ApiErrorConsistent response shape enforced by the compiler

The migration pattern — JS route → TS route:

typescript// JS (Day 37)
app.post("/users", async (req, res) => {
  const { name, email } = req.body;   // req.body is "any" — no help
  ...
});

// TS (Day 44)
interface CreateUserBody { name: string; email: string; }

app.post("/users", async (req: Request<{}, {}, CreateUserBody>, res: Response) => {
  const { name, email } = req.body;   // fully typed — autocomplete + error on typo
  ...
});

Key Takeaway

TypeScript doesn't change how Express works — it just makes every contract explicit. The core pattern: define an interface for req.body, req.params, and req.query, then pass them as generics to Request<Params, ResBody, ReqBody, Query>. Use declare global to extend the Request type when adding custom fields in middleware. The payoff: req.body.titel becomes a compile-time error instead of a silent production.
