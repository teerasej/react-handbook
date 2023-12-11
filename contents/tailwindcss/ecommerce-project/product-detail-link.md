
# กำหนด link เพื่อให้กดปุ่ม more detail เปิดมาหน้า Product Detail Page

## 1. กำหนด Route สำหรับหน้า Product Detail Page

```tsx
// เปิดไฟล์  src/App.tsx

import './App.css'
import { BrowserRouter as Router, Route, Routes } from 'react-router-dom';
import NavBar from './components/NavBar';
import HomePage from './pages/home-page/HomePage';
import ProductsPage from './pages/products-page/ProductsPage';
import ProductDetailPage from './pages/product-detail-page/ProductDetailPage'; 

function App() {
  return (
    <>
      <Router>
        <NavBar />
        <Routes>
          <Route path="/" element={<HomePage />} />
          <Route path="/products" element={<ProductsPage />} />

          {/* กำหนด Route สำหรับหน้า Product Detail Page โดยกำหนดส่วนท้ายให้เป็น parameter ชื่อ id */}
          <Route path="/products/:id" element={<ProductDetailPage />} /> 
        </Routes>
      </Router>
    </>
  )
}

export default App;
```

## 2. ทำให้ปุ่ม More Detail สามารถ Link ไปหน้า Product Detail Page ได้

```tsx
// เปิดไฟล์ src/pages/products-page/ProductItemComponent.tsx


// ใช้ hook useNavigate จาก react-router-dom เพื่อใช้ในการเปลี่ยนหน้า
import { useNavigate } from 'react-router-dom';
import imageUrl from './../../assets/toy-box.webp';
import IProduct from './IProduct';

interface IProductItemComponentProps {
    key: string;
    product: IProduct;
}

const ProductItemComponent = ({ product }: IProductItemComponentProps) => {
    
    // ใช้ hook useNavigate จาก react-router-dom เพื่อใช้ในการเปลี่ยนหน้า
    const navigate = useNavigate();

    // กำหนด function เมื่อกดปุ่ม more detail ให้เปลี่ยนหน้าไปหน้า Product Detail Page โดยส่ง id ของ product เข้าไปใน URL ด้วย
    const handleMoreDetails = () => {
        navigate(`/products/${product.id}`);
    };

    return (
        <div className="bg-white rounded-lg shadow-md p-4 border border-gray-300">
            <h2 className="text-xl font-bold mb-4">{product.title}</h2>
            <img src={imageUrl} alt="Product Image" className="w-fit mb-4 " />
            {/* ปุ่ม more detail ที่เมื่อกดจะเปลี่ยนหน้าไปหน้า Product Detail Page โดยเรียกใช้ function handleMoreDetails */}
            <button
                className="bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 rounded"
                onClick={handleMoreDetails}
            >
                More Details
            </button>
        </div>
    );
};

export default ProductItemComponent;
```

## 3. เสร็จแล้วทดสอบการทำงาน