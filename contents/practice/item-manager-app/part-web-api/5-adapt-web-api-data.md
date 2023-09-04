
# ปรับให้ reducer และ ItemList ใช้ข้อมูลใหม่ที่มาจาก Web API

## 1. ปรับให้ reducer จัดการ payload ที่ได้จาก action MESSAGE_LOADED

```js
// src/redux/reducer.js

import Action from './action'

const initialState = {
    items: []
}

export default (state = initialState, { type, payload }) => {
    switch (type) {

        case Action.CREATE_NEW_ITEM:
            return { ...state, items: [...state.items, payload] }

        // โหลด payload เป็นข้อมูลของ property items
        case Action.MESSAGE_LOADED:
            return { ...state, items: [...payload] }

        default:
            return state
    }
}

```

