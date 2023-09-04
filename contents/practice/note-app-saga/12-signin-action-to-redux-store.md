
# 12. ส่ง SignIn Action เข้า Redux Store 



## 1. สร้าง SignIn Action

เปิดไฟล์ `src/redux/actions.js` 

เราจะทำการสร้าง type, action function และ export ไปใช้งาน

```js
const ActionTypes = {
    SAVE_NEW_NOTE: "SAVE_NEW_NOTE",
    START_SIGN_IN: "START_SIGN_IN"
}


const saveNewNote = (message) => ({
    type: ActionTypes.SAVE_NEW_NOTE,
    payload: message
})

const startSignIn = (authInfo) => ({
    type: ActionTypes.START_SIGN_IN,
    payload: authInfo
})

export default {
    saveNewNote,
    ActionTypes,
    startSignIn
}
```

## 2. นำ Action SignIn มาใช้งานใน LoginPage Component

เปิดไฟล์​ `src/pages/login-page/LoginPage.js`

และแก้ไขส่วนของ `mapDispatchToProps` ให้เรียกใช้ action function 

```js
const mapDispatchToProps = dispatch => {
    return {
        startSignIn: (authInfo) => dispatch(actions.startSignIn(authInfo))
    }
}
```

และในส่วนของ function ที่ทำงานตอนกดปุ่ม Sign In ก็จะเรียกใช้ `this.props.startSignIn` ตามที่สร้างเอาไว้

```js
handleSubmit = e => {
    e.preventDefault();

    this.props.form.validateFields((err, values) => {
        if (!err) {
            const username = this.props.form.getFieldValue('username');
            const password = this.props.form.getFieldValue('password');
            console.log('Username:', username, 'Password:', password);

            this.props.startSignIn({ username: username, password: password });
        }
    });
}
```

