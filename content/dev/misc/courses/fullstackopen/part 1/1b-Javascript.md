# JavaScript: Essential Modern Features

## 1. ES6 standards

JavaScript follows ECMAScript standards. ES6 (2015) through ES2024 (ES15) introduced modern features. 
- Babel transpiles newer JS to older versions for browser compatibility.
- **Use Case:** Write modern, clean code while ensuring compatibility across browsers and Node.js environments.
### Environment Setup

```bash
# Run JavaScript with Node.js
node filename.js

# Or use browser console / online tools like JS Bin
```

***

## 2. Variables: const, let, var

- **const:** Block-scoped constant, cannot be reassigned. **Use Case:** Default choice for values that won't change.[^4][^5][^6]
- **let:** Block-scoped variable, can be reassigned. **Use Case:** When you need to update the variable.[^5][^6][^4]
- **var:** Function/global-scoped, can be reassigned and redeclared. **Use Case:** Legacy code (avoid in modern development)

```js
const x = 1;           // Cannot be changed
let y = 5;             // Can be changed
var z = 10;            // Old way (avoid)

y += 10;               // ✅ Valid
// x = 4;              // ❌ Error - cannot reassign const
```

```js
// User authentication function
const authenticateUser = (credentials) => {
  let isValid = false;  // May change based on validation
  const API_URL = 'https://api.example.com/auth'; // Won't change
  
  if (credentials.username && credentials.password) {
    isValid = true;     // Reassigning let variable
  }
  
  return { isValid, API_URL };
};
```

***

## 3. Arrays \& Immutable Operations

1. Arrays store ordered lists. Modern React prefers immutable operations (concat, map, filter) over mutating methods (push, splice).
2. **Use Case:** Managing lists of data in React state without direct mutation.

```js
const t = [1, -1, 3];

// Mutating (avoid in React)
t.push(5);

// Immutable (preferred in React)
const t2 = t.concat(5);  // Returns new array

// Iteration
t.forEach(value => console.log(value));
```


E-commerce cart management:

```js
const CartManager = () => {
  const [cartItems, setCartItems] = useState([]);
  
  const addItem = (newItem) => {
    // Immutable update - creates new array
    setCartItems(prevItems => prevItems.concat(newItem));
  };
  
  const removeItem = (itemId) => {
    // Filter creates new array without mutation
    setCartItems(prevItems => prevItems.filter(item => item.id !== itemId));
  };
  
  return { cartItems, addItem, removeItem };
};
```

***

## 4. Array Methods: map, filter, reduce

### 1. map() - Transform Arrays

1. Creates new array by transforming each element with a function.
2. Converting data formats, rendering lists in React.

```js
const newArray = array.map(element => transformation);
```

```js
// Transform product data for display
const products = [
  { id: 1, price: 100, name: 'Laptop' },
  { id: 2, price: 50, name: 'Mouse' }
];

const productCards = products.map(product => ({
  ...product,
  displayPrice: `$${product.price}`,
  slug: product.name.toLowerCase()
}));

// In React JSX:
const ProductList = () => (
  <div>
    {products.map(product => (
      <ProductCard key={product.id} product={product} />
    ))}
  </div>
);
```


### 2. filter() - Select Elements

1. Creates new array with elements that pass a test function
2. Search functionality, conditional rendering.

```js
const filteredArray = array.filter(element => condition);
```

```js
// Search and filter users
const users = [
  { name: 'Alice', age: 25, role: 'admin' },
  { name: 'Bob', age: 30, role: 'user' },
  { name: 'Charlie', age: 22, role: 'user' }
];

const searchUsers = (searchTerm, minAge) => {
  return users
    .filter(user => user.name.toLowerCase().includes(searchTerm.toLowerCase()))
    .filter(user => user.age >= minAge);
};
```

```js
// Usage in React component
const UserSearch = () => {
  const [searchTerm, setSearchTerm] = useState('');
  const [minAge, setMinAge] = useState(18);
  
  const filteredUsers = users
    .filter(user => user.name.includes(searchTerm))
    .filter(user => user.age >= minAge);
  
  return (
    <div>
      {filteredUsers.map(user => <UserCard key={user.name} user={user} />)}
    </div>
  );
};
```


### 3. reduce() - Aggregate Data

1. Reduces array to single value by accumulating results.
2. Calculating totals, grouping data, complex transformations.

```js
const result = array.reduce((accumulator, currentValue) => {
  return newAccumulator;
}, initialValue);
```

