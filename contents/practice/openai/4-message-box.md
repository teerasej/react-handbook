
# 4. สร้าง Message Box 

## 1. สร้างไฟล์ CSS 

สร้างไฟล​์ `src/components/chatHistory/ChatMessage.css`

```css
/* src/components/chatHistory/ChatMessage.css */
.msg-bubble {
    
    margin-bottom: 5px;
    background-color: darkgrey;
} 

.msg-bubble .sender {
    padding: 10px;
    background-color: grey;
}

.msg-bubble .message {
    padding: 10px;
}
```

## 2. สร้างไฟล์ JavaScript

สร้าง `src/components/chatHistory/ChatMessageComponent.js`

```js

//src/components/chatHistory/ChatMessageComponent.js

import './ChatMessage.css';
import { Col, Container, Row } from 'react-bootstrap'

function ChatMessageComponent(props) {

  let { sender, text } = props
  // สามารถใช้วิธีด้านล่างนี้ได้เหมือนกัน
  // let sender = props.sender
  // let text = props.text

  return (
    <Container>
      <Row className='msg-bubble'>
        <Col xs={1} className='sender'>
          {sender}
        </Col>
        <Col md="auto" className='message'>
          {text}
        </Col>
      </Row>
    </Container>
  )
}

export default ChatMessageComponent
```

## 3. เพิ่มมาใช้งานใน Chatroom.js

เพิ่ม component ใช้งานใน `src/components/chatHistory/ChatHistoryComponent.js` 

```jsx
// src/components/chatHistory/ChatHistoryComponent.js

import React from 'react'
import { Container, Row } from 'react-bootstrap';
import ChatMessageComponent from './ChatMessageComponent'

function ChatHistoryComponent() {

    // สร้าง Array เป็นตัวแทนของ message ที่จะแสดงใน chat history 
    const chatHistory = [
        { id: 1, sender: 'User', message: 'Hello' },
        { id: 2, sender: 'GPT', message: 'Hi' }
    ];

    // สร่้าง Array ของ JSX 
    const renderedMessage = chatHistory.map(chatMessage => (
        <ChatMessageComponent
            key={chatMessage.id}
            sender={chatMessage.sender}
            text={chatMessage.text} />
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
