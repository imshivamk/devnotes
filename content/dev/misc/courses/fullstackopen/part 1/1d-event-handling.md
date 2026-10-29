## 1. Common Event Handler Mistakes

### 1. Definition \& Use Case

- **Definition:** Event handlers must be functions or function references, not strings, numbers, or function calls.[^1][^2][^3]
- **Use Case:** Understanding what causes common errors prevents infinite loops and re-render issues.


### 2. Syntax - Wrong vs Right

```js
// ❌ WRONG - String (React warns about this)
<button onClick="crap...">button</button>

// ❌ WRONG - Number (React warns about this)
<button onClick={value + 1}>button</button>

// ❌ WRONG - Assignment (React warns + doesn't mutate state properly)
<button onClick={value = 0}>button</button>

// ❌ WRONG - Function call (executes immediately, causes infinite loops)
<button onClick={setValue(0)}>button</button>

// ✅ CORRECT - Function reference or arrow function
<button onClick={() => setValue(0)}>button</button>
<button onClick={handleClick}>button</button>
```


### 3. Real-World Example

Button reset functionality done correctly:

```js
const App = () => {
  const [value, setValue] = useState(10)

  // Separate handler function (recommended)
  const handleReset = () => {
    console.log('Resetting value')
    setValue(0)
  }

  return (
    <div>
      <div>Current value: {value}</div>
      <button onClick={handleReset}>Reset to Zero</button>
      {/* Inline arrow function (use sparingly) */}
      <button onClick={() => setValue(value + 1)}>Increment</button>
    </div>
  )
}
```


***

## 2. Defining Event Handlers Separately

### 1. Definition \& Use Case

- **Definition:** Define event handlers as separate functions rather than inline for better readability and performance.[^3][^1]
- **Use Case:** Makes code more maintainable, easier to test, and prevents unnecessary re-renders.


### 2. Syntax

```js
const App = () => {
  const [value, setValue] = useState(10)

  // Handler defined separately
  const handleClick = () => {
    console.log('clicked the button')
    setValue(0)
  }

  return (
    <div>
      {value}
      <button onClick={handleClick}>button</button>
    </div>
  )
}
```


### 3. Real-World Example

Form with multiple handlers:

```js
const ContactForm = () => {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    message: ''
  })

  // Separate handlers for different actions
  const handleNameChange = (e) => {
    setFormData(prev => ({ ...prev, name: e.target.value }))
  }

  const handleEmailChange = (e) => {
    setFormData(prev => ({ ...prev, email: e.target.value }))
  }

  const handleSubmit = (e) => {
    e.preventDefault()
    console.log('Submitting:', formData)
    // Submit logic here
  }

  const handleReset = () => {
    setFormData({ name: '', email: '', message: '' })
  }

  return (
    <form onSubmit={handleSubmit}>
      <input 
        type="text" 
        value={formData.name}
        onChange={handleNameChange}
        placeholder="Name"
      />
      <input 
        type="email" 
        value={formData.email}
        onChange={handleEmailChange}
        placeholder="Email"
      />
      <button type="submit">Submit</button>
      <button type="button" onClick={handleReset}>Reset</button>
    </form>
  )
}
```


***

## 3. Functions That Return Functions (Advanced Pattern)

### 1. Definition \& Use Case

- **Definition:** Higher-order functions that return event handler functions, enabling parameterized event handling.[^4]
- **Use Case:** Create reusable event handlers with different parameters for similar actions.


### 2. Syntax

```js
const App = () => {
  const [value, setValue] = useState(10)

  // Function that returns a function
  const hello = (who) => {
    const handler = () => {
      console.log('hello', who)
    }
    return handler
  }

  // More concise version
  const setToValue = (newValue) => () => {
    console.log('value now', newValue)
    setValue(newValue)
  }

  return (
    <div>
      {value}
      <button onClick={hello('world')}>Greet World</button>
      <button onClick={setToValue(1000)}>Set to 1000</button>
      <button onClick={setToValue(0)}>Reset</button>
      <button onClick={setToValue(value + 1)}>Increment</button>
    </div>
  )
}
```


### 3. Real-World Example

Dynamic button generator for different user actions:

