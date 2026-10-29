
# Backend
```
npm init -y
npm install express mongoose cors dotenv
npm install --save-dev typescript ts-node @types/node @types/express @types/cors @types/mongoose
npx tsc --init

```

```
src/
	models/
		user.model.ts
	controllers/
		auth.controllers.ts
	db/
		connectDb.ts
	middleware/
		verifyToken.ts
	mailtrap/
		emails.ts
		emailTemplates.ts
		mailtrap.config.ts
	routes/
		auth.routes.ts
	types/
		customRequest.ts
	utils/
		getRandom.ts
		generateTokenSetCookie.ts
```

*Mongodb network settings allow access from anywhere during dev*

`.env` file
```
MONGO_URI = mongodb+srv://imshivamk:pass@cluster0.qi68c.mongodb.net/authApp?retryWrites=true&w=majority&appName=Cluster0

PORT = 5000

JWT_SECRET = mysecretkey

NODE_ENV= development

  

# mailtrap

MAILTRAP_TOKEN = <mailtrap_token>

MAILTRAP_ENDPOINT= https://send.api.mailtrap.io/

  

# client

CLIENT_URL = http://localhost:5173
```

## Setup
```bash
npm init -y
npm i -D typescript
npm i -D ts-node
npm i -D nodemon
npm i --save-dev @types/node @types/express @types/cors @types/mongoose
```

`tsconfig.json`
```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "NodeNext",               // allows import/export
    "moduleResolution": "NodeNext",   // required for ESM
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

`nodemon.json`
```json
{
  "watch": ["src"],
  "ext": "ts",
  "exec": "node --loader ts-node/esm src/index.ts"
}
```

### dependencies
1. express
2. cookie-parser
3. mailtrap
4. bcryptjs
5. dotenv
6. jsonwebtoken
7. mongoose
8. crypto
9. Nodemon (dev)

```bash
npm i express mongoose cookie-parser mailtrap bcryptjs dotenv jsonwebtoken crypto cors

npm i nodemon -D
```

### server.js

```ts
const app = express();
const PORT = process.env.PORT;
const _dirname = path.resolve();


// middleware
app.use(cors({
    origin:"http:localhost:5173",
    credentials:true
}))
app.use(express.json());
app.use(cookieParser()); // parses incoming cookies


//routes
app.get("/healthCheck", (req:Request, res:Response)=>{
    res.status(200).json({
        message:"The server is working properly"
    })
})
app.use("/api/auth", authRoutes);


// start the server
async function startServer() {
    try {
        await connectDb();
        app.listen(PORT, ()=>{
            console.log(`Server running at port:${PORT}`);
        })
    } catch (error) {
        console.log("Error connecting to the server:", error);
    }
}

startServer();
```

### routes

```ts
const router = express.Router();

router.post("/signup", signup);
router.post("/verify-email", verifyEmail);

router.post("/login", login);
router.post("/logout", logout);

router.post("/forgot-password", forgotPassword);
router.post("/reset-password/:token", resetPassword);

router.get("/check-auth", verifyToken, checkAuth);

export default router;
```


