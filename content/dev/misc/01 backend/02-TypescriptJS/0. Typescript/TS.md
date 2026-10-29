## 1. **Backend (Express + TypeScript + ESM)**

### Step 1: Init project

```bash
mkdir backend && cd backend
npm init -y
```

### Step 2: Install dependencies

```bash
npm install express mongoose cors dotenv
npm install --save-dev typescript ts-node nodemon @types/express @types/node @types/cors
```

### Step 3: Enable ESM in `package.json`

Add:

```json
{
  "type": "module",
  "scripts": {
    "dev": "nodemon src/index.ts",
    "build": "tsc",
    "start": "node dist/index.js"
  }
}
```

### Step 4: `tsconfig.json` (important!)

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "ESNext",               // allows import/export
    "moduleResolution": "NodeNext",   // required for ESM
    "esModuleInterop": true,
    "forceConsistentCasingInFileNames": true,
    "skipLibCheck": true,
    "strict": true,
    "rootDir": "src",
    "outDir": "dist"
  },
  "include": ["src"]
}
```

### Step 5: `nodemon.json`

We need Node’s loader so `.ts` is recognized:

```json
{
  "watch": ["src"],
  "ext": "ts",
  "exec": "node --loader ts-node/esm src/index.ts"
}
```

### Step 6: Example `index.ts`

```ts
import express from "express";
import mongoose from "mongoose";
import cors from "cors";
import dotenv from "dotenv";

dotenv.config();

const app = express();
app.use(cors());
app.use(express.json());

app.get("/", (req, res) => {
  res.send("API running with ESM + TypeScript 🚀");
});

const PORT = process.env.PORT || 5000;
const MONGO_URI = process.env.MONGO_URI || "";

mongoose
  .connect(MONGO_URI)
  .then(() => {
    app.listen(PORT, () => console.log(`Server running on port ${PORT}`));
  })
  .catch((err) => console.error(err));
```

Now run:

```bash
npm run dev
```

✅ You can now use `import` everywhere.

---

## 2. **Frontend (React + TS + Vite/Next.js)**

Frontend already supports ESM, so you don’t need extra config. Just do:

```bash
npm create vite@latest frontend
# choose React + TypeScript
```

Or if you prefer **Next.js**:

```bash
npx create-next-app@latest frontend --typescript
```

Both already use import/export by default.

---

## 3. **Best Practices for ESM + TS**

1. Always set `"type": "module"` in backend `package.json`.
    
2. Use `"module": "ESNext"` and `"moduleResolution": "NodeNext"` in `tsconfig.json`.
    
3. Use `node --loader ts-node/esm` with nodemon (or `ts-node-esm` alias).
    
4. For production:
    
    - Compile with `tsc` → outputs `.js` in `dist/`
        
    - Run with plain Node:
        
        ```bash
        npm run build
        node dist/index.js
        ```
        

---

- **CommonJS** = simpler, no loaders needed, but you use `require`.
    
- **ESM** = modern `import/export`, but requires `"type": "module"`, `NodeNext` resolution, and running with `ts-node/esm`.
    
