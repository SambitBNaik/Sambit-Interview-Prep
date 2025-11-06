# React Interview Preparation Guide

## Table of Contents
1. [React Fundamentals](#react-fundamentals)
2. [Components & Props](#components--props)
3. [State Management](#state-management)
4. [Hooks](#hooks)
5. [Lifecycle Methods](#lifecycle-methods)
6. [Advanced Concepts](#advanced-concepts)
7. [Performance Optimization](#performance-optimization)
8. [Common Interview Questions](#common-interview-questions)
9. [Coding Challenges](#coding-challenges)

---

## React Fundamentals

### What is React?
React is a JavaScript library for building user interfaces, developed by Facebook. It allows developers to create reusable UI components and efficiently update the DOM using a virtual DOM.

### Key Features
- **Component-Based Architecture**: Build encapsulated components that manage their own state
- **Virtual DOM**: Efficient rendering through DOM diffing
- **Unidirectional Data Flow**: Data flows from parent to child components
- **JSX**: JavaScript XML syntax for writing HTML-like code in JavaScript
- **Declarative**: Describe what the UI should look like, React handles the updates

### Virtual DOM
The Virtual DOM is a lightweight copy of the actual DOM. React uses it to:
1. Create a virtual representation of the UI
2. Compare changes (diffing algorithm)
3. Update only the changed parts in the real DOM (reconciliation)

---

## Components & Props

### Functional vs Class Components

**Functional Component:**
```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

**Class Component:**
```javascript
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

### Props
- Props are read-only inputs passed from parent to child components
- Cannot be modified by the child component
- Used for component communication

**Default Props:**
```javascript
function Button({ text = "Click me", onClick }) {
  return <button onClick={onClick}>{text}</button>;
}
```

### Prop Types
```javascript
import PropTypes from 'prop-types';

MyComponent.propTypes = {
  name: PropTypes.string.isRequired,
  age: PropTypes.number,
  onClick: PropTypes.func
};
```

---

## State Management

### useState Hook
```javascript
const [count, setCount] = useState(0);

// Updating state
setCount(count + 1);

// Functional update (when new state depends on previous)
setCount(prevCount => prevCount + 1);
```

### State vs Props
| State | Props |
|-------|-------|
| Mutable | Immutable |
| Managed within component | Passed from parent |
| Can be changed with setState | Read-only |
| Asynchronous updates | Synchronous |

### Lifting State Up
When multiple components need to share state, lift it up to their closest common ancestor.

```javascript
function Parent() {
  const [value, setValue] = useState('');
  
  return (
    <>
      <ChildA value={value} onChange={setValue} />
      <ChildB value={value} />
    </>
  );
}
```

---

## Hooks

### Rules of Hooks
1. Only call hooks at the top level (not inside loops, conditions, or nested functions)
2. Only call hooks from React function components or custom hooks

### Common Hooks

#### useEffect
```javascript
useEffect(() => {
  // Side effect code
  document.title = `Count: ${count}`;
  
  // Cleanup function (optional)
  return () => {
    // Cleanup code
  };
}, [count]); // Dependency array
```

**Dependency Array:**
- `[]` - Run once on mount
- `[dep1, dep2]` - Run when dependencies change
- No array - Run after every render

#### useContext
```javascript
const ThemeContext = React.createContext('light');

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Child />
    </ThemeContext.Provider>
  );
}

function Child() {
  const theme = useContext(ThemeContext);
  return <div>{theme}</div>;
}
```

#### useReducer
```javascript
const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      return state;
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);
  
  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
    </>
  );
}
```

#### useRef
```javascript
function TextInput() {
  const inputRef = useRef(null);
  
  const focusInput = () => {
    inputRef.current.focus();
  };
  
  return (
    <>
      <input ref={inputRef} />
      <button onClick={focusInput}>Focus</button>
    </>
  );
}
```

#### useMemo
Memoizes expensive calculations:
```javascript
const memoizedValue = useMemo(() => {
  return expensiveCalculation(a, b);
}, [a, b]);
```

#### useCallback
Memoizes functions:
```javascript
const memoizedCallback = useCallback(() => {
  doSomething(a, b);
}, [a, b]);
```

### Custom Hooks
```javascript
function useLocalStorage(key, initialValue) {
  const [value, setValue] = useState(() => {
    const item = window.localStorage.getItem(key);
    return item ? JSON.parse(item) : initialValue;
  });
  
  useEffect(() => {
    window.localStorage.setItem(key, JSON.stringify(value));
  }, [key, value]);
  
  return [value, setValue];
}
```

---

## Lifecycle Methods

### Class Component Lifecycle

**Mounting:**
1. constructor()
2. static getDerivedStateFromProps()
3. render()
4. componentDidMount()

**Updating:**
1. static getDerivedStateFromProps()
2. shouldComponentUpdate()
3. render()
4. getSnapshotBeforeUpdate()
5. componentDidUpdate()

**Unmounting:**
1. componentWillUnmount()

### Functional Component Equivalents

```javascript
// componentDidMount
useEffect(() => {
  // Code here runs once after mount
}, []);

// componentDidUpdate
useEffect(() => {
  // Code here runs after every update
});

// componentWillUnmount
useEffect(() => {
  return () => {
    // Cleanup code
  };
}, []);
```

---

## Advanced Concepts

### Context API
Provides a way to pass data through the component tree without prop drilling.

```javascript
const UserContext = React.createContext();

function App() {
  const user = { name: 'John', role: 'admin' };
  
  return (
    <UserContext.Provider value={user}>
      <Dashboard />
    </UserContext.Provider>
  );
}

function Dashboard() {
  const user = useContext(UserContext);
  return <div>Welcome, {user.name}</div>;
}
```

### Higher-Order Components (HOC)
A function that takes a component and returns a new component.

```javascript
function withAuth(Component) {
  return function AuthComponent(props) {
    const isAuthenticated = useAuth();
    
    if (!isAuthenticated) {
      return <Login />;
    }
    
    return <Component {...props} />;
  };
}

const ProtectedPage = withAuth(Dashboard);
```

### Render Props
```javascript
function MouseTracker({ render }) {
  const [position, setPosition] = useState({ x: 0, y: 0 });
  
  const handleMouseMove = (e) => {
    setPosition({ x: e.clientX, y: e.clientY });
  };
  
  return (
    <div onMouseMove={handleMouseMove}>
      {render(position)}
    </div>
  );
}

// Usage
<MouseTracker render={({ x, y }) => (
  <p>Mouse at {x}, {y}</p>
)} />
```

### Error Boundaries
```javascript
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }
  
  static getDerivedStateFromError(error) {
    return { hasError: true };
  }
  
  componentDidCatch(error, errorInfo) {
    console.log(error, errorInfo);
  }
  
  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong.</h1>;
    }
    return this.props.children;
  }
}
```

### Portals
Render children into a DOM node outside the parent hierarchy.

```javascript
import { createPortal } from 'react-dom';

function Modal({ children }) {
  return createPortal(
    children,
    document.getElementById('modal-root')
  );
}
```

### Code Splitting & Lazy Loading
```javascript
const LazyComponent = React.lazy(() => import('./LazyComponent'));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <LazyComponent />
    </Suspense>
  );
}
```

---

## Performance Optimization

### React.memo
Prevents unnecessary re-renders of functional components:
```javascript
const MyComponent = React.memo(function MyComponent({ data }) {
  return <div>{data}</div>;
});
```

### useCallback vs useMemo
- **useCallback**: Memoizes functions
- **useMemo**: Memoizes values

### Key Prop
Always provide unique keys when rendering lists:
```javascript
{items.map(item => (
  <Item key={item.id} data={item} />
))}
```

### Avoiding Inline Functions
```javascript
// Bad - creates new function on every render
<button onClick={() => handleClick(id)}>Click</button>

// Good - memoized function
const handleClickMemo = useCallback(() => handleClick(id), [id]);
<button onClick={handleClickMemo}>Click</button>
```

### Code Splitting
Split code into smaller chunks to reduce initial bundle size:
```javascript
const Dashboard = lazy(() => import('./Dashboard'));
```

### Virtualization
For long lists, use libraries like `react-window` or `react-virtualized`:
```javascript
import { FixedSizeList } from 'react-window';

<FixedSizeList
  height={400}
  itemCount={1000}
  itemSize={35}
  width={300}
>
  {Row}
</FixedSizeList>
```

---

## Common Interview Questions

### 1. What is the difference between state and props?
State is managed within a component and can be changed, while props are passed from parent components and are read-only.

### 2. What is the Virtual DOM and how does it work?
The Virtual DOM is a lightweight copy of the actual DOM. React compares the Virtual DOM with the real DOM and updates only the changed parts, making updates efficient.

### 3. Explain the component lifecycle
Components go through mounting (creation), updating (changes), and unmounting (removal) phases, each with specific lifecycle methods or hook equivalents.

### 4. What are hooks and why were they introduced?
Hooks are functions that let you use state and other React features in functional components. They were introduced to simplify code and avoid the complexity of class components.

### 5. What is the difference between useEffect and useLayoutEffect?
`useEffect` runs asynchronously after render, while `useLayoutEffect` runs synchronously before the browser paints, useful for DOM measurements.

### 6. How do you prevent unnecessary re-renders?
Use React.memo, useMemo, useCallback, proper key props, and avoid inline function definitions.

### 7. What is prop drilling and how do you avoid it?
Prop drilling is passing props through multiple levels of components. Avoid it using Context API, state management libraries, or component composition.

### 8. Explain controlled vs uncontrolled components
Controlled components have their state managed by React (value prop + onChange), while uncontrolled components manage their own state using refs.

### 9. What is reconciliation?
Reconciliation is the process by which React updates the DOM by comparing the new Virtual DOM with the previous one and applying minimal changes.

### 10. What are React fragments?
Fragments let you group multiple elements without adding extra DOM nodes:
```javascript
<>
  <Child1 />
  <Child2 />
</>
```

---

## Coding Challenges

### 1. Counter with Increment/Decrement
```javascript
function Counter() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <h1>{count}</h1>
      <button onClick={() => setCount(count + 1)}>+</button>
      <button onClick={() => setCount(count - 1)}>-</button>
      <button onClick={() => setCount(0)}>Reset</button>
    </div>
  );
}
```

### 2. Todo List
```javascript
function TodoList() {
  const [todos, setTodos] = useState([]);
  const [input, setInput] = useState('');
  
  const addTodo = () => {
    if (input.trim()) {
      setTodos([...todos, { id: Date.now(), text: input, done: false }]);
      setInput('');
    }
  };
  
  const toggleTodo = (id) => {
    setTodos(todos.map(todo => 
      todo.id === id ? { ...todo, done: !todo.done } : todo
    ));
  };
  
  return (
    <div>
      <input value={input} onChange={(e) => setInput(e.target.value)} />
      <button onClick={addTodo}>Add</button>
      <ul>
        {todos.map(todo => (
          <li key={todo.id} onClick={() => toggleTodo(todo.id)}>
            <span style={{ textDecoration: todo.done ? 'line-through' : 'none' }}>
              {todo.text}
            </span>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

### 3. Fetch Data from API
```javascript
function UserList() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    fetch('https://api.example.com/users')
      .then(res => res.json())
      .then(data => {
        setUsers(data);
        setLoading(false);
      })
      .catch(err => {
        setError(err.message);
        setLoading(false);
      });
  }, []);
  
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  
  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

### 4. Debounced Search
```javascript
function useDebounce(value, delay) {
  const [debouncedValue, setDebouncedValue] = useState(value);
  
  useEffect(() => {
    const handler = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);
    
    return () => clearTimeout(handler);
  }, [value, delay]);
  
  return debouncedValue;
}

function Search() {
  const [search, setSearch] = useState('');
  const debouncedSearch = useDebounce(search, 500);
  
  useEffect(() => {
    if (debouncedSearch) {
      // Perform search API call
      console.log('Searching for:', debouncedSearch);
    }
  }, [debouncedSearch]);
  
  return (
    <input 
      value={search} 
      onChange={(e) => setSearch(e.target.value)} 
      placeholder="Search..."
    />
  );
}
```

### 5. Modal Component
```javascript
function Modal({ isOpen, onClose, children }) {
  if (!isOpen) return null;
  
  return (
    <div style={{
      position: 'fixed',
      top: 0,
      left: 0,
      right: 0,
      bottom: 0,
      backgroundColor: 'rgba(0,0,0,0.5)',
      display: 'flex',
      alignItems: 'center',
      justifyContent: 'center'
    }} onClick={onClose}>
      <div style={{
        backgroundColor: 'white',
        padding: '20px',
        borderRadius: '8px'
      }} onClick={(e) => e.stopPropagation()}>
        {children}
        <button onClick={onClose}>Close</button>
      </div>
    </div>
  );
}
```

---

## Study Tips

1. **Practice Building Projects**: Create real-world applications like todo lists, weather apps, or e-commerce sites
2. **Understand Core Concepts**: Focus on fundamentals before advanced patterns
3. **Read Documentation**: React's official documentation is excellent
4. **Code Daily**: Consistent practice is key
5. **Review Code**: Study well-written React codebases on GitHub
6. **Mock Interviews**: Practice explaining concepts out loud
7. **Build a Portfolio**: Showcase your projects on GitHub

## Recommended Resources

- **Official React Documentation**: https://react.dev
- **React Patterns**: Common design patterns and best practices
- **Frontend Masters**: React courses
- **Egghead.io**: Short, focused React tutorials
- **Kent C. Dodds Blog**: Advanced React concepts
- **React DevTools**: Browser extension for debugging

---

## Good Luck!

Remember: Understanding the "why" behind React's design decisions is just as important as knowing "how" to use it. Be prepared to explain your reasoning and discuss trade-offs in your solutions.