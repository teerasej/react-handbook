

# นำ formik มาใช้กับ JSX ในหน้าลงทะเบียน และหน้า Login

- [ตัวอย่างการใช้ค่าต่างๆ ของ formik กับ material-ui](https://formik.org/docs/examples/with-material-ui) 

## 1. ใช้กับหน้าลงทะเบียน

```jsx
// src/pages/SignupPage.js


import React from 'react'
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
            
        },
    });


    return (

        <div>
            <h1>Create new account: </h1>
            {/* กำหนดค่า formik เข้ากับ JSX */}
            <form onSubmit={formik.handleSubmit}>
                <div>
                    {/* กำหนดค่า formik เข้ากับ JSX */}
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
                    {/* กำหนดค่า formik เข้ากับ JSX */}
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
        </div>
    )
}

```

## 2. ใช้กับหน้า Login

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
            
        },
    });


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