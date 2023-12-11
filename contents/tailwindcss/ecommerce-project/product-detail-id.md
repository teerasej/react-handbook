
# เรียกใช้ id ของสินค้าจาก url ใน Product Detail Page



```tsx
// เปิดไฟล์ src/pages/product-detail-page/ProductDetailPage.tsx
import { useParams } from "react-router-dom";
import { useProducts } from "../../contexts/ProductProvider";
import IProduct from "../products-page/IProduct";



export default function ProductDetailPage() {

    // ใช้ react hook function ชื่อ useParams เพื่อดึงค่า id จาก url
    const { id } = useParams<{ id: string }>();

    // ใช้ react hook function ชื่อ useProducts เพื่อดึงข้อมูลสินค้าทั้งหมดมาใช้งาน (ส่วนนี้คือที่เราเตรียมไว้ใน ProductProvider)
    const products:IProduct[] = useProducts();

    // ใช้ array method ชื่อ find เพื่อหาข้อมูลสินค้าที่มี id ตรงกับที่ระบุใน url
    const product = products.find( product => product.id === Number(id));


    return (
        <div className="flex justify-center">
            <div className="pt-10 m-10 bg-slate-200 w-1024">
                <div className="bg-white rounded-lg shadow-md p-4 border border-gray-300">
                    <h2 className="text-xl font-bold mb-4">{product?.title}</h2>
                    <div className="flex flex-row justify-between">
                        <p className="p-2">
                            ราคา {product?.price} บาท
                        </p>
                        <button 
                            className="bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 rounded float-right">
                            Add to Cart
                        </button>
                    </div>
                </div>
            </div>
        </div>
    );
}


```