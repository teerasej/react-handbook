
# สร้าง Context และ Provider สำหรับข้อมูลสินค้า

## 1. สร้างไฟล์สำหรับเก็บ Context และ Provider component 

- เขียนไฟล์ `src/contexts/ProductProvider.tsx` ดังด้านล่าง
- จะเห็นว่า ดูเหมือน component ปกติที่มีการรับ Props เข้ามาตามปกติ
- แต่ Props ของเรานั้นจะมี type เป็น ReactNode ซึ่งก็คือ type ของ JSX Component ส่วนใหญ่นั่นเอง

```tsx
// เปิดไฟล์ src/contexts/ProductProvider.tsx

import React, { ReactNode, createContext } from 'react'

interface ProductProviderProps {
    children: ReactNode;
}
  
export default function ProductProvider({ children }: ProductProviderProps) {

  return (
    <div>ProductProvider</div>
  )
}
```

## 2. ใช้แนวคิด Context และ Provider ในการจัดการข้อมูลสินค้า เพื่อใช้ในทุก component ในโปรเจค 

- ใช้ createContext สร้าง context สำหรับเก็บ array ของ IProduct 
- โหลดข้อมูล json มาเก็บไว้ในตัวแปร products 
- นำ ProductContext ที่สร้างไว้ มาใช้เป็น component 
- ใส่ตัวแปร product ที่เป็น array เข้าไปใน provider ของ context
- สร้าง react hook function ชื่อ `useProducts` เพื่อให้สามารถเรียกใช้ context จากที่ไหนก็ได้ที่อยู่ภายใน component `ProductProvider`


```tsx
// เปิดไฟล์ src/contexts/ProductProvider.tsx


import React, { ReactNode, createContext } from 'react'
import ProductJSON from "./../data/products.json";
import IProduct from '../pages/products-page/IProduct';

// ใช้ createContext สร้าง context สำหรับเก็บ array ของ IProduct
const ProductContext = createContext<IProduct[]>([]);

interface ProductProviderProps {
    children: ReactNode;
}
  

export default function ProductProvider({ children }: ProductProviderProps) {

  // โหลดข้อมูล json มาเก็บไว้ในตัวแปร products 
  // สังเกตว่าคล้ายกับที่เราทำใน ProductPage ก่อนหน้านี้
  const products: IProduct[] = ProductJSON.products;

  return (
    {/* นำ ProductContext ที่สร้างไว้ มาใช้เป็น component  */}
    {/* ใส่ตัวแปร product ที่เป็น array เข้าไปใน provider ของ context เพื่อให้ตรงตามที่สร้างไว้ด้านบน */}
    <ProductContext.Provider value={products}>
        {children}
    </ProductContext.Provider>
  )
}

// สร้าง react hook function ชื่อ `useProducts` เพื่อให้สามารถเรียกใช้ context จากที่ไหนก็ได้ที่อยู่ภายใน component `ProductProvider`
export const useProducts = () => useContext(ProductContext);
```

## 3. ทำ Product Provider มาใช้

```tsx
// เปิดไฟล์ src/App.tsx
import './App.css'
import { BrowserRouter as Router, Route, Routes } from 'react-router-dom';
import NavBar from './components/NavBar';
import HomePage from './pages/home-page/HomePage';
import ProductsPage from './pages/products-page/ProductsPage';
import ProductDetailPage from './pages/product-detail-page/ProductDetailPage'; // Import the product detail page component
import ProductProvider from './contexts/ProductProvider';

function App() {
  return (
    <>
      <Router>
        {/* ใช้ ProductProvider ที่สร้างไว้เป็น component ครอบทุกๆ อย่างที่อยู่ภายใน Router */}
        <ProductProvider>
          <NavBar />
          <Routes>
            <Route path="/" element={<HomePage />} />
            <Route path="/products" element={<ProductsPage />} />
            <Route path="/products/:id" element={<ProductDetailPage />} />
          </Routes>
        </ProductProvider>
      </Router>
    </>
  )
}

export default App;
```