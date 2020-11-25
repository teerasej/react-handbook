
# จัดแยก Reducer ให้สะดวกมากขึ้น

หลังจากเราใช้ combineReducer จะเห็นว่า state จะแยกข้อมูลตามชื่อ reducer ทำให้เราสามารถสร้าง reducer และเรียกใช้งานใน component ที่เจาะจนมากขึ้น

## 1. สร้าง app reducer เพื่อจัดการ app state โดยเฉพาะ

สร้างไฟล์ `src/redux/app.reducer.js`

และย้าย Action ที่เกี่ยวข้องกับการอัพเดต State มาไว้ใน Reducer นี้ 

```js

import Actions from "./actions";

const initialState = {
    notification: {}
}

export default (state = initialState, { type, payload }) => {
    switch (type) {

        case Actions.ActionTypes.REQUEST_INIT_DATA_SUCCESS: {
            return {
                ...state,
                notification: {
                    message: 'Data Loaded',
                    isError: false,
                    isShow: true
                }
            }
        }

        case Actions.ActionTypes.REQUEST_INIT_DATA_FAILED: {
            return {
                ...state,
                notification: {
                    message: payload.message,
                    isError: true,
                    isShow: true
                }
            }
        }

        case Actions.ActionTypes.USER_LOGIN_SUCCESS: {
            return { ...state, ...payload }
        }

        case Actions.ActionTypes.USER_LOGIN_FAILED: {
            return { ...state, ...payload }
        }

        default:
            return state
    }
}
```

เสร็จแล้วเอาไปเพิ่มใน `combineReducer()` ของไฟล์​ `src/redux/root.reducer.js` และตั้งชื่อว่า `app`

```js
import { combineReducers } from 'redux'
import { connectRouter } from 'connected-react-router'
import dashboardReducer from "./dashboard.reducer";
import appReducer from './app.reducer';

export default (history) => combineReducers({
  router: connectRouter(history),
  dashboard: dashboardReducer,
  app: appReducer
})
```

จากนั้น ไปปรับ ในไฟล์ `src/App.js`

```js
const mapStateToProps = (state) => {

  console.log(state)
  return {
    notification: state.app.notification
  }

}

```

### ไฟล์ dashboard.reducer.js หลังการย้าย 

```js

import Actions from "./actions";
import BranchModel from "../models/branchModel";

const initialState = {
    branches: [],
    branchDataInChart: [['Month', 'Amount'], ['', 0]],
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
                }

            } else {
                return { ...state }
            }
        }

        case Actions.ActionTypes.REQUEST_INIT_DATA_SUCCESS: {
            return {
                ...state,
                branches: payload.branches,
            }
        }

        default:
            return state
    }
}
```

จะเห็นว่าส่วน `case Actions.ActionTypes.REQUEST_INIT_DATA_SUCCESS` ยังคงอยู่  เพราะต้องเป็นคนจ่ายข้อมูลที่ได้มาให้กับ **MapBranch** component

```js
case Actions.ActionTypes.REQUEST_INIT_DATA_SUCCESS: {
    return {
        ...state,
        branches: payload.branches,
    }
}
```

## 2. ส่ง Action เพื่อแสดง notification ถ้า login ไม่ผ่าน 

เปิดไฟล์ `src/redux/actions.js`

สั่งให้ dispatch action ชื่อ `ActionTypes.USER_LOGIN_FAILED` ถ้าต้องการแสดง notification แบบ error 

ซึ่งถ้าเทียบจากชื่อ Action Type แล้ว ตัว appReducer จะทำงานกับ Action พวกนี้แน่นอน เพราะประกาศเอาไว้

```js
const userLogin = (username, password) => {
    return async dispatch => {
        try {
            if (username === 'a' && password === '1234') {
                // dispatch({ type: ActionTypes.USER_LOGIN_SUCCESS, payload: 'ok' })
                dispatch(push('/dashboard'))
            } else {
                dispatch({
                    type: ActionTypes.USER_LOGIN_FAILED,
                    payload: {
                        message: 'Please check username & password'
                    }
                })
            }

        } catch (error) {
            dispatch({ type: ActionTypes.USER_LOGIN_FAILED, payload: error })
        }
    }
}
```

### ไฟล์เต็ม actions.js 

