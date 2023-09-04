
# สร้างกลไกการลงทะเบียน และแสดง Dialog เมื่อการลงทะเบียนเสร็จสมบูรณ์

```jsx
// src/pages/SignupPage.js

import React from 'react'

// import useState hook
import { useState } from 'react'

import { Button, TextField } from '@material-ui/core'
import * as yup from 'yup';
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

    const formik = useFormik({
        initialValues: {
            email: '',
            password: '',
        },
        validationSchema: validationSchema,
        onSubmit: (values) => {
            
            const jsonFormData = JSON.stringify(values);
            console.log('Sign up Form:', jsonFormData);
            
            // เริ่มการลงทะเบียน
            onStartRegisteration(jsonFormData);
        },
    });

    // สร้างตัวแปร state ที่ใช้ในการยืนยันการลงทะเบียนที่สมบูรณ์
    const [isRegisterationSuccess, setIsRegisterationSuccess] = useState(false);

    // function ที่รับข้อมูลจาก form และใช้ในการลงทะเบียน
    const onStartRegisteration = (jsonFormData:any) => {

        // register new account

        // กำหนดค่าตัวแปร isRegisterationSuccess เป็น true เพื่อแสดง Dialog
        setIsRegisterationSuccess(true);
    }

    // 
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

            {/* แสดง Dialog ตามค่าตัวแปร isRegisterationSuccess */}
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
                    {/* เรียกใช้ function onDialogClose ตอนผู้ใช้คลิกปุ่มปิด */}
                    <Button onClick={onDialogClose} color="primary">
                        ok
                    </Button>
                </DialogActions>
            </Dialog>
        </div>
    )
}

```