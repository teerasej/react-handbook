

# สร้างโปรเจค และติดตั้ง module

## 1. สร้างโปรเจค

```bash
npx create-react-app member-app
```

## 2. ติดตั้ง module เบื้องต้น

รายชื่อ module ที่ติดตั้งในขั้นตอนนี้
1. react
2. react-dom
3. @material-ui/core
4. formik
5. redux
6. react-redux
7.  redux-logger
8.  yup
9.  react-router-dom 
10. axios
11. use-async-effect

### ใช้ npm

```bash
cd member-app
npm i --save react react-dom @material-ui/core formik redux react-redux redux-logger yup react-router-dom axios use-async-effect
```

### ใช้ yarn

```bash
cd member-app
yarn add react react-dom @material-ui/core formik redux react-redux redux-logger yup react-router-dom axios use-async-effect
```

3. ติดตั้ง CORS Extension 

เนื่องจากแอพที่เราทำรันอยู่บน Localhost การติดต่อกับ Web API ที่อยู่บน Server อื่นอาจจะเกิดปัญหา CORS บน Web Browser ได้ 

พวกเราที่ใช้ Chrome หรือ Edge สามารถดาวน์โหลดติดตั้ง ส่วนเสริมที่ชื่อ [allow-cors-access-control](https://chrome.google.com/webstore/detail/allow-cors-access-control/lhobafahddgcelffkeicbaginigeejlf)