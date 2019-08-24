
# 20. ทดสอบว่ามี Spin Component อยู่ใน App Component ไหม

## 1. เขียน test case ทดสอบว่ามี component ดังกล่าวใน component ที่สนใจไหม

เราจะมีการ import Spin component ของ Ant Design เข้ามาช่วยในที่นี้ด้วย 

```js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import { Spin } from "antd";

import Enzyme, { shallow } from 'enzyme'
import Adapter from 'enzyme-adapter-react-16'

Enzyme.configure({ adapter: new Adapter() })

describe('App component', () => {
  
  it('renders without crashing', () => {
 
    const component = shallow(<App />);
  
  });

  it('should contain <Spin>', () => {
    
    const component = shallow(<App />);

    expect(component.find('Spin')).toHaveLength(1);

  });


})
```

ทดสอบรัน test 

- `component.find()` สามารถใช้หา children component ที่อยู่ด้านใน App ได้ ซึ่งถ้าเจอจะนับเป็นจำนวนที่พบ 
- ` expect().toHaveLength(1)` เป็นการเทียบค่าที่ได้จริง ว่ามีจำนวนเท่ากับ 1 (ในที่นี้คือเจอ Spin 1 ตัว)

## 2. เพิ่ม Spin Component ใน App 

เริ่มจาก import เพิ่มเติมจาก Ant Design 

```js
import { Layout, Spin } from 'antd';
```

และครอบ Component ของเรา 

```js
function App() {
  return (
      <div>
        <Spin>
        <Layout className="layout">
          <MenuBar />
          <Switch>
            //...
          <Footer style={{
            textAlign: 'center'
          }}>React Redux Workshop ©2012-2019 Created by Nextflow.in.th</Footer>
        </Layout>,
        </Layout>
        </Spin>
      </div>
  );
}
```

ทดสอบรัน test 

