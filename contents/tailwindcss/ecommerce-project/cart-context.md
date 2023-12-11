
# ใช้ Context ในการสร้างตัวแปร และ function สำหรับจัดการสินค้าในตะกร้า

```tsx
// เปิดไฟล์ src/contexts/CartProvider.tsx

import { ReactNode, createContext, useContext, useState } from "react";
import IProduct from "../pages/products-page/IProduct";

interface ICartProviderProps {
    children: ReactNode
}

// 1. สร้าง interface สำหรับกำหนดว่า context ตัวนี้จะมีตัวแปร หรือ function อะไรบ้าง
interface ICartContext {
    
    // 2. สร้างตัวแปร state เก็บข้อมูลสินค้าในตะกร้า อันนี้เป็น array 
    itemsInCart: IProduct[];
    
    // 3. สร้าง function ที่จะรับ product (กดจากปุ่ม add to cart) และอัพเดตในตัวแปร
    addToCart: (product: IProduct) => void;
}

// 4. สร้าง context ด้วย hook ชื่อ createContext โดยกำหนดค่าเริ่มต้นตาม interface ที่เราสร้างไว้
const CartContext = createContext<ICartContext>({ itemsInCart: [], addToCart: () => {} });


export default function CartProvider({ children }: ICartProviderProps) {

    // สร้างตัวแปร state เพื่อที่จะได้ใส่ลงไปใน context และมีกลไกในการอัพเดตค่่าให้ component ที่ใช้ context นี้
    const [itemsInCart, setItemsInCart] = useState<IProduct[]>([]);

    // สร้าง function ที่จะรับ product (กดจากปุ่ม add to cart) และอัพเดตในตัวแปร โดยในที่นี้เรารับข้อมูลล่าสุดของตัวแปร state มาสร้างเป็น array ใหม่ 
    const addToCart = (product:IProduct) => {
        setItemsInCart(existingCart => [...existingCart, product]);    
    }

  return (
    {/* นำตัวแปร state และ function ใส่ลงใน value ของ context ตามที่กำหนดโดย interface */}
    <CartContext.Provider value={{ itemsInCart, addToCart }}>
        {children}
    </CartContext.Provider>
  )
}


// สร้าง hook ชื่อ useCart เพื่อที่จะเรียกใช้จาก component ไหนก็ได้
export const useCart = () => useContext(CartContext);


```