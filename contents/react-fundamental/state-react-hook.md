
# Control component's state with React Hook

ในที่นี้เราเราสามารถสร้าง **ตัวแปร และ function ที่ไว้ทำงานกับ State** ของ function component ได้ ผ่าน React Hook ชื่อ `useState()`

### Wanna try Vite project in codespace?

ถ้าต้องการลองแบบ Vite JS สามารถใช้[โปรเจคเดียวกันนี้](https://github.com/teerasej/nextflow-react-js-vite) รันบน Github Codespace ได้นะ

```
https://github.com/teerasej/nextflow-react-js-vite
```

## 1. สร้าง CounterComponent.jsx

สร้างไฟล์ `src/CounterComponent.jsx`

ในไฟล์ให้เขียนประกาศ Component ตามด้านล่าง (สามารถใช้ **rfc** หรือ **rafc** snippet ได้)

```jsx
// src/CounterComponent.jsx

import React from 'react'

export default function CounterComponent() {

    // ประกาศตัวแปร แสดงตัวนับ
    let count = 0

    // ประกาศ function ที่กดแล้ว จะเพิ่มตัวนับ
    const increase = () => {
        count++
        console.log(count)
    }

    return (
        <div>
            <h2>Counter:</h2>
            {/* นำตัวแปรมาแสดงใน JSX */}
            <p>total: {count}</p>

            {/* นำ function มาใช้กับ event ของ button */}
            <button onClick={increase}>add 1</button>
        </div>
    )
}
```

## 2. แสดง CounterComponent ใน App.js

เสร็จแล้วเอาไปแสดงใน `src/App.jsx`

```jsx
// src/App.jsx

import { useState } from 'react'
import reactLogo from './assets/react.svg'
import viteLogo from '/vite.svg'
import './App.css'

// คำสั่ง import CounterComponent จากไฟล์ที่ประกาศไว้
import CounterComponent from './CounterComponent';

const Hello = (props) => {

  let username = props.username

  return (
    <div className="danger">
      <h1>Hello {username}</h1>
    </div>
  )
}

function Goodbye() {
  return (
    <div>
      <h1>Good Bye</h1>
    </div>
  )
}

function App() {

  let city = 'Bangkok';
  const popUpAlert = () => {
    alert('Sign in!')
  }

  return (
    <div>
      <Hello username={city}/>
      <Goodbye/>
      <button onClick={popUpAlert}>Sign in</button>

      {/* แสดง CounterComponent ใน App */}
      <CounterComponent/>
    </div>
  );
}

export default App;

```

## 3. ทดสอบกดปุ่ม

จะสังเกตเห็นว่า ตัวแปรมีการบวกค่าเพิ่ม แต่ในหน้าเว็บค่ายังเป็น 0 อยู่ เพราะ React render UI โดยใช้ค่าเริ่มต้นครั้งแรก

## 4. ใช้งาน react hook `useState()`


useState สามารถใช้สร้าง **ตัวแปร** และ **setter function** ที่ใช้อัพเดตตัวแปรนั้น การกำหนดค่าให้ตัวแปรผ่าน function ดังกล่าว เทียบเท่าการใช้คำสั่ง `setState()`

รูปแบบการใช้งาน react hook **useState** จะเป็นดังนี้ 

```
const [ชื่อตัวแปร, function ที่ใช้อัพเดตตัวแปร] = useState(ค่าเริ่มต้นของตัวแปร);
```

ดังนั้นเราจะปรับการทำงานของ `src/CounterComponent.js` ใหม่ตามด้านล่าง

```jsx
// src/CounterComponent.js
import React, { useState } from 'react'

export default function CounterComponent() {

    // สร้าง log เพื่อเช็คการ render ของ component
    // FYI: อาจจะเป็น log แสดงขึ้น 2 ครั้ง เพราะ <React.StrictMode> ในไฟล์ src/index.js ใน production จะไม่มีการทำงานแบบเบิ้ลแบบนี้
    console.log('Render counter component...')

    // ประกาศตัวแปร state, setter function และค่าเริ่มต้น
    const [countState, setCountState] = useState(1)

    console.log('Now, countState is ' + countState)
    
    const increase = () => {
        // กำหนดค่าให้ countState ผ่าน setter function
        setCountState(countState+1)
        console.log(countState)
    }

    return (
        <div>
            <h2>Counter:</h2>

            {/* นำตัวแปร state มาแสดงใน JSX */}
            <p>total: {countState}</p>

            <button onClick={increase}>add 1</button>
        </div>
    )
}

```
