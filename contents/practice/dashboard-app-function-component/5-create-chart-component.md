
# สร้าง Stat Chart Component

## ติดตั้ง package 

ถ้ายังไม่ได้ติดตั้ง package ชื่อ `react-google-charts` ล่ะก็ อย่าลืมรันคำสั่งติดตั้งให้กับโปรเจคล่ะ 

```bash
npm i react-google-charts
```

### ใช้ yarn?

```bash
yarn add react-google-charts
```

## 1. สร้าง StatChart Component

สร้างไฟล์ `src/components/StatChart.js`

```jsx
// ใช้ snippet rfc ได้
import React from 'react'

export default function StatChart() {
    return (
        <div>
            Chart me
        </div>
    )
}

```

เปิดไฟล์ `src/App.js`

import `StatChart` component เข้ามาใช้งาน

```js
import StatChart from './components/StatChart';


render() {
   //...
    <Row gutter={16}>
        <Col span={12}><MapBranch /></Col>
        <Col span={12}><StatChart/></Col>
    </Row>
    //...
}
```

### ไฟล์เต็ม App.js

```js
import logo from './logo.svg';
import './App.css';
import HeaderBar from './components/HeaderBar';
import { Layout, Menu, Row, Col } from 'antd';
import MapBranch from './components/MapBranch';
import StatChart from './components/StatChart';

const { Header, Content, Footer } = Layout;

function App() {
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
            <Row gutter={16}>
              <Col span={12}><MapBranch /></Col>
              <Col span={12}><StatChart/></Col>
            </Row>

          </div>
        </Content>
        <Footer style={{
          textAlign: 'center'
        }}>React Redux Workshop ©2012-2020 Created by Nextflow.in.th</Footer>
      </Layout>,
    </div>
  );
}

export default App;


```

## 2. ใช้ AntDesign Component จัดหน้าตาของ StatChart

เปิดไฟล์ `src/components/StatChart.js`

และใช้ `Card` component ของ AntDesign 

```jsx
import React from 'react'
import { Card } from 'antd'

export default function StatChart() {
    return (
        <div>
            <Card title="Chart" style={{
                width: '100%'
            }}>
                <p>Card content</p>
            </Card>
        </div>
    )
}

```

## 3. ใส่ Line Chart 

> Documentation: [react-google-charts](https://react-google-charts.com/)

```js
import React from 'react'
import { Card } from 'antd'
// import Chart component
import Chart from "react-google-charts";

export default function StatChart() {

    return (
        <div>
            <Card title="Chart" style={{
                width: '100%'
            }}>
                {/* Use chart component   */}
                <Chart
                        width={'100%'}
                        height={'400px'}
                        chartType="LineChart"
                        loader={<div>Loading Chart</div>}
                        data={[
                            ['x', 'dogs'],
                            [0, 0],
                            [1, 10],
                            [2, 23],
                            [3, 17],
                            [4, 18],
                            [5, 9],
                            [6, 11],
                            [7, 27],
                            [8, 33],
                            [9, 40],
                            [10, 32],
                            [11, 35],
                        ]}
                        
                        rootProps={{ 'data-testid': '1' }}
                    />
            </Card>
        </div>
    )
}
```

## 4. แสดง Chart แบบ Material Design

ทดสองแก้ไข props `chartType` เป็น `Line`

```jsx
<Chart
    width={'100%'}
    height={'400px'}
    chartType="Line"
    >
```