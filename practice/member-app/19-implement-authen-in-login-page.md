
# สร้างกลไกการ authen และแชร์ token ใน component ต่างๆ ผ่าน redux

## 1. สร้าง function ที่จะทำการ authen ผู้ใช้ และจัดการ token

```jsx
// src/pages/LoginPage.js

import React from 'react'
import { Button, TextField } from '@material-ui/core'
import {
    Link
  } from "react-router-dom";
import * as yup from 'yup';
import { useFormik } from 'formik';

export default function LoginPage() {

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
            console.log('Login Form:', jsonFormData);
            
            // เรียกใช้ function เพื่อทำการ authen
            onAuthentication(jsonFormData)
        },
    });

    // function รับข้อมูลจาก form 
    const onAuthentication = (jsonFormData) => {

    }


    return (

        <div>
            <h1>Login: </h1>
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
                        Sign in
                    </Button>
                </div>
                <div>
                    <Button component={Link} to={'/signup'}>
                        Create Account?
                    </Button>
                </div>
            </form>
        </div>
    )
}

```

## 2. Dispatch action object เข้า reducer โดยใส่ token ไปในชื่อของ payload ด้วย

```jsx
// src/pages/LoginPage.js


import React from 'react'
import { Button, TextField } from '@material-ui/core'
import {
    Link
  } from "react-router-dom";
import * as yup from 'yup';
import { useFormik } from 'formik';

// import useDispatch hook เพื่อใช้ในการ dispatch action object เข้า reducer
import { useDispatch } from 'react-redux'
// import ชื่อ action มากำหนดเป็นค่า type property ใน action object
import { actions } from '../redux/actions';

export default function LoginPage() {

    // เรียกใช้ dispatch function จาก useDispatch() hook
    const dispatch = useDispatch();

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
            console.log('Login Form:', jsonFormData);
            
            onAuthentication(jsonFormData)
        },
    });

    const onAuthentication = (jsonFormData) => {
        
        // Authen with Web API


        // dispatch action object (javascript object ธรรมดาที่มี property ชื่อ type และ payload)
        // กำหนด type เป็น actions.LOGIN_SUCCESS
        // กำหนด payload เป็นค่า token 
        dispatch({
            type: actions.LOGIN_SUCCESS,
            payload: '1234'
        })
    }


    return (

        <div>
            <h1>Login: </h1>
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
                        Sign in
                    </Button>
                </div>
                <div>
                    <Button component={Link} to={'/signup'}>
                        Create Account?
                    </Button>
                </div>
            </form>
        </div>
    )
}

```


## 3. ใช้ Router useHistory ในการเปิดกลับไปหน้า Main

```jsx
// src/pages/LoginPage.js


// ...
// import useHistory Hook
import {
    Link, useHistory
  } from "react-router-dom";
// ...

export default function LoginPage() {

    // เรียกใช้ history object 
    const history = useHistory();

    //...

    const onAuthentication = (jsonFormData) => {
        
        // Authen with Web API


        /
        dispatch({
            type: actions.LOGIN_SUCCESS,
            payload: '1234'
        })

        // เสร็จสิ้นกระบวนการ กลับไปหน้า home
        history.push('/');
    }
```

## 4. เช็ค token จาก redux ถ้ามี token แปลว่า login แล้ว ให้แสดงเป็นหน้า Home

```jsx
// src/components/Main.js


import React, { useState, useEffect} from 'react'
import { CircularProgress } from "@material-ui/core";
import { useHistory } from 'react-router';

// เรียกใช้ useSelector hook ของ redux
import { useSelector } from 'react-redux';

export default function Main() {

    const [onLoading, setOnLoading] = useState(true);
    const history = useHistory();
    // useSelector hook จะรับ state ของ redux เข้ามา (ที่ถูก return จาก reducer)
    // เราสามารถเลือก property ของ state object ว่าจะดึงค่าอะไรออกมาใช้เก็บในตัวแปร
    // ในที่นี้เราดึงค่า state.token
    const token = useSelector(state => state.token);

    let alreadyLogin = false;


    useEffect(() => {
        // check token ว่ามีในระบบไหม ถ้ามีถือว่า login แล้ว
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
            <div>Home</div>
        );
    }
}

```