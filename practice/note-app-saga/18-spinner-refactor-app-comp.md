# 18. Refactor และแปลง App จาก function component เป็น Class component

เราจะทำการแปลง App Component จาก function มาเป็น Class 

## ไฟล์เต็ม `src\App.js`

```js
import React, { Component } from 'react';
import './App.css';
import { Layout } from 'antd';
import { Route, Switch } from "react-router-dom";
import { connect } from 'react-redux'

import MenuBar from './components/MenuBar';
import HomePage from './pages/home-page/HomePage';
import LoginPage from './pages/login-page/LoginPage';

const { Footer } = Layout;



class App extends Component {
  render() {
    return (
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
    );
  }
}

const mapStateToProps = (state) => ({

})

const mapDispatchToProps = {

}


export default connect(mapStateToProps, mapDispatchToProps)(App);
```