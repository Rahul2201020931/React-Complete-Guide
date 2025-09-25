# Chapter 10: CSS in React (Making Your App Look Beautiful) 🎨

## Ways of Writing CSS in React

### 1. Inline CSS
```jsx
const Button = () => (
  <button style={{
    backgroundColor: 'blue',
    color: 'white',
    padding: '10px 20px',
    border: 'none',
    borderRadius: '4px'
  }}>
    Click me
  </button>
);
```

### 2. External CSS Files
```css
/* styles.css */
.button {
  background-color: blue;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
}
```

```jsx
import './styles.css';

const Button = () => (
  <button className="button">Click me</button>
);
```

### 3. CSS Modules
```css
/* Button.module.css */
.button {
  background-color: blue;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
}
```

```jsx
import styles from './Button.module.css';

const Button = () => (
  <button className={styles.button}>Click me</button>
);
```

### 4. Styled Components
```jsx
import styled from 'styled-components';

const StyledButton = styled.button`
  background-color: blue;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
`;

const Button = () => <StyledButton>Click me</StyledButton>;
```

### 5. CSS-in-JS Libraries
```jsx
import { makeStyles } from '@material-ui/core/styles';

const useStyles = makeStyles({
  button: {
    backgroundColor: 'blue',
    color: 'white',
    padding: '10px 20px',
    border: 'none',
    borderRadius: '4px'
  }
});

const Button = () => {
  const classes = useStyles();
  return <button className={classes.button}>Click me</button>;
};
```

## How to Configure Tailwind CSS

### Installation
```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

### Configuration
```javascript
// tailwind.config.js
module.exports = {
  content: [
    "./src/**/*.{js,jsx,ts,tsx}",
  ],
  theme: {
    extend: {
      colors: {
        primary: '#3B82F6',
        secondary: '#6B7280',
      },
      fontFamily: {
        sans: ['Inter', 'sans-serif'],
      },
    },
  },
  plugins: [],
}
```

### CSS File
```css
/* src/index.css */
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer components {
  .btn-primary {
    @apply bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600;
  }
}
```

### Usage
```jsx
const Button = () => (
  <button className="btn-primary">
    Click me
  </button>
);
```

## Understanding tailwind.config.js Keys

### content
```javascript
content: [
  "./src/**/*.{js,jsx,ts,tsx}", // Scan these files for classes
  "./public/index.html"
]
```
**Purpose**: Tells Tailwind which files to scan for class names to include in the final CSS.

### theme
```javascript
theme: {
  colors: {
    primary: '#3B82F6',
    secondary: '#6B7280',
  },
  spacing: {
    '72': '18rem',
    '84': '21rem',
  },
  fontFamily: {
    sans: ['Inter', 'sans-serif'],
  }
}
```
**Purpose**: Defines the design system - colors, spacing, fonts, etc.

### extend
```javascript
theme: {
  extend: {
    colors: {
      brand: '#FF6B6B',
    }
  }
}
```
**Purpose**: Extends the default theme without overriding it completely.

### plugins
```javascript
plugins: [
  require('@tailwindcss/forms'),
  require('@tailwindcss/typography'),
]
```
**Purpose**: Adds additional functionality and components.

## Why Do We Have .postcssrc File?

### .postcssrc.js
```javascript
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
}
```

### Purpose
- **PostCSS**: CSS processor that transforms CSS with JavaScript
- **Tailwind**: Processes Tailwind directives (@tailwind base, etc.)
- **Autoprefixer**: Automatically adds vendor prefixes
- **Build Integration**: Works with webpack, Vite, etc.

## CSS-in-JS Solutions

### 1. Styled Components
```jsx
import styled from 'styled-components';

const Container = styled.div`
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
`;

const Button = styled.button`
  background-color: ${props => props.primary ? 'blue' : 'gray'};
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  
  &:hover {
    opacity: 0.8;
  }
`;

const App = () => (
  <Container>
    <Button primary>Primary Button</Button>
    <Button>Secondary Button</Button>
  </Container>
);
```

### 2. Emotion
```jsx
import { css } from '@emotion/react';

const buttonStyle = css`
  background-color: blue;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  
  &:hover {
    opacity: 0.8;
  }
`;

const Button = () => (
  <button css={buttonStyle}>Click me</button>
);
```

### 3. JSS
```jsx
import { createUseStyles } from 'react-jss';

const useStyles = createUseStyles({
  button: {
    backgroundColor: 'blue',
    color: 'white',
    padding: '10px 20px',
    border: 'none',
    borderRadius: '4px',
    '&:hover': {
      opacity: 0.8,
    }
  }
});

const Button = () => {
  const classes = useStyles();
  return <button className={classes.button}>Click me</button>;
};
```

## Component Styling Patterns

### 1. Conditional Styling
```jsx
const Button = ({ variant, size, disabled }) => {
  const baseClasses = 'px-4 py-2 rounded font-medium';
  
  const variantClasses = {
    primary: 'bg-blue-500 text-white hover:bg-blue-600',
    secondary: 'bg-gray-500 text-white hover:bg-gray-600',
    outline: 'border border-blue-500 text-blue-500 hover:bg-blue-50'
  };
  
  const sizeClasses = {
    sm: 'px-2 py-1 text-sm',
    md: 'px-4 py-2',
    lg: 'px-6 py-3 text-lg'
  };
  
  const disabledClasses = disabled ? 'opacity-50 cursor-not-allowed' : '';
  
  return (
    <button 
      className={`${baseClasses} ${variantClasses[variant]} ${sizeClasses[size]} ${disabledClasses}`}
      disabled={disabled}
    >
      Click me
    </button>
  );
};
```

### 2. CSS Variables
```css
:root {
  --primary-color: #3B82F6;
  --secondary-color: #6B7280;
  --border-radius: 4px;
  --spacing: 1rem;
}

