# 🟩 Node.js Cheatsheet

> Part of [CodeCraftClub-Cheatsheets](../README.md) · Node.js + Express quick reference.

---

## Setup

```bash
node app.js                 # run a script
npm init -y                  # create package.json
npm install express          # add a dependency
npm install -D nodemon       # dev dependency
npx nodemon app.js           # auto-restart on change
```

## Modules

```js
// CommonJS (default)
const fs = require("fs");
module.exports = { add };

// ES Modules ("type": "module" in package.json)
import fs from "fs";
export const add = (a, b) => a + b;
export default function () {}
```

## Core Modules

```js
const fs = require("fs");
const path = require("path");
const os = require("os");
const http = require("http");

fs.readFileSync("file.txt", "utf-8");
fs.readFile("file.txt", "utf-8", (err, data) => {});
await fs.promises.readFile("file.txt", "utf-8");
fs.writeFileSync("out.txt", "data");

path.join(__dirname, "files", "a.txt");
path.basename("/a/b.txt");   // "b.txt"
process.env.PORT; process.argv; process.cwd();
```

## Async Patterns

```js
// Promise
function read() {
  return new Promise((resolve, reject) => {
    setTimeout(() => resolve("done"), 100);
  });
}

// async/await
async function main() {
  try {
    const result = await read();
  } catch (e) {
    console.error(e);
  }
}

await Promise.all([p1, p2]);
```

## Express — Basic Server

```js
const express = require("express");
const app = express();

app.use(express.json());                  // parse JSON body
app.use(express.static("public"));        // serve static files

app.get("/", (req, res) => {
  res.send("Hello World");
});

app.listen(3000, () => console.log("on :3000"));
```

## Express — Routing

```js
app.get("/users/:id", (req, res) => {
  const id = req.params.id;               // route param
  const sort = req.query.sort;            // ?sort=asc
  res.json({ id, sort });
});

app.post("/users", (req, res) => {
  const body = req.body;                  // JSON body
  res.status(201).json(body);
});

app.put("/users/:id", (req, res) => {});
app.delete("/users/:id", (req, res) => {});

// Router module
const router = express.Router();
router.get("/", handler);
app.use("/api", router);
```

## Middleware

```js
// Custom middleware
function logger(req, res, next) {
  console.log(`${req.method} ${req.url}`);
  next();                                  // pass control on
}
app.use(logger);

// Error-handling middleware (4 args)
app.use((err, req, res, next) => {
  res.status(500).json({ error: err.message });
});

// Auth guard
function auth(req, res, next) {
  if (!req.headers.authorization) return res.sendStatus(401);
  next();
}
```

## Common Responses

```js
res.send("text");
res.json({ ok: true });
res.status(404).json({ error: "not found" });
res.redirect("/login");
res.set("X-Custom", "value");
```

## Environment & Config

```js
require("dotenv").config();      // load .env
const PORT = process.env.PORT || 3000;
```

```bash
# .env
PORT=4000
DB_URL=mongodb://localhost/app
```

## Useful npm Packages

```bash
npm i express          # web framework
npm i mongoose         # MongoDB ODM
npm i dotenv           # env vars
npm i cors             # CORS handling
npm i jsonwebtoken     # JWT auth
npm i bcrypt           # password hashing
npm i axios            # HTTP client
```

---

[🔝 Back to README](../README.md)
