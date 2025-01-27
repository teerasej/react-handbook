
# 3. สร้าง ChatHistoryComponent

## 1. สร้าง Chat History Component

```js
// src/components/chatHistory/ChatHistoryComponent.js

import React from 'react';
import { Container, Row } from 'react-bootstrap';
import './ChatMessage.css'; // Import the CSS file

function ChatHistoryComponent() {
  return (
    <Row>
      <Container>
        <div className="chatroom">
          {/* Chat messages will go here */}
        </div>
      </Container>
    </Row>
  );
}

export default ChatHistoryComponent;
```
## 2. กำหนด style ให้ chat history

```css
// src/components/chatHistory/ChatMessage.css
.chatroom {
  border: 1px solid #ccc;
  padding: 10px;
  border-radius: 5px;
}
```


## 3. เพิ่ม Component ใน App.js 

```js
// src/App.js

import './App.css';
import { Container, Row, Col } from 'react-bootstrap';
import PromptInputComponent from './components/promptInput/PromptInputComponent';
import ChatHistoryComponent from './components/chatHistory/ChatHistoryComponent';

function App() {
  return (
    <Container>
      <Row>
        <Col>
          <h1>My GPT</h1>
        </Col>
      </Row>
      {/* ChatHistoryComponent */}
      <ChatHistoryComponent/>
      {/* PromptInput */}
      <PromptInputComponent/>
    </Container>
  );
}

export default App;

```
