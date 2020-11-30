
# สร้างส่วนแสดงจำนวน Counter

สร้างไฟล์ `src\components\Counter.js`

```jsx
// src/components/Counter.js

import React, { Component } from 'react'
import { connect } from 'react-redux'

export default class Counter extends Component {
    render() {
        return (
            <div>
                <h1>{this.props.count}</h1>
            </div>
        )
    }
}
```