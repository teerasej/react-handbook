

```jsx
import React, { Component } from 'react'
import { connect } from 'react-redux'
import action from '../redux/action'

export class AddNumber extends Component {

    addMoreNumber = () => {
        // เรียกใช้ dispatch function ผ่าน props object
        this.props.add();
    }


    render() {
        return (
            <div>
                
                <button onClick={this.addMoreNumber}>เพิ่ม</button>
            </div>
        )
    }
}

const mapStateToProps = (state) => ({
    
})

const mapDispatchToProps = (dispatch) => {
    // สร้าง object ที่มี function เป็น property
    // object นี้จะสามารถเข้าถึงได้ จากภายใน component ผ่าน this.props
    return {
        add: () => dispatch({ type: action.Actions.ADD_NUMBER, payload: 1 })
    }
}

export default connect(mapStateToProps, mapDispatchToProps)(AddNumber)
```