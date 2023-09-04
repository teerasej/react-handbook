
# Props

เราสามารถส่งค่า เข้ามาใน Function Component ในรูปแบบของ object ตัวหนึ่งที่ชื่อว่า `props` 

```jsx
// src/App.js

import logo from './logo.svg';
import './App.css';

// ประกาศ parameter ชื่อ props เพื่อดึงค่ามาใช้งานภายใน component
const Hello = (props) => {

  // property ที่ส่งเข้ามา สามารถเข้าถึงได้ผ่านการเรียกใช้ parameter
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

- การส่งค่าเข้ามาใน `props` ของ component จะแลดูเหมือน HTML attribute ตามปกติ
- ค่า HTML attribute จะเป็นชื่อของ property ใน `props` เช่น `<Nextflow logo="brand.jpg">` ก็จะเป็น `props.logo` 

## การใช้งาน Props ใน Class component

ค่าของ props ที่ส่งเข้ามาใน Class component สามารถเข้าถึงได้จาก properties ของ class ในชื่อเดียวกัน

```js
this.props.<props name>
```

### ตัวอย่าง

```jsx
class App extends React.Component {
        render() {
            return (
                <div>
                    <Food name={this.props.foodName}/>
                </div>
            )
        }
    }

    ReactDOM.render(
        <App foodName="ต้มยำกุ้ง"/>,
        document.getElementById("root")
    );
```

## De-construct props

เราสามารถประกาศค่าที่ส่งเข้ามาโดยตรง และไม่ผ่าน Props ได้ด้วย 

ผลจากการทำวิธีนี้ เราสามารถควบคุมค่าที่ส่งเข้ามาใน Component ได้โดยตรง ไม่ dynamic เมื่อการเรียกใช้ค่าผ่าน `props`

```html
<script type="text/babel">

    const Hello = ({library, message}) => {
        return (
            <div className="red">
                <h1>Hello {library}</h1>
                <p>{message}</p>
            </div>
        )
    }

    ReactDOM.render(
        <Hello library="Angular" message="Yep!"/>,
        document.getElementById("root")
    );
    
</script>
```


