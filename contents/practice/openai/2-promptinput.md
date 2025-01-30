
## 2. สร้าง PromptInput

ให้ทำการสร้าง User Interface ของ **PromptInputComponent** ตามด้านล่าง

```js
// src/components/promptInput/PromptInputComponent.js


import React, { useState } from 'react'
import { Row, Col, Form, Button } from 'react-bootstrap';

function PromptInputComponent() {
    
    const [message, setMessage] = useState("")

    const handleChange = (e) => {
        e.preventDefault(); 
        setMessage(e.target.value); 
    };

    const handleKeyDown = (e) => {
        if (e.key === 'Enter') {
            console.log('Enter');
            handleSubmit(e);
        }
    };

    const handleSubmit = (e) => {
        console.log(message);
    }

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
                    <Button variant="primary" onClick={handleSubmit}>
                        Send
                    </Button>
                </Form>
            </Col>
        </Row>
    )
}

export default PromptInputComponent
```

## 2. นำมาแสดงใน App 

จากนั้นทำ PromppInputComponent มาแสดงใน `src/App.js`

```jsx
import './App.css';
import { Container, Row, Col } from 'react-bootstrap';
import PromptInputComponent from './components/promptInput/PromptInputComponent';

function App() {
  return (
    <Container>
      <Row>
        <Col>
          <h1>My GPT</h1>
        </Col>
      </Row>
      {/* ChatHistoryComponent */}

      {/* PromptInputComponent */}
      <PromptInputComponent/>
    </Container>
  );
}

export default App;

```
