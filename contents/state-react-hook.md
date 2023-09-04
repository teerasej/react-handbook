
# State with React Hook

ในที่นี้เราเราสามารถสร้าง **ตัวแปร และ function ที่ไว้ทำงานกับ State** ของ function component ได้ ผ่าน React Hook ชื่อ `useState()`

## ตัวอย่าง

```jsx

function App() {

    const [count, setCount] = useState(0);

    const increase = () => {
        let newCount = count + 1;
        setCount(newCount);
    }
    
    return (
        <div>
            <p>{count}</p>
            <button onClick={increase}>
        </div>
    )
    
}

```

## การกำหนดค่าเริ่มต้นให้กับ state

ส่วนนี้คือการกำหนดค่าให้กับตัวแประ State โดยใช้ react hook function ชื่อ `useState`

```js
const [count, setCount] = useState(0);
```

## การใช้งานค่าที่เก็บใน State ใน Component

สังเกตว่า ส่วน JSX มีการดึงค่าของ State ได้ เหมือนเป็นตัวแปรทั่วไป 

```jsx
<p>{count}</p>
```

เพียงแต่เราไม่สามารถกำหนดค่าใหม่ลงไปในตัวแปร State ได้โดยตรง ต้องทำผ่าน function ที่สร้างขึ้นมาจาก hook ที่ชื่อว่า `useState()`

## การอัพเดตค่าตัวแปร State

ดังนั้นหากต้องการ อัพเดตค่าให้ตัวแปร state เราจะทำผ่าน function ที่สร้างขึ้นมาโดยเฉพาะ เช่น

```js
const [count, setCount] = useState(0);

setCount(9);
```

การเรียกใช้ function นี้จะทำให้ function component render ตัวเองใหม่อีกครั้ง