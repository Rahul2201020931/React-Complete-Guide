# React Interview Questions & Answers 🎯

A comprehensive collection of React interview questions with detailed answers to help you prepare for technical interviews.

## Table of Contents

1. [Basic Concepts](#basic-concepts)
2. [Components & JSX](#components--jsx)
3. [State & Props](#state--props)
4. [Hooks](#hooks)
5. [Lifecycle & Effects](#lifecycle--effects)
6. [Performance](#performance)
7. [Advanced Topics](#advanced-topics)
8. [Coding Challenges](#coding-challenges)

## Basic Concepts

### 1. What is React?

**Answer:**
React is a JavaScript library for building user interfaces, particularly web applications. It was created by Facebook and is now maintained by Facebook and the community.

**Key Features:**
- Component-based architecture
- Virtual DOM for efficient updates
- Declarative programming model
- Unidirectional data flow
- Learn once, write anywhere

### 2. What is the difference between a library and a framework?

**Answer:**
- **Library**: Collection of reusable functions that you call. You control the flow of the application.
- **Framework**: Complete solution with predefined structure. The framework controls the flow of the application.

**React is a library** because:
- It only handles the UI layer
- You choose your own routing, state management, etc.
- More flexibility in project structure

### 3. What is Virtual DOM?

**Answer:**
Virtual DOM is a JavaScript representation of the real DOM. React uses it to optimize updates by:

1. Creating a virtual representation of the UI
2. Comparing it with the previous version (diffing)
3. Updating only the changed parts of the real DOM

**Benefits:**
- Better performance
- Optimized updates
- Simplified development

### 4. What is JSX?

**Answer:**
JSX (JavaScript XML) is a syntax extension for JavaScript that allows you to write HTML-like code in your JavaScript files.

**Example:**
```jsx
const element = <h1>Hello, World!</h1>;
```

**Key Points:**
- Not HTML - it's JavaScript
- Gets compiled to JavaScript
- Can embed expressions using `{}`
- Must have a single root element

## Components & JSX

### 5. What are the different ways to create components?

**Answer:**

**Functional Components (Modern):**
```jsx
const MyComponent = (props) => {
  return <div>{props.name}</div>;
};
```

**Class Components (Legacy):**
```jsx
class MyComponent extends React.Component {
  render() {
    return <div>{this.props.name}</div>;
  }
}
```

**Arrow Function Components:**
```jsx
const MyComponent = ({ name }) => <div>{name}</div>;
```

### 6. What is the difference between functional and class components?

**Answer:**

| **Functional Components** | **Class Components** |
|---------------------------|----------------------|
| JavaScript functions | ES6 classes |
| Use hooks for state | Use this.state |
| Simpler syntax | More verbose |
| Better performance | Slightly slower |
| Modern approach | Legacy approach |
| No 'this' keyword | Uses 'this' keyword |

### 7. What are the rules of JSX?

**Answer:**
1. Must have a single root element
2. Use `className` instead of `class`
3. Use `htmlFor` instead of `for`
4. Self-closing tags must end with `/>`
5. Use camelCase for event handlers
6. Use `{}` for JavaScript expressions
7. Use `{/* */}` for comments

### 8. What is the difference between `{TitleComponent}` and `{<TitleComponent/>}`?

**Answer:**
- `{TitleComponent}`: Reference to the component function itself
- `{<TitleComponent/>}`: JSX element that renders the component

**Example:**
```jsx
// This renders the component
const element = <div>{<TitleComponent/>}</div>;

// This renders [object Function]
const element = <div>{TitleComponent}</div>;
```

## State & Props

### 9. What are props?

**Answer:**
Props are inputs to components. They are passed from parent to child components and are immutable.

**Example:**
```jsx
// Parent component
const App = () => <Welcome name="John" age={25} />;

// Child component
const Welcome = ({ name, age }) => (
  <h1>Hello, {name}! You are {age} years old.</h1>
);
```

### 10. What is state?

**Answer:**
State is data that belongs to a component and can change over time. When state changes, the component re-renders.

**Example:**
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

### 11. What is the difference between props and state?

**Answer:**

| **Props** | **State** |
|-----------|-----------|
| Immutable | Mutable |
| Passed from parent | Managed by component |
| Read-only | Can be updated |
| External data | Internal data |
| Cannot be changed by child | Can be changed by component |

### 12. How do you pass data from child to parent component?

**Answer:**
Pass a function as a prop from parent to child, then call that function from the child with the data.

**Example:**
```jsx
// Parent component
const Parent = () => {
  const [data, setData] = useState('');
  
  const handleDataFromChild = (childData) => {
    setData(childData);
  };
  
  return <Child onData={handleDataFromChild} />;
};

// Child component
const Child = ({ onData }) => {
  const sendData = () => {
    onData('Hello from child!');
  };
  
  return <button onClick={sendData}>Send Data</button>;
};
```

## Hooks

### 13. What are React Hooks?

**Answer:**
Hooks are functions that let you use state and other React features in functional components. They were introduced in React 16.8.

**Rules of Hooks:**
1. Only call hooks at the top level
2. Don't call hooks inside loops, conditions, or nested functions
3. Only call hooks from React function components

### 14. What is useState?

**Answer:**
useState is a Hook that lets you add state to functional components.

**Syntax:**
```jsx
const [state, setState] = useState(initialValue);
```

**Example:**
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

### 15. What is useEffect?

**Answer:**
useEffect is a Hook that lets you perform side effects in functional components.

**Syntax:**
```jsx
useEffect(() => {
  // Effect logic
}, [dependencies]);
```

**Example:**
```jsx
const UserProfile = ({ userId }) => {
  const [user, setUser] = useState(null);
  
  useEffect(() => {
    fetchUser(userId).then(setUser);
  }, [userId]);
  
  return <div>{user?.name}</div>;
};
```

### 16. What is the difference between useEffect and componentDidMount?

**Answer:**

| **useEffect** | **componentDidMount** |
|---------------|----------------------|
| Functional component | Class component |
| Can run multiple times | Runs only once |
| Can have dependencies | No dependencies |
| Can return cleanup function | No cleanup function |

### 17. What is useMemo?

**Answer:**
useMemo is a Hook that memoizes the result of a computation and only recalculates when dependencies change.

**Example:**
```jsx
const ExpensiveComponent = ({ data }) => {
  const expensiveValue = useMemo(() => {
    return expensiveCalculation(data);
  }, [data]);
  
  return <div>{expensiveValue}</div>;
};
```

### 18. What is useCallback?

**Answer:**
useCallback is a Hook that memoizes a function and only recreates it when dependencies change.

**Example:**
```jsx
const Parent = () => {
  const [count, setCount] = useState(0);
  
  const handleClick = useCallback(() => {
    console.log('Button clicked');
  }, []);
  
  return <Child onClick={handleClick} />;
};
```

## Lifecycle & Effects

### 19. What are the different phases of component lifecycle?

**Answer:**
1. **Mounting**: Component is created and inserted into DOM
2. **Updating**: Component re-renders due to state or props changes
3. **Unmounting**: Component is removed from DOM

### 20. What is the purpose of componentDidMount?

**Answer:**
componentDidMount is called after a component is mounted to the DOM. It's commonly used for:
- API calls
- Setting up subscriptions
- DOM manipulation
- Initializing third-party libraries

### 21. What is the purpose of componentWillUnmount?

**Answer:**
componentWillUnmount is called before a component is unmounted. It's used for:
- Cleaning up subscriptions
- Canceling network requests
- Removing event listeners
- Clearing timers

### 22. How do you handle side effects in functional components?

**Answer:**
Use the useEffect Hook to handle side effects in functional components.

**Example:**
```jsx
const MyComponent = () => {
  useEffect(() => {
    // Side effect logic
    const subscription = subscribe();
    
    return () => {
      // Cleanup
      subscription.unsubscribe();
    };
  }, []);
  
  return <div>Component content</div>;
};
```

## Performance

### 23. How do you optimize React performance?

**Answer:**
1. **React.memo**: Prevent unnecessary re-renders
2. **useMemo**: Memoize expensive calculations
3. **useCallback**: Memoize functions
4. **Code splitting**: Load components on demand
5. **Lazy loading**: Load components when needed
6. **Virtual scrolling**: For large lists
7. **Avoid inline functions**: In render methods

### 24. What is React.memo?

**Answer:**
React.memo is a higher-order component that prevents unnecessary re-renders by memoizing the component.

**Example:**
```jsx
const MyComponent = React.memo(({ name }) => {
  return <div>{name}</div>;
});
```

### 25. When should you use useMemo vs useCallback?

**Answer:**
- **useMemo**: When you want to memoize a value (computed result)
- **useCallback**: When you want to memoize a function

**Example:**
```jsx
const MyComponent = ({ data, onItemClick }) => {
  // Memoize computed value
  const expensiveValue = useMemo(() => {
    return expensiveCalculation(data);
  }, [data]);
  
  // Memoize function
  const handleClick = useCallback((id) => {
    onItemClick(id);
  }, [onItemClick]);
  
  return <div>{expensiveValue}</div>;
};
```

## Advanced Topics

### 26. What is Context API?

**Answer:**
Context API is a way to share data between components without passing props through every level.

**Example:**
```jsx
const MyContext = createContext();

const App = () => (
  <MyContext.Provider value="Hello">
    <MyComponent />
  </MyContext.Provider>
);

const MyComponent = () => {
  const value = useContext(MyContext);
  return <div>{value}</div>;
};
```

### 27. What is Redux?

**Answer:**
Redux is a predictable state container for JavaScript apps. It helps manage application state in a predictable way.

**Key Concepts:**
- Store: Single source of truth
- Actions: Plain objects describing what happened
- Reducers: Pure functions that specify how state changes

### 28. What is the difference between Context API and Redux?

**Answer:**

| **Context API** | **Redux** |
|-----------------|-----------|
| Built into React | External library |
| Simple state management | Complex state management |
| Good for small apps | Good for large apps |
| No middleware | Rich middleware ecosystem |
| No dev tools | Excellent dev tools |

### 29. What are Higher Order Components (HOCs)?

**Answer:**
HOCs are functions that take a component and return a new component with additional functionality.

**Example:**
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

### 30. What are Render Props?

**Answer:**
Render Props is a pattern where a component accepts a function as a prop and calls it to render content.

**Example:**
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

## Coding Challenges

### 31. Create a Counter Component

**Answer:**
```jsx
const Counter = () => {
  const [count, setCount] = useState(0);
  
  const increment = () => setCount(count + 1);
  const decrement = () => setCount(count - 1);
  const reset = () => setCount(0);
  
  return (
    <div>
      <h2>Count: {count}</h2>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
      <button onClick={reset}>Reset</button>
    </div>
  );
};
```

### 32. Create a Todo List Component

**Answer:**
```jsx
const TodoList = () => {
  const [todos, setTodos] = useState([]);
  const [inputValue, setInputValue] = useState('');
  
  const addTodo = () => {
    if (inputValue.trim()) {
      setTodos([...todos, {
        id: Date.now(),
        text: inputValue,
        completed: false
      }]);
      setInputValue('');
    }
  };
  
  const toggleTodo = (id) => {
    setTodos(todos.map(todo =>
      todo.id === id ? { ...todo, completed: !todo.completed } : todo
    ));
  };
  
  const deleteTodo = (id) => {
    setTodos(todos.filter(todo => todo.id !== id));
  };
  
  return (
    <div>
      <input
        value={inputValue}
        onChange={(e) => setInputValue(e.target.value)}
        onKeyPress={(e) => e.key === 'Enter' && addTodo()}
      />
      <button onClick={addTodo}>Add</button>
      <ul>
        {todos.map(todo => (
          <li key={todo.id}>
            <input
              type="checkbox"
              checked={todo.completed}
              onChange={() => toggleTodo(todo.id)}
            />
            <span style={{ textDecoration: todo.completed ? 'line-through' : 'none' }}>
              {todo.text}
            </span>
            <button onClick={() => deleteTodo(todo.id)}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
};
```

### 33. Create a Custom Hook for API Calls

**Answer:**
```jsx
const useApi = (url) => {
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
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };
    
    fetchData();
  }, [url]);
  
  return { data, loading, error };
};
```

### 34. Create a Form with Validation

**Answer:**
```jsx
const ContactForm = () => {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    message: ''
  });
  const [errors, setErrors] = useState({});
  
  const handleChange = (e) => {
    setFormData({
      ...formData,
      [e.target.name]: e.target.value
    });
  };
  
  const validateForm = () => {
    const newErrors = {};
    
    if (!formData.name) newErrors.name = 'Name is required';
    if (!formData.email) newErrors.email = 'Email is required';
    if (!formData.message) newErrors.message = 'Message is required';
    
    return newErrors;
  };
  
  const handleSubmit = (e) => {
    e.preventDefault();
    const newErrors = validateForm();
    
    if (Object.keys(newErrors).length === 0) {
      console.log('Form submitted:', formData);
      setFormData({ name: '', email: '', message: '' });
    } else {
      setErrors(newErrors);
    }
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input
        name="name"
        value={formData.name}
        onChange={handleChange}
        placeholder="Name"
      />
      {errors.name && <span>{errors.name}</span>}
      
      <input
        name="email"
        type="email"
        value={formData.email}
        onChange={handleChange}
        placeholder="Email"
      />
      {errors.email && <span>{errors.email}</span>}
      
      <textarea
        name="message"
        value={formData.message}
        onChange={handleChange}
        placeholder="Message"
      />
      {errors.message && <span>{errors.message}</span>}
      
      <button type="submit">Submit</button>
    </form>
  );
};
```

## Key Takeaways

1. **Understand the basics**: Components, JSX, props, state
2. **Master hooks**: useState, useEffect, useMemo, useCallback
3. **Know lifecycle**: Mounting, updating, unmounting
4. **Performance matters**: React.memo, useMemo, useCallback
5. **Advanced patterns**: HOCs, render props, custom hooks
6. **Practice coding**: Build projects, solve problems
7. **Stay updated**: Follow React updates and best practices

---

**Good luck with your React interviews! 🚀**

*Remember: Understanding concepts is more important than memorizing answers.*
