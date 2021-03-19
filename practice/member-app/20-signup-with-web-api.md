
# ลงทะเบียนกับ Web API

## 1. สร้าง signup function ด้วย axios 

ส่ง email และ password ไปที่ web api และเช็คข้อมูล เพื่อ return ค่า true false ออกไปใช้งาน

เราจะสร้างไฟล์ใหม่ขึ้นมา เพื่อจัดการโค้ดที่ส่ง request และรับ response จาก Web API โดยเฉพาะ

```js
// src/services/WebAPI.js

import axios from 'axios';

// กำหนด endpoint ของ Web API ที่รัน
const endpoint = '';

// กำหนด base URL เป็น endpoint ของ Web API
const client = axios.create({
    baseURL: endpoint
})

// สร้าง async function ที่รับ json object ที่มีค่า property email และ password

export const signUp = async (jsonData) => {

    // ใช้ axios ส่ง post request ไปที่ /signup
    // ส่ง json เป็น parameter ตัวที่ 2 ไปกับ request
    // กำหนด header ที่กำหนดประเภทข้อมูลที่แนบไป เป็นแบบ json
    // สิ่งที่ server ตอบกลับมาจะเก็บไว้ในตัวแปร response
    let response = await client.post(
        '/signup',
        jsonData,
        {
            headers: {
                'Content-Type': 'application/json'
            }
        }
    );

    console.log(response);

    // ในที่นี้มองว่า server จะส่งข้อมูลกลับมาในชื่อ user ถ้าการสร้างบัญชีสมบูรณ์
    // เลยส่งค่า true ออกจาก function เพื่อแสดงว่าการสร้างบัญชีใน server สมบูรณ์
    // ถ้าไม่มี จะคืนค่า false แทน
    if(response.data.user) {
        return true;
    } else {    
        return false;
    }
}

```

## 2. เรียกใช้ function signup และเอาค่าที่ได้มากำหนด dialog

```jsx
// src/pages/SignupPage.js

//..

// import signUp function จาก Web API Service
import { signUp } from '../services/WebAPI';

export default function SignupPage() {

    //...

    const onStartRegisteration = async (jsonFormData)  => {

        // ส่งข้อมูล form ในรูปแบบ json ให้กับ function signup
        // ค่าที่ได้จาก function จะเป็น boolean true/false
        let regisResult = await signUp(jsonFormData);
        
        // กำหนดค่า boolean ให้มีผลกับกลไกการแสดง dialog
        setIsRegisterationSuccess(regisResult);
    }

    //..


```

## ไฟล์เต็ม

```jsx

import React from 'react'
import { useState } from 'react'
import { Button, TextField } from '@material-ui/core'
import { Dialog, DialogTitle, DialogContent, DialogContentText, DialogActions } from '@material-ui/core'
import * as yup from 'yup';
import { useFormik } from 'formik';
import { useHistory } from 'react-router';
import { signUp } from '../services/WebAPI';

export default function SignupPage() {

    const validationSchema = yup.object({
        email: yup
            .string()
            .email('Enter a valid email')
            .required('Email is required'),
        password: yup
            .string()
            .min(4, 'Password should be of minimum 4 characters length')
            .required('Password is required'),
    });

    const formik = useFormik({
        initialValues: {
            email: '',
            password: '',
        },
        validationSchema: validationSchema,
        onSubmit: (values) => {
            
            const jsonFormData = JSON.stringify(values);
            console.log('Sign up Form:', jsonFormData);
            onStartRegisteration(jsonFormData);
        },
    });

    const [isRegisterationSuccess, setIsRegisterationSuccess] = useState(false);
    const history = useHistory();

    const onStartRegisteration = async (jsonFormData)  => {

        // register new account
        let regisResult = await signUp(jsonFormData);
        
        setIsRegisterationSuccess(regisResult);
    }

    const onDialogClose = () => {
        history.push('/login');
    }

    return (

        <div>
            <h1>Create new account: </h1>
            <form onSubmit={formik.handleSubmit}>
                <div>
                    <TextField
                        id="email"
                        name="email"
                        label="Email"
                        value={formik.values.email}
                        onChange={formik.handleChange}
                        error={formik.touched.email && Boolean(formik.errors.email)}
                        helperText={formik.touched.email && formik.errors.email}
                    />
                </div>
                <div>
                    <TextField
                        id="password"
                        name="password"
                        label="Password"
                        type="password"
                        value={formik.values.password}
                        onChange={formik.handleChange}
                        error={formik.touched.password && Boolean(formik.errors.password)}
                        helperText={formik.touched.password && formik.errors.password}
                    />
                </div>
                <div>
                    <Button variant="contained" type="submit">
                        Register
                    </Button>
                </div>
            </form>
            <Dialog
                open={isRegisterationSuccess}
                aria-labelledby="alert-dialog-title"
                aria-describedby="alert-dialog-description"
            >
                <DialogTitle id="alert-dialog-title">Successful!</DialogTitle>
                <DialogContent>
                    <DialogContentText id="alert-dialog-description">
                        Registration Completed! Please login with your account.
                    </DialogContentText>
                </DialogContent>
                <DialogActions>
                    <Button onClick={onDialogClose} color="primary">
                        ok
                    </Button>
                </DialogActions>
            </Dialog>
        </div>
    )
}

```