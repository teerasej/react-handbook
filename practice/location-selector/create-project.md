
# สร้างโปรเจคและติดตั้ง Bootstrap 4

## 1. สร้างโปรเจค create-react-app

รันคำสั่งด้านล่างเพื่อสร้างโปรเจค

```bash
npx create-react-app locator-web
```

## 2. ทดสอบรันโปรเจคทำงาน 

ใช้คำสั่ง npm start ใน directory โปรเจค เพื่อรัน web server

```bash
cd locator-web
npm start
```

### **หากมีปัญหา error babel 
ให้หยุดการทำงานของ web server ก่อนและรันคำสั่งใน Terminal 

```bash
npm add @babel/runtime
```

## 3. ติดตั้ง Bootstrap

ปิด server โดยใช้ปุ่ม Ctrl+C

จากนั้นรันคำสั่ง 

```bash
npm install --save bootstrap
```

เปิดไฟล์ `src/index.js` และเพิ่มโค้ดด้านบนสุดเพื่อนำ bootstrap มาใช้งาน 

```js
import 'bootstrap/dist/css/bootstrap.css';
```

เปิดไฟล์ `public/index.html` และใส่ tag `<script>` ของ bootstrap และ jquery ลงไปในส่วนก่อน tag `</body>`

```html
    <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js" integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1" crossorigin="anonymous"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>

  </body>
</html>

```

เสร็จแล้วรัน web server ด้วย `npm start` อีกครั้ง