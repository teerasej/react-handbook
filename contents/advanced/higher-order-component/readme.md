
# Higher Order Component in React

A Higher Order Component (HOC) is a pattern in React that allows you to reuse component logic. It is a function that takes a component and returns a new component with additional props. HOCs are commonly used for cross-cutting concerns such as logging, authentication, and data fetching.

## Step

### 1. Setup 

Create a new React app using Create React App:

```bash
npx create-react-app hoc-logger
cd hoc-logger
```

### 2. Create a new file for HOC

Create a new file `src/withLogging.js` and add the following code:

```js
import React from 'react';

const withLogging = WrappedComponent => {
  return class extends React.Component {
    componentDidMount() {
      console.log('Props:', this.props);
    }

    render() {
      return <WrappedComponent {...this.props} />;
    }
  };
};

export default withLogging;
```

This is a Higher-Order Component that logs the props of the component it wraps.

### 3. Create a component to wrap

Create a new file `src/Hello.js` and add the following code:

```js
import React from 'react';

const Hello = props => <h1>Hello, {props.name}!</h1>;

export default Hello;
```

This is a simple functional component that displays a greeting.

### 4. Use the Higher-Order Component

In the src directory, create a new file called `App.js`. This is where we'll use our custom hook.

In `App.js`, use the useFetch hook to fetch data from an API and display it.

```js
import logo from './logo.svg';
import './App.css';
import Hello from './Hello';
import withLogging from './withLogging';



function App() {
 const LoggedHello = withLogging(Hello);
  return (
    <div className="App">
      <LoggedHello name="React" />
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

Open the browser console to see the logged props.

This exercise demonstrates how to create and use a Higher-Order Component in React. The withLogging HOC can be used to wrap any component, and it will log the props of the wrapped component when it mounts.


