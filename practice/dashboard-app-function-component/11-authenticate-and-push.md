
# สร้าง Action ที่จัดการเรื่อง Login และเปิดหน้า dashboard

## 1. กำหนด Action และ Action function 

เปิดไฟล์ `src/redux/actions.js`

เริ่มจากกำหนดประเภทของ Action Type 

```js
const ActionTypes = {
    //...

    USER_LOGIN_START: "USER_LOGIN_START",
    USER_LOGIN_SUCCESS: "USER_LOGIN_SUCCESS",
    USER_LOGIN_FAILED: "USER_LOGIN_FAILED"
}
```

และเขียน Action function สำหรับรับ username และ password มาทำการ authenticate ตอนนี้เราจำลองการเช็คก่อน

```js
const userLogin = (username, password) => {
    return async dispatch => {
        try {
            if(username === 'a' && password === '1234'){
                dispatch({ type: ActionTypes.USER_LOGIN_SUCCESS, payload: 'ok' })
            } else {
                dispatch({ type: ActionTypes.USER_LOGIN_FAILED, payload: 'fail reason' })
            }

        } catch (error) {
            console.error(error);
        }
    }
}
```
และอย่าลืม export เอาไปใช้งาน 

```js
export default {
    requestInitData,
    showBranchData,
    userLogin,
    ActionTypes
}
```

### ไฟล์เต็ม Actions.js

```js
import API from "../services/API";

const ActionTypes = {
    SHOW_BRANCH_DATA: "SHOW_BRANCH_DATA",
    REQUEST_INIT_DATA: "REQUEST_INIT_DATA",
    REQUEST_INIT_DATA_SUCCESS: "REQUEST_INIT_DATA_SUCCESS",
    REQUEST_INIT_DATA_FAILED: "REQUEST_INIT_DATA_FAILED",

    USER_LOGIN_START: "USER_LOGIN_START",
    USER_LOGIN_SUCCESS: "USER_LOGIN_SUCCESS",
    USER_LOGIN_FAILED: "USER_LOGIN_FAILED"
}


const showBranchData = (branchId) => ({
    type: ActionTypes.SHOW_BRANCH_DATA,
    payload: branchId
})

const requestInitData = () => {
    return async (dispatch) => {
        let onSuccess = (success) => {
            console.log('load data success');
            return success;
        }

        let onError = (error) => {
            console.log(`Load data error: ${error}`);
            return error;
        }
 
        try {
            const success = await API.get('https://www.nextflow.in.th/api/branch-info/branch.json');
            console.log(success.data);
            dispatch({ type: ActionTypes.REQUEST_INIT_DATA_SUCCESS, payload: success.data })
            return onSuccess(success);
        } catch (error) {
            dispatch({ type: ActionTypes.REQUEST_INIT_DATA_FAILED, payload: error })
            return onError(error)
        }
    }
}

const userLogin = (username, password) => {
    return async dispatch => {
        try {
            if(username === 'a' && password === '1234'){
                dispatch({ type: ActionTypes.USER_LOGIN_SUCCESS, payload: 'ok' })
            } else {
                dispatch({ type: ActionTypes.USER_LOGIN_FAILED, payload: 'fail reason' })
            }
            
        } catch (error) {
            console.error(error);
        }
    }
}

export default {
    requestInitData,
    showBranchData,
    userLogin,
    ActionTypes
}
```

## 2. Dispatch Action จาก Login Page

เปิดไฟล์ `src/pages/LoginPage.js`

ทำการเอา Action มาใช้กับ `mapDispatchToProps`

```js
import Actions from "../redux/actions";

const mapDispatchToProps = dispatch => ({
    login: (username, password) => dispatch(Actions.userLogin(username, password))
})
```

และเรียกใช้ใน `handleSubmit()` เพื่อส่ง username และ password เข้าสู่การ login

```js
handleSubmit = (e) => {
    e.preventDefault();
    console.log(`Loggin in with U:${this.state.username} P:${this.state.password}`)
    this.props.login(this.state.username, this.state.password);
}

```

### ไฟล์เต็ม LoginPage.js

