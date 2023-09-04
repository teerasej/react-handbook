
# สร้าง Navigation ด้วย Router 

- สร้างระบบ Navigation ด้วยเครื่องมือที่ [`react-router-dom`](https://reactrouter.com/web/guides/quick-start) เตรียมไว้ให้ 
- กำหนด [Route](https://reactrouter.com/web/api/Route) '/' ให้แสดง Main component 

```jsx
// src/App.js

import logo from './logo.svg';
import './App.css';
import Main from './components/Main';

// Import component ที่จำเป็นต่อการสร้าง Navigation
import {
  BrowserRouter as Router,
  Switch,
  Route
} from "react-router-dom";


function App() {
  return (
    <div>
      {/* Router: ตัวควบคุมพื้นที่ Navigation ทั้งหมด ปกติจะครอบในส่วนที่ต้องการใช้งาน Navigation */}
      <Router>
        {/* Switch: บริเวณที่ต้องการแสดง Component ขึ้นมาตาม Route path ต่างๆ */}
        <Switch>
          {/* Route: Component ที่ต้องการให้ตอบสนองต่อระบบ Router ส่วนใหญ่ใช้กำหนด path อ้างอิง และกำหนด Component ที่ต้องการแสดงขึ้นมา เวลามีการเรียกใช้ path นั้นๆ   */}
          {/* exact props เป็นการบอกให้ Router เทียบ path แบบตรงตัว   */}
          <Route path="/" exact>
            <Main/>
          </Route>
        </Switch>
      </Router>
    </div>
  );
}

export default App;

```