```js
const UserDashboard = () => {
  const [selectedUser, setSelectedUser] = useState(null)
  const [users] = useState([
    { id: 1, name: 'Alice', role: 'admin' },
    { id: 2, name: 'Bob', role: 'user' },
    { id: 3, name: 'Charlie', role: 'moderator' }
  ])

  // Function factory for user selection
  const createUserSelector = (user) => () => {
    console.log(`Selecting user: ${user.name}`)
    setSelectedUser(user)
  }

  // Function factory for user actions
  const createUserAction = (action, userId) => () => {
    console.log(`Performing ${action} on user ${userId}`)
    // Perform the action
  }

  return (
    <div>
      <h2>User Dashboard</h2>
      
      <div>
        <h3>Select User:</h3>
        {users.map(user => (
          <button 
            key={user.id}
            onClick={createUserSelector(user)}
            style={{ 
              backgroundColor: selectedUser?.id === user.id ? 'lightblue' : 'white' 
            }}
          >
            {user.name} ({user.role})
          </button>
        ))}
      </div>

      {selectedUser && (
        <div>
          <h3>Actions for {selectedUser.name}:</h3>
          <button onClick={createUserAction('edit', selectedUser.id)}>
            Edit User
          </button>
          <button onClick={createUserAction('delete', selectedUser.id)}>
            Delete User
          </button>
          <button onClick={createUserAction('promote', selectedUser.id)}>
            Promote User
          </button>
        </div>
      )}
    </div>
  )
}
```


***

## 4. Alternative to Functions Returning Functions

### 1. Definition \& Use Case

- **Definition:** Use regular functions with arrow functions in JSX instead of higher-order functions.[^1][^3]
- **Use Case:** Simpler approach that's often easier to understand and debug.


### 2. Syntax

```js
const App = () => {
  const [value, setValue] = useState(10)

  // Regular function
  const setToValue = (newValue) => {
    console.log('value now', newValue)
    setValue(newValue)
  }

  return (
    <div>
      {value}
      <button onClick={() => setToValue(1000)}>thousand</button>
      <button onClick={() => setToValue(0)}>reset</button>
      <button onClick={() => setToValue(value + 1)}>increment</button>
    </div>
  )
}
```


### 3. Real-World Example

Shopping cart with quantity controls:

```js
const ShoppingCartItem = ({ item, onUpdateQuantity, onRemove }) => {
  const updateQuantity = (newQuantity) => {
    if (newQuantity < 0) return
    onUpdateQuantity(item.id, newQuantity)
  }

  const removeItem = () => {
    onRemove(item.id)
  }

  return (
    <div className="cart-item">
      <h3>{item.name}</h3>
      <p>Price: ${item.price}</p>
      <div className="quantity-controls">
        <button onClick={() => updateQuantity(item.quantity - 1)}>
          -
        </button>
        <span>Quantity: {item.quantity}</span>
        <button onClick={() => updateQuantity(item.quantity + 1)}>
          +
        </button>
      </div>
      <button onClick={removeItem} className="remove-btn">
        Remove from Cart
      </button>
    </div>
  )
}
```


***

## 5. Passing Event Handlers to Child Components

### 1. Definition \& Use Case

- **Definition:** Pass event handler functions as props to child components for communication between parent and child.[^2][^1]
- **Use Case:** Maintain separation of concerns while allowing child components to trigger parent actions.


### 2. Syntax

```js
const Button = (props) => (
  <button onClick={props.onClick}>
    {props.text}
  </button>
)

const App = () => {
  const [value, setValue] = useState(10)

  const setToValue = (newValue) => {
    setValue(newValue)
  }

  return (
    <div>
      {value}
      <Button onClick={() => setToValue(1000)} text="thousand" />
      <Button onClick={() => setToValue(0)} text="reset" />
      <Button onClick={() => setToValue(value + 1)} text="increment" />
    </div>
  )
}
```


### 3. Real-World Example

Todo list with item management:

