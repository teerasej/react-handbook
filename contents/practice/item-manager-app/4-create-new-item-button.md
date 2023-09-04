
# สร้างและจัด Layout ให้ปุ่มเพิ่มสินค้า

```jsx
// src/App.js

// ทำการ import component ที่จำเป็นเพิ่มเติม
import { Layout, Row, Col, Button, Divider } from 'antd';

//..
return (
      <div>
        <Layout className="layout">
          <HeaderBar />
          <Content style={{
            padding: '0 50px'
          }}>
            <div
              style={{
                background: '#fff',
                padding: 24,
                minHeight: 280
              }}>

              {/* สร้าง Layout ให้ปุ่มเพิ่มสินค้าใหม่ */}
              <Row gutter={16}>
                <Col span={20}></Col>
                <Col span={4}>
                  {/* ปุ่มเพิ่มสินค้าใหม่ */}
                  <Button type="primary" size="large" block>Add Item</Button>
                </Col>
              </Row>
              <Divider/>
               {/* จัด ItemList อยู่ด้านล่าง */}
              <Row gutter={16}>
                <Col span={24}><ItemList /></Col>
              </Row>
              
            </div>
          </Content>
          <Footer style={{
            textAlign: 'center'
          }}>React Redux Workshop ©2012-2021 Created by Nextflow.in.th</Footer>
        </Layout>,
      </div>
    );
```