
# ติดตั้ง Redux เข้ากับโปรเจค

Redux ประกอบไปด้วย 3 ส่วนที่ต้องสร้างขึ้นมา เพื่อให้ทำงานสอดประสานกันเป็นหนึ่งเดียว เหมือนทีมฟุตบอล หรือทีมเกมส์​ MOBA ต้องมีทั้งรุก รับ support มีฝ่ายใดฝ่ายหนึ่งไม่ได้ 

3 ฝ่ายหลักคือ 
1. กลุ่ม Actions (type และ action object)
2. กลุ่ม Reducer
3. กลุ่ม Store

![Paper React   React Native 27](https://user-images.githubusercontent.com/85179/63178797-f921ec00-c074-11e9-9781-48541785d151.png)

## 1. สร้าง action type และ action object สำหรับตอนที่มีการกดเลือกหมุดบนแผนที่

Action ส่วนใหญ่จะถูกส่งจาก Component มายัง reducer

สร้างไฟล์ `src/redux/action.js`

```js
export default {
    SHOW_BRANCH_DATA: "SHOW_BRANCH_DATA"
}
```

### Action

_เปรียบได้คล้ายๆ กับ Event ใน MVC แต่ Action จะวิ่งระหว่างส่วนต่างๆ ของ Redux_

ไฟล์กลุ่มแรกคือ **Actions** จะมีส่วนประกอบใหญ่ๆ อยู่ 2 ส่วนคือ 

1. **Action Type** เป็นประเภทของ Action คล้ายๆ ค่า Enum เอาไว้อ้างอิงในส่วนอื่นๆ ของ Redux
2. **Action Object** ทำหน้าที่คล้ายๆ พัศดุที่เก็บข้อมูล (ข้อมูลส่วนนี้ เรียกทั่วไปว่า payload) ตัว Action Object มักจะถูกส่งจาก Component เพื่อเดินทางไปยัง Reducer

จากไฟล์ตรงนี้ เรากำหนดประเภท Action ในแอพเราขึ้นมา 1 อัน นั่นคือ**ตอนที่เรากดเลือกหมุดในแผนที่ จะมีการแสดงข้อมูลขึ้นมาใน Chart** นั่นเอง

## 2. สร้าง Reducer สำหรับหน้า Dashboard

สร้างไฟล์ `src/redux/reducer.js`

ใช่้ snippet `rxreducer` ได้

แล้ว import `./action` เข้ามา เพื่อกำหนด Action Type เป็น 1 เคสของ Reducer

```js
import Action from './action' 

const initialState = {

}

export default (state = initialState, { type, payload }) => {
    switch (type) {

    case Action.SHOW_BRANCH_DATA: {
        return { ...state, ...payload }
    }

    default:
        return state
    }
}
```

### Reducer

_เปรียบได้คล้ายๆ กับ Controller ใน MVC แต่ Reducer จะส่ง State ให้กับ Component_

_มองว่า State ตอนนี้ ถูกใช้แทน Model ของ MVC ก็ได้_

Reducer เป็นส่วนที่จะจัดการรับ Action ที่ส่งมาจากส่วนต่างๆ ของ Redux, ทำตาม business logic และอัพเดต State กลับไปที่ Component ที่ใช้งาน

## 3. โหลดข้อมูลจาก model กำหนดให้เป็นข้อมูลสาขาเริ่มต้น

import **BranchModel** มากำหนดเป็นค่าเริ่มต้นของ `initialState` 

```js
import Action from "./action";
import BranchModel from "../models/branchModel";

const initialState = {
    branches: BranchModel.branches
}

export default (state = initialState, { type, payload }) => {
    switch (type) {

    case Action.SHOW_BRANCH_DATA: {
        return { ...state, ...payload }
    }
        
    default:
        return state
    }
}

```

reducer ต้องการ ข้อมูลเริ่มต้น สำหรับใช้ส่งไปให้ Component ต่างๆ เสมอ เรามักเรียกส่วนนี้ว่า **initialState**

**initialState** ก็คือ object ตัวหนึ่งที่ reducer จะส่งไปให้กับ Component ต่างๆ ตอนเริ่มทำงานนั่นเอง

เราสามารถกำหนดอะไรลงไปใน **initialState** ก็ได้ เหมือนเป็น object ของ JavaScript ทั่วไป

## 4. สร้าง Redux store สำหรับใช้กับ App component

![Paper React   React Native 29](https://user-images.githubusercontent.com/85179/63178875-1b1b6e80-c075-11e9-82a6-d187cfcc7606.png)

เปิดไฟล์ `src/index.js`

```js

// เริ่มจาก import `Provider` component จาก `react-redux` 
import { Provider } from 'react-redux';
// เริ่มจาก import `createStore()` function จาก `redux` 
import { createStore } from 'redux';
// import reducer จาก module ที่สร้างไว้
import reducer from './redux/reducer'

// กำหนด reducer ในการสร้าง store
const store = createStore(reducer)

ReactDOM.render(
  <React.StrictMode>
    {/* ครอบ store ด้วย Provider component */}
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>,
  document.getElementById('root')
);
```

### Store

ไฟล์ส่วนที่รวมการทำงานของทุกส่วนใน Redux เข้าด้วยกัน ให้มองว่าเป็น Global Setting หรือ main board ใน computer ก็ได้ 

ในที่นี้สร้างเป็นไฟล์แยก เพื่อการปรับแต่งการทำงานได้ง่าย



### ไฟล์เต็ม index.js

```js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';

import { Provider } from 'react-redux';
import { createStore } from 'redux';
import reducer from './redux/reducer'

const store = createStore(reducer)

ReactDOM.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>,
  document.getElementById('root')
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();

```

## 7. จับคู่ค่า state ที่จะส่งออกมาจาก Redux Store 

![Paper React   React Native 28](https://user-images.githubusercontent.com/85179/63178859-15258d80-c075-11e9-9a0b-359a3743f06c.png)

Function component จะสามารถเข้าถึง Redux store ได้ ผ่าน React Hook ที่ Redux ได้เตรียมไว้ให้

Component ที่มีการเข้าถึง Redux พวกนี้ เรียกในอีกชื่อว่า **Redux Container** หรือ **Component Container** ครับ

> ซึ่งการที่เราจะใช้ React Hook เข้าถึง Redux Store ได้ ต้องมีการครอบ Component ทั้งหมด ด้วย store ผ่าน Provider ซะก่อน ตามขั้นตอนที่แล้ว

เริ่มจาก `src/components/MapBranch.js`

```jsx
import { useSelector } from 'react-redux'

const branches = useSelector(state => state.branches);
console.log('braches: ' + branches);
```

## 8. เรียกใช้งาน Redux state ที่ได้มาจาก `useSelector()`


```js
const handleApiLoaded = (map, maps) => {
        let bounds = new maps.LatLngBounds();

        // สลับมาใช้ตัว branch ที่ได้มาจาก Redux store ผ่านการเรียกใช้ useSelector()
        branches.forEach(branch => {
            new maps.Marker({
                position: branch.position,
                map,
                title: branch.name
            });


            bounds.extend(branch.position);
        });

        map.fitBounds(bounds);
    }
```

### ไฟล์เต็ม MapBranch.js

```jsx
import React from 'react'
import GoogleMapReact from 'google-map-react';
import { useSelector } from 'react-redux'

export default function MapBranch({
    center = {
        lat: 13.7200452,
        lng: 100.5135078
    },
    zoom = 15
}) {

    const branches = useSelector(state => state.branches);
    console.log('braches: ' + branches);

    const handleApiLoaded = (map, maps) => {
        let bounds = new maps.LatLngBounds();

        branches.forEach(branch => {
            let marker = new maps.Marker({
                position: branch.position,
                map,
                title: branch.name
            });

            bounds.extend(branch.position);
        });

        map.fitBounds(bounds);
    }

    return (
        <div style={{
            height: '100vh',
            width: '100%'
        }}>
            <GoogleMapReact
                bootstrapURLKeys={{
                    key: 'AIzaSyBDqlW1EIlePcA48oLVV_kYQJXm9dQ75uw'
                }}
                defaultCenter={center}
                defaultZoom={zoom}

                yesIWantToUseGoogleMapApiInternals
                onGoogleApiLoaded={({ map, maps }) => handleApiLoaded(map, maps)}
            >

            </GoogleMapReact>
        </div>
    )
}

```