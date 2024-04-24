
# Lazy Loading

Lazy loading is a technique that defers loading non-essential resources at page load time. This can help improve the performance of your app by reducing the initial load time and the amount of data that needs to be downloaded.

## Step 

### 1. Setup 

Create a new React app using Create React App:

```bash
npx create-react-app lazy-loading-demo
cd lazy-loading-demo
```

### 2. Create a new component

Create a new file at `src/HeavyComponent.js`. This will be the component we'll load lazily.

```js
// src/HeavyComponent.js

import React from 'react';

const HeavyComponent = () => {
  return <h1>This is a heavy component</h1>;
};

export default HeavyComponent;
```

### 3. Implement lazy loading

Modify `src/App.js` to use `React.lazy()` to load `HeavyComponent`.

```js
// src/App.js

import React, { Suspense } from 'react';
const HeavyComponent = React.lazy(() => import('./HeavyComponent'));

function App() {
  return (
    <div className="App">
      <Suspense fallback={<div>Loading...</div>}>
        <HeavyComponent />
      </Suspense>
    </div>
  );
}

export default App;
```


### 4. Test Lazy load Functionality

Run the app in development mode:

```bash
npm start
```

Run the project using npm start and open http://localhost:3000 in your browser. You should see "Loading..." displayed briefly before "This is a heavy component" is displayed.
