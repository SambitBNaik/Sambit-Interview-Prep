# React Hooks – Complete Guide with Examples

-----

## 1\. Introduction to Hooks 🎣

### What are hooks?

**Hooks** are functions that let you "hook into" React state and lifecycle features from **functional components**. They allow you to write React applications without using class components.

### Why hooks were introduced?

Hooks solved several long-standing problems in React:

  * **Reusing Stateful Logic:** Before hooks, reusing logic between components (like setting up a subscription) was complex, often requiring patterns like Higher-Order Components (HOCs) or Render Props, which led to "wrapper hell" (deeply nested components). Hooks simplify this with **Custom Hooks**.
  * **Complex Components:** Class components with many lifecycle methods (`componentDidMount`, `componentDidUpdate`, `componentWillUnmount`) often contained related but scattered logic, making them hard to understand and maintain.
  * **The `this` Keyword:** Classes required an understanding of JavaScript's `this` keyword, which can be confusing and requires binding event handlers. Functional components avoid this entirely.

### Difference Between Class Components vs Functional Components

| Feature | Functional Components (with Hooks) | Class Components |
| :--- | :--- | :--- |
| **State** | Managed using `useState`, `useReducer` | Managed using `this.state` and `this.setState` |
| **Lifecycle** | Managed using `useEffect`, `useLayoutEffect` | Managed using dedicated methods (e.g., `componentDidMount`) |
| **Syntax** | Simple JavaScript functions | JavaScript classes extending `React.Component` |
| **`this`** | Not used | Heavily used, often requires binding |
| **Reusability** | Easier with **Custom Hooks** | Harder, often leads to HOCs/Render Props |

### Rules of Hooks

There are two fundamental rules you must follow when using hooks:

1.  **Only call Hooks at the Top Level:** Don't call hooks inside loops, conditions, or nested functions. Hooks must be called in the exact same order on every render.
2.  **Only call Hooks from React Functions:** Call them from **Functional Components** or from **Custom Hooks**. Don't call them from regular JavaScript functions.

-----

## 2\. Core React Hooks (with examples) ⚛️

### ➡️ `useState`

| Detail | Description |
| :--- | :--- |
| **When to use it** | To add **state** (data that changes and triggers a re-render) to functional components. |
| **Syntax** | `const [state, setState] = useState(initialState);` |
| **Best practices** | State updates are **asynchronous**. Use the **functional update** form (`setState(prev => ...)` ) if the new state depends on the previous state. |
| **Performance** | Causes a re-render only when the state value changes. |

```javascript
import React, { useState } from 'react';

function Counter() {
  // Real-world example: A simple counter component
  const [count, setCount] = useState(0);

  const increment = () => {
    // Best Practice: Functional update
    setCount(prevCount => prevCount + 1); 
  };

  return (
    <div>
      <p>Current count: {count}</p>
      <button onClick={increment}>Increment</button>
    </div>
  );
}
```

-----

### ➡️ `useEffect`

| Detail | Description |
| :--- | :--- |
| **When to use it** | To handle **side effects** like data fetching, subscriptions, manual DOM manipulation, or setting up event listeners. |
| **Syntax** | `useEffect(setupFunction, [dependencies]);` |
| **Real-world example** | Fetching data from an API on component mount. |
| **Best practices** | Always include all external values used inside the effect (props, state, or functions) in the **dependency array** (`[]`). Return a cleanup function for effects like subscriptions or timers. |
| **Performance** | Prevents unnecessary side-effect execution by only running when dependencies change. Missing dependencies can cause **stale closures**. |

```javascript
import React, { useState, useEffect } from 'react';

function DataFetcher({ userId }) {
  const [data, setData] = useState(null);

  useEffect(() => {
    // Real-world example: API call
    fetch(`https://api.example.com/users/${userId}`)
      .then(res => res.json())
      .then(setData);

    // Cleanup function: runs on unmount or before the effect runs again
    return () => {
      // e.g., cleanup a subscription, cancel a pending request
    };

  }, [userId]); // Dependency Array: effect re-runs when userId changes

  return <div>{data ? `User: ${data.name}` : 'Loading...'}</div>;
}
```

-----

### ➡️ `useContext`

| Detail | Description |
| :--- | :--- |
| **When to use it** | To subscribe to a **React Context**, allowing components to access global state or data without prop drilling. |
| **Syntax** | `const value = useContext(MyContext);` |
| **Real-world example** | Accessing the current theme (light/dark) or an authenticated user object from anywhere in the component tree. |
| **Best practices** | Use context for data that is truly **global** or shared by many components, not for component-local data. Combine with `useReducer` for complex global state management. |
| **Performance** | Any component consuming the context will **re-render** whenever the context value changes. |

```javascript
import React, { useContext, createContext } from 'react';

