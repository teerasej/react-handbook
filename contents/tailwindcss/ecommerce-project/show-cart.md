
# แสดงข้อมูลสินค้าในตะกร้า

```tsx
// เปิดไฟล์ src/components/CartCounter.tsx


// import hook ชื่อ useCart เพื่อใช้ function เข้าถึง context ที่สร้างไว้ใน CartProvider
import { useCart } from '../contexts/CartProvider'

export default function CartCounter() {

    // เรียกใช้ hook ชื่อ useCart เพื่อใช้ function ที่เราสร้างไว้ใน CartProvider
   const { itemsInCart } = useCart();

  // แสดงจำนวนสินค้าในตะกร้า
  return (
    <div className="text-1m font-bold p-3 hover:bg-red-600">Cart: {itemsInCart.length}</div>
  )
}


```