# JavaScript Fundamentals

## Variables and Data Types

**Variables**
* Declared using `let`, `const`, or `var` (older)
* Block-scoped (`let`/`const`) vs function-scoped (`var`)

```javascript
// Modern variable declarations
let count = 0;           // Mutable, block-scoped
const PI = 3.14159;      // Immutable, block-scoped
var legacy = "old";      // Function-scoped (avoid when possible)
```

**Primitive Data Types**
* `string`: Text values
* `number`: Integers and floating-point
* `boolean`: `true` or `false`
* `undefined`: Value not assigned
* `null`: Intentional absence of value
* `symbol`: Unique identifiers
* `bigint`: Large integers

```javascript
const name = "John";             // string
const age = 30;                  // number
const isActive = true;           // boolean
const nothing = null;            // null
let notDefined;                  // undefined
const uniqueId = Symbol("id");   // symbol
const bigNumber = 9007199254740991n; // bigint
```

**Reference Types**
* `object`: Collection of key-value pairs
* `array`: Ordered list of values
* `function`: Callable objects

```javascript
// Object
const person = {
  name: "Alice",
  age: 25
};

// Array
const colors = ["red", "green", "blue"];

// Function
const greet = function(name) {
  return `Hello, ${name}!`;
};
```

## Operators and Expressions

**Arithmetic Operators**
* `+`, `-`, `*`, `/`, `%` (modulo), `**` (exponentiation)

```javascript
const sum = 5 + 3;       // 8
const remainder = 10 % 3; // 1
const power = 2 ** 3;     // 8
```

**Comparison Operators**
* `==`, `===` (strict equality), `!=`, `!==`, `>`, `<`, `>=`, `<=`

```javascript
5 === "5";  // false (strict - different types)
5 == "5";   // true (loose - converts types)
```

**Logical Operators**
* `&&` (AND), `||` (OR), `!` (NOT)
* Short-circuit evaluation

```javascript
const a = true && false;  // false
const b = true || false;  // true
const c = !true;          // false

// Short-circuit
const name = user?.name || "Guest";
```

## Control Flow

**Conditional Statements**
* `if`, `else if`, `else`
* Ternary operator: `condition ? trueExpr : falseExpr` 
* `switch` statement

```javascript
// If statements
if (age >= 18) {
  console.log("Adult");
} else if (age >= 13) {
  console.log("Teenager");
} else {
  console.log("Child");
}

// Ternary operator
const status = age >= 18 ? "Adult" : "Minor";

// Switch statement
switch (day) {
  case "Monday":
    console.log("Start of work week");
    break;
  case "Friday":
    console.log("End of work week");
    break;
  default:
    console.log("Another day");
}
```

**Loops**
* `for`, `while`, `do-while`
* `for...of` (arrays), `for...in` (objects)

```javascript
// Standard for loop
for (let i = 0; i < 5; i++) {
  console.log(i);
}

// While loop
let i = 0;
while (i < 5) {
  console.log(i);
  i++;
}

// For...of (iterables)
const numbers = [1, 2, 3];
for (const num of numbers) {
  console.log(num);
}

// For...in (object properties)
const user = { name: "John", age: 30 };
for (const key in user) {
  console.log(`${key}: ${user[key]}`);
}
```

# Functions

## Function Basics

**Function Declaration**
* Hoisted (can be called before declaration)
* Creates named function

```javascript
function add(a, b) {
  return a + b;
}
```

**Function Expression**
* Not hoisted
* Can be anonymous or named

```javascript
const subtract = function(a, b) {
  return a - b;
};
```

**Arrow Functions**
* Shorter syntax
* Lexical `this`
* No `arguments` object

```javascript
const multiply = (a, b) => a * b;

// With block body
const divide = (a, b) => {
  if (b === 0) throw new Error("Cannot divide by zero");
  return a / b;
};
```

## Advanced Function Concepts

**Parameters & Arguments**
* Default parameters
* Rest parameters
* Parameter destructuring

```javascript
// Default parameters
function greet(name = "Guest") {
  return `Hello, ${name}!`;
}

// Rest parameters
function sum(...numbers) {
  return numbers.reduce((total, num) => total + num, 0);
}

// Parameter destructuring
function processUser({name, age, email}) {
  console.log(`${name} (${age}): ${email}`);
}
```

