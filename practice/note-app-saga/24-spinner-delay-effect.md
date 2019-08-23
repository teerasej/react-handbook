# 24. เพิ่ม delay effect เพื่อจำลองการหน่วงเวลา signin

เปิดไฟล​์ `src/redux/sagas/signin.saga.js`

เขียนคำสั่ง import 

```js
import { takeEvery, put, delay } from "redux-saga/effects";
```

และคั่นใน saga ดังนี้ 

```js
//...
    yield put({ type: actions.ActionTypes.SIGN_IN_SUCCESS, payload: result });
    yield delay(2000);
    yield put({ type: actions.ActionTypes.APP_LOADING_END });
    yield put(push('/home'));
```