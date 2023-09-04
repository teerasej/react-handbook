ในที่นี้เราสามารถกำหนดค่าของ state เป็น object และใช้เป็น property ของ Class Component ได้

## ตัวอย่าง

```jsx

    class App extends React.Component {

        // declare state
        state = {
            count: 0
        }

        increase = () => {
            this.setState({ count: this.state.count + 1});
        }

        render() {
           
            return (
                <div>
                    <p>{this.state.count}</p>
                    <button onClick={this.increase}>
                </div>
            )
        }
    }

    ReactDOM.render(
        <App />,
        document.getElementById("root")
    );

```

## การกำหนดค่าเริ่มต้นให้กับ state

- ส่วนนี้คือการกำหนดค่า property เริ่มต้นให้กับ State
- สำหรับในตอนแรก เราอาจมองได้ว่า **state เป็น property ตัวหนึ่งของ Class** และ**เก็บข้อมูลในรูปแบบ object**

```js
state = {
    count: 0
}
```

## การใช้งานค่าที่เก็บใน State ใน Component

สังเกตว่า ส่วน JSX มีการดึงค่าของ State ได้ เหมือนเป็น property หรือตัวแปรทั่วไป 

```jsx
<p>{this.state.count}</p>
```

หรือเราจะทำการดึงค่าออกมาเก็บไว้ในตัวแปร หรือทำอย่างอื่นก็ได้

```jsx
render() {

    let count = this.state.count;

    return (
        <p>{count}</p>
    )

}
```

## การอัพเดตค่าใน State

การอัพเดตค่าที่เก็บไว้ใน State จะไม่สามารถทำได้ โดยตรง เช่นแบบด้านล่างนี้ 

```js
// ไม่ได้ผล ค่าตัวแปรมีการอัพเดต แต่ไม่เกิดจาก render ตัว component ใหม่
this.state.count += 1;
```

การอัพเดตข้อมูลใน state ทำได้ผ่านคำสั่งมาตรฐาน อย่าง

```js
// ค่าเดิมของ count ใน state คือ 1 
// { count: 1 }
this.setState({ count: 2 });
```

ในกรณีที่ตัวแปร state มีข้อมูลเก็บไว้มากกว่า 1 property การอัพเดตให้กำหนดเฉพาะค่าที่ต้องการเปลี่ยนแปลง

```js
state = {
    count: 0,
    username: 'pon'
}

// การอัพเดต สามารถกำหนดเฉพาะค่าที่ต้องการเปลี่ยนแปลงได้ 
this.setState({ count: 1 });

// ค่าใน state หลังจากอัพเดต
// state = {
//     count: 1,
//     username: 'pon'
// }

// ถ้าเป็นการกำหนด property ที่ไม่มีมาก่อน จะเป็นการเพิ่มเข้าไปใน state 
this.setState({ isSignedIn: true });

// ค่าใน state หลังจากอัพเดต
// state = {
//     count: 1,
//     username: 'pon',
//     isSignedIn: true
// }
```
