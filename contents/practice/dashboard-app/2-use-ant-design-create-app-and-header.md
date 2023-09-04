
# ใช้งาน Ant Design สร้างหน้า App สร้าง Header Menu

## 1. เรียกใช้ CSS ของ AntDesign

เปิดไฟล์ `src/App.css`

```css
@import '~antd/dist/antd.css';

.App {
  text-align: center;
}

/* CSS ที่เหลือ */
```

## 2. สร้าง HeaderBar component สำหรับแสดงเมนู

สร้างไฟล์​ `src/components/HeaderBar.js`

```js
import React, {Component} from 'react'
import {Layout, Menu} from 'antd';
const {Header} = Layout;

export default class HeaderBar extends Component {
  render() {
    return (
      <Header>
        <div className="logo"/>
        <Menu
          theme="dark"
          mode="horizontal"
          defaultSelectedKeys={['1']}
          style={{
          lineHeight: '64px'
        }}>
          <Menu.Item key="1">Dashboard</Menu.Item>
        </Menu>
      </Header>
    )
  }
}
```

## 3. ใช้ Layout component ของ AntDesign ในการจัดหน้าตาของ App Component

> Reference: [Layout](https://ant.design/components/layout/) 

เปิดไฟล์ `src/App.js`

เราจะเพ่ิมคำสั่ง import Component ต่างๆ ของ Ant Design เข้ามา รวมถึง `Layout`

```js
// เอา HeaderBar มาวางไว้เป็นเมนูด้านบน
import HeaderBar from './components/HeaderBar';
import {Layout, Menu, Breadcrumb} from 'antd';

const {Header, Content, Footer} = Layout;
```

## 4. แปลง App component จาก Fucntion component เป็น class component

เปิดไฟล์ `src/App.js`

เราจะปรับการประกาศ App component แบบ function เป็นแบบ class

### จาก function component

```js
function App() {
  return ...;
}
```

### เป็น class component

```js
class App extends React.Component {
  render() {
    return ...;
  }
}
```

## 5. กำหนด UI ใน App component 

ในส่วนของ method `render()` เราจะใช้ JSX component ของ Ant Design และ JSX ธรรมดาสร้างหน้าแอพ ดังตัวอย่างด้านล่าง

```js
class App extends React.Component {
  render() {
    return (
    <div>
      <Layout className="layout">
        <HeaderBar/>
        <Content style={{
          padding: '0 50px'
        }}>
          <div
            style={{
            background: '#fff',
            padding: 24,
            minHeight: 280
          }}>Content</div>
        </Content>
        <Footer style={{
          textAlign: 'center'
        }}>React Redux Workshop ©2012-2019 Created by Nextflow.in.th</Footer>
      </Layout>,
    </div>
    );
  }
}
```

## ไฟล์ App.js เต็ม

```js
import React from 'react';
import logo from './logo.svg';
import './App.css';
import HeaderBar from './components/HeaderBar';
import {Layout, Menu, Breadcrumb} from 'antd';
const {Header, Content, Footer} = Layout;

class App extends React.Component {
  render() {
    return (
    <div>
      <Layout className="layout">
        <HeaderBar/>
        <Content style={{
          padding: '0 50px'
        }}>
          <div
            style={{
            background: '#fff',
            padding: 24,
            minHeight: 280
          }}>Content</div>
        </Content>
        <Footer style={{
          textAlign: 'center'
        }}>React Redux Workshop ©2012-2019 Created by Nextflow.in.th</Footer>
      </Layout>,
    </div>
    );
  }
}

export default App;

```