Here's a minimal, modern quickstart for initializing a **Node.js + TypeScript** project.

## 1. Create the project

```bash
mkdir my-app
cd my-app

npm init -y
```

## 2. Install dependencies

Development dependencies:

```bash
npm install -D typescript tsx @types/node
```

- `typescript` — TypeScript compiler
    
- `tsx` — Run TypeScript files directly during development
    
- `@types/node` — Node.js type definitions
    

## 3. Initialize TypeScript

```bash
npx tsc --init
```

Replace the generated `tsconfig.json` with:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "sourceMap": true
  },
  "include": ["src"]
}
```

## 4. Create your source folder

```bash
mkdir src
touch src/index.ts
```

`src/index.ts`

```ts
console.log("Hello, TypeScript!");
```

## 5. Update `package.json`

```json
{
  "type": "module",
  "scripts": {
    "dev": "tsx watch src/index.ts",
    "build": "tsc",
    "start": "node dist/index.js"
  }
}
```

## 6. Run in development

```bash
npm run dev
```

## 7. Build

```bash
npm run build
```

Compiled files will be in:

```
dist/
```

## 8. Run production build

```bash
npm start
```

---

## Recommended project structure

```
my-app/
│
├── src/
│   └── index.ts
│
├── dist/
│
├── package.json
├── tsconfig.json
└── node_modules/
```

---

## Useful additional packages

For most backend projects, you'll likely also install:

```bash
npm install dotenv
npm install -D eslint prettier @types/node
```

If you're building an Express API:

```bash
npm install express
npm install -D @types/express
```

This setup uses the modern **ES Modules (`NodeNext`)** configuration and `tsx` for a fast development experience without needing to compile before every run.