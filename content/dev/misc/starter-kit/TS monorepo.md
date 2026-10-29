Here's the complete npm workspaces monorepo with your referenced backend structure + frontend, bash commands, and code snippets.

## 1. Root monorepo setup

```bash
mkdir my-monorepo && cd my-monorepo
npm init -y
mkdir frontend backend
```

**Root package.json:**
```json
{
  "name": "my-monorepo",
  "private": true,
  "workspaces": ["frontend", "backend"],
  "scripts": {
    "dev:backend": "npm run dev --workspace=backend",
    "dev:frontend": "npm run dev --workspace=frontend",
    "dev": "npm run dev:backend & npm run dev:frontend"
  }
}
```

```bash
npm install -D typescript ts-node nodemon @types/node
npx tsc --init
```

## 2. Backend setup (your referenced structure)

```bash
cd backend
npm init -y
npm install express mongoose cors dotenv cookie-parser bcryptjs jsonwebtoken mailtrap crypto
npm install -D @types/node @types/express @types/cors @types/mongoose @types/bcryptjs @types/jsonwebtoken @types/cookie-parser
npx tsc --init
mkdir -p src/{models,controllers,db,middleware,mailtrap,routes,types,utils}
touch src/index.ts
touch .env
cd ..
```

**backend/tsconfig.json:**
```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
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

**backend/nodemon.json:**
```json
{
  "watch": ["src"],
  "ext": "ts",
  "exec": "node --loader ts-node/esm src/index.ts"
}
```

**backend/package.json scripts:**
```json
{
  "scripts": {
    "dev": "nodemon",
    "build": "tsc"
  }
}
```

**backend/.env:**
```env
MONGO_URI=mongodb+srv://your-mongo-uri
PORT=5000
JWT_SECRET=mysecretkey
NODE_ENV=development
MAILTRAP_TOKEN=your-mailtrap-token
MAILTRAP_ENDPOINT=https://send.api.mailtrap.io/
CLIENT_URL=http://localhost:5173
```

### Backend directory structure + key files

```
backend/src/
├── models/
│   └── user.model.ts
├── controllers/
│   └── auth.controllers.ts
├── db/
│   └── connectDb.ts
├── middleware/
│   └── verifyToken.ts
├── mailtrap/
│   ├── emails.ts
│   ├── emailTemplates.ts
│   └── mailtrap.config.ts
├── routes/
│   └── auth.routes.ts
├── types/
│   └── customRequest.ts
├── utils/
│   ├── getRandom.ts
│   └── generateTokenSetCookie.ts
└── index.ts
```

**backend/src/models/user.model.ts:**
```ts
import mongoose from "mongoose";

const userSchema = new mongoose.Schema({
  email: { type: String, required: true, unique: true },
  password: { type: String, required: true },
  name: { type: String, required: true },
  lastLogin: { type: Date, default: Date.now },
  isVerified: { type: Boolean, default: false },
  verificationToken: String,
  verificationTokenExpiresAt: Date,
  resetPasswordToken: String,
  resetPasswordExpiredAt: Date,
}, { timestamps: true });

const User = mongoose.model("User", userSchema);
export default User;
```

**backend/src/types/customRequest.ts:**
```ts
import { Request } from "express";
import { JwtPayload } from "jsonwebtoken";

export interface CustomRequest extends Request {
  user?: string | JwtPayload;
  userId?: string;
}
```

**backend/src/utils/generateTokenSetCookie.ts:**
```ts
import { Response } from "express";
import jwt from "jsonwebtoken";

export const generateTokenSetCookie = (res: Response, userID: string) => {
  const token = jwt.sign({ userId: userID }, process.env.JWT_SECRET as string, {
    expiresIn: "7d",
  });
  
  res.cookie("token", token, {
    httpOnly: true,
    secure: process.env.NODE_ENV === "production",
    maxAge: 7 * 24 * 60 * 60 * 1000,
    sameSite: "strict"
  });
  return token;
};
```

**backend/src/utils/getRandom.ts:**
```ts
export const getRandomInteger = (min: number, max: number) => {
  return Math.floor(Math.random() * (max - min + 1)) + min;
};
```

**backend/src/index.ts:**
```ts
import express from "express";
import cors from "cors";
import cookieParser from "cookie-parser";
import dotenv from "dotenv";
import { connectDb } from "./db/connectDb.js";
import authRoutes from "./routes/auth.routes.js";

dotenv.config();

const app = express();
const PORT = process.env.PORT || 5000;
const _dirname = process.cwd();

app.use(cors({
  origin: "http://localhost:5173",
  credentials: true
}));
app.use(express.json());
app.use(cookieParser());

app.get("/healthCheck", (req, res) => {
  res.status(200).json({ message: "The server is working properly" });
});

app.use("/api/auth", authRoutes);

async function startServer() {
  try {
    await connectDb();
    app.listen(PORT, () => {
      console.log(`Server running at port: ${PORT}`);
    });
  } catch (error) {
    console.log("Error connecting to the server:", error);
  }
}

startServer();
```

## 3. Frontend setup (Vite + React + TS)

```bash
cd frontend
npm create vite@latest . -- --template react-ts
npm install
npm install axios @types/node
touch .env
cd ..
```

**frontend/.env:**
```env
VITE_API_URL=http://localhost:5000
VITE_CLIENT_URL=http://localhost:5173
```

**frontend/package.json scripts:**
```json
{
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "preview": "vite preview"
  }
}
```

**frontend/src/App.tsx** (basic auth CRUD client):
```tsx
import { useState, useEffect } from 'react';
import axios from 'axios';

const API_URL = import.meta.env.VITE_API_URL;

function App() {
  const [token, setToken] = useState('');
  const [user, setUser] = useState<any>(null);

  const login = async () => {
    try {
      const res = await axios.post(`${API_URL}/api/auth/login`, {
        email: "test@example.com",
        password: "password123"
      }, { withCredentials: true });
      setToken(res.data.token || '');
    } catch (error) {
      console.error('Login failed', error);
    }
  };

  const checkAuth = async () => {
    try {
      const res = await axios.get(`${API_URL}/api/auth/check-auth`, {
        withCredentials: true
      });
      setUser(res.data);
    } catch (error) {
      console.error('Auth check failed', error);
    }
  };

  useEffect(() => {
    if (token) checkAuth();
  }, [token]);

  return (
    <div style={{ padding: 20 }}>
      <h1>Fullstack Monorepo Auth</h1>
      {!token && <button onClick={login}>Login</button>}
      {user && <p>Welcome, {user.name}!</p>}
    </div>
  );
}

export default App;
```

## 4. Run everything

```bash
# From root - install all deps
npm install

# Run both frontend + backend
npm run dev

# Or individually
npm run dev:backend   # Backend on :5000
npm run dev:frontend  # Frontend on :5173
```

## Final structure

```
my-monorepo/
├── package.json (workspaces)
├── node_modules/ (shared)
├── backend/
│   ├── src/ (your full structure)
│   ├── .env
│   ├── tsconfig.json
│   ├── nodemon.json
│   └── package.json
└── frontend/
    ├── src/
    ├── .env
    └── package.json
```

**Backend**: Port 5000, MongoDB, JWT cookies, full auth flow[1][2]
**Frontend**: Vite + React TS, connects via CORS + credentials[3]
**Shared**: npm workspaces hoists deps to root 





## 




## Frontend
