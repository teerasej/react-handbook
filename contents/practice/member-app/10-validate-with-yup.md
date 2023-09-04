
# เช็คความถูกต้องของข้อมูลด้วย yup

เราสามารถกำหนดเงื่อนไขการเช็คข้อมูลด้วย [yup](https://www.npmjs.com/package/yup) 

- ข้อมูลชื่อ email เป็น string 
- เช็คความถูกต้องของ email ด้วย `.email()` พร้อมข้อความเตือน 
- เช็คการกรอกข้อมูลด้วย `.required()` พร้อมข้อความเตือน

- ข้อมูลชื่อ password เป็น string
- เช็คความยาวรหัสผ่านอย่างน้อย 4 ตัว ด้วย `.min()` พร้อมข้อความเตือน
- เช็คการกรอกข้อมูลด้วย `.required()` พร้อมข้อความเตือน


```jsx
// src/pages/SignupPage.js


import React from 'react'
import { Button, TextField } from '@material-ui/core'

// import yup
import * as yup from 'yup';

export default function SignupPage() {

    // กำหนด property field ใน yup object
    // โดยการกำหนดเงื่อนไขผ่าน function ของ yup
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


    return (

        <div>
            <h1>Create new account: </h1>
            <form>
                <div>
                    <TextField
                        id="email"
                        name="email"
                        label="Email"
                    />
                </div>
                <div>
                    <TextField
                        id="password"
                        name="password"
                        label="Password"
                        type="password"
                    />
                </div>
                <div>
                    <Button variant="contained" type="submit">
                        Register
                    </Button>
                </div>
            </form>
        </div>
    )
}

```