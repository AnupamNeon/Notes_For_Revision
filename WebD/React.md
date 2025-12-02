# Complete React Notes: Basic to Advanced

## Table of Contents

1. [Introduction & Setup](#1-introduction--setup)
2. [JSX Fundamentals](#2-jsx-fundamentals)
3. [Components](#3-components)
4. [Props](#4-props)
5. [State](#5-state)
6. [Event Handling](#6-event-handling)
7. [Conditional Rendering](#7-conditional-rendering)
8. [Lists & Keys](#8-lists--keys)
9. [Forms](#9-forms)
10. [React Hooks](#10-react-hooks)
11. [Context API](#11-context-api)
12. [React Router](#12-react-router)
13. [Redux Toolkit](#13-redux-toolkit)
14. [Testing & Performance in React](#14-testing--performance-in-react)
15. [Advanced React Patterns](#15-advanced-react-patterns)

---

## 1. Introduction & Setup

### What is React?

**Theory:**  
React is a declarative, component-based JavaScript library for building user interfaces. It was developed by Facebook (now Meta) in 2013 and has become one of the most popular front-end libraries.

```
React is a JavaScript library for building user interfaces
├── Developed by Facebook (2013)
├── Component-based architecture
├── Virtual DOM for performance
├── Declarative approach
└── Unidirectional data flow
```

### Key Concepts Explained

| Concept                      | Description                                                                                                                                                  |
|------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Virtual DOM**              | A lightweight copy of the real DOM. React compares changes in the Virtual DOM first, then updates only the changed parts in the real DOM (“reconciliation”). |
| **Declarative**              | You describe what the UI should look like, not how to change it. React handles DOM manipulation.                                                             |
| **Component-Based**          | UI is broken into reusable pieces. Each component manages its own logic and rendering.                                                                       |
| **Unidirectional Data Flow** | Data flows from parent to child via props, making debugging easier.                                                                                           |

### Why React?

| Feature         | Benefit                            |
|-----------------|------------------------------------|
| Virtual DOM     | Faster updates, better performance |
| Component-Based | Reusability & maintainability      |
| JSX             | Readable, intuitive syntax         |
| Large Ecosystem | Many tools and libraries           |
| React Native    | Mobile app development             |

---

### Setup Methods

#### Method 1: Create React App (CRA)

```bash
npx create-react-app my-app
cd my-app
npm start
```

**Project Structure**
```
my-app/
├── node_modules/       # Dependencies
├── public/
│   ├── index.html      # Main HTML file (entry point)
│   └── favicon.ico
├── src/
│   ├── App.js          # Root component
│   ├── App.css         # Styles for App
│   ├── index.js        # JavaScript entry point
│   └── index.css       # Global styles
├── package.json        # Project config & dependencies
└── README.md
```

#### Method 2: Vite (Recommended – Faster)

```bash
npm create vite@latest my-app -- --template react
cd my-app
npm install
npm run dev
```

**Why Vite?**  
Vite uses ES modules and provides instant HMR (Hot Module Replacement) → much faster dev experience.

#### Method 3: Next.js (Full-Stack)

```bash
npx create-next-app@latest my-app
cd my-app
npm run dev
```

**When to use Next.js?**  
When you need SSR, SSG, API routes, or better SEO.

---

### Basic React App Structure

#### `index.js` – Entry Point

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root'));

root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

#### `App.js`

```jsx
function App() {
  return (
    <div className="App">
      <h1>Hello, React!</h1>
    </div>
  );
}

export default App;
```

```
React.StrictMode Behavior:
├── Development Only (no effect in production)
├── Helps find bugs by:
│   ├── Double-invoking components (render twice)
│   ├── Double-invoking effects (mount → unmount → mount)
│   ├── Detecting legacy API usage
│   └── Warning about deprecated patterns
│
├── Why double rendering?
│   ├── Exposes side effects in render
│   ├── Ensures effects have proper cleanup
│   └── Catches impure components
│
└── Common confusion:
    ├── "Why is my useEffect running twice?"
    ├── "Why is my console.log appearing twice?"
    └── Answer: StrictMode is doing its job!
```
    
```jsx
// Example: This will log TWICE in development with StrictMode
function Example() {
  console.log('Rendering'); // Logs twice in dev

  useEffect(() => {
    console.log('Effect runs'); // Runs twice in dev
    
    return () => {
      console.log('Cleanup'); // Also runs on the "fake" unmount
    };
  }, []);

  return <div>Hello</div>;
}

// This is EXPECTED behavior, not a bug!
// It helps you catch missing cleanup functions
```
---

## 2. JSX Fundamentals

### What is JSX?

**Theory:**  
JSX (JavaScript XML) is a syntax extension that lets you write HTML-like code in JavaScript. Babel compiles JSX → `React.createElement()`.

```
JSX = JavaScript XML
├── Syntax extension for JavaScript
├── Looks like HTML but it's JavaScript
├── Compiles to React.createElement()
└── Makes React code readable
```

### JSX Rules

```jsx
// Rule 1: Must return single parent
function Good() {
  return (
    <div>
      <h1>Title</h1>
      <p>Paragraph</p>
    </div>
  );
}

// Using Fragment
function BetterWithFragment() {
  return (
    <>
      <h1>Title</h1>
      <p>Paragraph</p>
    </>
  );
}
```

```jsx
// Rule 2: Use className instead of class
function Styling() {
  return <div className="container">Content</div>;
}
```

```jsx
// Rule 3: Close all tags
function ClosedTags() {
  return (
    <div>
      <img src="image.jpg" alt="description" />
      <input type="text" />
      <br />
    </div>
  );
}
```

```jsx
// Rule 4: camelCase for attributes
function CamelCase() {
  return (
    <div 
      onClick={() => {}}
      tabIndex={0}
      htmlFor="input"
      style={{ backgroundColor: "red" }} 
    >
      Content
    </div>
  );
}
```

### JavaScript Expressions in JSX

```jsx
function Expressions() {
  const name = "John";
  const age = 25;
  const isLoggedIn = true;
  const items = ["Apple", "Banana", "Orange"];

  return (
    <div>
      <h1>Hello, {name}!</h1>
      <p>Age: {age * 2}</p>
      <p>{isLoggedIn ? "Welcome!" : "Please login"}</p>
      <p>Uppercase: {name.toUpperCase()}</p>

      <ul>
        {items.map((item, index) => (
          <li key={index}>{item}</li>
        ))}
      </ul>

      <p>{`User ${name} is ${age} years old`}</p>
    </div>
  );
}
```

### JSX Behind the Scenes

```jsx
const element = <h1 className="greeting">Hello, World!</h1>;
```

Compiles to:

```js
const element = React.createElement(
  'h1',
  { className: 'greeting' },
  'Hello, World!'
);
```

Virtual DOM object:

```js
{
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, World!'
  }
}
```

---

## 3. Components

### Understanding Components

```
Components
├── Functional Components (Recommended)
│   ├── Simple JS functions
│   ├── Use Hooks
│   └── Easier to test
└── Class Components (Legacy)
    ├── ES6 classes
    ├── Lifecycle methods
    └── More verbose
```

### Functional Components

```jsx
function Welcome() {
  return <h1>Hello, World!</h1>;
}

const Welcome = () => <h1>Hello, World!</h1>;
```

With props:

```jsx
function Greeting({ name, age }) {
  return (
    <div>
      <h1>Hello, {name}!</h1>
      <p>You are {age} years old</p>
    </div>
  );
}
```

### Class Components (Legacy)

```jsx
import React, { Component } from 'react';

class Welcome extends Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  }

  componentDidMount() {
    console.log('Mounted!');
  }

  handleClick = () => {
    this.setState({ count: this.state.count + 1 });
  }

  render() {
    return (
      <div>
        <h1>Hello, {this.props.name}!</h1>
        <p>Count: {this.state.count}</p>
        <button onClick={this.handleClick}>Increment</button>
      </div>
    );
  }
}
```

### Real-Life Example: Product Card

```jsx
function ProductCard({ product }) {
  const { name, price, image, rating, inStock } = product;

  return (
    <div className="product-card">
      <img src={image} alt={name} />

      <div className="product-info">
        <h3>{name}</h3>

        <div className="product-rating">
          {[...Array(5)].map((_, i) => (
            <span key={i} className={i < rating ? 'star filled' : 'star'}>
              Star
            </span>
          ))}
        </div>

        <p>${price.toFixed(2)}</p>

        <button 
          className={`add-to-cart ${!inStock ? 'disabled' : ''}`}
          disabled={!inStock}
        >
          {inStock ? 'Add to Cart' : 'Out of Stock'}
        </button>
      </div>
    </div>
  );
}
```

---

## 4. Props

### Understanding Props

```
Props (Properties)
├── Read-only
├── Passed parent to child
├── Any JS type
└── Enables reusability
```

### Passing & Receiving Props

**Parent:**

```jsx
function App() {
  const userData = { name: 'John', role: 'Developer' };

  return (
    <>
      <UserCard name="John" role="Developer" />
      <Counter initialCount={10} />
      <Modal isOpen />
      <Profile user={{ name: 'John', age: 25 }} />
      <TodoList items={['Task 1', 'Task 2']} />
      <Button onClick={() => alert('Clicked!')} />
      <UserCard {...userData} />
    </>
  );
}
```

**Child:**

```jsx
function UserCard({ name, role }) {
  return (
    <div>
      <h1>{name}</h1>
      <p>{role}</p>
    </div>
  );
}
```

### Special Props

#### `children`

```jsx
function Card({ children, title }) {
  return (
    <div className="card">
      <h2>{title}</h2>
      <div>{children}</div>
    </div>
  );
}
```

#### `key`

```jsx
function TodoList({ items }) {
  return items.map(item => <li key={item.id}>{item.text}</li>);
}
```

#### `ref`

```jsx
function TextInput() {
  const inputRef = useRef(null);
  return <input ref={inputRef} type="text" />;
}
```

### PropTypes

```jsx
import PropTypes from 'prop-types';

UserProfile.propTypes = {
  name: PropTypes.string.isRequired,
  age: PropTypes.number,
  isAdmin: PropTypes.bool,
  hobbies: PropTypes.arrayOf(PropTypes.string),
  address: PropTypes.shape({
    city: PropTypes.string,
    country: PropTypes.string
  })
};
```

### Reusable Button Component

```jsx
function Button({
  children,
  variant = 'primary',
  size = 'medium',
  disabled = false,
  loading = false,
  icon = null,
  onClick,
  type = 'button',
  fullWidth = false,
  ...rest
}) {
  const className = `
    btn
    btn-${variant}
    btn-${size}
    ${fullWidth ? 'btn-full' : ''}
    ${loading ? 'btn-loading' : ''}
  `.trim();

  return (
    <button
      type={type}
      className={className}
      disabled={disabled || loading}
      onClick={onClick}
      {...rest}
    >
      {loading && <span className="spinner" />}
      {icon && !loading && <span className="icon">{icon}</span>}
      {children}
    </button>
  );
}
```

---

## 5. State

### Understanding State

```
State vs Props
├── State
│   ├── Internal
│   ├── Mutable
│   ├── Triggers re-render
│   └── Private
└── Props
    ├── External
    ├── Immutable
    ├── Passed parent to child
```

### useState Hook

```jsx
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>+</button>
      <button onClick={() => setCount(count - 1)}>-</button>
      <button onClick={() => setCount(0)}>Reset</button>
    </>
  );
}
```

### State with Different Types

```jsx
const [name, setName] = useState('');
const [items, setItems] = useState([]);
const [user, setUser] = useState({ name: '', email: '' });

const [data, setData] = useState(() => {
  const saved = localStorage.getItem('data');
  return saved ? JSON.parse(saved) : [];
});
```

### Updating State Correctly

**Never mutate directly:**

```js
user.name = 'Jane'; // Not allowed
items.push('a');    // Not allowed
```

**Correct way:**

```jsx
setUser(prev => ({ ...prev, name: 'Jane' }));
setItems(prev => [...prev, 'a']);
```

### Real-Life Example: Shopping Cart

```jsx
function ShoppingCart() {
  const [cart, setCart] = useState([]);

  const addToCart = (product) => {
    setCart(prev => {
      const existing = prev.find(item => item.id === product.id);
      if (existing) {
        return prev.map(item =>
          item.id === product.id
            ? { ...item, quantity: item.quantity + 1 }
            : item
        );
      }
      return [...prev, { ...product, quantity: 1 }];
    });
  };

  const removeFromCart = (id) => {
    setCart(prev => prev.filter(item => item.id !== id));
  };
}
```

---

## 6. Event Handling

### Event Handling Basics

**Theory:**  
React events are named using **camelCase** (`onClick`, not `onclick`) and you pass a **function** as the event handler, not a string.  
React uses **SyntheticEvents**, which wrap native events to provide cross-browser compatibility.

### Example: Event Handling

```jsx
function EventHandling() {
  // Method 1: Named handler function
  const handleClick = () => {
    console.log('Button clicked!');
  };
  
  // Method 2: Handler with event object
  const handleClickWithEvent = (e) => {
    console.log('Event:', e);           // SyntheticEvent
    console.log('Target:', e.target);   // The element that triggered
    e.preventDefault();                 // Prevent default behavior
    e.stopPropagation();                // Stop event bubbling
  };
  
  // Method 3: Handler with custom arguments
  const handleItemClick = (id, e) => {
    console.log('Item ID:', id);
    console.log('Event:', e);
  };
  
  return (
    <div>
      {/* Reference the function - DON'T call it with () */}
      <button onClick={handleClick}>Click Me</button>
      
      {/* Inline arrow function */}
      <button onClick={() => console.log('Clicked!')}>
        Inline Handler
      </button>
      
      {/* Passing arguments - wrap in arrow function */}
      <button onClick={(e) => handleItemClick(123, e)}>
        With Arguments
      </button>
      
      {/* Form submission */}
      <form onSubmit={handleClickWithEvent}>
        <button type="submit">Submit</button>
      </form>
    </div>
  );
}
```

### Common Events

```jsx
function AllEvents() {
  return (
    <div>
      {/* Mouse Events */}
      <button onClick={() => {}}>Click</button>
      <button onDoubleClick={() => {}}>Double Click</button>
      <div onMouseEnter={() => {}}>Mouse Enter</div>
      <div onMouseLeave={() => {}}>Mouse Leave</div>
      <div onMouseMove={(e) => console.log(e.clientX, e.clientY)}>
        Mouse Move
      </div>
      
      {/* Keyboard Events */}
      <input onKeyDown={(e) => console.log('Key:', e.key)} />
      <input onKeyUp={(e) => console.log('Key:', e.key)} />
      
      {/* Form Events */}
      <input onChange={(e) => console.log(e.target.value)} />
      <input onFocus={() => console.log('Focused')} />
      <input onBlur={() => console.log('Blurred')} />
      <form onSubmit={(e) => e.preventDefault()}>
        <button type="submit">Submit</button>
      </form>
      
      {/* Clipboard Events */}
      <input 
        onCopy={() => console.log('Copied')}
        onPaste={() => console.log('Pasted')}
        onCut={() => console.log('Cut')}
      />
      
      {/* Drag Events */}
      <div
        draggable
        onDragStart={() => console.log('Started dragging')}
        onDragEnd={() => console.log('Stopped dragging')}
        onDragOver={(e) => e.preventDefault()}  // Required to allow drop
        onDrop={() => console.log('Dropped')}
      >
        Drag me
      </div>
      
      {/* Scroll Event */}
      <div 
        style={{ height: '100px', overflow: 'auto' }}
        onScroll={(e) => console.log(e.target.scrollTop)}
      >
        Scrollable content
      </div>
    </div>
  );
}
```

### Real-Life Use Case: Interactive Form with Validation

```jsx
import { useState } from 'react';

function RegistrationForm() {
  const [formData, setFormData] = useState({
    username: '',
    email: '',
    password: '',
    confirmPassword: ''
  });
  
  const [errors, setErrors] = useState({});
  const [touched, setTouched] = useState({});  // Track which fields were visited
  const [isSubmitting, setIsSubmitting] = useState(false);
  
  // Handle input change
  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData(prev => ({
      ...prev,
      [name]: value
    }));
    
    // Clear error when user starts typing
    if (errors[name]) {
      setErrors(prev => ({
        ...prev,
        [name]: ''
      }));
    }
  };
  
  // Handle blur (field touched)
  const handleBlur = (e) => {
    const { name } = e.target;
    setTouched(prev => ({
      ...prev,
      [name]: true
    }));
    validateField(name, formData[name]);
  };
  
  // Validate single field
  const validateField = (name, value) => {
    let error = '';
    
    switch (name) {
      case 'username':
        if (!value) error = 'Username is required';
        else if (value.length < 3) error = 'Username must be at least 3 characters';
        break;
      case 'email':
        if (!value) error = 'Email is required';
        else if (!/\S+@\S+\.\S+/.test(value)) error = 'Email is invalid';
        break;
      case 'password':
        if (!value) error = 'Password is required';
        else if (value.length < 8) error = 'Password must be at least 8 characters';
        else if (!/(?=.*[0-9])/.test(value)) error = 'Password must contain a number';
        break;
      case 'confirmPassword':
        if (!value) error = 'Please confirm your password';
        else if (value !== formData.password) error = 'Passwords do not match';
        break;
      default:
        break;
    }
    
    setErrors(prev => ({ ...prev, [name]: error }));
    return error;
  };
  
  // Handle form submission
  const handleSubmit = async (e) => {
    e.preventDefault();
    
    // Validate all fields
    const newErrors = {};
    Object.keys(formData).forEach(key => {
      const error = validateField(key, formData[key]);
      if (error) newErrors[key] = error;
    });
    
    // Mark all as touched
    setTouched({
      username: true,
      email: true,
      password: true,
      confirmPassword: true
    });
    
    // Stop if there are errors
    if (Object.keys(newErrors).some(key => newErrors[key])) {
      return;
    }
    
    setIsSubmitting(true);
    
    try {
      // Simulate API call
      await new Promise(resolve => setTimeout(resolve, 2000));
      console.log('Form submitted:', formData);
      alert('Registration successful!');
    } catch (error) {
      console.error('Submission error:', error);
    } finally {
      setIsSubmitting(false);
    }
  };
  
  return (
    <form onSubmit={handleSubmit} className="registration-form">
      <h2>Create Account</h2>
      
      {/* Username Field */}
      <div className="form-group">
        <label htmlFor="username">Username</label>
        <input
          type="text"
          id="username"
          name="username"
          value={formData.username}
          onChange={handleChange}
          onBlur={handleBlur}
          className={touched.username && errors.username ? 'error' : ''}
        />
        {touched.username && errors.username && (
          <span className="error-message">{errors.username}</span>
        )}
      </div>
      
      {/* Similar fields for email, password, confirmPassword */}
      
      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? 'Creating Account...' : 'Register'}
      </button>
    </form>
  );
}
```

---

## 7. Conditional Rendering

### Conditional Rendering Methods

**Theory:**  
React allows you to conditionally render components or elements based on conditions. There are several patterns to achieve this, each suited for different situations.

### Example: Conditional Rendering

```jsx
function ConditionalRendering({ isLoggedIn, user, items, status }) {
  
  // Method 1: Early return with if statement
  if (!isLoggedIn) {
    return <LoginPage />;
  }
  
  return (
    <div>
      {/* Method 2: Ternary Operator - best for if/else */}
      {isLoggedIn ? <Dashboard /> : <LoginPage />}
      
      {/* Method 3: Logical AND (&&) - best for show/hide */}
      {isLoggedIn && <WelcomeMessage />}
      
      {/* Warning: Be careful with falsy values */}
      {items.length && <ItemList items={items} />}
      
      {/* Good: Convert to boolean */}
      {items.length > 0 && <ItemList items={items} />}
      
      {/* Method 4: Logical OR (||) for fallbacks */}
      <p>{user.name || 'Anonymous User'}</p>
      
      {/* Method 5: Nullish Coalescing (??) */}
      <p>{user.name ?? 'Default Name'}</p>
      
      {/* Method 6: IIFE */}
      {(() => {
        if (status === 'loading') return <Spinner />;
        if (status === 'error') return <ErrorMessage />;
        if (status === 'success') return <SuccessMessage />;
        return null;
      })()}
      
      {/* Method 7: Object lookup */}
      {{
        'loading': <Spinner />,
        'error': <ErrorMessage />,
        'success': <SuccessMessage />
      }[status]}
      
      {/* Method 8: Conditional classes */}
      <div className={`card ${isLoggedIn ? 'logged-in' : 'logged-out'}`}>
        Content
      </div>
      
      {/* Method 9: Conditional inline styles */}
      <div style={{ display: isLoggedIn ? 'block' : 'none' }}>
        Hidden/Shown Content
      </div>
    </div>
  );
}
```

### Component-Based Conditional Rendering

```jsx
// Reusable conditional wrapper component
function ShowIf({ condition, children, fallback = null }) {
  return condition ? children : fallback;
}

// Usage
function App() {
  const { isLoggedIn } = useAuth();
  
  return (
    <ShowIf condition={isLoggedIn} fallback={<LoginPrompt />}>
      <Dashboard />
    </ShowIf>
  );
}
```

### Real-Life Use Case: Data Fetching States

```jsx
import { useState, useEffect } from 'react';

function UserList() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    fetchUsers();
  }, []);
  
  const fetchUsers = async () => {
    try {
      setLoading(true);
      setError(null);
      
      const response = await fetch('https://api.example.com/users');
      
      if (!response.ok) {
        throw new Error('Failed to fetch users');
      }
      
      const data = await response.json();
      setUsers(data);
    } catch (err) {
      setError(err.message);
    } finally {
      setLoading(false);
    }
  };
  
  // Loading State
  if (loading) {
    return (
      <div className="loading-container">
        <div className="spinner" />
        <p>Loading users...</p>
      </div>
    );
  }
  
  // Error State
  if (error) {
    return (
      <div className="error-container">
        <h2>Oops! Something went wrong</h2>
        <p>{error}</p>
        <button onClick={fetchUsers}>Try Again</button>
      </div>
    );
  }
  
  // Empty State
  if (users.length === 0) {
    return (
      <div className="empty-container">
        <img src="/empty-state.svg" alt="No users" />
        <h2>No users found</h2>
        <p>There are no users to display at the moment.</p>
      </div>
    );
  }
  
  // Success State
  return (
    <div className="user-list">
      <h2>Users ({users.length})</h2>
      <ul>
        {users.map(user => (
          <li key={user.id} className="user-card">
            <img src={user.avatar || '/default-avatar.png'} alt={user.name} />
            <div className="user-info">
              <h3>{user.name}</h3>
              <p>{user.email}</p>
              {user.isVerified && <span className="badge">Verified</span>}
            </div>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

---

## 8. Lists & Keys

### Understanding Lists and Keys

**Theory:**  
When rendering lists in React, each item needs a **unique key prop**.  
Keys help React identify which items have changed, been added, or removed, enabling efficient DOM updates.

### Rendering Lists

```jsx
function ListRendering() {
  const fruits = ['Apple', 'Banana', 'Orange', 'Mango'];
  const users = [
    { id: 1, name: 'John', email: 'john@example.com' },
    { id: 2, name: 'Jane', email: 'jane@example.com' },
    { id: 3, name: 'Bob', email: 'bob@example.com' }
  ];
  
  return (
    <div>
      {/* Simple array - use index as key (only for static lists) */}
      <ul>
        {fruits.map((fruit, index) => (
          <li key={index}>{fruit}</li>
        ))}
      </ul>
      
      {/* Array of objects - use unique ID */}
      <ul>
        {users.map(user => (
          <li key={user.id}>
            <strong>{user.name}</strong> - {user.email}
          </li>
        ))}
      </ul>
      
      {/* Extracting to separate component */}
      {users.map(user => (
        <UserCard key={user.id} user={user} />
      ))}
    </div>
  );
}
```

### Keys Best Practices

```jsx
// Good - Use unique, stable IDs from your data
{users.map(user => (
  <UserCard key={user.id} user={user} />
))}

// Good - Use unique string identifiers
{posts.map(post => (
  <PostCard key={post.slug} post={post} />
))}

// Warning: OKAY - Index as last resort (ONLY for static lists that never reorder)
{staticItems.map((item, index) => (
  <li key={index}>{item}</li>
))}

// Bad - Random keys cause re-renders every time!
{users.map(user => (
  <UserCard key={Math.random()} user={user} />
))}

// Bad - Index for dynamic/sortable/editable lists
{dynamicItems.map((item, index) => (
  <EditableItem key={index} item={item} />
))}
```

**Why Keys Matter**
- Without proper keys, React may reuse DOM elements incorrectly
- Child component state can get mixed up
- Animations may break
- Performance suffers

### Real-Life Use Case: Todo List with CRUD

```jsx
import { useState } from 'react';

function TodoApp() {
  const [todos, setTodos] = useState([
    { id: 1, text: 'Learn React', completed: false, priority: 'high' },
    { id: 2, text: 'Build a project', completed: false, priority: 'medium' },
  ]);
  const [newTodo, setNewTodo] = useState('');
  const [filter, setFilter] = useState('all');  // 'all' | 'active' | 'completed'
  const [editingId, setEditingId] = useState(null);
  const [editText, setEditText] = useState('');
  
  // Generate unique ID
  const generateId = () => Date.now();
  
  // CREATE - Add new todo
  const addTodo = (e) => {
    e.preventDefault();
    if (!newTodo.trim()) return;
    
    setTodos(prev => [
      ...prev,
      {
        id: generateId(),
        text: newTodo.trim(),
        completed: false,
        priority: 'medium',
        createdAt: new Date()
      }
    ]);
    setNewTodo('');
  };
  
  // UPDATE - Toggle completion
  const toggleTodo = (id) => {
    setTodos(prev =>
      prev.map(todo =>
        todo.id === id ? { ...todo, completed: !todo.completed } : todo
      )
    );
  };
  
  // DELETE - Remove todo
  const deleteTodo = (id) => {
    setTodos(prev => prev.filter(todo => todo.id !== id));
  };
  
  // UPDATE - Save edit
  const saveEdit = (id) => {
    setTodos(prev =>
      prev.map(todo =>
        todo.id === id ? { ...todo, text: editText } : todo
      )
    );
    setEditingId(null);
    setEditText('');
  };
  
  // Filter todos based on current filter
  const filteredTodos = todos.filter(todo => {
    if (filter === 'active') return !todo.completed;
    if (filter === 'completed') return todo.completed;
    return true;
  });
  
  return (
    <div className="todo-app">
      <h1>Todo List</h1>
      
      {/* Add Todo Form */}
      <form onSubmit={addTodo}>
        <input
          type="text"
          value={newTodo}
          onChange={(e) => setNewTodo(e.target.value)}
          placeholder="What needs to be done?"
        />
        <button type="submit">Add</button>
      </form>
      
      {/* Filter Buttons */}
      <div className="filters">
        {['all', 'active', 'completed'].map(f => (
          <button
            key={f}
            className={filter === f ? 'active' : ''}
            onClick={() => setFilter(f)}
          >
            {f.charAt(0).toUpperCase() + f.slice(1)}
          </button>
        ))}
      </div>
      
      {/* Todo List */}
      <ul className="todo-list">
        {filteredTodos.map(todo => (
          <li key={todo.id} className={todo.completed ? 'completed' : ''}>
            <input
              type="checkbox"
              checked={todo.completed}
              onChange={() => toggleTodo(todo.id)}
            />
            
            {editingId === todo.id ? (
              <input
                type="text"
                value={editText}
                onChange={(e) => setEditText(e.target.value)}
                onKeyDown={(e) => {
                  if (e.key === 'Enter') saveEdit(todo.id);
                  if (e.key === 'Escape') setEditingId(null);
                }}
                autoFocus
              />
            ) : (
              <span
                onDoubleClick={() => {
                  setEditingId(todo.id);
                  setEditText(todo.text);
                }}
              >
                {todo.text}
              </span>
            )}
            
            <button onClick={() => deleteTodo(todo.id)}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

---

## 9. Forms

### Controlled vs Uncontrolled Components

**Theory:**

- **Controlled Components**: React state is the single source of truth.
- **Uncontrolled Components**: DOM manages the value; use refs to read it.

### Controlled Component

```jsx
import { useState } from 'react';

// CONTROLLED - React controls the value
function ControlledForm() {
  const [value, setValue] = useState('');
  
  return (
    <input
      type="text"
      value={value}                            // Value from state
      onChange={(e) => setValue(e.target.value)}  // State updates on change
    />
  );
}
```

### Uncontrolled Component

```jsx
import { useRef } from 'react';

function UncontrolledForm() {
  const inputRef = useRef(null);
  
  const handleSubmit = (e) => {
    e.preventDefault();
    console.log('Value:', inputRef.current.value);  // Access DOM directly
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input type="text" ref={inputRef} defaultValue="Initial" />
      <button type="submit">Submit</button>
    </form>
  );
}
```

### When to use which

| Type         | Use Case                            |
|--------------|-------------------------------------|
| **Controlled** | Validation, conditional rendering, instant UI updates, formatted inputs |
| **Uncontrolled** | Simple forms, file inputs, integrating with non-React code |

### All Form Elements

```jsx
function FormElements() {
  const [form, setForm] = useState({
    text: '',
    email: '',
    password: '',
    textarea: '',
    select: '',
    multiSelect: [],
    checkbox: false,
    checkboxGroup: [],
    radio: '',
    range: 50,
    date: '',
    file: null
  });
  
  // Generic handler for most inputs
  const handleChange = (e) => {
    const { name, value, type, checked, files } = e.target;
    
    setForm(prev => ({
      ...prev,
      [name]: type === 'checkbox' ? checked :
              type === 'file' ? files[0] :
              value
    }));
  };
  
  // Special handler for multi-select
  const handleMultiSelect = (e) => {
    const options = Array.from(e.target.selectedOptions);
    setForm(prev => ({
      ...prev,
      multiSelect: options.map(opt => opt.value)
    }));
  };
  
  // Special handler for checkbox group
  const handleCheckboxGroup = (e) => {
    const { value, checked } = e.target;
    setForm(prev => ({
      ...prev,
      checkboxGroup: checked
        ? [...prev.checkboxGroup, value]             // Add if checked
        : prev.checkboxGroup.filter(v => v !== value) // Remove if unchecked
    }));
  };
  
  return (
    <form>
      {/* Text Input */}
      <input
        type="text"
        name="text"
        value={form.text}
        onChange={handleChange}
        placeholder="Enter text"
      />
      
      {/* Textarea */}
      <textarea
        name="textarea"
        value={form.textarea}
        onChange={handleChange}
        rows={4}
      />
      
      {/* Select Dropdown */}
      <select name="select" value={form.select} onChange={handleChange}>
        <option value="">Select an option</option>
        <option value="option1">Option 1</option>
        <option value="option2">Option 2</option>
      </select>
      
      {/* Multi-Select */}
      <select
        name="multiSelect"
        multiple
        value={form.multiSelect}
        onChange={handleMultiSelect}
      >
        <option value="a">A</option>
        <option value="b">B</option>
        <option value="c">C</option>
      </select>
      
      {/* Single Checkbox */}
      <label>
        <input
          type="checkbox"
          name="checkbox"
          checked={form.checkbox}
          onChange={handleChange}
        />
        Accept terms
      </label>
      
      {/* Checkbox Group */}
      <div>
        {['React', 'Vue', 'Angular'].map(tech => (
          <label key={tech}>
            <input
              type="checkbox"
              value={tech}
              checked={form.checkboxGroup.includes(tech)}
              onChange={handleCheckboxGroup}
            />
            {tech}
          </label>
        ))}
      </div>
      
      {/* Radio Group */}
      <div>
        {['small', 'medium', 'large'].map(size => (
          <label key={size}>
            <input
              type="radio"
              name="radio"
              value={size}
              checked={form.radio === size}
              onChange={handleChange}
            />
            {size}
          </label>
        ))}
      </div>
      
      {/* Range Slider */}
      <input
        type="range"
        name="range"
        min="0"
        max="100"
        value={form.range}
        onChange={handleChange}
      />
      <span>{form.range}</span>
      
      {/* File Input */}
      <input
        type="file"
        name="file"
        onChange={handleChange}
        accept="image/*"
      />
    </form>
  );
}
```

## 10. React Hooks

### Hooks Overview

**Theory:**  
Hooks are functions that let you "hook into" React state and lifecycle features from functional components. Introduced in React 16.8, they allow you to use state and other React features without writing class components.

```
React Hooks
├── Basic Hooks
│   ├── useState     → State management
│   ├── useEffect    → Side effects (data fetching, subscriptions, DOM)
│   └── useContext   → Consume context values
│
├── Additional Hooks
│   ├── useReducer   → Complex state logic (Redux-like)
│   ├── useCallback  → Memoize callback functions
│   ├── useMemo      → Memoize expensive computations
│   ├── useRef       → DOM access & mutable values
│   └── useLayoutEffect → Runs synchronously after DOM mutations
│
├── React (NEW) Hooks
│   ├── useId            → Generate unique IDs for accessibility
│   ├── useTransition    → Mark updates as non-urgent
│   ├── useDeferredValue → Defer re-rendering of non-urgent parts
│   ├── useSyncExternalStore → Subscribe to external stores
│   └── useInsertionEffect   → For CSS-in-JS libraries
│
└── Rules of Hooks
    ├── Only call hooks at the top level
    └── Only call hooks inside React functions (components or custom hooks)
```

---

### useEffect

**Theory:**  
`useEffect` lets you perform side effects in function components. It serves the same purpose as `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount` in class components.

```jsx
import { useState, useEffect } from 'react';

function UseEffectExamples() {
  const [count, setCount] = useState(0);
  const [data, setData] = useState(null);

  // 1. Runs on EVERY render (mount + every update)
  useEffect(() => {
    console.log('Component rendered');
  });

  // 2. Runs ONLY on mount (empty dependency array)
  useEffect(() => {
    console.log('Component mounted');

    // Cleanup runs on unmount
    return () => {
      console.log('Component will unmount');
    };
  }, []);

  // 3. Runs on mount AND whenever count changes
  useEffect(() => {
    console.log('Count changed:', count);
  }, [count]);

  // 4. Data fetching with cleanup (AbortController)
  useEffect(() => {
    const abortController = new AbortController();
    
    const fetchData = async () => {
      try {
        const response = await fetch('https://api.example.com/data', {
          signal: abortController.signal
        });
        const result = await response.json();
        setData(result);
      } catch (error) {
        if (error.name !== 'AbortError') {
          console.error('Error:', error);
        }
      }
    };
    fetchData();

    return () => abortController.abort();
  }, []);

  // 5. Event listener + cleanup
  useEffect(() => {
    const handleResize = () => console.log('Window resized');
    window.addEventListener('resize', handleResize);

    return () => window.removeEventListener('resize', handleResize);
  }, []);

  // 6. Timer + cleanup
  useEffect(() => {
    const timer = setInterval(() => console.log('Tick'), 1000);
    return () => clearInterval(timer);
  }, []);

  // 7. Update document title
  useEffect(() => {
    document.title = `Count: ${count}`;
  }, [count]);

  return <div>Check console for logs</div>;
}
```

---

### useRef

**Theory:**  
`useRef` returns a mutable ref object whose `.current` property persists for the lifetime of the component. Used for DOM access and storing mutable values without causing re-renders.

```jsx
import { useRef, useEffect, useState } from 'react';

function UseRefExamples() {
  const inputRef = useRef(null);
  const videoRef = useRef(null);
  const renderCount = useRef(0);
  const previousValue = useRef(null);
  const timerRef = useRef(null);

  const [value, setValue] = useState('');

  useEffect(() => {
    inputRef.current.focus();
  }, []);

  useEffect(() => {
    renderCount.current += 1;
    console.log('Render count:', renderCount.current);
  });

  useEffect(() => {
    previousValue.current = value;
  }, [value]);

  const startTimer = () => {
    timerRef.current = setInterval(() => console.log('Timer running'), 1000);
  };

  const stopTimer = () => clearInterval(timerRef.current);

  return (
    <div>
      <input ref={inputRef} type="text" placeholder="Auto-focused" />
      
      <input value={value} onChange={(e) => setValue(e.target.value)} />
      <p>Current: {value}</p>
      <p>Previous: {previousValue.current || 'None'}</p>

      <video ref={videoRef} src="video.mp4" />
      <button onClick={() => videoRef.current.play()}>Play</button>
      <button onClick={() => videoRef.current.pause()}>Pause</button>

      <button onClick={startTimer}>Start Timer</button>
      <button onClick={stopTimer}>Stop Timer</button>
    </div>
  );
}
```

---

### useReducer

**Theory:**  
Alternative to `useState` when you have complex state logic or the next state depends on the previous one. Works like Redux.

```jsx
import { useReducer } from 'react';

const initialState = { count: 0, loading: false, error: null };

function reducer(state, action) {
  switch (action.type) {
    case 'INCREMENT':
      return { ...state, count: state.count + 1 };
    case 'DECREMENT':
      return { ...state, count: state.count - 1 };
    case 'SET_COUNT':
      return { ...state, count: action.payload };
    case 'RESET':
      return initialState;
    default:
      throw new Error(`Unknown action: ${action.type}`);
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'INCREMENT' })}>Plus 1</button>
      <button onClick={() => dispatch({ type: 'DECREMENT' })}>Minus 1</button>
      <button onClick={() => dispatch({ type: 'SET_COUNT', payload: 100 })}>
        Set 100
      </button>
      <button onClick={() => dispatch({ type: 'RESET' })}>Reset</button>
    </div>
  );
}
```

**When to use useReducer instead of useState:**
- Complex state logic with multiple sub-values
- Next state depends on previous state
- Easier to test and debug state transitions
- Large apps with shared state patterns

---

### useMemo & useCallback

**Theory:**  
Performance optimization hooks using memoization.

- `useMemo` → memoizes a **value**
- `useCallback` → memoizes a **function**

```jsx
import { useState, useMemo, useCallback } from 'react';

function ExpensiveComponent({ items, onItemClick }) {
  const [filter, setFilter] = useState('');
  const [sortOrder, setSortOrder] = useState('asc');

  const filteredAndSortedItems = useMemo(() => {
    console.log('Running expensive filtering + sorting...');
    return items
      .filter(item => item.name.toLowerCase().includes(filter.toLowerCase()))
      .sort((a, b) =>
        sortOrder === 'asc'
          ? a.name.localeCompare(b.name)
          : b.name.localeCompare(a.name)
      );
  }, [items, filter, sortOrder]);

  const statistics = useMemo(() => ({
    total: items.length,
    filtered: filteredAndSortedItems.length,
    avgPrice: items.reduce((sum, i) => sum + i.price, 0) / items.length || 0
  }), [items, filteredAndSortedItems]);

  const handleItemClick = useCallback((id) => {
    console.log('Clicked item:', id);
    onItemClick(id);
  }, [onItemClick]);

  const handleFilter = useCallback((e) => {
    setFilter(e.target.value);
  }, []);

  return (
    <div>
      <input value={filter} onChange={handleFilter} placeholder="Filter..." />
      <p>Showing {statistics.filtered} of {statistics.total} items</p>
      <ul>
        {filteredAndSortedItems.map(item => (
          <li key={item.id} onClick={() => handleItemClick(item.id)}>
            {item.name} - ${item.price}
          </li>
        ))}
      </ul>
    </div>
  );
}
```

**Use only when you have actual performance issues!**

---

## 11. Context API

### Understanding Context

**Theory:**  
Context provides a way to pass data through the component tree without prop drilling. Perfect for global data like theme, authentication, language, etc.

```
Context API
├── Solves "prop drilling"
├── Provides global state to subtree
├── Three main parts:
│   ├── createContext()
│   ├── <Provider>
│   └── useContext()
└── Ideal for: Theme, Auth, User, i18n
```

### Basic Theme Context Example

```jsx
import { createContext, useContext, useState } from 'react';

// 1. Create Context
const ThemeContext = createContext(null);

// 2. Provider Component
function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');

  const toggleTheme = () => {
    setTheme(prev => (prev === 'light' ? 'dark' : 'light'));
  };

  const value = { theme, toggleTheme, isDark: theme === 'dark' };

  return (
    <ThemeContext.Provider value={value}>
      {children}
    </ThemeContext.Provider>
  );
}

// 3. Custom Hook (Best Practice)
function useTheme() {
  const context = useContext(ThemeContext);
  if (!context) throw new Error('useTheme must be used within ThemeProvider');
  return context;
}

// 4. Consume in Components
function ThemeButton() {
  const { theme, toggleTheme } = useTheme();
  return (
    <button onClick={toggleTheme}>
      Switch to {theme === 'light' ? 'dark' : 'light'} mode
    </button>
  );
}

function App() {
  return (
    <ThemeProvider>
      <Header />
      <ThemeButton />
      <Main />
    </ThemeProvider>
  );
}
```

### Real-Life: Authentication Context

```jsx
import { createContext, useContext, useState, useEffect } from 'react';

const AuthContext = createContext(null);

export function AuthProvider({ children }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    // Check token on mount
    const token = localStorage.getItem('token');
    if (token) {
      // Validate token with API...
      // setUser(...)
    }
    setLoading(false);
  }, []);

  const login = async (userData, token) => {
    localStorage.setItem('token', token);
    setUser(userData);
  };

  const logout = () => {
    localStorage.removeItem('token');
    setUser(null);
  };

  const value = {
    user,
    login,
    logout,
    isAuthenticated: !!user,
    loading
  };

  return <AuthContext.Provider value={value}>{children}</AuthContext.Provider>;
}

export function useAuth() {
  const context = useContext(AuthContext);
  if (!context) throw new Error('useAuth must be used within AuthProvider');
  return context;
}
```

---

## 12. React Router

### Installation

```bash
npm install react-router-dom
```

### Basic Setup

```jsx
import {
  BrowserRouter,
  Routes,
  Route,
  Link,
  NavLink,
  Navigate,
  Outlet
} from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
        <NavLink
          to="/dashboard"
          className={({ isActive }) => (isActive ? 'active' : '')}
        >
          Dashboard
        </NavLink>
      </nav>

      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/products" element={<Products />} />
        <Route path="/products/:id" element={<ProductDetail />} />
        <Route path="*" element={<NotFound />} />
      </Routes>
    </BrowserRouter>
  );
}
```

### Dynamic Routes & Hooks

```jsx
// /products/123
function ProductDetail() {
  const { id } = useParams();
  return <h1>Product ID: {id}</h1>;
}

// Query params: ?q=react&sort=asc
function Search() {
  const [searchParams, setSearchParams] = useSearchParams();
  const query = searchParams.get('q');
  return <div>Searching: {query}</div>;
}

// Programmatic navigation
function Login() {
  const navigate = useNavigate();
  const handleLogin = () => {
    navigate('/dashboard', { replace: true });
  };
}
```

### Nested Routes & Layouts

```jsx
function App() {
  return (
    <Routes>
      <Route element={<Layout />}>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/dashboard" element={<DashboardLayout />}>
          <Route index element={<Overview />} />
          <Route path="profile" element={<Profile />} />
          <Route path="settings" element={<Settings />} />
        </Route>
      </Route>
    </Routes>
  );
}

function Layout() {
  return (
    <>
      <Header />
      <main><Outlet /></main>
      <Footer />
    </>
  );
}
```

### Protected Routes

```jsx
function ProtectedRoute({ children }) {
  const { isAuthenticated, loading } = useAuth();
  const location = useLocation();

  if (loading) return <Loading />;
  if (!isAuthenticated) return <Navigate to="/login" state={{ from: location }} replace />;

  return children;
}

// Usage
<Route
  path="/admin"
  element={
    <ProtectedRoute>
      <AdminPanel />
    </ProtectedRoute>
  }
/>
```

---

### Quick Reference Cheat Sheet

| Hook             | Purpose                                | Example |
|------------------|----------------------------------------|---------|
| `useState`       | Local state                            | `const [x, setX] = useState()` |
| `useEffect`      | Side effects                          | `useEffect(() => {}, [deps])` |
| `useContext`     | Consume context                        | `useContext(MyContext)` |
| `useRef`         | DOM refs / mutable values              | `useRef(null)` |
| `useReducer`     | Complex state logic                    | `useReducer(reducer, init)` |
| `useMemo`        | Memoize values                         | `useMemo(() => compute(), [deps])` |
| `useCallback`    | Memoize functions                     | `useCallback(() => {}, [deps])` |

**Common Patterns**

```jsx
{isLoading && <Spinner />}
{user ? <Dashboard /> : <Login />}

{items.map(item => <Item key={item.id} item={item} />)}

<button onClick={(e) => handleClick(id, e)}>Click</button>

<input value={value} onChange={e => setValue(e.target.value)} />

<Component {...props} />
```

## 13. Redux Toolkit

---

### 🛠 Installation

```bash
npm install @reduxjs/toolkit react-redux
```

---

### **1. Store Setup (`store.js`)**

```js
// src/app/store.js
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from '../features/counter/counterSlice';

export const store = configureStore({
  reducer: {
    counter: counterReducer,
  },
});
```

---

### **2. Counter Slice (`counterSlice.js`)**

```js
// src/features/counter/counterSlice.js
import { createSlice } from '@reduxjs/toolkit';

const initialState = {
  value: 0,
};

export const counterSlice = createSlice({
  name: 'counter',
  initialState,
  reducers: {
    increment: (state) => {
      state.value += 1;
    },
    decrement: (state) => {
      state.value -= 1;
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload;
    },
    reset: (state) => {
      state.value = 0;
    },
  },
});

export const { increment, decrement, incrementByAmount, reset } = counterSlice.actions;

export default counterSlice.reducer;
```

---

### **3. Hooks (`hooks.js`)**

```js
// src/app/hooks.js
import { useDispatch, useSelector } from 'react-redux';

export const useAppDispatch = () => useDispatch();
export const useAppSelector = useSelector;
```

---

### **4. Counter Component (`Counter.jsx`)**

```jsx
// src/components/Counter.jsx
import { useAppSelector, useAppDispatch } from '../app/hooks';
import { increment, decrement, incrementByAmount, reset } from '../features/counter/counterSlice';

export function Counter() {
  const count = useAppSelector((state) => state.counter.value);
  const dispatch = useAppDispatch();

  return (
    <div>
      <h2>Count: {count}</h2>
      <button onClick={() => dispatch(increment())}>+</button>
      <button onClick={() => dispatch(decrement())}>-</button>
      <button onClick={() => dispatch(incrementByAmount(10))}>+10</button>
      <button onClick={() => dispatch(reset())}>Reset</button>
    </div>
  );
}
```

---

### **5. Main Entry (`main.jsx` or `index.jsx`)**

```jsx
// src/main.jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import { Provider } from 'react-redux';
import { store } from './app/store';

ReactDOM.createRoot(document.getElementById('root')).render(
  <Provider store={store}>
    <App />
  </Provider>
);
```

---

### 🌐 **Async Logic using createAsyncThunk**

### **postsSlice.js**

```js
// src/features/posts/postsSlice.js
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit';

export const fetchPosts = createAsyncThunk('posts/fetchPosts', async () => {
  const response = await fetch('https://jsonplaceholder.typicode.com/posts');
  return await response.json();
});

const postsSlice = createSlice({
  name: 'posts',
  initialState: {
    items: [],
    status: 'idle',
    error: null,
  },
  reducers: {},
  extraReducers: (builder) => {
    builder
      .addCase(fetchPosts.pending, (state) => {
        state.status = 'loading';
      })
      .addCase(fetchPosts.fulfilled, (state, action) => {
        state.status = 'succeeded';
        state.items = action.payload;
      })
      .addCase(fetchPosts.rejected, (state, action) => {
        state.status = 'failed';
        state.error = action.error?.message || 'Failed to fetch';
      });
  },
});

export default postsSlice.reducer;
```

---

### **PostsList.jsx**

```jsx
// src/components/PostsList.jsx
import { useEffect } from 'react';
import { useAppSelector, useAppDispatch } from '../app/hooks';
import { fetchPosts } from '../features/posts/postsSlice';

export function PostsList() {
  const dispatch = useAppDispatch();
  const posts = useAppSelector((state) => state.posts.items);
  const status = useAppSelector((state) => state.posts.status);
  const error = useAppSelector((state) => state.posts.error);

  useEffect(() => {
    if (status === 'idle') {
      dispatch(fetchPosts());
    }
  }, [status, dispatch]);

  if (status === 'loading') return <div>Loading...</div>;
  if (status === 'failed') return <div>Error: {error}</div>;

  return (
    <ul>
      {posts.slice(0, 5).map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
}
```

---

### ⭐ BONUS: RTK Query (API Layer) — JavaScript Version

```js
// src/features/api/apiSlice.js
import { createApi, fetchBaseQuery } from '@reduxjs/toolkit/query/react';

export const apiSlice = createApi({
  reducerPath: 'api',
  baseQuery: fetchBaseQuery({ 
    baseUrl: '/api',
    prepareHeaders: (headers, { getState }) => {
      // Add auth token if available
      const token = getState().auth?.token;
      if (token) {
        headers.set('Authorization', `Bearer ${token}`);
      }
      return headers;
    },
  }),
  tagTypes: ['User', 'Posts'],
  endpoints: (builder) => ({
    // Mutation: Login
    login: builder.mutation({
      query: (credentials) => ({
        url: '/auth/login',
        method: 'POST',
        body: credentials,
      }),
    }),
    
    // Query: Get Profile
    getProfile: builder.query({
      query: () => '/auth/me',
      providesTags: ['User'],
    }),
    
    // Query: Get Posts
    getPosts: builder.query({
      query: () => '/posts',
      providesTags: ['Posts'],
    }),
    
    // Mutation: Create Post
    createPost: builder.mutation({
      query: (newPost) => ({
        url: '/posts',
        method: 'POST',
        body: newPost,
      }),
      invalidatesTags: ['Posts'],
    }),
  }),
});

export const { 
  useLoginMutation, 
  useGetProfileQuery,
  useGetPostsQuery,
  useCreatePostMutation,
} = apiSlice;
```

```js
// src/app/store.js
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from '../features/counter/counterSlice';
import { apiSlice } from '../features/api/apiSlice';

export const store = configureStore({
  reducer: {
    counter: counterReducer,
    // RTK Query reducer - MUST be added with computed property
    [apiSlice.reducerPath]: apiSlice.reducer,
  },
  // RTK Query middleware - MUST be added for caching, invalidation, polling
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware().concat(apiSlice.middleware),
});
```

**Usage Example in Component**
```js
import { useGetPostsQuery, useCreatePostMutation } from '../features/api/apiSlice';

function PostsList() {
  // Automatic fetching, caching, and re-fetching
  const { 
    data: posts, 
    isLoading, 
    isError, 
    error,
    refetch 
  } = useGetPostsQuery();

  const [createPost, { isLoading: isCreating }] = useCreatePostMutation();

  const handleAddPost = async () => {
    try {
      await createPost({ title: 'New Post', body: 'Content' }).unwrap();
      // Posts will automatically refetch due to invalidatesTags
    } catch (err) {
      console.error('Failed to create post:', err);
    }
  };

  if (isLoading) return <div>Loading...</div>;
  if (isError) return <div>Error: {error.message}</div>;

  return (
    <div>
      <button onClick={handleAddPost} disabled={isCreating}>
        {isCreating ? 'Adding...' : 'Add Post'}
      </button>
      <button onClick={refetch}>Refresh</button>
      <ul>
        {posts?.map(post => (
          <li key={post.id}>{post.title}</li>
        ))}
      </ul>
    </div>
  );
}
```
---

## 14. Testing & Performance in React

### Testing Pyramid for React Applications

A well-balanced test suite follows the **testing pyramid** principle:

```text
E2E Tests (Cypress, Playwright)          ~5–10%
Integration Tests (React Testing Library) ~20–30%
Unit Tests (Jest + RTL)                  ~60–80%
```

- **Unit Tests** — Test individual components or pure functions
- **Integration Tests** — Test how multiple components work together
- **E2E Tests** — Test complete user flows in a real browser

---

### 1. React Testing Library (RTL)

**Officially recommended by the React team**

**Core Philosophy:**
> Write tests that resemble how real users interact with your application — query the DOM, not implementation details.

#### Installation

```bash
npm install --save-dev @testing-library/react @testing-library/jest-dom @testing-library/user-event
```

#### Setup (`jest.setup.js`)

```js
import '@testing-library/jest-dom';
```

Add to `jest.config.js` (or `package.json`):

```json
{
  "jest": {
    "setupFilesAfterEnv": ["<rootDir>/jest.setup.js"]
  }
}
```

---

#### Query Priority (Most → Least Recommended)

Always prefer queries that reflect real user behavior:

```tsx
// 1. Best — Accessible & user-facing
screen.getByRole('button', { name: /submit/i })
screen.getByRole('heading', { name: 'Welcome' })

// 2. Good
screen.getByLabelText('Email')
screen.getByPlaceholderText('Enter password')
screen.getByText('Login')

// 3. Only when necessary
screen.getByTestId('submit-button')

// Avoid — too tied to implementation
screen.getByTitle(), getByClassName(), querySelector()
```

---

### Example Component Tests

#### Counter Component

```tsx
test('increments and decrements count', async () => {
  const user = userEvent.setup()
  render(<Counter />)

  const increment = screen.getByRole('button', { name: 'plus' })
  const decrement = screen.getByRole('button', { name: 'minus' })

  await user.click(increment)
  expect(screen.getByText('1')).toBeInTheDocument()

  await user.click(decrement)
  expect(screen.getByText('0')).toBeInTheDocument()
})
```

#### Form Validation

```tsx
test('shows validation errors on submit', async () => {
  const user = userEvent.setup()
  render(<LoginForm />)

  await user.click(screen.getByRole('button', { name: /login/i }))

  expect(await screen.findByText(/email is required/i)).toBeInTheDocument()
  expect(screen.getByText(/password is required/i)).toBeInTheDocument()
})
```

---

### Testing Custom Hooks (Modern Approach)

Use `@testing-library/react`'s `renderHook`.

#### Hook Example

```tsx
export function useCounter(initialValue = 0) {
  const [count, setCount] = useState(initialValue)

  return {
    count,
    increment: () => setCount(c => c + 1),
    decrement: () => setCount(c => c - 1),
    reset: () => setCount(initialValue),
  }
}
```

#### Hook Test

```tsx
import { renderHook, act } from '@testing-library/react'

test('should increment counter', () => {
  const { result } = renderHook(() => useCounter())

  act(() => result.current.increment())
  expect(result.current.count).toBe(1)
})
```

---

### 2. Useful Jest + RTL Matchers

```tsx
expect(element).toBeInTheDocument()
expect(element).toHaveTextContent('Hello')
expect(element).toBeVisible()
expect(element).toBeDisabled()
expect(input).toHaveValue('john@example.com')
expect(element).toHaveClass('active')
expect(screen.getByTestId('alert')).toHaveAttribute('role', 'alert')
```

---

### 3. Mocking in Jest

#### Mock API Calls

```tsx
jest.mock('../api', () => ({
  fetchUser: jest.fn(),
}))
```

#### Mock React Router

```tsx
jest.mock('react-router-dom', () => ({
  ...jest.requireActual('react-router-dom'),
  useNavigate: () => jest.fn(),
  useParams: () => ({ id: '123' }),
}))
```

---

### 4. Performance Optimization in React

| Problem                        | Solution                                      | Tools / Hooks                     |
|--------------------------------|-----------------------------------------------|-----------------------------------|
| Unnecessary re-renders         | `React.memo`, `useMemo`, `useCallback`       | React DevTools Profiler           |
| Expensive child components     | Wrap with `React.memo`                        | `React.memo()`                    |
| Large lists / tables           | Virtualization                                | `react-window`, TanStack Virtual |
| Heavy calculations             | Memoize results                               | `useMemo`                         |
| Functions recreated on render  | Memoize callbacks                             | `useCallback`                     |
| Context triggering re-renders  | Split contexts or use selectors               | `useContextSelector`              |

#### React.memo Example

```tsx
const ExpensiveItem = React.memo(({ item, onClick }) => {
  return <div onClick={onClick}>{item.name}</div>
})
// Re-renders only when `item` or `onClick` actually change
```

#### useMemo Example

```tsx
const expensiveValue = useMemo(() => heavyCalculation(data), [data])
const filteredItems = useMemo(() => items.filter(i => i.active), [items])
```

#### useCallback Example

```tsx
const handleSave = useCallback((data) => {
  saveToServer(data)
}, [])
```

---

### React DevTools Profiler (Essential Tool)

1. Open React DevTools → **Profiler** tab  
2. Click **Record** (red circle)  
3. Interact with your app  
4. Stop recording  
5. Yellow/red flames = slow renders  

**Pro Tip:** Enable **“Highlight updates when components render”** to visually see re-renders.

---

### Virtualized Lists (react-window)

```bash
npm install react-window
```

```tsx
import { FixedSizeList } from 'react-window'

const Row = ({ index, style }) => (
  <div style={style}>{items[index].name}</div>
)

<FixedSizeList
  height={600}
  width={400}
  itemCount={items.length}
  itemSize={60}
>
  {Row}
</FixedSizeList>
```

---

### Performance Quick Reference

| Tool / Hook         | Use When                                           |
|---------------------|----------------------------------------------------|
| `React.memo`        | Child component re-renders with same props         |
| `useMemo`           | Expensive computations or stable references        |
| `useCallback`       | Passing callbacks to memoized children             |
| `react-window`      | Lists with 100+ items                              |
| `React.lazy + Suspense` | Lazy-load routes or heavy components           |
| Profiler API        | Measuring real-world render performance            |

---

### Bonus: Common Performance Patterns

#### Lazy Loading Routes

```tsx
const Dashboard = React.lazy(() => import('./Dashboard'))

<Suspense fallback={<div>Loading...</div>}>
  <Routes>
    <Route path="/dashboard" element={<Dashboard />} />
  </Routes>
</Suspense>
```

#### Memoized Redux Selectors (Reselect)

```tsx
import { createSelector } from '@reduxjs/toolkit'

const selectExpensiveData = createSelector(
  state => state.items,
  items => items.filter(heavyFilter)
)
```

## 15. Advanced React Patterns

### 🔥 1. Compound Components Pattern (JavaScript)

```jsx
import { createContext, useContext, useState } from 'react';

const ToggleContext = createContext(null);

function Toggle({ children }) {
  const [on, setOn] = useState(false);
  const toggle = () => setOn(!on);

  return (
    <ToggleContext.Provider value={{ on, toggle }}>
      {children}
    </ToggleContext.Provider>
  );
}

function useToggle() {
  const context = useContext(ToggleContext);
  if (!context) throw new Error('useToggle must be used within <Toggle>');
  return context;
}

// Compound components
Toggle.On = function ToggleOn({ children }) {
  const { on } = useToggle();
  return on ? <>{children}</> : null;
};

Toggle.Off = function ToggleOff({ children }) {
  const { on } = useToggle();
  return on ? null : <>{children}</>;
};

Toggle.Button = function ToggleButton() {
  const { on, toggle } = useToggle();
  return (
    <button onClick={toggle}>
      {on ? 'On' : 'Off'}
    </button>
  );
};

// Usage
function App() {
  return (
    <Toggle>
      <Toggle.Button />
      <Toggle.On>The button is ON</Toggle.On>
      <Toggle.Off>The button is OFF</Toggle.Off>
    </Toggle>
  );
}
```

---

### ⚙️ 2. Flexible Compound Components (with cloneElement)

```jsx
import React from 'react';

function Toggle({ on, children }) {
  return (
    <>
      {React.Children.map(children, child =>
        React.isValidElement(child)
          ? React.cloneElement(child, { on })
          : child
      )}
    </>
  );
}

Toggle.Button = function ({ on, ...props }) {
  return <button {...props}>Turn {on ? 'Off' : 'On'}</button>;
};
```

---

### 🖱 3. Render Props Pattern

```jsx
import { useState, useEffect } from 'react';

function MouseTracker({ render }) {
  const [pos, setPos] = useState({ x: 0, y: 0 });

  useEffect(() => {
    const handleMove = (e) => setPos({ x: e.clientX, y: e.clientY });
    window.addEventListener('mousemove', handleMove);
    return () => window.removeEventListener('mousemove', handleMove);
  }, []);

  return <div style={{ height: '100vh' }}>{render(pos)}</div>;
}

// Usage
<MouseTracker
  render={({ x, y }) => (
    <h1>The mouse is at: {x}, {y}</h1>
  )}
/>
```

---

### 🔐 4. Higher-Order Components (HOC)

```jsx
import React from 'react';
import { Navigate } from 'react-router-dom';
import { useAuth } from '../contexts/AuthContext';

function withAuth(WrappedComponent) {
  return function EnhancedComponent(props) {
    const { isAuthenticated, loading } = useAuth();

    if (loading) return <div>Loading...</div>;
    if (!isAuthenticated) return <Navigate to="/login" />;

    return <WrappedComponent {...props} />;
  };
}

// Usage
const ProtectedDashboard = withAuth(Dashboard);
```

---

### 🧩 5. Custom Hooks – The Most Powerful Pattern

```jsx
import { useState, useEffect } from 'react';

function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const abortController = new AbortController();

    fetch(url, { signal: abortController.signal })
      .then(res => res.json())
      .then(setData)
      .catch(err => err.name !== 'AbortError' && setError(err.message))
      .finally(() => setLoading(false));

    return () => abortController.abort();
  }, [url]);

  return { data, loading, error };
}

// Usage example
function UserProfile({ userId }) {
  const { data: user, loading } = useFetch(`/api/users/${userId}`);
  if (loading) return <div>Loading...</div>;
  return <div>{user?.name}</div>;
}
```

---

### 🎚 6. Controlled vs Uncontrolled via Custom Hooks

```jsx
function useToggle(initial = false) {
  const [on, setOn] = useState(initial);
  const toggle = () => setOn(prev => !prev);
  return { on, toggle, setOn };
}

// Usage
const { on, toggle } = useToggle();
```

---

### 🧠 7. State Reducer Pattern (useReducer)

```jsx
import { useReducer } from 'react';

function toggleReducer(state, action) {
  switch (action.type) {
    case 'toggle': return !state;
    case 'set': return action.payload;
    default: return state;
  }
}

function useToggleWithReducer() {
  return useReducer(toggleReducer, false);
}
```

---

### 🎨 8. Provider Pattern (Scalable Context)

```jsx
import { createContext, useState, useContext } from 'react';

const ThemeContext = createContext(undefined);

export function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');
  const toggleTheme = () => setTheme(prev => prev === 'light' ? 'dark' : 'light');

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

export function useTheme() {
  const context = useContext(ThemeContext);
  if (!context) throw new Error('useTheme must be used inside ThemeProvider');
  return context;
}
```

---

### 🧱 9. Layout / Children-as-Props Pattern

```jsx
function SplitPane({ left, right }) {
  return (
    <div className="split-pane">
      <div className="left">{left}</div>
      <div className="right">{right}</div>
    </div>
  );
}

// Usage
<SplitPane
  left={<Contacts />}
  right={<Chat />}
/>
```

---

### 🌀 10. Portals — Render Outside DOM Hierarchy

```jsx
import { useState, useEffect } from 'react';
import { createPortal } from 'react-dom';

function Modal({ children }) {
  const [mounted, setMounted] = useState(false);

  useEffect(() => setMounted(true), []);

  if (!mounted) return null;

  return createPortal(
    <div className="modal-backdrop">
      <div className="modal">{children}</div>
    </div>,
    document.body
  );
}
```

---

### 🧯 11. Error Boundaries (Still Class-Based)

```jsx
import React from 'react';

class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false, error: null };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }

  componentDidCatch(error, errorInfo) {
    // Log to error reporting service
    console.error('Error caught:', error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return this.props.fallback || <h1>Something went wrong.</h1>;
    }
    return this.props.children;
  }
}

export default ErrorBoundary;
```

```
Error Boundaries DO catch:
├── Errors in render methods
├── Errors in lifecycle methods
├── Errors in constructors of child components
└── Errors in static getDerivedStateFromError

Error Boundaries DO NOT catch:
├── Event handlers (use try/catch instead)
├── Asynchronous code (setTimeout, promises, async/await)
├── Server-side rendering (SSR)
├── Errors thrown in the error boundary itself
└── Errors in useEffect (partially - depends on timing)
```

```jsx
// Handling errors that Error Boundaries DON'T catch

function ButtonWithEventError() {
  const handleClick = () => {
    try {
      // Error boundaries WON'T catch this
      throw new Error('Event handler error');
    } catch (error) {
      // Handle it manually
      console.error('Caught in event handler:', error);
      // Show user-friendly message, report to service, etc.
    }
  };

  return <button onClick={handleClick}>Click me</button>;
}

function ComponentWithAsyncError() {
  const [error, setError] = useState(null);

  useEffect(() => {
    async function fetchData() {
      try {
        const response = await fetch('/api/data');
        if (!response.ok) throw new Error('Fetch failed');
      } catch (err) {
        // Error boundaries WON'T catch this
        setError(err);
        // Or: report to error service
      }
    }
    fetchData();
  }, []);

  if (error) return <div>Error: {error.message}</div>;
  return <div>Data loaded</div>;
}
```

``` jsx
// Usage
function App() {
  return (
    <ErrorBoundary fallback={<div>Something went wrong!</div>}>
      <Header />
      <MainContent />
      <Footer />
    </ErrorBoundary>
  );
}
```
---

### 🧵 12. Suspense for Data Fetching (Experimental)

```jsx
import { Suspense } from 'react';

function Profile({ resource }) {
  const user = resource.user.read(); // Will suspend
  return <h1>{user.name}</h1>;
}

<Suspense fallback={<div>Loading...</div>}>
  <Profile resource={resource} />
</Suspense>
```

---

### 📘 Advanced Patterns Summary Table

| Pattern               | Best For                       | Modern Alternative        |
| --------------------- | ------------------------------ | ------------------------- |
| Compound Components   | Tabs, dropdowns, accordions    | Still great in 2025       |
| Render Props          | Sharing logic like mouse/fetch | Custom Hooks              |
| HOC                   | Auth, analytics, logging       | Hooks + Composition       |
| Custom Hooks          | Reusable UI logic              | ⭐ #1 best practice        |
| Provider Pattern      | Context-based global state     | Context + hooks           |
| Portals               | Modals, overlays               | Still essential           |
| Error Boundaries      | Catch render errors            | Only option (class)       |
| State Reducer Pattern | Complex logic machines         | Zustand / XState optional |

---

