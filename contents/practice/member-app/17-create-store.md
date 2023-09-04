
# สร้าง store ใน redux 

- กำหนด reducer ที่เตรียมไว้ พร้อมใช้งานใน store

```jsx
// src/redux/store.js

// function createStore() ที่ redux เตรียมให้นักพัฒนา setup store object
import { createStore } from 'redux';

// import reducer ที่เราสร้างไว้ก่อนหน้านี้ 
import reducer from './reducer';

export default function configureStore() {
    // กำหนด reducer ลงไปใน store
    return createStore(reducer);
}
```