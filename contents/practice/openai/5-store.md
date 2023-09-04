
# 5. สร้าง Redux Store

## 1. สร้างไฟล์ store

สร้างไฟล์ `src/redux/store.js`

```js
// src/redux/store.js

import { configureStore } from '@reduxjs/toolkit'

export default configureStore({
  reducer: {}
})
```

## 2. Setup Redux store กับ component

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
    {/* กำหนด Provider และ store */}
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
