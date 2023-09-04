
# Dispatch ข้อมูลที่ได้จาก Form เข้า Redux 

## 1. Dispatch ตัว Action object 

ในที่นี้เราจะใช้ `useDispatch()` ในการส่งข้อมูลที่ได้จาก form เข้า Redux

โดยการกำหนด `type` และ `payload` ของ object ที่ต้องการ ด้วยข้อมูลที่มี

```js
// src/components/new-item/NewItemForm.js

import {useDispatch} from 'react-redux'
import Action from '../../redux/action'

//..

export default function NewItemForm() {

    const dispatch = useDispatch()

    //..

    const onFinish = values => {
        console.log(values);

        dispatch({ 
            type: Action.CREATE_NEW_ITEM,
            payload: values
        })
    };

    //..
}
```

## 2. check Action object ใน Reducer 

```js
// src/redux/reducer.js

import Action from './action' 

const initialState = {
    
}

// parameter ตัวที่ 2 ของ Reducer function จะได้รับ action ที่ส่งเข้ามาจาก Redux store
// ส่วนนี้มีการใช้ Deconstructor เพื่อแยก property ของ Action object ออกมาเป็นชื่อ type และ payload
export default (state = initialState, { type, payload }) => {
    switch (type) {

    // check เทียบ action type กับชื่อที่กำหนด
    case Action.CREATE_NEW_ITEM:
        console.log('Creating new item... ')
        // แสดง Payload ที่ได้มา
        console.log(payload)
        return { ...state, ...payload }

    default:
        return state
    }
}

```