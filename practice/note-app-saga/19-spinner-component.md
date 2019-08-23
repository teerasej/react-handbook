# 19. 

## 1. import Spin Component 

ในที่นี้เราจะเอา Spin Component จาก Ant Design มาใช้

```js
import { Layout, Spin } from 'antd';
```

## 2. ครอบ Spin ในส่วนของหน้าแอพ

```js
render() {
    return (
      <Spin spinning={this.props.loading}>
        <div>
            <Layout className="layout">
            //...
      </Spin>
    )
}
```

## 3. กำหนดใช้ดึง property จาก Redux State

```js
const mapStateToProps = (state) => ({
  loading: state.app.loading
})
```

## ไฟล์เต็ม 

```js
import React, { Component } from 'react';
import './App.css';
import { Layout, Spin } from 'antd';
import { Route, Switch } from "react-router-dom";
import { connect } from 'react-redux'

import MenuBar from './components/MenuBar';
import HomePage from './pages/home-page/HomePage';
import LoginPage from './pages/login-page/LoginPage';

const { Footer } = Layout;



class App extends Component {
  render() {
    return (
      <Spin spinning={this.props.loading}>
        <div>
          <Layout className="layout">
            <MenuBar />
            <Switch>
              <Route path="/" exact component={LoginPage} />
              <Route path="/home" exact component={HomePage} />
            </Switch>
            <Footer style={{
              textAlign: 'center'
            }}>React Redux Workshop ©2012-2019 Created by Nextflow.in.th</Footer>
          </Layout>,
        </div>
      </Spin>
    );
  }
}

const mapStateToProps = (state) => ({
  loading: state.app.loading
})

const mapDispatchToProps = {

}


export default connect(mapStateToProps, mapDispatchToProps)(App);
```