
# ทำการ dispatch Action เข้า redux store จาก component ที่ต้องการ

```jsx
import ActionButton from 'antd/lib/modal/ActionButton';
import React from 'react'

// เรียกใช้ useDispatch hook เพื่อใช้ในการ dispatch object เข้า redux 
import { useDispatch } from 'react-redux'

export default function IncreaseButton() {

    // สร้าง dispatch function
    const dispatch = useDispatch();

    const addMoreNumber = () => {

        // ส่ง object เข้าระบบ เราเรียก object แบบนี้ว่า action
        // action object จะมี type ที่ระบุชื่อของ action และ payload ที่เก็บข้อมูลที่ต้องการส่งให้ redux
       dispatch({
           type: Action.ADD_NUMBER,
           payload: 1
       })
    }

    return (
        <div>
            <button onClick={this.addMoreNumber}>เพิ่ม</button>
        </div>
    )
}
```