**Closures**
* Function that remembers its lexical environment
* Used for data privacy and stateful functions

```javascript
function createCounter() {
  let count = 0;  // Private variable
  
  return {
    increment() { count++; },
    decrement() { count--; },
    getValue() { return count; }
  };
}

const counter = createCounter();
counter.increment();
counter.increment();
console.log(counter.getValue());  // 2
```

**Higher-Order Functions**
* Functions that take/return functions

```javascript
// Function that returns a function
function multiplier(factor) {
  return function(number) {
    return number * factor;
  };
}

const double = multiplier(2);
console.log(double(5));  // 10
```

# Objects and Object-Oriented Programming

## Object Basics

**Object Creation**
* Object literals
* Constructor functions
* `Object.create()`

```javascript
// Object literal
const person = {
  name: "Alice",
  greet() {
    return `Hello, I'm ${this.name}`;
  }
};

// Constructor function
function Person(name) {
  this.name = name;
  this.greet = function() {
    return `Hello, I'm ${this.name}`;
  };
}
const john = new Person("John");

// Object.create()
const employee = Object.create(person);
employee.role = "Developer";
```

**Property Operations**
* Adding, accessing, modifying, deleting properties
* Property descriptors and `Object.defineProperty()`

```javascript
// Property access
person.name;        // Dot notation
person["name"];     // Bracket notation

// Property descriptor
Object.defineProperty(person, "age", {
  value: 30,
  writable: false,   // Cannot change value
  enumerable: true,  // Shows up in for...in
  configurable: true // Can delete or reconfigure
});
```

## Prototypes and Inheritance

**Prototype Chain**
* Objects inheriting from other objects
* `Object.getPrototypeOf()`, `Object.setPrototypeOf()`

```javascript
// Prototype inheritance
function Animal(name) {
  this.name = name;
}

Animal.prototype.makeSound = function() {
  return "Some generic sound";
};

function Dog(name) {
  Animal.call(this, name);
}

// Set up inheritance
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

// Override method
Dog.prototype.makeSound = function() {
  return "Woof!";
};

const rex = new Dog("Rex");
console.log(rex.makeSound());  // "Woof!"
```

**ES6 Classes**
* Syntactic sugar over prototype-based inheritance
* Constructors, methods, static methods, getters/setters

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }
  
  makeSound() {
    return "Some generic sound";
  }
  
  get species() {
    return this._species;
  }
  
  set species(value) {
    this._species = value;
  }
  
  static isAnimal(obj) {
    return obj instanceof Animal;
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name);
    this.breed = breed;
  }
  
  makeSound() {
    return "Woof!";
  }
}

const spot = new Dog("Spot", "Beagle");
```

# Arrays and Collection Methods

## Array Basics

**Creation and Access**
* Array literals, `Array()` constructor
* Indices start at 0

```javascript
const fruits = ["apple", "banana", "orange"];
const empty = new Array(3);  // [undefined, undefined, undefined]

console.log(fruits[0]);      // "apple"
console.log(fruits.length);  // 3
```

**Modification Methods**
* `push`, `pop`, `shift`, `unshift`, `splice`

```javascript
fruits.push("grape");        // Add to end
const last = fruits.pop();   // Remove from end
const first = fruits.shift(); // Remove from start
fruits.unshift("kiwi");      // Add to start
fruits.splice(1, 2, "pear"); // Remove 2 items at index 1, insert "pear"
```

## Array Iteration Methods

**Functional Array Methods**
* `forEach`, `map`, `filter`, `reduce`, `find`, `some`, `every`
* Non-mutating, return new arrays or values

```javascript
const numbers = [1, 2, 3, 4, 5];

// forEach - just iterates
numbers.forEach(num => console.log(num));

// map - transforms each element
const doubled = numbers.map(num => num * 2);  // [2, 4, 6, 8, 10]

// filter - keeps elements passing test
const evens = numbers.filter(num => num % 2 === 0);  // [2, 4]

// reduce - accumulates to single value
const sum = numbers.reduce((total, num) => total + num, 0);  // 15

// find - returns first match
const found = numbers.find(num => num > 3);  // 4

// some/every - test conditions
const hasEven = numbers.some(num => num % 2 === 0);  // true
const allPositive = numbers.every(num => num > 0);   // true
```

