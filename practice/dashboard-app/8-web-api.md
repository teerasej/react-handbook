
# สร้างกลไก Action: ดึงข้อมูล JSON

## 1. ติดตั้ง redux-thunk และ redux-logger

- `redux-thunk` จะเป็นส่วนที่ทำงานแบบ Asynchronous ได้
- `redux-logger` เป็นกลไก log console ของ redux 

```bash
npm i redux-thunk redux-logger
```

## 2. ย้ายข้อมูลไปที่ branch.json

สร้างไฟล์ `public\branch.json`

```js
{
    "headquarterId" : 1,
    "branches": [
        {
            "id": 1,
            "name": "Silom",
            "position": {
              "lat": 13.7200452,
              "lng": 100.5135078
            },
            "chartData": {
              "chartField": ["Month","Amount"],
              "datas": [
                { "month": "February", "amount": 10392 },
                { "month": "March", "amount": 18239 },
                { "month": "April", "amount": 14290 },
                { "month": "May", "amount": 23912 },
                { "month": "June", "amount": 26167 },
                { "month": "July", "amount": 28199 }
              ]
            }
          },
          {
            "id": 2,    
            "name": "Cholburi",
            "position": {
              "lat": 13.1247584,
              "lng": 100.9133127
            },
            "chartData": {
              "chartField": ["Month","Amount"],
              "datas": [
                { "month": "February", "amount": 70032 },
                { "month": "March", "amount": 54789 },
                { "month": "April", "amount": 62789 },
                { "month": "May", "amount": 89272 },
                { "month": "June", "amount": 100328 },
                { "month": "July", "amount": 128903 }
              ]
            }
          }
    ]
} 
```

## 3. เรียกใช้งาน thunk และ logger ใน Redux store 

จำที่พลบอกว่า store เหมือนเป็นตัวเชื่อมทุกอย่างเข้าด้วยกันได้ไหม? 

เราจะเอา thunk และ logger มาใส่ในส่วนที่เรียกว่า `applyMiddleware` ครับ

```js
import { createStore, applyMiddleware } from 'redux';   
import dashboardReducer from "./dashboard.reducer";

import thunk from 'redux-thunk';
import { logger } from 'redux-logger';

export default function configureStore() {
    const store = createStore(
        dashboardReducer,
        applyMiddleware(thunk, logger)
    );
    return store;
}
```

## 4. กำหนดให้ App เป็นตัวส่ง Action เพื่อเรียกดึงข้อมูลจาก API 

เริ่มแรกเลย เราจะไปที่ `src/index.js` และย้าย Redux Provider มาไว้ที่นี่แทน

```js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import * as serviceWorker from './serviceWorker';

import {Provider} from 'react-redux'
import configureStore from "./redux/store";

const store = configureStore();

ReactDOM.render((
  <Provider store={store}><App/></Provider>
), document.getElementById('root'));

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls. Learn
// more about service workers: https://bit.ly/CRA-PWA
serviceWorker.unregister();
```

จากนั้น เราจะปรับ `App.js` ให้เป็น Class component 

```js
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

import HeaderBar from './components/HeaderBar';
import MapBranch from './components/MapBranch';
import StatChart from './components/StatChart';

import { Layout, Menu, Row, Col } from 'antd';
const { Header, Content, Footer } = Layout;

class App extends Component {
  render() {
    return (
    
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
      
    );
  }
}

export default App;

```

## 5. ทำ App Component ให้เป็น Redux container

```js
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

import { connect } from 'react-redux';

import HeaderBar from './components/HeaderBar';
import MapBranch from './components/MapBranch';
import StatChart from './components/StatChart';

import { Layout, Menu, Row, Col } from 'antd';
const { Header, Content, Footer } = Layout;

class App extends Component {
  render() {
    return (
    
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
      
    );
  }
}

const mapStateToProps = (state) => ({
  
})

const mapDispatchToProps = {
  
}


export default connect(mapStateToProps, mapDispatchToProps)(App);
```

## 6. สร้าง Action สำหรับดึงข้อมูล JSON 

เปิดไฟล์ `src/redux/actions.js`

```js
const ActionTypes = {
    SHOW_BRANCH_DATA: "SHOW_BRANCH_DATA",
    REQUEST_INIT_DATA: "REQUEST_INIT_DATA"
}


const showBranchData = (branchId) => ({
    type: ActionTypes.SHOW_BRANCH_DATA,
    payload: branchId
})

const requestInitData = () => {
    return async (dispatch) => {
        let onSuccess = (success) => {
            console.log('load data success');
            return success;
        }

        let onError = (error) => {
            console.log(`Load data error: ${error}`);
            return error;
        }

        try {
            const success = await fetch('https://www.nextflow.in.th/api/branch-info/branch.json');
            console.log(success);
            return onSuccess(success);
        } catch (error) {
            return onError(error)
        }
    }
}

export default {
    requestInitData,
    showBranchData,
    ActionTypes
}
```


## 7. เรียก Action สำหรับดึงข้อมูล JSON ใน App Component เมื่อ

เราสามารถใช้ method `componentDidMount()` ตอนที่มีการแสดง App Component ได้ 

```js
class App extends Component {

  componentDidMount() {
    this.props.initData();
  }
```

โดยเราทำการผูก action เข้ากับ props ในส่วนของ `mapDispatchToProps`

```js
const mapDispatchToProps = dispatch => {
  return {
    initData: () => dispatch(actions.requestInitData())
  }
}
```

### ไฟล์เต็ม App.js 

```js
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

import { connect } from 'react-redux';

import HeaderBar from './components/HeaderBar';
import MapBranch from './components/MapBranch';
import StatChart from './components/StatChart';

import { Layout, Menu, Row, Col } from 'antd';
import actions from './redux/actions';
const { Header, Content, Footer } = Layout;


class App extends Component {

  componentDidMount() {
    this.props.initData();
  }

  render() {
    return (
    
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
      
    );
  }
}

const mapStateToProps = (state) => ({
  
})

const mapDispatchToProps = dispatch => {
  return {
    initData: () => dispatch(actions.requestInitData())
  }
}


export default connect(mapStateToProps, mapDispatchToProps)(App);
```
