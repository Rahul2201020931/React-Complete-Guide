# React Fundamentals Reference 📚

A comprehensive reference guide for React core concepts, patterns, and best practices.

## Table of Contents

1. [Core Concepts](#core-concepts)
2. [JSX Syntax](#jsx-syntax)
3. [Components](#components)
4. [Props and State](#props-and-state)
5. [Event Handling](#event-handling)
6. [Lifecycle Methods](#lifecycle-methods)
7. [Hooks](#hooks)
8. [Context API](#context-api)
9. [Performance Optimization](#performance-optimization)
10. [Common Patterns](#common-patterns)

## Core Concepts

### What is React?

React is a JavaScript library for building user interfaces, particularly web applications. It was created by Facebook and is now maintained by Facebook and the community.

### Key Features
- **Component-Based**: Build encapsulated components that manage their own state
- **Declarative**: Describe what the UI should look like for any given state
- **Virtual DOM**: Efficient updates and rendering
- **Unidirectional Data Flow**: Data flows down from parent to child components
- **Learn Once, Write Anywhere**: Can be used for web, mobile, and desktop apps

### Virtual DOM

The Virtual DOM is a JavaScript representation of the real DOM. React uses it to optimize updates by:
1. Creating a virtual representation of the UI
2. Comparing it with the previous version
3. Updating only the changed parts of the real DOM

## JSX Syntax

### Basic JSX
```jsx
const element = <h1>Hello, World!</h1>;
```

### JSX with Expressions
```jsx
const name = "React";
const element = <h1>Hello, {name}!</h1>;
```

### JSX Attributes
```jsx
const element = <div className="container" id="main">Content</div>;
```

### JSX with Children
```jsx
const element = (
  <div>
    <h1>Title</h1>
    <p>Paragraph</p>
  </div>
);
```

### JSX Rules
- Must have a single root element
- Use `className` instead of `class`
- Use `htmlFor` instead of `for`
- Self-closing tags must end with `/>`
- Use camelCase for event handlers

## Components

### Functional Components
```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

// Arrow function
const Welcome = (props) => <h1>Hello, {props.name}</h1>;
```

### Class Components
```jsx
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

### Component Composition
```jsx
const App = () => (
  <div>
    <Header />
    <Main />
    <Footer />
  </div>
);
```

## Props and State

### Props
Props are inputs to components. They are passed from parent to child components.

```jsx
// Parent component
const App = () => <Welcome name="John" age={25} />;

// Child component
const Welcome = ({ name, age }) => (
  <h1>Hello, {name}! You are {age} years old.</h1>
);
```

### State
State is data that belongs to a component and can change over time.

```jsx
const Counter = () => {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
};
```

### Props vs State
| **Props** | **State** |
|-----------|-----------|
| Immutable | Mutable |
| Passed from parent | Managed by component |
| Read-only | Can be updated |
| External data | Internal data |

## Event Handling

### Basic Event Handling
```jsx
const Button = () => {
  const handleClick = () => {
    alert('Button clicked!');
  };
  
  return <button onClick={handleClick}>Click me</button>;
};
```

### Event with Parameters
```jsx
const Item = ({ item, onDelete }) => {
  const handleDelete = () => {
    onDelete(item.id);
  };
  
  return (
    <div>
      <span>{item.name}</span>
      <button onClick={handleDelete}>Delete</button>
    </div>
  );
};
```

### Synthetic Events
React wraps native events in SyntheticEvent objects for cross-browser compatibility.

```jsx
const handleChange = (e) => {
  console.log(e.target.value); // SyntheticEvent
};
```

## Lifecycle Methods

### Class Component Lifecycle
```jsx
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = { data: null };
  }
  
  componentDidMount() {
    // Called after component is mounted
    this.fetchData();
  }
  
  componentDidUpdate(prevProps, prevState) {
    // Called after component updates
    if (prevProps.id !== this.props.id) {
      this.fetchData();
    }
  }
  
  componentWillUnmount() {
    // Called before component is unmounted
    this.cleanup();
  }
  
  render() {
    return <div>{this.state.data}</div>;
  }
}
```

### useEffect Hook
```jsx
const MyComponent = () => {
  const [data, setData] = useState(null);
  
  useEffect(() => {
    // componentDidMount equivalent
    fetchData();
    
    return () => {
      // componentWillUnmount equivalent
      cleanup();
    };
  }, []); // Empty dependency array = run once
  
  useEffect(() => {
    // componentDidUpdate equivalent
    if (id) {
      fetchData();
    }
  }, [id]); // Run when id changes
  
  return <div>{data}</div>;
};
```

## Hooks

### useState
```jsx
const [state, setState] = useState(initialValue);
```

### useEffect
```jsx
useEffect(() => {
  // Effect logic
}, [dependencies]);
```

### useContext
```jsx
const value = useContext(MyContext);
```

### useReducer
```jsx
const [state, dispatch] = useReducer(reducer, initialState);
```

### useMemo
```jsx
const memoizedValue = useMemo(() => {
  return expensiveCalculation(a, b);
}, [a, b]);
```

### useCallback
```jsx
const memoizedCallback = useCallback(() => {
  doSomething(a, b);
}, [a, b]);
```

### useRef
```jsx
const ref = useRef(initialValue);
```

## Context API

### Creating Context
```jsx
const MyContext = createContext(defaultValue);
```

### Provider
```jsx
<MyContext.Provider value={value}>
  <MyComponent />
</MyContext.Provider>
```

### Consumer
```jsx
const MyComponent = () => {
  const value = useContext(MyContext);
  return <div>{value}</div>;
};
```

## Performance Optimization

### React.memo
```jsx
const MyComponent = React.memo(({ name }) => {
  return <div>{name}</div>;
});
```

### useMemo
```jsx
const expensiveValue = useMemo(() => {
  return expensiveCalculation(data);
}, [data]);
```

### useCallback
```jsx
const handleClick = useCallback(() => {
  onItemClick(id);
}, [id, onItemClick]);
```

### Lazy Loading
```jsx
const LazyComponent = React.lazy(() => import('./LazyComponent'));

const App = () => (
  <Suspense fallback={<div>Loading...</div>}>
    <LazyComponent />
  </Suspense>
);
```

## Common Patterns

### Higher Order Components
```jsx
const withLoading = (WrappedComponent) => {
  return function WithLoadingComponent({ isLoading, ...props }) {
    if (isLoading) {
      return <div>Loading...</div>;
    }
    return <WrappedComponent {...props} />;
  };
};
```

### Render Props
```jsx
const DataProvider = ({ children }) => {
  const [data, setData] = useState(null);
  
  return children({ data, setData });
};

// Usage
<DataProvider>
  {({ data, setData }) => (
    <div>{data}</div>
  )}
</DataProvider>
```

### Compound Components
```jsx
const Tabs = ({ children }) => {
  const [activeTab, setActiveTab] = useState(0);
  
  return (
    <div>
      {React.Children.map(children, (child, index) =>
        React.cloneElement(child, {
          isActive: index === activeTab,
          onClick: () => setActiveTab(index)
        })
      )}
    </div>
  );
};
```

### Custom Hooks
```jsx
const useCounter = (initialValue = 0) => {
  const [count, setCount] = useState(initialValue);
  
  const increment = () => setCount(count + 1);
  const decrement = () => setCount(count - 1);
  const reset = () => setCount(initialValue);
  
  return { count, increment, decrement, reset };
};
```

## Best Practices

### 1. Keep Components Small
```jsx
// ✅ Good - Small, focused component
const UserCard = ({ user }) => (
  <div className="user-card">
    <h3>{user.name}</h3>
    <p>{user.email}</p>
  </div>
);
```

### 2. Use Descriptive Names
```jsx
// ✅ Good - Descriptive name
const UserProfileHeader = () => <header>...</header>;

// ❌ Bad - Generic name
const Header = () => <header>...</header>;
```

### 3. Extract Complex Logic
```jsx
// ✅ Good - Logic extracted
const UserList = ({ users }) => {
  const filteredUsers = users.filter(user => user.active);
  
  return (
    <ul>
      {filteredUsers.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
};
```

### 4. Use Keys for Lists
```jsx
// ✅ Good - Using keys
const UserList = ({ users }) => (
  <ul>
    {users.map(user => (
      <li key={user.id}>{user.name}</li>
    ))}
  </ul>
);
```

### 5. Handle Loading and Error States
```jsx
const UserProfile = ({ userId }) => {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    fetchUser(userId)
      .then(setUser)
      .catch(setError)
      .finally(() => setLoading(false));
  }, [userId]);
  
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;
  if (!user) return <div>User not found</div>;
  
  return <div>{user.name}</div>;
};
```

## Common Mistakes

### 1. Mutating State
```jsx
// ❌ Bad - Mutating state
const [items, setItems] = useState([]);
const addItem = (item) => {
  items.push(item); // This mutates the array
  setItems(items); // This won't trigger re-render
};

// ✅ Good - Creating new state
const addItem = (item) => {
  setItems([...items, item]); // This creates a new array
};
```

### 2. Missing Dependencies in useEffect
```jsx
// ❌ Bad - Missing dependencies
useEffect(() => {
  fetchData(id);
}, []); // Missing id dependency

// ✅ Good - Correct dependencies
useEffect(() => {
  fetchData(id);
}, [id]); // Includes id dependency
```

### 3. Using Index as Key
```jsx
// ❌ Bad - Using index as key
const UserList = ({ users }) => (
  <ul>
    {users.map((user, index) => (
      <li key={index}>{user.name}</li>
    ))}
  </ul>
);

// ✅ Good - Using unique ID as key
const UserList = ({ users }) => (
  <ul>
    {users.map(user => (
      <li key={user.id}>{user.name}</li>
    ))}
  </ul>
);
```

## Key Takeaways

1. **React is a library** for building user interfaces
2. **Components** are the building blocks of React apps
3. **Props** pass data from parent to child components
4. **State** manages data that can change over time
5. **Hooks** allow functional components to use state and lifecycle features
6. **JSX** makes React code more readable and maintainable
7. **Virtual DOM** optimizes rendering performance
8. **Performance optimization** is important for large applications
9. **Best practices** help write maintainable code
10. **Common mistakes** should be avoided for better code quality

---

**Master these fundamentals to become a React expert! 🚀**
