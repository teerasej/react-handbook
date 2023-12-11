
# สร้าง Menu Bar (Navigation Bar/NavBar) ด้านบนของเว็บไซต์

เปิดไฟล์ `src/components/NavBar.tsx` และเพิ่มโค้ดดังนี้

```tsx
// src/components/NavBar.tsx

import React from 'react'
import CartCounter from './CartCounter'

export default function NavBar() {
  return (
    {/* สร้าง navigation bar ด้านบนของเว็บไซต์ */   }
    <nav className="bg-black text-white">
      <div className="flex justify-between">

        {/* จัดกลุ่ม navigation item เพื่อให้เมนู อยู่คนละด้านกับ Cart Counter เมื่อใส่ justify-between ใน div container */}
        <div className='flex'>
          {/* สร้าง link และจัดเป็น navigation item  */}
          <a to="/" className="text-1m font-bold p-3 hover:bg-red-600">Home</a>
          <a to="/products" className="text-1m font-bold p-3  hover:bg-red-600">Products</a>
        </div>

        {/* นำ Cart counter มาวางในเมนู  */}
        <CartCounter/>
      </div>
    </nav>
  )
}

```