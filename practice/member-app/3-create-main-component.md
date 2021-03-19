

# สร้าง Main component

- สำหรับแสดงตัวโหลดก่อนเข้าแอพ 
- สำหรับแสดงหน้าสมาชิกที่ login เข้ามา 

## 1. สร้างไฟล์ 

```jsx
// src/components/Main.js
// rfc snippet
import React from 'react'

export default function Main() {

    return (
        <div>Home</div>
    );

}

```

## 2. นำ Main component มาแสดงใน App

```jsx
// src/App.js

import logo from './logo.svg';
import './App.css';
// import main component
import Main from './components/Main';

function App() {
  return (
    <div>
     {/* แสดง Main component */}
      <Main></Main>
    </div>
  );
}

export default App;

```