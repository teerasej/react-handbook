

# กำหนดการทำงานของ Form ด้วย formik

กำหนดการใช้งาน formik object 

- `initialValues` กำหนดค่าเริ่มต้นของข้อมูลชื่อ email และ password
- `validationSchema` ใช้ตามที่กำหนดไว้ใน yup 
- `onSubmit` เป็น function ที่จะทำงานเมื่อมีการกดปุ่ม submit ใน form


```jsx
// src/pages/SignupPage.js


import React from 'react'
import { Button, TextField } from '@material-ui/core'
import * as yup from 'yup';

// import formik hook
import { useFormik } from 'formik';

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

    // กำหนดค่าต่างๆ ของ formik object ที่จะนำไปใช้กับ form
    const formik = useFormik({
        // กำหนดค่าเริ่มต้นของ field ต่างๆ 
        initialValues: {
            email: '',
            password: '',
        },
        // กำหนด validation schema
        validationSchema: validationSchema,
        // onSubmit ที่จะทำงานตอนปุ่ม submit ถูกกด
        onSubmit: (values) => {
            
            // แปลงข้อมูลเป็น JSON
            const jsonFormData = JSON.stringify(values);
            console.log('Login Form:', jsonFormData);
            
        },
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