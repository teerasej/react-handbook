
# 9. เรียกใช้ Azure OpenAI Service


## เรียนรู้เพิ่มเติม

- [เริ่มต้นเรียนรู้ ใช้งาน Azure OpenAI Service](https://learn.nextflow.in.th/azure-openai-service)
- [เริ่มต้นเรียนรู้ ทำแอพ AI ด้วย Semantic Kernel ฉบับคนใช้ Python](https://learn.nextflow.in.th/getting-started-with-semantic-kernel)

## 1. สร้าง thunk ชื่อ askAI

ใน Redux เราสามารถสร้าง thunk เพื่อจัดการกับการทำงานที่เป็น asynchronous ได้ เช่น การเรียก API หรือการทำงานที่ใช้เวลาในการทำงาน
เราจะสร้าง thunk ชื่อ `askAI` เพื่อเรียกใช้ OpenAI API และส่ง

สร้างไฟล์​ `src/redux/askAIThunk.js`

```js
// src/redux/askAIThunk.js

// import createAsyncThunk 
import { createAsyncThunk } from '@reduxjs/toolkit'
// import axios ในการส่ง request
import axios from 'axios';

// นำ key ของ OpenAI มาใช้งาน
const key = 'AL6VOGtIdqUhC0OSZpDYebzXaOam8O1xFblOsyWFzMSecWVCsk4fJQQJ99BGACYeBjFXJ3w3AAABACOGqWAT';

// สร้าง AsyncThunk ชื่อ askAI
export const askAI = createAsyncThunk(
  // ตั้งชื่อ async thunk
  'user/askAI',

  // รับค่าที่เข้ามาใช้่งาน ในที่นี้คือ prompt message
  async (prompt) => {

    console.log('Asking AI...')

    // สร้าง JSON object ในการส่งไปที่ OpenAI API
    const jsonPrompt = JSON.stringify({
      "messages": [
        {
          "role": "system",
          "content": "You are an AI assistant that helps people find information. You will answer questions in short responsee and precisely. If you don't know the answer, say 'I don't know'."
        },
        {
          "role": "user",
          "content": prompt
        },
      ],
      "temperature": 0.7,
      "max_tokens": 800
    });

    console.log('Sending prompt:')
    console.log(jsonPrompt)

    // ใช้ axios ส่ง request โดยการกำหนด key และ json 
    const response = await axios.post(
      'https://scbreact-2025-july.openai.azure.com/openai/deployments/gpt-4o-mini/chat/completions?api-version=2025-01-01-preview',
      jsonPrompt,
      {
        headers: {
          'Content-Type': 'application/json',
          'api-key': key
        }
      });

    console.log('got response:')
    console.log(response)
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
          sender: 'User',
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

## 3. เรียกใช้ Thunk จากใน Component 

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

        setMessage("")
    };

    const handleKeyDown = (e) => {
        if (e.key === 'Enter') {
            onSubmit(e);
        }
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
                            onKeyDown={handleKeyDown}
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


