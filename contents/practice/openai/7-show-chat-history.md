
# 7. แสดงข้อความใน Chat History component

ใช้ react hook function ชื่อ `useSelector` เพื่อดึงข้อมูลจาก Reducer's state มาแสดงใน `ChatHistoryComponent`

```jsx
// src/components/chatHistory/ChatHistoryComponent.js

import React from 'react'
import { Container, Row } from 'react-bootstrap';
import ChatMessageComponent from './ChatMessageComponent'
// เรียกใช้ useSelector()
import { useSelector } from 'react-redux';

function ChatHistoryComponent() {

    // เรียกใช้งาน useSelector() เพื่อดึงค่า chatHistory จาก reducer ที่กำหนดชื่อว่า chatroom ไว้ออกมาใช้งานแทน
  const chatHistory = useSelector(state => state.chatroom.chatHistory);

    // สร่้าง Array ของ JSX 
    const renderedMessage = chatHistory.map(message => (
        <ChatMessageComponent
            key={message.id}
            sender={message.sender}
            text={message.text} />
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

export default ChatHistoryComponent
```
