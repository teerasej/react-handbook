
# สร้าง CartProvider เพื่อจัดการข้อมูลตะกร้าสินค้า

## 1. สร้างไฟล์สำหรับเก็บ Context และ Provider component

```tsx
// เปิดไฟล์ src/contexts/CartProvider.tsx

import { ReactNode } from "react";

interface ICartProviderProps {
    children: ReactNode
}

export default function CartProvider({ children }: ICartProviderProps) {
  return (
    <div>
        {children}
    </div>
  )
}

```

## 2. นำ CartProvider มาใช้ใน App

```tsx
// เปิดไฟล์ src/App.tsx

import './App.css'
import { BrowserRouter as Router, Route, Routes } from 'react-router-dom';
import NavBar from './components/NavBar';
import HomePage from './pages/home-page/HomePage';
import ProductsPage from './pages/products-page/ProductsPage';
import ProductDetailPage from './pages/product-detail-page/ProductDetailPage'; // Import the product detail page component
import ProductProvider from './contexts/ProductProvider';
import CartProvider from './contexts/CartProvider';

function App() {
  return (
    <>
      <Router>

        {/* นำ CartProvider มาใช้ใน App */}
        <CartProvider>
          <ProductProvider>
            <NavBar />
            <Routes>
              <Route path="/" element={<HomePage />} />
              <Route path="/products" element={<ProductsPage />} />
              <Route path="/products/:id" element={<ProductDetailPage />} />
            </Routes>
          </ProductProvider>
        </CartProvider>
      </Router>
    </>
  )
}

export default App;
```

