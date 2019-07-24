
# State 

State เป็นการกำหนดสถานะต่างๆ ของ Component ของเรา เราสามารถพิจารณาการกำหนดค่าของ State ได้จากสถานะการทำงานของแอพเราเช่น

- ข้อความบนปุ่ม log in จะเปลี่ยนเป็น log out หลังจากลงชื่อเข้าใช้แล้ว
- จำนวนนับของสินค้าที่เรากดหยิบใส่ตะกร้า

ในที่นี้เราสามารถกำหนดค่าของ state เป็น object และใช้เป็น property ของ Class Component ได้

## ตัวอย่าง

```html
<script type="text/babel">

    const Food = ({name}) => <h2>{name}</h2>;

    class App extends React.Component {

        state = {
            loggedIn: true
        }

        render() {
            return (
                <div>
                    <div>User is {this.state.loggedIn ? "logged in" : "not logged in"}</div>
                    <Food name="Tom Yam Goong"/>
                    <Food name ="Khao Pad Sapparod"/>
                </div>
            )
        }
    }

    ReactDOM.render(
        <App />,
        document.getElementById("root")
    );

</script>
```

ส่วนนี้คือการกำหนดค่าให้กับ State

```js
state = {
    loggedIn: true
}
```

สังเกตว่า ส่วน JSX มีการดึงค่าของ State มาใช้พิจารณาแสดงข้อความ โดยอิงจากค่า boolean ของ `state.loggedIn`

```html
<div>User is {this.state.loggedIn ? "logged in" : "not logged in"}</div>
```