// 1. Create the Context
const ThemeContext = createContext('light');

function ThemedButton() {
  // 2. Consume the Context
  const theme = useContext(ThemeContext); // theme will be 'light'
  return <button className={theme}>Click Me</button>;
}
```

-----

### ➡️ `useRef`

| Detail | Description |
| :--- | :--- |
| **When to use it** | To reference a value that **persists** across re-renders without causing a re-render when it changes. Common uses: managing focus, text selection, or media playback, and storing a mutable value (like a timer ID). |
| **Syntax** | `const ref = useRef(initialValue);` |
| **Real-world example** | Getting a direct reference to a DOM element (e.g., an input field) or storing the ID of a `setInterval` timer. |
| **Best practices** | Use `ref.current` to access or change the value. **Do not** read or write `ref.current` during the render phase unless absolutely necessary. |
| **Performance** | Does **not** cause a re-render when its `.current` value is changed. Excellent for storing mutable values that don't belong in state. |

```javascript
import React, { useRef, useEffect } from 'react';

function TextInputWithFocusButton() {
  // Real-world example: Accessing a DOM node
  const inputRef = useRef(null);

  const focusInput = () => {
    inputRef.current.focus(); // Direct DOM manipulation
  };

  return (
    <>
      <input ref={inputRef} type="text" />
      <button onClick={focusInput}>Focus the Input</button>
    </>
  );
}
```

-----

### ➡️ `useReducer`

| Detail | Description |
| :--- | :--- |
| **When to use it** | As an alternative to `useState` for state that involves complex logic, multiple sub-values, or where the next state depends on the previous state. Excellent for managing state with a defined set of actions. |
| **Syntax** | `const [state, dispatch] = useReducer(reducer, initialArg, init);` |
| **Real-world example** | Managing the state for a shopping cart (add item, remove item, update quantity) or a complex form. |
| **Best practices** | The **reducer function** must be a **pure function**. Use `dispatch` to trigger state changes, making the change predictable and testable. |
| **Performance** | Can be more performant than multiple `useState` calls, especially when passing the `dispatch` function down, as `dispatch` is guaranteed to be stable and won't trigger re-renders on its own. |

```javascript
const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      throw new Error();
  }
}

function CounterReducer() {
  const [state, dispatch] = useReducer(reducer, initialState);
  
  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
    </>
  );
}
```

-----

### ➡️ `useCallback`

| Detail | Description |
| :--- | :--- |
| **When to use it** | To **memoize** a function. Returns a memoized callback that only changes if one of its dependencies has changed. Primarily used to prevent unnecessary re-renders in optimized child components (like those using `React.memo`). |
| **Syntax** | `const memoizedCallback = useCallback( () => { /* ... */ }, [dependencies]);` |
| **Real-world example** | Passing an event handler function (e.g., `handleChange`) as a prop to a child component that uses `React.memo`. |
| **Best practices** | Only use it when passing functions as props to **memoized children**. Do not wrap every function; this can hurt performance more than it helps. |
| **Performance** | Prevents child components from re-rendering by providing a stable function reference. The hook itself has a small overhead. |

```javascript
import React, { useCallback, useState } from 'react';

// Child component wrapped in React.memo for optimization
const Button = React.memo(({ onClick }) => {
  console.log('Button rendered');
  return <button onClick={onClick}>Click</button>;
});

function Parent() {
  const [count, setCount] = useState(0);

  // Real-world example: This function will ONLY be recreated when 'count' changes
  const handleClick = useCallback(() => {
    setCount(c => c + 1);
  }, [count]); // Dependency array

  // Button will NOT re-render if Parent re-renders (and count hasn't changed)
  return <Button onClick={handleClick} />; 
}
```

-----

### ➡️ `useMemo`

| Detail | Description |
| :--- | :--- |
| **When to use it** | To **memoize** a computed value. It caches the result of an expensive calculation and only re-calculates it if one of its dependencies changes. |
| **Syntax** | `const memoizedValue = useMemo( () => computeExpensiveValue(a, b), [a, b]);` |
| **Real-world example** | Filtering a large list, complex mathematical calculation, or deep comparison of objects derived from props/state. |
| **Best practices** | Use it for calculations that are **expensive** (take a noticeable amount of time). Do not wrap every computation. |
| **Performance** | Avoids redundant, expensive calculations on every re-render. Note that `useCallback(fn, deps)` is equivalent to `useMemo(() => fn, deps)`. |

```javascript
import React, { useMemo, useState } from 'react';

