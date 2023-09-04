
# สร้าง Action method สำหรับเรียกข้อมูลจาก Web API 

หลังจากเราต่อ MapBranch เข้ากับ Redux แล้ว เราจะมาทำกลไกแรกกัน นั่นก็คือ

**เปิด MapBranch แล้ว เราจะเรียกข้อมูลสาขามาจาก Web API**

## 1. สร้าง Action method 

กลับมาที่ไฟล์ `redux/actions.js` เราจะประกาศ function ไว้ใช้งาน

```js
// ประกาศ function ชื่อ getBranchLocation 
// และกำหนดให้รับ parameter ชื่อว่า dispatch มาใช้งาน
export const getBranchLocation = async (dispatch) => {

    console.log('getting data');

    // กำหนด URL ที่มีข้อมูลอยู่บน server
    // ข้อมูลที่ได้เป็น json
    let api = 'https://www.nextflow.in.th/api/branch-info/branch.json';

    // ใช้ fetch() function เป็นตัวส่ง request ไปเรียกข้อมูล
    let response = await fetch(api);
    // แปลงข้อมูลเป็นตัวแปร json
    let json = await response.json();

    // ลองแสดงข้อมูลใน console เพื่อดูว่าได้อะไรมาจาก web api
    console.log(json);

    // ใช้ dispatch function ส่ง action เข้าไปที่ reducer
    // โดยกำหนด type เป็น ActionType.GET_BRANCH_LOCATION_FINISH
    // และกำหนด payload เป็น json.branches
    dispatch({ 
        type: ActionType.GET_BRANCH_LOCATION_FINISH, 
        payload: json.branches
    });
}
```


## ไฟล์ `redux/actions.js` สมบูรณ์

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


```