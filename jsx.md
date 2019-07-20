
# Using JSX

## Add babel 

Babel จะทำให้เราสามารถใช้ JSX แบบเปิดผ่าน Browser โดยตรงได้

```html
<script src="https:/unpkg.com/@babel/standalone/babel.min.js"></script>
```

## Use JSX in react

JSX จะเป็นส่วนของ HTML ที่เขียนลงไปใน JavaScript โดยตรง

```html
<script type="text/babel">
    ReactDOM.render(
        <h1>Hello World</h1>,
        document.getElementById("root")
    );
</script>
```

## การใช้ตัวแปร กับ JSX

```html
<script type="text/babel">

    let city = 'Bangkok';

    ReactDOM.render(
        <h1>Hello {city}</h1>,
        document.getElementById("root")
    );
</script>
```

## การใช้ CSS Style กับ JSX

เราสามารถกำหนดค่า CSS ให้กับ JSX ได้ คล้ายๆ กับตอนเขียน HTML ทั่วไป

```html
<style>
    #header {
        font-size: 30px;
    }

    .red {
        color: red;
    }
</style>
```

```html
<script type="text/babel">

    let city = 'Bangkok';

    ReactDOM.render(
        <h1 id="header" className="red">Hello {city}</h1>,
        document.getElementById("root")
    );
</script>
```

สังเกตว่า `id selector` กำหนดเป็น attribute id ตามปกติ แต่ `class selector` จะใช้เป็น `className`