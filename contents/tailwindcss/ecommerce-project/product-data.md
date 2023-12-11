
# โหลดข้อมูล json มาแสดงในหน้า Product Page

## 1. ดูไฟล์ json 

เปิดไฟล์ `src/data/products.json` และดูโครงสร้างของไฟล์ json ที่มีข้อมูลสินค้าอยู่

## 2. สร้าง Interface ที่มีโครงสร้างรองรับข้อมูลในไฟล์ json

ในที่นี้ให้เปิดดูไฟล์ `src/pages/products-page/IProduct.ts`

```tsx
// เปิดไฟล์ src/pages/products-page/IProduct.ts

export default interface Product {
    id: number,
    title: string;
    price: number;
    description: string;
    image: string;
}
```

## 3. นำเข้าข้อมูล json มาใช้ในหน้า Product Page

```tsx
// เปิดไฟล์ src/pages/products-page/ProductsPage.tsx

import React, { useState, useEffect } from 'react';
import ProductItemComponent from './ProductItemComponent';
import IProduct from './IProduct';

// import ไฟล์ json มาใช้
import ProductJSON from "./../../data/products.json";


export default function ProductsPage() {
 
  // เลือกเฉพาะ key ที่เป็น array ของ product มาใช้ จากไฟล์ ่json
  // โดยเก็บไว้ในตัวแปร array ของ product
  const products: IProduct[] = ProductJSON.products;

  return (
    <div className="flex justify-center">
      <div className="pt-10 m-10 bg-slate-200 w-1024">
        <h1 className="text-3xl mb-5 text-center">Products Page</h1>
        <div className="grid grid-cols-2 gap-4">
            {/* ใช้ function .map ของตัวแปร array ชื่อ products ในการวนลูปเอา item ใน array ทีละตัว ใส่เข้ามาใน function เพื่อสร้างเป็น jsx component และส่งข้อมูลเข้าในทาง props ด้วย */}
          {products.map((product) => (
            <ProductItemComponent key={product.title} product={product} />
          ))}
        </div>
      </div>
    </div>
  );
}

```

## 4. แสดงข้อมูลใน ProductItemComponent

```tsx
// เปิดไฟล์ src/pages/products-page/ProductItemComponent.tsx


import imageUrl from './../../assets/toy-box.webp';
import IProduct from './IProduct';

interface IProductItemComponentProps {
    key: string;
    product: IProduct;
}

const ProductItemComponent = ({ product }: IProductItemComponentProps) => {

    return (
        <div className="bg-white rounded-lg shadow-md p-4 border border-gray-300">
            {/* แสดงข้อมูลที่ส่งเข้ามาใน props โดยเลือกส่วนของ title */}
            <h2 className="text-xl font-bold mb-4">{product.title}</h2>

            {/* แสดงข้อมูลที่ส่งเข้ามาใน props โดยเลือกส่วนของ image ที่เป็น URL */}
            <img src={product.image} alt="Product Image" className="w-fit mb-4 " />
            <button className="bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 rounded">
                More Details
            </button>
        </div>
    );
};

export default ProductItemComponent;

