# Chapter 8: Class Components (The Old School Way) 🏗️

## Class Components vs Functional Components

### Class Components
```jsx
class Welcome extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  }
  
  render() {
    return <h1>Hello, {this.props.name}!</h1>;
  }
}
```

### Functional Components
```jsx
const Welcome = ({ name }) => {
  const [count, setCount] = useState(0);
  return <h1>Hello, {name}!</h1>;
};
```

## Lifecycle Methods

### Mounting Phase
1. **constructor()** - Initialize state and bind methods
2. **render()** - Render the component
3. **componentDidMount()** - Called after mounting

### Updating Phase
1. **render()** - Re-render the component
2. **componentDidUpdate()** - Called after updates

### Unmounting Phase
1. **componentWillUnmount()** - Called before unmounting

## Constructor and super(props)

### Why super(props)?
```jsx
class MyComponent extends React.Component {
  constructor(props) {
    super(props); // Must call super first
    this.state = { data: null };
  }
}
```

**Reasons:**
- **Inheritance**: Calls parent constructor
- **Props Access**: Makes this.props available
- **React Requirements**: Required by React

## componentDidMount

```jsx
class UserProfile extends React.Component {
  componentDidMount() {
    // Perfect for:
    // - API calls
    // - Setting up subscriptions
    // - DOM manipulation
    // - Initializing third-party libraries
    
    this.fetchUserData();
    this.setupWebSocket();
  }
  
  fetchUserData = async () => {
    const response = await fetch(`/api/users/${this.props.userId}`);
    const userData = await response.json();
    this.setState({ user: userData });
  };
}
```

## componentWillUnmount

```jsx
class Timer extends React.Component {
  componentDidMount() {
    this.timer = setInterval(() => {
      this.setState({ time: new Date() });
    }, 1000);
  }
  
  componentWillUnmount() {
    // Cleanup:
    // - Clear timers
    // - Cancel network requests
    // - Remove event listeners
    // - Unsubscribe from services
    
    clearInterval(this.timer);
  }
}
```

## Why Can't useEffect Callback Be Async?

### The Problem
```jsx
// ❌ This won't work
useEffect(async () => {
  const data = await fetchData();
  setData(data);
}, []);
```

### The Solution
```jsx
// ✅ Correct approach
useEffect(() => {
  const fetchData = async () => {
    const data = await fetchData();
    setData(data);
  };
  
  fetchData();
}, []);
```

**Why?**
- **useEffect** expects either nothing or a cleanup function
- **Async functions** always return a Promise
- **React** doesn't know how to handle Promise returns

## State in Class Components

### Setting State
```jsx
class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  }
  
  increment = () => {
    this.setState({ count: this.state.count + 1 });
  };
  
  incrementWithCallback = () => {
    this.setState({ count: this.state.count + 1 }, () => {
      console.log('State updated:', this.state.count);
    });
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
```

### Functional Updates
```jsx
increment = () => {
  this.setState(prevState => ({
    count: prevState.count + 1
  }));
};
```

## Event Handling in Class Components

### Arrow Functions (Recommended)
```jsx
class MyComponent extends React.Component {
  handleClick = () => {
    console.log('Button clicked');
  };
  
  render() {
    return <button onClick={this.handleClick}>Click me</button>;
  }
}
```

### Binding in Constructor
```jsx
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.handleClick = this.handleClick.bind(this);
  }
  
  handleClick() {
    console.log('Button clicked');
  }
  
  render() {
    return <button onClick={this.handleClick}>Click me</button>;
  }
}
```

## Error Boundaries

### Class Component Error Boundary
```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }
  
  static getDerivedStateFromError(error) {
    return { hasError: true };
  }
  
  componentDidCatch(error, errorInfo) {
    console.log('Error caught:', error, errorInfo);
  }
  
  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong.</h1>;
    }
    
    return this.props.children;
  }
}
```

### Usage
```jsx
<ErrorBoundary>
  <MyComponent />
</ErrorBoundary>
```

## Higher Order Components (HOCs)

### Creating HOCs
```jsx
const withLoading = (WrappedComponent) => {
  return class extends React.Component {
    render() {
      if (this.props.isLoading) {
        return <div>Loading...</div>;
      }
      return <WrappedComponent {...this.props} />;
    }
  };
};

// Usage
const UserProfile = ({ user }) => <div>{user.name}</div>;
const UserProfileWithLoading = withLoading(UserProfile);
```

## Ref in Class Components

### Creating Refs
```jsx
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.inputRef = React.createRef();
  }
  
  focusInput = () => {
    this.inputRef.current.focus();
  };
  
  render() {
    return (
      <div>
        <input ref={this.inputRef} />
        <button onClick={this.focusInput}>Focus Input</button>
      </div>
    );
  }
}
```

## When to Use Class Components

### Still Use Class Components For:
1. **Error Boundaries** (only class components can be error boundaries)
2. **Legacy Code** (when migrating existing code)
3. **Third-party Libraries** (some libraries require class components)

### Prefer Functional Components For:
1. **New Development** (modern approach)
2. **Hooks** (useState, useEffect, etc.)
3. **Performance** (slightly better performance)
4. **Simplicity** (easier to write and understand)

## Migration from Class to Functional

### Before (Class Component)
```jsx
class UserProfile extends React.Component {
  constructor(props) {
    super(props);
    this.state = { user: null, loading: true };
  }
  
  componentDidMount() {
    this.fetchUser();
  }
  
  fetchUser = async () => {
    const user = await api.getUser(this.props.userId);
    this.setState({ user, loading: false });
  };
  
  render() {
    if (this.state.loading) return <div>Loading...</div>;
    return <div>{this.state.user.name}</div>;
  }
}
```

### After (Functional Component)
```jsx
const UserProfile = ({ userId }) => {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    const fetchUser = async () => {
      const userData = await api.getUser(userId);
      setUser(userData);
      setLoading(false);
    };
    
    fetchUser();
  }, [userId]);
  
  if (loading) return <div>Loading...</div>;
  return <div>{user.name}</div>;
};
```

## Key Takeaways

1. **Class Components** are still valid but functional components are preferred
2. **Lifecycle Methods** provide hooks for different phases
3. **Constructor** must call super(props) first
4. **componentDidMount** is perfect for side effects
5. **componentWillUnmount** is essential for cleanup
6. **useEffect** cannot be async directly
7. **Error Boundaries** require class components
8. **HOCs** are powerful patterns for reusability
9. **Refs** work differently in class components
10. **Migration** from class to functional is straightforward

## Next Steps

- Learn about performance optimization
- Understand modern React patterns
- Practice building complex applications

---

**Ready to optimize? Let's move to Chapter 9! ⚡**