```js
import React, { Component, useState } from 'react'
import PropTypes from 'prop-types'
import { connect } from 'react-redux'
import { Form, Icon, Input, Button, Checkbox } from 'antd';
import Actions from "../redux/actions";

export class LoginPage extends Component {
    
    state = {
        username: '',
        password: ''
    }

    handleSubmit = (e) => {
        e.preventDefault();
        console.log(`Loggin in with U:${this.state.username} P:${this.state.password}`)
        this.props.login(this.state.username, this.state.password);
    }

    setUsername = (value) => {
        this.setState({
            ...this.state,
            username: value
        })
        // console.log(this.state);
    }
    setPassword = (value) => {
        this.setState({
            ...this.state,
            password: value
        })
        // console.log(this.state);
    }

    render() {
        return (
            <Form onSubmit={this.handleSubmit} className="login-form">
                <Form.Item>
                    <Input
                        prefix={<Icon type="user" style={{ color: 'rgba(0,0,0,.25)' }} />}
                        placeholder="Username"
                        value={this.state.username} 
                        onInput={e => this.setUsername(e.target.value)}
                    />
                </Form.Item>
                <Form.Item>
                    <Input
                        prefix={<Icon type="lock" style={{ color: 'rgba(0,0,0,.25)' }} />}
                        type="password"
                        placeholder="Password"
                        value={this.state.password} 
                        onInput={e => this.setPassword(e.target.value)}
                    />
                </Form.Item>
                <Form.Item>
                    <Button type="primary" htmlType="submit" className="login-form-button">
                        Log in
              </Button>
                </Form.Item>
            </Form>
        )
    }
}

const mapStateToProps = (state) => ({

})

const mapDispatchToProps = dispatch => ({
    login: (username, password) => dispatch(Actions.userLogin(username, password))
})

export default connect(mapStateToProps, mapDispatchToProps)(LoginPage)
```

## 3. ใช้คำสั่ง Push ของ connected-react-router เพื่อเปิดไปหน้า dashboard 

เปิดไฟล์ `src/redux/actions.js`

เราจะใช้ `push` function ของ `connected-react-router` ในการ redirect ไปยัง URL path ที่ต้องการ

```js
import { push } from 'connected-react-router'

//...

const userLogin = (username, password) => {
    return async dispatch => {
        try {
            if(username === 'a' && password === '1234'){
                // dispatch({ type: ActionTypes.USER_LOGIN_SUCCESS, payload: 'ok' })
                dispatch(push('/dashboard'))
            } else {
                //...
            }
            
        } catch (error) {
            //...
        }
    }
}
```

### ไฟล์เต็ม Action.js

```js
import { push } from 'connected-react-router'
import API from "../services/API";

const ActionTypes = {
    SHOW_BRANCH_DATA: "SHOW_BRANCH_DATA",
    REQUEST_INIT_DATA: "REQUEST_INIT_DATA",
    REQUEST_INIT_DATA_SUCCESS: "REQUEST_INIT_DATA_SUCCESS",
    REQUEST_INIT_DATA_FAILED: "REQUEST_INIT_DATA_FAILED",

    USER_LOGIN_START: "USER_LOGIN_START",
    USER_LOGIN_SUCCESS: "USER_LOGIN_SUCCESS",
    USER_LOGIN_FAILED: "USER_LOGIN_FAILED"
}


const showBranchData = (branchId) => ({
    type: ActionTypes.SHOW_BRANCH_DATA,
    payload: branchId
})

const requestInitData = () => {
    return async (dispatch) => {
        let onSuccess = (success) => {
            console.log('load data success');
            return success;
        }

        let onError = (error) => {
            console.log(`Load data error: ${error}`);
            return error;
        }
 
        try {
            const success = await API.get('https://www.nextflow.in.th/api/branch-info/branch.json');
            console.log(success.data);
            dispatch({ type: ActionTypes.REQUEST_INIT_DATA_SUCCESS, payload: success.data })
            return onSuccess(success);
        } catch (error) {
            dispatch({ type: ActionTypes.REQUEST_INIT_DATA_FAILED, payload: error })
            return onError(error)
        }
    }
}

const userLogin = (username, password) => {
    return async dispatch => {
        try {
            if(username === 'a' && password === '1234'){
                // dispatch({ type: ActionTypes.USER_LOGIN_SUCCESS, payload: 'ok' })
                dispatch(push('/dashboard'))
            } else {
                dispatch({ type: ActionTypes.USER_LOGIN_FAILED, payload: 'fail reason' })
            }
            
        } catch (error) {
            dispatch({ type: ActionTypes.USER_LOGIN_FAILED, payload: error.message })
        }
    }
}

export default {
    requestInitData,
    showBranchData,
    ActionTypes,
    userLogin
}
```