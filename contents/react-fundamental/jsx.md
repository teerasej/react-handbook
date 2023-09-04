
# Using JSX


## 1. Use JSX in react

JSX จะเป็นของเลียนแบบ HTML ที่เขียนลงไปในโค้ดไฟล์ JavaScript โดยตรง 

### 1.1 มาดู Component ตัวแรกของโปรเจค

ลองเปิดไฟล์ `src/index.js` จะเห็นว่ามีคำสั่ง JavaScript ในการสั่ง render `<App>` component อยู่ในบรรทัดที่ 10

```jsx
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

ซึ่งนี่เป็นการเรียกใช้ React ให้ทำการเริ่มทำงานเพื่อสร้าง component ตัวแรก โดยกำหนดให้ render component ในไฟล์ `public/index.html` ในโค้ด บรรทัดที่ 31 ที่เป็น `<div id="root">`

### 1.2 App Component อยู่ที่ไหน

ในที่นี้ให้เปิดไฟล์ `src/App.js` ที่นี่จะมี code ซึ่งกำหนดการทำงาน และหน้าตาของ App Component อยู่

ให้ลองแก้ไขโค้ดบรรทัดที่ 9 - 11 จาก

```html
<p>
   Edit <code>src/App.js</code> and save to reload.
</p>
```

เป็นแบบด้านล้าง และบันทึกไฟล์

```html
<p>
   สวัสดี React
</p>
```

จะสังเกตว่า เราจะเห็นข้อความบนหน้าเว็บเปลี่ยนไป เพราะในที่นี้ App component คือ User Interface เดียวที่ถูก render ขึ้นมาด้วย React 

และ JSX ที่ถูก return ออกไปจากตัว `function App()` นี้ ก็คือส่วนที่ React ทำการ render ขึ้นมานั่นเอง

เอาล่ะ ก่อนไปขั้นตอนต่อไป ลองแก้ไข JSX ของ App component ให้เป็นแบบด้านล่างดู

```jsx
// src/App.js

import logo from './logo.svg';
import './App.css';

function App() {
  return (
    <div>
      <h1>Hello</h1>
    </div>
  );
}

export default App;

```

## การใช้ตัวแปร กับ JSX

เราสามารถใช้ **expression** (หรือที่เห็นเป็นเครื่องหมาย `{}`) ใน JSX เพื่อแทรกตัวแปรลงไปแสดงผลได้

```jsx
import logo from './logo.svg';
import './App.css';

function App() {

  // ประกาศตัวแปร 
  let city = 'Bangkok';

  return (
    <div>
      {/* แทรกตัวแปรเข้ามาใน JSX ด้วย {} */}
      <h1>Hello, {city}</h1>
    </div>
  );
}

export default App;

```

## การใช้ CSS Style กับ JSX

เราสามารถกำหนดค่า CSS ให้กับ JSX ได้ คล้ายๆ กับตอนเขียน HTML ทั่วไป

โดยในที่นี้จะเห็นว่าตัวโปรเจคมีการเขียนไฟล์ `src/App.css` แยกออกมาด้วย ลองเปิดไฟล์ และเขียน CSS Selector เพิ่มเข้าไปตามด้านล่าง

```css
#header {
    font-size: 60px;
} 

.danger {
    color: red;
}
```

กลับมาที่ไฟล์ `src/App.js` จะเห็นคำสั่ง `import` ไฟล์ CSS เพื่อเอาเข้ามาใช้ใน component

```jsx
// src/App.js
import logo from './logo.svg';

// คำสั่ง import css
import './App.css';

function App() {

  let city = 'Bangkok';

  return (
    <div>
      {/* กำหนด id และ className เพื่อเรียกใช้ CSS ตามที่กำหนด */}
      <h1 id="header" className='danger'>Hello, {city}</h1>
    </div>
  );
}

export default App;

```

สังเกตว่า `ID selector` กำหนดใช้งานกับ `id` attribute ตามปกติ แต่การใช้งาน `class selector` จะกำหนดใช้งานกับ `className` (ไม่เหมือนใน HTML ทั่วไปที่จะใช้กับ `class` attribute)