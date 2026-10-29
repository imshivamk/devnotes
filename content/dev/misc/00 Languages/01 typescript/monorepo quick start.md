
##  Core Rules (READ FIRST)

1. **Root package has ZERO dependencies**
    
2. **Never run `npm install` inside subfolders**
    
3. All installs use:
    
    ```bash
    npm install -w <workspace> <packages>
    ```
    
4. `workspace:*` is used **only inside workspace packages**
    

If you follow these, npm will not break.

---

## 📁 Final Folder Structure

```
bloggr/
├── package.json
├── tsconfig.base.json
├── node_modules/
│
├── backend/
│   ├── package.json
│   ├── tsconfig.json
│   ├── drizzle.config.ts
│   └── src/
│       ├── index.ts
│       ├── auth.ts
│       └── db/
│           ├── index.ts
│           └── schema.ts
│
├── frontend/
│   ├── package.json
│   ├── tsconfig.json
│   └── src/
│
└── shared/
    ├── package.json
    ├── tsconfig.json
    └── src/
        ├── schemas.ts
        └── index.ts
```

---

## 1️⃣ Root Setup

```bash
mkdir bloggr
cd bloggr
npm init -y
```

### `package.json` (ROOT — replace fully)

```json
{
  "name": "bloggr",
  "private": true,
  "workspaces": ["frontend", "backend", "shared"],
  "scripts": {
    "dev:frontend": "npm --workspace frontend run dev",
    "dev:backend": "npm --workspace backend run dev",
    "db:push": "npm --workspace backend run db:push"
  }
}
```

🚫 No dependencies here. Ever.

---

## 2️⃣ Shared TypeScript Config

### `tsconfig.base.json`

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "ESNext",
    "moduleResolution": "Bundler",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true
  }
}
```

---

## 3️⃣ Shared Package (Zod schemas)

```bash
mkdir shared
cd shared
npm init -y
mkdir src
cd ..
```

### `shared/package.json`

```json
{
  "name": "@repo/shared",
  "version": "0.0.1",
  "type": "module"
}
```

### `shared/tsconfig.json`

```json
{
  "extends": "../tsconfig.base.json",
  "include": ["src"]
}
```

### Install Zod

```bash
npm install -w shared zod
```

### `shared/src/schemas.ts`

```ts
import { z } from "zod";

export const registerSchema = z.object({
  email: z.string().email(),
  password: z.string().min(6)
});

export const loginSchema = registerSchema;

export type RegisterInput = z.infer<typeof registerSchema>;
```

### `shared/src/index.ts`

```ts
export * from "./schemas";
```

---

## 4️⃣ Backend (Express + Drizzle + JWT)

```bash
mkdir backend
cd backend
npm init -y
mkdir src/db
cd ..
```

### Install backend dependencies

```bash
npm install -w backend \
  express cors dotenv cookie-parser jsonwebtoken bcrypt \
  pg drizzle-orm
```

### Dev dependencies

```bash
npm install -D -w backend \
  typescript tsx \
  @types/node @types/express \
  @types/cookie-parser @types/jsonwebtoken
```

---

### `backend/package.json`

```json
{
  "name": "@repo/backend",
  "private": true,
  "type": "module",
  "scripts": {
    "dev": "tsx watch src/index.ts",
    "db:push": "drizzle-kit push"
  },
  "dependencies": {
    "@repo/shared": "workspace:*"
  }
}
```

### `backend/tsconfig.json`

```json
{
  "extends": "../tsconfig.base.json",
  "include": ["src"]
}
```

---

## 5️⃣ Drizzle + Postgres

### `backend/drizzle.config.ts`

```ts
import type { Config } from "drizzle-kit";

export default {
  schema: "./src/db/schema.ts",
  out: "./drizzle",
  driver: "pg",
  dbCredentials: {
    connectionString: process.env.DATABASE_URL!
  }
} satisfies Config;
```

### `backend/src/db/schema.ts`

```ts
import { pgTable, uuid, varchar, timestamp } from "drizzle-orm/pg-core";

export const users = pgTable("users", {
  id: uuid("id").defaultRandom().primaryKey(),
  email: varchar("email", { length: 255 }).unique().notNull(),
  password: varchar("password", { length: 255 }).notNull(),
  createdAt: timestamp("created_at").defaultNow()
});
```

### `backend/src/db/index.ts`

```ts
import { drizzle } from "drizzle-orm/node-postgres";
import pkg from "pg";

const { Pool } = pkg;

export const pool = new Pool({
  connectionString: process.env.DATABASE_URL
});

export const db = drizzle(pool);
```

---

## 6️⃣ JWT Cookie Auth

### `backend/src/auth.ts`

```ts
import jwt from "jsonwebtoken";

const JWT_SECRET = process.env.JWT_SECRET!;

export function signToken(userId: string) {
  return jwt.sign({ userId }, JWT_SECRET, { expiresIn: "7d" });
}
```

---

### `backend/src/index.ts`

```ts
import express from "express";
import cors from "cors";
import cookieParser from "cookie-parser";
import bcrypt from "bcrypt";
import { db } from "./db";
import { users } from "./db/schema";
import { registerSchema } from "@repo/shared";
import { signToken } from "./auth";

const app = express();

app.use(cors({ origin: "http://localhost:5173", credentials: true }));
app.use(express.json());
app.use(cookieParser());

app.post("/auth/register", async (req, res) => {
  const parsed = registerSchema.safeParse(req.body);
  if (!parsed.success) return res.status(400).json(parsed.error);

  const hashed = await bcrypt.hash(parsed.data.password, 10);

  const [user] = await db
    .insert(users)
    .values({ email: parsed.data.email, password: hashed })
    .returning();

  const token = signToken(user.id);

  res.cookie("token", token, {
    httpOnly: true,
    sameSite: "lax"
  });

  res.json({ id: user.id, email: user.email });
});

app.listen(5000, () =>
  console.log("Backend running on http://localhost:5000")
);
```

---

## 7️⃣ Frontend (Vite + React + TS)

```bash
mkdir frontend
cd frontend
npm create vite@latest .
```

Choose:

- React
    
- TypeScript
    
- Install now → **NO**
    

Return to root:

```bash
cd ..
```

### Add shared package

```bash
npm install -w frontend @repo/shared
```

### `frontend/tsconfig.json`

```json
{
  "extends": "../tsconfig.base.json",
  "include": ["src"]
}
```

---

## 8️⃣ ONE-TIME Root Install

```bash
npm install
```

This resolves:

- all workspaces
    
- all `workspace:*`
    
- creates single `node_modules`
    

---

## 9️⃣ Run the App

```bash
npm run dev:backend
npm run dev:frontend
```

---

## ✅ What You Now Have

✔ PERN monorepo  
✔ Postgres + Drizzle  
✔ JWT auth via HTTP-only cookies  
✔ Zod validation shared FE + BE  
✔ Type safety end-to-end  
✔ npm workspace-safe  
✔ Production-grade structure

---

## 🔜 Natural Next Steps

1. Auth middleware (`requireAuth`)
    
2. Refresh tokens
    
3. Role-based access
    
4. API client layer
    
5. Next.js frontend
    
6. Switch npm → pnpm or Bun (simpler)


If you want, I can turn this into:

- a **starter repo**
    
- a **checklist**
    
- or a **step-by-step learning path**
    

Just tell me 👌