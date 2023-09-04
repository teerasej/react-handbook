
# 5. สร้าง List ไว้แสดง Note message สำหรับหน้า HomePage

## 1. สร้างไฟล์ NoteList component

- [อ้างอิง Ant Design - List Component](https://ant.design/components/list/)

สร้างไฟล์ `src/pages/home-page/NoteList.js` 

ในที่นี้เราจะมีการเอา List component มาจาก Ant Design ด้วย

```js

import React, { Component } from 'react'
import { List } from 'antd';

export default class NoteList extends Component {

    data = [
        {
          title: 'Note 1',
        },
        {
          title: 'Note 2',
        },
        {
          title: 'Note 3',
        },
        {
          title: 'Note 4',
        },
      ];


    render() {
        return (
            <List
                bordered
                itemLayout="horizontal"
                dataSource={this.data}
                renderItem={item => (
                    <List.Item>
                        <List.Item.Meta
                            title={<p>{item.title}</p>}
                            description=""
                        />
                    </List.Item>
                )}
            />
        )
    }
}
```

- สังเกตว่าเรามีการประกาศ property ชื่อ `data` เป็น Array เพื่อเอามาใช้เป็นข้อมูลให้กับ List Component ของ Ant Design 
- `dataSource` เป็น props ที่ให้กำหนดข้อมูลแบบ Array ได้
- `renderItem` เป็น props ที่กำหนด function เพื่อสร้าง component ที่จะเป็น item ใน List ของเราได้

```js
<List
    ...
    dataSource={this.data}
    renderItem={item => (
        <List.Item>
            <List.Item.Meta
                title={<p>{item.title}</p>}
                description=""
            />
        </List.Item>
    )}
/>
```

## 2. นำ NoteList มาใส่ใน HomePage


### ไฟล์เต็ม `src/pages/home-page/HomePage.js`

```js
import React, { Component } from 'react'

import { Layout } from 'antd';
import NoteInputBox from './NoteInputBox';
import NoteList from './NoteList';
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
                }}>
                <NoteInputBox /> 
                <NoteList /> // <-- ตรงนี้แหละ
                </div>
              </Content>
        )
    }
}
```

## 3. เพิ่มระยะห่างระหว่าง NoteBox กับ NoteList 

```js
    <NoteInputBox /> 
    <div style={{height: 10}}/>
    <NoteList />
```

### ไฟล์เต็ม `src/pages/home-page/HomePage.js`

```js


import React, { Component } from 'react'

import { Layout } from 'antd';
import NoteInputBox from './NoteInputBox';
import NoteList from './NoteList';
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
                }}>
                <NoteInputBox /> 
                <div style={{height: 10}}/>
                <NoteList />
                </div>
              </Content>
        )
    }
}

```