```js
// Shopping cart calculations
const cartItems = [
  { name: 'Laptop', price: 1000, quantity: 1 },
  { name: 'Mouse', price: 25, quantity: 2 },
  { name: 'Keyboard', price: 75, quantity: 1 }
];

// Calculate total price
const totalPrice = cartItems.reduce((total, item) => {
  return total + (item.price * item.quantity);
}, 0);

// Group items by price range
const groupByPriceRange = cartItems.reduce((groups, item) => {
  const range = item.price > 100 ? 'expensive' : 'affordable';
  groups[range] = groups[range] || [];
  groups[range].push(item);
  return groups;
}, {});

console.log(totalPrice);      // 1125
console.log(groupByPriceRange); // { expensive: [...], affordable: [...] }
```


***
## 4. Array Destructuring

1. Extract values from arrays into variables using pattern matching.
2. Clean variable assignment, function return values, React hooks.

```js
const [first, second, ...rest] = array;
```

```js
// React Hook destructuring
const [count, setCount] = useState(0);
const [loading, setLoading] = useState(false);

// API response handling
const getUserData = async () => {
  const response = await fetch('/api/user');
  const [userData, permissions, preferences] = await response.json();
  
  return { userData, permissions, preferences };
};
```

```js
// Coordinate handling
const coordinates = [10, 20, 30];
const [x, y, z] = coordinates;

// Swapping variables
let a = 1, b = 2;
[a, b] = [b, a];  // a is now 2, b is now 1
```


***

## 5. Objects & Object Literals

1. Objects store key-value pairs. Properties accessed via dot notation or brackets
2. Representing complex data structures, API responses, component props.


```js
const object = {
  name: 'John',
  age: 30,
  'complex key': 'value'
};

// Access
console.log(object.name);           // Dot notation
console.log(object['complex key']); // Bracket notation

// Dynamic property addition
object.newProperty = 'new value';
object['another property'] = 'another value';
```

```js
// User profile management
const createUserProfile = (userData) => {
  const profile = {
    id: userData.id,
    name: {
      first: userData.firstName,
      last: userData.lastName
    },
    contact: {
      email: userData.email,
      phone: userData.phone
    },
    preferences: {
      theme: 'light',
      notifications: true
    },
    // Method (though we avoid this in modern React)
    getFullName() {
      return `${this.name.first} ${this.name.last}`;
    }
  };
  
  return profile;
};

// API response handling
// a function
const handleApiResponse = (response) => {
	// extract values for data, status and metadata
  const { data, status, metadata } = response;
	  // return 
  return {
    success: status === 200,
    userData: data.user,
    pagination: metadata.pagination,
    timestamp: Date.now()
  };
};
```

1. Objects can have methods. 
2. `this` refers to the object calling the method. 
3. Arrow functions don't have their own this.


```js
const person = {
  name: 'John',
  greet: function() {
    console.log('Hello, ' + this.name);  // 'this' refers to person object
  },
  arrowGreet: () => {
    console.log('Hello, ' + this.name);  // 'this' is undefined in arrow function
  }
};
```

***

## 6. Functions: Arrow Functions vs Regular Functions

- **Arrow Functions:** 
	- Concise syntax, lexical `this` binding.
	- preferred for callbacks, event handlers;

```js
// Arrow function (preferred)
const add = (a, b) => a + b;
const square = x => x * x;                    // Single param, no parentheses
const complex = (a, b) => {                   // Multiple statements
  const result = a * b;
  return result + 10;
};
```

- **Regular Functions:**
	- Traditional syntax, own `this` context.
	- regular functions for methods.

```js
// Regular function
function multiply(a, b) {
  return a * b;
}
```

example:

```js
// Event handling in React
const LoginForm = () => {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  
  // Arrow function for event handler
  const handleSubmit = (e) => {
    e.preventDefault();
    authenticateUser({ email, password });
  };
  
  // Arrow function for input handlers
  const handleEmailChange = (e) => setEmail(e.target.value);
  const handlePasswordChange = (e) => setPassword(e.target.value);
  
  return (
    <form onSubmit={handleSubmit}>
      <input value={email} onChange={handleEmailChange} />
      <input value={password} onChange={handlePasswordChange} />
      <button type="submit">Login</button>
    </form>
  );
};

// Array processing
const processUsers = (users) => {
  return users
    .filter(user => user.active)
    .map(user => ({
      id: user.id,
      displayName: `${user.firstName} ${user.lastName}`,
      role: user.role
    }))
    .sort((a, b) => a.displayName.localeCompare(b.displayName));
};
```