# Asynchronous JavaScript

## Callbacks and Event Loop

**Callbacks**
* Functions passed as arguments
* Traditional way to handle async operations

```javascript
function fetchData(callback) {
  setTimeout(() => {
    const data = { id: 1, name: "Product" };
    callback(data);
  }, 1000);
}

fetchData(data => {
  console.log(data);  // Runs after 1 second
});
```

**Event Loop**
* Single-threaded but non-blocking
* Call stack, callback queue, event loop mechanism

## Promises

**Promise States**
* `pending`, `fulfilled`, `rejected`
* `then()`, `catch()`, `finally()`

```javascript
function fetchUser() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      const user = { id: 1, name: "John" };
      if (user) {
        resolve(user);
      } else {
        reject(new Error("User not found"));
      }
    }, 1000);
  });
}

fetchUser()
  .then(user => console.log(user))
  .catch(error => console.error(error))
  .finally(() => console.log("Operation complete"));
```

**Promise Chaining**
* Return values passed to next `then()`
* Error propagation

```javascript
fetchUser()
  .then(user => fetchUserPosts(user.id))
  .then(posts => fetchPostComments(posts[0].id))
  .then(comments => console.log(comments))
  .catch(error => console.error("Error in chain:", error));
```

**Promise Combinators**
* `Promise.all()`, `Promise.race()`, `Promise.allSettled()`, `Promise.any()`

```javascript
// Wait for all to complete
Promise.all([fetchUser(), fetchPosts(), fetchComments()])
  .then(([user, posts, comments]) => {
    // All resolved
  });

// Take first to complete
Promise.race([fetchFromAPI1(), fetchFromAPI2()])
  .then(result => console.log("First response:", result));
```

## Async/Await

**Basic Syntax**
* Syntactic sugar over promises
* Makes async code look synchronous

```javascript
async function getUserData() {
  try {
    const user = await fetchUser();
    const posts = await fetchUserPosts(user.id);
    const comments = await fetchPostComments(posts[0].id);
    
    return { user, posts, comments };
  } catch (error) {
    console.error("Error fetching data:", error);
  }
}

// Using the async function
getUserData().then(data => console.log(data));
```

**Parallel Operations**
* Combining with `Promise.all()`

```javascript
async function getUserAndPosts(userId) {
  try {
    // Run in parallel
    const [user, posts] = await Promise.all([
      fetchUser(userId),
      fetchPosts(userId)
    ]);
    
    return { user, posts };
  } catch (error) {
    console.error("Error:", error);
  }
}
```

# DOM Manipulation

## DOM Selection

**Selecting Elements**
* `getElementById`, `querySelector`, `querySelectorAll`

```javascript
// Get by ID
const header = document.getElementById("header");

// Query selector (returns first match)
const button = document.querySelector(".btn-primary");

// Query selector all (returns NodeList)
const paragraphs = document.querySelectorAll("p");
```

## DOM Modification

**Creating and Modifying**
* Creating elements
* Modifying content and attributes
* Manipulating classes and styles

```javascript
// Create element
const div = document.createElement("div");
div.textContent = "Hello World";
div.className = "container";

// Append to DOM
document.body.appendChild(div);

// Modify existing element
header.innerHTML = "<span>New Content</span>";
header.setAttribute("data-info", "important");

// Classes and styles
button.classList.add("active");
button.classList.remove("disabled");
button.style.backgroundColor = "blue";
```

## Event Handling

**Event Listeners**
* `addEventListener`
* Event bubbling and capturing
* Preventing default behavior

```javascript
// Add event listener
button.addEventListener("click", function(event) {
  console.log("Button clicked!");
  event.preventDefault();  // Prevent default action
});

// Using event delegation
document.getElementById("list").addEventListener("click", function(event) {
  if (event.target.tagName === "LI") {
    console.log("List item clicked:", event.target.textContent);
  }
});
```

# Advanced JavaScript Topics

## Modules

**ES Modules**
* `import` and `export` 
* Named vs default exports

```javascript
// math.js
export const PI = 3.14159;
export function add(a, b) { return a + b; }
export default function multiply(a, b) { return a * b; }

// app.js
import multiply, { PI, add as sum } from './math.js';
console.log(PI);          // 3.14159
console.log(sum(2, 3));   // 5
console.log(multiply(2, 3)); // 6
```

