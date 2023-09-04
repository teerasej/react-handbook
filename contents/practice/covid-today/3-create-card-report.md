
# สร้าง Grid Layout และ Card Report

## 1. สร้าง Grid Layout

- [อ้างอิง Ant Design - Grid Component](https://ant.design/components/grid/)

เปิดไฟล์ `src/App.js`

เราจะทำการ import `Row` และ `Col` component จาก `antd`

```js
import { Row, Col } from 'antd';
```

จากนั้นเราจะกำหนด grid layout ในส่วน content ของ App component

```jsx
return (
    <div>
      <Layout className="layout">
        <MenuBar />
        <Content style={{
          padding: '0 50px'
        }}>
          <div
            style={{
              background: '#fff',
              padding: 24,
              minHeight: 280
            }}>
            {/* กำหนด layout 3 ช่วง */}
            <Row>
              <Col span={8}>col-8</Col>
              <Col span={8}>col-8</Col>
              <Col span={8}>col-8</Col>
            </Row>
          </div>
        </Content>
        <Footer style={{
          textAlign: 'center'
        }}>React Redux Workshop ©2012-2020 Created by Nextflow.in.th</Footer>
      </Layout>,
    </div>
  );
```

## 3. วาง Card report 

- [อ้างอิง Ant Design - Card Component](https://ant.design/components/card/)

เราจะทำการ import `Card` component มาจาก `antd`

```js
import { Card } from 'antd';
```

จากนั้นจะวาง Card 3 ตัวลงไปใน Column แต่ละช่อง 



```jsx
<Row>
    {/* กำหนด margin left ของ column เป็น 10 */}
    <Col span={8} style={{marginLeft: 10}}>
        {/* กำหนด property ชื่อ title และ loading */}
        <Card title="จำนวนผู้ติดเชื้อ"  loading={true}>
            <p>0 คน</p>
        </Card>
    </Col>
    {/* กำหนด margin left ของ column เป็น 10 */}
    <Col span={8} style={{marginLeft: 10}}>
        <Card title="จำนวนป่วยที่หายดีแล้ว" loading={true}>
            <p>0 คน</p> 
        </Card>
    </Col>
     {/* กำหนด margin left และ margin top ของ column เป็น 10 */}
    <Col span={8} style={{marginLeft: 10, marginTop: 10}}>
        <Card title="จำนวนผู้เสียชีวิต" loading={true}>
            <p>0 คน</p>
        </Card>
    </Col>
</Row>
```