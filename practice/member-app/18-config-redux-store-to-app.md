
# ตั้งค่า Store ให้เชื่อมกับ Component ในระบบแอพ

1. import function สร้าง store object มาในไฟล์ `App.js `
2. นำ `Provider` component มาครอบด้านนอกสุดของ App 
3. กำหนด store object ให้กับ Provider เพื่อใช้งาน
4. Provider จะทำให้ทุก Component ภายใต้มัน เชื่อมต่อกับ redux store ได้ 

```jsx
// src/App.js

import logo from './logo.svg';
import './App.css';
import Main from './components/Main';
import {
  BrowserRouter as Router,
  Switch,
  Route
} from "react-router-dom";
import LoginPage from './pages/LoginPage';
import SignupPage from './pages/SignupPage';

// import Provider component และ function สร้าง store object ที่เราทำไว้
import { Provider } from 'react-redux';
import configureStore from './redux/store';

// เรียกใช้ function เพื่อสร้าง store object
const store = configureStore();

function App() {
  return (
    {/* Provider component ต้องการ store object ที่ได้จากการเรียกใช้ createStore()  */}
    <Provider store={store}>
    <div>
      <Router>
        <Switch>
          <Route path="/" exact>
            <Main/>
          </Route>
          <Route path="/login" exact>
            <LoginPage/>
          </Route>
          <Route path="/signup" exact>
            <SignupPage/>
          </Route>
        </Switch>
      </Router>
    </div>

    {/* tag ปิด Provider component */}
    </Provider>
  );
}

export default App;

```