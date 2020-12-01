
# แสดงและอัพเดตข้อมูลใน ItemList

```jsx
// src/components/list-item/ItemList.js

import { useSelector } from 'react-redux'

export default function ItemList() {

    // เรียกใช้ค่า items จาก Redux State
    const items = useSelector(state => state.items)

    return (
        <List
            header={<div>Store</div>}
            bordered

            {/* กำหนด items เป็น dataSource ของ List */}
            dataSource={items}
            {/* return JSX ของ Item List โดยใช้ Component ของ Ant Design */}
            renderItem={item => (
                <List.Item>
                    <div>{item.name}</div>
                    <div>
                        <Typography.Text mark>
                            {item.status}
                        </Typography.Text>
                    </div>
                </List.Item>
            )}
        />
    )
}

```

## ไฟล์เต็ม `src/components/list-item/ItemList.js`

```jsx
import { List, Typography } from 'antd';
import React from 'react'
import { useSelector } from 'react-redux'


export default function ItemList() {

    const items = useSelector(state => state.items)

    return (
        <List
            header={<div>Store</div>}
            bordered
            dataSource={items}
            renderItem={item => (
                <List.Item>
                    <div>{item.name}</div>
                    <div>
                        <Typography.Text mark>
                            {item.status}
                        </Typography.Text>
                    </div>
                </List.Item>
            )}
        />
    )
}

```