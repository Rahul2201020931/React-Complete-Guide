# Chapter 12: Redux (The Global State Manager) 🏪

## useContext vs Redux

### useContext
- **Built into React**: No additional dependencies
- **Simple Setup**: Easy to implement
- **Good for Small Apps**: When state is minimal
- **No DevTools**: Limited debugging capabilities
- **No Middleware**: No advanced features

### Redux
- **External Library**: Requires installation
- **Complex Setup**: More configuration needed
- **Good for Large Apps**: Complex state management
- **Excellent DevTools**: Great debugging experience
- **Rich Middleware**: Thunk, Saga, etc.

### When to Use Which?

| **Use Context** | **Use Redux** |
|-----------------|---------------|
| Small applications | Large applications |
| Simple state | Complex state logic |
| Few components | Many components |
| No time-travel debugging | Need time-travel debugging |
| No middleware needed | Need middleware |

## Advantages of Redux Toolkit over Redux

### Redux Toolkit Advantages
- **Less Boilerplate**: Reduces code significantly
- **Built-in Best Practices**: Immer, DevTools, etc.
- **TypeScript Support**: Better type safety
- **Modern Redux**: Uses modern Redux patterns
- **Easier Learning**: Simpler API

### Redux vs Redux Toolkit
```javascript
// Redux (verbose)
const INCREMENT = 'INCREMENT';
const DECREMENT = 'DECREMENT';

const increment = () => ({ type: INCREMENT });
const decrement = () => ({ type: DECREMENT });

const counterReducer = (state = 0, action) => {
  switch (action.type) {
    case INCREMENT:
      return state + 1;
    case DECREMENT:
      return state - 1;
    default:
      return state;
  }
};

// Redux Toolkit (concise)
import { createSlice } from '@reduxjs/toolkit';

const counterSlice = createSlice({
  name: 'counter',
  initialState: 0,
  reducers: {
    increment: (state) => state + 1,
    decrement: (state) => state - 1,
  },
});
```

## Explain Dispatcher

**Dispatcher** is a function that sends actions to the Redux store to update the state.

### How Dispatcher Works
1. **Component calls dispatch**: `dispatch(increment())`
2. **Action is sent to store**: Store receives the action
3. **Reducer processes action**: State is updated
4. **Components re-render**: UI updates with new state

### Example
```jsx
import { useDispatch } from 'react-redux';

const Counter = () => {
  const dispatch = useDispatch();
  
  const handleIncrement = () => {
    dispatch(increment()); // Sends action to store
  };
  
  return <button onClick={handleIncrement}>Increment</button>;
};
```

## Explain Reducer

**Reducer** is a pure function that takes the current state and an action, and returns the new state.

### Reducer Rules
1. **Pure Function**: No side effects
2. **Immutable Updates**: Don't mutate state
3. **Same Input, Same Output**: Predictable behavior
4. **No API Calls**: Keep reducers pure

### Example
```javascript
const counterReducer = (state = 0, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1;
    case 'DECREMENT':
      return state - 1;
    case 'RESET':
      return 0;
    default:
      return state;
  }
};
```

## Explain Slice

**Slice** is a Redux Toolkit concept that combines reducers, actions, and selectors in one object.

### Slice Components
- **name**: Identifier for the slice
- **initialState**: Starting state
- **reducers**: Functions that handle actions
- **extraReducers**: Handle actions from other slices

### Example
```javascript
import { createSlice } from '@reduxjs/toolkit';

const counterSlice = createSlice({
  name: 'counter',
  initialState: {
    value: 0,
    loading: false
  },
  reducers: {
    increment: (state) => {
      state.value += 1; // Immer handles immutability
    },
    decrement: (state) => {
      state.value -= 1;
    },
    reset: (state) => {
      state.value = 0;
    },
    setLoading: (state, action) => {
      state.loading = action.payload;
    }
  }
});

export const { increment, decrement, reset, setLoading } = counterSlice.actions;
export default counterSlice.reducer;
```

## Explain Selector

**Selector** is a function that extracts specific data from the Redux store state.

### Basic Selector
```javascript
const selectCounter = (state) => state.counter.value;
const selectLoading = (state) => state.counter.loading;
```

### Memoized Selector
```javascript
import { createSelector } from '@reduxjs/toolkit';

const selectCounter = (state) => state.counter;

const selectCounterValue = createSelector(
  selectCounter,
  (counter) => counter.value
);

const selectIsEven = createSelector(
  selectCounterValue,
  (value) => value % 2 === 0
);
```

### Usage in Components
```jsx
import { useSelector } from 'react-redux';

const Counter = () => {
  const count = useSelector(selectCounterValue);
  const isEven = useSelector(selectIsEven);
  
  return (
    <div>
      <p>Count: {count}</p>
      <p>Is Even: {isEven ? 'Yes' : 'No'}</p>
    </div>
  );
};
```

## Explain createSlice and Configuration

### createSlice Configuration
```javascript
import { createSlice } from '@reduxjs/toolkit';

const userSlice = createSlice({
  name: 'user',                    // Slice name
  initialState: {                  // Initial state
    user: null,
    loading: false,
    error: null
  },
  reducers: {                     // Reducer functions
    setUser: (state, action) => {
      state.user = action.payload;
    },
    setLoading: (state, action) => {
      state.loading = action.payload;
    },
    setError: (state, action) => {
      state.error = action.payload;
    },
    clearUser: (state) => {
      state.user = null;
      state.error = null;
    }
  },
  extraReducers: (builder) => {   // Handle external actions
    builder
      .addCase(fetchUser.pending, (state) => {
        state.loading = true;
      })
      .addCase(fetchUser.fulfilled, (state, action) => {
        state.loading = false;
        state.user = action.payload;
      })
      .addCase(fetchUser.rejected, (state, action) => {
        state.loading = false;
        state.error = action.error.message;
      });
  }
});
```

