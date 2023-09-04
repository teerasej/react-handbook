
# 6. สร้าง Reducer Slice

## 1. สร้างไฟล์ reducer

สร้างไฟล์ `src/redux/messageSlice.js`

```js
// src/redux/messageSlice.js

import { createSlice } from '@reduxjs/toolkit'

const initialState = [
  { id:1, sender: 'User', message: 'Hello' },
  { id:2, sender: 'GPT', message: 'Hi!' }
]

const messageSlice = createSlice({
  name: 'messages',
  initialState,
  reducers: {
    
  },
});

export const {  } = messageSlice.actions

export default messageSlice.reducer
```

## 2. setup reducer เข้าไปใน store 

```js
// src/redux/store.js

import { configureStore } from '@reduxjs/toolkit'
import messageSlice from './messageSlice'

export default configureStore({
  reducer: {
    // ตั้งชื่อ reducer สำหรับเรียกใช้ใน component
    messages: messageSlice
  }
})
```

## 3. เรียกใช้งาน redux store ผ่าน useSelector()

```js
// src/components/chatroom/Chatroom.js

import React from 'react'
import { Container, Row } from 'react-bootstrap';
import ChatMessage from './ChatMessage'

// เรียกใช้ useSelector()
import { useSelector } from 'react-redux';

function Chatroom() {

  // เรียกใช้งาน Redux state เพื่อดึงค่าจาก reducer ที่กำหนดชื่อว่า messages ไว้ออกมาใช้งานแทน
  const publishedMessage = useSelector(state => state.messages);

  const renderedMessage = publishedMessage.map(message => (
    <ChatMessage key={message.id} sender={message.sender} message={message.message}/>
  ))

  return (
    <Row>
        <Container>
          <div className="chatroom">
            {
              renderedMessage
            }
          </div>
        </Container>
    </Row>
  )
}

export default Chatroom
```