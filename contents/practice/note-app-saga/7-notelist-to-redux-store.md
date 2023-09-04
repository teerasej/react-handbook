# 7. เชื่อม NoteList Component เข้า Redux Store

## 1. Mapping component เข้ากับ redux connect เพื่อสร้างเป็น component container

![Paper React   React Native 28](https://user-images.githubusercontent.com/85179/63178859-15258d80-c075-11e9-9a0b-359a3743f06c.png)

React Component ทั่วไป จะไม่มีการเชื่อมกับระบบ Redux ตั้งแต่แรก แต่เราสามารถจับมันมาตั้งค่าให้ใช้งานกับ Redux ได้

เราเรียก Component พวกนี้ว่า **Redux Container** หรือ **Component Container** ครับ

เริ่มจาก `src/pages/home-page/NoteList.js`

เรา import module ชื่อ `connect` เข้ามาก่อน 

```js
// ใช้ snippet 'redux' ได้
import { connect } from "react-redux";
```

จากนั้นเราจะย้าย `export default` จากที่ใช้กับ Class ของเรา ไปใช้กับส่วนอื่น

```js
export default class NoteList extends Component {

// เป็น

class NoteList extends Component {
```

ซึ่งเราจะมาเขียนในส่วนด้านล่างแทน

เป็น

```js
// snippet 'reduxmap'
const mapStateToProps = (state) => ({

})

const mapDispatchToProps = {

}


export default connect(mapStateToProps, mapDispatchToProps)(NoteList) 
```

### ไฟล์เต็ม `src/pages/home-page/NoteList.js`

```js
import React, { Component } from 'react'
import { List } from 'antd';
import { connect } from 'react-redux'


class NoteList extends Component {

    data = [
        {
          title: 'Note 1',
        },
        {
          title: 'Note 2',
        },
        {
          title: 'Note 3',
        },
        {
          title: 'Note 4',
        },
      ];


    render() {
        return (
            <List
                bordered
                itemLayout="horizontal"
                dataSource={this.data}
                renderItem={item => (
                    <List.Item>
                        <List.Item.Meta
                            title={<p>{item.title}</p>}
                            description=""
                        />
                    </List.Item>
                )}
            />
        )
    }
}

const mapStateToProps = (state) => ({
  
})

const mapDispatchToProps = {
  
}


export default connect(mapStateToProps, mapDispatchToProps)(NoteList)
```

## 2. จับคู่ค่า state ที่จะส่งออกมาจาก store เข้ากับ props ของ component

ในที่นี้ Reducer ของเรามี initialState มีที่ข้อมูลของตำแหน่งสาขาอยู่ 

ซึ่งถ้าต้องการเอามาใช้ใน Container เราก็สามารถเขียนค่าใน `mapStateToProps` ได้แบบด้านล่าง 

**state** ที่เห็นตรงนี้คือ state ที่ reducer ส่งออกมาให้ Component ต่างๆ นั่นเอง

```js
const mapStateToProps = (state) => ({
    noteData: state.notes
})
```
หรือแบบนี้ก็ได้
```js
const mapStateToProps = (state) => {
    return {
        noteData: state.notes
    }
}
```

## 3. สลับมาดึงข้อมูลจาก props ของ component ที่ได้จากการ map กับ Redux store 

`mapStateToProps` เป็นส่วนที่เราเขียนดึงค่าจาก state ที่ได้ ให้กับ `props` ดังนั้นในที่นี้เราจึงสามารถดึงค่า `this.props.noteData` มาใช้ใน component ได้ 

```js
return (
    <List
        bordered
        itemLayout="horizontal"
        dataSource={this.props.noteData} // <-- ตรงนี้แหละ
        renderItem={item => (
            <List.Item>
                <List.Item.Meta
                    title={<p>{item.title}</p>}
                    description=""
                />
            </List.Item>
        )}
    />
)
```

### ไฟล์เต็ม `src/pages/home-page/NoteList.js`

```js

import React, { Component } from 'react'
import { List } from 'antd';
import { connect } from 'react-redux'


class NoteList extends Component {


    render() {
        return (
            <List
                bordered
                itemLayout="horizontal"
                dataSource={this.props.noteData}
                renderItem={item => (
                    <List.Item>
                        <List.Item.Meta
                            title={<p>{item.title}</p>}
                            description=""
                        />
                    </List.Item>
                )}
            />
        )
    }
}

const mapStateToProps = (state) => ({
  noteData: state.notes
})

const mapDispatchToProps = {
  
}


export default connect(mapStateToProps, mapDispatchToProps)(NoteList)
```