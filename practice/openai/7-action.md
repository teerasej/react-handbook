
# 7. สร้างและใช้งาน Action 

เพิ่ม action ที่ทำการรับค่า action จาก `dispatch()` และ state ล่าสุดมาอัพเดต

```js
// src/redux/messageSlice.js

import { createSlice } from '@reduxjs/toolkit'

const initialState = [
  { id: 1, sender: 'User', message: 'Hello' },
  { id: 2, sender: 'GPT', message: 'Hi!' }
]

const messageSlice = createSlice({
  name: 'messages',
  initialState,
  reducers: {
    // เพิ่ม function ที่จะรับ state และ action เพื่อเอามาใช้งาน
    messageAdded(state, action) {
      state.push({...action.payload, id: Math.floor(Math.random() * 1000)})
    },
  },
});

// export action เพื่อเอาไปเรียกใช้ใน Component
export const { messageAdded } = messageSlice.actions

export default messageSlice.reducer
```

## 2. 

```js
// src/components/promptinput/PromptInput.js

import React from 'react'
import { Row, Col, Form, Button } from 'react-bootstrap';

// เรียกใช้ useDispatch
import { useDispatch } from 'react-redux';
import { messageAdded } from '../../redux/messageSlice';

function PromptInput() {

    // สร้าง function dispatch
    const dispatch = useDispatch();

    // สร้าง function สำหรับ dispatch action พร้อมข้อมูลเข้า redux store
    const onSubmit = () => {

        dispatch(
            messageAdded({
                sender: 'User',
                message: 'Test'
            })
        )
    };


    return (
        <Row>
            <Col>
                <Form>
                    <Form.Group controlId="message">
                        <Form.Label>Message</Form.Label>
                        <Form.Control type="text" placeholder="Type your message here"/>
                        <Form.Control.Feedback type="invalid">
                            Please enter a message.
                        </Form.Control.Feedback>
                    </Form.Group>
                    {/* ปรับไม่ให้เป็น submit ชั่วคราว และเรียกใช้ function */}
                    <Button variant="primary" onClick={onSubmit}>
                        Send
                    </Button>
                </Form>
            </Col>
        </Row>
    )
}

export default PromptInput
```