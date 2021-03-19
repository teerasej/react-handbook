
# เรียกข้อมูลผู้ใช้ 

## 1. สร้าง function เรียกข้อมูลผู้ใช้จาก WebAPI

```js
// src/services/WebAPI.js

import axios from 'axios';
const endpoint = '';

const client = axios.create({
    baseURL: endpoint
})

export const signUp = async (jsonData) => {
    //...
}


export const login = async (jsonData) => {
    //...
}

// สร้าง function ที่รับค่า token เข้ามาใช้งาน
export const getUserProfile = async (token) => {

    console.log(token)

    // สร้าง object config สำหรับใช้ในการขอข้อมูลผู้ใช้
    // ในที่นี้เราใช้วิธีแทรก token ไปกับ header 
    const config = { 
        headers: {
            'Authorization' : `Bearer ${token}`,
            'Content-Type': 'application/json'
        }
    };

    // ส่ง request ไปที่ web api แบบแนบ token ไปกับ header
    let response = await client.get(
        '/users/profile',
        config
    );

    console.log(response);

    // ในที่นี้ถือว่าถ้า status 200 เราน่าจะได้ข้อมูลมาจาก Web API 
    if(response.status == 200) {
        return response.data.user;
    } else {
        return undefined;
    }
}


```

## 2. สร้าง HomePage Component เพื่อเป็นตัวแทนของหน้า Home หลังจาก login ได้แล้ว

```jsx
// src/pages/HomePage.js

import React, { useEffect,useState } from 'react'
import { useSelector } from 'react-redux';
import useAsyncEffect from 'use-async-effect'
import { getUserProfile } from '../services/WebAPI';

export default function HomePage() {

    return (
        <div>
        </div>
    )
}

```

## 3. นำ HomePage Component มาแสดงใน Main Component

```jsx
// src/components/Main.js


import React, { useState, useEffect} from 'react'
import { CircularProgress } from "@material-ui/core";
import { useHistory } from 'react-router';
import { useSelector } from 'react-redux';
import HomePage from '../pages/HomePage';

export default function Main() {

    const [onLoading, setOnLoading] = useState(true);
    const history = useHistory();
    const token = useSelector(state => state.token);

    let alreadyLogin = false;


    useEffect(() => {
        // check token 
        if(token) {
            alreadyLogin = true;
        }

        // หยุดแสดงตัวโหลด
        setOnLoading(false);

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
            <div>
                <HomePage/>
            </div>
        );
    }
}

```

## 4. ดึง token มาจาก redux state และใช้ในการส่ง request ขอข้อมูลผู้ใช้จาก web api

```jsx
// src/pages/HomePage.js


import React, { useEffect,useState } from 'react'
import { useSelector } from 'react-redux';
import useAsyncEffect from 'use-async-effect'
import { getUserProfile } from '../services/WebAPI';

export default function HomePage() {

    // ใช้ useSelector hook เพื่อดึงค่า token มาจาก redux state
    const token = useSelector(state => state.token);

    // ใช้ useAsyncEffect hook ในการเรียกใช้ function getUserProfile ที่สร้างไว้ แบบ async
    // useAsyncEffect เป็น package ติดตั้งเพิ่มเติม ไม่มีใน react ปกติ
    useAsyncEffect(async () => {

        // ถ้าขอข้อมูลสำเร็จ ค่าที่ได้จาก function getUserProfile จะเป็น object ที่มีข้อมูล user
        // ถ้าไม่สำเร็จ จะเป็น undefined
        let userFromWebAPI = await getUserProfile(token);

    }, []);

    return (
        <div>
        </div>
    )
}

```

## 5. สร้างกลไกการแสดง email ผู้ใช้ที่ login 

หลังจากได้ข้อมูลมาจาก Web API แล้ว เราจะ render `HomePage` ใหม่ โดยใช้ `useState` hook

```jsx
// src/pages/HomePage.js


import React, { useEffect,useState } from 'react'
import { useSelector } from 'react-redux';
import useAsyncEffect from 'use-async-effect'
import { getUserProfile } from '../services/WebAPI';

export default function HomePage() {


    const token = useSelector(state => state.token);

    // สร้างตัวแปร state ชื่อ user และกำหนดค่าเริ่มต้นเป็น null
    const [user, setUser] = useState(null);

    useAsyncEffect(async () => {

        let userFromWebAPI = await getUserProfile(token);

        // ถ้าได้ข้อมูลมา ก็กำหนดให้กับตัวแปร user ผ่าน hook function
        setUser(userFromWebAPI);

    }, []);

    return (
        <div>
            {/* นำตัวแปร user มาแสดงข้อมูล email ถ้า user ไม่มีค่าเป็น null หรือ undefined */}
            <p>{user?.email}</p>
        </div>
    )
}

```