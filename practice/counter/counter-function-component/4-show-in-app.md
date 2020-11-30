
# แสดงทั้ง 2 component ไว้ใน App component


```jsx
// src/App.js

import React from 'react';
import logo from './logo.svg';
import './App.css';
import Counter from './components/Counter';
import IncreaseButton from './components/IncreaseButton';


function App() {
  return (
   
      <div className="App">
        <Counter/>
        <IncreaseButton/>
      </div>
   
  );
}

export default App;
```