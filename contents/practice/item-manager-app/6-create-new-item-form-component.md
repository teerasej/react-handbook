
# สร้างและแสดง NewItemForm

## 1. สร้างไฟล์ NewItemForm

```jsx
// src/components/new-item/NewItemForm.js

import React from 'react'
import { Form, Input, Button, Select } from 'antd';

const { Option } = Select;

export default function NewItemForm() {

    const layout = {
        labelCol: { span: 8 },
        wrapperCol: { span: 16 },
    };
    const tailLayout = {
        wrapperCol: { offset: 8, span: 16 },
    };

    const [form] = Form.useForm();

    const onFinish = values => {
        console.log(values);
    };

    return (
        <div
            style={{
                background: '#fff',
                padding: 24,
                width: 350
            }}>
            <Form {...layout} form={form} name="control-hooks" onFinish={onFinish}>
                <Form.Item name="message" label="Message" rules={[{ required: true }]}>
                    <Input />
                </Form.Item>
                
                <Form.Item {...tailLayout}>
                    <Button type="primary" htmlType="submit">
                        Create
                    </Button>
                    <Button htmlType="button">
                        Cancel
                    </Button>
                </Form.Item>
            </Form>
        </div>
    )
}

```

## 2. กำหนดให้ <ModalRoute> แสดง NewNoteForm

```jsx
// src/App.js

import NewItemForm from './components/new-item/NewItemForm';


<Switch>
    <ModalRoute path='/add-new-item' parentPath='/' component={NewItemForm}/>
</Switch>
```

ทดสอบว่าสามารถกดเปิด Form ได้หรือไม่


## 3. ใช้ `<Link>` เพื่อทำให้ปุ่ม Cancel ปิดหน้าต่าง

```jsx
// src/components/new-item/NewItemForm.js

import { Link } from "react-router-dom";

<Link to="/">
    <Button htmlType="button">
        Cancel
    </Button>
</Link>
```

ทดสอบว่าสามารถกดปิด Form ได้หรือไม่