
# แก้ไขปัญหากดลิ้งค์เปลี่ยนหน้าแล้วข้อมูลหาย

ในที่นี้เรามีการใช้ `<a>` ใน NavBar Component ซึ่งจะทำให้เกิดการ refresh ของหน้าเว็บ เราเลยต้องเปลี่ยนเป็นใช้ `<Link>` ของ react-router-dom แทน

```tsx
// เปิดไฟล์ src/components/NavBar.tsx


import CartCounter from './CartCounter'

// import Link มาจาก react-router-dom
import { Link } from 'react-router-dom'

export default function NavBar() {
  return (
    <nav className="bg-black text-white">
      <div className="flex justify-between">
        <div className='flex'>

        {/* ใช้ Link แทน a */}
          <Link to="/" className="text-1m font-bold p-3 hover:bg-red-600">Home</Link>

        {/* ใช้ Link แทน a */}
          <Link to="/products" className="text-1m font-bold p-3  hover:bg-red-600">Products</Link>
        </div>
        <CartCounter/>
      </div>
    </nav>
  )
}

```