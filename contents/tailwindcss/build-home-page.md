
# สร้าง Layout หน้า Home

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

       
      <div className='flex flex-col items-center'>
        <h1 className='text-2xl mt-9'>Welcome to the Product Catalog</h1>
        <p className='text-xl'>Click on the link above to see the products</p>
        <div className='sm:w-full sm:mx-3 md:w-full md:mx-3 lg:w-1024 bg-gray-300 rounded flex flex-col items-center mt-10'>
          <p className='p-32 text-4xl'>Hej!</p>
        </div>
      </div>
    </div>
  );
}

export default App;

```

- `<div className='flex flex-col items-center'>`: คือ HTML element ที่ใช้สร้าง "division" หรือส่วนของหน้าเว็บ มี class ต่อไปนี้:
  - `flex`: ใช้ flexbox layout สำหรับการจัดวาง child elements
  - `flex-col`: จัด child elements ในแนวตั้ง
  - `items-center`: จัด child elements ให้อยู่ตรงกลางแนวตั้ง
- <h1 className='text-2xl mt-9'>: คือ HTML element ที่ใช้สำหรับหัวข้อขนาดใหญ่ มี class ต่อไปนี้:
  - `text-2xl`: ตั้งขนาดข้อความเป็น 2xl
  - `mt-9`: ตั้ง margin-top เป็น 9 units
- <p className='text-xl'>: คือ HTML element ที่ใช้สำหรับข้อความย่อย มี class ต่อไปนี้:
  - `text-xl`: ตั้งขนาดข้อความเป็น xl
- <div className='sm:w-full sm:mx-3 md:w-full md:mx-3 lg:w-1024 bg-gray-300 rounded flex flex-col items-center mt-10'>: คือ HTML element ที่ใช้สร้าง "division" หรือส่วนของหน้าเว็บ มี class ต่อไปนี้:
  - `sm:w-full sm:mx-3 md:w-full md:mx-3 lg:w-1024`: ตั้งค่า width และ margin-x ตามขนาดหน้าจอ
  - `bg-gray-300`: ตั้งสีพื้นหลังเป็นสีเทา 300
  - rounded: ตั้งขอบมน
  - `flex flex-col items-center`: ใช้ flexbox layout สำหรับการจัดวาง child elements ในแนวตั้ง และจัดให้อยู่ตรงกลางแนวตั้ง
  - `mt-10`: ตั้ง margin-top เป็น 10 units
- <p className='p-32 text-4xl'>: คือ HTML element ที่ใช้สำหรับข้อความย่อย มี class ต่อไปนี้:
  - `p-32`: ตั้ง padding ทั้ง 4 ด้านเป็น 32 units
  - `text-4xl`: ตั้งขนาดข้อความเป็น 4xl

