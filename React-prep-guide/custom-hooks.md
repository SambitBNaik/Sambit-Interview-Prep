# React Custom Hooks - Complete Guide

## Table of Contents
1. [Introduction](#introduction)
2. [What are Custom Hooks?](#what-are-custom-hooks)
3. [Why Use Custom Hooks?](#why-use-custom-hooks)
4. [Rules of Hooks](#rules-of-hooks)
5. [Creating Custom Hooks](#creating-custom-hooks)
6. [Common Custom Hook Examples](#common-custom-hook-examples)
7. [Best Practices](#best-practices)
8. [Advanced Patterns](#advanced-patterns)

---

## Introduction

Custom Hooks are a powerful feature in React that allow you to extract component logic into reusable functions. They enable you to share stateful logic between components without changing your component hierarchy.

---

## What are Custom Hooks?

A custom hook is a JavaScript function whose name starts with "use" and that may call other hooks. Custom hooks allow you to:

- Extract component logic into reusable functions
- Share stateful logic between multiple components
- Keep your components clean and focused
- Build your own abstractions on top of React's built-in hooks

### Basic Structure

```javascript
function useCustomHook(initialValue) {
  const [state, setState] = useState(initialValue);
  
  // Custom logic here
  
  return [state, setState];
}
```

---

## Why Use Custom Hooks?

### 1. **Code Reusability**
Extract common logic used across multiple components into a single, reusable hook.

### 2. **Separation of Concerns**
Keep your components focused on rendering UI while moving complex logic to custom hooks.

### 3. **Better Testing**
Custom hooks can be tested independently from components.

### 4. **Cleaner Components**
Reduce component complexity by moving logic outside the component body.

### 5. **Composition**
Combine multiple hooks to create more powerful abstractions.

---

## Rules of Hooks

When creating custom hooks, you must follow React's Rules of Hooks:

1. **Only call hooks at the top level** - Don't call hooks inside loops, conditions, or nested functions
2. **Only call hooks from React functions** - Call hooks from React function components or custom hooks
3. **Custom hook names must start with "use"** - This convention allows React to automatically check for violations of hook rules

---

## Creating Custom Hooks

### Step 1: Identify Reusable Logic

Look for patterns in your components that are repeated or could be abstracted.

### Step 2: Extract to a Function

Move the logic into a function that starts with "use".

### Step 3: Return Values

Return the state, functions, or values that components need.

### Example: Basic Counter Hook

```javascript
function useCounter(initialValue = 0) {
  const [count, setCount] = useState(initialValue);
  
  const increment = () => setCount(c => c + 1);
  const decrement = () => setCount(c => c - 1);
  const reset = () => setCount(initialValue);
  
  return { count, increment, decrement, reset };
}
```

**Usage:**

```javascript
function CounterComponent() {
  const { count, increment, decrement, reset } = useCounter(0);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
      <button onClick={reset}>Reset</button>
    </div>
  );
}
```

---

## Common Custom Hook Examples

### 1. useToggle

Toggle between boolean states.

```javascript
function useToggle(initialValue = false) {
  const [value, setValue] = useState(initialValue);
  
  const toggle = () => setValue(v => !v);
  const setTrue = () => setValue(true);
  const setFalse = () => setValue(false);
  
  return [value, { toggle, setTrue, setFalse }];
}
```

### 2. useLocalStorage

Sync state with localStorage.

```javascript
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
  
  const setValue = (value) => {
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
```

### 3. useFetch

Fetch data from an API with loading and error states.

```javascript
function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    const fetchData = async () => {
      try {
        setLoading(true);
        const response = await fetch(url);
        if (!response.ok) throw new Error('Network response was not ok');
        const result = await response.json();
        setData(result);
        setError(null);
      } catch (err) {
        setError(err.message);
        setData(null);
      } finally {
        setLoading(false);
      }
    };
    
    fetchData();
  }, [url]);
  
  return { data, loading, error };
}
```

### 4. useDebounce

Debounce a value to limit updates.

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
```

### 5. useWindowSize

Track window dimensions.

```javascript
function useWindowSize() {
  const [windowSize, setWindowSize] = useState({
    width: window.innerWidth,
    height: window.innerHeight,
  });
  
  useEffect(() => {
    const handleResize = () => {
      setWindowSize({
        width: window.innerWidth,
        height: window.innerHeight,
      });
    };
    
    window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
  }, []);
  
  return windowSize;
}
```

### 6. useOnClickOutside

Detect clicks outside an element.

```javascript
function useOnClickOutside(ref, handler) {
  useEffect(() => {
    const listener = (event) => {
      if (!ref.current || ref.current.contains(event.target)) {
        return;
      }
      handler(event);
    };
    
    document.addEventListener('mousedown', listener);
    document.addEventListener('touchstart', listener);
    
    return () => {
      document.removeEventListener('mousedown', listener);
      document.removeEventListener('touchstart', listener);
    };
  }, [ref, handler]);
}
```

### 7. usePrevious

Store the previous value of a state or prop.

```javascript
function usePrevious(value) {
  const ref = useRef();
  
  useEffect(() => {
    ref.current = value;
  }, [value]);
  
  return ref.current;
}
```

### 8. useInterval

Declarative interval hook.

```javascript
function useInterval(callback, delay) {
  const savedCallback = useRef();
  
  useEffect(() => {
    savedCallback.current = callback;
  }, [callback]);
  
  useEffect(() => {
    if (delay !== null) {
      const id = setInterval(() => savedCallback.current(), delay);
      return () => clearInterval(id);
    }
  }, [delay]);
}
```

---

## Best Practices

### 1. Name Your Hooks Appropriately

Always start with "use" and make the name descriptive.

```javascript
// Good
function useFormValidation() { }
function useAuthentication() { }

// Bad
function formValidation() { }
function authHook() { }
```

### 2. Keep Hooks Focused

Each hook should have a single responsibility.

```javascript
// Good - Focused hooks
function useUser() { }
function useUserPermissions() { }

// Bad - Too much responsibility
function useUserAndPermissionsAndSettings() { }
```

### 3. Return Appropriate Values

Consider what your hook's consumers need and return values accordingly.

```javascript
// Return array for simple cases
function useToggle() {
  return [value, toggle];
}

// Return object for multiple related values
function useFetch() {
  return { data, loading, error, refetch };
}
```

### 4. Handle Cleanup

Always clean up side effects in useEffect.

```javascript
function useEventListener(eventName, handler, element = window) {
  useEffect(() => {
    element.addEventListener(eventName, handler);
    
    // Cleanup function
    return () => element.removeEventListener(eventName, handler);
  }, [eventName, handler, element]);
}
```

### 5. Document Your Hooks

Add JSDoc comments to explain parameters and return values.

```javascript
/**
 * Custom hook to manage form state
 * @param {Object} initialValues - Initial form values
 * @returns {Object} Form state and handlers
 */
function useForm(initialValues) {
  // Implementation
}
```

### 6. Make Hooks Composable

Custom hooks can use other hooks (built-in or custom).

```javascript
function useUserData(userId) {
  const { data, loading, error } = useFetch(`/api/users/${userId}`);
  const [isAdmin, setIsAdmin] = useState(false);
  
  useEffect(() => {
    if (data?.role === 'admin') {
      setIsAdmin(true);
    }
  }, [data]);
  
  return { data, loading, error, isAdmin };
}
```

---

## Advanced Patterns

### 1. Hooks with Dependencies

Pass dependencies to allow dynamic behavior.

```javascript
function useApiCall(endpoint, options = {}) {
  const [result, setResult] = useState(null);
  
  useEffect(() => {
    fetch(endpoint, options)
      .then(res => res.json())
      .then(setResult);
  }, [endpoint, JSON.stringify(options)]);
  
  return result;
}
```

### 2. Hooks with Callbacks

Accept callback functions for flexible behavior.

```javascript
function useAsync(asyncFunction) {
  const [status, setStatus] = useState('idle');
  const [value, setValue] = useState(null);
  const [error, setError] = useState(null);
  
  const execute = useCallback(() => {
    setStatus('pending');
    setValue(null);
    setError(null);
    
    return asyncFunction()
      .then(response => {
        setValue(response);
        setStatus('success');
      })
      .catch(error => {
        setError(error);
        setStatus('error');
      });
  }, [asyncFunction]);
  
  return { execute, status, value, error };
}
```

### 3. Hooks with Reducers

Use useReducer for complex state logic.

```javascript
function useComplexState(initialState) {
  function reducer(state, action) {
    switch (action.type) {
      case 'increment':
        return { ...state, count: state.count + 1 };
      case 'decrement':
        return { ...state, count: state.count - 1 };
      case 'reset':
        return initialState;
      default:
        return state;
    }
  }
  
  const [state, dispatch] = useReducer(reducer, initialState);
  
  return [state, dispatch];
}
```

### 4. Memoized Custom Hooks

Use useMemo and useCallback for performance optimization.

```javascript
function useExpensiveCalculation(data) {
  const result = useMemo(() => {
    // Expensive calculation
    return data.map(item => complexOperation(item));
  }, [data]);
  
  const processData = useCallback((newData) => {
    // Processing logic
  }, []);
  
  return { result, processData };
}
```

---

## Conclusion

Custom hooks are a powerful tool for creating reusable, maintainable React code. They allow you to extract and share component logic while keeping your components clean and focused. By following the rules of hooks and best practices, you can build a library of custom hooks that make your React development more efficient and enjoyable.

Remember: Custom hooks are just JavaScript functions that use React hooks. Start simple, identify patterns in your code, and gradually build up your custom hook library as you recognize opportunities for reuse.