
# สร้าง Action Method สำหรับส่งข้อมูลของสาขาที่ถูกเลือก เข้า reducer


หลังจากเราทำ action แรกเสร็จแล้ว เราก็จะใช้รูปแบบเดียวกับ Action แรกในการสร้าง

1. Action Method 
2. ใช้งาน Action method ใน MapBranch 
3. ส่ง action และข้อมูลสาขาที่ถูกเลือก เข้า reducer 
4. reducer เอาข้อมูลสาขาที่ถูกเลือก อัพเดตใน state object
5. reducer ส่ง state object ใน component ทุกตัว
6. เราจะเชื่อมต่อ StatChart เข้ากับ redux เพื่อเอา state object มาใช้


## สร้าง Action Method ที่จะรับข้อมูลสาขา และ dispatch function

เราจะสร้าง function ชื่อ `showBranchChart` เพื่อส่งต่อข้อมูลเข้า reducer

```js
export const showBranchChart = (branchData, dispatch) => {
    console.log('Branch Data', branchData);

    dispatch({
        type: ActionType.SHOW_BRANCH_CHART,
        payload: branchData
    });
}
```


## ไฟล์เต็ม redux/actions.js
```js


export const ActionType = {
    GET_BRANCH_LOCATION_FINISH: 'GET_BRANCH_LOCATION_FINISH',
    SHOW_BRANCH_CHART: 'SHOW_BRANCH_CHART'
};

export const getBranchLocation = async (dispatch) => {

    console.log('getting data');

    let api = 'https://www.nextflow.in.th/api/branch-info/branch.json';

    let response = await fetch(api);
    let json = await response.json();

    console.log(json);

    dispatch({ 
        type: ActionType.GET_BRANCH_LOCATION_FINISH, 
        payload: json.branches
    });
}

export const showBranchChart = (branchData, dispatch) => {
    console.log('Branch Data', branchData);

    dispatch({
        type: ActionType.SHOW_BRANCH_CHART,
        payload: branchData
    });
}

```