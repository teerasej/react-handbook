
# สร้างโปรเจค และติดตั้ง module ที่จำเป็น

สามารถที่จะสร้างโปรเจคใหม่ตามคำสั่งด้านล่าง หรือว่าจะ[ใช้ git repo จากที่นี่](https://github.com/teerasej/nextflow-react-js-vite-counter/tree/starter)มาได้เหมือนกัน (ให้แน่ใจว่าเลือก branch 'starter' นะ) 

หรือจะใช้ git command ด้านล่างในการ clone โปรเจคก็ได้
```bash
git clone -b start https://github.com/teerasej/nextflow-react-js-vite-counter
```

## 1. สร้างโปรเจค React

```bash
npm create vite@latest counter-redux-app -- --template react
```

## 2. รันคำสั่งติดตั้ง package ที่จำเป็น

### npm

```bash
cd counter-redux-app
npm install redux react-redux
```

