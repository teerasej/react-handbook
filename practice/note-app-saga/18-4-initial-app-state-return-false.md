# 22. test initial state ของแอพต้อง return false

## 1. สร้าง test file ใหม่

สร้างไฟล์ `src/redux/app.reducer.test.js`

```js
import reducer from "./app.reducer";

describe('App Reducer: loading effect', () => {

    it('loading should be false', () => {
        expect(reducer(undefined, {}).loading).toBe(false);
    });


}); 
```
## 2. สร้าง action สำหรับควบคุม App loading state

```js
// src/redux/actions.js

const ActionTypes = {
    SAVE_NEW_NOTE: "SAVE_NEW_NOTE",
    SIGN_IN_START: "SIGN_IN_START",
    SIGN_IN_SUCCESS: "SIGN_IN_SUCCESS",
    SIGN_IN_FAILED: "SIGN_IN_FAILED",

    LOADING_START: "LOADING_START",
    LOADING_END: "LOADING_END"
}
//...
```


## 3. เขียน test app loading state เมื่อเกิด action

```js
import reducer from "./app.reducer";
import actions from "./actions";

describe('App Reducer: loading effect', () => {
    
    it('loading should be false', () => {
        expect(reducer(undefined, {}).loading).toBe(false);
    });

    it('loading should true if loading start happened', () => {
        const action = {
            type: actions.ActionTypes.LOADING_START
        }

        expect(reducer(undefined, action).loading).toBe(true);
    });

    it('loading should true if loading end happened', () => {
        const action = {
            type: actions.ActionTypes.LOADING_END
        }

        expect(reducer(undefined, action).loading).toBe(false);
    });

});
```

## 4. สร้าง app.reducer.js

สร้างไฟล์ `src/redux/app.reducer.js`

```js
import actions from "./actions";

const initialState = {
    loading: false
}

export default (state = initialState, { type, payload }) => {
    switch (type) {

    case actions.ActionTypes.LOADING_START:
        return { ...state, loading: true }

    case actions.ActionTypes.LOADING_END:
        return { ...state, loading: false }

    default:
        return state
    }
}
```

## 5. เพิ่ม app reducer เข้าไปใน store

แก้ไขไฟล์ `src/redux/root.reducer.js`

```js
import { combineReducers } from 'redux'
import { connectRouter } from 'connected-react-router'
import homepageReducer from "./homepage.reducer";
import appReducer from "./app.reducer";

export default (history) => combineReducers({
  router: connectRouter(history),
  homepage: homepageReducer,
  app: appReducer
}) 
```

## 6. กำหนดให้รับ app.loading จาก redux store

แก้ไขไฟล์ `src/App.js` 

```js
import React, { Component } from 'react';
import './App.css';
import MenuBar from './components/MenuBar';
import { Layout, Spin } from 'antd';
import HomePage from './pages/home-page/HomePage';
import { Route, Switch } from "react-router-dom";
import LoginPage from './pages/login-page/LoginPage';
import { connect } from 'react-redux';
const { Footer } = Layout;

export class App extends Component {
  render() {
    return (
      <div>
        <Spin spinning={this.props.loading}>
        <Layout className="layout">
          <MenuBar />
          <Switch>
            <Route path="/" exact component={LoginPage} />
            <Route path="/home" exact component={HomePage} />
          </Switch>
          <Footer style={{
            textAlign: 'center'
          }}>React Redux Workshop ©2012-2019 Created by Nextflow.in.th</Footer>
        </Layout>
        </Spin>
      </div>
  );
  }
  
}
App.defaultProps = {
  loading: false
};

const mapStateToProps = (state) => ({
  loading: state.app.loading
})

const mapDispatchToProps = {
  
}


export default connect(mapStateToProps, mapDispatchToProps)(App);

```