
# ส่งข้อมูล Form ไปที่ Web API ด้วย Axios

```jsx
//..
import Axios from 'axios';

export default function NewItemForm() {

    //...

    const onFinish = async (values) => {
        console.log(values);

        // ใช้ post function ส่ง object ที่ได้จาก form ไปที่ Web API ไปที่ Web API
        await Axios.post('http://localhost:3010/notes', values)

        dispatch({ 
            type: Action.CREATE_NEW_ITEM,
            payload: values
        })
        history.goBack();
    };
```