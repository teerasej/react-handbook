# 16. ทดสอบใช้งาน saga ผ่าน Action

## 1. แก้ไข props ให้สามารถ dispatch action ที่ saga รอรับ 

จะเห็นว่า saga ของเรารอ action ที่ชื่อ `INCREMENT_ASYNC`

```js
// src/redux/sagas/increment.saga.js
function* watchIncrementAsync() {
    yield takeEvery('INCREMENT_ASYNC', incrementAsync)
}
```

เราจึงเลือก LoginPage เป็นตัวที่จะ dispatch action ดังกล่าว 

เปิดไฟล์ `src/pages/login-page/LoginPage.js`

และแก้ไขส่วนของ `mapDispatchToProps` โดยการเพิ่ม function ลงไป 

```js
activateIncrementSaga: () => dispatch({ type: 'INCREMENT_ASYNC'})
```

```js
const mapDispatchToProps = (dispatch) => {
    return {
        startSignIn: (authInfo) => dispatch(actions.startSignIn(authInfo)),
        passLogin: () => dispatch(push('/home')),
        activateIncrementSaga: () => dispatch({ type: 'INCREMENT_ASYNC'})
    }
}
```
และทดสอบใช้งานใน function `handleSubmit`

```js
 handleSubmit = e => {
        e.preventDefault();

        this.props.form.validateFields((err, values) => {
            if (!err) {
                
                //...

                this.props.activateIncrementSaga();
            }
        });
    }
```

จากนั้นให้ทดสอบกดปุ่ม sign in และดูการทำงาน ผ่าน console 

## ไฟล์เต็ม `src/pages/login-page/LoginPage.js`

```jsx
import React, { Component } from 'react'
import { connect } from 'react-redux'
import { Layout, Form, Icon, Input, Button } from 'antd';
import actions from '../../redux/actions';

import { push } from 'connected-react-router'

const { Content } = Layout;

class LoginPage extends Component {

    handleSubmit = e => {
        e.preventDefault();

        this.props.form.validateFields((err, values) => {
            if (!err) {
                const username = this.props.form.getFieldValue('username');
                const password = this.props.form.getFieldValue('password');
                console.log('Username:', username, 'Password:', password);

                // this.props.startSignIn({ username: username, password: password });
                // this.props.passLogin();
                this.props.activateIncrementSaga();
            }
        });
    }

    render() {

        const { getFieldDecorator } = this.props.form;

        return (
            <Content style={{
                padding: '0 50px'
            }}>
                <div
                    style={{
                        background: '#fff',
                        padding: 24,
                        minHeight: 280
                    }}>
                    <Form onSubmit={this.handleSubmit} className="login-form">
                        <Form.Item>
                            {getFieldDecorator('username', {
                                rules: [{ required: true, message: 'Please input your username!' }],
                            })(
                                <Input
                                    prefix={<Icon type="user" style={{ color: 'rgba(0,0,0,.25)' }} />}
                                    placeholder="Username"
                                />,
                            )}
                        </Form.Item>
                        <Form.Item>
                            {getFieldDecorator('password', {
                                rules: [{ required: true, message: 'Please input your password!' }],
                            })(
                                <Input
                                    prefix={<Icon type="lock" style={{ color: 'rgba(0,0,0,.25)' }} />}
                                    type="password"
                                    placeholder="Password"
                                />,
                            )}
                        </Form.Item>
                        <Form.Item>
                            <Button type="primary" htmlType="submit" className="login-form-button">
                                Sign in
                            </Button>
                        </Form.Item>
                    </Form>
                </div>
            </Content>
        )
    }
}

const mapStateToProps = (state) => ({

})

const mapDispatchToProps = (dispatch) => {
    return {
        startSignIn: (authInfo) => dispatch(actions.startSignIn(authInfo)),
        passLogin: () => dispatch(push('/home')),
        activateIncrementSaga: () => dispatch({ type: 'INCREMENT_ASYNC'})
    }
}

LoginPage = Form.create({})(LoginPage);
export default connect(mapStateToProps, mapDispatchToProps)(LoginPage)
```