## Complete Redux Toolkit Setup

### 1. Store Configuration
```javascript
// store/index.js
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from './slices/counterSlice';
import userReducer from './slices/userSlice';

export const store = configureStore({
  reducer: {
    counter: counterReducer,
    user: userReducer,
  },
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware({
      serializableCheck: {
        ignoredActions: ['persist/PERSIST'],
      },
    }),
});

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```

### 2. Provider Setup
```jsx
// App.js
import { Provider } from 'react-redux';
import { store } from './store';

const App = () => (
  <Provider store={store}>
    <Counter />
    <UserProfile />
  </Provider>
);
```

### 3. Slice Example
```javascript
// slices/counterSlice.js
import { createSlice } from '@reduxjs/toolkit';

const counterSlice = createSlice({
  name: 'counter',
  initialState: {
    value: 0,
    history: []
  },
  reducers: {
    increment: (state) => {
      state.value += 1;
      state.history.push('increment');
    },
    decrement: (state) => {
      state.value -= 1;
      state.history.push('decrement');
    },
    reset: (state) => {
      state.value = 0;
      state.history.push('reset');
    }
  }
});

export const { increment, decrement, reset } = counterSlice.actions;
export default counterSlice.reducer;
```

### 4. Component Usage
```jsx
// components/Counter.jsx
import { useSelector, useDispatch } from 'react-redux';
import { increment, decrement, reset } from '../store/slices/counterSlice';

const Counter = () => {
  const count = useSelector((state) => state.counter.value);
  const history = useSelector((state) => state.counter.history);
  const dispatch = useDispatch();
  
  return (
    <div>
      <h2>Count: {count}</h2>
      <button onClick={() => dispatch(increment())}>+</button>
      <button onClick={() => dispatch(decrement())}>-</button>
      <button onClick={() => dispatch(reset())}>Reset</button>
      <div>
        <h3>History:</h3>
        <ul>
          {history.map((action, index) => (
            <li key={index}>{action}</li>
          ))}
        </ul>
      </div>
    </div>
  );
};
```

## Async Actions with Redux Toolkit

### createAsyncThunk
```javascript
import { createAsyncThunk, createSlice } from '@reduxjs/toolkit';

export const fetchUser = createAsyncThunk(
  'user/fetchUser',
  async (userId, { rejectWithValue }) => {
    try {
      const response = await fetch(`/api/users/${userId}`);
      if (!response.ok) throw new Error('Failed to fetch user');
      return await response.json();
    } catch (error) {
      return rejectWithValue(error.message);
    }
  }
);

const userSlice = createSlice({
  name: 'user',
  initialState: {
    user: null,
    loading: false,
    error: null
  },
  reducers: {
    clearUser: (state) => {
      state.user = null;
      state.error = null;
    }
  },
  extraReducers: (builder) => {
    builder
      .addCase(fetchUser.pending, (state) => {
        state.loading = true;
        state.error = null;
      })
      .addCase(fetchUser.fulfilled, (state, action) => {
        state.loading = false;
        state.user = action.payload;
      })
      .addCase(fetchUser.rejected, (state, action) => {
        state.loading = false;
        state.error = action.payload;
      });
  }
});
```

## Redux DevTools

### Setup
```javascript
// store/index.js
export const store = configureStore({
  reducer: {
    counter: counterReducer,
    user: userReducer,
  },
  devTools: process.env.NODE_ENV !== 'production', // Enable in development
});
```

### Features
- **Time Travel**: Go back to previous states
- **Action Logging**: See all dispatched actions
- **State Inspection**: View current state
- **Diff View**: See what changed

## Best Practices

### 1. Normalize State
```javascript
// ❌ Bad - Nested state
const initialState = {
  users: [
    { id: 1, name: 'John', posts: [{ id: 1, title: 'Post 1' }] }
  ]
};

// ✅ Good - Normalized state
const initialState = {
  users: {
    byId: {
      1: { id: 1, name: 'John' }
    },
    allIds: [1]
  },
  posts: {
    byId: {
      1: { id: 1, title: 'Post 1', userId: 1 }
    },
    allIds: [1]
  }
};
```

### 2. Use Selectors
```javascript
// Create reusable selectors
const selectUserById = (state, userId) => state.users.byId[userId];
const selectUserPosts = (state, userId) => 
  state.posts.allIds
    .map(id => state.posts.byId[id])
    .filter(post => post.userId === userId);
```

### 3. Keep Reducers Pure
```javascript
// ❌ Bad - Side effects in reducer
const userReducer = (state, action) => {
  if (action.type === 'SET_USER') {
    localStorage.setItem('user', JSON.stringify(action.payload));
    return { ...state, user: action.payload };
  }
  return state;
};

// ✅ Good - Side effects in middleware
const userMiddleware = (store) => (next) => (action) => {
  if (action.type === 'SET_USER') {
    localStorage.setItem('user', JSON.stringify(action.payload));
  }
  return next(action);
};
```

## Key Takeaways

1. **Redux Toolkit** reduces boilerplate compared to Redux
2. **Dispatcher** sends actions to the store
3. **Reducer** processes actions and updates state
4. **Slice** combines reducers, actions, and selectors
5. **Selector** extracts data from state
6. **createSlice** simplifies Redux setup
7. **Async actions** use createAsyncThunk
8. **DevTools** provide excellent debugging
9. **Best practices** improve code quality
10. **Normalize state** for better performance

## Next Steps

- Learn about testing strategies
- Understand performance optimization
- Practice building complex applications

---

**Ready to test? Let's move to Chapter 13! 🧪**
