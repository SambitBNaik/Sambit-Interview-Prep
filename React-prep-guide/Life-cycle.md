# React Component Lifecycle - Complete Documentation

## Table of Contents
1. [Introduction](#introduction)
2. [Class Component Lifecycle](#class-component-lifecycle)
3. [Functional Component Equivalents](#functional-component-equivalents)
4. [Best Practices](#best-practices)

---

## Introduction

React components go through a series of phases from creation to removal from the DOM. Understanding these lifecycle phases is crucial for managing side effects, optimizing performance, and building robust applications.

**Three Main Phases:**
- **Mounting**: Component is being created and inserted into the DOM
- **Updating**: Component is being re-rendered due to changes in props or state
- **Unmounting**: Component is being removed from the DOM

---

## Class Component Lifecycle

### Mounting Phase

These methods are called in order when a component is being created and inserted into the DOM.

#### 1. `constructor(props)`

**Purpose**: Initialize state and bind methods

**When it's called**: Before the component is mounted

**Use cases**:
- Initialize local state by assigning an object to `this.state`
- Bind event handler methods to the instance

```javascript
class MyComponent extends React.Component {
  constructor(props) {
    super(props); // Must call super(props) first
    
    // Initialize state
    this.state = {
      count: 0,
      user: null
    };
    
    // Bind methods (alternative: use arrow functions)
    this.handleClick = this.handleClick.bind(this);
  }
  
  handleClick() {
    this.setState({ count: this.state.count + 1 });
  }
}
```

**Important Notes**:
- Always call `super(props)` before any other statement
- Don't call `setState()` in the constructor
- Don't introduce side effects or subscriptions here
- Avoid copying props to state unless intentional

**Anti-pattern to avoid**:
```javascript
// DON'T do this - creates a state that ignores prop updates
constructor(props) {
  super(props);
  this.state = { color: props.color }; // Bad!
}
```

---

#### 2. `static getDerivedStateFromProps(props, state)`

**Purpose**: Sync state with props changes (rare use case)

**When it's called**: 
- Right before calling render, both on initial mount and subsequent updates
- Called on every render, regardless of the cause

**Returns**: 
- An object to update state, or `null` to update nothing

```javascript
class MyComponent extends React.Component {
  static getDerivedStateFromProps(props, state) {
    // Compare props to state
    if (props.userId !== state.prevUserId) {
      return {
        prevUserId: props.userId,
        user: null, // Reset user when userId changes
        isLoading: true
      };
    }
    return null; // No state update needed
  }
  
  componentDidMount() {
    this.fetchUser(this.state.prevUserId);
  }
  
  componentDidUpdate(prevProps, prevState) {
    if (this.state.prevUserId !== prevState.prevUserId) {
      this.fetchUser(this.state.prevUserId);
    }
  }
}
```

**Important Notes**:
- This is a static method - no access to `this`
- Rarely needed - most use cases can be solved more simply
- Causes re-renders on every parent render
- Use sparingly - usually there's a better solution

**When NOT to use**:
- Fetching data based on prop changes (use `componentDidUpdate` instead)
- Triggering side effects (use `componentDidUpdate` instead)
- Resetting state when props change (use fully controlled or fully uncontrolled components with keys)

---

#### 3. `render()`

**Purpose**: Return the JSX to be rendered

**When it's called**: Every time the component needs to update

**Returns**: 
- React elements (JSX)
- Arrays and fragments
- Portals
- String and numbers
- Booleans or null (renders nothing)

```javascript
class MyComponent extends React.Component {
  render() {
    const { name, age } = this.props;
    const { isLoading, error } = this.state;
    
    if (error) {
      return <div>Error: {error.message}</div>;
    }
    
    if (isLoading) {
      return <div>Loading...</div>;
    }
    
    return (
      <div>
        <h1>{name}</h1>
        <p>Age: {age}</p>
      </div>
    );
  }
}
```

**Important Notes**:
- Must be pure - same inputs always produce same output
- Should not modify component state
- Should not directly interact with the browser
- Don't call `setState()` here - causes infinite loop
- Can return null to render nothing

---

#### 4. `componentDidMount()`

**Purpose**: Perform side effects after component is mounted in the DOM

**When it's called**: Immediately after the component is inserted into the DOM tree

**Use cases**:
- Fetch data from APIs
- Set up subscriptions
- Add event listeners
- Initialize third-party libraries
- Set timers

```javascript
class MyComponent extends React.Component {
  componentDidMount() {
    // 1. Fetch data
    fetch(`https://api.example.com/user/${this.props.userId}`)
      .then(response => response.json())
      .then(data => this.setState({ user: data }));
    
    // 2. Set up subscriptions
    this.subscription = dataSource.subscribe(
      this.handleDataChange
    );
    
    // 3. Add event listeners
    window.addEventListener('resize', this.handleResize);
    
    // 4. Set timers
    this.timer = setInterval(() => {
      this.setState({ time: new Date() });
    }, 1000);
    
    // 5. Initialize third-party libraries
    this.chart = new Chart(this.chartRef.current, config);
  }
  
  componentWillUnmount() {
    // Clean up everything set up in componentDidMount
    this.subscription.unsubscribe();
    window.removeEventListener('resize', this.handleResize);
    clearInterval(this.timer);
    this.chart.destroy();
  }
}
```

**Important Notes**:
- Safe to call `setState()` here (will trigger re-render)
- DOM nodes are available
- Network requests can be made
- Always clean up in `componentWillUnmount()`

---

### Updating Phase

These methods are called when props or state change.

#### 1. `static getDerivedStateFromProps(props, state)`

Same as in mounting phase - called before every render.

---

#### 2. `shouldComponentUpdate(nextProps, nextState)`

**Purpose**: Optimize performance by preventing unnecessary re-renders

**When it's called**: Before rendering when new props or state are received

**Returns**: 
- `true` (default) - component should update
- `false` - skip rendering

```javascript
class MyComponent extends React.Component {
  shouldComponentUpdate(nextProps, nextState) {
    // Only update if userId changes
    if (this.props.userId !== nextProps.userId) {
      return true;
    }
    
    // Only update if count changes
    if (this.state.count !== nextState.count) {
      return true;
    }
    
    // Don't update otherwise
    return false;
  }
  
  render() {
    return <div>{this.props.userId}: {this.state.count}</div>;
  }
}
```

**Better Alternative - Use PureComponent**:
```javascript
// PureComponent does shallow comparison automatically
class MyComponent extends React.PureComponent {
  render() {
    return <div>{this.props.userId}: {this.state.count}</div>;
  }
}
```

**Important Notes**:
- Use for performance optimization
- Don't do deep comparisons (expensive)
- Consider using `React.PureComponent` instead
- Not called on initial render or when `forceUpdate()` is used
- Returning false doesn't prevent child components from re-rendering when their state changes

---

#### 3. `render()`

Same as in mounting phase.

---

#### 4. `getSnapshotBeforeUpdate(prevProps, prevState)`

**Purpose**: Capture information from the DOM before it's updated

**When it's called**: Right before the most recently rendered output is committed to the DOM

**Returns**: A snapshot value (or null) that will be passed to `componentDidUpdate()`

```javascript
class ScrollingList extends React.Component {
  listRef = React.createRef();
  
  getSnapshotBeforeUpdate(prevProps, prevState) {
    // Capture scroll position before new items are added
    if (prevProps.list.length < this.props.list.length) {
      const list = this.listRef.current;
      return list.scrollHeight - list.scrollTop;
    }
    return null;
  }
  
  componentDidUpdate(prevProps, prevState, snapshot) {
    // Adjust scroll position so new items don't push old ones out of view
    if (snapshot !== null) {
      const list = this.listRef.current;
      list.scrollTop = list.scrollHeight - snapshot;
    }
  }
  
  render() {
    return (
      <div ref={this.listRef}>
        {this.props.list.map(item => (
          <div key={item.id}>{item.text}</div>
        ))}
      </div>
    );
  }
}
```

**Use cases**:
- Reading scroll position
- Capturing cursor position
- Getting element dimensions before update
- Any DOM measurements needed before changes are applied

**Important Notes**:
- Rarely needed
- Called right before DOM mutations
- Return value passed to `componentDidUpdate()`
- Not called on initial render

---

#### 5. `componentDidUpdate(prevProps, prevState, snapshot)`

**Purpose**: Perform side effects after component updates

**When it's called**: Immediately after updating occurs (not called on initial render)

**Use cases**:
- Fetch data when props change
- Update DOM in response to state changes
- Perform network requests based on prop comparison

```javascript
class MyComponent extends React.Component {
  componentDidUpdate(prevProps, prevState, snapshot) {
    // 1. Fetch data when userId prop changes
    if (this.props.userId !== prevProps.userId) {
      this.fetchUser(this.props.userId);
    }
    
    // 2. Update DOM based on state
    if (this.state.isOpen && !prevState.isOpen) {
      document.body.style.overflow = 'hidden';
    } else if (!this.state.isOpen && prevState.isOpen) {
      document.body.style.overflow = 'auto';
    }
    
    // 3. Use snapshot from getSnapshotBeforeUpdate
    if (snapshot !== null) {
      console.log('Snapshot value:', snapshot);
    }
    
    // 4. Conditional setState (MUST have comparison to avoid infinite loop)
    if (this.state.value !== prevState.value) {
      this.calculateDerivedData();
    }
  }
  
  fetchUser(userId) {
    fetch(`https://api.example.com/user/${userId}`)
      .then(response => response.json())
      .then(data => this.setState({ user: data }));
  }
}
```

**Important Notes**:
- Can call `setState()` but MUST wrap it in a condition
- Not called on initial render
- Previous props and state available for comparison
- Receives snapshot value from `getSnapshotBeforeUpdate()`

**Common Pattern - Fetch on Prop Change**:
```javascript
componentDidUpdate(prevProps) {
  // Always compare before fetching to avoid infinite loop
  if (this.props.searchQuery !== prevProps.searchQuery) {
    this.performSearch(this.props.searchQuery);
  }
}
```

---

### Unmounting Phase

#### `componentWillUnmount()`

**Purpose**: Clean up resources before component is removed

**When it's called**: Immediately before a component is unmounted and destroyed

**Use cases**:
- Cancel network requests
- Remove event listeners
- Clear timers
- Unsubscribe from subscriptions
- Clean up third-party libraries

```javascript
class MyComponent extends React.Component {
  componentDidMount() {
    // Set up resources
    this.timer = setInterval(this.tick, 1000);
    this.subscription = dataSource.subscribe(this.handleData);
    window.addEventListener('resize', this.handleResize);
    this.chart = new Chart(this.canvasRef.current, config);
  }
  
  componentWillUnmount() {
    // Clean up ALL resources
    clearInterval(this.timer);
    this.subscription.unsubscribe();
    window.removeEventListener('resize', this.handleResize);
    this.chart.destroy();
    
    // Cancel any pending network requests
    this.abortController.abort();
  }
}
```

**Important Notes**:
- Don't call `setState()` here (component will never re-render)
- Should perform all necessary cleanup
- Called when component is removed from DOM
- Called when component unmounts due to parent re-render with different key

---

## Functional Component Equivalents

Functional components use Hooks to replicate lifecycle behavior.

### Basic Equivalents

#### `componentDidMount` - Run Once After Mount

```javascript
// Class Component
class MyComponent extends React.Component {
  componentDidMount() {
    console.log('Component mounted');
    this.fetchData();
  }
}

// Functional Component
function MyComponent() {
  useEffect(() => {
    console.log('Component mounted');
    fetchData();
  }, []); // Empty dependency array = runs once after mount
  
  return <div>My Component</div>;
}
```

---

#### `componentDidUpdate` - Run After Every Update

```javascript
// Class Component
class MyComponent extends React.Component {
  componentDidUpdate(prevProps, prevState) {
    console.log('Component updated');
  }
}

// Functional Component
function MyComponent() {
  useEffect(() => {
    console.log('Component updated');
  }); // No dependency array = runs after every render
  
  return <div>My Component</div>;
}
```

---

#### `componentWillUnmount` - Cleanup

```javascript
// Class Component
class MyComponent extends React.Component {
  componentDidMount() {
    this.timer = setInterval(() => console.log('tick'), 1000);
  }
  
  componentWillUnmount() {
    clearInterval(this.timer);
  }
}

// Functional Component
function MyComponent() {
  useEffect(() => {
    const timer = setInterval(() => console.log('tick'), 1000);
    
    // Cleanup function (like componentWillUnmount)
    return () => {
      clearInterval(timer);
    };
  }, []);
  
  return <div>My Component</div>;
}
```

---

### Advanced Patterns

#### Running Effect on Specific Prop/State Changes

```javascript
// Class Component
class UserProfile extends React.Component {
  componentDidUpdate(prevProps) {
    if (this.props.userId !== prevProps.userId) {
      this.fetchUser(this.props.userId);
    }
  }
}

// Functional Component
function UserProfile({ userId }) {
  useEffect(() => {
    fetchUser(userId);
  }, [userId]); // Runs when userId changes
  
  return <div>User Profile</div>;
}
```

---

#### Multiple Effects (Separation of Concerns)

```javascript
// Class Component - All logic in one place
class MyComponent extends React.Component {
  componentDidMount() {
    // Subscribe to multiple data sources
    this.userSubscription = userSource.subscribe(this.handleUser);
    this.postSubscription = postSource.subscribe(this.handlePosts);
    window.addEventListener('resize', this.handleResize);
  }
  
  componentWillUnmount() {
    this.userSubscription.unsubscribe();
    this.postSubscription.unsubscribe();
    window.removeEventListener('resize', this.handleResize);
  }
}

// Functional Component - Separate effects for each concern
function MyComponent() {
  // Effect 1: User subscription
  useEffect(() => {
    const subscription = userSource.subscribe(handleUser);
    return () => subscription.unsubscribe();
  }, []);
  
  // Effect 2: Post subscription
  useEffect(() => {
    const subscription = postSource.subscribe(handlePosts);
    return () => subscription.unsubscribe();
  }, []);
  
  // Effect 3: Window resize
  useEffect(() => {
    window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
  }, []);
  
  return <div>My Component</div>;
}
```

---

#### Complex Initialization (Constructor Alternative)

```javascript
// Class Component
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      data: this.expensiveCalculation(props)
    };
  }
  
  expensiveCalculation(props) {
    // Complex calculation
    return props.items.map(item => item * 2);
  }
}

