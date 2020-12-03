
# dispatch ข้อมูลที่ได้จาก Web API เข้า Redux

## 1. เพิ่ม action type สำหรับตอนได้ข้อมูลมาจาก Web API

```js
// src/redux/action.js

export default {
    CREATE_NEW_ITEM: 'CREATE_NEW_ITEM',

    // เพิ่มค่า action type สำหรับตอนได้ข้อมูลมาจาก Web API
    MESSAGE_LOADED: 'MESSAGE_LOADED'
}
```

## 2. ทำการ dispatch action object เข้า Redux

ในที่นี้เราจึงมีการใช้ React Hook `useDispatch` ในการส่ง action เข้า redux

```js
// src/components/list-item/ItemList.js

import { useSelector, useDispatch } from 'react-redux'
import action from '../../redux/action';
//..

export default function ItemList() {

    //..
    const dispatch = useDispatch()

    useEffect(() => {
        const loadMessage = async () => {
            const notes = await Axios.get('http://localhost:3010/notes')
            console.log('notes loaded:', notes.data);

            // Dispatch Action เข้า redux
            dispatch({
                // กำหนด action type
                type: action.MESSAGE_LOADED,
                // กำหนดข้อมูลที่ได้จาก web api เป็น payload
                payload: notes.data
            })
        }
        loadMessage();
    }, [])
```