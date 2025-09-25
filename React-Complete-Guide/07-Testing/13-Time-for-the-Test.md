# Chapter 13: Testing Your React App (Making Sure Everything Works) 🧪

## What are Different Types of Testing?

### 1. Unit Testing
**Unit Testing** tests individual components or functions in isolation.

```jsx
// Component to test
const Button = ({ onClick, children }) => (
  <button onClick={onClick}>{children}</button>
);

// Unit test
import { render, fireEvent } from '@testing-library/react';

test('calls onClick when clicked', () => {
  const handleClick = jest.fn();
  render(<Button onClick={handleClick}>Click me</Button>);
  
  fireEvent.click(screen.getByText('Click me'));
  expect(handleClick).toHaveBeenCalledTimes(1);
});
```

### 2. Integration Testing
**Integration Testing** tests how multiple components work together.

```jsx
// Integration test
test('user can add todo item', () => {
  render(<TodoApp />);
  
  const input = screen.getByPlaceholderText('Add todo');
  const addButton = screen.getByText('Add');
  
  fireEvent.change(input, { target: { value: 'New todo' } });
  fireEvent.click(addButton);
  
  expect(screen.getByText('New todo')).toBeInTheDocument();
});
```

### 3. End-to-End (E2E) Testing
**E2E Testing** tests the entire application flow from user perspective.

```javascript
// E2E test with Cypress
describe('Todo App', () => {
  it('should add and complete todo', () => {
    cy.visit('/');
    cy.get('[data-testid="todo-input"]').type('New todo');
    cy.get('[data-testid="add-button"]').click();
    cy.get('[data-testid="todo-item"]').should('contain', 'New todo');
    cy.get('[data-testid="complete-button"]').click();
    cy.get('[data-testid="todo-item"]').should('have.class', 'completed');
  });
});
```

### 4. Visual Testing
**Visual Testing** ensures UI components look correct.

```javascript
// Visual regression test
test('button renders correctly', () => {
  const { container } = render(<Button>Click me</Button>);
  expect(container.firstChild).toMatchSnapshot();
});
```

## What is Enzyme?

**Enzyme** is a JavaScript testing utility for React that makes it easier to test React components' output.

### Enzyme Features
- **Shallow Rendering**: Test components in isolation
- **Full DOM Rendering**: Test with real DOM
- **Static Rendering**: Test component output
- **jQuery-like API**: Easy to use selectors

### Enzyme Example
```jsx
import { shallow } from 'enzyme';

const Button = ({ onClick, children }) => (
  <button onClick={onClick}>{children}</button>
);

test('renders button with text', () => {
  const wrapper = shallow(<Button>Click me</Button>);
  expect(wrapper.find('button').text()).toBe('Click me');
});

test('calls onClick when clicked', () => {
  const handleClick = jest.fn();
  const wrapper = shallow(<Button onClick={handleClick}>Click me</Button>);
  
  wrapper.find('button').simulate('click');
  expect(handleClick).toHaveBeenCalled();
});
```

## Enzyme vs React Testing Library

### Enzyme Approach
```jsx
// Enzyme - Testing implementation details
test('increments counter', () => {
  const wrapper = shallow(<Counter />);
  expect(wrapper.state('count')).toBe(0);
  
  wrapper.find('button').simulate('click');
  expect(wrapper.state('count')).toBe(1);
});
```

### React Testing Library Approach
```jsx
// React Testing Library - Testing user behavior
test('increments counter', () => {
  render(<Counter />);
  expect(screen.getByText('Count: 0')).toBeInTheDocument();
  
  fireEvent.click(screen.getByText('Increment'));
  expect(screen.getByText('Count: 1')).toBeInTheDocument();
});
```

### Comparison

| **Enzyme** | **React Testing Library** |
|------------|---------------------------|
| Tests implementation | Tests user behavior |
| Access to component state | No access to state |
| jQuery-like API | DOM queries |
| More complex setup | Simpler setup |
| Legacy approach | Modern approach |

## What is Jest and Why Do We Use It?

**Jest** is a JavaScript testing framework created by Facebook, designed to work out of the box with React applications.

