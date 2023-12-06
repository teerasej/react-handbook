
# Props

เราสามารถส่งค่า เข้ามาใน Function Component ในรูปแบบของ object ตัวหนึ่งที่ชื่อว่า `props` 

- การส่งค่าเข้ามาใน `props` ของ component จะแลดูเหมือน HTML attribute ตามปกติ
- ค่า HTML attribute จะเป็นชื่อของ property ใน `props` เช่น `<Nextflow logo="brand.jpg">` ก็จะเป็น `props.logo` 

ทดลองกำหนด และใช้งาน props กับ Hello Component ตามด้านล่าง

```jsx
// src/App.ts

import logo from './logo.svg';
import './App.css';

// กำหนด interface ของ props ที่จะใช้ใน component 
interface HelloProps {
  username: string;
}

// ประกาศ parameter ชื่อ props เพื่อดึงค่ามาใช้งานภายใน component
function Hello(props: HelloProps) {
  
  const username = props.username;

  // หรือจะเขียนแบบ de-construct ก็ได้
  // const { username } = props;

  return (
    <div className="danger">
      <h1>Hello {username}</h1>
    </div>
  );
}

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
      {/* กำหนดค่า props ให้กับ Hello Component เพื่อนำไปใช้้ภายใน component */}
      <Hello username={city}/>
      <Goodbye/>
    </div>
  );
}

export default App;

```




## เสริมให้ De-construct props

เราสามารถประกาศค่าที่ส่งเข้ามาโดยตรง และไม่ผ่าน Props ได้ด้วย 

ผลจากการทำวิธีนี้ เราสามารถควบคุมค่าที่ส่งเข้ามาใน Component ได้โดยตรง ไม่ dynamic เมื่อการเรียกใช้ค่าผ่าน `props`

```jsx
// src/App.ts

import logo from './logo.svg';
import './App.css';

interface HelloProps {
  username: string;
}

// ใช้เทคนิค de-construct กับ parameter object โดยเลือกแค่ชื่อ Property 'username' มาใช้งาน
const Hello = ({ username }:HelloProps) => {

  return (
    <div className="danger">
      <h1>Hello {username}</h1>
    </div>
  )
}

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
      <Hello username={city}/>
      <Goodbye/>
    </div>
  );
}

export default App;

```


