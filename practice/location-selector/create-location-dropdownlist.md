
# สร้าง Location DropDown List

## 1. สร้าง LocationDropDownList component

สร้างไฟล์ และสร้าง class แบบด้านล่าง (ใช้ snippet **rcc** ได้)

```js
import React, { Component } from 'react'

export default class LocationDropDown extends Component {
    render() {
        return (
            <div>
                Hello
            </div>
        )
    }
}
```

## 2. สร้าง Array สำหรับแสดงสถานที่ใน Dropdown

เปิดไฟล์ `src/App.js` และสร้าง property ขึ้นมา

```js
function App() {

  let provinces = [
    { name: 'Bangkok' },
    { name: 'Nonthaburi' }
  ]


  return (
    <div>
      
    </div>
  );
}
```

## 3. ใน Location DropDown Component มาใช้

จากไฟล์ `src/App.js` เขียนคำสั่ง `import` Component มาใช้ และกำหนด array เป็น attibute ของ component 

```js
import React from 'react';
import logo from './logo.svg';
import './App.css';
import LocationDropDown from './components/LocationDropDown';

function App() {

  let provinces = [
    { name: 'Bangkok' },
    { name: 'Nonthaburi' }
  ]


  return (
    <div>
      <LocationDropDown locations={provinces}/>
    </div>
  );
}

export default App;

```

## 4. สร้างหน้าตาของ Dropdown List ด้วย Bootstrap

เปิดไฟล์ `src/components/LocationDropDown.js` และใส่ code dropdownList ลงไป 