// Functional Component - Use lazy initial state
function MyComponent({ items }) {
  // Function runs only once on initial render
  const [data, setData] = useState(() => {
    return items.map(item => item * 2);
  });
  
  return <div>{data.length} items</div>;
}
```

---

#### `getDerivedStateFromProps` Alternative

```javascript
// Class Component
class MyComponent extends React.Component {
  static getDerivedStateFromProps(props, state) {
    if (props.value !== state.prevValue) {
      return {
        prevValue: props.value,
        derived: props.value * 2
      };
    }
    return null;
  }
}

// Functional Component - Calculate during render
function MyComponent({ value }) {
  const derived = value * 2; // Just calculate it!
  return <div>{derived}</div>;
}

// Or use useMemo for expensive calculations
function MyComponent({ value }) {
  const derived = useMemo(() => {
    return expensiveCalculation(value);
  }, [value]);
  
  return <div>{derived}</div>;
}
```

---

#### `shouldComponentUpdate` Alternative

```javascript
// Class Component
class MyComponent extends React.Component {
  shouldComponentUpdate(nextProps) {
    return nextProps.userId !== this.props.userId;
  }
}

// Functional Component - Use React.memo
const MyComponent = React.memo(
  function MyComponent({ userId, name }) {
    return <div>{userId}: {name}</div>;
  },
  (prevProps, nextProps) => {
    // Return true if props are equal (skip render)
    // Return false if props are different (do render)
    return prevProps.userId === nextProps.userId;
  }
);