## Error Handling 

```javascript
try {
  const result = riskyOperation();
  console.log("Success:", result);
} catch (error) {
  console.error("Error caught:", error.message);
} finally {
  console.log("This runs regardless of success or failure");
}

// Custom error types
class ValidationError extends Error {
  constructor(message) {
    super(message);
    this.name = "ValidationError";
  }
}

try {
  throw new ValidationError("Invalid input");
} catch (error) {
  if (error instanceof ValidationError) {
    console.error("Validation failed:", error.message);
  } else {
    console.error("Different error:", error);
  }
}
```

## Garbage Collection & Memory Management

**Memory Lifecycle**
* Allocation, use, release
* Garbage collector identifies and clears unused objects

**Memory Leaks**
* Common causes: uncleared event listeners, closures, global variables
* Prevention techniques

```javascript
// Potential memory leak (not cleaning up)
function setupEvent() {
  const button = document.getElementById("myButton");
  button.addEventListener("click", function() {
    console.log("Clicked");
  });
}

// Fixed version (cleanup on removal)
function setupWithCleanup() {
  const button = document.getElementById("myButton");
  const handler = function() { console.log("Clicked"); };
  
  button.addEventListener("click", handler);
  
  // Return cleanup function
  return function cleanup() {
    button.removeEventListener("click", handler);
  };
}
```

## Regular Expressions

**Pattern Creation and Usage**
* Literal notation vs constructor
* Common methods: `test`, `exec`, `match`, `replace`

```javascript
// Regex literal
const emailRegex = /^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$/;

// Using the RegExp constructor
const nameRegex = new RegExp("^[A-Za-z\\s]{2,30}$");

// Testing for matches
emailRegex.test("user@example.com");  // true

// String methods with regex
const text = "Contact me at john@example.com or visit example.com";
const emails = text.match(/[\w-\.]+@([\w-]+\.)+[\w-]{2,4}/g);
const replaced = text.replace(/example\.com/g, "mysite.com");
```

## Advanced Design Patterns

**Module Pattern**
* Encapsulation through closures

```javascript
const calculator = (function() {
  // Private variables
  let result = 0;
  
  // Public interface
  return {
    add(x) {
      result += x;
      return this;
    },
    subtract(x) {
      result -= x;
      return this;
    },
    getResult() {
      return result;
    }
  };
})();

calculator.add(5).subtract(2);
console.log(calculator.getResult());  // 3
```

**Singleton Pattern**
* Ensures only one instance exists

```javascript
const Database = (function() {
  let instance;
  
  function createInstance() {
    return {
      data: {},
      add(key, value) {
        this.data[key] = value;
      },
      get(key) {
        return this.data[key];
      }
    };
  }
  
  return {
    getInstance() {
      if (!instance) {
        instance = createInstance();
      }
      return instance;
    }
  };
})();

const db1 = Database.getInstance();
const db2 = Database.getInstance();
console.log(db1 === db2);  // true
```

**Observer Pattern**
* Subject maintains list of dependents (observers)
* Notifies them of state changes

```javascript
class Subject {
  constructor() {
    this.observers = [];
  }
  
  subscribe(observer) {
    this.observers.push(observer);
  }
  
  unsubscribe(observer) {
    this.observers = this.observers.filter(obs => obs !== observer);
  }
  
  notify(data) {
    this.observers.forEach(observer => observer.update(data));
  }
}

class Observer {
  update(data) {
    console.log("Data received:", data);
  }
}

const subject = new Subject();
const observer1 = new Observer();
const observer2 = new Observer();

subject.subscribe(observer1);
subject.subscribe(observer2);
subject.notify("New data!");
```

# Modern JavaScript Features

## ES6+ Features

**Destructuring**
* Arrays and objects

```javascript
// Array destructuring
const [first, second, ...rest] = [1, 2, 3, 4, 5];
console.log(first);  // 1
console.log(rest);   // [3, 4, 5]

// Object destructuring
const { name, age, city = "Unknown" } = { name: "John", age: 30 };
console.log(name);  // "John"
console.log(city);  // "Unknown" (default value)
```

