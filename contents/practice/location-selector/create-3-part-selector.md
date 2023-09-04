
# สร้างตัวเลือกจังหวัด, เขต, และแขวง

## 1. ทดลองแสดงตัวเลือก

เปิดไฟล์ `src/App.js` และปรับส่วนของข้อมูลให้มีเขตและแขวง

```js
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
```
และทดสอบแสดงตัวเลือกเขต และแขวงโดยใช้ `<LocationDropDown />`

```js

import React, { Component } from 'react'
import LocationDropDown from './LocationDropDown';

export default class ThaiLocationSelectForm extends Component {


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

    render() {
        return (
            <div>
                <LocationDropDown defaultLabel="จังหวัด" locations={this.provinces} />
                <LocationDropDown defaultLabel="เขต/อำเภอ" locations={this.provinces} />
                <LocationDropDown defaultLabel="แขวง/ตำบล" locations={this.provinces} />
            </div>
        )
    }
}

```

## 2. สร้าง ThaiLocationSelectForm Component 

สร้างไฟล์ `src/components/ThaiLocationSelectForm.js` และย้ายโค้ดจาก `src/App.js` มาไว้ที่ไฟล์นี้ 

และเรามีการส่ง function ลงไปใน component เพื่อเรียกใช้ตอนที่มีการเลือกตัวเลือก

```js
selectedProvince = (province) => {
    console.log(`เลือกจังหวัด ${province}`);
}
```

```html
<LocationDropDown defaultLabel="จังหวัด" locations={this.provinces} selectedCallback={this.selectedProvince} />
```

```js

import React, { Component } from 'react'
import LocationDropDown from './LocationDropDown';

export default class ThaiLocationSelectForm extends Component {


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

    selectedProvince = (province) => {
        console.log(`เลือกจังหวัด ${province}`);
    }

    render() {
        return (
            <div>
                <LocationDropDown defaultLabel="จังหวัด" locations={this.provinces} selectedCallback={this.selectedProvince} />
                <LocationDropDown defaultLabel="เขต/อำเภอ" locations={this.provinces} />
                <LocationDropDown defaultLabel="แขวง/ตำบล" locations={this.provinces} />
            </div>
        )
    }
}

```

ไฟล์ `src/App.js` จะเหลือแค่นี้ 

```js

import React, { Component } from 'react'
import logo from './logo.svg';
import './App.css';
import ThaiLocationSelectForm from './components/ThaiLocationSelectForm';

function App() {


    return (
      <div>
        <ThaiLocationSelectForm />
      </div>
    );
}

export default App;

```

## 3. เรียกใช้ function ที่ส่งเข้ามา ผ่าน props 

เปิดไฟล์ `src/components/LocationDropDown.js` และเรียกใช้ function ใน method `locationSelected(e)`

```js
locationSelected(e) {
    console.log(e.target);

    let locationName = e.target.getAttribute('data-name');
    console.log(locationName);

    this.setState({
        label: locationName
    })

    if(this.props.selectedCallback) {
    this.props.selectedCallback(locationName);
    }
}
```

## 4. เขียน function ที่รับค่าจังหวัดจากการเลือก และใช้ในการแสดงตัวเลือกเขต 

เปิดไฟล์ `src/components/ThaiLocationSelectForm.js` 

ตอนแรกสร้าง state เปล่าขึ้นมาใน Component 

```js
export default class ThaiLocationSelectForm extends Component {

    state = {}

    ...
```

จากนั้นปรับปรุงให้ function ที่รับค่ามาจาก Dropdown ของเรากำหนดค่าให้กับ state 

```js
selectedProvince = (provinceName) => {
    console.log(`เลือกจังหวัด ${provinceName}`);

    let chosenProvince = this.provinces.find(province => {
        return province.name === provinceName
    })

    this.setState({
        selectedProvince: chosenProvince
    });
}
```

จากนั้นปรับปรุง method `render()` ให้พิจารณาแสดง JSX ของตัวเลือกเขต จาก state ที่มี

```js
render() {

    let districtDropDown;

    if(this.state.selectedProvince) {
        let districts = this.state.selectedProvince.districts;
        districtDropDown = <LocationDropDown defaultLabel="เขต/อำเภอ" locations={districts}/>
    }

    return (
        <div>
            <LocationDropDown defaultLabel="จังหวัด" locations={this.provinces} selectedCallback={this.selectedProvince} />
            {districtDropDown}
        </div>
    )
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

        this.setState({
            selectedProvince: chosenProvince
        });
    }

    render() {

        let districtDropDown;

        if(this.state.selectedProvince) {
            let districts = this.state.selectedProvince.districts;
            districtDropDown = <LocationDropDown defaultLabel="เขต/อำเภอ" locations={districts}/>
        }

        return (
            <div>
                <LocationDropDown defaultLabel="จังหวัด" locations={this.provinces} selectedCallback={this.selectedProvince} />
                {districtDropDown}
            </div>
        )
    }
}

```

## 5. สร้างตัวเลือกแขวงด้วยขั้นตอนเดียวกัน

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

        this.setState({
            selectedProvince: chosenProvince
        });
    }

    selectedDistrict = (districtName) => {
        console.log(`เลือกเขต/อำเภอ ${districtName}`);

        let chosenDistrict = this.state.selectedProvince.districts.find(district => {
            return district.name === districtName
        })

        this.setState({
            selectedDistrict: chosenDistrict
        });
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
            khwangDropDown = <LocationDropDown defaultLabel="แขวง/ตำบล" locations={khwangs}/>
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