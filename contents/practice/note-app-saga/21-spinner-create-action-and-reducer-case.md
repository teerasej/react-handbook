# 21. สร้าง Action และ Reducer case สำหรับอัพเดตสถานะ

## สร้าง action type เพิ่ม

เปิดไฟล์ `src/redux/actions.js`

และเพิ่ม Action type ใหม่เข้าไป 2 ตัว 

```js
const ActionTypes = {
    SAVE_NEW_NOTE: "SAVE_NEW_NOTE",
    SIGN_IN_START: "SIGN_IN_START",
    SIGN_IN_SUCCESS: "SIGN_IN_SUCCESS",
    SIGN_IN_FAILED: "SIGN_IN_FAILED",

    APP_LOADING_START: "APP_LOADING_START",
    APP_LOADING_END: "APP_LOADING_END"
}
```

## 2. และไปเพิ่ม case ใน app.reducer.js เพื่อส่ง state loading 

เปิดไฟล์ `src/redux/app.reducer.js`

```js
import Actions from '../redux/actions'

const initialState = {
    loading: false
}

export default (state = initialState, { type, payload }) => {
    switch (type) {

    case Actions.ActionTypes.APP_LOADING_START: 
        return {
            ...state,
            loading: true
        }

    case Actions.ActionTypes.APP_LOADING_END: 
        return {
            ...state,
            loading: false
        }

    default:
        return state
    }
}
```