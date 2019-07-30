
# สร้างระบบ Navigation ด้วย React Router

## 1. ติดตั้ง Module 

```bash
npm i react-router-dom connected-react-router
```

## 2. สร้าง Root Reducer เพื่อใช้งาน Reducer อื่นๆ ของเรา ร่วมกับ Reducer ของ React-Router 

สร้างไฟล์ `src/redux/root.reducer.js`

ไฟล์นี้จะเป็นตัวรวม reducer ในแอพของเรา และสามารถสร้างเพิ่มได้ 

แน่นอนว่าตอนนี้เรามีแค่ `dashboardReducer` และเราจะเพิ่ม reducer ของ `react-router` ที่ชื่อ `connectRouter()` เข้ามา

สังเกตว่าเราตั้งชื่อให้กับ reducer แต่ละตัวได้ด้วย

```js
import { combineReducers } from 'redux'
import { connectRouter } from 'connected-react-router'
import dashboardReducer from "./dashboard.reducer";

export default (history) => combineReducers({
  router: connectRouter(history),
  dashboard: dashboardReducer
}) 
```

## 3. ปรับให้ Store ทำงานกับ History Module และ React-router-middleware

เปิดไฟล์ `src/redux/store.js`

เร่ิมแรกเราจะเอา History Module เข้ามาใช้ 

```js
import { createBrowserHistory } from 'history'

export const history = createBrowserHistory()
```

จากนั้น เราจะเอา `rootReducer` มาใช้แทน `dashboardReducer` 

```js
import createRootReducer from "./root.reducer";

export default function configureStore() {
    const store = createStore(
        createRootReducer(history),
        applyMiddleware(thunk,logger),
       ),
    );
    return store;
}
```

และเราจะใช้ `compose` จาก redux เป็นตัวประกอบ middleware เดิม และ middleware ของ `react-router`เข้าไปใน store 

```js
import { createStore, applyMiddleware, compose } from 'redux';   
import { routerMiddleware } from 'connected-react-router'

const store = createStore(
    createRootReducer(history),
    compose(
        applyMiddleware(
            routerMiddleware(history), 
            thunk,
            logger
        ),
        ),
);
```

### ไฟล์เต็ม Store.js

```js
import { createStore, applyMiddleware, compose } from 'redux';   
import dashboardReducer from "./dashboard.reducer";

import thunk from 'redux-thunk';
import { logger } from 'redux-logger';
import { routerMiddleware } from 'connected-react-router'
import { createBrowserHistory } from 'history'
import createRootReducer from "./root.reducer";

export const history = createBrowserHistory()

export default function configureStore() {
    const store = createStore(
        createRootReducer(history),
        compose(
            applyMiddleware(
              routerMiddleware(history), 
              thunk,
              logger
            ),
          ),
    );
    return store;
}
```

## 4. แยกส่วนของ App.js ออกไปเป็นหน้า Dashboard

สร้างไฟล์ `src/pages/Dashboard.js` 

ใช้ snippet ได้

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

const mapDispatchToProps = {

}

export default connect(mapStateToProps, mapDispatchToProps)(Dashboard)
```

ปรับไฟล์ App.js ใหม่

```js
import React, { Component } from 'react';
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

## 5. นำ history มากำหนดให้กับ Connected Router ในระดับนอกสุดของ Component 

เปิดไฟล์​ `src\index.js`

เริ่มจาก import ส่วนที่ต้องการเข้ามาใช้งาน

```js
import configureStore, { history } from "./redux/store";
import { ConnectedRouter } from 'connected-react-router'
```

และครอบ App ของเราด้วย `<ConnectedRouter>`

```js
ReactDOM.render((
  <Provider store={store}>
    <ConnectedRouter history={history}>
      <App/>
    </ConnectedRouter>
  </Provider>
), document.getElementById('root'));
```

### ไฟล์เต็ม index.js