> อ้างอิง - [Bootstrap 4](https://getbootstrap.com/docs/4.0/components/dropdowns/)

```js
import React, {Component} from 'react'

export default class LocationDropDown extends Component {
  render() {
    return (
      <div class="dropdown">
        <a
          class="btn btn-secondary dropdown-toggle"
          href="#"
          role="button"
          id="dropdownMenuLink"
          data-toggle="dropdown"
          aria-haspopup="true"
          aria-expanded="false">
          Dropdown link
        </a>

        <div class="dropdown-menu" aria-labelledby="dropdownMenuLink">
          <a class="dropdown-item" href="#">Action</a>
          <a class="dropdown-item" href="#">Another action</a>
          <a class="dropdown-item" href="#">Something else here</a>
        </div>
      </div>
    )
  }
}

```

## 5. แสดงรายการของ DropDownList จาก props ของ Component

ในที่นี้เราสามารถเข้าถึงค่าที่ส่งผ่านเข้ามาใน Component ผ่าน `this.props` ได้

และเราสามารถใช้ คำสั่ง `array.map((item,index) => {}) ในการวนแสดง JSX ได้

```js
import React, {Component} from 'react'

export default class LocationDropDown extends Component {
  render() {

    let locations = this.props.locations;
    console.log(locations);

    return (
      <div class="dropdown">
        <a
          class="btn btn-secondary dropdown-toggle"
          href="#"
          role="button"
          id="dropdownMenuLink"
          data-toggle="dropdown"
          aria-haspopup="true"
          aria-expanded="false">
          Dropdown link
        </a>

        <div class="dropdown-menu" aria-labelledby="dropdownMenuLink">
          {
              locations.map((location, i) => 
                <a class="dropdown-item" href="#" key={location.id}>{location.name}</a>
              )
          }
        </div>
      </div>
    )
  }
}

```

## 6. เพิ่ม id และ name ให้กับ list item

เปิดไฟล์ `src/index.js` และเพิ่ม id เข้าไปใน array

```js
function App() {

  let provinces = [
    { name: 'Bangkok', id: 1 },
    { name: 'Nonthaburi', id: 2 }
  ]


  return (
    <div>
      <LocationDropDown locations={provinces}/>
    </div>
  );
}
```

เปิดไฟล์ `src/components/LocationDropDown.js` และแสดงค่า id กับ name ลงไปใน attribute `data-`

```js
<div class="dropdown-menu" aria-labelledby="dropdownMenuLink">
    {
        locations.map((location, i) => 
        <a class="dropdown-item" href="#" key={location.id} data-id={location.id} data-name={location.name}>{location.name}</a>
        )
    }
</div>
```

## 7. กำหนด label เริ่มต้นใน Dropdown

เปิดไฟล์ `src/index.js` และเพิ่ม attribute `defaultLabel` เข้าไปใน Component

```js
function App() {

  let provinces = [
    { name: 'Bangkok', id: 1 },
    { name: 'Nonthaburi', id: 2 }
  ]


  return (
    <div>
      <LocationDropDown defaultLabel="Province" locations={provinces}/>
    </div>
  );
}
```

เปิดไฟล์ `src/components/LocationDropDown.js` และแสดงค่า `defaultLabel` ลงไปใน Component

```js
import React, {Component} from 'react'

export default class LocationDropDown extends Component {
  render() {

    let defaultLabel = this.props.defaultLabel;
    let locations = this.props.locations;
    console.log(locations);

    return (
      <div class="dropdown">
        <a
          class="btn btn-secondary dropdown-toggle"
          href="#"
          role="button"
          id="dropdownMenuLink"
          data-toggle="dropdown"
          aria-haspopup="true"
          aria-expanded="false">
          {defaultLabel}
        </a>

        <div class="dropdown-menu" aria-labelledby="dropdownMenuLink">
          {
              locations.map((location, i) => 
                <a class="dropdown-item" href="#" key={location.id} data-id={location.id}>{location.name}</a>
              )
          }
        </div>
      </div>
    )
  }
}
```

## 8. ใส่ Event ให้กับ list item 

เราสามารถประกาศ method และนำไปใช้กับ JSX ได้

```js
locationSelected(e) {
    console.log(e.target);
}
```

JSX

```html
<a onClick={e => this.locationSelected(e)}>...</a>
```


```js
import React, {Component} from 'react'

export default class LocationDropDown extends Component {


  locationSelected(e) {
      console.log(e.target);
  }

  render() {

    let defaultLabel = this.props.defaultLabel;
    let locations = this.props.locations;
    console.log(locations);

    return (
      <div class="dropdown">
        <a
          class="btn btn-secondary dropdown-toggle"
          href="#"
          role="button"
          id="dropdownMenuLink"
          data-toggle="dropdown"
          aria-haspopup="true"
          aria-expanded="false">
          {defaultLabel}
        </a>

        <div class="dropdown-menu" aria-labelledby="dropdownMenuLink">
          {
              locations.map((location, i) => 
                <a class="dropdown-item" href="#" key={location.id} data-id={location.id} onClick={e => this.locationSelected(e)}>{location.name}</a>
              )
          }
        </div>
      </div>
    )
  }
}
```

## 9. ดึงค่า data-name ออกมาจาก HTML

เราสามารถใช้คำสั่ง JavaScript ทั่วไปในการดึงค่า Attribute จาก HTML ได้ 

```js
locationSelected(e) {
    console.log(e.target);

    let locationName = e.target.getAttribute('data-name');
    console.log(locationName);
}
```

```js
import React, {Component} from 'react'

export default class LocationDropDown extends Component {


  locationSelected(e) {
      console.log(e.target);

      let locationName = e.target.getAttribute('data-name');
      console.log(locationName);

  }

  render() {

    let defaultLabel = this.props.defaultLabel;
    let locations = this.props.locations;
    console.log(locations);

    return (
      <div class="dropdown">
        <a
          class="btn btn-secondary dropdown-toggle"
          href="#"
          role="button"
          id="dropdownMenuLink"
          data-toggle="dropdown"
          aria-haspopup="true"
          aria-expanded="false">
          {defaultLabel}
        </a>

        <div class="dropdown-menu" aria-labelledby="dropdownMenuLink">
          {
              locations.map((location, i) => 
                <a class="dropdown-item" href="#" key={location.id} data-id={location.id} onClick={e => this.locationSelected(e)}>{location.name}</a>
              )
          }
        </div>
      </div>
    )
  }
}

```

## 10. กำหนดใช้งาน state แทนการกำหนดค่า props 

เพราะเราสามารถอัพเดตค่า state และทำให้ component render ตัวมันเองใหม่ได้ 

โดยเราสามารถกำหนดค่าเริ่มต้นใน state ได้เหมือน property ทั่วไป

```js
state = {
    label: this.props.defaultLabel
}
```

และเอามาใช้แสดงใน JSX

```js
<a
    class="btn btn-secondary dropdown-toggle"
    href="#"
    role="button"
    id="dropdownMenuLink"
    data-toggle="dropdown"
    aria-haspopup="true"
    aria-expanded="false">
    {this.state.label}
</a>
```

```js
import React, {Component} from 'react'

export default class LocationDropDown extends Component {

  state = {
    label: this.props.defaultLabel
  }

  locationSelected(e) {
      console.log(e.target);

      let locationName = e.target.getAttribute('data-name');
      console.log(locationName);

  }

  render() {

    let defaultLabel = this.props.defaultLabel;
    let locations = this.props.locations;
    console.log(locations);

    return (
      <div class="dropdown">
        <a
          class="btn btn-secondary dropdown-toggle"
          href="#"
          role="button"
          id="dropdownMenuLink"
          data-toggle="dropdown"
          aria-haspopup="true"
          aria-expanded="false">
          {this.state.label}
        </a>

        <div class="dropdown-menu" aria-labelledby="dropdownMenuLink">
          {
              locations.map((location, i) => 
                <a class="dropdown-item" href="#" key={location.id} data-id={location.id} data-name={location.name} onClick={e => this.locationSelected(e)}>{location.name}</a>
              )
          }
        </div>
      </div>
    )
  }
}

```

## 11. ใช้ state ในการอัพเดต Component 

method `setState({})` จะทำให้ React ทำการ render JSX ใหม่อีกครั้ง ทำให้ส่วนที่ดึงข้อมูลจาก state ถูกดึงค่าใหม่ไปแสดงผลด้วย 

```js
locationSelected(e) {
      console.log(e.target);

      let locationName = e.target.getAttribute('data-name');
      console.log(locationName);

      this.setState({
        label: locationName
      })

  }
```