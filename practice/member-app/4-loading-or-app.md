
# สร้าง state เพื่อแสดงตัวโหลดตอนที่แอพยังไม่พร้อม 

- แอพจะพร้อมเมื่อเช็ค token เสร็จ เมื่อพร้อมแล้วก็จะแสดงเป็นหน้าข้อมูลสมาชิก 
  

## 1. ใช้ React Hook `useState`

- ในที่นี้เราจะใช้ [CircularProgress](https://material-ui.com/components/progress/) component ของ material-ui เป็นตัวแสดงสถานะการทำงาน
- [useState](https://reactjs.org/docs/hooks-state.html) เป็น react hook ที่ทำให้สร้างตัวแปร และกลไกที่ทำให้ component render ตัวเองใหม่ได้

```jsx
// src/components/Main.js

// นำเข้า useState hook
import React, { useState } from 'react';
// ตัว loading แบบวงกลม
import { CircularProgress } from "@material-ui/core";

export default function Main() {

    // กำหนดตัวแปร state ชื่อ onLoading
    // กำหนดฟังก์ชั่นสำหรับอัพเดตค่าตัวแปร state ชื่อ setOnLoading
    // กำหนดค่าเริ่มต้นให้ตัวแปร onLoading เป็น true
    const [onLoading, setOnLoading] = useState(true);

    // ถ้า onLoading เป็น true
    // แสดงเป็นตัวโหลดวงกลม
    if (onLoading) {
        return (
            <CircularProgress />
        );
    // ไม่งั้นก็แสดงเป็นหน้า home
    } else {
        return (
            <div>Home</div>
        );
    }
}

```

## 2. ใช้ useEffect hook เพื่อรันโค้ดทำงานหลังจากแสดงตัว Component แล้ว 

โค้ดใน [useEffect](https://reactjs.org/docs/hooks-effect.html) hook จะทำงานหลังจากแสดง component Main แล้ว ในที่นี้เราเตรียมเช็ค token และหยุดแสดงตัวโหลดเมื่อทำงานเสร็จ

```jsx
// src/components/Main.js

// import useEffect hook 
import React, { useState, useEffect} from 'react'
import { CircularProgress } from "@material-ui/core";

export default function Main() {

    const [onLoading, setOnLoading] = useState(true);

    // กำหนด function ให้ useEffect ซึ่งจะมีการ check token (ได้จากการ login)
    // หลังจากได้ token แล้ว ก็ให้กำหนดค่าตัวแปร state onLoading เป็น false 
    // กลไกนี้จะทำให้ Main component รันตัวเองใหม่อีกครั้ง
    useEffect(() => {
        // check token 

        // หยุดแสดงตัวโหลด
        setOnLoading(false);
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