### Why Jest?
- **Zero Configuration**: Works out of the box
- **Fast**: Parallel test execution
- **Snapshot Testing**: Visual regression testing
- **Mocking**: Built-in mocking capabilities
- **Coverage**: Built-in code coverage
- **Watch Mode**: Automatic re-running of tests

### Jest Features
```javascript
// Test structure
describe('Button Component', () => {
  test('renders correctly', () => {
    render(<Button>Click me</Button>);
    expect(screen.getByText('Click me')).toBeInTheDocument();
  });
  
  test('calls onClick handler', () => {
    const handleClick = jest.fn();
    render(<Button onClick={handleClick}>Click me</Button>);
    
    fireEvent.click(screen.getByText('Click me'));
    expect(handleClick).toHaveBeenCalledTimes(1);
  });
});
```

## Setting Up Testing Environment

### 1. Install Dependencies
```bash
npm install --save-dev @testing-library/react @testing-library/jest-dom @testing-library/user-event
```

### 2. Setup Test Environment
```javascript
// setupTests.js
import '@testing-library/jest-dom';
```

### 3. Jest Configuration
```javascript
// jest.config.js
module.exports = {
  testEnvironment: 'jsdom',
  setupFilesAfterEnv: ['<rootDir>/setupTests.js'],
  moduleNameMapping: {
    '\\.(css|less|scss|sass)$': 'identity-obj-proxy',
  },
};
```

## Testing React Components

### 1. Basic Component Testing
```jsx
import { render, screen } from '@testing-library/react';
import Button from './Button';

test('renders button with text', () => {
  render(<Button>Click me</Button>);
  expect(screen.getByText('Click me')).toBeInTheDocument();
});
```

### 2. Testing Props
```jsx
test('renders button with custom className', () => {
  render(<Button className="custom-class">Click me</Button>);
  expect(screen.getByRole('button')).toHaveClass('custom-class');
});
```

### 3. Testing Events
```jsx
import { fireEvent } from '@testing-library/react';

test('calls onClick when clicked', () => {
  const handleClick = jest.fn();
  render(<Button onClick={handleClick}>Click me</Button>);
  
  fireEvent.click(screen.getByText('Click me'));
  expect(handleClick).toHaveBeenCalledTimes(1);
});
```

### 4. Testing State Changes
```jsx
test('toggles visibility when clicked', () => {
  render(<ToggleComponent />);
  
  expect(screen.queryByText('Hidden content')).not.toBeInTheDocument();
  
  fireEvent.click(screen.getByText('Toggle'));
  expect(screen.getByText('Hidden content')).toBeInTheDocument();
});
```

## Testing Hooks

### 1. Testing useState
```jsx
import { renderHook, act } from '@testing-library/react';
import { useCounter } from './useCounter';

test('should increment counter', () => {
  const { result } = renderHook(() => useCounter(0));
  
  act(() => {
    result.current.increment();
  });
  
  expect(result.current.count).toBe(1);
});
```

### 2. Testing useEffect
```jsx
test('should fetch data on mount', async () => {
  const mockFetch = jest.fn().mockResolvedValue({ data: 'test' });
  
  render(<DataComponent fetchData={mockFetch} />);
  
  await waitFor(() => {
    expect(mockFetch).toHaveBeenCalled();
  });
});
```

## Testing Async Operations

### 1. Testing API Calls
```jsx
import { waitFor } from '@testing-library/react';

test('displays user data after fetch', async () => {
  const mockUser = { name: 'John', email: 'john@example.com' };
  jest.spyOn(global, 'fetch').mockResolvedValue({
    json: () => Promise.resolve(mockUser),
  });
  
  render(<UserProfile userId="1" />);
  
  await waitFor(() => {
    expect(screen.getByText('John')).toBeInTheDocument();
  });
});
```

### 2. Testing Loading States
```jsx
test('shows loading spinner while fetching', () => {
  render(<UserProfile userId="1" />);
  expect(screen.getByText('Loading...')).toBeInTheDocument();
});
```

## Testing Forms

### 1. Testing Form Input
```jsx
import userEvent from '@testing-library/user-event';

test('updates input value', async () => {
  const user = userEvent.setup();
  render(<ContactForm />);
  
  const input = screen.getByLabelText('Name');
  await user.type(input, 'John Doe');
  
  expect(input).toHaveValue('John Doe');
});
```

