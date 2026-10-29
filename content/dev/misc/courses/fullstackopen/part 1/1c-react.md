# React Component State and Event Handlers

## 1. Component Helper Functions

1. Functions defined inside React components can directly access props and state. 
2. They encapsulate component-specific logic.
3. Calculating derived values, formatting data, or performing component-specific operations.

```js
const Hello = (props) => {
  const bornYear = () => {    // Helper function inside component
    const yearNow = new Date().getFullYear()
    return yearNow - props.age  // Direct access to props
  }
  
  return (
    <div>
      <p>Hello {props.name}, you are {props.age} years old</p>
      <p>So you were probably born in {bornYear()}</p>
    </div>
  )
}
```

***

## 2. Destructuring Props

1. Extract specific values from objects/arrays into variables for cleaner code.
2. Simplify prop access, improve readability, and avoid repetitive `props.property` syntax.

```js
// Method 1: Inside component body
const Hello = (props) => {
  const { name, age } = props  // Extract properties
  // Use name and age directly
}

// Method 2: In function parameters (preferred)
const Hello = ({ name, age }) => {
  // Direct access to name and age
}

// Method 3: Nested destructuring
const UserCard = ({ user: { firstName, lastName, email } }) => {
  // Direct access to nested properties
}

// Method 4: Default values
const Greeting = ({ name = 'Guest', message = 'Welcome' }) => {
  // Provides fallbacks if props are undefined
}
```

E-commerce product component with various destructuring patterns:

```js
// Complex product object destructuring
const ProductCard = ({ 
  product: { 
    id, 
    title, 
    price, 
    images: [mainImage], // Extract first image
    category: { name: categoryName } // Rename during destructuring
  },
  onAddToCart,
  discount = 0, // Default value
  showReviews = true
}) => {
  const discountedPrice = price - (price * discount / 100)
  
  return (
    <div className="product-card">
      <img src={mainImage} alt={title} />
      <h3>{title}</h3>
      <p>Category: {categoryName}</p>
      <div className="pricing">
        {discount > 0 && <span className="original-price">${price}</span>}
        <span className="price">${discountedPrice.toFixed(2)}</span>
      </div>
      {showReviews && <ReviewSection productId={id} />}
      <button onClick={() => onAddToCart(id)}>
        Add to Cart
      </button>
    </div>
  )
}

// Usage
<ProductCard 
  product={{
    id: 1,
    title: "Gaming Laptop",
    price: 1299,
    images: ["laptop1.jpg", "laptop2.jpg"],
    category: { name: "Electronics" }
  }}
  onAddToCart={handleAddToCart}
  discount={15}
/>
```


***

## 3. Component State with useState

1. useState hook adds state to functional components. Returns current state and setter function.
2. **Use Case:** Managing component data that changes over time - counters, form inputs, toggles, loading states.

syntax :

```js
import { useState } from 'react'

const Component = () => {
  const [state, setState] = useState(initialValue)
  
  // State update triggers re-render
  const handleUpdate = () => {
    setState(newValue)
  }
  
  return <div>{state}</div>
}
```

Shopping cart with multiple state variables:

```js
const ShoppingCart = () => {
  const [items, setItems] = useState([])           // Cart items
  const [isOpen, setIsOpen] = useState(false)     // Cart visibility
  const [loading, setLoading] = useState(false)   // Loading state
  const [total, setTotal] = useState(0)          // Cart total
  
  // Add item to cart
  const addItem = (product) => {
    setLoading(true)
    
    // Simulate API call
    setTimeout(() => {
      setItems(prevItems => {
        const existingItem = prevItems.find(item => item.id === product.id)
        
        if (existingItem) {
          // Update quantity for existing item
          return prevItems.map(item =>
            item.id === product.id
              ? { ...item, quantity: item.quantity + 1 }
              : item
          )
        } else {
          // Add new item
          return [...prevItems, { ...product, quantity: 1 }]
        }
      })
      setLoading(false)
    }, 500)
  }
  
  // Calculate total when items change
  useEffect(() => {
    const newTotal = items.reduce((sum, item) => 
      sum + (item.price * item.quantity), 0
    )
    setTotal(newTotal)
  }, [items])
  
  return (
    <div className="shopping-cart">
      <button onClick={() => setIsOpen(!isOpen)}>
        Cart ({items.length}) - ${total.toFixed(2)}
      </button>
      
      {isOpen && (
        <div className="cart-dropdown">
          {loading && <div className="loading">Adding item...</div>}
          {items.length === 0 ? (
            <p>Your cart is empty</p>
          ) : (
            <div>
              {items.map(item => (
                <CartItem key={item.id} item={item} />
              ))}
              <div className="total">Total: ${total.toFixed(2)}</div>
            </div>
          )}
        </div>
      )}
    </div>
  )
}
```

