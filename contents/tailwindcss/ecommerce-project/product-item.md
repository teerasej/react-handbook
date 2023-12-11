

# แสดง Product Item ในหน้า Product Page

```tsx
// src/pages/products-page/ProductsPage.tsx

import ProductItemComponent from './ProductItemComponent';

export default function ProductsPage() {
  return (
    <div className="flex justify-center">
      <div className="pt-10 m-10 bg-slate-200 w-1024">

        <h1 className="text-3xl mb-5 text-center">Products Page</h1>

        <div className="grid grid-cols-2 gap-4">

          {/* สร้าง Product Item Component ขึ้นมา 5 ตัว */}
          <ProductItemComponent />
          <ProductItemComponent />
          <ProductItemComponent />
          <ProductItemComponent />
          <ProductItemComponent />
        </div>

      </div>
    </div>
  );
}

```