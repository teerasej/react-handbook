
# สร้างระบบ Navigation ด้วย React Router

- [อ้่างอิงการติดตั้งใช้งาน React Router](https://github.com/supasate/connected-react-router) 

## รูปแบบของ React-Redux App ก่อนและหลังใช้งาน Router

ก่อนใช้ Router

![Paper React   React Native 33](https://user-images.githubusercontent.com/85179/63219432-19989600-c19c-11e9-88d0-dad7984be2a2.png)

หลังใช้ Router

![Paper React   React Native 34](https://user-images.githubusercontent.com/85179/63219433-1a312c80-c19c-11e9-9b0f-7db4d665a800.png)


## 1. ติดตั้ง Module 

```bash
npm i react-router-dom connected-react-router redux-logger
```

## 2. สร้าง Root Reducer เพื่อใช้งาน Reducer อื่นๆ ของเรา ร่วมกับ Reducer ของ React-Router 

สร้างไฟล์ `src/redux/root.reducer.js`

ไฟล์นี้จะเป็นตัวรวม reducer ในแอพของเรา และสามารถสร้างเพิ่มได้ 

แน่นอนว่าตอนนี้เรามีแค่ `homepageReducer` และเราจะเพิ่ม reducer ของ `react-router` ที่ชื่อ `connectRouter()` เข้ามา

สังเกตว่าเราตั้งชื่อให้กับ reducer แต่ละตัวได้ด้วย

```js
import { combineReducers } from 'redux'
import { connectRouter } from 'connected-react-router'
import homepageReducer from "./homepage.reducer";

export default (history) => combineReducers({
  router: connectRouter(history),
  homepage: homepageReducer
}) 
```

## 3. ปรับให้ Store ทำงานกับ History Module และ React-router-middleware

เปิดไฟล์ `src/redux/store.js`

เร่ิมแรกเราจะเอา History Module เข้ามาใช้ 

```js
import { createBrowserHistory } from 'history'

export const history = createBrowserHistory()
```

จากนั้น เราจะเอา `rootReducer` มาใช้แทน `homepageReducer` 

```js
import createRootReducer from "./root.reducer";

export default function configureStore() {
    const store = createStore(
        createRootReducer(history),
        applyMiddleware(logger),
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
            logger
        ),
    ),
);
```

### ไฟล์เต็ม Store.js

```js
import { createStore, applyMiddleware, compose } from 'redux';   
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
              routerMiddleware(history)
              logger
            ),
          ),
    );
    return store;
}
```
## 4. ลบ `<Provider>` ออกจาก App.js 

ไฟล์เต็ม src/App.js

```js
import React from 'react';
import './App.css';
import MenuBar from './components/MenuBar';
import { Layout } from 'antd';
import HomePage from './pages/home-page/HomePage';
const { Footer } = Layout;

function App() {
  return (
      <div>
        <Layout className="layout">
          <MenuBar />
          <HomePage />
          <Footer style={{
            textAlign: 'center'
          }}>React Redux Workshop ©2012-2019 Created by Nextflow.in.th</Footer>
        </Layout>,
      </div>
  );
}

export default App;

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

และ HomePage component 

```js
import HomePage from './pages/home-page/HomePage';
```

ทำให้เราสามารถวางระบบ Route เพื่อจัดการแสดงหน้าตาของแอพได้

```jsx
<Switch>
    <Route path="/" exact component={Dashboard} />
</Switch>
```

### ไฟล์เต็ม src/App.js

```js
import React from 'react';
import './App.css';
import MenuBar from './components/MenuBar';
import { Layout } from 'antd';
import HomePage from './pages/home-page/HomePage';
import { Route, Switch } from "react-router-dom";
const { Footer } = Layout;

function App() {
  return (
      <div>
        <Layout className="layout">
          <MenuBar />
          <Switch>
            <Route path="/" exact component={HomePage} />
          </Switch>
          <Footer style={{
            textAlign: 'center'
          }}>React Redux Workshop ©2012-2019 Created by Nextflow.in.th</Footer>
        </Layout>,
      </div>
  );
}

export default App;
```

## 7. ปรับให้ mapStateToProps ของ Component ต่างๆ รับกับ Ruducer แบบรวม

จะเห็นว่าหลังจากเราใช้ `combineReducer` แล้วตัว Component ของเราจะพังในส่วน `mapStateToProps` เป็นแถบๆ 

เพราะในที่นี้ state object ที่ส่งมาเข้า `mapStateToProps` จะได้เป็นโครงสร้างใหม่ ตามที่เราตั้งชื่อไว้ในไฟล์ `root.reducer.js`

ให้เราเริ่มเช็ค state ที่ส่งเข้ามาใน `mapStateToProps` ของ Component ที่มีการใช้งาน และปรับให้เป็นแบบใหม่


### ไฟล์ `src/pages/home-page/NoteList.js`

```js
const mapStateToProps = (state) => {
    console.log(state);
    return {
        noteData: state.homepage.notes
    }
}
```

## 8. สร้าง Login Page และเอามาแสดงใน App.js

สร้างไฟล์ `src/pages/login-page/LoginPage.js`

```js
// ใช้ snippet rcredux ได้
import React, { Component } from 'react'
import { connect } from 'react-redux'

class LoginPage extends Component {
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

เสร็จแล้วให้เอา LoginPage component มาใช้กับ `<Switch>` ใน App.js Component

```js
import LoginPage from './pages/login-page/LoginPage';
```

### ไฟล์เต็ม src/App.js

```js
import React from 'react';
import './App.css';
import MenuBar from './components/MenuBar';
import { Layout } from 'antd';
import HomePage from './pages/home-page/HomePage';
import { Route, Switch } from "react-router-dom";
import LoginPage from './pages/login-page/LoginPage';
const { Footer } = Layout;

function App() {
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

export default App;

```

