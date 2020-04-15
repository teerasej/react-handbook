
# Props

เราสามารถส่งค่า เข้ามาใน Function Component ในรูปแบบของ object ตัวหนึ่งที่ชื่อว่า `props` 

```html
<script type="text/babel">

    const Hello = (props) => {
        return (
            <div className="red">
                <h1>Hello {props.library}</h1>
                <p>{props.message}</p>
            </div>
        )
    }

    ReactDOM.render(
        <Hello library="Angular" message="Yep!"/>,
        document.getElementById("root")
    );
    
</script>
```

- การส่งค่าเข้ามาใน `props` ของ component จะแลดูเหมือน HTML attribute ตามปกติ
- ค่า HTML attribute จะเป็นชื่อของ property ใน `props` เช่น `<Nextflow logo="brand.jpg">` ก็จะเป็น `props.logo` 

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
