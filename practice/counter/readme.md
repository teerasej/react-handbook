
# โจทย์ Counter App

## ทดสอบ

- React
- Redux

## การเตรียมโจทย์

### 1. สร้างโปรเจคใหม่

```
npx create-react-app counter-app
```

### 2. ติดตั้ง package ที่จำเป็น 

```bash
cd counter-app
npm i react react-dom redux react-redux antd css-animation
```

หรือถ้าใช้ yarn 

```bash
cd counter-app
yarn add react react-dom redux react-redux antd css-animation
```

### 3. สร้างโปรเจคตาม Scope ต่อไปนี้

- ในแอพจะมี 2 Component แยกกัน คือ
    1. ตัวแสดงจำนวนนับ (Counter Component)
    2. ปุ่ม "เพิ่มจำนวน" (AddNumber Component)
- หากกดปุ่มเพิ่มจำนวนนับ ตัวเลขที่แสดงต้องบวกจำนวนเพิ่มอีก 1 

![Paper React_React_Native 78](https://user-images.githubusercontent.com/85179/71774258-2e849300-2f9e-11ea-9010-2d1b06151cfb.png)



## โจทย์ท้าทายเพิ่มเติม

หากทำตามข้อ 3 ได้แล้ว สามารถทดลองเพิ่มฟีเจอร์ด้านล่าง

- เพิ่มอีก 1 Component เป็นปุ่ม "ลดจำนวน" กดหนึ่งครั้งจำนวนที่แสดงจะลดลง 1 หน่วย และไม่ลดต่ำกว่า 0 
- ถ้าจำนวนนับที่แสดง หาร 2 ลงตัวให้เป็นข้อความสีฟ้า ถ้าหาร 2 ไม่ลงตัวให้แสดงเป็นข้อความสีเขียว
- ถ้าจำนวนนับมีค่าเท่ากับ 0 ให้ซ่อนปุ่ม "ลดจำนวน"
- ถ้าจำนวนนับมีค่าเท่ากับ 10 ให้ซ่อนปุ่ม "เพิ่มจำนวน"