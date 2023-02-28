
## 2. สร้าง PromptInput

```js
// src/components/promptinput/PromptInput.js

import React from 'react'
import { Row, Col, Form, Button } from 'react-bootstrap';

function PromptInput() {
    return (
        <Row>
            <Col>
                <Form>
                    <Form.Group controlId="message">
                        <Form.Label>Message</Form.Label>
                        <Form.Control type="text" placeholder="Type your message here"/>
                        <Form.Control.Feedback type="invalid">
                            Please enter a message.
                        </Form.Control.Feedback>
                    </Form.Group>
                    <Button variant="primary" type="submit">
                        Send
                    </Button>
                </Form>
            </Col>
        </Row>
    )
}

export default PromptInput
```