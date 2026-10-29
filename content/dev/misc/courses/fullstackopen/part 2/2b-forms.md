# Essential React Patterns: Rendering Collections & Modules

## 1. Console.log Debugging

1. Definition \& Use Case

- Use `console.log` liberally to inspect variables, props, and state rather than guessing errors.

2. Syntax
```js
// ✅ Separate arguments
console.log('props:', props)  
// ❌ Don’t concatenate objects  
console.log('props: ' + props)  
```

3. Example
```js
const Button = (props) => {
  console.log('Button props:', props)  
  return <button onClick={props.onClick}>{props.text}</button>
}
```


***

## 2. Rendering Collections with map()

1. Definition \& Use Case

- Use `array.map()` to transform data arrays into lists of React elements.

2. Syntax
```jsx
<ul>
  {items.map(item => 
    <li key={item.id}>{item.content}</li>
  )}
</ul>
```

3. Example
```jsx
const App = ({ notes }) => (
  <ul>
    {notes.map(note => (
      <li key={note.id}>{note.content}</li>
    ))}
  </ul>
)
```


***

## 3. Keys in Lists

1. Definition \& Use Case

- `key` helps React identify items across re-renders for efficient updates.

2. Syntax
```jsx
{notes.map(note => 
  <li key={note.id}>{note.content}</li>
)}
```

3. Example
```jsx
// Don’t use array index as key—it can cause UI bugs
{todos.map(t => <TodoItem key={t.id} todo={t}/>)}
```


***

## 4. Component Extraction \& Props

1. Definition \& Use Case

- Break UIs into reusable components; pass data via props.

2. Syntax
```jsx
// Note.jsx
const Note = ({ note }) => <li>{note.content}</li>
export default Note

// App.jsx
import Note from './components/Note'
<ul>
  {notes.map(n => <Note key={n.id} note={n}/>)}
</ul>
```

3. Example
```jsx
const UserCard = ({ user }) => (
  <div>
    <h3>{user.name}</h3>
    <p>{user.email}</p>
  </div>
)
```


***

## 5. ES6 Modules \& File Organization

1. Definition \& Use Case

- Place each component in its own file; use `export default` / `import` for clarity and reuse.

2. Syntax
```js
// components/ProductCard.jsx
export default function ProductCard({ product }) { /* ... */ }

// App.jsx
import ProductCard from './components/ProductCard'
```

3. Example
```jsx
import ReactDOM from 'react-dom/client'
import App from './App'
ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```


***

## 6. Array Methods: map, filter, reduce

1. Definition \& Use Case

- `map`: transform arrays.
- `filter`: select elements.
- `reduce`: aggregate to single value.

2. Syntax
```js
const doubled = nums.map(n => n * 2)
const evens = nums.filter(n => n % 2 === 0)
const sum = nums.reduce((acc, n) => acc + n, 0)
```

3. Example
```jsx
const HighlightedNotes = ({ notes }) => {
  const important = notes.filter(n => n.important)
  return (
    <ul>
      {important.map(n => (
        <li key={n.id}>{n.content}</li>
      ))}
    </ul>
  )
}
```


***

## 7. Controlled Forms \& State

1. Definition \& Use Case

- Keep form inputs in state for instant synchronization and validation.

2. Syntax
```jsx
const [value, setValue] = useState('')
<input 
  value={value} 
  onChange={e => setValue(e.target.value)} 
/>
```

3. Example
```jsx
const NoteForm = ({ onSubmit }) => {
  const [text, setText] = useState('')
  const handleSubmit = e => {
    e.preventDefault()
    onSubmit(text)
    setText('')
  }
  return (
    <form onSubmit={handleSubmit}>
      <input value={text} onChange={e => setText(e.target.value)}/>
      <button type="submit">Save</button>
    </form>
  )
}
```


***

## 8. Filtering Displayed Elements

1. Definition \& Use Case

- Conditionally show data based on state (e.g., show all vs. important notes).

2. Syntax
```js
const notesToShow = showAll 
  ? notes 
  : notes.filter(n => n.important)
<button onClick={() => setShowAll(!showAll)}>
  show {showAll ? 'important' : 'all'}
</button>
```

3. Example
```jsx
const App = ({ notes }) => {
  const [showAll, setShowAll] = useState(true)
  const filtered = showAll ? notes : notes.filter(n => n.important)
  return (
    <>
      <button onClick={() => setShowAll(!showAll)}>
        show {showAll ? 'important' : 'all'}
      </button>
      <ul>
        {filtered.map(n => <Note key={n.id} note={n} />)}
      </ul>
    </>
  )
}
```


***

## 9. Phonebook Application Steps

### 2.6: Add Names

```jsx
const App = () => {
  const [persons, setPersons] = useState([{ name: 'Arto Hellas' }])
  const [newName, setNewName] = useState('')

  const addPerson = e => {
    e.preventDefault()
    setPersons([...persons, { name: newName }])
    setNewName('')
  }

  return (
    <>
      <h2>Phonebook</h2>
      <form onSubmit={addPerson}>
        name: <input value={newName} onChange={e => setNewName(e.target.value)}/>
        <button type="submit">add</button>
      </form>
      <h2>Numbers</h2>
      {persons.map((p,i) => <div key={i}>{p.name}</div>)}
    </>
  )
}
```


### 2.7: Prevent Duplicates

```js
if (persons.some(p => p.name === newName)) {
  alert(`${newName} is already added to phonebook`)
  return
}
```


### 2.8: Add Numbers

```jsx
const [newNumber, setNewNumber] = useState('')
// In form
number: <input value={newNumber} onChange={e=>setNewNumber(e.target.value)}/>
// In addPerson:
setPersons([...persons, { name: newName, number: newNumber }])
```


### 2.9: Search Filter

```jsx
const [filter, setFilter] = useState('')
const shown = persons.filter(p => 
  p.name.toLowerCase().includes(filter.toLowerCase())
)
// Render input: <input value={filter} onChange={e=>setFilter(e.target.value)}/>
// Render shown list
```


### 2.10: Component Refactoring

```jsx
// PersonForm.jsx, Filter.jsx, Persons.jsx
const App = () => {
  const [persons, setPersons] = useState([...])
  // state and handlers
  return (
    <>
      <h2>Phonebook</h2>
      <Filter value={filter} onChange={e=>setFilter(e.target.value)}/>
      <h3>Add New</h3>
      <PersonForm 
        onSubmit={addPerson}
        name={newName} onNameChange={...}
        number={newNumber} onNumberChange={...}
      />
      <h3>Numbers</h3>
      <Persons persons={shown}/>
    </>
  )
}
```


***

**Key Takeaways**

- Always use `console.log` with commas for debugging.
- Render lists with `map()` + `key`.
- Extract components into modules for clarity.
- Use array methods for immutable data transforms.
- Control form inputs with state and `onChange`.
- Filter views via conditional logic.
- Refactor large components into small, reusable ones.
<span style="display:none">[^1]</span>

<div align="center">⁂</div>

[^1]: https://react.dev/reference/react/useState

