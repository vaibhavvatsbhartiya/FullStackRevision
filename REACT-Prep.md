# Comprehensive Guide to React.js - Fundamental and Advanced Concepts

## 1. Introduction to React

### What is React?
- React is a JavaScript library developed by Facebook (now Meta) for building user interfaces
- It focuses on creating reusable UI components that efficiently update when data changes
- React uses a Virtual DOM to optimize rendering performance
- Released in 2013, it has become one of the most popular frontend frameworks

### Features and Benefits
- **Component-Based Architecture**: Encourages modular, reusable code
- **Declarative Syntax**: Describe what the UI should look like based on the current state
- **Virtual DOM**: Efficiently updates only what's necessary in the real DOM
- **Unidirectional Data Flow**: Makes applications more predictable and easier to debug
- **Strong Community Support**: Vast ecosystem of libraries, tools, and extensions
- **React Native**: Enables building mobile applications with the same concepts

## 2. JSX (JavaScript XML)

### Syntax and Usage
- JSX is a syntax extension for JavaScript that looks similar to XML/HTML
- It allows you to write HTML-like code in your JavaScript files
- JSX gets transpiled to regular JavaScript by tools like Babel
- Makes component structure more readable and intuitive
- Requires parentheses for multi-line JSX expressions

### Code Example

```jsx
// JSX syntax
const element = (
  <div className="greeting">
    <h1>Hello, world!</h1>
    <p>Welcome to React</p>
  </div>
);

// After transpilation (simplified):
const element = React.createElement(
  'div',
  { className: 'greeting' },
  React.createElement('h1', null, 'Hello, world!'),
  React.createElement('p', null, 'Welcome to React')
);
```

- JSX expressions can include JavaScript logic using curly braces:

```jsx
function Greeting({ user }) {
  return (
    <div>
      {user ? (
        <h1>Hello, {user.name}!</h1>
      ) : (
        <h1>Hello, Stranger!</h1>
      )}
    </div>
  );
}
```

## 3. Components in React

### Functional Components
- Simple JavaScript functions that return JSX
- Modern approach to React components
- Originally stateless, but can now use hooks for state and lifecycle features

```jsx
function Welcome({ name }) {
  return <h1>Hello, {name}</h1>;
}

// Using arrow function syntax
const Welcome = ({ name }) => <h1>Hello, {name}</h1>;
```

### Class Components
- ES6 classes that extend from React.Component
- Traditional way to create stateful components before hooks
- Include lifecycle methods and state management

```jsx
import React, { Component } from 'react';

class Welcome extends Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

### Props and State

#### Props
- Short for "properties"
- Read-only data passed from parent to child components
- Helps make components reusable
- Passed as attributes in JSX

```jsx
// Parent component passing props
function App() {
  return <Welcome name="Sara" age={25} />;
}

// Child component receiving props
function Welcome({ name, age }) {
  return (
    <div>
      <h1>Hello, {name}!</h1>
      <p>You are {age} years old.</p>
    </div>
  );
}
```

#### State
- Private data controlled by the component
- Can change over time in response to user actions or events
- In class components, state is managed via `this.state` and `this.setState()`
- In functional components, state is managed via the `useState` hook

```jsx
// Class component with state
class Counter extends Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  }
  
  increment = () => {
    this.setState({ count: this.state.count + 1 });
  };
  
  render() {
    return (
      <div>
        <p>Count: {this.state.count}</p>
        <button onClick={this.increment}>Increment</button>
      </div>
    );
  }
}

// Functional component with state hook
function Counter() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

## 4. React Lifecycle Methods

### Lifecycle Phases in Class Components
Class components have several lifecycle methods that execute at specific phases:

#### Mounting Phase
- `constructor()`: Initialize state and bind methods
- `static getDerivedStateFromProps()`: Update state based on props
- `render()`: Create the component's JSX
- `componentDidMount()`: Run after component is inserted into the DOM

