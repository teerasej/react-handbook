
# 4. สร้างกล่องกรอกข้อความสำหรับหน้า HomePage

## 1. สร้างไฟล์ NoteInputBox component

- [อ้างอิง Ant Design - Form Component](https://ant.design/components/form/)
- [อ้างอิง Ant Design - Card Component](https://ant.design/components/card/)

สร้างไฟล์ `src/pages/home-page/NoteInputBox.js` 

ในที่นี้เราจะมีการเอา `Form`, `Input`, `Button`, `Card` component มาจาก Ant Design ด้วย

```js
import React, { Component } from 'react'
import { Form, Input, Button, Card } from 'antd';

export default class NoteInputBox extends Component {
    render() {
        return (
            <Card>
                <Form>
                    <Form.Item layout="inline" label="Message">
                        <Input
                            placeholder="message"
                        />
                    </Form.Item>
                    <Form.Item>
                        <Button type="primary" htmlType="submit">
                            Save
                        </Button>
                    </Form.Item>
                </Form>
            </Card>
        )
    }
}
```

## 2. นำ NoteInputBox มาใส่ใน HomePage


### ไฟล์เต็ม `src/pages/home-page/HomePage.js`

```js
import React, { Component } from 'react'

import { Layout } from 'antd';
import NoteInputBox from './NoteInputBox';
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
                </div>
              </Content>
        )
    }
}

```