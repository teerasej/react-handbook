
# สร้าง Chatroom

```js
// src/components/chatroom/Chatroom.js

import React from 'react'
import { Container, Row } from 'react-bootstrap';
function Chatroom() {

  return (
    <Row>
        <Container>
          <div className="chatroom">
            
          </div>
        </Container>
    </Row>
  )
}

export default Chatroom
```

## เพิ่ม Component ใน App.js 

```js
// src/App.js

import './App.css';
import { Container, Row, Col } from 'react-bootstrap';
import Chatroom from './components/chatroom/Chatroom';
import PromptInput from './components/promptinput/PromptInput';

function App() {
  return (
    <Container>
      <Row>
        <Col>
          <h1>My GPT</h1>
        </Col>
      </Row>
      {/* Chatroom */}
      <Chatroom/>
      {/* PromptInput */}
      <PromptInput/>
    </Container>
  );
}

export default App;
```