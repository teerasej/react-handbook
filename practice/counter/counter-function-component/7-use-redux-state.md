
# เข้าถึงค่า redux state จากภายใน component

## 1. กำหนดค่าเริ่มต้นใน Redux state

```jsx
// src/redux/reducer.js

// กำหนดค่าให้กับ redux state object เริ่มต้น ค่านี้จะถูกส่งไป component ทุกตัวที่ต่อกับ redux store ผ่าน mapStateToProps()
const initialState = {
    count: 0
}

export default (state = initialState, { type, payload }) => {
    switch (type) {

    case '':
        return { ...state }

    default:
        return state
    }
}


```

## 2. เรียกใช้ค่าจาก Redux state object ใน Function component ด้วย useSelector


```jsx
import React from 'react'
// import React hook ชื่อ useSelector 
import {useSelector} from 'react-redux'

export default function counter() {

    // รับค่า Redux state ผ่าน useSelector()
    // ซึ่งเราสามารถ return ค่าที่ต้องการจาก Redux state ออกมาใช้งานได้
    const count = useSelector(state => state.count)

    return (
        <div>
            <h1>{count}</h1>
        </div>
    )
}
```