```js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import * as serviceWorker from './serviceWorker';

import {Provider} from 'react-redux'
import configureStore, { history } from "./redux/store";
import { ConnectedRouter } from 'connected-react-router'

const store = configureStore();

ReactDOM.render((
  <Provider store={store}>
    <ConnectedRouter history={history}>
      <App/>
    </ConnectedRouter>
  </Provider>
), document.getElementById('root'));

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls. Learn
// more about service workers: https://bit.ly/CRA-PWA
serviceWorker.unregister();
```


## 6. เรียกใช้ <Route/> และ <Switch/> ของ react-router-dom

เปิดไฟล์ `src/App.js`

เราจะมีการ import Component ที่ต้องใช้มาจาก `react-router-dom`

```js
import { Route, Switch } from "react-router-dom";
```

และ Dashboard component 

```js
import { Dashboard } from './pages/Dashboard';
```

ทำให้เราสามารถวางระบบ Route เพื่อจัดการแสดงหน้าตาของแอพได้

```html
<Switch>
    <Route path="/" exact component={Dashboard} />
</Switch>
```

```js
import React, { Component } from 'react';
import './App.css';

import { connect } from 'react-redux';
import { Route, Switch } from "react-router-dom";

import HeaderBar from './components/HeaderBar';
import MapBranch from './components/MapBranch';
import StatChart from './components/StatChart';

import { Layout, Menu, Row, Col, message } from 'antd';
import actions from './redux/actions';
import { Dashboard } from './pages/Dashboard';

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
              <Switch>
                <Route path="/" exact component={Dashboard} />
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

## 7. ปรับให้ mapStateToProps ของ Component ต่างๆ รับกับ Ruducer แบบรวม

จะเห็นว่าหลังจากเราใช้ `combineReducer` แล้วตัว Component ของเราจะพังในส่วน `mapStateToProps` เป็นแถบๆ 

เพราะในที่นี้ state object ที่ส่งมาเข้า `mapStateToProps` จะได้เป็นโครงสร้างใหม่ ตามที่เราตั้งชื่อไว้ในไฟล์ `root.reducer.js`

### ไฟล์ `src/App.js`

```js
const mapStateToProps = (state) => {
 
  console.log(state)
  return {
    notification: state.dashboard.app.notification
  }

}
```

### ไฟล์ `src/components/MapBranch.js`

```js
const mapStateToProps = (state) => ({
  branches: state.dashboard.branches
})
```

### ไฟล์ `src/components/StatChart.js`

```js
const mapStateToProps = (state) => ({
    chartData: state.dashboard.branchDataInChart
})
```

## 8. สร้าง Login Page และเอามาแสดงใน App.js

สร้างไฟล์ `src/pages/LoginPage.js`

```js
import React, { Component } from 'react'
import { connect } from 'react-redux'

export class LoginPage extends Component {
    

    render() {
        return (
            <div>
                Log me in
            </div>
        )
    }
}

const mapStateToProps = (state) => ({

})

const mapDispatchToProps = {

}

export default connect(mapStateToProps, mapDispatchToProps)(LoginPage)

```

### ไฟล์เต็ม App.js

```js
import React, { Component } from 'react';
import './App.css';

import { connect } from 'react-redux';
import { Route, Switch } from "react-router-dom";

import HeaderBar from './components/HeaderBar';
import MapBranch from './components/MapBranch';
import StatChart from './components/StatChart';

import { Layout, Menu, Row, Col, message } from 'antd';
import actions from './redux/actions';
import Dashboard from './pages/Dashboard';
import LoginPage from './pages/LoginPage';

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
              <Switch>
                <Route path="/" exact component={LoginPage} />
                <Route path="/dashboard" exact component={Dashboard} />
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

## 9. สร้าง UI ของหน้า Login 

