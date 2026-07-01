## Authentication: JWT, Login & Signup

1. What is Authentication?

Authentication is how your app verifies who a user is. A JWT (JSON Web Token) is the most common way to implement this in a REST API — the server issues a signed token on login, and the client sends that token with every subsequent request to prove who they are.

Authentication = proving you are who you say you are. JWT = the signed ID badge the server gives you after checking your password.

1. Why Does It Matter?

Right now your Day 37–40 API lets anyone create, read, update, or delete data — no identity required. Real apps need:

Only logged-in users can see their own data
Only admins can delete records
Sessions that expire automatically

JWT auth is the standard approach for REST APIs — used by almost every production app with a login screen. It also comes up in virtually every backend interview.

1. The 20% That Covers 80% of Real Work

Setup

bashnpm install bcryptjs jsonwebtoken

bcryptjs — hashes passwords so you never store plain text.
jsonwebtoken — creates and verifies JWTs.

How JWT auth works — the full flow

1. User sends POST /auth/signup  → { email, password }
   Server hashes password, saves user to DB, returns JWT

2. User sends POST /auth/login  → { email, password }
   Server checks password against hash, returns JWT if correct

3. User sends GET /users/me  → with header: Authorization: Bearer <token>
   Server verifies the token, extracts user ID, returns their data

User model with password

javascript// models/User.js
const mongoose = require("mongoose");
const bcrypt = require("bcryptjs");

const userSchema = new mongoose.Schema({
  email: { type: String, required: true, unique: true },
  password: { type: String, required: true },
  name: { type: String, required: true }
});

// Hash password BEFORE saving to database
userSchema.pre("save", async function (next) {
  if (!this.isModified("password")) return next();
  this.password = await bcrypt.hash(this.password, 12);
  next();
});

// Method to check if a given password matches the stored hash
userSchema.methods.comparePassword = async function (candidate) {
  return bcrypt.compare(candidate, this.password);
};

module.exports = mongoose.model("User", userSchema);

Generating a JWT

javascript// utils/token.js
const jwt = require("jsonwebtoken");

const SECRET = process.env.JWT_SECRET || "changethisinproduction";

function generateToken(userId) {
  return jwt.sign(
    { id: userId },          // payload — what's encoded inside the token
    SECRET,
    { expiresIn: "7d" }      // token expires in 7 days
  );
}

module.exports = { generateToken, SECRET };

Signup and Login routes

javascript// routes/auth.js
const express = require("express");
const User = require("../models/User");
const { generateToken } = require("../utils/token");

const router = express.Router();

// POST /auth/signup
router.post("/signup", async (req, res) => {
  const { name, email, password } = req.body;

  if (!name || !email || !password) {
    return res.status(400).json({ error: "All fields are required" });
  }

  try {
    const user = await User.create({ name, email, password });
    const token = generateToken(user._id);
    res.status(201).json({ data: { token, user: { id: user._id, name, email } } });
  } catch (err) {
    if (err.code === 11000) {   // MongoDB duplicate key error
      return res.status(400).json({ error: "Email already in use" });
    }
    res.status(500).json({ error: err.message });
  }
});

// POST /auth/login
router.post("/login", async (req, res) => {
  const { email, password } = req.body;

  if (!email || !password) {
    return res.status(400).json({ error: "Email and password are required" });
  }

  const user = await User.findOne({ email });
  if (!user) {
    return res.status(401).json({ error: "Invalid email or password" });
  }

  const isMatch = await user.comparePassword(password);
  if (!isMatch) {
    return res.status(401).json({ error: "Invalid email or password" });
  }

  const token = generateToken(user._id);
  res.status(200).json({ data: { token, user: { id: user._id, name: user.name, email } } });
});

module.exports = router;

Always return the same error for wrong email AND wrong password ("Invalid email or password") — never reveal which one is incorrect. It prevents attackers from figuring out which emails are registered.

Auth middleware — protecting routes

javascript// middleware/auth.js
const jwt = require("jsonwebtoken");
const { SECRET } = require("../utils/token");

function protect(req, res, next) {
  const header = req.headers.authorization;

  if (!header || !header.startsWith("Bearer ")) {
    return res.status(401).json({ error: "No token provided" });
  }

  const token = header.split[" "](1);

  try {
    const decoded = jwt.verify(token, SECRET);
    req.user = decoded;     // attach user info to the request
    next();
  } catch (err) {
    res.status(401).json({ error: "Invalid or expired token" });
  }
}

module.exports = protect;

Using the middleware on protected routes

javascript// server.js
const express = require("express");
const mongoose = require("mongoose");
const authRoutes = require("./routes/auth");
const protect = require("./middleware/auth");
const User = require("./models/User");

const app = express();
app.use(express.json());

mongoose.connect("mongodb://localhost:27017/myapp");

app.use("/auth", authRoutes);

// Protected route — must send a valid JWT to access
app.get("/users/me", protect, async (req, res) => {
  const user = await User.findById(req.user.id).select("-password");
  res.status(200).json({ data: user });
});

app.listen(3000, () => console.log("Server running on port 3000"));

Testing the flow with curl

bash# 1. Signup
curl -X POST <http://localhost:3000/auth/signup> \
  -H "Content-Type: application/json" \
  -d '{"name":"Harsh","email":"<harsh@example.com>","password":"secret123"}'

# 2. Login — copy the token from the response

curl -X POST <http://localhost:3000/auth/login> \
  -H "Content-Type: application/json" \
  -d '{"email":"<harsh@example.com>","password":"secret123"}'

# 3. Access protected route with token

curl <http://localhost:3000/users/me> \
  -H "Authorization: Bearer YOUR_TOKEN_HERE"

1. Real-Life Mental Model

ConceptReal EquivalentPassword hashingConverting a key into a lock shape that can't be reversedJWTA signed event wristband — proves you paid to get inJWT payloadWhat's printed on the wristband (your ID, expiry)JWT secretThe stamp only the venue (your server) knowsprotect middlewareThe bouncer at the door who checks every wristbandreq.userWho the bouncer confirmed you are.select("-password")Handing back your profile with your password covered up

What's inside a JWT:

header.payload.signature

eyJhbGci...  .  eyJpZCI6IjY2MiIsImlhdCI6...  .  abc123sig

Decoded payload: { "id": "662...", "iat": 1718000000, "exp": 1718604800 }

JWTs are base64 encoded, not encrypted — anyone can decode the payload. Never put sensitive data (passwords, card numbers) inside a JWT. The signature ensures it hasn't been tampered with, but the content is readable.

Key Takeaway

JWT auth follows a 3-step loop: signup (hash password, save, issue token) → login (verify password, issue token) → protected route (verify token via middleware, attach user to req). The two golden rules: never store plain passwords (always bcrypt.hash) and never reveal whether it's the email or password that's wrong in login error messages. Everything else is wiring these pieces together with middleware.
