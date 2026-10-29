A backend is responsible for handling business logic, data persistence, authentication, and integration with other services, exposing APIs for frontend or third-party use.

> Request-Response Cycle: Clients (browser, apps) send requests to the backend, which processes them, fetches (or stores) data, and returns a response.


```js
app.get('/users/:id', (req, res) => {
  // Fetch user from database
  res.json({ id: req.params.id, name: "John" });
});
```

## How the Internet Works (Brief Essentials)

- Clients (browsers, apps) send HTTP requests to servers (your backend).
- DNS (Domain Name System): Translates domain names (`example.com`) to IP addresses.

- Hosting Models:
    - _Shared hosting_ (many sites share one server)
    - _VPS/Cloud_ (dedicated virtual machines, e.g., AWS EC2, Digital-Ocean)
    - _Serverless_ (code runs in response to events, no server management, e.g., AWS Lambda)

## **HTTP Fundamentals**

- **Requests**: Made by clients.
    - _Methods_: GET, POST, PUT, DELETE, PATCH
    - _Headers_: Metadata (e.g., `Content-Type`, `Authorization`)
    - _Body_: Payload for POST/PUT

- **Responses**: Sent by servers.
    - _Status Codes_: 200 (OK), 404 (Not Found), etc.
    - _Headers_: Metadata (e.g., `Set-Cookie`, `Cache-Control`)
    - _Body_: Usually JSON or HTML


 ## Sample HTTP Status Codes Table

| Code | Meaning        | Typical Use       |
| ---- | -------------- | ----------------- |
| 200  | OK             | Success           |
| 201  | Created        | New resource made |
| 400  | Bad Request    | Validation error  |
| 401  | Unauthorized   | No/invalid token  |
| 404  | Not Found      | Resource missing  |
| 500  | Internal Error | Server crash      |

---
## 1. What Happens When a Client Accesses Your Server?

1. The user enters a URL (e.g., `myblog.com`).
2. **DNS** resolves the domain to an IP address.
3. The browser makes an **HTTP request** to your backend server at this IP.
4. Your server processes the request (Express + MongoDB in our MERN setup).
5. The server sends an **HTTP response** (JSON, status code, headers).
6. The user’s browser/app displays or uses this data.
## 2. Request-Response Cycle with Express 

```/mern-backend
backend/
  |-- server.js
  |-- package.json
```

>npm install express


```js
const express = require('express');
const app = express();

// Middleware: parse JSON and log requests
app.use(express.json());
app.use((req, res, next) => {
  console.log(`[${req.method}] ${req.path}`);
  next();
});

// Simple demo endpoint
app.get('/api/hello', (req, res) => {
  res.status(200).json({ message: "Hello, world!" });
});

// Start server
app.listen(5000, () => console.log('Server running on http://localhost:5000'));

```

>curl http://localhost:5000/api/hello
>{ "message": "Hello, world!" }



NEXT CHAPTER ==> [[b. REST APIs]]