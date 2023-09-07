
# 8. สร้างและใช้งาน Action 

เพิ่ม action ที่ทำการรับค่า action จาก `dispatch()` และ state ล่าสุดมาอัพเดต

```js
// src/redux/chatSlice.js

import { createSlice } from '@reduxjs/toolkit'

const initialState = {
  chatHistory: [
    { id: 1, sender: 'User', text: 'Hello' },
    { id: 2, sender: 'GPT', text: 'Hi!' }
  ]
}

const chatSlice = createSlice({
  name: 'chatSlice',
  initialState,
  reducers: {
    // เพิ่ม function ที่จะรับ state และ action เพื่อเอามาใช้งาน
    addMessageToHistory(state, action) {

      // แสดงข้อมูลของ action object
      console.log(action.type)
      console.log(action.payload)

      // นำข้อมูลจาก action.payload มาเพิ่มเป็น chat message item ใหม่ใน state 
      state.chatHistory.push({
          id: Math.floor(Math.random() * 1000),
          sender: 'Me',
          text: action.payload
        })

    },
  },
});

// export action เพื่อเอาไปเรียกใช้ใน Component
export const { addMessageToHistory } = chatSlice.actions

export default chatSlice.reducer
```

## 2. เรียกใช้ และ Dispatch Action เข้า Reducer

```js
// src/components/promptInput/PromptInputComponent.js

import React from 'react'
import { Row, Col, Form, Button } from 'react-bootstrap';

// เรียกใช้ useDispatch
import { useDispatch } from 'react-redux';
import { addMessageToHistory } from '../../redux/chatSlice';

function PromptInputComponent() {

    const [message, setMessage] = useState("")

    const handleChange = (e) => {
        e.preventDefault(); 
        setMessage(e.target.value); 
    };

    // สร้าง function dispatch
    const dispatch = useDispatch();

    // onSubmit ทำงานตอนกดปุ่ม send
    // 1. เอาข้อความที่ user พิมพ์ลงในช่อง มาสร้างเป็น action object ผ่านการใช้ addMessageToHistory ที่สร้างไว้ใน slice
    // 2. dispatch ตัว action object เข้าไปหา redux
    // 3. เคลียรฺ์ข้อความออกจาก Form.Control
    const onSubmit = () => {
        const actionObject = addMessageToHistory(message)
        dispatch(actionObject)

        setMessage("");
    };


    return (
        <Row>
            <Col>
                <Form>
                    <Form.Group controlId="message">
                        <Form.Label>Message</Form.Label>
                        <Form.Control 
                            placeholder="Type your message here"
                            type="text"
                            onChange={handleChange}
                            value={message}
                            />
                    </Form.Group>

                    {/* เรียกใช้ onSubmit function */}
                    <Button variant="primary" onClick={onSubmit}>
                        Send
                    </Button>
                </Form>
            </Col>
        </Row>
    )
}

export default PromptInputComponent
```