```js
const TodoItem = ({ todo, onToggle, onDelete, onEdit }) => {
  const [isEditing, setIsEditing] = useState(false)
  const [editText, setEditText] = useState(todo.text)

  const handleSave = () => {
    onEdit(todo.id, editText)
    setIsEditing(false)
  }

  const handleCancel = () => {
    setEditText(todo.text)
    setIsEditing(false)
  }

  return (
    <div className={`todo-item ${todo.completed ? 'completed' : ''}`}>
      {isEditing ? (
        <div>
          <input 
            value={editText}
            onChange={(e) => setEditText(e.target.value)}
          />
          <button onClick={handleSave}>Save</button>
          <button onClick={handleCancel}>Cancel</button>
        </div>
      ) : (
        <div>
          <input 
            type="checkbox"
            checked={todo.completed}
            onChange={() => onToggle(todo.id)}
          />
          <span>{todo.text}</span>
          <button onClick={() => setIsEditing(true)}>Edit</button>
          <button onClick={() => onDelete(todo.id)}>Delete</button>
        </div>
      )}
    </div>
  )
}

const TodoList = () => {
  const [todos, setTodos] = useState([
    { id: 1, text: 'Learn React', completed: false },
    { id: 2, text: 'Build an app', completed: false }
  ])

  const handleToggle = (id) => {
    setTodos(prev => prev.map(todo =>
      todo.id === id ? { ...todo, completed: !todo.completed } : todo
    ))
  }

  const handleDelete = (id) => {
    setTodos(prev => prev.filter(todo => todo.id !== id))
  }

  const handleEdit = (id, newText) => {
    setTodos(prev => prev.map(todo =>
      todo.id === id ? { ...todo, text: newText } : todo
    ))
  }

  return (
    <div>
      {todos.map(todo => (
        <TodoItem
          key={todo.id}
          todo={todo}
          onToggle={handleToggle}
          onDelete={handleDelete}
          onEdit={handleEdit}
        />
      ))}
    </div>
  )
}
```


***

## 6. Anti-Pattern: Components Inside Components

### 1. Definition \& Use Case

- **Definition:** Never define React components inside other components as it causes performance issues and state loss.[^5][^6][^7]
- **Use Case:** Understanding this anti-pattern prevents major performance problems and debugging nightmares.


### 2. Syntax - Wrong vs Right

```js
// ❌ WRONG - Component defined inside another component
const App = () => {
  const [value, setValue] = useState(10)

  // DON'T DO THIS - Display component defined inside App
  const Display = props => <div>{props.value}</div>

  return (
    <div>
      <Display value={value} />
    </div>
  )
}

// ✅ CORRECT - Components defined outside
const Display = props => <div>{props.value}</div>

const Button = ({ onClick, text }) => (
  <button onClick={onClick}>{text}</button>
)

const App = () => {
  const [value, setValue] = useState(10)

  return (
    <div>
      <Display value={value} />
      <Button onClick={() => setValue(0)} text="reset" />
    </div>
  )
}
```


### 3. Real-World Example

Properly structured component hierarchy:

```js
// ✅ All components defined at module level
const UserAvatar = ({ user, size = 'medium' }) => (
  <img 
    src={user.avatar}
    alt={user.name}
    className={`avatar avatar-${size}`}
  />
)

const UserInfo = ({ user }) => (
  <div className="user-info">
    <h3>{user.name}</h3>
    <p>{user.email}</p>
    <p>Role: {user.role}</p>
  </div>
)

const UserActions = ({ user, onEdit, onDelete }) => (
  <div className="user-actions">
    <button onClick={() => onEdit(user.id)}>Edit</button>
    <button onClick={() => onDelete(user.id)}>Delete</button>
  </div>
)

const UserCard = ({ user, onEdit, onDelete }) => {
  // ❌ NEVER define components here
  // const UserAvatar = ... // This would be wrong!

  return (
    <div className="user-card">
      <UserAvatar user={user} />
      <UserInfo user={user} />
      <UserActions user={user} onEdit={onEdit} onDelete={onDelete} />
    </div>
  )
}
```


***

## 7. Exercise Solutions

### Exercise 1.6: Unicafe Step 1

```js
import { useState } from 'react'

const App = () => {
  const [good, setGood] = useState(0)
  const [neutral, setNeutral] = useState(0)
  const [bad, setBad] = useState(0)

  return (
    <div>
      <h1>Give Feedback</h1>
      <button onClick={() => setGood(good + 1)}>Good</button>
      <button onClick={() => setNeutral(neutral + 1)}>Neutral</button>
      <button onClick={() => setBad(bad + 1)}>Bad</button>
      
      <h2>Statistics</h2>
      <p>Good: {good}</p>
      <p>Neutral: {neutral}</p>
      <p>Bad: {bad}</p>
    </div>
  )
}

export default App
```


### Exercise 1.7: Unicafe Step 2

