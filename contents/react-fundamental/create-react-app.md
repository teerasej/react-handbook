# สร้าง Create React App

> ในที่นี้จะเป็นการสร้างโปรเจคเก็บไว้ใน directory หนึ่งในเครื่อง สามารถสร้าง folder ใหม่ขึ้นมาและตั้งชื่อว่า my_react_project

### Wanna try project in codespace?

ถ้าต้องการ สามารถใช้[โปรเจคเดียวกันนี้](https://github.com/teerasej/nextflow-react-js-app-starter)รันบน Github Codespace ได้นะ

```
https://github.com/teerasej/nextflow-react-js-app-starter
```

## 1. ใช้คำสั่ง สร้าง react project

```bash
npx create-react-app my-app
```

## 2. เปิด directory ผ่าน terminal

```bash
cd my-app
```
### Note: React 19 (Jan 27, 2025)

เนื่องจาก Create react app ที่ใช้ React 19 นั้นมีการใช้ @testing-library/react@13.4.0 ที่ยังใช้ React 18 อยู่[ทำให้เกิดปัญหา](https://github.com/facebook/react/issues/32016) หากต้องการการทำงานที่ใช้งานได้ ต้องทำการ downgrade ตัวโปรเจคไปใช้ React 18 ตามคำสั่งต่อไปนี้ 

```bash
npm uninstall react react-dom
```
```bash
npm install react@18 react-dom@18
```
```bash
npm i web-vitals
```

## 3. รัน web server ภายในโปรเจค

```bash
npm start
```

## 4. ปิดการทำงานของ Server

เลือก Command Prompt หรือ terminal แล้วกด Ctrl + C

## ใครติดปัญหาสร้างโปรเจคไม่ได้

1. ให้[ดาวน์โหลด zip file มาจากที่นี่](https://github.com/teerasej/nextflow-react-my-app-starter)
2. แตก zip file
3. เปิดโฟลเดอร์ของโปรเจคขึ้นมาใน Visual Studio Code (หรือ Terminal หรือ Command Prompt)
4. รันคำสั่ง `npm install`
