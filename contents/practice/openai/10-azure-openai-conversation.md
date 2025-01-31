
# การคุยต่อเนื่อง Conversation ด้วย Azure OpenAI Service

หลักการของ Continuous Conversation คือการสร้างระบบที่สามารถคุยต่อเนื่องกับผู้ใช้ได้ โดยไม่ต้องรอผลลัพธ์ก่อนหน้า และสามารถเรียนรู้จากการคุยกับผู้ใช้ด้วย ซึ่งจริงๆ แล้วคือการส่งข้อความใน chat history เดิม รวมกับข้อความใหม่เข้าไปในระบบ และระบบจะตอบกลับด้วยข้อความที่สอดคล้อง และเกี่ยวข้องกับข้อความทั้งหมด

## ดึงข้อมูลจาก Chat History และส่งไปให้ Azure OpenAI Service

```javascript
// src/redux/askAIThunk.js

import { createAsyncThunk } from '@reduxjs/toolkit'
import axios from 'axios';

const key = '';

export const askAI = createAsyncThunk(
  'user/askAI',

 // เรียกใช้ getState function เพื่อเข้าถึง redux store จากภายใน thunk
  async (prompt , { getState }) => {

    console.log('fetching openAI')

    // ดึง chat history จาก redux store
    const chatHistory = getState().chatroom.chatHistory;

    // สร้าง JSON object ในการส่งไปที่ OpenAI API
    const jsonPrompt = JSON.stringify({
      "messages": [
        {
          "role": "system",
          "content": "You are an AI assistant that helps people find information."
        },
        // ส่วนนี้คือการส่ง chat history ทั้งหมดไปให้ OpenAI API
        ...chatHistory.map(chatMessage => ({
            role: chatMessage.sender === 'User' ? 'user' : 'assistant',
            content: chatMessage.text
        })),
        // จากนั้นเพิ่มข้อความใหม่ที่ผู้ใช้พิมพ์เข้าไป
        {
            role: 'user',
            content: prompt
        }
      ],
      "temperature": 0.7,
      "top_p": 0.95,
      "frequency_penalty": 0,
      "presence_penalty": 0,
      "max_tokens": 800,
      "stop": null
    });

    console.log('Sending prompt:')
    console.log(jsonPrompt)

    const response = await axios.post(
      'https://openai-nextflow.openai.azure.com/openai/deployments/gpt-4/chat/completions?api-version=2024-02-15-preview',
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

    return response.data.choices[0].message.content;

  }
);
```
