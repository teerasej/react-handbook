
# สร้าง Redux Store 

store จะทำหน้าที่เชื่อม component เข้ากับ reducer 
มองว่าเป็น router wifi ในบ้านที่เชื่อมต่อคอมพิวเตอร์ทุกเครื่องเข้ากับอินเตอร์เน็ตก็ได้ 

สร้างไฟล์ `redux/store.js` 

ส่วนนี้ไม่มี snippet นะต้องทำเอง

```js
// เรียกใช้ reducer ที่เราประกาศไว้ในไฟล์ reducer.js
import reducer from "./reducer";
// เรียกใช้ function ของ redux ในการสร้าง store
import { createStore } from 'redux'; 

// ประกาศ function ชื่อ 'configureStore' เดี๋ยวเอาไปใช้ในไฟล์ App.js
export const configureStore = () => {

    // ใส่ reducer เข้าไปใน store และ return ไปใช้งานที่อื่น
    const store = createStore(reducer);
    return store;
} 
```