**Spread Operator**
* Expanding arrays and objects

```javascript
// Array spread
const arr1 = [1, 2, 3];
const arr2 = [...arr1, 4, 5];  // [1, 2, 3, 4, 5]

// Object spread
const obj1 = { x: 1, y: 2 };
const obj2 = { ...obj1, z: 3 };  // { x: 1, y: 2, z: 3 }
```

**Template Literals**
* String interpolation and multiline strings

```javascript
const name = "John";
const greeting = `Hello, ${name}! 
Welcome to our site.`;

// Tagged template literals
function highlight(strings, ...values) {
  return strings.reduce((result, str, i) => {
    const value = values[i] || '';
    return `${result}${str}<strong>${value}</strong>`;
  }, '');
}

const title = "JavaScript";
const html = highlight`Learn ${title} today!`;
```

**Optional Chaining**
* Safe property access

```javascript
const user = {
  profile: {
    // address might not exist
  }
};

// Without optional chaining
const city = user.profile && user.profile.address && user.profile.address.city;

// With optional chaining
const city2 = user.profile?.address?.city;
```

**Nullish Coalescing**
* Fallback for null/undefined only

```javascript
// Logical OR uses falsy values (0, "", false are considered "not set")
const count = data.count || 10;

// Nullish coalescing only considers null/undefined as "not set"
const count2 = data.count ?? 10;
```

## Modern APIs

**Fetch API**
* Modern replacement for XMLHttpRequest
* Promise-based

```javascript
fetch('https://api.example.com/data')
  .then(response => {
    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }
    return response.json();
  })
  .then(data => console.log('Data:', data))
  .catch(error => console.error('Error:', error));

// With async/await
async function fetchData() {
  try {
    const response = await fetch('https://api.example.com/data');
    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }
    const data = await response.json();
    console.log('Data:', data);
  } catch (error) {
    console.error('Error:', error);
  }
}
```

**Web Storage API**
* `localStorage` and `sessionStorage`

```javascript
// Store data
localStorage.setItem('user', JSON.stringify({name: 'John', id: 123}));

// Retrieve data
const user = JSON.parse(localStorage.getItem('user'));

// Clear specific item
localStorage.removeItem('user');

// Clear all storage
localStorage.clear();
```

**Intersection Observer**
* Detect element visibility

```javascript
const observer = new IntersectionObserver(entries => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      console.log('Element is visible!');
      entry.target.classList.add('visible');
    }
  });
}, {
  threshold: 0.5  // 50% visibility
});

// Start observing
const element = document.querySelector('.lazy-load');
observer.observe(element);
```

# Frameworks and Libraries

## State Management

**Component State**
* Local UI state
* Frameworks like React, Vue, Angular

```javascript
// React useState example
import React, { useState } from 'react';

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

**Global State**
* Libraries like Redux, MobX, Zustand

```javascript
// Redux simplified example
// Action creators
const increment = () => ({ type: 'INCREMENT' });
const decrement = () => ({ type: 'DECREMENT' });

// Reducer
function counterReducer(state = { count: 0 }, action) {
  switch (action.type) {
    case 'INCREMENT':
      return { count: state.count + 1 };
    case 'DECREMENT':
      return { count: state.count - 1 };
    default:
      return state;
  }
}

// Store usage (simplified)
const store = createStore(counterReducer);
store.subscribe(() => console.log(store.getState()));
store.dispatch(increment());
```

## Component Architecture

**Composition**
* Building complex UIs from simple components
* Prop drilling and component communication

```javascript
// React component composition
function App() {
  const [user, setUser] = useState(null);
  
  return (
    <div>
      <Header user={user} />
      <Main>
        <Sidebar />
        <Content user={user} />
      </Main>
      <Footer />
    </div>
  );
}
```

**Hooks Pattern**
* Reusing stateful logic
* Custom hooks

```javascript
// Custom React hook
function useLocalStorage(key, initialValue) {
  const [storedValue, setStoredValue] = useState(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.error(error);
      return initialValue;
    }
  });
  
  const setValue = value => {
    try {
      const valueToStore = value instanceof Function ? value(storedValue) : value;
      setStoredValue(valueToStore);
      window.localStorage.setItem(key, JSON.stringify(valueToStore));
    } catch (error) {
      console.error(error);
    }
  };
  
  return [storedValue, setValue];
}

