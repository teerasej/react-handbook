
# 9. แก้ไข Reducer เพื่อตอบรับ Action ที่ชื่อ SAVE_NEW_NOTE

## 1. อัพเดต list ด้วย payload จาก action และส่งเป็น object state คืนให้ component ผ่าน Redux Store

เราจะเปิดไฟล์ `src/redux/homepage.reducer.js`

และมีการ import actions เข้ามาใช้กับ switch case ใน reducer

```js
import Actions from '../redux/actions'

//...

export default (state = initialState, { type, payload }) => {
    switch (type) {

    case Actions.ActionTypes.SAVE_NEW_NOTE: {

        // log ดูค่าที่ส่งเข้ามาใน reducer
        console.log(type, payload);

        return { 
            ...state
        }
    }

    default:
        return state
    }
}

```

ทดสอบดูว่าตอนเรากดปุ่ม มี log แสดงขึ้นมาเป็นอะไร

## 2. ทำการสร้าง State ใหม่จากของเดิม return ออกจาก Reducer

ในกรณีนี้เราแค่ต้องการเติมข้อความใหม่ลงไปใน array ที่ชื่อ `notes` ของ state เดิม เราสามารถเขียนได้แบบนี้

```js
case Actions.ActionTypes.SAVE_NEW_NOTE: {

    return { 
        ...state,
        notes: [ ...state.notes, { title: payload }]
    }
}
```

1. การเขียนแบบนี้ คือการนำค่าทั้งหมดจากตัวแปรช่่ือ state เดิม ใส่ลงไปใน object 

```js
{ 
    ...state
}
```

2. หากต้องการโอนข้อมูลเดิมใน array เก่าใส่ array ใหม่ จะเขียนแบบนี้ได้ 

```js
{ 
    ...state,
    notes: [ ...state.notes ]
}
```

3. และเพิ่ม item เข้าไปใน array ด้วยจะเขียนแบบนี้ได้ 

```js
{ 
    ...state,
    notes: [ ...state.notes, { title: payload }]
}
```

### ไฟล์เต็ม 

```js
import Actions from '../redux/actions'

const initialState = {
    notes : [
        {
            title: 'wowwwww.'
        }
    ]
}

export default (state = initialState, { type, payload }) => {
    switch (type) {

    case Actions.ActionTypes.SAVE_NEW_NOTE: {

    

        return { 
            ...state,
            notes: [ ...state.notes, { title: payload }]
        }
    }

    default:
        return state
    }
}
```