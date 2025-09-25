# Chapter 11: Context API & Prop Drilling (Sharing Data Between Components) 📊

## What is Prop Drilling?

**Prop Drilling** is the process of passing data through multiple levels of components, even when intermediate components don't need the data.

### Example of Prop Drilling
```jsx
// Grandparent component
const App = () => {
  const [user, setUser] = useState({ name: 'John', theme: 'dark' });
  
  return <Parent user={user} setUser={setUser} />;
};

// Parent component (doesn't use user data)
const Parent = ({ user, setUser }) => {
  return <Child user={user} setUser={setUser} />;
};

// Child component (doesn't use user data)
const Child = ({ user, setUser }) => {
  return <Grandchild user={user} setUser={setUser} />;
};

// Grandchild component (actually uses user data)
const Grandchild = ({ user, setUser }) => {
  return (
    <div>
      <h1>Hello, {user.name}!</h1>
      <button onClick={() => setUser({...user, theme: 'light'})}>
        Toggle Theme
      </button>
    </div>
  );
};
```

### Problems with Prop Drilling
- **Unnecessary Props**: Components receive props they don't use
- **Maintenance Issues**: Hard to refactor component structure
- **Performance**: Unnecessary re-renders
- **Code Complexity**: Makes components harder to understand

## What is Lifting State Up?

**Lifting State Up** is the process of moving state from child components to their common parent component.

### Before Lifting State
```jsx
// Two separate components with their own state
const Counter1 = () => {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(count + 1)}>Count: {count}</button>;
};

const Counter2 = () => {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(count + 1)}>Count: {count}</button>;
};

const App = () => (
  <div>
    <Counter1 />
    <Counter2 />
  </div>
);
```

### After Lifting State
```jsx
// State lifted to parent component
const App = () => {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <Counter count={count} onIncrement={() => setCount(count + 1)} />
      <Counter count={count} onIncrement={() => setCount(count + 1)} />
      <TotalCount count={count} />
    </div>
  );
};

const Counter = ({ count, onIncrement }) => (
  <button onClick={onIncrement}>Count: {count}</button>
);

const TotalCount = ({ count }) => <p>Total: {count}</p>;
```

## What is Context Provider and Context Consumer?

### Context Provider
**Context Provider** is a component that provides data to all its child components through React Context.

```jsx
import { createContext, useContext } from 'react';

// Create context
const UserContext = createContext();

// Provider component
const UserProvider = ({ children }) => {
  const [user, setUser] = useState({ name: 'John', theme: 'dark' });
  
  return (
    <UserContext.Provider value={{ user, setUser }}>
      {children}
    </UserContext.Provider>
  );
};

// Usage
const App = () => (
  <UserProvider>
    <Parent />
  </UserProvider>
);
```

### Context Consumer
**Context Consumer** is a component or hook that consumes data from React Context.

```jsx
// Using useContext hook (modern approach)
const Grandchild = () => {
  const { user, setUser } = useContext(UserContext);
  
  return (
    <div>
      <h1>Hello, {user.name}!</h1>
      <button onClick={() => setUser({...user, theme: 'light'})}>
        Toggle Theme
      </button>
    </div>
  );
};

// Using Consumer component (legacy approach)
const Grandchild = () => (
  <UserContext.Consumer>
    {({ user, setUser }) => (
      <div>
        <h1>Hello, {user.name}!</h1>
        <button onClick={() => setUser({...user, theme: 'light'})}>
          Toggle Theme
        </button>
      </div>
    )}
  </UserContext.Consumer>
);
```

## If You Don't Pass a Value to the Provider, Does It Take the Default Value?

**Yes**, if you don't pass a value to the Provider, it will use the default value from createContext.

### Example
```jsx
// Context with default value
const ThemeContext = createContext('light');

// Provider without value
const App = () => (
  <ThemeContext.Provider>
    <ThemedComponent />
  </ThemeContext.Provider>
);

// Component will use default value 'light'
const ThemedComponent = () => {
  const theme = useContext(ThemeContext);
  return <div className={theme}>Theme: {theme}</div>;
};
```

### Best Practice
```jsx
// Always provide a value
const ThemeContext = createContext();

const App = () => {
  const [theme, setTheme] = useState('light');
  
  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      <ThemedComponent />
    </ThemeContext.Provider>
  );
};
```

## Context API Patterns

### 1. Simple Context
```jsx
const ThemeContext = createContext();

export const ThemeProvider = ({ children }) => {
  const [theme, setTheme] = useState('light');
  
  const toggleTheme = () => {
    setTheme(prev => prev === 'light' ? 'dark' : 'light');
  };
  
  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};

export const useTheme = () => {
  const context = useContext(ThemeContext);
  if (!context) {
    throw new Error('useTheme must be used within ThemeProvider');
  }
  return context;
};
```

