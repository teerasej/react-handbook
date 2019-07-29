
# ใช้ Axios จัดการข้อมูล JSON

## 1. ติดตั้ง Axios 

```bash
npm i axios
```

## 2. สร้าง API module สำหรับตั้งค่าให้ axios

สร้างไฟล์ `src/services/API.js`

```js
import axios from "axios";

export default axios.create({
    responseType: "json"
}); 
```

## 3. ใช้ axios ในดึงข้อมูล json

เปิดไฟล์ `src/redux/actions.js`

import `API` module และใช้ในการดึงข้อมูล

```js
import API from "../services/API";

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

        try {
            const success = await API.get('./branch.json');
            console.log(success.data);
            
        } catch (error) {
            console.error(error);
        }
    }
}

export default {
    requestInitData,
    showBranchData,
    ActionTypes
}
```

## 4. เพิ่ม Action สำหรับตอบรับผลการเรียกข้อมูล

```js
import API from "../services/API";

const ActionTypes = {
    SHOW_BRANCH_DATA: "SHOW_BRANCH_DATA",
    REQUEST_INIT_DATA: "REQUEST_INIT_DATA",
    REQUEST_INIT_DATA_SUCCESS: "REQUEST_INIT_DATA_SUCCESS",
    REQUEST_INIT_DATA_FAILED: "REQUEST_INIT_DATA_FAILED"
}


const showBranchData = (branchId) => ({
    type: ActionTypes.SHOW_BRANCH_DATA,
    payload: branchId
})

const requestInitData = () => {
    return async (dispatch) => {
        
        try {
            const success = await API.get('./branch.json');
            console.log(success.data);
            dispatch({ type: ActionTypes.REQUEST_INIT_DATA_SUCCESS, payload: success.data })
        } catch (error) {
            dispatch({ type: ActionTypes.REQUEST_INIT_DATA_FAILED, payload: error })
        }
    }
}

export default {
    requestInitData,
    showBranchData,
    ActionTypes
}
```

## 5. สร้างกลไกแจ้งเตือนผลการโหลดข้อมูล

เปิดไฟล์ `src/redux/dashboard.reducer.js`

เริ่มจากกำหนดค่า `initialState.app` เพื่อใช้อ้างอิงใน App Component ว่าต้องแสดง notification หรือไม่

```js
const initialState = {
    branches: BranchModel.branches,
    branchDataInChart: [['Month', 'Amount'], ['', 0]],
    app: {
        
    }
}
```

ถ้าไม่ต้องแสดง ให้กำหนด `initialState.app.notification` เป็น {}

เช่นกรณีของการกดเลือกหมุดแล้วแสดงข้อมูลในกราฟ **ไม่ต้องแสดง notification**

```js
 case Actions.ActionTypes.SHOW_BRANCH_DATA: {
     //..
    if() {
     return {
            ...state,
            branchDataInChart: finalChartData,
            app: { notification: {} }
     }
    //..
    } else {
        return { ...state, app: { notification: {} } }
    }   
 }
```

แต่ถ้าเป็น Action ผลลัพธ์จากการโหลดข้อมูล (ทั้งสำเร็จ และไม่สำเร็จ) จะมีการกำหนดค่าให้ ``

```js
case Actions.ActionTypes.REQUEST_INIT_DATA_SUCCESS: {
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
```

### ไฟล์เต็ม dashboard.reducer.js

```js
import Actions from "./actions";
import BranchModel from "../models/branchModel";

const initialState = {
    branches: BranchModel.branches,
    branchDataInChart: [['Month', 'Amount'], ['', 0]],
    app: {
        
    }
}

export default (state = initialState, { type, payload }) => {
    switch (type) {

        case Actions.ActionTypes.SHOW_BRANCH_DATA: {

            console.log(`branchId: ${payload}`)

            let selectingBranch = BranchModel.branches.find(branch => {
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
                    branchDataInChart: finalChartData,
                    app: { notification: {} }
                }

            } else {
                return { ...state, app: { notification: {} } }
            }
        }

        case Actions.ActionTypes.REQUEST_INIT_DATA_SUCCESS: {
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

## 6. แสดง notification โดยพิจารณาจาก state ที่ได้

เปิดไฟล์ `src/App.js` 

แล้ว import `message` ของ Ant Design เข้ามา

```js
import { Layout, Menu, Row, Col, message } from 'antd';
```

จากนั้น กำหนดค่า `state.app.notification` ให้กับ `props.notification` ใน `mapStateToProps`

```js
const mapStateToProps = (state) => {

  console.log(state)
  return {
    notification: state.app.notification
  }

}
```


จากนั้นเรียกใช้ ค่า `props` ใน method `componentDidUpdate()` ของ React component ที่จะทำงานทุกครั้งที่ state มีการอัพเดต (ในที่นี้มาจาก Reducer)

```js
componentDidUpdate() {

    if (this.props.notification.isShow) {
      if (this.props.notification.isError) {
        message.error(this.props.notification.message);
      } else {
        message.success(this.props.notification.message);
      }
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

import { Layout, Menu, Row, Col, message } from 'antd';
import actions from './redux/actions';
const { Header, Content, Footer } = Layout;

class App extends Component {

  componentDidMount() {
    this.props.initData();
  }

  componentDidUpdate() {

    if (this.props.notification.isShow) {
      if (this.props.notification.isError) {
        message.error(this.props.notification.message);
      } else {
        message.success(this.props.notification.message);
      }
    }

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
                <Col span={12}><StatChart /></Col>
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

const mapStateToProps = (state) => {

  console.log(state)
  return {
    notification: state.app.notification
  }

}

const mapDispatchToProps = dispatch => {
  return {
    initData: () => dispatch(actions.requestInitData())
  }
}


export default connect(mapStateToProps, mapDispatchToProps)(App);
```

## 7. กำหนดข้อมูลสาขาจาก Web API ให้กับ state

เปิดไฟล์ `src/redux/dashboard.reducer.js`

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

            let selectingBranch = BranchModel.branches.find(branch => {
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
                    branchDataInChart: finalChartData,
                    app: { notification: {} }
                }

            } else {
                return { ...state, app: { notification: {} } }
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