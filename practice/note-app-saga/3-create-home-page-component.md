
# สร้าง HomePage Component สำหรับจัดเก็บหน้า UI ของ HomePage 

## 1. สร้างไฟล์ `HomePage.js`

สร้างไฟล์​ `src/pages/home-page/HomePage.js` 

```js
import React, { Component } from 'react'

export default class HomePage extends Component {
    render() {
        return (
            <div>

            </div>
        )
    }
}
```

## 2. ย้ายโค้ดส่วน Content มาไว้ใน HomePage.js

- [อ้างอิง Ant Design - Layout Component](https://ant.design/components/layout/)

### ไฟล์เต็ม `src/pages/home-page/HomePage.js` 

```js
import React, { Component } from 'react'

import { Layout } from 'antd';
const { Content } = Layout;

export default class HomePage extends Component {
    render() {
        return (
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
        )
    }
}
```

## 3. นำ HomePage component มาใส่แทนใน App.js 

เปิดไฟล์​ `src/App.js`

เริ่มจาก import HomePage.js เข้ามา

```js
import HomePage from './pages/home-page/HomePage';


function App() {
  return (
    <div>
      <Layout className="layout">
        <MenuBar/>
        <HomePage/>  // <-- ตรงนี้แหละ
        <Footer style={{
          textAlign: 'center'
        }}>React Redux Workshop ©2012-2019 Created by Nextflow.in.th</Footer>
```

### ไฟล์เต็ม

```js
import React from 'react';
import './App.css';

import MenuBar from './components/MenuBar';
import HomePage from './pages/home-page/HomePage';

import { Layout } from 'antd';
const { Footer} = Layout;

function App() {
  return (
    <div>
      <Layout className="layout">
        <MenuBar/>
        <HomePage/>
        <Footer style={{
          textAlign: 'center'
        }}>React Redux Workshop ©2012-2019 Created by Nextflow.in.th</Footer>
      </Layout>,
    </div>
  );
}

export default App;
```