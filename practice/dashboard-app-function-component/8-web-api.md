
# สร้างกลไก Action: ดึงข้อมูล JSON

## 0. ติดตั้ง Chrome Extension

- [Chrome Extension เพื่อยกเลิก CORS ในฝั่ง Browser](https://chrome.google.com/webstore/detail/allow-control-allow-origi/nlfbmbojpeacfghkpbjhddihlkkiljbi)

## 1. ติดตั้ง redux-logger

- `redux-logger` เป็นกลไก log console ของ redux 

```bash
npm i  redux-logger
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

## 3. เรียกใช้งาน Logger ใน Redux store 

จำที่พลบอกว่า store เหมือนเป็นตัวเชื่อมทุกอย่างเข้าด้วยกันได้ไหม? 

เราจะเอา logger มาใส่ในส่วนที่เรียกว่า `applyMiddleware` ครับ

```js
// src/index.js
import { createStore, applyMiddleware } from 'redux'  
import { Provider } from 'react-redux'
import reducer from './redux/reducer'
import { logger } from 'redux-logger'

const store = createStore(
  reducer,
  applyMiddleware(logger)
)
```


## 5. สร้าง Action สำหรับดึงข้อมูล JSON 

เปิดไฟล์ `src/redux/action.js`

```js
export default {
    SHOW_BRANCH_DATA: "SHOW_BRANCH_DATA",

    // กำหนด action type ใหม่
    REQUEST_INIT_DATA: "REQUEST_INIT_DATA",
}
```


## 7. เรียก Action สำหรับดึงข้อมูล JSON ใน App Component 

เราจะมีการเรียกใช้ React hook 2 ตัวในส่วนนี้

1. `useEffect()` ของ `react` จะไว้กำหนดการทำงานตอนที่ Component เริ่มถูกโหลดขึ้นมาใช้งาน
2. `useDispatch()` ของ `react-redux` ไว้ใช้สำหรับการ dispatch action ในขั้นตอนต่อไป


### ไฟล์เต็ม App.js 

```jsx
import logo from './logo.svg';
import './App.css';
import HeaderBar from './components/HeaderBar';
import { Layout, Menu, Row, Col } from 'antd';
import MapBranch from './components/MapBranch';
import StatChart from './components/StatChart';

import { useEffect } from 'react'
import { useDispatch } from 'react-redux'

import Action from './redux/action'

const { Header, Content, Footer } = Layout;



function App() {

  const dispatch = useDispatch();

  // ในที่นี้เราเรียกใช้ useEffect เพื่อให้โค้ดในส่วน callback function นี้ทำงานตอนที่ Web browser ทำการแสดงหน้าเว็บขึ้นมา
  // การใช้ Empty array ([]) เป็น parameter ตัวที่ 2 ทำให้ callback function นี้ทำงานครั้งเดียว
  // ถ้าไม่ใส่อะไรเลย จะทำงานทุกครั้งที่ render 
  // ถ้าใส่ค่าตัวแปรลงไปใน [] จะมีการเทียบค่า ถ้าค่ามีการเปลี่ยนแปลงจะทำงานอีกครั้ง
  // https://dev.to/trentyang/replace-lifecycle-with-hooks-in-react-3d4n
  useEffect(() => {

    const requestInitData = async (dispatch) => {
      
    }

    requestInitData()
  }, [])

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
              <Col span={12}><StatChart /></Col>
            </Row>

          </div>
        </Content>
        <Footer style={{
          textAlign: 'center'
        }}>React Redux Workshop ©2012-2020 Created by Nextflow.in.th</Footer>
      </Layout>,
    </div>
  );
}

export default App;

```
