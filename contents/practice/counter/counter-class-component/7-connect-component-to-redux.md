
# เชื่อม Component เข้ากับ Redux store

## AddNumber component

```jsx
// src/components/AddNumber.js

import React, { Component } from 'react'
import { connect } from 'react-redux'
import action from '../redux/action'

// ตรงนี้จะไม่ใช่ export default แล้ว
export class AddNumber extends Component {

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

// ใช้จับคู่ redux state กับ props ของ component
const mapStateToProps = (state) => ({
    
})

// ใช้จับคู่ function กับ props ของ component
const mapDispatchToProps = () => {
}

export default connect(mapStateToProps, mapDispatchToProps)(AddNumber)
```

## Counter component

```jsx
import React, { Component } from 'react'
import { connect } from 'react-redux'

export class Counter extends Component {
    render() {
        return (
            <div>
                <h1>0</h1>
            </div>
        )
    }
}

const mapStateToProps = (state) => ({
})

const mapDispatchToProps = {
    
}

export default connect(mapStateToProps, mapDispatchToProps)(Counter)

```