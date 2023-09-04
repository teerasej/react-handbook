
# สร้าง reducer

reducer จะทำหน้าที่

1. เก็บข้อมูลทั้งหมดไว้ในตัวแปร object ตัวหนึ่ง เรียกว่า state (บางคนเรียนก global state เพราะทุก component จะได้ object ตัวนี้ไปใช้งาน)
2. ส่งตัวแปร state ให้กับทุก component เวลามี event ส่งเข้ามา หรือตอนเริ่ม application

สร้างไฟล์ `redux/reducer.js`

ใช้ snippet ชื่อ `rxreducer` ได้

```js
// import action type มาจากไฟล์ actions.js
import { ActionType } from "./actions"

// object เก็บข้อมูลที่จะแชร์ให้ทุก component
const initialState = {
    
    // ประกาศ properties ไว้ใน state
    allBranches: []
}

// function ที่จะทำหน้าที่รับ action ที่ส่งเข้ามาใน redux 
// และทำการอัพเดตข้อมูลใน state ก่อนที่จะส่งออกไปให้ component ทุกตัว
export default (state = initialState, { type, payload }) => {

    // รับ type ของ action มาเทียบว่าต้องจัดการข้อมูลที่อยู่ใน payload ยังไง
    switch (type) {

        // เทียบกรณีที่ได้รับ action type GET_BRANCH_LOCATION_FINISH
        case ActionType.GET_BRANCH_LOCATION_FINISH: 
            return { ...state, ...payload };
        
        // เทียบกรณีที่ได้รับ action type SHOW_BRANCH_CHART
        case ActionType.SHOW_BRANCH_CHART: 
            return { ...state, ...payload };

        // ถ้าเป็นการเริ่มต้นแอพ จะส่งค่าที่อยู่ใน initialState ออกไปให้ component แทน
        default:
            return state
    }
}

```