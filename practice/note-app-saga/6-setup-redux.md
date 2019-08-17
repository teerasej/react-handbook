
# 6. ติดตั้ง Redux เข้ากับโปรเจค

Redux ประกอบไปด้วย 3 ส่วนที่ต้องสร้างขึ้นมา เพื่อให้ทำงานสอดประสานกันเป็นหนึ่งเดียว เหมือนทีมฟุตบอล หรือทีมเกมส์​ MOBA ต้องมีทั้งรุก รับ support มีฝ่ายใดฝ่ายหนึ่งไม่ได้ 

3 ฝ่ายหลักคือ 
1. กลุ่ม Actions (type และ action object)
2. กลุ่ม Reducer
3. กลุ่ม Store

![Paper React   React Native 27](https://user-images.githubusercontent.com/85179/63178797-f921ec00-c074-11e9-9781-48541785d151.png)


## 1. สร้าง action type และ action object สำหรับตอนที่มีการกดปุ่ม Save

Action ส่วนใหญ่จะถูกส่งจาก Component มายัง reducer

สร้างไฟล์ `src/redux/actions.js`

```js
const ActionTypes = {
    SAVE_NEW_NOTE: "SAVE_NEW_NOTE"
}


const saveNewNote = (message) => ({
    type: ActionTypes.SAVE_NEW_NOTE,
    payload: message
})

export default {
    saveNewNote,
    ActionTypes
}
```

### Action

_เปรียบได้คล้ายๆ กับ Event ใน MVC แต่ Action จะวิ่งระหว่างส่วนต่างๆ ของ Redux_

ไฟล์กลุ่มแรกคือ **Actions** จะมีส่วนประกอบใหญ่ๆ อยู่ 2 ส่วนคือ 

1. **Action Type** เป็นประเภทของ Action คล้ายๆ ค่า Enum เอาไว้อ้างอิงในส่วนอื่นๆ ของ Redux
2. **Action Object** ทำหน้าที่คล้ายๆ พัศดุที่เก็บข้อมูล (ข้อมูลส่วนนี้ เรียกทั่วไปว่า payload) ตัว Action Object มักจะถูกส่งจาก Component เพื่อเดินทางไปยัง Reducer

จากไฟล์ตรงนี้ เรากำหนดประเภท Action ในแอพเราขึ้นมา 1 อัน นั่นคือ**ตอนที่เรากดปุ่ม Save เพื่อต้องการบันทึกข้อความลงใน List** นั่นเอง




## 2. สร้าง Reducer สำหรับหน้า Dashboard

สร้างไฟล์ `src/redux/homepage.reducer.js`

_ใช่้ snippet `rxreducer` ได้_

แล้ว import `./actions` เข้ามา เพื่อกำหนด Action Type เป็น 1 เคสของ Reducer

```js
const initialState = {
    notes : [
        {
            title: 'wowwwww.'
        }
    ]
}

export default (state = initialState, { type, payload }) => {
    switch (type) {

    case '':
        return { ...state, ...payload }

    default:
        return state
    }
}

```

### Reducer

_เปรียบได้คล้ายๆ กับ Controller ใน MVC แต่ Reducer จะส่ง State ให้กับ Component_

_มองว่า State ตอนนี้ ถูกใช้แทน Model ของ MVC ก็ได้_

Reducer เป็นส่วนที่จะจัดการรับ Action ที่ส่งมาจากส่วนต่างๆ ของ Redux, ทำตาม business logic และอัพเดต State กลับไปที่ Component ที่ใช้งาน

reducer ต้องการ ข้อมูลเริ่มต้น สำหรับใช้ส่งไปให้ Component ต่างๆ เสมอ เรามักเรียกส่วนนี้ว่า **initialState**

**initialState** ก็คือ object ตัวหนึ่งที่ reducer จะส่งไปให้กับ Component ต่างๆ ตอนเริ่มทำงานนั่นเอง

เราสามารถกำหนดอะไรลงไปใน **initialState** ก็ได้ เหมือนเป็น object ของ JavaScript ทั่วไป




## 3. สร้าง store สำหรับใช้ใน App

สร้างไฟล์ `src/redux/store.js`

```js

import { createStore } from 'redux'; 
import homepageReducer from './homepage.reducer';

export default function configureStore() {
    const store = createStore(homepageReducer);
    return store;
} 

```

### Store

ไฟล์ส่วนที่รวมการทำงานของทุกส่วนใน Redux เข้าด้วยกัน ให้มองว่าเป็น Global Setting หรือ main board ใน computer ก็ได้ 

ในที่นี้สร้างเป็นไฟล์แยก เพื่อการปรับแต่งการทำงานได้ง่าย


## 4. นำ store ที่สร้างไว้ ในกำหนดให้กับ Provider component ที่จะส่งผ่าน store ให้กับทุก component ที่อยู่ด้านใน

![Paper React   React Native 29](https://user-images.githubusercontent.com/85179/63178875-1b1b6e80-c075-11e9-82a6-d187cfcc7606.png)

เปิดไฟล์ `src/App.js`

เริ่มจาก import `Provider` component จาก `react-redux` และตัว store ที่เราสร้างไว้ 

```js
import { Provider } from 'react-redux'
import configureStore from "./redux/store";
```

เรียกใช้งาน `configureStore()` เพื่อสร้าง store object

```js
const store = configureStore();
```

และกำหนด `store` ที่ได้ให้กับ `<Provider>` สังเกตว่าเราจะครอบทุกส่วนของ Application 

```js
function App() {
  return (
    <Provider store={store}>
        <div>
            <Layout className="layout">
                ...
            </Layout>
        </div>
    </Provider>
  );
}
```

### ไฟล์เต็ม App.js

```js
import React from 'react';
import './App.css';
import { Provider } from 'react-redux'
import configureStore from "./redux/store";

import MenuBar from './components/MenuBar';

import { Layout } from 'antd';
import HomePage from './pages/home-page/HomePage';
const { Footer } = Layout;

const store = configureStore();

function App() {
  return (
    <Provider store={store}>
      <div>
        <Layout className="layout">
          <MenuBar />
          <HomePage />
          <Footer style={{
            textAlign: 'center'
          }}>React Redux Workshop ©2012-2019 Created by Nextflow.in.th</Footer>
        </Layout>,
      </div>
    </Provider>
  );
}

export default App;

```

