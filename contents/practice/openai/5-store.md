
# 5. สร้าง Redux Store

- [Redux for React](https://redux.js.org/basics/usage-with-react)

## 1. สร้าง Redux Store 

ให้สร้างไฟล์ `src/redux/store.js`

```jsx
// src/redux/store.js
import { configureStore } from '@reduxjs/toolkit'

// ใช้ function configureStore สร้าง store เปล่าๆ ไม่มี reducer 
export default configureStore({
  reducer: {}
})

```

## 2. นำ Redux Store มาใช้กับ App ผ่าน Provider component

เราจะทำการ import ทั้ง store ที่เตรียมไว้ และ **Provider** มาใช้กับ **App** ของเรา

```js
// src/index.js

import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';

// เรียกใช้ provider และ store
import { Provider } from 'react-redux';
import store from "./redux/store";

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    {/* ครอบ component ทั้งหมดด้วย Provider ที่มีการใส่ store ลงไปใช้งาน */}
    <Provider store={store}>
        <App />
    </Provider>
  </React.StrictMode>
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();

```