// Or for simple shallow comparison
const MyComponent = React.memo(function MyComponent({ userId, name }) {
  return <div>{userId}: {name}</div>;
});
```

---

#### `getSnapshotBeforeUpdate` Alternative

```javascript
// Class Component
class ScrollingList extends React.Component {
  getSnapshotBeforeUpdate(prevProps) {
    if (prevProps.list.length < this.props.list.length) {
      return this.listRef.current.scrollHeight;
    }
    return null;
  }
  
  componentDidUpdate(prevProps, prevState, snapshot) {
    if (snapshot !== null) {
      this.listRef.current.scrollTop += 
        this.listRef.current.scrollHeight - snapshot;
    }
  }
}

// Functional Component - Use useLayoutEffect
function ScrollingList({ list }) {
  const listRef = useRef();
  const prevListLength = useRef(list.length);
  
  useLayoutEffect(() => {
    // Runs synchronously after DOM mutations
    if (prevListLength.current < list.length) {
      const currentScrollHeight = listRef.current.scrollHeight;
      // Adjust scroll as needed
    }
    prevListLength.current = list.length;
  }, [list]);
  
  return (
    <div ref={listRef}>
      {list.map(item => <div key={item.id}>{item.text}</div>)}
    </div>
  );
}
```

---

### Complete Comparison Table

| Class Component | Functional Component (Hooks) | Notes |
|----------------|------------------------------|-------|
| `constructor()` | `useState()` initialization | Use lazy initialization for expensive calculations |
| `getDerivedStateFromProps()` | Calculate during render or `useMemo()` | Usually not needed in hooks |
| `componentDidMount()` | `useEffect(() => {}, [])` | Empty array means run once |
| `componentDidUpdate()` | `useEffect(() => {})` | No array means run every render |
| `componentDidUpdate()` with condition | `useEffect(() => {}, [dep])` | Runs when `dep` changes |
| `componentWillUnmount()` | `useEffect(() => { return cleanup }, [])` | Return cleanup function |
| `shouldComponentUpdate()` | `React.memo()` | Wraps component |
| `getSnapshotBeforeUpdate()` | `useLayoutEffect()` | Runs synchronously |
| Error boundaries | No hook equivalent yet | Still need class components |

---

## Best Practices

### General Guidelines

1. **Prefer Functional Components**: Use hooks for new code, they're simpler and more composable

2. **Separate Concerns**: Use multiple `useEffect` hooks instead of one large effect

3. **Always Clean Up**: Return cleanup functions from `useEffect` for subscriptions, timers, and event listeners

4. **Specify Dependencies**: Always include all dependencies in the dependency array (use ESLint plugin)

```javascript
// ❌ Bad - missing dependency
useEffect(() => {
  fetchData(userId);
}, []); // userId is missing!

