
# เรียกใช้ Action Method ใน MapBranch

เปิดไฟล์ `src/components/MapBranch.js`

ในที่นี้เราต้องการที่จะเรียกข้อมูลจาก Web API ตอนที่ MapBranch ถูกโหลดขึ้นมา ซึ่งเราจะ

## 1. ทำการเรียก Action method ที่เตรียมไว้มาใช้ใน component 

เราจะ import `getBranchLocation` จากไฟล์ actions มาใช้

```js
import { getBranchLocation } from '../redux/actions';
```

## 2. กำหนด function ให้ props ของ component ผ่าน `mapDispatchToProps`

ใน `mapDispatchToProps` จะเป็น function ที่ Redux ส่ง function ที่ชื่อ dispatch เข้ามาให้เรา เราสามารถเชื่อม function ของเรากับ props ของ Component ในที่นี้

```js
const mapDispatchToProps = (dispatch) => {
  return {
    // ประกาศ initData เป็น function ที่จะเรียกใช้ 
    initData: () => getBranchLocation(dispatch),
  }
}
```

`componentDidMount()` เป็น function ที่จะทำงานหลังจาก Component แสดงขึ้นมาบนหน้าเว็บแล้ว จึงเหมาะมากที่เราจะเรียกใช้ function `initData()` ของเรา

```js
export class MapBranch extends Component {

  componentDidMount() {
    this.props.initData();
  }
```