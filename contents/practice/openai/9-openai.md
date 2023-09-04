
# 9. เรียกใช้ OpenAI API

1. [สมัครใช้งาน OpenAI Developer](https://platform.openai.com/signup)
   - สามารถสร้าง account ใหม่ โดยการใช้ Google Account หรือ Microsoft Account ได้ 
   - จะมีการกรอกเบอร์โทรศัพท์ และรับ SMS-OTP เพื่อยืนยันตัวตน
2. หลังจากได้ account และ login เข้าไปใน Dashboard ได้แล้ว ให้[เข้าไปสร้าง OpenAI API Key ขึ้นมา และ copy มาเตรียมใช้งาน](https://platform.openai.com/account/api-keys) 

## 1. สร้าง async thunk ชื่อ askAI

สร้างไฟล์​ `src/redux/askAIThunk.js`

```js
// src/redux/askAIThunk.js

// import createAsyncThunk 
import { createAsyncThunk } from '@reduxjs/toolkit'
// import axios ในการส่ง request
import axios from 'axios';

// นำ key ของ OpenAI มาใช้งาน
const key = '';

// สร้าง AsyncThunk ชื่อ askAI
export const askAI = createAsyncThunk(
  // ตั้งชื่อ async thunk
  'user/askAI',

  // รับค่าที่เข้ามาใช้่งาน ในที่นี้คือ prompt message
  async (prompt) => {

    console.log('fetching openAI')

    // สร้าง JSON object ในการส่งไปที่ OpenAI API
    const jsonPrompt = JSON.stringify({
      "model": "gpt-3.5-turbo",
      "messages": [{ "role": "user", "content": prompt }]
    });

    console.log('Sending prompt:')
    console.log(jsonPrompt)

    // ใช้ axios ส่ง request โดยการกำหนด key และ json 
    const response = await axios.post(
      'https://api.openai.com/v1/chat/completions',
      jsonPrompt,
      {
        headers: {
          'Content-Type': 'application/json',
          'Authorization': `Bearer ${key}`
        }
      });

    console.log('got response:')
    console.log(response.data.choices[0].message.content);

    // ดึงเฉพาะส่วนข้อความที่ API ตอบกลับมา
    return response.data.choices[0].message.content;

  }
);

```

## 2. กำหนด extraReducer ลงใน Slice ที่ต้องการให้รับข้อมูลจาก thunk ในกรณีต่างๆ 

```js
// src/redux/chatSlice.js

import { createSlice } from '@reduxjs/toolkit'

// import askAI thunk เพื่อมากำหนด case ใน extraReducer
import { askAI } from './askAIThunk';

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
    addMessageToHistory(state, action) {
      console.log(action.type)
      console.log(action.payload)
      state.chatHistory.push({
          id: Math.floor(Math.random() * 1000),
          sender: 'Me',
          text: action.payload
        })
    },
  },
  // กำหนด reducer พิเศษ ที่จะทำงานตามกรณีของ AsyncThunk
  extraReducers: (builder) => {
    // กำหนด case ที่จะทำงานจากกลไกของ async thunk
    builder
      .addCase(askAI.fulfilled, (state, action) => {
        console.log('succeeded');

        // ถ้าได้การทำงานของ Async thunk สมบูรณ์ ค่าที่ return ออกมาจะอยู่ใน payload 
        // ในที่นี้เราจะเอามาใส่เพิ่มในห้องแชท โดยกำหนดชื่อ sender เป็น GPT
        state.chatHistory.push({ 
          sender:'GPT', 
          text: action.payload, 
          id: Math.floor(Math.random() * 1000)
        });
        
      })
      // ในกรณีที่ Async Thunk ล้มเหลวจะเข้าเคสนี้
      .addCase(askAI.rejected, (state, action) => {
        console.log('failed');
        // แสดงข้อความ error ที่ได้
        console.error(action.error.message);
      });
  },
});

export const { addMessageToHistory } = chatSlice.actions

export default chatSlice.reducer
```

## 2. เรียกใช้ Thunk จากใน Component 

เราจะทำการเรียกใช้ thunk และส่งข้อความไปหา openAI จากใน PromptInputComponent

```js
// src/components/promptInput/PromptInputComponent.js

import React, { useState } from 'react'
import { Row, Col, Form, Button } from 'react-bootstrap';
import { addMessageToHistory } from '../../redux/chatSlice';
import { useDispatch } from 'react-redux';

// import askAI thunk เพื่อใช้งาน
import { askAI } from '../../redux/askAIThunk';

function PromptInputComponent() {

    const [message, setMessage] = useState("")
    const dispatch = useDispatch()

    const handleChange = (e) => {
        e.preventDefault()
        setMessage(e.target.value)
    };

    const onSubmit = () => {
        const actionObject = addMessageToHistory(message)
        dispatch(actionObject)

        // เรียกใช้งาน askAI thunk โดยส่งข้อความ message ให้เป็น parameter
        const thunkAction = askAI(message)
        dispatch(thunkAction)
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
