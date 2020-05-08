

# เชื่อม redux store เข้ากับ Component ผ่าน <Provider>

การจะติดตั้ง Redux ให้ทำงานกับ React component ได้ (เหมือนเรา setup router wifi ในบ้าน ให้พร้อมต่อกับ computer ในบ้านเรา) ก็ต้องเอามาครอบ component ทุกตัวซะก่อน

ในที่นี้เปิด `App.js` 

```js
// เรียกใช้ Provider เป็น component ที่ redux เตรียมไว้ให้เราใช้ครอบ component react ของเรา 
import { Provider } from "react-redux";

// เรียกใช้ function ที่สร้างเตรียมไว้ในไฟล์ store.js
import { configureStore } from './redux/store';
const { Content, Footer } = Layout;

// ใช้งาน function ของเรา ก็จะได้ store ที่มี reducer อยู่ข้างใน พร้อมใช้งาน
let store = configureStore();
```

จากนั้นเราจะใช้ `<Provider>` ครอบ JSX ทั้งหมด

```js
return (
    {/* ครอบ JSX ทั้งหมดด้วย Provider และกำหนด store เป็น props ของมัน */}
    <Provider store={store}>
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
        }}>React Redux Workshop ©2012-2019 Created by Nextflow.in.th</Footer>
      </Layout>,
    </div>

    {/* อย่าลืมครอบตัวปิดด้วยล่ะ */}
    </Provider>
  );
```

## ไฟล์สมบูรณ์ App.js

```js
import React from 'react';
import './App.css';



import HeaderBar from './components/HeaderBar';
import { Layout, Row, Col } from 'antd';
import MapBranch from './components/MapBranch';
import StatChart from './components/StatChart';
import { Provider } from "react-redux";
import { configureStore } from './redux/store';
const { Content, Footer } = Layout;


let store = configureStore();

function App() {
  return (
    <Provider store={store}>
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
    </Provider>
  );
}

export default App;
```