***

## 4. Event Handlers
1. Functions that respond to user interactions (clicks, inputs, form submissions).
2. Handle user actions, update state, trigger side effects, form validations.

Syntax:

```js
const Component = () => {
  // Method 1: Separate function (recommended)
  const handleClick = () => {
    console.log('Button clicked')
  }
  
  // Method 2: Inline arrow function (use sparingly)
  return (
    <div>
      <button onClick={handleClick}>Click Me</button>
      <button onClick={() => console.log('Inline')}>Inline</button>
    </div>
  )
}
```

Contact form with comprehensive event handling:

```js
const ContactForm = () => {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    message: '',
    subject: 'general'
  })
  const [errors, setErrors] = useState({})
  const [isSubmitting, setIsSubmitting] = useState(false)
  const [submitStatus, setSubmitStatus] = useState(null)
  
  // Handle input changes
  const handleInputChange = (e) => {
    const { name, value } = e.target
    setFormData(prev => ({
      ...prev,
      [name]: value
    }))
    
    // Clear error for this field when user starts typing
    if (errors[name]) {
      setErrors(prev => ({
        ...prev,
        [name]: ''
      }))
    }
  }
  
  // Validate form
  const validateForm = () => {
    const newErrors = {}
    
    if (!formData.name.trim()) {
      newErrors.name = 'Name is required'
    }
    
    if (!formData.email.trim()) {
      newErrors.email = 'Email is required'
    } else if (!/\S+@\S+\.\S+/.test(formData.email)) {
      newErrors.email = 'Email is invalid'
    }
    
    if (!formData.message.trim()) {
      newErrors.message = 'Message is required'
    }
    
    setErrors(newErrors)
    return Object.keys(newErrors).length === 0
  }
  
  // Handle form submission
  const handleSubmit = async (e) => {
    e.preventDefault() // Prevent default form submission
    
    if (!validateForm()) return
    
    setIsSubmitting(true)
    setSubmitStatus(null)
    
    try {
      const response = await fetch('/api/contact', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(formData)
      })
      
      if (response.ok) {
        setSubmitStatus('success')
        setFormData({ name: '', email: '', message: '', subject: 'general' })
      } else {
        setSubmitStatus('error')
      }
    } catch (error) {
      setSubmitStatus('error')
    } finally {
      setIsSubmitting(false)
    }
  }
  
  // Handle reset
  const handleReset = () => {
    setFormData({ name: '', email: '', message: '', subject: 'general' })
    setErrors({})
    setSubmitStatus(null)
  }
  
  return (
    <form onSubmit={handleSubmit} className="contact-form">
      <div className="form-group">
        <label htmlFor="name">Name:</label>
        <input
          type="text"
          id="name"
          name="name"
          value={formData.name}
          onChange={handleInputChange}
          className={errors.name ? 'error' : ''}
        />
        {errors.name && <span className="error-message">{errors.name}</span>}
      </div>
      
      <div className="form-group">
        <label htmlFor="email">Email:</label>
        <input
          type="email"
          id="email"
          name="email"
          value={formData.email}
          onChange={handleInputChange}
          className={errors.email ? 'error' : ''}
        />
        {errors.email && <span className="error-message">{errors.email}</span>}
      </div>
      
      <div className="form-group">
        <label htmlFor="subject">Subject:</label>
        <select
          id="subject"
          name="subject"
          value={formData.subject}
          onChange={handleInputChange}
        >
          <option value="general">General Inquiry</option>
          <option value="support">Support</option>
          <option value="business">Business</option>
        </select>
      </div>
      
      <div className="form-group">
        <label htmlFor="message">Message:</label>
        <textarea
          id="message"
          name="message"
          value={formData.message}
          onChange={handleInputChange}
          className={errors.message ? 'error' : ''}
        />
        {errors.message && <span className="error-message">{errors.message}</span>}
      </div>
      
      <div className="form-actions">
        <button 
          type="submit" 
          disabled={isSubmitting}
          className="submit-btn"
        >
          {isSubmitting ? 'Sending...' : 'Send Message'}
        </button>
        <button 
          type="button" 
          onClick={handleReset}
          className="reset-btn"
        >
          Reset
        </button>
      </div>
      
      {submitStatus === 'success' && (
        <div className="success-message">Message sent successfully!</div>
      )}
      {submitStatus === 'error' && (
        <div className="error-message">Failed to send message. Please try again.</div>
      )}
    </form>
  )
}
```


