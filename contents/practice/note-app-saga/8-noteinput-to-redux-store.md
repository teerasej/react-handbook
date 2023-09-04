# 8. เชื่อม NoteInputBox Component เข้า Redux Store

## 1. Mapping component เข้ากับ redux connect เพื่อสร้างเป็น component container

![Paper React   React Native 28](https://user-images.githubusercontent.com/85179/63178859-15258d80-c075-11e9-9a0b-359a3743f06c.png)

React Component ทั่วไป จะไม่มีการเชื่อมกับระบบ Redux ตั้งแต่แรก แต่เราสามารถจับมันมาตั้งค่าให้ใช้งานกับ Redux ได้

เราเรียก Component พวกนี้ว่า **Redux Container** หรือ **Component Container** ครับ

เริ่มจาก `src/pages/home-page/NoteInputBox.js`

เรา import module ชื่อ `connect` เข้ามาก่อน 

```js
// ใช้ snippet 'redux' ได้
import { connect } from "react-redux";
```

จากนั้นเราจะย้าย `export default` จากที่ใช้กับ Class ของเรา ไปใช้กับส่วนอื่น

```js
export default class NoteInputBox extends Component {

// เป็น

class NoteInputBox extends Component {
```

ซึ่งเราจะมาเขียนในส่วนด้านล่างแทน

เป็น

```js
// snippet 'reduxmap'
const mapStateToProps = (state) => ({

})

const mapDispatchToProps = {

}


export default connect(mapStateToProps, mapDispatchToProps)(NoteInputBox) 
```

### ไฟล์เต็ม `src/pages/home-page/NoteInputBox.js`

```js
import React, { Component } from 'react'
import { Form, Input, Button, Card } from 'antd';
import { connect } from 'react-redux'


class NoteInputBox extends Component {
    render() {
        return (
            <Card>
                <Form>
                    <Form.Item layout="inline" label="Message">
                        <Input
                            placeholder="message"
                        />
                    </Form.Item>
                    <Form.Item>
                        <Button type="primary" htmlType="submit">
                            Save
                        </Button>
                    </Form.Item>
                </Form>
            </Card>
        )
    }
}

const mapStateToProps = (state) => ({
    
})

const mapDispatchToProps = {
    
}

export default connect(mapStateToProps, mapDispatchToProps)(NoteInputBox)
```

## 2. เชื่อม props ของ NoteInputBox เข้ากับ dispatch function เพื่อส่ง action เข้า Redux store

ในที่นี้เราสามารถใช้ Action ที่เราสร้างขึ้น ส่ง object เข้า Redux Store ผ่าน การกำหนด function ให้กับ props ผ่าน `mapDispatchToProps` ได้แบบด้านล่าง 

```js
// import action ที่เขียนไว้เข้ามาก่อน
import Actions from '../../redux/actions'

// การนั้น return object ที่กำหนด function ไปเรียกใช้คำสั่ง 'dispatch' ซึ่งจะเป็นคนส่ง action object เข้า Redux store
const mapDispatchToProps = (dispatch) => {
    return {
        saveNote: (message) => dispatch(Actions.saveNewNote(message))
    }
}
```


## 3. ใช้กลไกของ Form ใน AntDesign ดึงค่าออกมาจาก Form.Input

เราจะเขียน function ให้ทำงานหลังจากที่ Form ถูก submit แล้ว 

```js
handleSubmit = e => {
    e.preventDefault();
    const message = this.props.form.getFieldValue('message');
    console.log(message);
}
```

และเราในส่วนของ `render()` เราจะเขียนใหม่ดังนี้ 

```js
render() {

        const { getFieldDecorator } = this.props.form;


        return (
            <Card>
                <Form onSubmit={this.handleSubmit}> 
                    <Form.Item >
                        {getFieldDecorator('message', {
                            rules: [{ required: true, message: 'Please input your message!' }],
                        })(
                            <Input
                                placeholder="message"
                            />,
                        )}
                    </Form.Item>
                    <Form.Item>
                        <Button type="primary" htmlType="submit">
                            Save
                        </Button>
                    </Form.Item>
                </Form>
            </Card>
        )
    }
```

1. เรามีการใช้ `getFieldDecorator` เพื่อกำหนด Validation rule และตั้งชื่อ form field ว่า **message**

```js
const { getFieldDecorator } = this.props.form;
```

2. เราใส่ event `onSubmit` ให้กับ `<Form>`

```jsx
<Form onSubmit={this.handleSubmit}>
```

3. เราเอา ในขั้นตอนที่ 1 มาใช้กับ `<Form.Input>` ของเรา

```js
<Form.Item >
    {getFieldDecorator('message', {
        rules: [{ required: true, message: 'Please input your message!' }],
    })(
        <Input
            placeholder="message"
        />,
    )}
</Form.Item>
```

## 4. แก้ไขส่วนของ export default ให้มีการใช้คุณสมบัติที่ Ant Design ออกแบบไว้ให้ Form กับ NoteInputBox

```js
NoteInputBox = Form.create({})(NoteInputBox);
export default connect(mapStateToProps, mapDispatchToProps)(NoteInputBox)
```

## 5. เรียกใช้ function ของ props ที่สร้างไว้ เพื่อส่ง action เข้า Redux store

```js
handleSubmit = e => {
    e.preventDefault();

    const message = this.props.form.getFieldValue('message')
    console.log(message);

    this.props.saveNote(message);
}
```

### ไฟล์เต็ม `src/pages/home-page/NoteInputBox.js`

```js
import React, { Component } from 'react'
import { Form, Input, Button, Card } from 'antd';
import { connect } from 'react-redux'
import Actions from '../../redux/actions'


class NoteInputBox extends Component {

    handleSubmit = e => {
        e.preventDefault();

        const message = this.props.form.getFieldValue('message')
        console.log(message);

        this.props.saveNote(message);
    }


    render() {

        const { getFieldDecorator } = this.props.form;


        return (
            <Card>
                <Form onSubmit={this.handleSubmit}>
                    <Form.Item >
                        {getFieldDecorator('message', {
                            rules: [{ required: true, message: 'Please input your message!' }],
                        })(
                            <Input
                                placeholder="message"
                            />,
                        )}
                    </Form.Item>
                    <Form.Item>
                        <Button type="primary" htmlType="submit">
                            Save
                        </Button>
                    </Form.Item>
                </Form>
            </Card>
        )
    }
}

const mapStateToProps = (state) => ({

})

const mapDispatchToProps = (dispatch) => {
    return {
        saveNote: (message) => dispatch(Actions.saveNewNote(message))
    }
}

NoteInputBox = Form.create({})(NoteInputBox);
export default connect(mapStateToProps, mapDispatchToProps)(NoteInputBox)

```
