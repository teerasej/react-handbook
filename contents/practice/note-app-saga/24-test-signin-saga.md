
# 24. สร้าง Unit Test ของ Saga 

## 1. สร้างไฟล์ test saga และสร้าง unit test

สร้างไฟล์ `src/redux/sagas/signin.saga.test.js`

```js
import { testSaga } from 'redux-saga-test-plan';

describe('Test Sign In Saga', () => {



}); 
```

## 2. แก้ไขไฟล์ signin.saga.js ให้ export doSignIn function 

เพราะเราต้องเอาไปใช้ใน test

```js
// src/redux/sagas/signin.saga.js
// จาก
function* doSignIn(action) {

// เป็น
export function* doSignIn(action) {
```

และนำมาเขียน test ในไฟล์ `src/redux/sagas/signin.saga.test.js`

```js
// src/redux/sagas/signin.saga.test.js

import { testSaga } from 'redux-saga-test-plan';
import watcher, { doSignIn }  from "./signin.saga";
import actions from '../actions';

describe('Sign In Saga', () => {
   
    it('watch SignIn Saga', () => {
        testSaga(watcher, { username: 'a', password: 'pass' })
        .next()
        .takeEvery(actions.ActionTypes.SIGN_IN_START, doSignIn)
        .next()
        .isDone();
    });

});
```

ทดสอบรัน test