// ✅ Good - includes all dependencies
useEffect(() => {
  fetchData(userId);
}, [userId]);
```

5. **Avoid Unnecessary Effects**: Don't use effects for calculations that can be done during render

```javascript
// ❌ Bad - unnecessary effect
function MyComponent({ items }) {
  const [total, setTotal] = useState(0);
  
  useEffect(() => {
    setTotal(items.reduce((sum, item) => sum + item.price, 0));
  }, [items]);
}

// ✅ Good - calculate during render
function MyComponent({ items }) {
  const total = items.reduce((sum, item) => sum + item.price, 0);
}
```

6. **Performance Optimization**: Use `React.memo`, `useMemo`, and `useCallback` judiciously

```javascript
// Only memoize expensive calculations
const expensiveValue = useMemo(() => {
  return calculateExpensiveValue(data);
}, [data]);

// Memoize callbacks passed to optimized child components
const handleClick = useCallback(() => {
  doSomething(value);
}, [value]);
```

---

### Common Pitfalls

**1. Infinite Loops**

```javascript
// ❌ Bad - infinite loop
useEffect(() => {
  setState(someValue); // Causes re-render
}); // No dependency array = runs every render

// ✅ Good - controlled updates
useEffect(() => {
  if (someCondition) {
    setState(someValue);
  }
}, [someCondition]);
```

**2. Stale Closures**

```javascript
// ❌ Bad - count is always 0 in the interval
function Counter() {
  const [count, setCount] = useState(0);
  
  useEffect(() => {
    const timer = setInterval(() => {
      setCount(count + 1); // count is stale!
    }, 1000);
    return () => clearInterval(timer);
  }, []); // Empty array causes closure over initial count
}