***

## 5. Event Handler Best Practices

### 1. Definition \& Use Case

- **Definition:** Proper patterns for defining and using event handlers to avoid performance issues and bugs.[^8][^7]
- **Use Case:** Ensuring components re-render efficiently and event handlers work correctly.


### 2. Syntax \& Common Pitfalls

```js
// ❌ WRONG - Function call instead of reference
<button onClick={handleClick()}>Click</button>

// ✅ CORRECT - Function reference
<button onClick={handleClick}>Click</button>

// ❌ WRONG - Creates new function on every render
<button onClick={() => console.log('clicked')}>Click</button>

// ✅ BETTER - Separate function
const handleClick = () => console.log('clicked')
<button onClick={handleClick}>Click</button>

// ✅ BEST - With useCallback for performance
const handleClick = useCallback(() => {
  console.log('clicked')
}, [])
```


### 3. Real-World Example

Optimized todo list with proper event handling:

```js
import { useState, useCallback } from 'react'

const TodoList = () => {
  const [todos, setTodos] = useState([])
  const [newTodo, setNewTodo] = useState('')
  
  // Memoized event handlers to prevent unnecessary re-renders
  const handleAddTodo = useCallback(() => {
    if (newTodo.trim()) {
      setTodos(prev => [...prev, {
        id: Date.now(),
        text: newTodo.trim(),
        completed: false
      }])
      setNewTodo('')
    }
  }, [newTodo])
  
  const handleToggleTodo = useCallback((id) => {
    setTodos(prev => prev.map(todo =>
      todo.id === id ? { ...todo, completed: !todo.completed } : todo
    ))
  }, [])
  
  const handleDeleteTodo = useCallback((id) => {
    setTodos(prev => prev.filter(todo => todo.id !== id))
  }, [])
  
  const handleInputChange = useCallback((e) => {
    setNewTodo(e.target.value)
  }, [])
  
  const handleKeyPress = useCallback((e) => {
    if (e.key === 'Enter') {
      handleAddTodo()
    }
  }, [handleAddTodo])
  
  return (
    <div className="todo-list">
      <div className="todo-input">
        <input
          type="text"
          value={newTodo}
          onChange={handleInputChange}
          onKeyPress={handleKeyPress}
          placeholder="Add new todo..."
        />
        <button onClick={handleAddTodo}>Add</button>
      </div>
      
      <ul className="todos">
        {todos.map(todo => (
          <TodoItem
            key={todo.id}
            todo={todo}
            onToggle={handleToggleTodo}
            onDelete={handleDeleteTodo}
          />
        ))}
      </ul>
    </div>
  )
}

// Separate component with memoization
const TodoItem = React.memo(({ todo, onToggle, onDelete }) => {
  const handleToggle = () => onToggle(todo.id)
  const handleDelete = () => onDelete(todo.id)
  
  return (
    <li className={`todo-item ${todo.completed ? 'completed' : ''}`}>
      <input
        type="checkbox"
        checked={todo.completed}
        onChange={handleToggle}
      />
      <span>{todo.text}</span>
      <button onClick={handleDelete}>Delete</button>
    </li>
  )
})
```