function List({ list, filterText }) {
  // Real-world example: Filtering a large list
  const filteredList = useMemo(() => {
    // This expensive calculation only runs when list or filterText changes
    return list.filter(item => item.name.includes(filterText));
  }, [list, filterText]); // Dependencies

  return (
    <ul>
      {filteredList.map(item => (
        <li key={item.id}>{item.name}</li>
      ))}
    </ul>
  );
}
```

-----

### ➡️ `useLayoutEffect`

| Detail | Description |
| :--- | :--- |
| **When to use it** | For side effects that need to run **synchronously** *after* the DOM mutations but *before* the browser paints the screen. Use it for DOM measurements, like getting scroll position, or adjusting styles based on layout. |
| **Syntax** | `useLayoutEffect(setupFunction, [dependencies]);` |
| **Real-world example** | Positioning a tooltip relative to its target element to prevent a noticeable flash of unstyled content (FOUC). |
| **Best practices** | Prefer `useEffect` for most side effects. Only use `useLayoutEffect` if you **need** to mutate the DOM and read the layout synchronously. |
| **Performance** | **Blocks** the browser from painting, which can hurt performance and cause a flicker if the logic is slow. Use sparingly. |

-----

### ➡️ `useImperativeHandle`

| Detail | Description |
| :--- | :--- |
| **When to use it** | Used with `useRef` and `forwardRef` to customize the instance value that is exposed when a parent component uses a `ref` on your component. It's for creating specific, intentional component APIs. |
| **Syntax** | `useImperativeHandle(ref, createHandle, [dependencies]);` |
| **Real-world example** | Exposing a method like `submitForm()` or `reset()` on a child form component to the parent component, without exposing *all* its internal logic. |
| **Best practices** | **Avoid** using imperative code. This hook should be used rarely, mainly for accessibility and integration with non-React libraries. |
| **Performance** | Minimal impact, as it only affects how a ref is structured, but overuse can lead to less declarative code. |

-----

### ➡️ `useDebugValue`

| Detail | Description |
| :--- | :--- |
| **When to use it** | To display a label for custom hooks in **React DevTools**. This is a debugging tool, not intended for application logic. |
| **Syntax** | `useDebugValue(value, formatFunction);` |
| **Real-world example** | Showing the current state value or status (e.g., 'Loading', 'Success') of a custom data-fetching hook. |
| **Best practices** | Use it inside custom hooks to make their internal state visible and meaningful in DevTools. |
| **Performance** | Only runs when DevTools is open. Minimal performance impact. |

-----

## 3\. Custom Hooks 🛠️

### What is a custom hook?

A **Custom Hook** is a JavaScript function whose name starts with "**use**" and that may call other hooks. They are a convention for encapsulating reusable, stateful logic that can be shared across multiple components without using HOCs or Render Props.

### How to create a custom hook

1.  Define a function starting with `use`.
2.  Inside the function, call other standard React hooks (e.g., `useState`, `useEffect`).
3.  Return the state and/or functions that your components need.

### Reusable Logic Example (`useLocalStorage`)

```javascript
import { useState, useEffect } from 'react';

// Custom Hook: useLocalStorage
function useLocalStorage(key, initialValue) {
  // 1. Get initial state from localStorage or use initialValue
  const [value, setValue] = useState(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.error(error);
      return initialValue;
    }
  });

  // 2. useEffect to sync state changes to localStorage
  useEffect(() => {
    try {
      window.localStorage.setItem(key, JSON.stringify(value));
    } catch (error) {
      console.error(error);
    }
  }, [key, value]); // Re-run when key or value changes

  // 3. Return the state and setter function
  return [value, setValue];
}

