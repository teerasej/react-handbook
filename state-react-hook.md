
# State with React Hook

State เป็นการกำหนดสถานะต่างๆ ของ Component ของเรา เราสามารถพิจารณาการกำหนดค่าของ State ได้จากสถานะการทำงานของแอพเราเช่น

- ข้อความบนปุ่ม log in จะเปลี่ยนเป็น log out หลังจากลงชื่อเข้าใช้แล้ว
- จำนวนนับของสินค้าที่เรากดหยิบใส่ตะกร้า

ในที่นี้เราสามารถกำหนดค่าของ state เป็น object และใช้เป็น property ของ Class Component ได้

## ตัวอย่าง

```html
<script type="text/babel">

    const Food = ({name}) => <h2>{name}</h2>;

    function App() {

        const [loggedIn, setLoggedIn] = useState(false);

        
            // use state for final component
            let userLogInStatus;

            if(loggedIn === true){
                userLogInStatus = <h1>User is logged in</h1>;
            } else {
                userLogInStatus = <p>Please log in before use</p>;
            }


            return (
                <div>
                    {userLogInStatus}
                    <Food name="Tom Yam Goong" />
                    <Food name="Khao Pad Sapparod" />
                </div>
            )
        
    }

    ReactDOM.render(
        <App />,
        document.getElementById("root")
    );

</script>
```

ส่วนนี้คือการกำหนดค่าให้กับตัวแประ State โดยใช้ react hook function ชื่อ `useState`

```js
const [loggedIn, setLoggedIn] = useState(false);
```

สังเกตว่า ส่วน JSX มีการดึงค่าของ State มาใช้พิจารณาแสดงข้อความ โดยอิงจากค่า boolean ของ `loggedIn`

```html
<div>User is {this.state.loggedIn ? "logged in" : "not logged in"}</div>
```

useState สามารถใช้สร้าง **ตัวแปร** และ **function** ที่ใช้อัพเดตตัวแปรนั้น การกำหนดค่าให้ตัวแปรผ่าน function ดังกล่าว เทียบเท่าการใช้คำสั่ง `setState()`

รูปแบบการใช้งาน react hook **useState** จะเป็นดังนี้ 

```
const [ชื่อตัวแปร, function ที่ใช้อัพเดตตัวแปร] = useState(ค่าเริ่มต้นของตัวแปร);
```

## การอัพเดตค่าตัวแปร State

ดังนั้นหากต้องการ อัพเดตค่าให้ตัวแปร state เราจะทำผ่าน function ที่สร้างขึ้นมาโดยเฉพาะ เช่น

```js
const [loggedIn, setLoggedIn] = useState(false);

setLoggedIn(true);
```

การเรียกใช้ function นี้จะทำให้ function component render ตัวเองใหม่อีกครั้ง