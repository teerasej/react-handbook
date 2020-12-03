
# สร้างส่วนแสดงรายการสินค้า List Item Component

## 1. สร้าง ItemList component

สร้างไฟล์ `src/components/list-item/ItemList.js`

```jsx
// src/components/list-item/ItemList.js

import { List, Typography } from 'antd';
import React from 'react'

export default function ItemList() {

    // กำหนด array เพื่อใช้ใน List
    const data = [
        'Racing car sprays burning fuel into crowd.',
        'Japanese princess to wed commoner.',
        'Australian walks 100km after outback crash.',
        'Man charged over missing wedding girl.',
        'Los Angeles battles huge wildfires.',
    ];

    // ใช้ List component ของ Ant Design 
    // นำข้อมูลมาใส่ในส่วนของ dataSource
    return (
        <List
            header={<div>Notes</div>}
            bordered
            dataSource={data}
            renderItem={item => (
                <List.Item>
                    <Typography.Text mark>[ITEM]</Typography.Text> {item}
                </List.Item>
            )}
        />
    )
}

```


## 2. เอา ItemList มาแสดงใน App Component

```jsx
// src/App.js
import ItemList from './components/list-item/ItemList';

//..

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
          }}>
            <ItemList/>
          </div>
        </Content>
        <Footer style={{
          textAlign: 'center'
        }}>React Redux Workshop ©2012-2021 Created by Nextflow.in.th</Footer>
      </Layout>,
    </div>
    );
```

## ไฟล์เต็ม App.js

```jsx
import React from 'react';
import logo from './logo.svg';
import './App.css';
import HeaderBar from './components/HeaderBar';
import {Layout, Menu} from 'antd';
import ItemList from './components/list-item/ItemList';
const {Header, Content, Footer} = Layout;

function App() {
  
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
          }}>
            <ItemList/>
          </div>
        </Content>
        <Footer style={{
          textAlign: 'center'
        }}>React Redux Workshop ©2012-2021 Created by Nextflow.in.th</Footer>
      </Layout>,
    </div>
    );
  
}

export default App;
```

## ไฟล์เต็ม src/components/list-item/ItemList.js

```jsx
import { List, Typography } from 'antd';
import React from 'react'

export default function ItemList() {

    const data = [
        'Racing car sprays burning fuel into crowd.',
        'Japanese princess to wed commoner.',
        'Australian walks 100km after outback crash.',
        'Man charged over missing wedding girl.',
        'Los Angeles battles huge wildfires.',
    ];

    return (
        <List
            header={<div>Notes</div>}
            bordered
            dataSource={data}
            renderItem={item => (
                <List.Item>
                    <Typography.Text mark>[ITEM]</Typography.Text> {item}
                </List.Item>
            )}
        />
    )
}

```