#### Updating Phase
- `static getDerivedStateFromProps()`
- `shouldComponentUpdate()`: Decide if re-rendering is needed
- `render()`
- `getSnapshotBeforeUpdate()`: Capture DOM information before changes
- `componentDidUpdate()`: Run after component updates

#### Unmounting Phase
- `componentWillUnmount()`: Clean up before component removal

#### Error Handling
- `static getDerivedStateFromError()`
- `componentDidCatch()`

### Example with Lifecycle Methods

```jsx
class Clock extends Component {
  constructor(props) {
    super(props);
    this.state = { date: new Date() };
    console.log('Constructor called');
  }
  
  componentDidMount() {
    console.log('Component mounted');
    this.timerID = setInterval(() => this.tick(), 1000);
  }
  
  componentDidUpdate() {
    console.log('Component updated');
  }
  
  componentWillUnmount() {
    console.log('Component will unmount');
    clearInterval(this.timerID);
  }
  
  tick() {
    this.setState({ date: new Date() });
  }
  
  render() {
    console.log('Render called');
    return (
      <div>
        <h2>Current time: {this.state.date.toLocaleTimeString()}</h2>
      </div>
    );
  }
}
```

## 5. React Hooks

### Basic Hooks

#### useState
Manages component state in functional components.

```jsx
import React, { useState } from 'react';

function Counter() {
  // Declare state variable 'count' with initial value 0
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

#### useEffect
Handles side effects like API calls, subscriptions, and DOM mutations.

```jsx
import React, { useState, useEffect } from 'react';

function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    // This runs after first render and when userId changes
    setLoading(true);
    fetch(`https://api.example.com/users/${userId}`)
      .then(response => response.json())
      .then(data => {
        setUser(data);
        setLoading(false);
      });
    
    // Cleanup function (runs before effect re-runs or component unmounts)
    return () => {
      // Cancel pending requests or clean up subscriptions
      console.log('Cleaning up before re-fetching or unmounting');
    };
  }, [userId]); // Dependency array - effect runs when these values change
  
  if (loading) return <div>Loading...</div>;
  
  return (
    <div>
      <h1>{user.name}</h1>
      <p>Email: {user.email}</p>
    </div>
  );
}
```

#### useContext
Accesses values from React Context without nesting.

```jsx
import React, { useContext } from 'react';

// Create a context
const ThemeContext = React.createContext('light');

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function Toolbar() {
  return <ThemedButton />;
}

function ThemedButton() {
  // Consume the context value
  const theme = useContext(ThemeContext);
  
  return (
    <button style={{ background: theme === 'dark' ? '#333' : '#fff', 
                     color: theme === 'dark' ? '#fff' : '#333' }}>
      I am styled by theme context!
    </button>
  );
}
```

### Additional Hooks

#### useReducer
Alternative to useState for complex state logic.

```jsx
import React, { useReducer } from 'react';

// Reducer function
function counterReducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    case 'reset':
      return { count: 0 };
    case 'add':
      return { count: state.count + action.payload };
    default:
      throw new Error('Unknown action');
  }
}

function Counter() {
  // useReducer returns current state and dispatch function
  const [state, dispatch] = useReducer(counterReducer, { count: 0 });
  
  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
      <button onClick={() => dispatch({ type: 'reset' })}>Reset</button>
      <button onClick={() => dispatch({ type: 'add', payload: 5 })}>Add 5</button>
    </div>
  );
}
```

#### useRef
Creates a mutable reference that persists across renders.

```jsx
import React, { useRef, useEffect } from 'react';

function FocusInput() {
  // Create a ref
  const inputRef = useRef(null);
  
  // Focus the input after component mounts
  useEffect(() => {
    inputRef.current.focus();
  }, []);
  
  // Track a value without causing re-renders
  const renderCount = useRef(1);
  
  useEffect(() => {
    renderCount.current = renderCount.current + 1;
    console.log(`Component rendered ${renderCount.current} times`);
  });
  
  return (
    <div>
      <input ref={inputRef} placeholder="I will be focused on mount" />
      <p>This component has rendered {renderCount.current} times</p>
    </div>
  );
}
```

#### useMemo
Optimizes performance by memoizing computed values.

```jsx
import React, { useState, useMemo } from 'react';

