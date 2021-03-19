
# 9. สร้างหน้าลงทะเบียน และกำหนด Router path

## 1. สร้างไฟล์ SignupPage.js

```jsx
// src/pages/SignupPage.js


import React from 'react'
import { Button, TextField } from '@material-ui/core'

export default function SignupPage() {
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

## 2. กำหนด Path ใน Router

```jsx
// src/App.js

import logo from './logo.svg';
import './App.css';
import Main from './components/Main';
import {
  BrowserRouter as Router,
  Switch,
  Route
} from "react-router-dom";
import LoginPage from './pages/LoginPage';

// import หน้าลงทะเบียน
import SignupPage from './pages/SignupPage';


function App() {
  return (
    <div>
      <Router>
        <Switch>
        
          <Route path="/" exact>
            <Main/>
          </Route>
          <Route path="/login" exact>
            <LoginPage/>
          </Route>
          {/* กำหนด path ให้หน้าลงทะเบียน */}
          <Route path="/signup" exact>
            <SignupPage/>
          </Route>
        </Switch>
      </Router>
    </div>
  );
}

export default App;

```

## 3. สร้าง link เปิดไปหน้าลงทะเบียน จากหน้า Login 

ในที่นี้ใช้การกำหนด Component ของ `react-router-dom` เข้ากับ `material-ui`

- [การใช้ React-router component ใน Button](https://material-ui.com/components/buttons/) 

```jsx
// src/pages/LoginPage.js

import React from 'react'
import { Button, TextField } from '@material-ui/core'

// เรียกใช้ Link component ของ react-router-dom
import {
    Link
  } from "react-router-dom";

export default function LoginPage() {
    return (

        <div>
            <h1>Login: </h1>
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
                        Sign in
                    </Button>
                </div>
                <div>
                    {/* กำหนด Link component เข้าไปใน Button และกำหนด to props เป็น path ของหน้าลงทะเบียน */}
                    <Button component={Link} to={'/signup'}>
                        Create Account?
                    </Button>
                </div>
            </form>
        </div>
    )
}

```