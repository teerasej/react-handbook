
# ตรวจสอบข้อมูล และปิด modal 

```js
// src/components/new-item/NewItemForm.js

// ใช้ useHistory จาก react-router-dom เพื่อควบคุม Navigator
import { useHistory } from "react-router-dom";

export default function NewItemForm() {

    const history = useHistory();

    const onFinish = values => {
        console.log(values);
        dispatch({ 
            type: Action.CREATE_NEW_ITEM,
            payload: values
        })

        // สั่งย้อนกลับไป route ก่อนหน้า
        history.goBack();
    };
}
```
