# React Programs Collection 💻

A super fun collection of React programs that you can build and learn from!

## Real-World Applications (Apps You Use Every Day! 📱)

These programs are inspired by the apps you probably use daily:
- **Social Media** 📘: Like buttons, comments, posts (like Facebook, Instagram)
- **E-commerce** 🛒: Shopping carts, product listings, reviews (like Amazon, eBay)
- **Weather Apps** 🌤️: Current conditions, forecasts (like Weather.com)
- **Task Management** ✅: Todo lists, project boards (like Trello, Asana)
- **Content Management** 📝: Blogs, portfolios (like Medium, WordPress)

### Fun Fact! 🎉
The first website ever created was in **1991** and it only had text - no colors, no images, no buttons! Now we can build amazing interactive apps with React! 🚀

## Table of Contents

1. [Basic Programs](#basic-programs) - Hello World, Components, JSX
2. [Component Examples](#component-examples) - Reusable UI components
3. [State Management](#state-management) - Managing app data
4. [Form Handling](#form-handling) - User input and validation
5. [API Integration](#api-integration) - Fetching external data
6. [Advanced Patterns](#advanced-patterns) - Hooks, Context, Performance

## Basic Programs

### Program 1: Hello World Without Create React App

**Real-World Use Case**: This is like building a simple landing page or welcome screen for a website. Think of it as the "Welcome to our app" message you see when you first visit a website.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Welcome to My App</title>
    <!-- React CDN - like including a library in your project -->
    <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <!-- Babel - converts JSX to regular JavaScript -->
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
</head>
<body>
    <!-- This is where our React app will be mounted -->
    <div id="root"></div>
    
    <script type="text/babel">
        // Create a React element (like creating a component)
        const element = <h1>Welcome to My Awesome App! 🚀</h1>;
        
        // Render the element to the DOM (like displaying it on the page)
        ReactDOM.render(element, document.getElementById('root'));
    </script>
</body>
</html>
```

**What This Does**: Creates a simple welcome message that appears when someone visits your website. It's the foundation of every React app.

### Program 2: Nested Elements

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Nested Elements</title>
    <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
</head>
<body>
    <div id="root"></div>
    
    <script type="text/babel">
        const element = (
            <div className="container">
                <h1>Welcome to React</h1>
                <p>This is a paragraph with some text.</p>
                <button onClick={() => alert('Button clicked!')}>
                    Click me
                </button>
            </div>
        );
        
        ReactDOM.render(element, document.getElementById('root'));
    </script>
</body>
</html>
```

### Program 3: Functional Component

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Functional Component</title>
    <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
</head>
<body>
    <div id="root"></div>
    
    <script type="text/babel">
        function Welcome(props) {
            return <h1>Hello, {props.name}!</h1>;
        }
        
        const element = <Welcome name="React Developer" />;
        ReactDOM.render(element, document.getElementById('root'));
    </script>
</body>
</html>
```

## Component Examples

### Counter Component

```jsx
import React, { useState } from 'react';

const Counter = () => {
  const [count, setCount] = useState(0);
  
  const increment = () => setCount(count + 1);
  const decrement = () => setCount(count - 1);
  const reset = () => setCount(0);
  
  return (
    <div className="counter">
      <h2>Counter: {count}</h2>
      <div className="buttons">
        <button onClick={decrement}>-</button>
        <button onClick={reset}>Reset</button>
        <button onClick={increment}>+</button>
      </div>
    </div>
  );
};

export default Counter;
```

### Todo List Component

```jsx
import React, { useState } from 'react';

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
    <div className="todo-list">
      <h2>Todo List</h2>
      <div className="add-todo">
        <input
          type="text"
          value={inputValue}
          onChange={(e) => setInputValue(e.target.value)}
          placeholder="Add a new todo..."
          onKeyPress={(e) => e.key === 'Enter' && addTodo()}
        />
        <button onClick={addTodo}>Add</button>
      </div>
      <ul>
        {todos.map(todo => (
          <li key={todo.id} className={todo.completed ? 'completed' : ''}>
            <input
              type="checkbox"
              checked={todo.completed}
              onChange={() => toggleTodo(todo.id)}
            />
            <span>{todo.text}</span>
            <button onClick={() => deleteTodo(todo.id)}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
};

export default TodoList;
```

### User Profile Component

```jsx
import React, { useState } from 'react';

const UserProfile = () => {
  const [user, setUser] = useState({
    name: '',
    email: '',
    age: '',
    bio: ''
  });
  
  const [isEditing, setIsEditing] = useState(false);
  
  const handleChange = (e) => {
    setUser({
      ...user,
      [e.target.name]: e.target.value
    });
  };
  
  const handleSave = () => {
    setIsEditing(false);
  };
  
  const handleEdit = () => {
    setIsEditing(true);
  };
  
  return (
    <div className="user-profile">
      <h2>User Profile</h2>
      {isEditing ? (
        <div className="edit-form">
          <input
            name="name"
            value={user.name}
            onChange={handleChange}
            placeholder="Name"
          />
          <input
            name="email"
            type="email"
            value={user.email}
            onChange={handleChange}
            placeholder="Email"
          />
          <input
            name="age"
            type="number"
            value={user.age}
            onChange={handleChange}
            placeholder="Age"
          />
          <textarea
            name="bio"
            value={user.bio}
            onChange={handleChange}
            placeholder="Bio"
          />
          <button onClick={handleSave}>Save</button>
        </div>
      ) : (
        <div className="profile-display">
          <h3>{user.name || 'No name'}</h3>
          <p>Email: {user.email || 'No email'}</p>
          <p>Age: {user.age || 'No age'}</p>
          <p>Bio: {user.bio || 'No bio'}</p>
          <button onClick={handleEdit}>Edit</button>
        </div>
      )}
    </div>
  );
};

export default UserProfile;
```

## State Management

### Multiple State Variables

```jsx
import React, { useState } from 'react';

const MultiStateExample = () => {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');
  const [age, setAge] = useState(0);
  const [isSubscribed, setIsSubscribed] = useState(false);
  
  const handleSubmit = (e) => {
    e.preventDefault();
    console.log({ name, email, age, isSubscribed });
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <h2>User Registration</h2>
      <input
        type="text"
        value={name}
        onChange={(e) => setName(e.target.value)}
        placeholder="Name"
        required
      />
      <input
        type="email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        placeholder="Email"
        required
      />
      <input
        type="number"
        value={age}
        onChange={(e) => setAge(parseInt(e.target.value))}
        placeholder="Age"
        required
      />
      <label>
        <input
          type="checkbox"
          checked={isSubscribed}
          onChange={(e) => setIsSubscribed(e.target.checked)}
        />
        Subscribe to newsletter
      </label>
      <button type="submit">Submit</button>
    </form>
  );
};

export default MultiStateExample;
```

### Object State Management

```jsx
import React, { useState } from 'react';

const ObjectStateExample = () => {
  const [formData, setFormData] = useState({
    firstName: '',
    lastName: '',
    email: '',
    phone: ''
  });
  
  const handleChange = (e) => {
    setFormData({
      ...formData,
      [e.target.name]: e.target.value
    });
  };
  
  const handleSubmit = (e) => {
    e.preventDefault();
    console.log('Form Data:', formData);
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <h2>Contact Form</h2>
      <input
        name="firstName"
        value={formData.firstName}
        onChange={handleChange}
        placeholder="First Name"
      />
      <input
        name="lastName"
        value={formData.lastName}
        onChange={handleChange}
        placeholder="Last Name"
      />
      <input
        name="email"
        type="email"
        value={formData.email}
        onChange={handleChange}
        placeholder="Email"
      />
      <input
        name="phone"
        type="tel"
        value={formData.phone}
        onChange={handleChange}
        placeholder="Phone"
      />
      <button type="submit">Submit</button>
    </form>
  );
};

export default ObjectStateExample;
```

## Form Handling

### Controlled Components

```jsx
import React, { useState } from 'react';

const ControlledForm = () => {
  const [formData, setFormData] = useState({
    username: '',
    password: '',
    confirmPassword: '',
    terms: false
  });
  
  const [errors, setErrors] = useState({});
  
  const handleChange = (e) => {
    const { name, value, type, checked } = e.target;
    setFormData({
      ...formData,
      [name]: type === 'checkbox' ? checked : value
    });
    
    // Clear error when user starts typing
    if (errors[name]) {
      setErrors({
        ...errors,
        [name]: ''
      });
    }
  };
  
  const validateForm = () => {
    const newErrors = {};
    
    if (!formData.username) {
      newErrors.username = 'Username is required';
    }
    
    if (!formData.password) {
      newErrors.password = 'Password is required';
    } else if (formData.password.length < 6) {
      newErrors.password = 'Password must be at least 6 characters';
    }
    
    if (formData.password !== formData.confirmPassword) {
      newErrors.confirmPassword = 'Passwords do not match';
    }
    
    if (!formData.terms) {
      newErrors.terms = 'You must accept the terms';
    }
    
    return newErrors;
  };
  
  const handleSubmit = (e) => {
    e.preventDefault();
    const newErrors = validateForm();
    
    if (Object.keys(newErrors).length === 0) {
      console.log('Form submitted:', formData);
      // Reset form
      setFormData({
        username: '',
        password: '',
        confirmPassword: '',
        terms: false
      });
    } else {
      setErrors(newErrors);
    }
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <h2>Registration Form</h2>
      
      <div>
        <input
          type="text"
          name="username"
          value={formData.username}
          onChange={handleChange}
          placeholder="Username"
        />
        {errors.username && <span className="error">{errors.username}</span>}
      </div>
      
      <div>
        <input
          type="password"
          name="password"
          value={formData.password}
          onChange={handleChange}
          placeholder="Password"
        />
        {errors.password && <span className="error">{errors.password}</span>}
      </div>
      
      <div>
        <input
          type="password"
          name="confirmPassword"
          value={formData.confirmPassword}
          onChange={handleChange}
          placeholder="Confirm Password"
        />
        {errors.confirmPassword && <span className="error">{errors.confirmPassword}</span>}
      </div>
      
      <div>
        <label>
          <input
            type="checkbox"
            name="terms"
            checked={formData.terms}
            onChange={handleChange}
          />
          I accept the terms and conditions
        </label>
        {errors.terms && <span className="error">{errors.terms}</span>}
      </div>
      
      <button type="submit">Register</button>
    </form>
  );
};

export default ControlledForm;
```

### Uncontrolled Components

```jsx
import React, { useRef } from 'react';

const UncontrolledForm = () => {
  const nameRef = useRef();
  const emailRef = useRef();
  const messageRef = useRef();
  
  const handleSubmit = (e) => {
    e.preventDefault();
    
    const formData = {
      name: nameRef.current.value,
      email: emailRef.current.value,
      message: messageRef.current.value
    };
    
    console.log('Form Data:', formData);
    
    // Reset form
    nameRef.current.value = '';
    emailRef.current.value = '';
    messageRef.current.value = '';
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <h2>Contact Form (Uncontrolled)</h2>
      
      <input
        ref={nameRef}
        type="text"
        placeholder="Name"
        required
      />
      
      <input
        ref={emailRef}
        type="email"
        placeholder="Email"
        required
      />
      
      <textarea
        ref={messageRef}
        placeholder="Message"
        rows="4"
        required
      />
      
      <button type="submit">Submit</button>
    </form>
  );
};

export default UncontrolledForm;
```

## API Integration

### Fetch Data from API

```jsx
import React, { useState, useEffect } from 'react';

const UserList = () => {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    fetch('https://jsonplaceholder.typicode.com/users')
      .then(response => response.json())
      .then(data => {
        setUsers(data);
        setLoading(false);
      })
      .catch(error => {
        setError(error.message);
        setLoading(false);
      });
  }, []);
  
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  
  return (
    <div>
      <h2>Users List</h2>
      <ul>
        {users.map(user => (
          <li key={user.id}>
            <h3>{user.name}</h3>
            <p>Email: {user.email}</p>
            <p>Phone: {user.phone}</p>
          </li>
        ))}
      </ul>
    </div>
  );
};

export default UserList;
```

### Post Data to API

```jsx
import React, { useState } from 'react';

const PostForm = () => {
  const [title, setTitle] = useState('');
  const [body, setBody] = useState('');
  const [loading, setLoading] = useState(false);
  const [message, setMessage] = useState('');
  
  const handleSubmit = async (e) => {
    e.preventDefault();
    setLoading(true);
    setMessage('');
    
    try {
      const response = await fetch('https://jsonplaceholder.typicode.com/posts', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          title,
          body,
          userId: 1
        })
      });
      
      if (response.ok) {
        const data = await response.json();
        setMessage('Post created successfully!');
        setTitle('');
        setBody('');
      } else {
        setMessage('Error creating post');
      }
    } catch (error) {
      setMessage('Error: ' + error.message);
    } finally {
      setLoading(false);
    }
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <h2>Create Post</h2>
      
      <input
        type="text"
        value={title}
        onChange={(e) => setTitle(e.target.value)}
        placeholder="Post Title"
        required
      />
      
      <textarea
        value={body}
        onChange={(e) => setBody(e.target.value)}
        placeholder="Post Body"
        rows="4"
        required
      />
      
      <button type="submit" disabled={loading}>
        {loading ? 'Creating...' : 'Create Post'}
      </button>
      
      {message && <p>{message}</p>}
    </form>
  );
};

export default PostForm;
```

## Advanced Patterns

### Custom Hook

```jsx
import { useState, useEffect } from 'react';

// Custom hook for API calls
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

// Component using the custom hook
const PostsList = () => {
  const { data: posts, loading, error } = useApi('https://jsonplaceholder.typicode.com/posts');
  
  if (loading) return <div>Loading posts...</div>;
  if (error) return <div>Error: {error}</div>;
  
  return (
    <div>
      <h2>Posts</h2>
      <ul>
        {posts?.slice(0, 10).map(post => (
          <li key={post.id}>
            <h3>{post.title}</h3>
            <p>{post.body}</p>
          </li>
        ))}
      </ul>
    </div>
  );
};

export default PostsList;
```

### Higher Order Component

```jsx
import React from 'react';

// HOC for adding loading functionality
const withLoading = (WrappedComponent) => {
  return function WithLoadingComponent({ isLoading, ...props }) {
    if (isLoading) {
      return <div>Loading...</div>;
    }
    return <WrappedComponent {...props} />;
  };
};

// Component to be wrapped
const UserProfile = ({ user }) => (
  <div>
    <h2>{user.name}</h2>
    <p>Email: {user.email}</p>
    <p>Phone: {user.phone}</p>
  </div>
);

// Wrapped component with loading functionality
const UserProfileWithLoading = withLoading(UserProfile);

// Usage
const App = () => {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    fetch('https://jsonplaceholder.typicode.com/users/1')
      .then(response => response.json())
      .then(data => {
        setUser(data);
        setLoading(false);
      });
  }, []);
  
  return <UserProfileWithLoading user={user} isLoading={loading} />;
};

export default App;
```

### Context API Example

```jsx
import React, { createContext, useContext, useState } from 'react';

// Create context
const ThemeContext = createContext();

// Provider component
const ThemeProvider = ({ children }) => {
  const [theme, setTheme] = useState('light');
  
  const toggleTheme = () => {
    setTheme(theme === 'light' ? 'dark' : 'light');
  };
  
  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};

// Custom hook to use context
const useTheme = () => {
  const context = useContext(ThemeContext);
  if (!context) {
    throw new Error('useTheme must be used within a ThemeProvider');
  }
  return context;
};

// Component using the context
const ThemedButton = () => {
  const { theme, toggleTheme } = useTheme();
  
  return (
    <button
      onClick={toggleTheme}
      style={{
        backgroundColor: theme === 'light' ? '#fff' : '#333',
        color: theme === 'light' ? '#333' : '#fff',
        border: '1px solid #ccc',
        padding: '10px 20px',
        cursor: 'pointer'
      }}
    >
      Switch to {theme === 'light' ? 'dark' : 'light'} theme
    </button>
  );
};

// App component
const App = () => {
  return (
    <ThemeProvider>
      <div>
        <h1>Theme Example</h1>
        <ThemedButton />
      </div>
    </ThemeProvider>
  );
};

export default App;
```

## Key Learning Points

1. **Basic React Concepts**: Components, JSX, props
2. **State Management**: useState hook, state updates
3. **Form Handling**: Controlled vs uncontrolled components
4. **API Integration**: Fetch data, POST requests
5. **Custom Hooks**: Reusable logic
6. **Higher Order Components**: Component composition
7. **Context API**: Global state management
8. **Error Handling**: Loading states, error boundaries
9. **Best Practices**: Clean code, performance optimization
10. **Real-world Patterns**: Common React patterns and solutions

## Practice Exercises

1. **Build a Calculator**: Create a calculator with basic operations
2. **Weather App**: Fetch weather data from an API
3. **Shopping Cart**: Implement add/remove items functionality
4. **Chat Application**: Real-time messaging interface
5. **Dashboard**: Create a dashboard with multiple widgets
6. **Authentication**: Login/logout functionality
7. **File Upload**: Handle file uploads with progress
8. **Search Functionality**: Implement search with filters
9. **Pagination**: Add pagination to lists
10. **Responsive Design**: Make components mobile-friendly

---

**Happy Coding! 🚀**

*Practice these examples to master React development.*
