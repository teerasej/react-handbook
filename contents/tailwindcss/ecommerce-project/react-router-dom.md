
# ใช้ React Router Dom จัดการระบบเมนู

```tsx
// src/App.tsx
import './App.css'

// นำเข้า React Router Dom
import { BrowserRouter as Router, Route, Routes } from 'react-router-dom';
import NavBar from './components/NavBar';
import HomePage from './pages/home-page/HomePage';
import ProductsPage from './pages/products-page/ProductsPage';
function App() {


  return (
    <>
      {/* ใช้ Router ครอบเพื่อให้เว็บไซต์ทำงานร่วมกับ React Router Dom */}
      <Router>

        {/* นำเข้า NavBar ที่สร้างไว้ */}
        <NavBar />

        {/* ใช้ Routes ครอบเพื่อกำหนด path และ component ที่จะแสดงเมื่อเข้าถึง path นั้นๆ */}
        <Routes>
          {/* กำหนด path และ component ที่จะแสดงเมื่อเข้าถึง path นั้นๆ โดยกำหนด home page และ Product Page ตามลำดับ*/}  
          <Route path="/" element={<HomePage />} />
          <Route path="/products" element={<ProductsPage />} />
        </Routes>
      </Router>
    </>
  )
}

export default App
