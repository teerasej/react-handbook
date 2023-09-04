
# Class Component 

เป็นรูปแบบการเขียนเชิง Object-Oriented Programing ซึ่งสำหรับหลายๆ คนอาจจะชอบแบบ [Function Component](/function-component.md) ก็สามารถเลือกใช้ได้ แต่ปัจจุบันก็จะเป็นแบบ Function หมดแล้ว

## โครงสร้างพื้นฐานของ Class Component

โดยส่วนประกอบพื้นฐานของ Class Component คือ
1. extends React.Component
2. มี Method ชื่อ `render()` ที่คืนค่าเป็น JSX 

```js
class App extends React.Component {
    render() {
        return (
            <div>
            </div>
        )
    }
}
```

## ตัวอย่าง

```html
<script type="text/babel">

    const Food = ({name}) => <h2>{name}</h2>;

    class App extends React.Component {
        render() {
            return (
                <div>
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



