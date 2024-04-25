
# Using useState, useReducer, and useContext hooks in React

In this article, we will learn how to use the `useState`, `useReducer`, and `useContext` hooks in React. We will create a simple counter application to demonstrate the usage of these hooks.

## Step

### 1. Setup 

Create a new React app using Create React App:

```bash
npx create-react-app counter-app-hooks
cd counter-app-hooks
```

### 2. Create a context

Navigate to the src directory and create a new file CounterContext.js. This file will hold our context.

```jsx
// src/CounterContext.js
import React from 'react';

const CounterContext = React.createContext();
export default CounterContext;
```


### 3. Create a Reducer

Create a new file counterReducer.js in the src directory. This file will hold our reducer function.

```jsx
// src/counterReducer.js
export const counterReducer = (state, action) => {
    switch (action.type) {
        case 'INCREMENT':
            return state + 1;
        case 'DECREMENT':
            return state - 1;
        default:
            return state;
    }
};
```


### 4. Create a Provider Component

In the src directory, create a new file CounterProvider.js. This component will use the useReducer hook to manage state and provide it to child components through context.

```jsx
// src/CounterProvider.js
import React, { useReducer } from 'react';
import CounterContext from './CounterContext';
import { counterReducer } from './counterReducer';

const CounterProvider = ({ children }) => {
    const [state, dispatch] = useReducer(counterReducer, 0);

    return (
        <CounterContext.Provider value={{ state, dispatch }}>
            {children}
        </CounterContext.Provider>
    );
};

export default CounterProvider;
```
### 5. Create the Counter Component

Create a new file Counter.js in the src directory. This component will use the useContext hook to access the state and dispatch function provided by the CounterProvider.

```jsx
// src/Counter.js
import React, { useContext } from 'react';
import CounterContext from './CounterContext';

const Counter = () => {
    const { state, dispatch } = useContext(CounterContext);

    return (
        <div>
            <p>Count: {state}</p>
            <button onClick={() => dispatch({ type: 'INCREMENT' })}>Increment</button>
            <button onClick={() => dispatch({ type: 'DECREMENT' })}>Decrement</button>
        </div>
    );
};

export default Counter;
```

### 6. Update the App Component

Update the App component in the src/App.js file to use the CounterProvider and Counter components.

```jsx
import logo from './logo.svg';
import './App.css';
import CounterProvider from './CounterProvider';
import Counter from './Counter';

function App() {
  return (
    <CounterProvider>
        <Counter />
    </CounterProvider>
  );
}

export default App;

```


### 5. Run the app

Run the app in development mode:

```bash
npm start
```

Run the project using npm start and open http://localhost:3000 in your browser. 

