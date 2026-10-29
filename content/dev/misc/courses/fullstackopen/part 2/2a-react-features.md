## 1. Console.log Debugging

**Use Case:** Debug React applications effectively, track state changes, component renders.

**Syntax:**

```js
// ✅ CORRECT - Separate with commas
console.log('props value is', props)

// ❌ WRONG - Concatenation with +
console.log('props value is ' + props) // Shows [object Object]
```

**Real Example:**

```js
const Button = (props) => {
  console.log('Button rendered with:', props) // Debug props
  const { onClick, text } = props
  return <button onClick={onClick}>{text}</button>
}
```


***

## 2. Array Rendering with map()

**Use Case:** Display lists of data dynamically. Essential for data-driven UIs.[^1][^2][^3][^4]

**Syntax:**

```js
// Basic array rendering
{items.map(item => <li key={item.id}>{item.name}</li>)}

// Multi-line for readability
{notes.map(note => 
  <li key={note.id}>
    {note.content}
  </li>
)}
```

**Real Example:**

```js
const App = ({ notes }) => {
  return (
    <div>
      <h1>Notes</h1>
      <ul>
        {notes.map(note => 
          <li key={note.id}>
            {note.content}
          </li>
        )}
      </ul>
    </div>
  )
}

// Usage
const notes = [
  { id: 1, content: 'HTML is easy', important: true },
  { id: 2, content: 'Browser executes JavaScript', important: false }
]
```


***

## 3. Keys in Lists - Critical!

**Use Case:** React needs unique keys to efficiently update lists when data changes.[^5][^6][^1]

**Syntax:**

```js
// ✅ CORRECT - Use unique ID
{notes.map(note => <li key={note.id}>{note.content}</li>)}

// ❌ AVOID - Using array index (can cause bugs)
{notes.map((note, i) => <li key={i}>{note.content}</li>)}
```

**Real Example:**

```js
const TodoList = ({ todos }) => (
  <ul>
    {todos.map(todo => (
      <TodoItem 
        key={todo.id}  // Key on component, not inner elements
        todo={todo}
      />
    ))}
  </ul>
)

const TodoItem = ({ todo }) => (
  <li>{todo.text}</li>  // No key needed here
)
```


***

## 4. Component Extraction \& Props

**Use Case:** Break large components into smaller, reusable pieces. Improve maintainability.

**Syntax:**

```js
// Extract single item into component
const Note = ({ note }) => <li>{note.content}</li>

// Use in parent
const App = ({ notes }) => (
  <ul>
    {notes.map(note => 
      <Note key={note.id} note={note} />
    )}
  </ul>
)
```

**Real Example:**

```js
const UserCard = ({ user }) => (
  <div className="user-card">
    <img src={user.avatar} alt={user.name} />
    <h3>{user.name}</h3>
    <p>{user.email}</p>
  </div>
)

const UserList = ({ users }) => (
  <div className="user-grid">
    {users.map(user => 
      <UserCard key={user.id} user={user} />
    )}
  </div>
)
```


***

## 5. ES6 Modules \& File Organization

**Use Case:** Split components into separate files for better organization and reusability.

**Syntax:**

```js
// components/Note.jsx
const Note = ({ note }) => <li>{note.content}</li>
export default Note

// App.jsx
import Note from './components/Note'
const App = ({ notes }) => (
  <ul>
    {notes.map(note => <Note key={note.id} note={note} />)}
  </ul>
)
```

**Real Example:**

```js
// components/ProductCard.jsx
const ProductCard = ({ product, onAddToCart }) => (
  <div className="product-card">
    <h3>{product.name}</h3>
    <p>${product.price}</p>
    <button onClick={() => onAddToCart(product.id)}>
      Add to Cart
    </button>
  </div>
)
export default ProductCard

// pages/ProductList.jsx
import ProductCard from '../components/ProductCard'

const ProductList = ({ products }) => {
  const handleAddToCart = (productId) => {
    // Add to cart logic
  }

  return (
    <div className="product-grid">
      {products.map(product => 
        <ProductCard 
          key={product.id} 
          product={product}
          onAddToCart={handleAddToCart}
        />
      )}
    </div>
  )
}
```


***

## 6. Array Methods: map, filter, reduce

**Use Case:** Transform, filter, and aggregate data before rendering.[^7][^8][^9]

**Syntax:**

```js
// Map - transform each item
const doubled = numbers.map(n => n * 2)

// Filter - select items that match condition
const adults = users.filter(user => user.age >= 18)

// Reduce - aggregate to single value
const total = prices.reduce((sum, price) => sum + price, 0)
```

**Real Example:**

