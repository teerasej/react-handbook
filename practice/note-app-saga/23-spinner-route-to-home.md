# 23. เปิดไปยังหน้า home เมื่อเสร็จกระบวนการ

ในที่นี้เราจะ import `push` module ของ `connected-react-router` มาใช้ใน saga 

เปิดไฟล์ `src/redux/sagas/signin.saga.js`

```js
import { takeEvery, put } from "redux-saga/effects";
import actions from "../actions";
import { push } from 'connected-react-router'

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
    yield put(push('/home'));
}

export default watchIncrementAsync;

```