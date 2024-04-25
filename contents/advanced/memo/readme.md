

# Testing the React.memo()

Sure, here's a step-by-step guide on how to prove that React.memo can prevent unnecessary re-rendering and improve performance:

## 0. Set up a new React project
Create a new React project using the following command:

```bash
npx create-react-app react-memo-example 
```
Navigate to the project directory:

```bash
cd react-memo-example
```

## 1. Create a simple React component

This component will be used to demonstrate the effect of React.memo. It should have a prop that changes over time, causing the component to re-render.

```jsx
// MyComponent.js

import React from 'react';

export default function MyComponent({ count }) {
    console.log('MyComponent rendered');
    return <div>{count}</div>;
}
```

## 2. Create a parent component

This component will contain MyComponent and a state variable that changes over time, causing MyComponent to re-render.

```jsx
// ParentComponent.js

import React, { useState } from "react";
import MyComponent from "./MyComponent";

export default function ParentComponent() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Increase Count</button>
      <MyComponent count={count} />
    </div>
  );
}
```

## 3. Add MyComponent to App.js

Open the `App.js` file and import the ParentComponent.

```jsx
import logo from './logo.svg';
import './App.css';
import ParentComponent from './ParentComponent';

function App() {
  return (
    <div className="App">
      <ParentComponent/>
    </div>
  );
}

export default App;

```

## 3. Observe the console

Every time you click the "Increase Count" button, you should see "MyComponent rendered" in the console. This is because MyComponent is re-rendering every time its props change.

> Tips: you can try to use profiler to see the re-rendering of the component.

## 4. Add React.memo to MyComponent

This will prevent MyComponent from re-rendering unless its props change.

```jsx
// MyComponent.js

import React from 'react';

export default React.memo(function MyComponent({ count }) {
    console.log('MyComponent rendered');
    return <div>{count}</div>;
});
```

## 5. Observe the console again

You will not see "MyComponent rendered" in the console every time you click the "Increase Count" button. This is because React.memo is preventing the unnecessary re-rendering of MyComponent.

> Tips: you can try to use profiler to see there is not re-rendering of the component.

## 6. Pass the count state in ParentComponent 

Update the ParentComponent to pass the count state as a prop to MyComponent.

```jsx
// ParentComponent.js

import React, { useState } from "react";
import MyComponent from "./MyComponent";

export default function ParentComponent() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Increase Count</button>
      <MyComponent count={count}/>
    </div>
  );
}
```

## 7. Observe the console again

Now, when you click the "Increase Count" button, you should see "MyComponent rendered" in the console only when the count actually changes. This demonstrates that React.memo is preventing unnecessary re-renders and potentially improving performance.

> Tips: you can try to use profiler to see there is not re-rendering of the component.