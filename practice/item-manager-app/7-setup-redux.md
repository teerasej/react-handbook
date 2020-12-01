
# Setup Redux 

## 1. สร้าง Action 

สร้างไฟล์ `src/redux/action.js`

```js
// src/redux/action.js

export default {
    CREATE_NEW_ITEM: 'CREATE_NEW_ITEM'
}

```

## 2. สร้าง Reducer 

สร้างไฟล์ `src/redux/reducer.js`

```js
// src/redux/reducer.js

// import action เพื่อกำหนด type ให้ reducer
import Action from './action' 

const initialState = {

}

export default (state = initialState, { type, payload }) => {
    switch (type) {

    // เทียบชื่อ action
    case Action.CREATE_NEW_ITEM:
        return { ...state, ...payload }

    default:
        return state
    }
}
```

## 3. สร้าง Store 

สร้างไฟล์ `src/redux/store.js`

```js
// src/redux/store.js

import { createStore } from 'redux'  
import reducer from './reducer'

export default function createReduxStore() {
    return createStore(reducer);
}
```

## 4. ครอบ Component ด้วย Redux store ผ่าน Provider

```js
// src/index.js

import createReduxStore from './redux/store'
import { Provider } from 'react-redux'  

const store = createReduxStore();

ReactDOM.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>,
  document.getElementById('root')
);
```