// Example usage in a component
function UserSettings() {
    // Reuses the logic: 'name' is synced to localStorage automatically
    const [name, setName] = useLocalStorage('userName', 'Guest');
    
    return (
        <input 
            type="text" 
            value={name} 
            onChange={e => setName(e.target.value)} 
            placeholder="Your Name"
        />
    );
}
```

-----

## 4\. React Hooks Use Cases 💡

| Use Case | Recommended Hooks | Description |
| :--- | :--- | :--- |
| **State management with hooks** | `useState`, `useReducer` | Use `useState` for simple values (strings, booleans). Use `useReducer` for complex state objects with multiple, related transitions. |
| **Avoiding unnecessary re-renders** | `useCallback`, `useMemo`, `React.memo` | Use `useCallback` for stable function references, `useMemo` for stable computed values, and `React.memo` to memoize component rendering. |
| **Form handling** | `useState`, **Custom Hooks** | `useState` manages input values. A custom hook (`useForm`) can encapsulate complex validation and submission logic. |
| **API calls** | `useEffect`, `useState`, **Custom Hooks** | `useEffect` handles the async fetch/cleanup logic. State manages loading/error/data status. **Custom Hooks** (`useFetch`) centralize this logic. |
| **Working with timers** | `useEffect`, `useRef` | `useEffect` sets up and tears down `setInterval`/`setTimeout`. `useRef` stores the timer ID so it persists across renders and can be cleared in the cleanup. |
| **Managing global state** | `useContext`, `useReducer` | `useReducer` manages the state logic, and the state/dispatch are passed down via `useContext` to avoid prop drilling. |

-----

## 5\. Common Mistakes with Hooks ❌

| Mistake | Explanation | How to Fix |
| :--- | :--- | :--- |
| **Infinite loops in `useEffect`** | Occurs when the effect updates a state variable that is also included in the dependency array, causing the effect to run, update state, trigger a re-render, and run the effect again. | Use the **functional update** form of `setState` to avoid needing the state variable in the dependency array, or fix missing dependencies. |
| **Missing dependency arrays** | An effect uses a prop or state variable but omits it from the dependency array (`[]`), leading to **stale closures** where the effect captures an old, stale value. | Use the ESLint rule `eslint-plugin-react-hooks` to automatically flag missing dependencies. |
| **Storing unnecessary data in state** | Putting a value in state that is easily computable from existing state or props. | Only store the minimum required state. Compute derived values during render. |
| **Using `useCallback`/`useMemo` incorrectly** | Wrapping simple functions or values. The overhead of memoization is greater than the re-calculation cost. | Only use for expensive calculations or when passing props to a **memoized child** component. |
| **Mutating state** | Directly modifying state objects/arrays (e.g., `state.list.push(newItem)`). State must be treated as **immutable**. | Always create a *new* copy of the object or array before making changes (e.g., `setList([...list, newItem])`). |
| **Problems with Stale Closures** | An effect or callback captures an outdated value of a variable from a previous render. | Add the variable to the dependency array. If that causes an infinite loop, use the **functional update** form or use a `useRef` to hold the mutable value. |

-----

## 6\. Advanced Patterns with Hooks ✨

### Debouncing with hooks

Debouncing ensures a function (like an API call from an input) is not called until a certain amount of time has passed without any further calls.

```javascript
import { useState, useEffect } from 'react';

// Custom Hook to debounce a value
function useDebounce(value, delay) {
  const [debouncedValue, setDebouncedValue] = useState(value);

  useEffect(() => {
    // Set a timer to update the debounced value
    const handler = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);

    // Cleanup: Cancel the timer if value or delay changes
    return () => {
      clearTimeout(handler);
    };
  }, [value, delay]); 

  return debouncedValue;
}

