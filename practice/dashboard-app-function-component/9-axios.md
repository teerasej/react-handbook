
# ใช้ Axios จัดการข้อมูล JSON

## 1. ติดตั้ง Axios 

```bash
npm i axios
```

หรือ 

```bash
yarn add axios
```

## 2. ทำการกำหนดค่าพื้นฐานของ Axios ไว้ใช้งาน

สร้างไฟล์ `src/redux/action.js`

```js
import axios from "axios";

//..

const axiosClient = axios.create({
    responseType: "json"
});
```

## 3. ใช้ axios ในดึงข้อมูล json

```js
// src/App.js

//..
useEffect(() => {

    const requestInitData = async (dispatch) => {
      try {
        // ใช้ Axios เข้าถึงข้อมูล JSON แทน
        const success = await axiosClient.get('./branch.json');
        console.log(success.data);
      } catch (error) {
        console.error(error);
      }
    }

    requestInitData()
  })
```

## 4. เพิ่ม Action type สำหรับแสดงสถานะผลการเรียกข้อมูล

```js
// src/redux/action.js

export default {
    SHOW_BRANCH_DATA: "SHOW_BRANCH_DATA",
    REQUEST_INIT_DATA: "REQUEST_INIT_DATA",

    // ในกรณีที่ได้ข้อมูลมาอย่างไม่มีปัญหา
    REQUEST_INIT_DATA_SUCCESS: "REQUEST_INIT_DATA_SUCCESS",

    // ในกรณีที่มีการผิดพลาดในการเรียกข้อมูล
    REQUEST_INIT_DATA_FAILED: "REQUEST_INIT_DATA_FAILED",
}
```

จากจุดนี้ให้ทำการ dispatch action type ที่เหมาะสมจาก App component

```js
// src/App.js
const requestInitData = () => {
    return async (dispatch) => {
        
        try {
            const success = await API.get('./branch.json');
            console.log(success.data);

            // dispatch action พร้อมข้อมูลที่ได้เข้า Redux store
            dispatch({ 
                type: ActionTypes.REQUEST_INIT_DATA_SUCCESS, 
                payload: success.data 
            })
        } catch (error) {
            // dispatch action พร้อม error ทีไ่ด้เข้า redux store
            dispatch({ 
                type: ActionTypes.REQUEST_INIT_DATA_FAILED,
                payload: error 
            })
        }
    }
}

```

## 5. สร้างกลไกแจ้งเตือนผลการโหลดข้อมูล

เปิดไฟล์ `src/redux/reducer.js`

เริ่มจากกำหนดค่า `initialState.app` เพื่อใช้อ้างอิงใน App Component ว่าต้องแสดง notification หรือไม่

```js
const initialState = {
    branches: BranchModel.branches,
    branchDataInChart: [['Month', 'Amount'], ['', 0]],
    app: {
        
    }
}
```

ถ้าไม่ต้องแสดงตัวแจ้งเตือน ให้กำหนด `initialState.app.notification` เป็น {}


แต่ถ้าเป็น Action ผลลัพธ์จากการโหลดข้อมูล (ทั้งสำเร็จ และไม่สำเร็จ) จะมีการกำหนดค่าให้ `app.notification`

```js
case Action.REQUEST_INIT_DATA_SUCCESS: {
    return {
        ...state,
        // กำหนดข้อมูลที่จะใช้ในกลไกสร้าง notification
        app: {
            notification: {
                message: 'Data Loaded',
                isError: false,
                isShow: true
            }
        }
    }
}

case Action.REQUEST_INIT_DATA_FAILED: {
    return { 
        ...state,
        // กำหนดข้อมูล error ที่จะใช้ในกลไกสร้าง notification
        app: {
            notification: {
                message: payload.message,
                isError: true,
                isShow: true
            }
        }
    }
}
```

### ไฟล์เต็ม reducer.js

```js
import branchModel from "../models/branchModel"
import Action from './action'


const initialState = {
    branches: branchModel.branches,
    branchDataInChart: [['Month', 'Amount'], ['', 0]],
    app: {

    }
}

export default (state = initialState, { type, payload }) => {
    switch (type) {

        case Action.SHOW_BRANCH_DATA: {

            console.log(`branchId: ${payload}`)

            let selectingBranch = state.branches.find(branch => {
                return branch.id === payload;
            })

            if (selectingBranch) {
                let finalChartData = [];

                finalChartData.push(selectingBranch.chartData.chartField);

                let chartDatas = selectingBranch.chartData.datas;

                if (chartDatas && chartDatas.length > 0) {
                    chartDatas.forEach(data => {
                        finalChartData.push([data.month, data.amount]);
                    })
                } else {
                    finalChartData.push(['', 0]);
                }


                return {
                    ...state,
                    branchDataInChart: finalChartData
                }

            } else {
                return { ...state }
            }
        }

        case Action.REQUEST_INIT_DATA_SUCCESS: {
            return {
                ...state,
                app: {
                    notification: {
                        message: 'Data Loaded',
                        isError: false,
                        isShow: true
                    }
                }
            }
        }

        case Action.REQUEST_INIT_DATA_FAILED: {
            return { 
                ...state,
                app: {
                    notification: {
                        message: payload.message,
                        isError: true,
                        isShow: true
                    }
                }
            }
        }

        default:
            return state
    }
}


```

