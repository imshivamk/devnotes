
# ✅ **1. Directory Structure (Monorepo)**

```
my-app/
│ package.json          <-- workspaces + root scripts
│
├── backend/
│   ├── package.json
│   ├── src/
│   └── server.js
│
└── frontend/
    ├── package.json
    ├── src/
    └── build/          <-- React build output
```

---

# ✅ **2. Root `package.json` (npm workspaces)**

```json
{
  "private": true,
  "workspaces": ["backend", "frontend"]
}
```

**What this gives you:**

- Single `node_modules` at root
    
- Shared dependencies
    
- Install from root only
    

---

# ❗ Important Behavior of Workspace Installs

### 👉 If you install from **root** without specifying workspace:

```
npm install axios
```

This installs **at root** and is available to all workspaces.

### 👉 If you want to install “only in backend”:

```
npm install axios --workspace=backend
```

### 👉 If you `cd backend` and run:

```
npm install axios
```

This **still installs at root**, not inside `backend/node_modules`.

Workspaces ALWAYS hoist to the top-level.

---

# ✅ **3. Build the React frontend**

From root:

```
npm run build --workspace=frontend
```

This creates:

```
frontend/build/
```

---

# ✅ **4. Serve the Frontend as Static Assets from Express**

Inside **backend/server.js**:

```js
import express from "express";
import path from "path";
import { fileURLToPath } from "url";

const app = express();

const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);

// STATIC FILES
app.use(express.static(path.join(__dirname, "../frontend/build")));

// API ROUTES
app.get("/api/hello", (req, res) => {
  res.json({ msg: "Hello from backend" });
});

// SERVE REACT FOR ALL NON-API ROUTES
app.get("*", (req, res) => {
  res.sendFile(path.join(__dirname, "../frontend/build/index.html"));
});

const PORT = process.env.PORT || 5000;
app.listen(PORT, () => console.log(`Server running on ${PORT}`));
```

✔ Works on local  
✔ Works on production hosting  
✔ React router handled  
✔ No conflicts with API

---

# ✅ **5. Root Scripts for Easy Workflow**

In root `package.json`:

```json
{
  "scripts": {
    "build": "npm run build --workspace=frontend",
    "start": "npm run start --workspace=backend"
  }
}
```

Then:

```
npm run build
npm start
```

---

# ✅ **6. Deployment on Sevalla (Node hosting)**

Sevalla = Node server → so deployment is simple.

### **Steps:**

1. **Push your monorepo to GitHub**
    
2. In Sevalla dashboard → create a Node project → select repo root
    
3. Configure **build command**:
    

```
npm install
npm run build
```

4. Configure **start command**:
    

```
npm start
```

5. Sevalla runs backend + serves frontend automatically.
    

---

# ⚙️ **Environment Variables on Sevalla**

Create `.env` in backend:

```
PORT=8080
MONGO_URL=...
```

Sevalla UI → Environment → add variables.

Your Express server reads them normally:

```js
import dotenv from "dotenv";
dotenv.config();
```

---

# 🟢 **Final Summary (Copy-Paste Setup)**

1. Use npm workspaces with:
    
    ```
    /backend
    /frontend
    ```
    
2. Install dependencies from root.
    
3. Build frontend → creates `frontend/build`.
    
4. Express serves static files from backend.
    
5. Sevalla runs `npm install`, `npm run build`, then `npm start`.
    

---

If you want, I can also give you:

✅ Template Express + React monorepo repo  
✅ Production-ready Sevalla config file  
✅ A `dockerfile` for container deployment  
✅ Your monorepo with TypeScript support

Just tell me.