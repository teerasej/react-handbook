
# สร้าง reducer ใน redux 

- โดยมีการเทียบ Type ของ Action ถ้าเป็น login_success หมายความว่าจะมี token มากับ action ด้วย 
- ในที่นี้กำหนดให้ token อยู่ในตัวแปร payload

```jsx
// src/redux/reducer.js

import { actions } from "./actions";

// ค่า state ของ redux เริ่มต้น เจ้า object นี้จะถูกส่งให้กับ component ตอนเริ่มการทำงานของ app 
const initialState = {
    
}

// function ที่จะรับ action object ที่ส่งมาจาก redux store
export default (state = initialState, { type, payload }) => {
    switch (type) {
    
    // เทียบ type ของ action ถ้าตรง ก็เข้าเคสนี้
    case actions.LOGIN_SUCCESS:
        // ถ้าเป็น action LOGIN_SUCCESS เราคาดว่า payload จะมีค่า token เพื่อเอามาเก็บไว้ใน state object
        return { ...state, token: payload }

    default:
        return state
    }
}


```