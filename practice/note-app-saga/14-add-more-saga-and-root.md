# 14. สร้าง Saga เพิ่มเติม และรวมเข้าเป็น root saga

## 1. ลองสร้าง saga watcher กับ saga worker

เปิดไฟล์ `src/redux/sagas/sagas.js`

และสร้าง generator function ขึ้นมา 2 ตัว โดยให้สั่งเกตการใช้งาน keyword `yield` ให้ดี

```js
// พวกนี้เรียกว่า effect เป็นกลไก function ที่ saga สร้างขึ้นมาให้เราเลือกใช้ใน generator function
import { all, delay, call, put, takeEvery, takeLatest } from 'redux-saga/effects'


// พวกนี้เรียกว่า watcher จะเป็นตัวจับ action type และส่งต่อให้ function ที่เป็นพวก workers
function* watchIncrementAsync() {
    yield takeEvery('INCREMENT_ASYNC', incrementAsync)
}

// พวกนี้เรียกว่า worker เป็น function ที่จะถูกเรียกจาก watcher อีกที
function* incrementAsync() {

    // สั่งให้ delay 1 วินาที
    yield delay(1000)

    // put เป็นการส่ง action ต่อเข้า Redux store 
    yield put({ type: 'INCREMENT' })
}
```

## 2. export saga ทุกตัวเป็น root saga

ในทำนองเดียวกับ Reducer พวก Saga สามารถ export รวมเป็นอันเดียวกันได้ 

เช่นตัวอย่างด้านล่าง จะรวมทั้ง `helloSaga()` และ `watchIncrementAsync()` ออกไปใช้งาน

```js
export default function* rootSaga() {
    yield all([
        helloSaga(),
        watchIncrementAsync()
    ])
} 
```

## ไฟล์เต็ม `src/redux/sagas/sagas.js`

```js
import { all, delay, call, put, takeEvery, takeLatest } from 'redux-saga/effects'


export function* helloSaga() {
    console.log('Hi Sagas');
}

function* watchIncrementAsync() {
    yield takeEvery('INCREMENT_ASYNC', incrementAsync)
}

function* incrementAsync() {
    yield delay(1000)
    yield put({ type: 'INCREMENT' })
}

export default function* rootSaga() {
    yield all([
        helloSaga(),
        watchIncrementAsync()
    ])
}
```

## 3. นำ Root Saga ไปใช้กับ Middleware แทน

เปิดไฟล์ `src/redux/store.js`

เราจะนำ rootSaga มาใช้แทน helloSaga 

```js
import rootSaga from './sagas/sagas';

//...

sagaMiddleware.run(rootSaga)
```


## ไฟล์เต็ม `src/redux/store.js`

```js
import { createStore, applyMiddleware, compose } from 'redux';
import { logger } from 'redux-logger';
import { routerMiddleware } from 'connected-react-router'
import { createBrowserHistory } from 'history'
import createRootReducer from "./root.reducer";

import createSagaMiddleware from 'redux-saga'
import rootSaga from './sagas/sagas';

const sagaMiddleware = createSagaMiddleware()

export const history = createBrowserHistory()


export default function configureStore() {
    const store = createStore(
        createRootReducer(history),
        compose(
            applyMiddleware(
                routerMiddleware(history),
                sagaMiddleware,
                logger
            ),
        ),
    );

    sagaMiddleware.run(rootSaga)

    return store;
}

```