***

## ES6 Classes

1. Syntactic sugar over prototypal inheritance. Creates constructor functions and methods.
2. **Use Case:** Legacy React components, Node.js applications, understanding older codebases.

Syntax:

```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  
  greet() {
    console.log(`Hello, I'm ${this.name}`);
  }
  
  static createAnonymous() {
    return new Person('Anonymous', 0);
  }
}

const john = new Person('John', 30);
john.greet();
```

e.g 

```js
// API service class
class ApiService {
  constructor(baseUrl, apiKey) {
    this.baseUrl = baseUrl;
    this.apiKey = apiKey;
  }
  
  async get(endpoint) {
    const response = await fetch(`${this.baseUrl}/${endpoint}`, {
      headers: {
        'Authorization': `Bearer ${this.apiKey}`,
        'Content-Type': 'application/json'
      }
    });
    return response.json();
  }
  
  async post(endpoint, data) {
    const response = await fetch(`${this.baseUrl}/${endpoint}`, {
      method: 'POST',
      headers: {
        'Authorization': `Bearer ${this.apiKey}`,
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(data)
    });
    return response.json();
  }
}

// Usage
const api = new ApiService('https://api.example.com', 'your-api-key');
const userData = await api.get('users/123');
const createdUser = await api.post('users', { name: 'John', email: 'john@example.com' });
```


***
## summary

| Concept | Modern Usage | MERN Stack Relevance |
| :-- | :-- | :-- |
| `const`/`let` | Default choice for variables | React state, props, API responses |
| Arrow Functions | Primary function syntax | Event handlers, array methods, React components |
| Array Methods | `map()`, `filter()`, `reduce()` | Rendering lists, data transformation, state updates |
| Object Destructuring | Extract props, API responses | React props, useState, API handling |
| Template Literals | String interpolation | Dynamic content, API URLs, JSX |
| Spread Operator | Copy arrays/objects | Immutable state updates |
| Classes | Legacy understanding | Old React components, Node.js services |


***
## Exercise Solutions (1.3-1.5)

### Exercise 1.3: Using Objects

```js
const App = () => {
  const course = 'Half Stack application development';
  const part1 = {
    name: 'Fundamentals of React',
    exercises: 10
  };
  const part2 = {
    name: 'Using props to pass data',
    exercises: 7
  };
  const part3 = {
    name: 'State of a component',
    exercises: 14
  };

  return (
    <div>
      <Header course={course} />
      <Content part1={part1} part2={part2} part3={part3} />
      <Total part1={part1} part2={part2} part3={part3} />
    </div>
  );
};

const Content = ({ part1, part2, part3 }) => (
  <div>
    <Part part={part1} />
    <Part part={part2} />
    <Part part={part3} />
  </div>
);

const Part = ({ part }) => (
  <p>{part.name} {part.exercises}</p>
);

const Total = ({ part1, part2, part3 }) => (
  <p>Number of exercises {part1.exercises + part2.exercises + part3.exercises}</p>
);
```


### Exercise 1.4: Using Arrays

```js
const App = () => {
  const course = 'Half Stack application development';
  const parts = [
    {
      name: 'Fundamentals of React',
      exercises: 10
    },
    {
      name: 'Using props to pass data',
      exercises: 7
    },
    {
      name: 'State of a component',
      exercises: 14
    }
  ];

  return (
    <div>
      <Header course={course} />
      <Content parts={parts} />
      <Total parts={parts} />
    </div>
  );
};

const Content = ({ parts }) => (
  <div>
    <Part part={parts[^0]} />
    <Part part={parts[^1]} />
    <Part part={parts[^2]} />
  </div>
);

const Total = ({ parts }) => (
  <p>Number of exercises {parts[^0].exercises + parts[^1].exercises + parts[^2].exercises}</p>
);
```


### Exercise 1.5: Single Object Structure

```js
const App = () => {
  const course = {
    name: 'Half Stack application development',
    parts: [
      {
        name: 'Fundamentals of React',
        exercises: 10
      },
      {
        name: 'Using props to pass data',
        exercises: 7
      },
      {
        name: 'State of a component',
        exercises: 14
      }
    ]
  };

  return (
    <div>
      <Header course={course.name} />
      <Content parts={course.parts} />
      <Total parts={course.parts} />
    </div>
  );
};
```


***
