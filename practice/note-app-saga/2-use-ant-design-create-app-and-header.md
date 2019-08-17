
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

## 2. สร้าง MenuBar component สำหรับแสดงเมนู

สร้างไฟล์​ `src/components/MenuBar.js`

```js
import React, {Component} from 'react'
import {Layout, Menu} from 'antd';
const {Header} = Layout;

export default class MenuBar extends Component {
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
          <Menu.Item key="1">Home</Menu.Item>
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
// เอา MenuBar มาวางไว้เป็นเมนูด้านบน
import MenuBar from './components/MenuBar';

import {Layout, Menu, Breadcrumb} from 'antd';
const {Header, Content, Footer} = Layout;
```

ในส่วนของ method `render()` ก็สามารถใช้

```js
render() {
    return (
    <div>
      <Layout className="layout">
        <MenuBar/>
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
    )
}
```

### ไฟล์ App.js เต็ม

```js
import React from 'react';
import './App.css';

import MenuBar from './components/MenuBar';

import { Layout } from 'antd';
const { Content, Footer} = Layout;

function App() {
  return (
    <div>
      <Layout className="layout">
        <MenuBar/>
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

export default App;


```