```js
import { push } from 'connected-react-router'
import API from "../services/API";

const ActionTypes = {
    SHOW_BRANCH_DATA: "SHOW_BRANCH_DATA",
    REQUEST_INIT_DATA: "REQUEST_INIT_DATA",
    REQUEST_INIT_DATA_SUCCESS: "REQUEST_INIT_DATA_SUCCESS",
    REQUEST_INIT_DATA_FAILED: "REQUEST_INIT_DATA_FAILED",

    USER_LOGIN_START: "USER_LOGIN_START",
    USER_LOGIN_SUCCESS: "USER_LOGIN_SUCCESS",
    USER_LOGIN_FAILED: "USER_LOGIN_FAILED"
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
            const success = await API.get('./branch.json');
            console.log(success.data);
            dispatch({ type: ActionTypes.REQUEST_INIT_DATA_SUCCESS, payload: success.data })
            return onSuccess(success);
        } catch (error) {
            dispatch({ type: ActionTypes.REQUEST_INIT_DATA_FAILED, payload: error })
            return onError(error)
        }
    }
}

const userLogin = (username, password) => {
    return async dispatch => {
        try {
            if (username === 'a' && password === '1234') {
                // dispatch({ type: ActionTypes.USER_LOGIN_SUCCESS, payload: 'ok' })
                dispatch(push('/dashboard'))
            } else {
                dispatch({
                    type: ActionTypes.USER_LOGIN_FAILED,
                    payload: {
                        message: 'Please check username & password'
                    }
                })
            }

        } catch (error) {
            dispatch({ type: ActionTypes.USER_LOGIN_FAILED, payload: error })
        }
    }
}

export default {
    requestInitData,
    showBranchData,
    ActionTypes,
    userLogin
}


```

## 3. เปลี่ยน Component ที่เป็นคนส่ง Action ขอข้อมูลเป็น Dashboard Page แทน

เปิดไฟล์​ `src/App.js`

เราจะลบส่วนของ `componentDidMount()` ออก

```js
 componentDidMount() {
    this.props.initData();
  }
```

รวมไปถึงเคลียร์ dispatch ที่ไม่ใช้ออกจาก `mapDispatchToProps` 

```js
const mapDispatchToProps = dispatch => {
  return {
    
  }
}
```

เปิดไฟล์​ `src/pages/Dashboard.js`

และเราจะเอา `componentDidMount()` มาใส่ใน Component นี้แทน

```js
 componentDidMount() {
    this.props.initData();
  }
```

รวมไปถึงใส่ dispatch ให้กับ  `mapDispatchToProps` 

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
import './App.css';

import { connect } from 'react-redux';
import { BrowserRouter as Router, Route, Switch } from "react-router-dom";

import HeaderBar from './components/HeaderBar';
import MapBranch from './components/MapBranch';
import StatChart from './components/StatChart';

import { Layout, Menu, Row, Col, message } from 'antd';
import actions from './redux/actions';
import Dashboard from './pages/Dashboard';
import LoginPage from './pages/LoginPage';

const { Header, Content, Footer } = Layout;






class App extends Component {

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
              <Switch>
                <Route path="/" exact component={LoginPage} />
                <Route path="/dashboard" component={Dashboard} />
              </Switch>
            </div>
          </Content>
          <Footer style={{
            textAlign: 'center'
          }}>React Redux Workshop ©2012-2019 Created by Nextflow.in.th</Footer>
        </Layout>
        
      </div>

    );
  }
}

const mapStateToProps = (state) => {
  console.log('State to App.js');
  
  console.log(state)
  return {
    notification: state.app.notification
  }

}

const mapDispatchToProps = dispatch => {
  return {
    
  }
}


export default connect(mapStateToProps, mapDispatchToProps)(App);
```

### ไฟล์เต็ม Dashboard.js

```js
import React, { Component } from 'react'
import PropTypes from 'prop-types'
import { connect } from 'react-redux'

import MapBranch from '../components/MapBranch';
import StatChart from '../components/StatChart';

import { Layout, Row, Col } from 'antd';
import actions from '../redux/actions';

const { Header, Content, Footer } = Layout;

export class Dashboard extends Component {
    static propTypes = {
        prop: PropTypes
    }

    componentDidMount() {
        this.props.initData();
    }

    render() {
        return (
            <Row gutter={16}>
                <Col span={12}><MapBranch /></Col>
                <Col span={12}><StatChart /></Col>
            </Row>
        )
    }
}

const mapStateToProps = (state) => ({
    
})

const mapDispatchToProps = dispatch => ({
    initData: () => dispatch(actions.requestInitData())
})

export default connect(mapStateToProps, mapDispatchToProps)(Dashboard)
```