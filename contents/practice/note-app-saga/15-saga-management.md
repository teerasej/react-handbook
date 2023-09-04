# 15. จัดกลุ่ม Saga เพื่อให้ดูแลได้ง่ายขึ้น

## 1. แยก HelloSaga ไปไว้ใน Module ของตัวเอง

สร้างไฟล์ `src/redux/sagas/hello.saga.js`

```js
export default function* helloSaga() {
    console.log('Hi Sagas');
}
```

## 2. แยก HelloSaga ไปไว้ใน Module ของตัวเอง

สร้างไฟล์ `src/redux/sagas/increment.saga.js`

```js
import { delay, put, takeEvery } from 'redux-saga/effects'

function* incrementAsync() {
    yield delay(1000)
    console.log('1 second pass')
    yield put({ type: 'INCREMENT' })
}

function* watchIncrementAsync() {
    yield takeEvery('INCREMENT_ASYNC', incrementAsync)
}

export default watchIncrementAsync; 
```


## 3. นำทั้ง 2 Saga มาใส่ใน root saga เหมือนเดิม

เปิดไฟล์​ `src/redux/sagas/sagas.js`

ทำการ import saga ทั้ง 2 ตัว มาใส่ไว้ใน root saga เพื่อเอาไปใช้งาน 

จัดโค้ดเอา import ที่ไม่จำเป็นออกไปด้วย 

```js
import { all } from 'redux-saga/effects'
import incrementAsync from './increment.saga';
import helloWorld from "./hello.saga";

export default function* rootSaga() {
    yield all([
        helloWorld(),
        incrementAsync()
    ])
}
```