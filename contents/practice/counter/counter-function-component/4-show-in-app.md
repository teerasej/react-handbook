
# แสดงทั้ง 2 component ไว้ใน App component


```jsx
// src/App.jsx

import { useState } from 'react'
import reactLogo from './assets/react.svg'
import viteLogo from '/vite.svg'
import './App.css'
import Counter from './components/Counter';
import IncreaseButton from './components/IncreaseButton';


function App() {
  return (
    <>
      <div>
        <Counter/>
        <IncreaseButton/>
      </div>
    </>
  );
}

export default App;
```