***

## 6. Lifting State Up

### 1. Definition \& Use Case

- **Definition:** Moving state to the closest common ancestor when multiple components need access to the same data.[^10][^1]
- **Use Case:** Sharing state between sibling components, coordinating component interactions.


### 2. Syntax Pattern

```js
// Parent component holds shared state
const Parent = () => {
  const [sharedState, setSharedState] = useState(initialValue)
  
  return (
    <div>
      <ChildA data={sharedState} onUpdate={setSharedState} />
      <ChildB data={sharedState} onUpdate={setSharedState} />
    </div>
  )
}

// Children receive state and update functions as props
const ChildA = ({ data, onUpdate }) => {
  // Use data and call onUpdate to modify shared state
}
```


### 3. Real-World Example

Temperature converter with shared state:

```js
const TemperatureCalculator = () => {
  const [temperature, setTemperature] = useState('')
  const [scale, setScale] = useState('c')
  
  const handleCelsiusChange = (temperature) => {
    setScale('c')
    setTemperature(temperature)
  }
  
  const handleFahrenheitChange = (temperature) => {
    setScale('f')
    setTemperature(temperature)
  }
  
  const celsius = scale === 'f' ? tryConvert(temperature, toCelsius) : temperature
  const fahrenheit = scale === 'c' ? tryConvert(temperature, toFahrenheit) : temperature
  
  return (
    <div className="temperature-calculator">
      <TemperatureInput
        scale="c"
        temperature={celsius}
        onTemperatureChange={handleCelsiusChange}
      />
      
      <TemperatureInput
        scale="f"
        temperature={fahrenheit}
        onTemperatureChange={handleFahrenheitChange}
      />
      
      <BoilingVerdict celsius={parseFloat(celsius)} />
    </div>
  )
}

const TemperatureInput = ({ scale, temperature, onTemperatureChange }) => {
  const scaleName = { c: 'Celsius', f: 'Fahrenheit' }
  
  const handleChange = (e) => {
    onTemperatureChange(e.target.value)
  }
  
  return (
    <fieldset>
      <legend>Enter temperature in {scaleName[scale]}:</legend>
      <input value={temperature} onChange={handleChange} />
    </fieldset>
  )
}

const BoilingVerdict = ({ celsius }) => {
  if (celsius >= 100) {
    return <p>The water would boil.</p>
  }
  return <p>The water would not boil.</p>
}

// Helper functions
function toCelsius(fahrenheit) {
  return (fahrenheit - 32) * 5 / 9
}

function toFahrenheit(celsius) {
  return (celsius * 9 / 5) + 32
}

function tryConvert(temperature, convert) {
  const input = parseFloat(temperature)
  if (Number.isNaN(input)) {
    return ''
  }
  const output = convert(input)
  const rounded = Math.round(output * 1000) / 1000
  return rounded.toString()
}
```


***

## Summary Table: State \& Event Handler Patterns

| Pattern | Use Case | Performance Impact | Best Practice |
| :-- | :-- | :-- | :-- |
| Helper Functions | Calculated values | Low | Keep inside component for prop access |
| Props Destructuring | Cleaner code | None | Use in parameters for direct access |
| useState | Local state | Triggers re-renders | Keep state close to where it's used |
| Event Handlers | User interactions | Can cause issues if misused | Use function references, not calls |
| useCallback | Expensive handlers | Prevents unnecessary re-renders | For handlers passed to memoized children |
| Lifting State Up | Shared state | Minimal | Move to closest common ancestor only |

