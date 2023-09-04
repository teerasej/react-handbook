
# 8. ใช้งาน react-hook-form

```js
// src/components/promptinput/PromptInput.js

import React from 'react'
import { Row, Col, Form, Button } from 'react-bootstrap';
import { useDispatch } from 'react-redux';
import { messageAdded } from '../../redux/messageSlice';

// เรียกใช้ react-hook-form
import { Controller, useForm } from 'react-hook-form';

function PromptInput() {

    const dispatch = useDispatch();

    // เรียกใช้ hook เพื่อนำส่วนประกอบต่างๆ มาใช้งาน
    const { control, handleSubmit, reset } = useForm();

    // ข้อมูลที่ได้จาก react-hook-form จะถูกส่งเข้ามาใน function
    const onSubmit = (data) => {

        console.log(data);
        dispatch(
            // ปรับให้ส่งข้อความจาก form ไปที่ redux
            messageAdded({
                sender: 'User',
                message: data.message
            })
        )

        // reset ฟอร์มด้วย react-hook-form
        reset();
    };


    return (
        <Row>
            <Col>
                 {/* กำหนด handleSubmit ให้กับ event และกำหนดเรียกใช้ function ที่เตรียมเอาไว้ */}
                <Form onSubmit={handleSubmit(onSubmit)}>
                    <Form.Group controlId="message">
                        <Form.Label>Message</Form.Label>

                         {/* 
                            ใช้ Controller component ในการ render Form Component
                            1. กำหนดชื่อ controller เพื่อให้เรียกใช้ค่า property ในชื่อเดียวกันได้
                            2. กำหนด control 
                            3. กำหนดค่าเริ่มต้นใน controller 
                            4. กำหนด rule ที่ต้องการใข้งาน
                            5. กำหนด render function ที่ return ค่าเป็น JSX component
                            6. ส่งผ่าน field property เข้าไปใน prop ของ JSX component เพื่อใช้งาน
                         */}
                        <Controller
                            name="message"
                            control={control}
                            defaultValue=""
                            rules={{ required: true }}
                            render={({ field, fieldState: { error } }) => (
                                <Form.Control type="text" placeholder="Type your message here" isInvalid={!!error} {...field} />
                            )}
                        />
                        <Form.Control.Feedback type="invalid">
                            Please enter a message.
                        </Form.Control.Feedback>
                    </Form.Group>
                    
                    {/* ปรับ type เป็น submit เพื่อให้ทำงานกับ redux hook form ได้ */}
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