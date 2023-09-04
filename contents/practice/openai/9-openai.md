
# 9. เรียกใช้ OpenAI API

1. [สมัครใช้งาน OpenAI Developer](https://platform.openai.com/)
   - สามารถสร้าง account ใหม่ โดยการใช้ Google Account หรือ Microsoft Account ได้ 
   - จะมีการกรอกเบอร์โทรศัพท์ และรับ SMS-OTP เพื่อยืนยันตัวตน
2. [สร้างและใช้ OpenAI API Key](https://platform.openai.com/account/api-keys) 

## 1. สร้าง async action ชื่อ fetchOpenAI

```js
// src/redux/messageSlice.js

// import createAsyncThunk เพื่อสร้าง Async action
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit'
// import axios ในการส่ง request
import axios from 'axios';

// นำ key ของ OpenAI มาใช้งาน
const key = '';

const initialState = [
  { id: 1, sender: 'User', message: 'Hello' },
  { id: 2, sender: 'GPT', message: 'Hi!' }
]

// สร้าง AsyncThunk ชื่อ fetchOpenAI
export const fetchOpenAI = createAsyncThunk(
    // ตั้งชื่อ async thunk
  'user/fetchOpenAI',

  // รับค่าที่เข้ามาใช้่งาน ในที่นี้คือ prompt message
  async (prompt) => {

    console.log('fetching openAI')

    // สร้าง JSON object ในการส่งไปที่ OpenAI API
    const jsonPrompt = JSON.stringify({
      "model": "gpt-3.5-turbo",
      "messages": [{"role": "user", "content": prompt}]
    });

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

    // ดึงเฉพาะส่วนข้อความที่ API ตอบกลับมา
    return response.data.choices[0].message.content;
  
  }
);


const messageSlice = createSlice({
  name: 'messages',
  initialState,
  reducers: {
    messageAdded(state, action) {
      state.push({...action.payload, id: Math.floor(Math.random() * 1000)})
    },
  },
  // กำหนด reducer พิเศษ ที่จะทำงานตามกรณีของ AsyncThunk
  extraReducers: (builder) => {
    // กำหนด case ที่จะทำงานจากกลไกของ async thunk
    builder
      .addCase(fetchOpenAI.fulfilled, (state, action) => {
        console.log('succeeded');

        // ถ้าได้การทำงานของ Async thunk สมบูรณ์ ค่าที่ return ออกมาจะอยู่ใน payload 
        // ในที่นี้เราจะเอามาใส่เพิ่มในห้องแชท โดยกำหนดชื่อ sender เป็น GPT
        state.push({ sender:'GPT', message: action.payload, id: Math.floor(Math.random() * 1000)})
        
      })
      // ในกรณีที่ Async Thunk ล้มเหลวจะเข้าเคสนี้
      .addCase(fetchOpenAI.rejected, (state, action) => {
        console.log('failed');
        // แสดงข้อความ error ที่ได้
        console.error(action.error.message);
      });
  },
});

export const { messageAdded } = messageSlice.actions

export default messageSlice.reducer
```

## 2. เรียกใช้ async action 

```js

const onSubmit = (data) => {

        console.log(data);
        dispatch(
            messageAdded({
                sender: 'User',
                message: data.message
            })
        )

        // Dispatch Async thunk action โดยการส่งข้อความเป็น prompt
        dispatch(fetchOpenAI(data.message))

        reset();
    };
```
