
# วิธีดึงค่าจากคัวเลือกจังหวัด, เขต, และแขวงมาใช้งาน

## 1. สร้าง property เพื่อส่งเข้าไปใน component 

เปิดไฟล์​ `src/App.js` 

สร้าง property เป็น object และส่งผ่าน attribute ของ component 

```js

locationData = {};

render() {
    return (
      <div>
        <ThaiLocationSelectForm locationData={this.locationData}/>
      </div>
    );
  }
```

จากนั้นสร้างปุ่ม เพื่อรันคำสั่งแสดงค่าของตัว property

```js

import React, { Component } from 'react'
import logo from './logo.svg';
import './App.css';
import ThaiLocationSelectForm from './components/ThaiLocationSelectForm';

class App extends Component {

  locationData = {};

  showData = () => {
    console.log(this.locationData);
  }

  render() {
    return (
      <div>
        <ThaiLocationSelectForm locationData={this.locationData}/>
        <button onClick={this.showData}>Show data location</button>
      </div>
    );
  }
}

export default App;

```

## 2. กำหนดชื่อจังหวัด, เขต, และแขวง ให้กับ props 

อย่างแรกเลย คือเราจะมี function ให้กับตัวเลือกแขวง เพื่อเก็บค่าไว้ใน props 

```js
selectedKhwang = (kwangName) => {
    console.log(`เลือกแขวง/เขต ${kwangName}`);

    let chosenKhwang = this.state.selectedDistrict.khwangs.find(khwang => {
        return khwang.name === kwangName
    })

    this.props.locationData.khwang = chosenKhwang.name;
}

render() {
    ...
    if(this.state.selectedDistrict) {
            ...
            khwangDropDown = <LocationDropDown defaultLabel="แขวง/ตำบล" locations={khwangs} selectedCallback={this.selectedKhwang}/>
    }
    ...
}   
```

และ function สำหรับตัวเลือกเขต และจังหวัดก็เช่นเดียวกัน

```js
selectedProvince = (provinceName) => {
    ...

    this.props.locationData.province = chosenProvince.name;

    ...
}

selectedDistrict = (districtName) => {
    ...

    this.props.locationData.district = chosenDistrict.name;

    ...
}
```


```js

import React, { Component } from 'react'
import LocationDropDown from './LocationDropDown';

export default class ThaiLocationSelectForm extends Component {

    state = {}

    provinces = [
        {
            name: 'กรุงเทพ', id: 1, districts: [
                {
                    name: 'บางรัก', id: 1, khwangs: [
                        { name: 'บางรัก' },
                        { name: 'สีลม' }
                    ]
                },
            ]
        },
        {
            name: 'นนทบุรี', id: 2, districts: [
                {
                    name: 'เมืองนนทบุรี', id: 1, khwangs: [
                        { name: 'บางไผ่' },
                        { name: 'ท่าทราย' },
                        { name: 'สวนใหญ่' }
                    ]
                },
                {
                    name: 'บางกรวย', id: 2, khwangs: [
                        { name: 'บางขนุน' },
                        { name: 'ศาลากลาง' }
                    ]
                }
            ]
        }
    ]

    selectedProvince = (provinceName) => {
        console.log(`เลือกจังหวัด ${provinceName}`);

        let chosenProvince = this.provinces.find(province => {
            return province.name === provinceName
        })

        this.props.locationData.province = chosenProvince.name;

        this.setState({
            selectedProvince: chosenProvince
        });
    }

    selectedDistrict = (districtName) => {
        console.log(`เลือกเขต/อำเภอ ${districtName}`);

        let chosenDistrict = this.state.selectedProvince.districts.find(district => {
            return district.name === districtName
        })

        this.props.locationData.district = chosenDistrict.name;

        this.setState({
            selectedDistrict: chosenDistrict
        });
    }

    selectedKhwang = (kwangName) => {
        console.log(`เลือกแขวง/เขต ${kwangName}`);

        let chosenKhwang = this.state.selectedDistrict.khwangs.find(khwang => {
            return khwang.name === kwangName
        })

        this.props.locationData.khwang = chosenKhwang.name;
    }

    render() {

        let districtDropDown;
        let khwangDropDown;

        if(this.state.selectedProvince) {
            let districts = this.state.selectedProvince.districts;
            districtDropDown = <LocationDropDown defaultLabel="เขต/อำเภอ" locations={districts} selectedCallback={this.selectedDistrict}/>
        }

        if(this.state.selectedDistrict) {
            let khwangs = this.state.selectedDistrict.khwangs;
            khwangDropDown = <LocationDropDown defaultLabel="แขวง/ตำบล" locations={khwangs} selectedCallback={this.selectedKhwang}/>
        }

        return (
            <div>
                <LocationDropDown defaultLabel="จังหวัด" locations={this.provinces} selectedCallback={this.selectedProvince} />
                {districtDropDown}
                {khwangDropDown}
            </div>
        )
    }
}

```