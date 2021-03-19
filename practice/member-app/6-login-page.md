
# สร้าง Login Page

## 1. สร้างไฟล์ LoginPage.js

```jsx
// src/pages/LoginPage.js

import React from 'react'

export default function LoginPage() {
    return (
        <div>
            Login Page
        </div>
    )
}

```

## 2. กำหนด Login Page ใน Router เป็น /login

เราจะนำ LoginPage Component มาวางใน Router และกำหนด path เป็น `/login`

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

// import LoginPage component
import LoginPage from './pages/LoginPage';


function App() {
  return (
    <div>
      <Router>
        <Switch>
          <Route path="/" exact>
            <Main/>
          </Route>
          {/* กำหนด route path ให้ LoginPage */}
          <Route path="/login" exact>
            <LoginPage/>
          </Route>
        </Switch>
      </Router>
    </div>
  );
}

export default App;

```