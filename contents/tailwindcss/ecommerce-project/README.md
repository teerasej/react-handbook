
# E-commerce Project

## 1. การติดตั้ง

1. ดาวน์โหลด/clone โปรเจคมาจาก https://github.com/teerasej/nextflow-vite-react-typescript-lab-01/tree/start
2. เปิดโปรเจคด้วย Visual Studio Code
3. รันคำสั่ง `npm install` เพื่อติดตั้ง package ที่จำเป็น
4. รันคำสั่ง `npm run dev` เพื่อรันโปรเจค

## 2. Navigation Menu

5. [สร้าง Menu Bar (Navigation Bar/NavBar) ด้านบนของเว็บไซต์](navbar.md)
6. [ใช้ React Router Dom จัดการระบบเมนู](react-router-dom.md)

## 3. Product Data

7. [แสดง Product Item ในหน้า Product Page](product-item.md)
8. [โหลดข้อมูล json มาแสดงในหน้า Product Page](product-data.md)

## 4. Product Detail Page

9. [กำหนด link เพื่อให้กดปุ่ม more detail เปิดมาหน้า Product Detail Page](product-detail-link.md)

## 5. Context และ Provider: ข้อมูลสินค้า

เรามักสร้าง Context และ​ Provider component เพื่อใช้ในการจัดการข้อมูลที่ต้องการให้สามารถเข้าถึงได้จากทุกๆ ที่ในโปรเจค

10. [สร้าง Context และ Provider สำหรับข้อมูลสินค้า](context-provider-product.md)
11. [กำหนดให้ Product Page ใช้ข้อมูลจาก Provider แทน](product-page-provider.md)  
12. [เรียกใช้ id ของสินค้าจาก url ใน Product Detail Page](product-detail-id.md)

## 6. Context และ Provider: สินค้าในตะกร้า

13. [สร้าง CartProvider เพื่อจัดการข้อมูลตะกร้าสินค้า](cart-provider.md)
14. [ใช้ Context ในการสร้างตัวแปร และ function สำหรับจัดการสินค้าในตะกร้า](cart-context.md)
15. [เพิ่มข้อมูลลงใน Cart ผ่านปุ่ม Add to Cart](add-to-cart.md)
16. [แสดงข้อมูลสินค้าในตะกร้า](show-cart.md)
17. [แก้ไขปัญหากดลิ้งค์เปลี่ยนหน้าแล้วข้อมูลหาย](fix-navigate-context-disappear.md)