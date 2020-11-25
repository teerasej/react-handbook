
# ติดตั้ง Redux เข้ากับโปรเจค

Redux ประกอบไปด้วย 3 ส่วนที่ต้องสร้างขึ้นมา เพื่อให้ทำงานสอดประสานกันเป็นหนึ่งเดียว เหมือนทีมฟุตบอล หรือทีมเกมส์​ MOBA ต้องมีทั้งรุก รับ support มีฝ่ายใดฝ่ายหนึ่งไม่ได้ 

3 ฝ่ายหลักคือ 
1. กลุ่ม Actions (type และ action object)
2. กลุ่ม Reducer
3. กลุ่ม Store

![Paper React   React Native 27](https://user-images.githubusercontent.com/85179/63178797-f921ec00-c074-11e9-9781-48541785d151.png)

## 1. สร้าง action type และ action object สำหรับตอนที่มีการกดเลือกหมุดบนแผนที่

Action ส่วนใหญ่จะถูกส่งจาก Component มายัง reducer

สร้างไฟล์ `src/redux/actions.js`

```js

const ActionTypes = {
    SHOW_BRANCH_DATA: "SHOW_BRANCH_DATA"
}


const showBranchData = (payload) => ({
    type: ActionTypes.SHOW_BRANCH_DATA,
    payload: payload
})

export default {
    showBranchData,
    ActionTypes
}
```

### Action

_เปรียบได้คล้ายๆ กับ Event ใน MVC แต่ Action จะวิ่งระหว่างส่วนต่างๆ ของ Redux_

ไฟล์กลุ่มแรกคือ **Actions** จะมีส่วนประกอบใหญ่ๆ อยู่ 2 ส่วนคือ 

1. **Action Type** เป็นประเภทของ Action คล้ายๆ ค่า Enum เอาไว้อ้างอิงในส่วนอื่นๆ ของ Redux
2. **Action Object** ทำหน้าที่คล้ายๆ พัศดุที่เก็บข้อมูล (ข้อมูลส่วนนี้ เรียกทั่วไปว่า payload) ตัว Action Object มักจะถูกส่งจาก Component เพื่อเดินทางไปยัง Reducer

จากไฟล์ตรงนี้ เรากำหนดประเภท Action ในแอพเราขึ้นมา 1 อัน นั่นคือ**ตอนที่เรากดเลือกหมุดในแผนที่ จะมีการแสดงข้อมูลขึ้นมาใน Chart** นั่นเอง

## 2. สร้าง Reducer สำหรับหน้า Dashboard

สร้างไฟล์ `src/redux/dashboard.reducer.js`

ใช่้ snippet `rxreducer` ได้

แล้ว import `./actions` เข้ามา เพื่อกำหนด Action Type เป็น 1 เคสของ Reducer

```js
import Actions from "./actions";

const initialState = {

}

export default (state = initialState, { type, payload }) => {
    switch (type) {

    case Actions.ActionTypes.SHOW_BRANCH_DATA: {
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
import Actions from "./actions";
import BranchModel from "../models/branchModel";

const initialState = {
    branches: BranchModel.branches
}

export default (state = initialState, { type, payload }) => {
    switch (type) {

    case Actions.ActionTypes.SHOW_BRANCH_DATA: {
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

## 4. สร้าง store สำหรับใช้ใน App

สร้างไฟล์ `src/redux/store.js`

```js

import { createStore } from 'redux';   
import dashboardReducer from "./dashboard.reducer";

export default function configureStore() {
    const store = createStore(dashboardReducer);
    return store;
} 

### Store

ไฟล์ส่วนที่รวมการทำงานของทุกส่วนใน Redux เข้าด้วยกัน ให้มองว่าเป็น Global Setting หรือ main board ใน computer ก็ได้ 

ในที่นี้สร้างเป็นไฟล์แยก เพื่อการปรับแต่งการทำงานได้ง่าย

```

## 5. จัดระเบียบ import เพราะเดี๋ยวต้อง import ของ redux เข้ามาอีก

เปิดไฟล์ `src/App.js`

```js
import React from 'react';
import logo from './logo.svg';
import './App.css';

import HeaderBar from './components/HeaderBar';
import MapBranch from './components/MapBranch';
import StatChart from './components/StatChart';

import { Layout, Menu, Row, Col } from 'antd';
const { Header, Content, Footer } = Layout;

// ...
```

## 6. นำ store ที่สร้างไว้ ในกำหนดให้กับ Provider component ที่จะส่งผ่าน store ให้กับทุก component ที่อยู่ด้านใน

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
import logo from './logo.svg';
import './App.css';

import { Provider } from 'react-redux'
import configureStore from "./redux/store";

import HeaderBar from './components/HeaderBar';
import MapBranch from './components/MapBranch';
import StatChart from './components/StatChart';

import { Layout, Menu, Row, Col } from 'antd';
const { Header, Content, Footer } = Layout;



const store = configureStore();

function App() {
  return (
    <Provider store={store}>
    <div>
      <Layout className="layout">
        <HeaderBar />
        <Content style={{
          padding: '0 50px'
        }}>
          <div
            style={{
              background: '#fff',
              padding: 24,
              minHeight: 280
            }}>
            <Row gutter={16}>
              <Col span={12}><MapBranch /></Col>
              <Col span={12}><StatChart/></Col>
            </Row>

          </div>
        </Content>
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

## 7. Mapping component เข้ากับ redux connect เพื่อสร้างเป็น component container

![Paper React   React Native 28](https://user-images.githubusercontent.com/85179/63178859-15258d80-c075-11e9-9a0b-359a3743f06c.png)

React Component ทั่วไป จะไม่มีการเชื่อมกับระบบ Redux ตั้งแต่แรก แต่เราสามารถจับมันมาตั้งค่าให้ใช้งานกับ Redux ได้

เราเรียก Component พวกนี้ว่า **Redux Container** หรือ **Component Container** ครับ

เริ่มจาก `src/components/MapBranch.js`

เรา import module ชื่อ `connect` เข้ามาก่อน 

```js
// snippet 'redux'
import { connect } from "react-redux";
```

จากนั้นเราจะย้าย `export default` จากที่ใช้กับ Class ของเรา ไปใช้กับส่วนอื่น

```js
export default class MapBranch extends Component {

// เป็น

class MapBranch extends Component {
```

ซึ่งเราจะมาเขียนในส่วนด้านล่างแทน

เป็น

```js
// snippet 'reduxmap'
const mapStateToProps = (state) => ({

})

const mapDispatchToProps = {

}


export default connect(mapStateToProps, mapDispatchToProps)(MapBranch) 
```

## 8. จับคู่ค่า state ที่จะส่งออกมาจาก store เข้ากับ props ของ component

ในที่นี้ Reducer ของเรามี initialState มีที่ข้อมูลของตำแหน่งสาขาอยู่ 

ซึ่งถ้าต้องการเอามาใช้ใน Container เราก็สามารถเขียนค่าใน `mapStateToProps` ได้แบบด้านล่าง 

**state** ที่เห็นตรงนี้คือ state ที่ reducer ส่งออกมาให้ Component ต่างๆ นั่นเอง

```js
const mapStateToProps = (state) => ({
    branches: state.branches
})
```
หรือแบบนี้ก็ได้
```js
const mapStateToProps = (state) => {
    return {
        branches: state.branches
    }
}
```

## 9. สลับมาดึงข้อมูลจาก props ของ component ที่ได้จากการ map กับ Redux store 

`mapStateToProps` เป็นส่วนที่เราเขียนดึงค่าจาก state ที่ได้ ให้กับ `props` ดังนั้นในที่นี้เราจึงสามารถดึงค่า `this.props.branches` มาใช้ใน component ได้ 

```js
handleApiLoaded(map, maps) {

    let bounds = new maps.LatLngBounds();
    let branches = this.props.branches;

    branches.forEach(branch => {
      //..
    });

    //..
  }
```

### ไฟล์เต็ม MapBranch.js

```js
import React, { Component } from 'react'
import GoogleMapReact from 'google-map-react';

import { connect } from "react-redux";

class MapBranch extends Component {

  static defaultProps = {
    // Kerry Siam Seaport Location
    center: {
      lat: 13.7200452,
      lng: 100.5135078
    },
    zoom: 15
  };

  handleApiLoaded(map, maps) {

    let bounds = new maps.LatLngBounds();
    let branches = this.props.branches;

    branches.forEach(branch => {
      new maps.Marker({
        position: branch.position,
        map,
        title: branch.name
      });

      
      bounds.extend(branch.position);
      // Alternative
      // bounds.extend(new maps.LatLng(branch.lat, branch.lng);
    });

    map.fitBounds(bounds); 
  }

  render() {
    return (
      <div style={{
        height: '100vh',
        width: '100%'
      }}>
        <GoogleMapReact
          bootstrapURLKeys={{
            key: 'AIzaSyBDqlW1EIlePcA48oLVV_kYQJXm9dQ75uw'
          }}
          defaultCenter={this.props.center}
          defaultZoom={this.props.zoom}
          yesIWantToUseGoogleMapApiInternals
          onGoogleApiLoaded={({ map, maps }) => this.handleApiLoaded(map, maps)}
        >

        </GoogleMapReact>
      </div>
    )
  }
}

const mapStateToProps = (state) => ({
  branches: state.branches
})

const mapDispatchToProps = {
  
}


export default connect(mapStateToProps, mapDispatchToProps)(MapBranch)
```