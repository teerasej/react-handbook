

# จัดการใส่ข้อมูลสาขาที่ได้มาจาก MapBranch ลงใน State ของ reducer 

เปิดไฟล์ `redux/reducer.js``

```js
import { ActionType } from "./actions"

const initialState = {
    allBranches: []
}

export default (state = initialState, { type, payload }) => {
    switch (type) {

        case ActionType.GET_BRANCH_LOCATION_FINISH:
            console.log('Branch location:',payload);
            return { ...state, allBranches: payload }

        case ActionType.SHOW_BRANCH_CHART: {
            console.log('Selected branch:', payload);

            // เอาข้อมูลที่ได้จาก payload กำหนดให้กับค่า selectedBranch ใน State
            // ...state คือการถ่ายข้อมูลจาก state ตัวเดิม ลง object ด้านล่าง
            // object ตัวนี้จะถูกส่งให้กับ component ทุกตัวที่ต่อกับ redux
            
            return { ...state, selectedBranch: payload }
        }
            
        default:
            return state
    }
}
```