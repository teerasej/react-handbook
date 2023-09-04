
# Function Component

เราสามารถประกาศ Function ที่ return ค่า เป็น JSX สำหรับการสร้าง Component ของเราได้

- ชื่อ Function ต้องขึ้นต้นด้วยตัวพิมพ์ใหญ่ (หลักการเดียวกับ Camel-case)
- ตัว function ต้อง return JSX ออกไป 1 ตัว ไม่มากไม่น้อยเกินไป

## 1. ประกาศ Function component เพื่อเอามาใช้งาน

```jsx
// src/App.js

import logo from './logo.svg';
import './App.css';

// สร้าง Function Component 
const Hello = () => {
  return (
      {/* สังเกตว่า CSS สามารถนำมาใช้งานใน component ได้ด้วย ตราบเท่าที่มีการ import เข้ามาใช้งาน */}
      <div className="danger">
          <h1>Hello World</h1>
      </div>
  )
}

function App() {

  let city = 'Bangkok';

  return (
    <div>
      {/* นำ component มาใช้งานในรูปแบบ JSX */}
      <Hello/>
    </div>
  );
}

export default App;

```

## 2. ลองประกาศ Component โดยใช้รูปแบบเก่าดู

```jsx
// src/App.js

import logo from './logo.svg';
import './App.css';

const Hello = () => {
  return (
    <div className="danger">
      <h1>Hello World</h1>
    </div>
  )
}

// สร้าง Goodbye Component โดยใช้การประกาศ function แบบ javascript ดั้งเดิม
function Goodbye() {
  return (
    <div>
      <h1>Good Bye</h1>
    </div>
  )
}

function App() {

  let city = 'Bangkok';

  return (
    <div>
      <Hello />
      {/* นำ component มาใช้งานในรูปแบบ JSX */}
      <Goodbye/>
    </div>
  );
}

export default App;

```
