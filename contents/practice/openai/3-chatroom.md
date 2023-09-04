
# 3. สร้าง ChatHistoryComponent

```js
// src/components/chatHistory/ChatHistoryComponent.js

import React from 'react'
import { Container, Row } from 'react-bootstrap';

function ChatHistoryComponent() {

  return (
    <Row>
        <Container>
          <div className="chatroom">
            
          </div>
        </Container>
    </Row>
  )
}

export default ChatHistoryComponent
```

## เพิ่ม Component ใน App.js 

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