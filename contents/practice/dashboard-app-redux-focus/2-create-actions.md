

# สร้าง Actions 


เราจะเริ่มจากการกำหนด action ที่จะเกิดขึ้นในแอพของเรา ได้แก่

1. โหลดข้อมูลสาขาเก็บไว้ในแอพ เมื่อเปิดแอพพลิเคชั่นขึ้นมา
2. แสดงยอดขายต่อเดือนในกราฟ เวลาคลิกเลือกสาขาในแผนที่ 

สร้างไฟล์ `redux/actions.js`

```js
export const ActionType = {
    // action ที่ระบุว่าได้ข้อมูลแล้ว
    GET_BRANCH_LOCATION_FINISH: 'GET_BRANCH_LOCATION_FINISH',
    // action ที่ระบุว่ามีการคลิกเลือกสาขาจากแผนที่ และจะแสดงข้อมูลในกราฟ
    SHOW_BRANCH_CHART: 'SHOW_BRANCH_CHART'
};
```