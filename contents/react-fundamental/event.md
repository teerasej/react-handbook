
# การจัดการ Event ของ Component 

เราสามารถใช้ function ผูกเข้ากับ **Event props** ของ JSX ได้โดยตรง


```jsx
// src/App.jsx

import logo from './logo.svg';
import './App.css';

const Hello = (props) => {

  let username = props.username

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

  // สร้าง function เพื่อนำไปใช้งานกับ event ของ button
  const popUpAlert = () => {
    alert('Sign in!')
  }

  return (
    <div>
      <Hello username={city}/>
      <Goodbye/>
      {/* กำหนดให้ button เรียกใช้งาน popUpAlert() function เพื่อเกิด event ชื่อ onClick */}
      <button onClick={popUpAlert}>Sign in</button>
    </div>
  );
}

export default App;

```