```js
const ProductDashboard = ({ products }) => {
  // Filter active products
  const activeProducts = products.filter(p => p.active)
  
  // Transform for display with discounted prices
  const displayProducts = activeProducts.map(product => ({
    ...product,
    displayPrice: product.price * 0.9,
    formattedPrice: `$${(product.price * 0.9).toFixed(2)}`
  }))
  
  // Calculate total inventory value
  const totalValue = activeProducts.reduce((sum, product) => 
    sum + (product.price * product.quantity), 0
  )

  return (
    <div>
      <h2>Total Inventory: ${totalValue.toFixed(2)}</h2>
      <div className="products">
        {displayProducts.map(product => (
          <div key={product.id}>
            <h3>{product.name}</h3>
            <p>Sale Price: {product.formattedPrice}</p>
            <p>Stock: {product.quantity}</p>
          </div>
        ))}
      </div>
    </div>
  )
}
```


***

## 7. Debugging React Explosions

**Use Case:** When React apps break, systematically debug with console.log.

**Syntax:**

```js
const Course = ({ course }) => {
  console.log('Course component received:', course) // Debug props
  
  return (
    <div>
      <Header course={course} />
    </div>
  )
}

// If component breaks, check props structure
const Course = (props) => {
  console.log('Raw props:', props) // See actual structure
  const { course } = props
  // ... rest of component
}
```

**Real Example:**

```js
const UserProfile = ({ user }) => {
  // Debug what we actually received
  console.log('UserProfile props:', { user })
  
  // Check if user exists before accessing properties
  if (!user) {
    console.log('No user provided!')
    return <div>Loading user...</div>
  }
  
  // Safe to access user properties now
  return (
    <div>
      <h1>{user.name}</h1>
      <p>{user.email}</p>
    </div>
  )
}
```


***

## 8. Exercise Solutions (2.1-2.5)

### 2.1: Course Information Step 6

```js
const Part = ({ part }) => (
  <p>{part.name} {part.exercises}</p>
)

const Content = ({ parts }) => (
  <div>
    {parts.map(part => 
      <Part key={part.id} part={part} />
    )}
  </div>
)

const Header = ({ course }) => <h1>{course}</h1>

const Course = ({ course }) => (
  <div>
    <Header course={course.name} />
    <Content parts={course.parts} />
  </div>
)

const App = () => {
  const course = {
    id: 1,
    name: 'Half Stack application development',
    parts: [
      { name: 'Fundamentals of React', exercises: 10, id: 1 },
      { name: 'Using props to pass data', exercises: 7, id: 2 },
      { name: 'State of a component', exercises: 14, id: 3 }
    ]
  }

  return <Course course={course} />
}
```


### 2.2: Course Information Step 7 (with total)

```js
const Total = ({ parts }) => {
  const total = parts.reduce((sum, part) => sum + part.exercises, 0)
  return <p><strong>Total of {total} exercises</strong></p>
}

const Course = ({ course }) => (
  <div>
    <Header course={course.name} />
    <Content parts={course.parts} />
    <Total parts={course.parts} />
  </div>
)
```


### 2.4: Multiple Courses

```js
const App = () => {
  const courses = [
    {
      name: 'Half Stack application development',
      id: 1,
      parts: [
        { name: 'Fundamentals of React', exercises: 10, id: 1 },
        { name: 'Using props to pass data', exercises: 7, id: 2 },
        { name: 'State of a component', exercises: 14, id: 3 },
        { name: 'Redux', exercises: 11, id: 4 }
      ]
    },
    {
      name: 'Node.js',
      id: 2,
      parts: [
        { name: 'Routing', exercises: 3, id: 1 },
        { name: 'Middlewares', exercises: 7, id: 2 }
      ]
    }
  ]

  return (
    <div>
      {courses.map(course => 
        <Course key={course.id} course={course} />
      )}
    </div>
  )
}
```


### 2.5: Separate Module

```js
// components/Course.jsx
const Header = ({ course }) => <h1>{course}</h1>

const Part = ({ part }) => (
  <p>{part.name} {part.exercises}</p>
)

const Content = ({ parts }) => (
  <div>
    {parts.map(part => 
      <Part key={part.id} part={part} />
    )}
  </div>
)

const Total = ({ parts }) => {
  const total = parts.reduce((sum, part) => sum + part.exercises, 0)
  return <p><strong>Total of {total} exercises</strong></p>
}

const Course = ({ course }) => (
  <div>
    <Header course={course.name} />
    <Content parts={course.parts} />
    <Total parts={course.parts} />
  </div>
)

export default Course

// App.jsx
import Course from './components/Course'

const App = () => {
  const courses = [/* ... */]
  return (
    <div>
      {courses.map(course => 
        <Course key={course.id} course={course} />
      )}
    </div>
  )
}
```


## Key Takeaways

| Concept | Critical Points | Performance Impact |
| :-- | :-- | :-- |
| **map()** | Always use keys, returns new array | Low |
| **Keys** | Must be unique, use IDs not indexes | Critical for updates |
| **Modules** | One component per file, clear exports/imports | None |
| **Debugging** | Console.log extensively, check props structure | None (remove in production) |
| **Array Methods** | map/filter/reduce are immutable, don't mutate originals | Low |
