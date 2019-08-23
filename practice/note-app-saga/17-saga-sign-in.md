# 17. สร้าง Saga สำหรับ Sign In Operation

## 1. เพิ่ม action ในโปรเจค

เราจะทำการสร้าง action สำหรับใช้ในกลไกการ Sign in อีก 2 อันดังนี้ 

เปิดไฟล์ `src/redux/actions.js`

```js
const ActionTypes = {
    SAVE_NEW_NOTE: "SAVE_NEW_NOTE",
    SIGN_IN_START: "SIGN_IN_START",
    SIGN_IN_SUCCESS: "SIGN_IN_SUCCESS",
    SIGN_IN_FAILED: "SIGN_IN_FAILED"
}

const saveNewNote = (message) => ({
    type: ActionTypes.SAVE_NEW_NOTE,
    payload: message
})

const startSignIn = (authInfo) => ({
    type: ActionTypes.SIGN_IN_START,
    payload: authInfo
})

export default {
    saveNewNote,
    ActionTypes,
    startSignIn
}

```

## 2. สร้าง watcher และ worker สำหรับ signIn Saga

สร้างไฟล์ `src/redux/sagas/signin.saga.js`

```js
import { takeEvery, put } from "redux-saga/effects";
import actions from "../actions";

function* watchIncrementAsync(action) {
    yield takeEvery(actions.ActionTypes.SIGN_IN_START, doSignIn)
}

function* doSignIn(action) {

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
}

export default watchIncrementAsync; 
```

## 3. เพิ่ม signIn Saga เข้า root Saga

เปิดไฟล์ `src/redux/sagas/sagas.js`

```js
import { all } from 'redux-saga/effects'
import incrementAsync from './increment.saga';
import helloWorld from "./hello.saga";
import signIn from "./signin.saga";

export default function* rootSaga() {
    yield all([
        signIn(),
        helloWorld(),
        incrementAsync()
    ])
```

## 4. ส่ง action เข้า store เพื่อ activate SignIn Saga

เปิดไฟล์ `src/pages/login-page/LoginPage.js`

```js
handleSubmit = e => {
        e.preventDefault();

        this.props.form.validateFields((err, values) => {
            if (!err) {
                //...

                this.props.startSignIn({ username: username, password: password });
                // this.props.passLogin();
                // this.props.activateIncrementSaga();
            }
        });
    }
```

## 5. เพิ่ม case ใน reducer เพื่อรับ token ที่ได้จากการ login มาใช้ใน state

เปิดไฟล์ `src/redux/homepage.reducer.js`

```js
    case Actions.ActionTypes.SIGN_IN_SUCCESS: {
        return {
            ...state, 
            token: payload.token
        }
    }

    default:
        return state
}
```

## 6. ทดสอบ sign in saga

1. ดาวน์โหลดโปรเจค node web api project จากที่นี่
2. แตก zip 
3. เปิด Terminal ขึ้นมาในโฟลเดอร์โปรเจค
4. รันคำสั่ง `npm i` เพื่อติดตั้ง node module
5. รันคำสั่ง `node index` เพื่อรัน web server
6. ทดสอบใช้งาน login ด้วยข้อมูลต่อไปนี้

- username: a
- password: pass