
# 19. เตรียมเขียน Test UI ตัวแรก

## 1. สร้าง global test file

สร้างไฟล์ `src/setupTests.js`

```js
import Enzyme, { shallow } from 'enzyme'
import Adapter from 'enzyme-adapter-react-16'

Enzyme.configure({ adapter: new Adapter() }) 
```

## 2. เราจะทำการ import และตั้งค่า Enzyme module

```js
// src/App.test.js

mport ReactDOM from 'react-dom';
import App from './App';

import Enzyme, { shallow } from 'enzyme'
import Adapter from 'enzyme-adapter-react-16'

Enzyme.configure({ adapter: new Adapter() });
```

## 3. จากนั้น ก็สร้าง test case แรกของเรา 

โดยส่วนแรก เราจะทดสอบว่า App component พังหรือไม่

```js
// src/App.test.js

describe('App component', () => {
  
  it('renders without crashing', () => {
 
    const component = shallow(<App />);
  
  });
})
```

จากนั้นให้ทดสอบรัน test โดยใช้คำสั่ง 

```bash
yarn test
```