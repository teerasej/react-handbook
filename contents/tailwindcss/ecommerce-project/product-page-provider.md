
# กำหนดให้ Product Page ใช้ข้อมูลจาก Provider แทน

```tsx
// เปิดไฟล์ src/pages/products-page/ProductsPage.tsx

import ProductItemComponent from './ProductItemComponent';
import IProduct from './IProduct';

// สลับจากการ import ข้อมูล json มาใช้ มาใช้จาก context แทน
import { useProducts } from '../../contexts/ProductProvider';

export default function ProductsPage() {

  // เรียกใช้ Context ที่อยู่ใน ProductProvider ผ่าน react hook function ชื่อ `useProducts` ที่เราสร้างไว้ 
  const products: IProduct[] = useProducts();

  return (
    <div className="flex justify-center">
      <div className="pt-10 m-10 bg-slate-200 w-1024">
        <h1 className="text-3xl mb-5 text-center">Products Page</h1>
        <div className="grid grid-cols-2 gap-4">
          {products.map((product) => (
            <ProductItemComponent key={product.title} product={product} />
          ))}
        </div>
      </div>
    </div>
  );
}

```