### 2. Multiple Contexts
```jsx
const UserContext = createContext();
const ThemeContext = createContext();

const App = () => (
  <UserProvider>
    <ThemeProvider>
      <MyComponent />
    </ThemeProvider>
  </UserProvider>
);

const MyComponent = () => {
  const { user } = useUser();
  const { theme } = useTheme();
  
  return <div className={theme}>Hello, {user.name}!</div>;
};
```

### 3. Context with Reducer
```jsx
const UserContext = createContext();

const userReducer = (state, action) => {
  switch (action.type) {
    case 'SET_USER':
      return { ...state, user: action.payload };
    case 'UPDATE_THEME':
      return { ...state, theme: action.payload };
    default:
      return state;
  }
};

export const UserProvider = ({ children }) => {
  const [state, dispatch] = useReducer(userReducer, {
    user: null,
    theme: 'light'
  });
  
  return (
    <UserContext.Provider value={{ state, dispatch }}>
      {children}
    </UserContext.Provider>
  );
};
```

## When to Use Context API

### Use Context When:
- **Global State**: Data needed by many components
- **Theme Management**: Dark/light mode
- **User Authentication**: User data across app
- **Language Settings**: Internationalization
- **Avoiding Prop Drilling**: When props go too deep

### Don't Use Context When:
- **Local State**: Component-specific data
- **Frequent Updates**: Performance-critical data
- **Complex State Logic**: Use Redux instead
- **Server State**: Use React Query instead

## Context API vs Other Solutions

### Context API vs Props
| **Context API** | **Props** |
|-----------------|-----------|
| Global access | Parent to child only |
| No prop drilling | Can cause prop drilling |
| More complex setup | Simple to use |
| Good for global state | Good for local state |

### Context API vs Redux
| **Context API** | **Redux** |
|-----------------|-----------|
| Built into React | External library |
| Simple state management | Complex state management |
| Good for small apps | Good for large apps |
| No dev tools | Excellent dev tools |
| No middleware | Rich middleware ecosystem |

## Performance Considerations

### Context Value Optimization
```jsx
// ❌ Bad - Creates new object on every render
const ThemeProvider = ({ children }) => {
  const [theme, setTheme] = useState('light');
  
  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};

// ✅ Good - Memoize the value
const ThemeProvider = ({ children }) => {
  const [theme, setTheme] = useState('light');
  
  const value = useMemo(() => ({
    theme,
    setTheme
  }), [theme]);
  
  return (
    <ThemeContext.Provider value={value}>
      {children}
    </ThemeContext.Provider>
  );
};
```

### Split Contexts
```jsx
// ❌ Bad - Single context with everything
const AppContext = createContext();

// ✅ Good - Split by concern
const UserContext = createContext();
const ThemeContext = createContext();
const NotificationContext = createContext();
```

## Common Patterns

### 1. Custom Hook Pattern
```jsx
const useUser = () => {
  const context = useContext(UserContext);
  if (!context) {
    throw new Error('useUser must be used within UserProvider');
  }
  return context;
};
```

### 2. Higher-Order Component Pattern
```jsx
const withUser = (WrappedComponent) => {
  return (props) => {
    const user = useUser();
    return <WrappedComponent {...props} user={user} />;
  };
};
```

### 3. Render Props Pattern
```jsx
const UserConsumer = ({ children }) => {
  const user = useUser();
  return children(user);
};

// Usage
<UserConsumer>
  {(user) => <div>Hello, {user.name}!</div>}
</UserConsumer>
```

## Best Practices

### 1. Create Custom Hooks
```jsx
const useAuth = () => {
  const context = useContext(AuthContext);
  if (!context) {
    throw new Error('useAuth must be used within AuthProvider');
  }
  return context;
};
```

### 2. Split Contexts by Domain
```jsx
// Separate contexts for different concerns
const AuthContext = createContext();
const ThemeContext = createContext();
const NotificationContext = createContext();
```

### 3. Provide Default Values
```jsx
const ThemeContext = createContext({
  theme: 'light',
  toggleTheme: () => {}
});
```

### 4. Use TypeScript
```tsx
interface ThemeContextType {
  theme: 'light' | 'dark';
  toggleTheme: () => void;
}

const ThemeContext = createContext<ThemeContextType | undefined>(undefined);
```

## Key Takeaways

1. **Prop Drilling** is passing data through multiple component levels
2. **Lifting State Up** moves state to common parent components
3. **Context Provider** provides data to child components
4. **Context Consumer** consumes data from context
5. **Default values** are used when no value is provided
6. **Context API** is good for global state management
7. **Performance** can be optimized with useMemo
8. **Split contexts** by domain for better organization
9. **Custom hooks** make context easier to use
10. **Best practices** improve code quality and maintainability

## Next Steps

- Learn about Redux for complex state management
- Understand data flow patterns
- Practice building applications with state management

---

**Ready to build our store? Let's move to Chapter 12! 🏪**
