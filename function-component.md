
# Function Component

เราสามารถกำหนด Function ที่ return ค่า เป็น JSX สำหรับการสร้าง Tag Component ของเราได้

```html
<script type="text/babel">

    const Hello = () => {
        return (
            <div className="blue">
                <h1>Hello World</h1>
            </div>
        )
    }

    ReactDOM.render(
        <Hello/>,
        document.getElementById("root")
    );

</script>
```