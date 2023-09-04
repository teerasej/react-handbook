
# Login เข้าใช้งานผ่าน Web API

## 1. สร้าง login function ด้วย axios 

- สร้าง function login 
- ส่งข้อมูล email, password แบบ json ไป web api 
- ถ้า login ได้สมบูรณ์ จะได้ token กลับมา 
- ถ้า login ไม่ผ่านจะไม่ได้ token กลับมา


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

    // ส่งข้อมูล email และ password แบบ json ไปกับ post request
    let response = await client.post(
        '/login',
        jsonData,
        {
            headers: {
                'Content-Type': 'application/json'
            }
        }
    );

    // ดูข้อมูลที่ได้กลับจาก server ถ้าสำเร็จจะมี token ส่งกลับมาด้วย
    console.log(response);

    // ถ้ามี token กลับมา ให้ส่ง token กลับออกไปจาก function
    if(response.data.token) {
        return response.data.token;
    } else {    
        return undefined;
    }
    
}



```

## 2. ทำการ login ถ้าได้ token กลับมา ให้ส่ง token เข้า reducer

```jsx
// src/pages/LoginPage.js

//..

// import login function จาก Web API Service
import { login } from '../services/WebAPI';


export default function LoginPage() {

    //...

    const onAuthentication = async (jsonFormData) => {
        
        // ส่งข้อมูล email และ password ที่ได้จากแบบฟอร์ม ในรูปแบบ json  ให้กับ function login
        // ถ้าสำหรับจะได้ token กลับมาเก็บไว้ในตัวแปร
        // ถ้าไม่สำหรับ ตัวแปร token จะมีค่า undefined
        let token = await login(jsonFormData);


        // ถ้าได้ token กลับมา เราจะส่ง token ไปกับ action เพื่อเก็บไว้ใน redux state 
        // ด้วยวิธีนี้ component ตัวอื่นจะสามารถเข้าถึง token ได้
        if(token) {
            dispatch({
                type: actions.LOGIN_SUCCESS,
                payload: token
            })
        }
       

        history.push('/');
    }

    //..


```

## ไฟล์เต็ม

```jsx


import React from 'react'
import { Button, TextField } from '@material-ui/core'
import {
    Link, useHistory
  } from "react-router-dom";
import * as yup from 'yup';
import { useFormik } from 'formik';
import { useDispatch } from 'react-redux'
import { actions } from '../redux/actions';
import { login } from '../services/WebAPI';

export default function LoginPage() {

    const dispatch = useDispatch();
    const history = useHistory();

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

    const onAuthentication = async (jsonFormData) => {
        
        let token = await login(jsonFormData);

        if(token) {
            dispatch({
                type: actions.LOGIN_SUCCESS,
                payload: token
            })
        }

        history.push('/');
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