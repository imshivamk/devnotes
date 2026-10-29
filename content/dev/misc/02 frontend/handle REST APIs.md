**Install Axios:**

bash

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

## 3. Testing Your REST API

>curl -X POST -H "Content-Type: application/json" -d '{"name": "Alice", "email": "alice@example.com"}' 

>http://localhost:5000/api/users

