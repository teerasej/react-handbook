
# สร้าง Message Box 

## 1. สร้างไฟล์ CSS 

สร้างไฟล​์ `src/components/chatroom/ChatMessage.css`

```css
/* src/components/chatroom/ChatMessage.css */
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

สร้าง `src/components/chatroom/ChatMessage.js`

```js

import './ChatMessage.css';
import { Col, Container, Row } from 'react-bootstrap'

function ChatMessage(props) {

  let { sender, message } = props

  return (
    <Container>
      <Row className='msg-bubble'>
        <Col xs={1} className='sender'>
          {sender}
        </Col>
        <Col md="auto" className='message'>
          {message}
        </Col>
      </Row>
    </Container>
  )
}

export default ChatMessage
```

## 3. เพิ่มมาใช้งานใน Chatroom.js

เพิ่ม component ใช้งานใน `src/components/chatroom/Chatroom.js` 

```js

import React from 'react'
import { Container, Row } from 'react-bootstrap';
import ChatMessage from './ChatMessage'
function Chatroom() {

  // สร้าง Array ตัวแทนของ message 
  const publishedMessage = [
    { id: 1, sender: 'User' , message:'Hello'},
    { id: 2, sender: 'GPT' , message:'Hi'}
  ];

  // สร่้าง Array ของ JSX 
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