// ✅ Good - use functional update
function Counter() {
  const [count, setCount] = useState(0);
  
  useEffect(() => {
    const timer = setInterval(() => {
      setCount(c => c + 1); // Access current value
    }, 1000);
    return () => clearInterval(timer);
  }, []);
}
```

**3. Race Conditions**

```javascript
// ❌ Bad - race condition
useEffect(() => {
  fetchUser(userId).then(user => {
    setUser(user); // May set wrong user if userId changed!
  });
}, [userId]);

// ✅ Good - use cleanup to ignore stale results
useEffect(() => {
  let cancelled = false;
  
  fetchUser(userId).then(user => {
    if (!cancelled) {
      setUser(user);
    }
  });
  
  return () => {
    cancelled = true;
  };
}, [userId]);

// ✅ Even better - use AbortController
useEffect(() => {
  const controller = new AbortController();
  
  fetch(`/api/user/${userId}`, { signal: controller.signal })
    .then(response => response.json())
    .then(user => setUser(user))
    .catch(error => {
      if (error.name !== 'AbortError') {
        console.error(error);
      }
    });
  
  return () => controller.abort();
}, [userId]);
```

---

## Conclusion

This comprehensive guide covers all major lifecycle methods and their modern equivalents. The trend in React is moving towards functional components with hooks, but understanding class component lifecycles remains valuable for maintaining existing code and understanding React's fundamentals.

### Key Takeaways

- **Functional components with Hooks** are the modern standard
- **Class components** are still valid and useful for error boundaries
- **Lifecycle methods** help you understand when and how components update
- **useEffect** is the Swiss Army knife for side effects in functional components
- **Always clean up** your effects to prevent memory leaks
- **Specify dependencies correctly** to avoid bugs and optimize performance

### Further Reading

- [React Official Documentation - Lifecycle Methods](https://react.dev/reference/react/Component)
- [React Official Documentation - Hooks](https://react.dev/reference/react)
- [React Official Documentation - useEffect](https://react.dev/reference/react/useEffect)
- [Overreacted - A Complete Guide to useEffect](https://overreacted.io/a-complete-guide-to-useeffect/)