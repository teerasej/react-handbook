
# ประกาศค่าเริ่มต้นของ initial state และอัพเดตตอนได้รับ action 

## 1. ประกาศค่าเริ่มต้นของรายการสินค้าใน initial state

โดยการกำหนดชื่อเป็น `items`

```js
// src/redux/reducer.js

const initialState = {
    items: []  
}
```

## 2. เพิ่มค่าใน items เมื่อได้รับข้อมูลจาก action 

ในที่นี้ action ที่ชื่อ `Action.CREATE_NEW_ITEM` จะเพิ่มข้อมูลเข้าไปใน `state.items`

```js
case Action.CREATE_NEW_ITEM:
    console.log('Creating new item... ')
    console.log(payload)
    return { 
        ...state, 
        items: [
            ...state.items, 
            payload
        ] 
    }
```

## ไฟล์เต็ม `src/redux/reducer.js`

```js
import Action from './action' 

const initialState = {
    items: []
}

export default (state = initialState, { type, payload }) => {
    switch (type) {

    case Action.CREATE_NEW_ITEM:
        console.log('Creating new item... ')
        console.log(payload)
        return { 
            ...state, 
            items: [
                ...state.items, 
                payload
            ] 
        }

    default:
        return state
    }
}

```