# 28. ทดลองเขียน Integration Test

## 1. สร้าง test case แบบ integrate app reducer

สร้างไฟล์ `src/redux/sagas/signin.saga.reducer.test.js`

```js
import { expectSaga } from 'redux-saga-test-plan';
import appReducer from "../app.reducer";

import watcher, { doSignIn }  from "./signin.saga";
import actions from '../actions';
import { push } from 'connected-react-router';

describe('Sign In Saga Integration Test', () => {

    it('worker SignIn Saga success', () => {

        const authInfo = { username: 'a', password: 'pass' }

        const payload = {
            type: actions.ActionTypes.SIGN_IN_START,
            payload: authInfo
        }

        return expectSaga(doSignIn, payload)
        .withReducer(appReducer)
        .hasFinalState({
            loading: false
        })
        .run()  
    });


}); 
```
และทดสอบรัน test

## 2. สร้าง test case แบบ integrate homepage.reducer

```js
it('worker SignIn Saga success effect home token', () => {

        const authInfo = { username: 'a', password: 'pass' }

        const payload = {
            type: actions.ActionTypes.SIGN_IN_START,
            payload: authInfo
        }

        return expectSaga(doSignIn, payload)
        .withReducer(homepageReducer)
        .run() 
        .then((result) => {
            expect(result.storeState.token).toBeTruthy()
        });
    });
```

และทดสอบรัน test