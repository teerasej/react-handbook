
# ติดตั้ง และ setup react router และ React router modal

ถ้สยังไม่ได้ติดตั้ง ให้ติดตั้งก่อนนะ

```bash
npm i react-router-dom react-router-modal

yarn add react-router-dom react-router-modal
```

## 1. import React Router และ React router modal

```js
// src/App.js

// เจ้าของ react-router-modal มีการเขียน CSS ไว้ ให้เรานำเข้ามาด้วย
import 'react-router-modal/css/react-router-modal.css';


import {
  BrowserRouter as Router,
  Switch,
  Link
} from "react-router-dom";

import { ModalContainer,ModalRoute } from 'react-router-modal';
```

## 2. กำหนด ส่วนการทำงานของ Router 

```jsx
// src/App.js

return (
      {/* กำหนด Router component เพื่อจัดการทำงานของ Router */ }
      <Router>
        <div>
            ...
        </div>
      </Router>
```

## 3. กำหนด Switch เพื่อสร้าง Route

```jsx
return (
      <Router>
      <div>
        <Layout className="layout">
          {/* ... */ }
            <Footer style={{
                textAlign: 'center'
            }}>...</Footer>

            {/* ส่วนของ Switch คือพื้นที่ที่สามารถสลับการแสดงผล จากการจับคู่ Route path เข้ากับ Component ต่างๆ ได้ */ }
            <Switch>

                {/* ในที่นี้เราใช้ react-router-modal เพื่อแสดง component ในลักษณะ pop up จึงใช้ <ModalRoute> แทน <Route> ทั่วไป */ }
                <ModalRoute path='/add-new-item' parentPath='/' >
                    Hello
                </ModalRoute>

            </Switch>
            
            {/* ModalContainer ของ react-router-modal จะเป็นส่วนที่จัดการแสดงผล modal pop up ให้ */ }
            <ModalContainer />
        </Layout>
      </div>
      </Router>
    );
```

## 4. กำหนด <Link> เพื่อเรียกใช้ Route

เราสามารถใช้ `<Link>` ที่ react-router-dom สร้างมาให้ กำหนดส่วน UI ให้สามารถตอบสนองกับการกด และเรียกใช้ route path ที่ต้องการได้

ในที่นี้เรากำหนดให้กับปุ่มของเรา

```jsx
<Link to="/add-new-item">
    <Button type="primary" size="large" block>Add Item</Button>
</Link>
```

จากตรงนี้สามารถทดสอบกดปุ่ม จะเห็นข้อความขึ้นมาตามที่กำหนดไว้ใน `<ModalRoute/>`


