
# 26. เขียน test unit ตอน signin failed

## 1. เขียนแยก test case สำหรับกรณีที่ error 

```js
it('worker SignIn Saga failed', () => {

        const authInfo = { username: 'a', password: 'pass' }

        const payload = {
            type: actions.ActionTypes.SIGN_IN_START,
            payload: authInfo
        }

        const mockResponse = { 
            status: 404, 
            json: () => {token:'a'}
        }

        const mockError = new Error('Url not found');

        testSaga(doSignIn, payload)
        .next()
        .call(fetch, 'http://localhost:3002/api/auth/signin', {
            method: 'POST',
            headers: {
                'Accept': 'application/json',
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(authInfo)
        })
        .throw(mockError)
        .put({ type: actions.ActionTypes.SIGN_IN_FAILED, payload: mockError})
        .next()
        .isDone();
    });
```

## ไฟล์เต็ม 

```js
import { testSaga } from 'redux-saga-test-plan';
import watcher, { doSignIn }  from "./signin.saga";
import actions from '../actions';
import { push } from 'connected-react-router';

describe('Sign In Saga', () => {
   
    it('watch SignIn Saga', () => {
        testSaga(watcher, { username: 'a', password: 'pass' })
        .next()
        .takeEvery(actions.ActionTypes.SIGN_IN_START, doSignIn)
        .next()
        .isDone();
    });

    it('worker SignIn Saga success', () => {

        const authInfo = { username: 'a', password: 'pass' }

        const payload = {
            type: actions.ActionTypes.SIGN_IN_START,
            payload: authInfo
        }

        const mockResponse = { 
            status: 200, 
            json: () => {token:'a'}
        }

        testSaga(doSignIn, payload)
        .next()
        .call(fetch, 'http://localhost:3002/api/auth/signin', {
            method: 'POST',
            headers: {
                'Accept': 'application/json',
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(authInfo)
        })
        .next(mockResponse)
        .call([mockResponse, mockResponse.json ])
        .next({token:'a'})
        .put({ type: actions.ActionTypes.SIGN_IN_SUCCESS, payload: { token: 'a' }})
        .next()
        .put(push('/home'))
        .next()
        .isDone();
    });

    it('worker SignIn Saga failed', () => {

        const authInfo = { username: 'a', password: 'pass' }

        const payload = {
            type: actions.ActionTypes.SIGN_IN_START,
            payload: authInfo
        }

        const mockResponse = { 
            status: 404, 
            json: () => {token:'a'}
        }

        const mockError = new Error('Url not found');

        testSaga(doSignIn, payload)
        .next()
        .call(fetch, 'http://localhost:3002/api/auth/signin', {
            method: 'POST',
            headers: {
                'Accept': 'application/json',
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(authInfo)
        })
        .throw(mockError)
        .put({ type: actions.ActionTypes.SIGN_IN_FAILED, payload: mockError})
        .next()
        .isDone();
    });

});
```