.button {
  background-color: var(--primary-color);
  border-radius: var(--border-radius);
  padding: var(--spacing);
}
```

### 3. Theme Provider
```jsx
import { ThemeProvider } from 'styled-components';

const theme = {
  colors: {
    primary: '#3B82F6',
    secondary: '#6B7280',
    success: '#10B981',
    error: '#EF4444',
  },
  spacing: {
    xs: '0.25rem',
    sm: '0.5rem',
    md: '1rem',
    lg: '1.5rem',
    xl: '2rem',
  }
};

const App = () => (
  <ThemeProvider theme={theme}>
    <MyComponent />
  </ThemeProvider>
);
```

## Responsive Design

### Tailwind Responsive Classes
```jsx
const ResponsiveCard = () => (
  <div className="
    w-full
    sm:w-1/2
    md:w-1/3
    lg:w-1/4
    xl:w-1/5
    p-4
    m-2
    bg-white
    rounded-lg
    shadow-md
  ">
    <h3 className="text-lg font-semibold mb-2">Card Title</h3>
    <p className="text-gray-600">Card content</p>
  </div>
);
```

### CSS Media Queries
```css
.card {
  width: 100%;
  padding: 1rem;
  margin: 0.5rem;
}

@media (min-width: 640px) {
  .card {
    width: 50%;
  }
}

@media (min-width: 768px) {
  .card {
    width: 33.333%;
  }
}

@media (min-width: 1024px) {
  .card {
    width: 25%;
  }
}
```

## Animation and Transitions

### CSS Transitions
```css
.button {
  transition: all 0.3s ease;
}

.button:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}
```

### Framer Motion
```jsx
import { motion } from 'framer-motion';

const AnimatedButton = () => (
  <motion.button
    whileHover={{ scale: 1.05 }}
    whileTap={{ scale: 0.95 }}
    className="bg-blue-500 text-white px-4 py-2 rounded"
  >
    Click me
  </motion.button>
);
```

### CSS Animations
```css
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

.fade-in {
  animation: fadeIn 0.5s ease-out;
}
```

## Best Practices

### 1. Use CSS Modules for Scoped Styles
```jsx
// Button.module.css
.button {
  background-color: blue;
  color: white;
}

// Button.jsx
import styles from './Button.module.css';
const Button = () => <button className={styles.button}>Click me</button>;
```

### 2. Create Reusable Style Utilities
```jsx
// utils/styles.js
export const buttonVariants = {
  primary: 'bg-blue-500 text-white hover:bg-blue-600',
  secondary: 'bg-gray-500 text-white hover:bg-gray-600',
  outline: 'border border-blue-500 text-blue-500 hover:bg-blue-50'
};

export const spacing = {
  sm: 'p-2',
  md: 'p-4',
  lg: 'p-6'
};
```

### 3. Use CSS Custom Properties
```css
:root {
  --color-primary: #3B82F6;
  --color-secondary: #6B7280;
  --spacing-unit: 0.25rem;
}

.button {
  background-color: var(--color-primary);
  padding: calc(var(--spacing-unit) * 4);
}
```

### 4. Organize Styles by Component
```
src/
├── components/
│   ├── Button/
│   │   ├── Button.jsx
│   │   ├── Button.module.css
│   │   └── Button.test.js
│   └── Card/
│       ├── Card.jsx
│       ├── Card.module.css
│       └── Card.test.js
```

## Common Styling Mistakes

### 1. Inline Styles for Complex Styling
```jsx
// ❌ Bad - Complex inline styles
const Button = () => (
  <button style={{
    backgroundColor: 'blue',
    color: 'white',
    padding: '10px 20px',
    border: 'none',
    borderRadius: '4px',
    fontSize: '16px',
    fontWeight: '500',
    cursor: 'pointer',
    transition: 'all 0.3s ease'
  }}>
    Click me
  </button>
);

// ✅ Good - External CSS
const Button = () => (
  <button className="btn-primary">Click me</button>
);
```

### 2. Not Using CSS Reset
```css
/* Reset CSS */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
```

### 3. Hardcoded Values
```jsx
// ❌ Bad - Hardcoded values
<div style={{ width: '300px', height: '200px' }}>

// ✅ Good - Responsive classes
<div className="w-full h-48 md:w-80 md:h-52">
```

## Key Takeaways

1. **Multiple CSS approaches** available for React
2. **Tailwind CSS** provides utility-first styling
3. **CSS Modules** offer scoped styling
4. **Styled Components** enable CSS-in-JS
5. **PostCSS** processes CSS with plugins
6. **Responsive design** is essential for modern apps
7. **Animations** enhance user experience
8. **Best practices** improve maintainability
9. **Component organization** keeps styles manageable
10. **Avoid common mistakes** for better code quality

## Next Steps

- Learn about state management solutions
- Understand data flow patterns
- Practice building styled applications

---

**Ready to manage data? Let's move to Chapter 11! 📊**
