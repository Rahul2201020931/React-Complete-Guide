# Chapter 7: Single Page Applications (Moving Between Pages) 🛣️

## What is SPA (Single Page Application)?

**SPA (Single Page Application)** is a web application that loads a single HTML page and dynamically updates that page as the user interacts with the application.

### Key Characteristics
- **Single HTML Page**: Only one HTML file is loaded
- **Dynamic Updates**: Content changes without full page reload
- **Client-Side Routing**: Navigation handled by JavaScript
- **Faster Navigation**: No server round-trips for page changes

### SPA vs Traditional Web Apps

| **SPA** | **Traditional Web App** |
|---------|-------------------------|
| Single HTML page | Multiple HTML pages |
| Client-side routing | Server-side routing |
| JavaScript handles navigation | Server handles navigation |
| Faster after initial load | Slower due to page reloads |
| Better user experience | More page refreshes |

## What is Routing?

**Routing** is the process of determining what content to display based on the current URL or path.

### Client-Side Routing
- **JavaScript handles navigation**
- **URL changes without page reload**
- **Content updates dynamically**
- **Browser history is maintained**

### Server-Side Routing
- **Server determines content**
- **Full page reload on navigation**
- **New HTML page for each route**
- **Traditional web application pattern**

## React Router

**React Router** is the standard routing library for React applications.

### Installation
```bash
npm install react-router-dom
```

### Basic Setup
```jsx
import { BrowserRouter, Routes, Route, Link } from 'react-router-dom';

const App = () => {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
        <Link to="/contact">Contact</Link>
      </nav>
      
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/contact" element={<Contact />} />
      </Routes>
    </BrowserRouter>
  );
};
```

## Route Parameters

### Path Parameters
```jsx
// Route definition
<Route path="/user/:id" element={<UserProfile />} />

// Component
const UserProfile = () => {
  const { id } = useParams();
  return <h1>User ID: {id}</h1>;
};
```

### Query Parameters
```jsx
const SearchResults = () => {
  const [searchParams] = useSearchParams();
  const query = searchParams.get('q');
  
  return <h1>Search results for: {query}</h1>;
};
```

## Navigation

### Programmatic Navigation
```jsx
import { useNavigate } from 'react-router-dom';

const LoginForm = () => {
  const navigate = useNavigate();
  
  const handleLogin = () => {
    // Login logic
    navigate('/dashboard');
  };
  
  return (
    <form onSubmit={handleLogin}>
      {/* Form fields */}
    </form>
  );
};
```

### Link Component
```jsx
import { Link } from 'react-router-dom';

const Navigation = () => (
  <nav>
    <Link to="/">Home</Link>
    <Link to="/about">About</Link>
    <Link to="/contact">Contact</Link>
  </nav>
);
```

## Nested Routes

```jsx
const App = () => (
  <BrowserRouter>
    <Routes>
      <Route path="/" element={<Layout />}>
        <Route index element={<Home />} />
        <Route path="about" element={<About />} />
        <Route path="contact" element={<Contact />} />
      </Route>
    </Routes>
  </BrowserRouter>
);

const Layout = () => (
  <div>
    <nav>Navigation</nav>
    <Outlet /> {/* Child routes render here */}
  </div>
);
```

## Protected Routes

```jsx
const ProtectedRoute = ({ children }) => {
  const isAuthenticated = useAuth();
  
  return isAuthenticated ? children : <Navigate to="/login" />;
};

// Usage
<Route 
  path="/dashboard" 
  element={
    <ProtectedRoute>
      <Dashboard />
    </ProtectedRoute>
  } 
/>
```

## Key Takeaways

1. **SPA** provides better user experience with dynamic updates
2. **Client-side routing** handles navigation without page reloads
3. **React Router** is the standard routing solution
4. **Route parameters** allow dynamic URLs
5. **Navigation** can be done with Link or programmatically
6. **Nested routes** create hierarchical navigation
7. **Protected routes** control access to certain pages

## Next Steps

- Learn about advanced React patterns
- Understand performance optimization
- Practice building complex applications

---

**Ready to get classy? Let's move to Chapter 8! 🏗️**
