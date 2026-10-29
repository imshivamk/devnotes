```
backend/
├── package.json
├── server.js
├── /config             # Environment and DB configs
├── /routes             # All API route definitions
├── /controllers        # Logic for handling each route
├── /models             # Database models (MongoDB via Mongoose)
├── /middleware         # Custom Express middleware functions
├── /utils              # Helper functions/utilities
└── .env                # Environment variables (never commit!)
```


- **Controller:** Functions containing business logic for each API endpoint.
- **Middleware:** Functions that sit between incoming requests and route handlers—can be used for logging, validation, authentication, etc.

## 1. Initialize the Project

```bash
mkdir mern-backend && cd mern-backend
npm init -y
npm install express mongoose dotenv morgan nodemon
```

- **express**: Web server framework
- **mongoose**: MongoDB ORM
- **dotenv**: Loads environment variables
- **morgan**: Logging HTTP requests
- **nodemon**: Auto-restarts server on file changes (dev use)

**Update `package.json` to add a start script:**


```json
"scripts": {
  "start": "node server.js",
  "dev": "nodemon server.js"
}
```


## 2. Basic express server setup

```js
require('dotenv').config();          // 1. Load env vars early

const express = require('express');
const morgan = require('morgan');

const app = express();

app.use(express.json());             // 2. Parse JSON body
app.use(morgan('dev'));              // 3. Log all requests

// Demo route
app.get('/api/health', (req, res) => {
  res.status(200).json({ status: "API is running" });
});

const PORT = process.env.PORT || 5000;
app.listen(PORT, () => {
  console.log(`Server on http://localhost:${PORT}`);
});
```

- **express.json()**: Built-in middleware to automatically parse incoming JSON request bodies, so you can use `req.body` in your controllers.
- **morgan('dev')**: Third-party middleware that logs HTTP requests in a concise format, extremely helpful during debugging.

## 3. Modules: Routes, Controllers, and Models

### A. Modular Routing

**routes/notesRoutes.js**

```js
import express from "express"
import { getNotes, createNote, updateNote, deleteNote, getNoteById } from "../controllers/notesControllers.js";
  
const router = express.Router();

router.get('/', getNotes);
router.get('/:id', getNoteById);
router.post('/', createNote);
router.put('/:id', updateNote);
router.delete('/:id', deleteNote);

export default router;
```

### B. Controller Example

**controllers/notesController.js**

```js
import Note from "../models/Note.js";

export async function getNotes(req, res) {
  try {
    const notes = await Note.find();
    // sort({createdAt: -1});
    res.status(200).json(notes);
  } catch (error) {
    console.error("Error in getNotes controller!", error);
    res.status(500).json({ message: "Internal server error" });
  }
}

export const createNote = async (req, res) => {
  try {
    const { title, content } = req.body;
    const note = await Note.create({ title, content });
    res.status(201).json(note);
  } catch (error) {
    console.error("Error in createNote controller", error);
    res.status(500).json({ message: "Internal Server Error" });
  }
};
```

### C. Integrate Route in Main App

Update `server.js`:
```js
import notesRoutes from "./routes/notesRoutes.js";
...
app.use("/api/notes", notesRoutes);
```

- `app.use('/api/notes', notesRoutes)`: Mounts all notes-related routes under `/api/notes`. Each method in `notesRoutes.js` becomes a sub-route.




## 4.Environment Variables & Config

- **Keeps secrets/config out of source code** (like DB passwords, API keys, PORT, etc.)
- Lets you change environments without touching code (dev, test, prod)

**Example `.env`:**

>npm i dotenv

>PORT=5000 
>MONGO_URI=mongodb://localhost:27017/mern_demo JWT_SECRET=dsddscxzcjzcckw23xsds


add to `.gitignore` 

## 5.Connecting MongoDB with Mongoose

>src/config/db.js

```js
import mongoose from "mongoose";
import dotenv from "dotenv";

dotenv.config();

const URI = process.env.URI;

export async function connectDb() {
  try {
    await mongoose.connect(URI);
    console.log("connected to MongoDB");
  } catch (error) {
    console.log("Error connecting to MongoDB");
    process.exit(1)
  }
}
```

**Update `server.js` to connect DB before starting server:**

```js
...
const connectDB = require('./config/db.js');
...
const startServer = async () => {
  try {
    await connectDb();
    app.listen(PORT, () => {
      console.log(`Server is listening at port ${PORT}`);
    });
  } catch (error) {
    console.error("Database connection failed:", error.message);
    process.exit(1);
  }
};

startServer();
```

Typescript : 

```ts
import express, { Request, Response } from "express";
import mongoose from "mongoose";
import cors from "cors";
import dotenv from "dotenv";
import { connectDb } from "./db/connectDb";

dotenv.config();

const app = express();
const PORT: number = Number(process.env.PORT) || 4000;

// middleware
app.use(cors());
app.use(express.json());

app.get("/", (req: Request, res: Response) => {
  res.send("Server running...");
});

app.get("/api/hello", (req: Request, res: Response) => {
  res.send("Hello world");
});

const startServer = async (): Promise<void> => {
  try {
    await connectDb();
    app.listen(PORT, () => {
      console.log("Server running at port " + PORT);
    });
  } catch (error: any) {
    console.error("Database connection failed:", error.message);
  }
};

// Optionally, start the server here
startServer();

```

## 6. Middleware Patterns


>middleware/logger.js

```js
const logger = (req, res, next) => {
  console.log(`${req.method} ${req.path} [${new Date().toISOString()}]`);
  next();  // Without next(), Express will hang!
};
module.exports = logger;
```

>middleware/errorHandler.js

```js
const errorHandler = (err, req, res, next) => { console.error(err.stack); /
// Logs stack trace (never send in prod)

res.status(err.status || 500).json({ message: err.message || 'Internal Server Error' }); };

module.exports = errorHandler;
```

>server.js

```js
const logger = require('./middleware/logger.js');
import { errorHandler } from "./middleware/errorHandler.js"

app.use(errorHandler);
app.use(logger);  // Will log every request
```

- **nodemon**: Run your server with `npm run dev`—it auto-restarts when you save code changes.
- **morgan**: Logs helpful details for every HTTP request (method, URL, status, response time).