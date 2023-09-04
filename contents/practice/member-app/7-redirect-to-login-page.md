
# เปิดไปหน้า Login ถ้ายังไม่ได้ลงชื่อเข้าใช้

กำหนดตัวแปรที่แสดงถึงการ Login ถ้ามีการ Login ของ User เรียบร้อย (check จาก token) ให้แสดงหน้าแอพปกติ ถ้ายังไม่ Login ให้แสดงไปที่หน้า Login แทน

- [useHistory()](https://reactrouter.com/web/api/Hooks)

```jsx
// src/components/Main.js


import React, { useState, useEffect} from 'react'
import { CircularProgress } from "@material-ui/core";

// เรียกใช้ hook ของ react-router
import { useHistory } from 'react-router';

export default function Main() {

    const [onLoading, setOnLoading] = useState(true);

    // เข้าถึงตัวแปร history จาก hook มักใช้ในการ navigate แอพไปยัง path ต่างๆ ที่กำหนดไว้ใน Router
    const history = useHistory();

    // สร้างตัวแปร ที่แสดงถึงการ login หรือยังไม่ login เข้าใช้งานโดยผู้ใช้
    let alreadyLogin = false;


    useEffect(() => {
        // check token 


        // หยุดแสดงตัวโหลด
        setOnLoading(false);

        // ถ้ายังไม่ได้ login เข้าใช้ ให้เปิดไปยังหน้า login
        if(!alreadyLogin) {
          history.push('/login');
        }

        
    }, []);


    if (onLoading) {
        return (
            <CircularProgress />
        );
    } else {
        return (
            <div>Home</div>
        );
    }
}

```