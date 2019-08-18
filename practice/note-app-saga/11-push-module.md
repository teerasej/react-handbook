
# 11. ใช้งาน Push module ในการ redirect

- [อ้่างอิงการใช้งาน React Router - Push Module](https://github.com/supasate/connected-react-router/blob/master/FAQ.md#how-to-navigate-with-redux-action)

## 1. สร้าง UI ของหน้า Login 

เปิดไฟล์​ `src/pages/login-page/LoginPage.js`

```js
import React, { Component } from 'react'
import { connect } from 'react-redux'
import { Layout, Form, Icon, Input, Button } from 'antd';
import actions from '../../redux/actions';
const { Content } = Layout;

export class LoginPage extends Component {

    handleSubmit = e => {
        e.preventDefault();

        this.props.form.validateFields((err, values) => {
            if (!err) {
                const username = this.props.form.getFieldValue('username');
                const password = this.props.form.getFieldValue('password');
                console.log('Username:', username, 'Password:', password);

                
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

const mapDispatchToProps = dispatch => {
   
}

LoginPage = Form.create({})(LoginPage);
export default connect(mapStateToProps, mapDispatchToProps)(LoginPage)
```

## 2. ใช้ push module ของ `connected-react-router` มาใช้ในการ redirect ก่อน

เปิดไฟล์ `src/pages/login-page/LoginPage.js`

import push module 

```js
import { push } from 'connected-react-router'
```

และประกาศใช้ใน `mapDispatchToProps`

```js
const mapDispatchToProps = (dispatch) => {
    return {
        passLogin: () => dispatch(push('/home'))
    }
}
```

และเอามาใช้ bypass ส่วนของการเช็ค sign in ก่อน

```js
handleSubmit = e => {
    e.preventDefault();

    this.props.form.validateFields((err, values) => {
        if (!err) {
            const username = this.props.form.getFieldValue('username');
            const password = this.props.form.getFieldValue('password');
            console.log('Username:', username, 'Password:', password);

            this.props.passLogin();
        }
    });
}
```

ทดสอบการทำงาน จะเห็นว่าเราสามารถกด sign in และเปิดมาที่หน้า home ได้เลย