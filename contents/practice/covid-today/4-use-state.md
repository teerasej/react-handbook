
# การใช้งาน react hook: useState และดึงข้อมูลจาก web api

- อ้างอิง API [https://covid19.th-stat.com/api/open/today](https://covid19.th-stat.com/api/open/today)

## 1. เรียกใช้งาน react hook 

- [อ้างอิง Ant Design - Spin Component](https://ant.design/components/spin/)

เปิดไฟล์ `src/App.js` 

เราจะเรียกใช้ `useState` และ `useEffect` จาก react

และเรียกใช้ Spin component จาก `antd`

```js
import { useState, useEffect } from 'react';
import { Spin } from 'antd';
```

## 2. ปรับใช้งาน UI ให้อิงกับค่าตัวแปร useState

```jsx
function App() {

  // กำหนด react hook ให้สร้าง ตัวแปร report 
  // และสร้าง function สำหรับกำหนดค่าใหม่ให้ ตัวแปร report ชื่อ function setReport()
  // และกำหนดค่าเริ่มต้นให้ ตัวแปร report เป็น {} (object เปล่า) ผ่าน method useState()
  const [report, setReport] = useState({});
 
  // สร้างกลไกเช็คว่า ตัวแปร report มีข้อมูลหรือไม่ 
  // ถ้าไม่มีข้อมูล ให้กำหนดค่าตัวแปร showContent เป็น Spin component เพื่อแสดงในหน้าแอพ
  // ถ้ามีข้อมูล (ได้มาจาก web api แล้ว) ให้แสดงข้อมูลใน card 
  let showContent;
  if (!report.Confirmed) {
    showContent = <Spin></Spin>;
  } else {
    showContent = (
      <Row>
        <Col span={8} style={{ marginLeft: 10 }}>
          <Card title="จำนวนผู้ติดเชื้อ" >
            <p>{report.Confirmed} คน</p>
          </Card>
        </Col>
        <Col span={8} style={{ marginLeft: 10 }}>
          <Card title="จำนวนป่วยที่หายดีแล้ว">
            <p>{report.Recovered} คน</p>
          </Card>
        </Col>
        <Col span={8} style={{ marginLeft: 10, marginTop: 10 }}>
          <Card title="จำนวนผู้เสียชีวิต">
            <p>{report.Deaths} คน</p>
          </Card>
        </Col>
      </Row>
    );
  }


  return (
    <div>
      <Layout className="layout">
        <MenuBar />
        <Content style={{
          padding: '0 50px'
        }}>
          <div
            style={{
              background: '#fff',
              padding: 24,
              minHeight: 280
            }}>
            {/* แสดง component ที่เก็บไว้ในตัวแปร showContent */}
            {showContent}
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

## 3. ใช้ useEffect กำหนดกลไกการเรียกข้อมูล

ีuseEffect เป็น hook ที่จะทำงานหลัง component โหลดขึ้นมาสมบูรณ์ จึงเป็นี่เหมาะมากสำหรับการส่งคำขอข้อมูลไปที่ web api 

ดังนั้นเราจะใช้ `fetch` function เรียกไปที่ web api และเอาค่าผลลัพธ์ที่ได้มากำหนดให้ตัวแปร `report` ผ่าน function `setReport`

```jsx
const getReport = async () => {
    const response = await fetch('https://covid19.th-stat.com/api/open/today');
    const recentReport = await response.json();
    console.log(recentReport);
    setReport(recentReport);
}
```

และเราจะเรียกใช้ useEffect เพื่อเริ่มการเรียกใช้คำสั่งขอข้อมูลของเรา

```jsx
useEffect(() => {
    console.log('ok');
    getReport();
}, []);
```

# ไฟล์เต็ม App.js

```jsx
import React from 'react';
import logo from './logo.svg';
import './App.css';
import MenuBar from './components/Menubar';

import { Layout } from 'antd';
import { Row, Col } from 'antd';
import { Card } from 'antd';
import { useState, useEffect } from 'react';
import { Spin } from 'antd';
const { Header, Content, Footer } = Layout;


function App() {

  const [report, setReport] = useState({});

  useEffect(() => {
    console.log('ok');
    getReport();
  }, []);

  const getReport = async () => {
    const response = await fetch('https://covid19.th-stat.com/api/open/today');
    const recentReport = await response.json();
    console.log(recentReport);
    setReport(recentReport);
  }

  let showContent;
  if (!report.Confirmed) {
    showContent = <Spin></Spin>;
  } else {
    showContent = (
      <Row>
        <Col span={8} style={{ marginLeft: 10 }}>
          <Card title="จำนวนผู้ติดเชื้อ" >
            <p>{report.Confirmed} คน</p>
                </Card>
        </Col>
        <Col span={8} style={{ marginLeft: 10 }}>
          <Card title="จำนวนป่วยที่หายดีแล้ว">
            <p>{report.Recovered} คน</p>
                </Card>
        </Col>
        <Col span={8} style={{ marginLeft: 10, marginTop: 10 }}>
          <Card title="จำนวนผู้เสียชีวิต">
            <p>{report.Deaths} คน</p>
                </Card>
        </Col>
      </Row>
    );
  }


  return (
    <div>
      <Layout className="layout">
        <MenuBar />
        <Content style={{
          padding: '0 50px'
        }}>
          <div
            style={{
              background: '#fff',
              padding: 24,
              minHeight: 280
            }}>
            {showContent}
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