// Usage:
// const searchTerm = useDebounce(inputValue, 500); 
// useEffect(() => { /* fetch data using searchTerm */ }, [searchTerm]);
```

### Throttling with hooks

Throttling ensures a function (like a scroll handler) is only called at most once per a specified time period.

### Creating reusable UI logic

Custom hooks are the definitive pattern for creating reusable UI *logic*. E.g., `useWindowSize` (to track window width/height), `useScrollPosition`, `useToggle`.

### Using hooks with Zustand/Redux/React Query

  * **Zustand/Redux:** Libraries provide their own custom hooks (e.g., `useSelector`, `useStore`) to easily connect components to their global state.
  * **React Query:** Offers powerful custom hooks (e.g., `useQuery`, `useMutation`) to manage server-side state, caching, background re-fetching, and more.

### `useEvent` (Experimental/Proposed React Hook)

While currently not stable, the concept of a `useEvent` hook is to define an event handler that **always has the latest state/props** without needing to be included in the dependency array of `useCallback`. This is intended to solve the "stale closure" problem for event handlers without sacrificing performance.

-----

## 7\. Hooks Performance Optimization Guide 🚀

| Technique | Goal | When to Use |
| :--- | :--- | :--- |
| **`useMemo` vs `useCallback`** | **`useMemo`** memoizes a **value**. **`useCallback`** memoizes a **function**. | Use `useMemo` for expensive calculations. Use `useCallback` when passing functions to memoized child components. |
| **`React.memo` usage** | Prevents a component from re-rendering if its props are shallowly equal to the previous props. | For pure, presentation-only components with costly render logic, or when props are objects/functions from `useMemo`/`useCallback`. |
| **Avoiding heavy re-renders** | Keeping component trees shallow, using the functional update form of `useState`, and minimizing state changes. | Always ensure state updates are batched when possible (React does this automatically for event handlers). |
| **How hooks help improve performance** | Hooks simplify logic separation, which can lead to better architecture. More importantly, **memoization hooks** (`useMemo`, `useCallback`) offer a clear, standardized way to control rendering performance. | Use these hooks to pass stable props to memoized child components, thus skipping their entire render phase. |

-----

## 8\. Hooks Interview Questions 🎤

### Basic Questions

1.  **Conceptual:** What are React Hooks, and what problem do they solve?
2.  **Conceptual:** What are the two main "Rules of Hooks"?
3.  **Code-based:** How do you update state in a functional component using `useState` when the new state depends on the previous state?
    ```javascript
    // Answer should use the functional update form
    setCount(prevCount => prevCount + 1); 
    ```
4.  **Code-based:** What is the difference between `useEffect` with an empty array (`[]`) and no array at all?

### Intermediate Questions

1.  **Conceptual:** Explain the purpose of the dependency array in `useEffect`. What is a **stale closure**?
2.  **Conceptual:** When should you use `useReducer` instead of `useState`?
3.  **Code-based:** Explain how `useCallback` and `useMemo` work to prevent unnecessary re-renders in this scenario:
    ```javascript
    const Child = React.memo(({ handler, data }) => { /* ... */ });

    function Parent() {
        // Why are both useCallback and useMemo necessary here?
        const stableHandler = useCallback(() => { /* ... */ }, []);
        const stableData = useMemo(() => expensiveCalculation(props.id), [props.id]);

        return <Child handler={stableHandler} data={stableData} />;
    }
    ```
4.  **Conceptual:** How do you manage focus on a specific input element when a component mounts? (Answer: `useRef` and `useEffect`)

### Advanced Questions

1.  **Conceptual:** What is the difference between `useEffect` and `useLayoutEffect`, and when would you choose the latter?
2.  **Conceptual:** How does `useImperativeHandle` work, and why is it generally discouraged?
3.  **Code-based:** Create a custom hook that detects if a user is online or offline.
4.  **Conceptual:** Describe a pattern for managing global state in a small-to-medium application without an external library like Redux. (Answer: `useReducer` and `useContext`)

-----

## 9\. Summary ✅

### Advantages of Hooks

  * **Better Code Reusability:** Custom hooks simplify sharing stateful logic.
  * **Cleaner Code:** Functional components are often shorter and easier to read than classes.
  * **Easier to Learn:** No need to understand the complex `this` keyword or component lifecycle methods.
  * **Improved Separation of Concerns:** Logic related to a single side-effect stays together in one `useEffect` call.

### When NOT to use hooks

  * Inside **Class Components**. Hooks are only for functional components.
  * Inside **loops, conditions, or nested functions**. (Violates Rule 1).
  * Inside **regular JavaScript functions**. (Violates Rule 2).

### Best Practices Checklist

  * ✅ Always follow the two Rules of Hooks.
  * ✅ Use the ESLint plugin for hooks to enforce rules and dependency completeness.
  * ✅ Use **Custom Hooks** to extract and share complex, reusable logic.
  * ✅ Prefer the **functional update** form of `setState` (`setState(prev => ...)`).
  * ✅ **Clean up** effects with a return function in `useEffect` (e.g., clearing timers, removing listeners).
  * ✅ Only use `useMemo`/`useCallback` when there is a known performance issue or when passing props to a `React.memo` component.
  * ✅ Treat state as **immutable**; never mutate objects or arrays directly.