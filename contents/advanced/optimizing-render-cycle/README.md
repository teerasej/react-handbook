

# Optimizing the render cycle with useCallback and useEffect

## 1. Set up a new React project
Create a new React project using the following command:

```bash
npx create-react-app optimizing-render-cycle-example
```
Navigate to the project directory:

```bash
cd optimizing-render-cycle-example
```

## 2. Create a simple React component

This component will be used to demonstrate the effect of useCallback and useEffect. It should have a prop that changes over time, causing the component to re-render.

```jsx
// Counter.js

import React, { useState, useEffect } from 'react';

export default function Counter() {
  const [count, setCount] = useState(0);

  const increment = () => {
    setCount(count + 1);
  };

  useEffect(() => {
    console.log('Component re-rendered!');
  });

  return (
    <div>
      <p>{count}</p>
      <button onClick={increment}>Increment</button>
    </div>
  );
}
```

In the above code, the increment function is recreated every time the component re-renders, causing unnecessary re-renders of the Button component. Also, the useEffect hook runs on every render, even when the count state doesn't change.

## 3. Add the Counter component to App.js

Open the `App.js` file and import the Counter component.

```jsx
import logo from './logo.svg';
import './App.css';
import Counter from './Counter';

function App() {
  return (
    <div className="App">
      <Counter/>
    </div>
  );
}

export default App;
```

## 4. Run the App

Start the application using the following command:

```bash
npm start
```

## 5. Observe the console

Every time you click the "Increment" button, you should see "Component re-rendered!" in the console. 

> Tips: you can try to use the React DevTools Profiler to see the re-rendering of the component.

## 6. Optimize with useCallback and useEffect

We can optimize the Counter component by using the useCallback and useEffect hooks. 

```jsx
// Counter.js

import React, { useState, useEffect, useCallback } from 'react';

export default function Counter() {
  const [count, setCount] = useState(0);

  const increment = useCallback(() => {
    setCount(count + 1);
  }, []);

  useEffect(() => {
    console.log('Component re-rendered!');
  }, []);

  return (
    <div>
      <p>{count}</p>
      <button onClick={increment}>Increment</button>
    </div>
  );
}
```

In the optimized version, the increment function is only recreated when the count state changes, reducing unnecessary re-renders. The useEffect hook also only runs when the count state changes.

## 7. Observe the console again

1. After making these changes, you should see that "Component re-rendered!" is only logged one time only. because the increment function is only recreated when the count state changes, reducing unnecessary re-renders.
2. for `useCallback` hook, it will return a memoized version of the callback that only changes if one of the dependencies has changed. This is useful when passing callbacks to optimized child components that rely on reference equality to prevent unnecessary renders.

3. The code below will make useCallback memorize the increment function in the time `count` is 0 (initial value). So the counter will stop at 1, everytime you click the button.

```jsx
const increment = useCallback(() => {
    setCount(count + 1);
  }, []);
```

4. The code below will make useCallback memorize the increment function with the `count` as a dependency. So the counter will increase everytime you click the button.

```jsx
const increment = useCallback(() => {
    setCount(count + 1);
  }, [count]);
```

5. same with `useEffect` hook, the code below will make useEffect run only once when the component is mounted.

```jsx
useEffect(() => {
    console.log('Component re-rendered!');
  }, []);
  ```

1. but for the code below will make useEffect run everytime the `count` changes.

```jsx
useEffect(() => {
    console.log('Component re-rendered!');
  }, [count]);
  ```