
# 13. ติดตั้ง และลองใช้งาน Redux Saga

## 1. ติดตั้ง Redux Saga

```bash
npm i redux-saga
```

หรือ

```bash
yarn add redux-saga
```

## 2. ทดสอบสร้าง Saga แบบง่ายๆ 

สร้างไฟล์ `src/redux/sagas/sagas.js`

```js
import { call, put, takeEvery, takeLatest } from 'redux-saga/effects'

export function* helloSaga() {
    console.log('Hi Sagas');
} 
```

- สังเกตว่า เราเขียน `function` เป็น `function*` ทำให้ฟังก์ชั่นนี้ถูกเรียกว่า **Generator function**
- Generator function จะสามารถใช้ keyword พิเศษที่ชื่อ `yield` ซึ่งจะเป็นกลไกสำคัญของการใช้งาน Saga 

เดี๋ยวจะมีตัวอย่าง ตอนนี้ setup ก่อนแล้วกัน

## 3. เพิ่ม Saga เป็น Middleware ใน Store 

เปิดไฟล์ `src/redux/store.js`

redux saga ก็จะมี middleware เหมือน library อื่นๆ เราสามารถสร้าง และเพิ่มลงใน store ได้

เพราะเราต้องการให้ Store ส่งผ่าน Action เข้า redux saga 

```js
import createSagaMiddleware from 'redux-saga'

//...

const sagaMiddleware = createSagaMiddleware()

//...

const store = createStore(
        createRootReducer(history),
        compose(
            applyMiddleware(
                routerMiddleware(history),
                sagaMiddleware,
                logger
            ),
        ),
```

จากนั้นให้เราเรียกใช้ sagaMiddleware รัน saga module ที่เราเขียนไว้ก่อนหน้านี้ 

```js
import { helloSaga } from './sagas/sagas';

//...

sagaMiddleware.run(helloSaga)
```
## ไฟล์เต็ม `src/redux/store.js`

```js
import { createStore, applyMiddleware, compose } from 'redux';
import { logger } from 'redux-logger';
import { routerMiddleware } from 'connected-react-router'
import { createBrowserHistory } from 'history'
import createRootReducer from "./root.reducer";

import createSagaMiddleware from 'redux-saga'
import { helloSaga } from './sagas/sagas';

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

    sagaMiddleware.run(helloSaga)

    return store;
}

```
