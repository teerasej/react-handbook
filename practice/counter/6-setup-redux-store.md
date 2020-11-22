

# Setup Redux store ให้ App component


```jsx
// src/App.js 

import React from 'react';
import logo from './logo.svg';
import './App.css';
import Counter from './components/Counter';
import AddNumber from './components/AddNumber';

// Provider component จาก redux 
import { Provider } from "react-redux";

// import function มาจาก store.js
import configureStore from "./redux/store";

// สร้าง object ของ store
const store = configureStore();

function App() {
  return (
    {/* กำหนด store object ให้ Provider ที่ครอบ component ทุกตัว */}
    <Provider store={store}>
      <div className="App">
        <Counter/>
        <AddNumber/>
      </div>
    </Provider>
  );
}

export default App;
```