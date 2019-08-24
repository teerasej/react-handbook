# 21. test ค่า App.props.loading และ implement

## 1. เขียน Test ทดสอบว่า App Component มี props.loading เป็น false ในตอนเริ่มแรกไหม

```js
// src/App.test.js

  it('should has loading state as false', () => {
    const component = shallow(<App/>);

    const props = component.instance().props;

    expect(props.loading).toBe(false);
  });
```

ทดสอบรัน test 

## 2. กำหนดค่าเริ่มต้นให้ Component ด้ย defaultProps

ในที่นี้เราจะแปลง App จาก function component เป็น class component ด้วย

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

class App extends Component {
  render() {
    return (
      <div>
        <Spin>
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



export default App;
```

ทดสอบรัน test