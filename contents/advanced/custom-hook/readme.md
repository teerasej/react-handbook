
# Custom Hook

Custom hooks are a way to extract component logic into reusable functions. They are similar to JavaScript functions, but they can use React features.

## Step

### 1. Setup 

Create a new React app using Create React App:

```bash
npx create-react-app custom-hooks-demo
cd custom-hooks-demo
```

### 2. Create a new file for custom hook

In the src directory, create a new file called `useFetch.js`. This is where we'll define our custom hook.

Path: `src/useFetch.js`


### 3. Implement custom hook

In useFetch.js, implement the custom hook. This hook will take a URL as a parameter and fetch data from that URL.

```js
import { useState, useEffect } from 'react';

const useFetch = (url) => {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    const fetchData = async () => {
      const response = await fetch(url);
      const data = await response.json();
      setData(data);
      setLoading(false);
    };

    fetchData();
  }, [url]);

  return { data, loading };
};

export default useFetch;
```


### 4. Use the custom hook in a component

In the src directory, create a new file called `App.js`. This is where we'll use our custom hook.

In `App.js`, use the useFetch hook to fetch data from an API and display it.

```js
import logo from './logo.svg';
import './App.css';
import useFetch from './useFetch';

function App() {
  const { data, loading } = useFetch('https://jsonplaceholder.typicode.com/posts/1');

  return (
    <div>
      {loading ? 'Loading...' : (
        <div>
          <h1>{data.title}</h1>
          <p>{data.body}</p>
        </div>
      )}
    </div>
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