```js
import React, { Component } from 'react'
import PropTypes from 'prop-types'
import { connect } from 'react-redux'

import { Form, Icon, Input, Button, Checkbox } from 'antd';


export class LoginPage extends Component {
    

    render() {
        return (
            <Form onSubmit={this.handleSubmit} className="login-form">
            <Form.Item>
              <Input
                  prefix={<Icon type="user" style={{ color: 'rgba(0,0,0,.25)' }} />}
                  placeholder="Username"
                />
            </Form.Item>
            <Form.Item>
              <Input
                  prefix={<Icon type="lock" style={{ color: 'rgba(0,0,0,.25)' }} />}
                  type="password"
                  placeholder="Password"
                />
            </Form.Item>
            <Form.Item>
              <Button type="primary" htmlType="submit" className="login-form-button">
                Log in
              </Button>
            </Form.Item>
          </Form>
        )
    }
}

const mapStateToProps = (state) => ({
    
})

const mapDispatchToProps = {
    
}

export default connect(mapStateToProps, mapDispatchToProps)(LoginPage)
```

## 10. ใช้กลไก State ของ React Component ในการดึงค่าจาก Input มาเก็บไว้ใน State

เปิดไฟล์ `src/pages/LoginPage.js`

เริ่มแรกจากการกำหนดค่าเริ่มต้นให้กับ State

```js
state = {
        username: '',
        password: ''
    }
```

จากนั้นสร้าง method function สำหรับใช้ทำงานตอนที่ผู้ใช้กรอกข้อมูล 

```js
setUsername = (value) => {
    this.setState({
        ...this.state,
        username: value
    })
    // console.log(this.state);
}

setPassword = (value) => {
    this.setState({
        ...this.state,
        password: value
    })
    // console.log(this.state);
}
```

จากนั้นใน `<Input>`: 
1. เราจะกำหนด props `value` เชื่อมกับค่าจาก State 
2. และ event `onInput` ให้เชื่อมกับ method ของเรา เพื่ออัพเดตข้อมูลเข้า State เวลาที่ผู้ใช้กรอกข้อมูล

```html
<Input
    ...
    value={this.state.username} 
    onInput={e => this.setUsername(e.target.value)}
/>

<Input
   ...
    value={this.state.password} 
    onInput={e => this.setPassword(e.target.value)}
/>
```

ไฟล์เต็ม `src/pages/LoginPage.js`

```js
import React, { Component, useState } from 'react'
import PropTypes from 'prop-types'
import { connect } from 'react-redux'
import { Form, Icon, Input, Button, Checkbox } from 'antd';


export class LoginPage extends Component {
    
    state = {
        username: '',
        password: ''
    }


    handleSubmit = (e) => {
        e.preventDefault();
        console.log(`Loggin in with U:${this.state.username} P:${this.state.password}`)
    }

    setUsername = (value) => {
        this.setState({
            ...this.state,
            username: value
        })
        // console.log(this.state);
    }
    setPassword = (value) => {
        this.setState({
            ...this.state,
            password: value
        })
        // console.log(this.state);
    }

    render() {
        return (
            <Form onSubmit={this.handleSubmit} className="login-form">
                <Form.Item>
                    <Input
                        prefix={<Icon type="user" style={{ color: 'rgba(0,0,0,.25)' }} />}
                        placeholder="Username"
                        value={this.state.username} 
                        onInput={e => this.setUsername(e.target.value)}
                    />
                </Form.Item>
                <Form.Item>
                    <Input
                        prefix={<Icon type="lock" style={{ color: 'rgba(0,0,0,.25)' }} />}
                        type="password"
                        placeholder="Password"
                        value={this.state.password} 
                        onInput={e => this.setPassword(e.target.value)}
                    />
                </Form.Item>
                <Form.Item>
                    <Button type="primary" htmlType="submit" className="login-form-button">
                        Log in
              </Button>
                </Form.Item>
            </Form>
        )
    }
}

const mapStateToProps = (state) => ({

})

const mapDispatchToProps = {

}

export default connect(mapStateToProps, mapDispatchToProps)(LoginPage)
```