```js
const App = () => {
  const [good, setGood] = useState(0)
  const [neutral, setNeutral] = useState(0)
  const [bad, setBad] = useState(0)

  const total = good + neutral + bad
  const average = total ? (good - bad) / total : 0
  const positive = total ? (good / total) * 100 : 0

  return (
    <div>
      <h1>Give Feedback</h1>
      <button onClick={() => setGood(good + 1)}>Good</button>
      <button onClick={() => setNeutral(neutral + 1)}>Neutral</button>
      <button onClick={() => setBad(bad + 1)}>Bad</button>
      
      <h2>Statistics</h2>
      <p>Good: {good}</p>
      <p>Neutral: {neutral}</p>
      <p>Bad: {bad}</p>
      <p>Total: {total}</p>
      <p>Average: {average.toFixed(2)}</p>
      <p>Positive: {positive.toFixed(1)}%</p>
    </div>
  )
}
```


### Exercise 1.8-1.11: Complete Unicafe

```js
const StatisticLine = ({ text, value }) => (
  <tr>
    <td>{text}</td>
    <td>{value}</td>
  </tr>
)

const Statistics = ({ good, neutral, bad }) => {
  const total = good + neutral + bad
  
  if (total === 0) {
    return <p>No feedback given</p>
  }

  const average = (good - bad) / total
  const positive = (good / total) * 100

  return (
    <table>
      <tbody>
        <StatisticLine text="Good" value={good} />
        <StatisticLine text="Neutral" value={neutral} />
        <StatisticLine text="Bad" value={bad} />
        <StatisticLine text="Total" value={total} />
        <StatisticLine text="Average" value={average.toFixed(2)} />
        <StatisticLine text="Positive" value={`${positive.toFixed(1)}%`} />
      </tbody>
    </table>
  )
}

const Button = ({ onClick, text }) => (
  <button onClick={onClick}>{text}</button>
)

const App = () => {
  const [good, setGood] = useState(0)
  const [neutral, setNeutral] = useState(0)
  const [bad, setBad] = useState(0)

  return (
    <div>
      <h1>Give Feedback</h1>
      <Button onClick={() => setGood(good + 1)} text="Good" />
      <Button onClick={() => setNeutral(neutral + 1)} text="Neutral" />
      <Button onClick={() => setBad(bad + 1)} text="Bad" />
      
      <h2>Statistics</h2>
      <Statistics good={good} neutral={neutral} bad={bad} />
    </div>
  )
}
```


### Exercise 1.12-1.14: Anecdotes Complete

```js
import { useState } from 'react'

const App = () => {
  const anecdotes = [
    'If it hurts, do it more often.',
    'Adding manpower to a late software project makes it later!',
    'The first 90 percent of the code accounts for the first 90 percent of the development time...The remaining 10 percent of the code accounts for the other 90 percent of the development time.',
    'Any fool can write code that a computer can understand. Good programmers write code that humans can understand.',
    'Premature optimization is the root of all evil.',
    'Debugging is twice as hard as writing the code in the first place. Therefore, if you write the code as cleverly as possible, you are, by definition, not smart enough to debug it.',
    'Programming without an extremely heavy use of console.log is same as if a doctor would refuse to use x-rays or blood tests when diagnosing patients.',
    'The only way to go fast, is to go well.'
  ]

  const [selected, setSelected] = useState(0)
  const [votes, setVotes] = useState(Array(anecdotes.length).fill(0))

  const handleNextAnecdote = () => {
    const randomIndex = Math.floor(Math.random() * anecdotes.length)
    setSelected(randomIndex)
  }

  const handleVote = () => {
    const newVotes = [...votes]
    newVotes[selected] += 1
    setVotes(newVotes)
  }

  const mostVotedIndex = votes.indexOf(Math.max(...votes))

  return (
    <div>
      <h1>Anecdote of the day</h1>
      <p>{anecdotes[selected]}</p>
      <p>Has {votes[selected]} votes</p>
      <button onClick={handleVote}>Vote</button>
      <button onClick={handleNextAnecdote}>Next anecdote</button>
      
      <h2>Anecdote with most votes</h2>
      <p>{anecdotes[mostVotedIndex]}</p>
      <p>Has {votes[mostVotedIndex]} votes</p>
    </div>
  )
}

export default App
```


***

## Summary: Key Event Handling Best Practices

| Practice | Why Important | Performance Impact |
| :-- | :-- | :-- |
| Function references, not calls | Prevents infinite loops | Critical |
| Separate handler functions | Better readability, testability | Medium |
| Avoid inline functions | Prevents unnecessary re-renders | Medium |
| Components outside components | Prevents state loss, re-mounting | Critical |
| Use useCallback for expensive handlers | Prevents child re-renders | Medium |
| Event delegation for lists | Reduces event listeners | Low |