// Usage
function App() {
  const [name, setName] = useLocalStorage('name', 'John');
  return (
    <input
      value={name}
      onChange={e => setName(e.target.value)}
    />
  );
}
```

# Performance Optimization

## Browser Rendering Optimization

**Critical Rendering Path**
* DOM, CSSOM, Render Tree, Layout, Paint, Composite

**Performance Techniques**
* Minimize reflows/repaints
* Use CSS transforms
* Debounce and throttle

```javascript
// Debounce function
function debounce(func, delay) {
  let timeout;
  
  return function executedFunction(...args) {
    const later = () => {
      clearTimeout(timeout);
      func(...args);
    };
    
    clearTimeout(timeout);
    timeout = setTimeout(later, delay);
  };
}

// Usage
const debouncedSearch = debounce(searchAPI, 300);
searchInput.addEventListener('input', debouncedSearch);
```

## Code Optimization

**Memoization**
* Cache expensive function results

```javascript
function memoize(fn) {
  const cache = new Map();
  
  return function(...args) {
    const key = JSON.stringify(args);
    if (cache.has(key)) {
      return cache.get(key);
    }
    
    const result = fn.apply(this, args);
    cache.set(key, result);
    return result;
  };
}

// Usage
const expensiveOperation = memoize(function(n) {
  console.log("Computing...");
  return n * n;
});

console.log(expensiveOperation(4));  // Computing... 16
console.log(expensiveOperation(4));  // 16 (from cache)
```

**Web Workers**
* Offload heavy computations to background threads

```javascript
// main.js
const worker = new Worker('worker.js');

worker.addEventListener('message', function(e) {
  console.log('Result from worker:', e.data);
});

worker.postMessage({numbers: [1, 2, 3, 4, 5]});

// worker.js
self.addEventListener('message', function(e) {
  const numbers = e.data.numbers;
  const result = numbers.map(x => x * x);
  
  self.postMessage(result);
});
```

# Security Best Practices

**Cross-Site Scripting (XSS) Prevention**
* Sanitize user input
* Use Content Security Policy

```javascript
// Unsafe
element.innerHTML = userInput;  // Vulnerable to XSS

// Safer
element.textContent = userInput;  // Escapes HTML

// For sanitizing HTML content
function sanitizeHTML(html) {
  const temp = document.createElement('div');
  temp.textContent = html;
  return temp.innerHTML;
}
```

**CSRF Protection**
* Use anti-CSRF tokens
* SameSite cookies

```javascript
// Adding CSRF token to requests
fetch('/api/data', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'CSRF-Token': document.querySelector('meta[name="csrf-token"]').content
  },
  body: JSON.stringify(data)
});
```

# Testing

**Unit Testing**
* Test individual functions/components
* Jest, Mocha, Jasmine

```javascript
// Jest example
function sum(a, b) {
  return a + b;
}

test('adds 1 + 2 to equal 3', () => {
  expect(sum(1, 2)).toBe(3);
});

// Testing async functions
test('fetchData returns data', async () => {
  const data = await fetchData();
  expect(data).toHaveProperty('id');
});
```

**Integration Testing**
* Test component interactions
* React Testing Library, Cypress

```javascript
// React Testing Library example
test('renders counter and increments', () => {
  const { getByText } = render(<Counter />);
  
  expect(getByText('Count: 0')).toBeInTheDocument();
  
  fireEvent.click(getByText('Increment'));
  
  expect(getByText('Count: 1')).toBeInTheDocument();
});
```

# ES Next and Future Features

**Top-level await**
* Use await outside of async functions in modules

```javascript
// ES modules only
const response = await fetch('/api/data');
const data = await response.json();
export { data };
```

**Private Class Fields**
* Better encapsulation

```javascript
class BankAccount {
  #balance = 0;
  
  deposit(amount) {
    this.#balance += amount;
  }
  
  get balance() {
    return this.#balance;
  }
}

const account = new BankAccount();
account.deposit(100);
console.log(account.balance);  // 100
console.log(account.#balance); // SyntaxError: Private field
```


## ES Next and Future Features 

**Record and Tuple** (continued)
```javascript
// Records (immutable objects)
const user = #{
  name: "John",
  age: 30
};

