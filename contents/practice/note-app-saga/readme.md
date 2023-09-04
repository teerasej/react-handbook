
# Practice: Note App

## React Setup & Ant Design Theme

1. [สร้างโปรเจค และติดตั้ง module](1-create-project-and-add-module.md) 
2. [ใช้งาน Ant Design สร้างหน้า App สร้าง Header Menu](2-use-ant-design-create-app-and-header.md)

## React Component

3. [สร้าง HomePage Component สำหรับจัดเก็บหน้า UI ของ HomePage ](3-create-home-page-component.md)
4. [สร้างกล่องกรอกข้อความสำหรับหน้า HomePage](4-create-note-box-component.md)
5. [สร้าง List ไว้แสดง Note message สำหรับหน้า HomePage](5-create-note-list-component.md)

## React + Redux

6. [ติดตั้ง Redux เข้ากับโปรเจค](6-setup-redux.md)
7. [เชื่อม NoteList Component เข้า Redux Store](7-notelist-to-redux-store.md)
8. [เชื่อม NoteInputBox Component เข้า Redux Store](8-noteinput-to-redux-store.md)
9. [แก้ไข Reducer เพื่อตอบรับ Action ที่ชื่อ SAVE_NEW_NOTE](9-modify-reducer-to-process-action.md)

## React Router

หากโปรเจคเก่าหายไปแล้ว ให้[ดาวน์โหลดไฟล์โปรเจคจากที่นี่](https://www.dropbox.com/s/z8tdhumhlj3q6vv/react-redux-note-app-1.0.zip?dl=0) 

10. [สร้างระบบ Navigation ด้วย React Router](10-setup-react-router.md)
11. [ใช้งาน Push module ในการ redirect](11-push-module.md)
12. [ส่ง SignIn Action เข้า Redux Store](12-signin-action-to-redux-store.md)

## Redux Saga

13. [ติดตั้ง และลองใช้งาน Redux Saga](13-setup-redux-saga.md)
14. [สร้าง Saga เพิ่มเติม และรวมเข้าเป็น root saga](14-add-more-saga-and-root.md)
15. [จัดกลุ่ม Saga เพื่อให้ดูแลได้ง่ายขึ้น](15-saga-management.md)
16. [ทดสอบใช้งาน saga ผ่าน Action](16-using-action-with-saga.md)
17. [สร้าง Saga สำหรับ Sign In Operation](17-saga-sign-in.md)

## Test React 

18. [ติดตั้ง Module และ Extension เพ่ิมเติม](18-0-setup-test.md)
19. [เตรียมเขียน Test UI ตัวแรก](18-1-prepare-first-test.md)
20. [ทดสอบว่ามี Spin Component อยู่ใน App Component ไหม](18-2-test-spin-exist.md)
21. [test ค่า App.props.loading และ implement](18-3-default-props.md)
22. [test initial state ของแอพต้อง return false](18-4-initial-app-state-return-false.md)

## Test Saga

23. [ติดตั้ง redux-saga-test-plan](23-install-saga-test-plan.md)
24. [สร้าง Unit Test ของ Saga](24-test-signin-saga.md)
25. [test การทำงานของ doSignIn Saga](25-test-saga-signin-success.md)
26. [เขียน test unit ตอน signin failed](26-test-signin-saga-failed.md)
27. [เขียนโค้ด saga ให้ผ่าน](27-test-signin-saga-implement.md)
28. [ทดลองเขียน Integration Test](28-test-signin-saga-integrate-1.md)