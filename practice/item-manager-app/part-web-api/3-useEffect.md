
# เรียกข้อมูลจาก Web API หลังจากแสดง Item List

## 1. ใช้ `useEffect()` ในการรันโค้ด หลัง component render

```jsx
// src/components/list-item/ItemList.js
import { useEffect } from 'react'

export default function ItemList() {

    //..

    useEffect(() => {
        const loadMessage = async () => {

        }
        loadMessage();
    }, [])
```

## 2. ใช้ Axios เรียกข้อมูลจาก Web API

```js
// src/components/list-item/ItemList.js
import Axios from 'axios';

export default function ItemList() {

    //..

    useEffect(() => {
        const loadMessage = async () => {
            // ส่ง GET request ไปที่ Web API
            const notes = await Axios.get('http://localhost:3010/notes')
            // แสดงข้อมูล json ที่ได้มาจาก web api
            console.log('notes loaded:', notes.data);

        }
        loadMessage();
    }, [])
```
