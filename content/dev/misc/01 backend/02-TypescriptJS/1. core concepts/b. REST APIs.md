
## What is a REST API?

- **REST** stands for **Representational State Transfer**.
- A **REST API** is a set of rules (interface) for communication between computer systems over the web using HTTP protocols.
- It uses standard HTTP methods: **GET** (retrieve), **POST** (create), **PUT** (update/replace), **DELETE** (remove), and sometimes **PATCH** (partial update)
## Key REST Principles

- **Stateless:** No client context is stored on the server between requests.
- **Client-Server:** The client and server operate independently.
- **Uniform Interface:** Consistent paths, status codes, and data formats.
- **Cacheable:** Resources and responses can be cached.
- **Layered System:** API can be behind proxies, gateways, etc.

## Structure

## API Request Components

- **Endpoint:** The URL that defines the resource (e.g., `/users/123`)
- **Method:** The action to perform (GET, POST, etc.)
- **Headers:** Metadata (e.g., content-type, authentication)
- **Body:** Data sent with the request (for POST/PUT)
- **Parameters:** Additional info in URL or request (e.g., filters, IDs)

## Example Endpoints

|HTTP Method|URL|Action|Description|
|---|---|---|---|
|GET|/api/users|Read all users|List users|
|POST|/api/users|Create user|Add a user|
|GET|/api/users/:id|Read user|Get one user|
|PUT|/api/users/:id|Update user|Replace user|
|PATCH|/api/users/:id|Partial update|Update part of user|
|DELETE|/api/users/:id|Delete user|Remove user|

## MERN Stack + REST API

The **MERN stack**:
- **MongoDB** (database)
- **Express.js** (backend API framework)
- **React.js** (frontend)
- **Node.js** (runtime/server)

## Building a REST API with MongoDB, Node and Express

### 1. Backend: Node.js + Express

**Install dependencies:**

>npm init -y
>npm install express mongoose cors dotenv


**Basic REST API Example:**

```js
// server.js
const express = require('express')
const mongoose = require('mongoose')
const cors = require('cors')

const app = express()
app.use(cors())
app.use(express.json())

mongoose.connect('mongodb://localhost:27017/mydb', { useNewUrlParser: true, useUnifiedTopology: true })
```

```js
// models, schema
const UserSchema = new mongoose.Schema({
  name: String,
  email: String
})
const User = mongoose.model('User', UserSchema)
```

```js
// Routes

// GET all users
app.get('/api/users', async (req, res) => {
  const users = await User.find()
  res.json(users)
})

// POST create user
app.post('/api/users', async (req, res) => {
  const user = new User(req.body)
  await user.save()
  res.json(user)
})

// PUT update user
app.put('/api/users/:id', async (req, res) => {
  const user = await User.findByIdAndUpdate(req.params.id, req.body, { new: true })
  res.json(user)
})

// DELETE remove user
app.delete('/api/users/:id', async (req, res) => {
  await User.findByIdAndDelete(req.params.id)
  res.json({ message: 'User deleted' })
})

```

```js
app.listen(5000, () => console.log('Server running on port 5000'))
```

### Frontend : handling REST APIs

**Install Axios:**

>npm install axios


**React Example to Fetch Users:**

```jsx
import axios from 'axios'
import { useEffect, useState } from 'react'

function UsersList() {
  const [users, setUsers] = useState([])

  useEffect(() => {
    axios.get('http://localhost:5000/api/users').then(res => setUsers(res.data))
  }, [])

  return (
    <ul>
      {users.map(user => <li key={user._id}>{user.name} - {user.email}</li>)}
    </ul>
  )
}

```

**To Add a New User:**

>axios.post('http://localhost:5000/api/users', { name: 'Alice', email: 'alice@example.com' })

### 3. Testing Your REST API

>curl -X POST -H "Content-Type: application/json" -d '{"name": "Alice", "email": "alice@example.com"}' 

>http://localhost:5000/api/users



## notes

- **Error Handling:** Always handle errors and return appropriate HTTP status codes (e.g., 404 for not found, 400 for bad request).
- **Validation:** Validate user input before processing (with packages like Joi or express-validator).
- **Authentication:** Secure endpoints with JWT or other token-based auth for protected routes.
- **CORS:** Use CORS middleware to allow requests from your frontend’s origin domain.
- **Environment Variables:** Use `.env` for sensitive configs (e.g., DB URI, secret keys).
- **Organization:** Use MVC or a similar structure as your API grows—separate routes, controllers, and models.

