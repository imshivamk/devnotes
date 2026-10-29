# Fetching Initial Data from JSON Server in React

## 1. JSON Server Setup

1. Definition \& Use Case

- **Definition:** A quick mock REST API serving data from a `db.json` file.
- **Use Case:** Simulate a backend during development without writing server code.

2. Syntax
```bash
# Install dev‐only
npm install --save-dev json-server

# Add to package.json scripts:
"server": "json-server -p 3001 db.json"

# Run:
npm run server
```

3. Example
```jsonc
// db.json
{
  "persons": [
    { "id": "1", "name": "Arto Hellas",  "number": "040-123456" },
    { "id": "2", "name": "Ada Lovelace", "number": "39-44-5323523" }
  ]
}
```

Browse to `http://localhost:3001/persons` to see data.

***

## 2. Installing Axios

1. Definition \& Use Case

- **Definition:** Promise-based HTTP client for the browser.
- **Use Case:** Fetch and post JSON data with concise syntax.

2. Syntax
```bash
npm install axios
```

```js
import axios from 'axios'
```

3. Example
```js
axios.get('http://localhost:3001/persons')
  .then(res => console.log(res.data))
```


***

## 3. useState for Storing Fetched Data

1. Definition \& Use Case

- **Definition:** React Hook to add state in functional components.
- **Use Case:** Hold the list of persons fetched from the server.

2. Syntax
```js
import { useState } from 'react'
const [persons, setPersons] = useState([])
```

3. Example
```js
const [persons, setPersons] = useState([])
// Initially empty until data loads
```


***

## 4. Fetching Data with useEffect

1. Definition \& Use Case

- **Definition:** React Hook to run side effects (e.g., data fetching) after render.
- **Use Case:** Load initial persons from JSON Server only once.

2. Syntax
```js
import { useEffect } from 'react'
useEffect(() => {
  axios
    .get('http://localhost:3001/persons')
    .then(response => setPersons(response.data))
}, [])  // Empty deps → run only on mount
```

3. Example
```jsx
useEffect(() => {
  console.log('Fetching persons...')
  axios.get('http://localhost:3001/persons')
    .then(res => {
      console.log('Data received:', res.data)
      setPersons(res.data)
    })
}, [])
```


***

## 5. Combining into App Component

1. Definition \& Use Case

- Complete example showing state, effect, and list rendering.

2. Syntax
```jsx
import React, { useState, useEffect } from 'react'
import axios from 'axios'

const App = () => {
  const [persons, setPersons] = useState([])
  const [newName, setNewName] = useState('')
  const [newNumber, setNewNumber] = useState('')

  useEffect(() => {
    axios.get('http://localhost:3001/persons')
      .then(res => setPersons(res.data))
  }, [])

  const addPerson = (e) => {
    e.preventDefault()
    const personExists = persons
      .some(p => p.name === newName)
    if (personExists) {
      alert(`${newName} is already added`)
      return
    }
    const newPerson = { name: newName, number: newNumber }
    axios.post('http://localhost:3001/persons', newPerson)
      .then(res => setPersons(persons.concat(res.data)))
    setNewName('')
    setNewNumber('')
  }

  return (
    <div>
      <h2>Phonebook</h2>
      <form onSubmit={addPerson}>
        <div>
          name: 
          <input 
            value={newName}
            onChange={e => setNewName(e.target.value)}
          />
        </div>
        <div>
          number: 
          <input 
            value={newNumber}
            onChange={e => setNewNumber(e.target.value)}
          />
        </div>
        <button type="submit">add</button>
      </form>
      <h2>Numbers</h2>
      {persons.map(p => 
        <div key={p.id}>
          {p.name} {p.number}
        </div>
      )}
    </div>
  )
}

export default App
```

3. Explanation

- **State Hooks:** `persons` holds array, `newName`/`newNumber` control form inputs.
- **Effect Hook:** Fetches initial data once on mount.
- **addPerson Handler:** Prevents default form submit, checks duplicates, posts to server, updates state.
- **Rendering:** `persons.map` displays dynamic list with unique `key` per entry.
<span style="display:none">[^1]</span>

