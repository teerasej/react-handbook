
# Authorization ด้วย JWT 

## 1. กำหนดให้ส่งข้อมูล username, password ไป Web API เพื่อ Authorize

เปิดไฟล์ `src/redux/actions.js`

เราจะทำการปรับให้ API ส่ง request แบบ Post ไปที่ Web API (ในที่นี้เป็น localhost)

```js
const userLogin = (username, password) => {
    return async dispatch => {
        try {

            const result = await API.post('http://localhost:3002/api/auth/signin', {
                username: username,
                password, password
            });

            console.log(result);

            dispatch(push('/dashboard'))


        } catch (error) {
            dispatch({
                type: ActionTypes.USER_LOGIN_FAILED, payload: {
                    message: 'Please check username & password'
                }
            })
        }
    }
}
```

## 2. จากนั้นปรับให้ผลลัพธ์เรา dispatch ต่อไปยังส่วนที่ต้องการขอข้อมูล 

```js
const result = await API.post('http://localhost:3002/api/auth/signin', {
    username: username,
    password, password
});

console.log(result);
dispatch(requestInitData(result.data.token))
```

## 3. ในส่วนของ action initRequestData ปรับให้ส่ง request แบบมี token JWT แนบไปด้วย 

```js
const requestInitData = (token) => {
    return async (dispatch) => {

        try {
            var config = {
                headers: {
                    'Content-Type': 'application/json',
                    'Authorization': 'Bearer ' + token
                }
            };

            var bodyParameters = {}

            // const success = await API.get('./branch.json');
            const success = await API.get('http://localhost:3002/api/branches', config)

            console.log(success.data);
            dispatch({ type: ActionTypes.REQUEST_INIT_DATA_SUCCESS, payload: success.data })
            dispatch(push('/dashboard'))

        } catch (error) {
            console.error(error);
            dispatch({ type: ActionTypes.REQUEST_INIT_DATA_FAILED, payload: error })
        }
    }
}
```

## 4. ยกเลิกการใช้ Dashboard component เป็นตัวส่ง Action.REQUEST_INIT_DATA

เปิดไฟล์​ `src/pages/Dashboard.js`

เราจะลบส่วนของ `componentDidMount()` ออก

```js
 componentDidMount() {
    this.props.initData();
  }
```

รวมไปถึงเคลียร์ dispatch ที่ไม่ใช้ออกจาก `mapDispatchToProps` 

```js
const mapDispatchToProps = dispatch => {
  return {
    
  }
}
```

### ไฟล์เต็ม actions.js

```js
import { push } from 'connected-react-router'
import API from "../services/API";
import Axios from 'axios';

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

const requestInitData = (token) => {
    return async (dispatch) => {

        try {
            var config = {
                headers: {
                    'Content-Type': 'application/json',
                    'Authorization': 'Bearer ' + token
                }
            };

            var bodyParameters = {}

            // const success = await API.get('./branch.json');
            const success = await API.get('http://localhost:3002/api/branches', config)

            console.log(success.data);
            dispatch({ type: ActionTypes.REQUEST_INIT_DATA_SUCCESS, payload: success.data })
            dispatch(push('/dashboard'))

        } catch (error) {
            console.error(error);
            dispatch({ type: ActionTypes.REQUEST_INIT_DATA_FAILED, payload: error })
        }
    }
}

const userLogin = (username, password) => {
    return async dispatch => {
        try {

            const result = await API.post('http://localhost:3002/api/auth/signin', {
                username: username,
                password, password
            });

            dispatch(requestInitData(result.data.token))


        } catch (error) {
            dispatch({
                type: ActionTypes.USER_LOGIN_FAILED, payload: {
                    message: 'Please check username & password'
                }
            })
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