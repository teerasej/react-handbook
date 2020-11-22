
# สร้างส่วนปุ่มเพิ่มจำนวน Counter

สร้างไฟล์ `src/components/AddNumber.js`

```jsx
// src/components/AddNumber.js

import React, { Component } from 'react'
import { connect } from 'react-redux'

export default class AddNumber extends Component {

    addMoreNumber = () => {
       
    }


    render() {
        return (
            <div>
                <button onClick={this.addMoreNumber}>เพิ่ม</button>
            </div>
        )
    }
}

```