// Tuples (immutable arrays)
const point = #[10, 20];

// Equality based on content, not reference
const user2 = #{ name: "John", age: 30 };
console.log(user === user2); // true
```

**Pipeline Operator**
* Function chaining (proposal)

```javascript
// Without pipeline
const result = h(g(f(x)));

// With pipeline operator
const result = x |> f |> g |> h;

// With arguments
const result = x |> f |> (y => g(y, z)) |> h;
```

**Pattern Matching**
* Advanced destructuring (proposal)

```javascript
// Proposed syntax
const result = match (value) {
  when { type: "user", name } => `User: ${name}`,
  when { type: "product", id } => `Product #${id}`,
  when [...items] if items.length > 0 => `${items.length} items`,
  when _ => "Unknown"
};
```

# Functional Programming in JavaScript

## Pure Functions

**Characteristics**
* Same input always produces same output
* No side effects

```javascript
// Impure (depends on external state)
let total = 0;
function addToTotal(value) {
  total += value;
  return total;
}

// Pure function
function add(a, b) {
  return a + b;
}
```

## Immutability

**Working with Immutable Data**
* Create new objects/arrays instead of modifying

```javascript
// Mutable approach (avoid)
function addItem(array, item) {
  array.push(item);
  return array;
}

// Immutable approach (preferred)
function addItem(array, item) {
  return [...array, item];
}

// With objects
function updateUser(user, changes) {
  return { ...user, ...changes };
}
```

## Function Composition

**Building Complex Functions**
* Combine simple functions 

```javascript
// Compose utility
const compose = (...fns) => x => fns.reduceRight((acc, fn) => fn(acc), x);

// Point-free style functions
const double = x => x * 2;
const square = x => x * x;
const addOne = x => x + 1;

// Combined function
const compute = compose(addOne, square, double);
console.log(compute(3)); // ((3*2)^2)+1 = 37
```

## Currying and Partial Application

**Function Transformation**
* Convert multi-arg function to chain of single-arg functions

```javascript
// Currying
const curry = (fn) => {
  return function curried(...args) {
    if (args.length >= fn.length) {
      return fn(...args);
    }
    return function(...more) {
      return curried(...args, ...more);
    };
  };
};

// Usage
const add = (a, b, c) => a + b + c;
const curriedAdd = curry(add);

console.log(curriedAdd(1)(2)(3)); // 6
console.log(curriedAdd(1, 2)(3)); // 6
console.log(curriedAdd(1)(2, 3)); // 6
```

# TypeScript Fundamentals

## Basic Types

**Type Annotations**
* Explicit type declaration

```typescript
// Primitive types
let name: string = "John";
let age: number = 30;
let isActive: boolean = true;
let nothing: null = null;
let notDefined: undefined = undefined;

// Arrays
let numbers: number[] = [1, 2, 3];
let names: Array<string> = ["John", "Jane"];

// Tuples
let person: [string, number] = ["John", 30];

// Enums
enum Color {
  Red,
  Green,
  Blue
}
let favoriteColor: Color = Color.Blue;

// Any and unknown
let dynamicValue: any = 4;
dynamicValue = "string";

let safeValue: unknown = 4;
if (typeof safeValue === "number") {
  let num = safeValue + 10;
}
```

## Interfaces and Types

**Defining Object Shapes**
* Interfaces and type aliases

```typescript
// Interface
interface User {
  id: number;
  name: string;
  email?: string; // Optional property
  readonly createdAt: Date; // Read-only
}

const user: User = {
  id: 1,
  name: "John",
  createdAt: new Date()
};

// Type alias
type Point = {
  x: number;
  y: number;
};

// Union types
type ID = string | number;
let userId: ID = 123;
userId = "abc123";

// Intersection types
type Employee = User & {
  department: string;
  salary: number;
};
```

## Generics

**Type-Safe Reusable Code**
* Parameterized types

```typescript
// Generic function
function identity<T>(arg: T): T {
  return arg;
}

const num = identity<number>(42);
const str = identity("hello");

// Generic interface
interface Box<T> {
  value: T;
}

const numberBox: Box<number> = { value: 42 };
const stringBox: Box<string> = { value: "hello" };

// Generic constraints
interface Lengthwise {
  length: number;
}