### 2. Testing Form Submission
```jsx
test('submits form with correct data', async () => {
  const handleSubmit = jest.fn();
  const user = userEvent.setup();
  
  render(<ContactForm onSubmit={handleSubmit} />);
  
  await user.type(screen.getByLabelText('Name'), 'John Doe');
  await user.type(screen.getByLabelText('Email'), 'john@example.com');
  await user.click(screen.getByRole('button', { name: 'Submit' }));
  
  expect(handleSubmit).toHaveBeenCalledWith({
    name: 'John Doe',
    email: 'john@example.com',
  });
});
```

## Testing Context

### 1. Testing Context Provider
```jsx
import { render } from '@testing-library/react';
import { ThemeProvider } from './ThemeContext';

const renderWithTheme = (component) => {
  return render(
    <ThemeProvider>
      {component}
    </ThemeProvider>
  );
};

test('uses theme from context', () => {
  renderWithTheme(<ThemedComponent />);
  expect(screen.getByText('Dark Theme')).toBeInTheDocument();
});
```

### 2. Testing Custom Hooks
```jsx
import { renderHook } from '@testing-library/react';
import { useTheme } from './useTheme';

test('returns theme from context', () => {
  const wrapper = ({ children }) => (
    <ThemeProvider>{children}</ThemeProvider>
  );
  
  const { result } = renderHook(() => useTheme(), { wrapper });
  expect(result.current.theme).toBe('dark');
});
```

## Snapshot Testing

### 1. Component Snapshots
```jsx
test('renders correctly', () => {
  const { container } = render(<Button>Click me</Button>);
  expect(container.firstChild).toMatchSnapshot();
});
```

### 2. Updating Snapshots
```bash
# Update snapshots
npm test -- --updateSnapshot
```

## Testing Best Practices

### 1. Test User Behavior
```jsx
// ❌ Bad - Testing implementation
test('sets state correctly', () => {
  const wrapper = shallow(<Counter />);
  wrapper.find('button').simulate('click');
  expect(wrapper.state('count')).toBe(1);
});

// ✅ Good - Testing user behavior
test('increments counter when clicked', () => {
  render(<Counter />);
  fireEvent.click(screen.getByText('Increment'));
  expect(screen.getByText('Count: 1')).toBeInTheDocument();
});
```

### 2. Use Semantic Queries
```jsx
// ❌ Bad - Fragile selectors
screen.getByTestId('submit-button');

// ✅ Good - Semantic queries
screen.getByRole('button', { name: 'Submit' });
```

### 3. Test Accessibility
```jsx
test('has proper accessibility attributes', () => {
  render(<Button aria-label="Close dialog">×</Button>);
  expect(screen.getByLabelText('Close dialog')).toBeInTheDocument();
});
```

### 4. Clean Up After Tests
```jsx
afterEach(() => {
  cleanup();
});
```

## Common Testing Patterns

### 1. Mock Functions
```jsx
const mockOnClick = jest.fn();
render(<Button onClick={mockOnClick}>Click me</Button>);
```

### 2. Mock Modules
```jsx
jest.mock('./api', () => ({
  fetchUser: jest.fn(),
}));
```

### 3. Mock Environment Variables
```jsx
beforeEach(() => {
  process.env.REACT_APP_API_URL = 'http://localhost:3000';
});
```

## Key Takeaways

1. **Unit Testing** tests individual components
2. **Integration Testing** tests component interactions
3. **E2E Testing** tests complete user flows
4. **Enzyme** is legacy, **React Testing Library** is modern
5. **Jest** is the standard testing framework
6. **Test user behavior**, not implementation
7. **Use semantic queries** for better tests
8. **Mock external dependencies** appropriately
9. **Snapshot testing** catches visual regressions
10. **Accessibility testing** ensures inclusive design

## Next Steps

- Practice writing tests for your components
- Learn about testing strategies
- Explore advanced testing patterns
- Build confidence in your code quality

---

**Congratulations! You've completed the React Complete Guide! 🎉**

**You now have a comprehensive understanding of React development from basics to advanced concepts! 🚀**
