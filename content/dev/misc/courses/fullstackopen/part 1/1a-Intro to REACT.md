
## 1. Setup

Vite is a fast build tool to scaffold and run modern web apps quickly. It supports React out of the box and provides a development server with hot module replacement

```bash
# Create new React app with Vite (npm 7+)
npm create vite@latest introdemo -- --template react
cd introdemo
npm install
npm run dev
```

The dev server runs at: `http://localhost:5173` (default port, auto-increments if busy).

***
## 2. App structure

1. `src/` holds React component code; index.html is the main HTML entry point. 
2. `index.html` contains `<div id="root"></div>` - the mount point for the React app
3. `main.jsx` renders your root React component into this `#root` div.

`index.html`

```html
<body>
  <div id="root"></div>
  <script type="module" src="/src/main.jsx"></script>
</body>
```

`main.jsx`

```jsx
import ReactDOM from 'react-dom/client'
import App from './App'
ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```


***

## 3. React Component

1. React components are functions (often arrow functions) returning JSX, describing UI.
2. Encapsulate rendering logic and UI for reusability and composability.

Basic `App.jsx` component:

```jsx
const App = () => {
  console.log('Hello from component') // Output visible in browser console
  return (
    <div>
      <p>Hello world</p>
    </div>
  )
}
```

***

## 4. Dynamic Rendering with JSX

1. JSX embeds JavaScript expressions within {} inside markup for dynamic UI.
2. Display changing data, compute values inline.
3. Shows current time and computed values on screen, updating each render if the state or prop changes.


```jsx
const App = () => {
  const now = new Date()
  const a = 10, b = 20
  return (
    <div>
      <p>Hello world, it is {now.toString()}</p>
      <p>{a} plus {b} is {a + b}</p>
    </div>
  )
}
```

4. JSX compiles to `React.createElement` calls which produce virtual DOM elements.
5. ffers a familiar HTML-like syntax but compiled into efficient .JS calls by [[babel]].

compiled `.jsx` -> `.js`

```js
const App = () => {
  return React.createElement(
    'div',
    null,
    React.createElement('p', null, 'Hello world, it is ', new Date().toString()),
    React.createElement('p', null, 10, ' plus ', 20, ' is ', 30)
  )
}
```

## 5. Multiple Components & Composition

1. React apps are composed by combining multiple components, which Enables modularity and reuse.
2. Reusable UI elements like buttons, cards, or form inputs implemented as small components then assembled.
3. React components must start with a capital letter; otherwise React treats lowercase tag names as DOM elements.
4. JSX elements must have one root element or use React Fragments `<>...</>`.

Define two components and use one inside the other:

```jsx
const Hello = () => (
  <div><p>Hello world</p></div>
)
const App = () => (
  <div>
    <h1>Greetings</h1>
    <Hello />
    <Hello />
    <Hello />
  </div>
)
```


## 6. Props: Passing Data to Components

1. Props are arbitrary data passed from parent to child components for customization.
2. Components behave differently depending on input props.

```jsx
const Hello = (props) => {
  return (
    <div>
      <p>Hello {props.name}</p>
    </div>
  )
}

const App = () => (
  <div>
    <Hello name="George" />
    <Hello name="Daisy" />
  </div>
)
```

## 7. Rendering Primitive Types, Not Objects

1. React cannot directly render plain objects; 
2. it expects strings, numbers, arrays of primitives, or React elements.
3. Use object property access to render UI safely.

```jsx
// friend is { name: 'Peter', age: 4 }, will cause error
<p>{friend}</p> 

// correct
<p>{friend.name} {friend.age}</p>
```

### iterating over a list : using the map function

```jsx
const friends = [
  { name: 'Peter', age: 4 },
  { name: 'Maya', age: 10 },
]
return (
  <div>
    {friends.map(f =>
	    <p key={f.name}>
		    {f.name} {f.age}
		</p>
	)}
  </div>
)
```


***

## 11. Exercises 1.1 \& 1.2: Course Information App

```jsx
const Header = ({ course }) => <h1>{course}</h1>;

const Part = ({ name, exercises }) => (
  <p>{name} {exercises}</p>
)

const Content = ({ parts }) => (
  <div>
    {parts.map(p => <Part key={p.name} name={p.name} exercises={p.exercises} />)}
  </div>
);

const Total = ({ parts }) => (
  <p>Number of exercises {parts.reduce((sum, p) => sum + p.exercises, 0)}</p>
);

const App = () => {
  const course = 'Half Stack application development';
  const parts = [
    { name: 'Fundamentals of React', exercises: 10 },
    { name: 'Using props to pass data', exercises: 7 },
    { name: 'State of a component', exercises: 14 }
  ];
  
  return (
    <div>
      <Header course={course}/>
      <Content parts={parts}/>
      <Total parts={parts}/>
    </div>
  );
}

export default App;
```