function logLength<T extends Lengthwise>(arg: T): T {
  console.log(arg.length);
  return arg;
}

logLength("hello"); // 5
logLength([1, 2, 3]); // 3
// logLength(42); // Error: number doesn't have length
```

## Advanced Type Features

**Utility Types**
* Built-in type transformations

```typescript
// Partial - make all properties optional
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}

function updateTodo(todo: Todo, fieldsToUpdate: Partial<Todo>) {
  return { ...todo, ...fieldsToUpdate };
}

// Pick - select subset of properties
type TodoPreview = Pick<Todo, "title" | "completed">;

// Omit - remove properties
type TodoSummary = Omit<Todo, "description">;

// Record - key-value mapping
type CatInfo = { age: number; breed: string };
type CatName = "miffy" | "boris" | "mordred";

const cats: Record<CatName, CatInfo> = {
  miffy: { age: 10, breed: "Persian" },
  boris: { age: 5, breed: "Maine Coon" },
  mordred: { age: 16, breed: "British Shorthair" }
};
```

# Node.js and Server-Side JavaScript

## Core Modules

**File System Operations**
* Reading, writing, and manipulating files

```javascript
const fs = require('fs');

// Synchronous
try {
  const data = fs.readFileSync('file.txt', 'utf8');
  console.log(data);
} catch (err) {
  console.error(err);
}

// Asynchronous with callback
fs.readFile('file.txt', 'utf8', (err, data) => {
  if (err) {
    console.error(err);
    return;
  }
  console.log(data);
});

// With promises
const fsPromises = require('fs').promises;

async function readFile() {
  try {
    const data = await fsPromises.readFile('file.txt', 'utf8');
    console.log(data);
  } catch (err) {
    console.error(err);
  }
}
```

**HTTP Server**
* Creating web servers

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
  // Parse URL
  const url = new URL(req.url, `http://${req.headers.host}`);
  
  // Route handling
  if (url.pathname === '/') {
    res.writeHead(200, { 'Content-Type': 'text/html' });
    res.end('<h1>Home Page</h1>');
  } else if (url.pathname === '/api/users') {
    res.writeHead(200, { 'Content-Type': 'application/json' });
    res.end(JSON.stringify({ users: ['John', 'Jane'] }));
  } else {
    res.writeHead(404);
    res.end('Not Found');
  }
});

server.listen(3000, () => {
  console.log('Server running at http://localhost:3000/');
});
```

## Express.js Framework

**Basic Setup**
* Building APIs and web applications

```javascript
const express = require('express');
const app = express();

// Middleware
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// Static files
app.use(express.static('public'));

// Routes
app.get('/', (req, res) => {
  res.send('Hello World!');
});

app.get('/api/users', (req, res) => {
  res.json([
    { id: 1, name: 'John' },
    { id: 2, name: 'Jane' }
  ]);
});

app.post('/api/users', (req, res) => {
  const { name } = req.body;
  // Create user logic
  res.status(201).json({ id: 3, name });
});

// Error handling middleware
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).send('Something broke!');
});

// Start server
app.listen(3000, () => {
  console.log('Server running on port 3000');
});
```

# Web Performance and Optimization

## Performance Metrics

**Core Web Vitals**
* LCP (Largest Contentful Paint)
* FID (First Input Delay)
* CLS (Cumulative Layout Shift)

**Measurement APIs**
* `performance` API for timing

```javascript
// Mark and measure
performance.mark('start');

// Some operation
for (let i = 0; i < 1000; i++) {
  // Do something expensive
}

performance.mark('end');
performance.measure('operation', 'start', 'end');

const measurements = performance.getEntriesByType('measure');
console.log(measurements);
```

## Modern Build Tools

**Bundlers**
* Webpack, Rollup, esbuild, Vite

```javascript
// webpack.config.js example
module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env']
          }
        }
      }
    ]
  }
};
```

**Code Splitting**
* Loading code on demand

```javascript
// Dynamic import
button.addEventListener('click', async () => {
  const module = await import('./feature.js');
  module.doSomething();
});
```

This covers the primary JavaScript topics from basic to advanced. Each section provides concise explanations with practical code examples to demonstrate the concepts. Let me know if you'd like me to expand on any specific area!