## 6. แสดง notification โดยพิจารณาจาก state ที่ได้

เปิดไฟล์ `src/App.js` 

แล้ว import `message` ของ Ant Design เข้ามา

```js
import { Layout, Row, Col, message } from 'antd';
```

จากนั้น เรียกใช้ค่า `state.app.notification` ผ่าน React hook ที่ชื่อ `useSelector()`

```js
import { useDispatch, useSelector } from 'react-redux'

//..

const notification = useSelector(state => state.app.notification)
```


จากนั้นเรียกใช้ React hook `useEffect` สำหรับค่า notification โดยเฉพาะ

```js
const notification = useSelector(state => state.app.notification)

useEffect(() => {

    // เช็คข้อมูล notification เพื่อแจ้งเตือน
    if (notification && notification.isShow) {
        if (notification.isError) {
        message.error(notification.message);
        } else {
        message.success(notification.message);
        }
    }
// รัน use effect อีกครั้งถ้ามีการเปลี่ยนแปลงค่าของ notification
}, [notification])
```
 
### ไฟล์เต็ม App.js

```jsx
//import logo from './logo.svg';
import './App.css';
import HeaderBar from './components/HeaderBar';
import { Layout, Row, Col, message } from 'antd';
import MapBranch from './components/MapBranch';
import StatChart from './components/StatChart';

import { useEffect } from 'react'
import { useDispatch, useSelector } from 'react-redux'

import Action from './redux/action'
import axios from "axios";

const axiosClient = axios.create({
  responseType: "json"
});

const { Content, Footer } = Layout;


function App() {

  const dispatch = useDispatch();
  useEffect(() => {

    const requestInitData = async (dispatch) => {
      try {
        const success = await axiosClient.get('./branch.json');
        console.log(success.data);
        dispatch({ type: Action.REQUEST_INIT_DATA_SUCCESS, payload: success.data })
      } catch (error) {
        console.error(error);
        dispatch({ type: Action.REQUEST_INIT_DATA_FAILED, payload: error })
      }
    }

    requestInitData(dispatch)
  }, [])


  const notification = useSelector(state => state.app.notification)

  useEffect(() => {
    if (notification && notification.isShow) {
      if (notification.isError) {
        message.error(notification.message);
      } else {
        message.success(notification.message);
      }
    }
  }, [notification])

  
  

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

## 7. กำหนดข้อมูลสาขาจาก Web API ให้กับ state

เปิดไฟล์ `src/redux/reducer.js`

ก่อนอื่น เรากำหนดค่า `initialState.branches` เป็น `[]`

```js
const initialState = {
    branches: [],
    branchDataInChart: [['Month', 'Amount'], ['', 0]]
}
```

จากนั้น ในกรณีที่การโหลด JSON สมบูรณ์ ให้ส่ง payload ที่ได้ออกไปให้ Component 

```js
case Actions.ActionTypes.REQUEST_INIT_DATA_SUCCESS: {
            return {
                ...state,
                branches: payload.branches,
                app: {
                    notification: {
                        message: 'Data Loaded',
                        isError: false,
                        isShow: true
                    }
                }
            }
        }
```

### ไฟล์เต็ม dashboard.reducer.js

```js

import Actions from "./actions";
import BranchModel from "../models/branchModel";

const initialState = {
    branches: [],
    branchDataInChart: [['Month', 'Amount'], ['', 0]],
    app: {
        
    }
}

export default (state = initialState, { type, payload }) => {
    switch (type) {

        case Actions.ActionTypes.SHOW_BRANCH_DATA: {

            console.log(`branchId: ${payload}`)

            let selectingBranch = this.branches.find(branch => {
                return branch.id === payload;
            })

            if (selectingBranch) {
                let finalChartData = [];

                finalChartData.push(selectingBranch.chartData.chartField);

                let chartDatas = selectingBranch.chartData.datas;

                if (chartDatas && chartDatas.length > 0) {
                    chartDatas.forEach(data => {
                        finalChartData.push([data.month, data.amount]);
                    })
                } else {
                    finalChartData.push(['', 0]);
                }


                return {
                    ...state,
                    branchDataInChart: finalChartData
                }

            } else {
                return { ...state }
            }
        }

        case Actions.ActionTypes.REQUEST_INIT_DATA_SUCCESS: {
            return {
                ...state,
                branches: payload.branches,
                app: {
                    notification: {
                        message: 'Data Loaded',
                        isError: false,
                        isShow: true
                    }
                }
            }
        }

        case Actions.ActionTypes.REQUEST_INIT_DATA_FAILED: {
            return { 
                ...state,
                app: {
                    notification: {
                        message: payload.message,
                        isError: true,
                        isShow: true
                    }
                }
            }
        }

        default:
            return state
    }
}
```