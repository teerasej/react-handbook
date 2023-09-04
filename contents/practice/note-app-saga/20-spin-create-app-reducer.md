# 20. สร้าง app reducer เพื่อกำหนด state

## 1. สร้าง app.reducer.js

สร้างไฟล์ `src/redux/app.reducer.js`

```js
import Actions from '../redux/actions'

const initialState = {
    loading: false
}

export default (state = initialState, { type, payload }) => {
    switch (type) {

    default:
        return state
    }
}
```

## 2. นำ app.reducer ไปใส่ใน root reducer

เปิดไฟล์ `src/redux/root.reducer.js`

```js
import { combineReducers } from 'redux'
import { connectRouter } from 'connected-react-router'
import homepageReducer from "./homepage.reducer";
import appReducer from './app.reducer';

export default (history) => combineReducers({
  router: connectRouter(history),
  app: appReducer,
  homepage: homepageReducer
})  
```

สังเกตว่าเราสามารถตั้งชื่อ reducer แต่ละตัวได้ และชื่อนี้แหละจะเป็นชื่อของ state จาก reducer แต่ละตัว

