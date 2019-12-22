
# Using React 

เราสามารถเอา React มาใช้งานในเว็บเพจเราได้ทันที ผ่านการใช้งาน `<script>`

```html
 <script src="https:/unpkg.com/react@16.7.0/umd/react.development.js"></script>
 <script src="https:/unpkg.com/react-dom@16.7.0/umd/react-dom.development.js"></script>
```

## Render a component 

เราสามารถใช้คำสั่ง `ReactDOM.render()` ในการสั่งให้ render Component ตามที่เราต้องการ

ตัวอย่างด้านล่าง เราใช้ `React.createElement( elementName, style, content)` ในการสร้าง Component

```html
<body>
    <div id="root"></div>
    <script type="text/javascript">
        ReactDOM.render(
            React.createElement("h1", {"style": {"color": "red"}}, "Hello World"),
            document.getElementById("root")
        );
    </script>
</body>
```

บันทึกไฟล์ และเปิดดูใน Web Browser จะเห็นว่าแสดงข้อความขึ้นมาได้ 

แต่แบบที่ใช้กันจริงๆ [จะใช้ JSX มากกว่า](/jsx.md)