function ExpensiveCalculation({ numbers }) {
  const [count, setCount] = useState(0);
  
  // This calculation will only re-run if 'numbers' changes
  const sum = useMemo(() => {
    console.log('Computing sum...');
    return numbers.reduce((acc, num) => acc + num, 0);
  }, [numbers]);
  
  return (
    <div>
      <p>Sum: {sum}</p>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}
```

#### useCallback
Memoizes functions to prevent unnecessary re-renders in child components.

```jsx
import React, { useState, useCallback } from 'react';

function ParentComponent() {
  const [count, setCount] = useState(0);
  const [todos, setTodos] = useState([]);
  
  // This function is memoized and only changes if dependencies change
  const addTodo = useCallback(() => {
    setTodos(prev => [...prev, `New Todo ${prev.length + 1}`]);
  }, []);
  
  return (
    <div>
      <ChildComponent todos={todos} addTodo={addTodo} />
      <div>
        <p>Count: {count}</p>
        <button onClick={() => setCount(count + 1)}>Increment</button>
      </div>
    </div>
  );
}

// React.memo prevents re-rendering if props haven't changed
const ChildComponent = React.memo(({ todos, addTodo }) => {
  console.log('Child component rendered');
  
  return (
    <div>
      <button onClick={addTodo}>Add Todo</button>
      <ul>
        {todos.map((todo, index) => (
          <li key={index}>{todo}</li>
        ))}
      </ul>
    </div>
  );
});
```

### Custom Hooks

Custom hooks are JavaScript functions that start with "use" and may call other hooks. They allow you to extract component logic into reusable functions.

#### Creating a Custom Hook

```jsx
// Custom hook for form handling
import { useState } from 'react';

function useForm(initialValues) {
  const [values, setValues] = useState(initialValues);
  
  const handleChange = (e) => {
    const { name, value } = e.target;
    setValues({
      ...values,
      [name]: value
    });
  };
  
  const resetForm = () => {
    setValues(initialValues);
  };
  
  return {
    values,
    handleChange,
    resetForm
  };
}

// Using the custom hook
function SignupForm() {
  const { values, handleChange, resetForm } = useForm({
    username: '',
    email: '',
    password: ''
  });
  
  const handleSubmit = (e) => {
    e.preventDefault();
    console.log('Form submitted with:', values);
    // Submit to API here
    resetForm();
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label>Username:</label>
        <input
          type="text"
          name="username"
          value={values.username}
          onChange={handleChange}
        />
      </div>
      <div>
        <label>Email:</label>
        <input
          type="email"
          name="email"
          value={values.email}
          onChange={handleChange}
        />
      </div>
      <div>
        <label>Password:</label>
        <input
          type="password"
          name="password"
          value={values.password}
          onChange={handleChange}
        />
      </div>
      <button type="submit">Sign Up</button>
      <button type="button" onClick={resetForm}>Reset</button>
    </form>
  );
}
```

#### Another Custom Hook Example - useFetch

```jsx
import { useState, useEffect } from 'react';

function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    const abortController = new AbortController();
    
    const fetchData = async () => {
      try {
        setLoading(true);
        const response = await fetch(url, { signal: abortController.signal });
        
        if (!response.ok) {
          throw new Error(`HTTP error! Status: ${response.status}`);
        }
        
        const result = await response.json();
        setData(result);
        setError(null);
      } catch (err) {
        if (err.name !== 'AbortError') {
          setError('An error occurred: ' + err.message);
          setData(null);
        }
      } finally {
        setLoading(false);
      }
    };
    
    fetchData();
    
    return () => {
      abortController.abort();
    };
  }, [url]);
  
  return { data, loading, error };