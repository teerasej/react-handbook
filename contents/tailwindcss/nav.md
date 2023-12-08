
# สร้าง Menu หรือ Navigation ของเว็บไซต์



เปิดไฟล์ `App.tsx` และใช้งาน JSX ในการสร้าง HTML และ CSS ของเว็บไซต์ ตามด้านล่าง


```tsx
import React from 'react';
import './App.css';

function App() {
  return (
    <div>
      <nav className=' bg-black flex flex-row'>
        <p className=' text-white p-4 hover:bg-red-400 active:bg-red-700'>Home</p>
      </nav>
    </div>
  );
}

export default App;
```

- `<nav>`: คือ HTML element ที่ใช้สำหรับการนำทาง มี class ต่อไปนี้:
  - `bg-black`: ตั้งสีพื้นหลังเป็นสีดำ
  - `flex`: ใช้ flexbox layout สำหรับการจัดวาง child elements
  - `flex-row`: จัด child elements ในแนวนอน

- `<p>`: คือ HTML element ที่ใช้สำหรับข้อความย่อย มี class ต่อไปนี้:
  - `text-white`: ตั้งสีข้อความเป็นสีขาว
  - `p-4`: ตั้ง padding ทั้ง 4 ด้านเป็น 4 units
  - `hover:bg-red-400`: เมื่อเคอร์เซอร์ hover บน element สีพื้นหลังจะเปลี่ยนเป็นสีแดง 400
  - `active:bg-red-700`: เมื่อ element ถูกคลิก (active state) สีพื้นหลังจะเปลี่ยนเป็นสีแดง 700