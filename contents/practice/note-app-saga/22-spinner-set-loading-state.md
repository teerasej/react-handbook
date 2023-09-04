# 22. วาง App Component ให้อยู่ในสถานะ Loading และปลดสถานะออกหลังเสร็จสิ้นกระบวนการ

เปิดไฟล์ `src/redux/sagas/signin.saga.js`

และใช้ saga effect ชื่อ `put` ในการส่ง action ไปยัง reducer 

```js
yield put({ type: actions.ActionTypes.APP_LOADING_START });

//...

yield put({ type: actions.ActionTypes.APP_LOADING_END });

```

```js
import { takeEvery, put } from "redux-saga/effects";
import actions from "../actions";

function* watchIncrementAsync(action) {
    yield takeEvery(actions.ActionTypes.SIGN_IN_START, doSignIn)
}

function* doSignIn(action) {

    yield put({ type: actions.ActionTypes.APP_LOADING_START });

    const authInfo = action.payload;

    const result = yield fetch('http://localhost:3002/api/auth/signin', {
        method: 'POST',
        headers: {
            'Accept': 'application/json',
            'Content-Type': 'application/json'
        },
        body: JSON.stringify(authInfo)
    }).then(response => response.json());

    console.log(result);
    yield put({ type: actions.ActionTypes.SIGN_IN_SUCCESS, payload: result });
    yield put({ type: actions.ActionTypes.APP_LOADING_END });
}

export default watchIncrementAsync;
```