
# สร้าง Map Branch Component

## ติดตั้ง package 

ถ้ายังไม่ได้ติดตั้ง package ชื่อ `google-map-react` ล่ะก็ อย่าลืมรันคำสั่งติดตั้งให้กับโปรเจคล่ะ 

```bash
npm i google-map-react
```

### ใช้ yarn?

```bash
yarn add google-map-react
```

## 1. สร้าง MapBranch Component

สร้างไฟล์ `src/components/MapBranch.js` (ใช้ snippet: rfc)

```js
import React from 'react'

export default function MapBranch() {
    return (
        <div>
            Map
        </div>
    )
}
```

## 2. วาง MapBranch component ใน App.js

เปิดไฟล์ `src/App.js`

```js
//...
import MapBranch from './components/MapBranch';
const {Header, Content, Footer} = Layout;

function App() {
  return (
    <div>
      <Layout className="layout">
        <HeaderBar/>
        <Content style={{
          padding: '0 50px'
        }}>
          <div
            style={{
            background: '#fff',
            padding: 24,
            minHeight: 280
          }}>
            {/* วางไว้ในส่วน content */}
            <MapBranch />

          </div>
        </Content>
        <Footer style={{
          textAlign: 'center'
        }}>React Redux Workshop ©2012-2019 Created by Nextflow.in.th</Footer>
      </Layout>,
    </div>
  );
}

export default App;
```


## 3. ใช้งาน GoogleMapReact component

> สามารถไปเปิดใช้งาน Google Map API Key ได้ที่ [Google Map Platform](https://console.cloud.google.com/google/maps-apis/overview)

> คู่มือการใช้งาน google-map-react  => [google-map-react](https://github.com/google-map-react/google-map-react)

เริ่มจากตั้งค่าเริ่มต้นให้กับ `props` ก่อน

```js
export default function MapBranch({
  center = {
    lat: 13.7200452,
    lng: 100.5135078
  },
  zoom = 15
}) {
    return (
        <div>
            Map
        </div>
    )
}
```

จากนั้น import `GoogleMapReact` มาใช้งานใน MapBranch

```js
import GoogleMapReact from 'google-map-react';
```


และใช้ API key ที่สร้างจาก Google Maps Platform กับ `<GoogleMapReact>` 

```jsx
<GoogleMapReact
    bootstrapURLKeys={{
    key: 'AIzaSyBDqlW1EIlePcA48oLVV_kYQJXm9dQ75uw'
}}
    defaultCenter={center}
    defaultZoom={zoom}>
</GoogleMapReact>
```


### ไฟล์ MapBranch.js เต็ม

```js
import React from 'react'
import GoogleMapReact from 'google-map-react';

export default function MapBranch({
    center = {
      lat: 13.7200452,
      lng: 100.5135078
    },
    zoom = 15
  }) {
      return (
        <div style={{
            height: '100vh',
            width: '100%'
          }}>
            <GoogleMapReact
              bootstrapURLKeys={{
              key: 'AIzaSyBDqlW1EIlePcA48oLVV_kYQJXm9dQ75uw'
            }}
            defaultCenter={center}
            defaultZoom={zoom}>
            </GoogleMapReact>
          </div>
      )
  }

```

## 4. จัด Layout แผนที่ กับ Chart ใน App.js

เปิดไฟล์ `src/App.js`

เราจะนำเอา `Row` กับ `Col` ของ Ant Design มาใช้ด้วย ให้เรา import เข้ามา 

```js
import { Layout, Menu, Row, Col } from 'antd';
```

จากนั้นเราจะใช้ `Row` กับ `Col` จัดพื้นที่ของ `MapBranch` ในส่วน `Content` เป็น 50% และตั้งค่าระยะห่าง (gutter) เป็น 16

```jsx
<Content style={{
    padding: '0 50px'
}}>
    <div
    style={{
        background: '#fff',
        padding: 24,
        minHeight: 280
    }}>

    {/* จัด layout ด้วย row และ column */}
    <Row gutter={16}>
        <Col span={12}><MapBranch /></Col>
        <Col span={12}>Chart</Col>
    </Row>

    </div>
</Content>
```

### ไฟล์เต็มของ App.js

```js
import logo from './logo.svg';
import './App.css';
import HeaderBar from './components/HeaderBar';
import { Layout, Menu, Row, Col } from 'antd';
import MapBranch from './components/MapBranch';

const { Header, Content, Footer } = Layout;

function App() {
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
            <Row gutter={16}>
              <Col span={12}><MapBranch /></Col>
              <Col span={12}>Chart</Col>
            </Row>

          </div>
        </Content>
        <Footer style={{
          textAlign: 'center'
        }}>React Redux Workshop ©2012-2020 Created by Nextflow.in.th</Footer>
      </Layout>,
    </div>
  );
}

export default App;


```

## 5. ปักหมุดเมื่อแผนที่โหลดเสร็จ

เปิดไฟล์ `src/components/MapBranch.js`

เริ่มต้นโดยการใช่ props `yesIWantToUseGoogleMapApiInternals` และ event `onGoogleApiLoaded` ให้กับ 

```jsx
<GoogleMapReact
    bootstrapURLKeys={{
    key: 'AIzaSyBDqlW1EIlePcA48oLVV_kYQJXm9dQ75uw'
}}
    defaultCenter={center}
    defaultZoom={zoom}

    yesIWantToUseGoogleMapApiInternals
    onGoogleApiLoaded={({ map, maps }) => handleApiLoaded(map, maps)}
    >
    
    </GoogleMapReact>
```

จากนั้น event `onGoogleApiLoaded` จะส่ง object `map` (นี่คือตัวแผนที่) และ `maps` (นี่คือ api `google.maps` นั่นเอง) ออกมา 

เราสามารถใช้คำสั่งใน [Google Maps JavaScript API](https://developers.google.com/maps/documentation/javascript/tutorial) ได้ตามปกติ เช่นการปักหมุด

```js
const handleApiLoaded = (map, maps) => {
    let marker = new maps.Marker({
        position: this.props.center,
        map,
        title: 'HQ'
    });
  }
```


### ไฟล์เต็ม MapBranch.js

```js
import React from 'react'
import GoogleMapReact from 'google-map-react';

export default function MapBranch({
    center = {
        lat: 13.7200452,
        lng: 100.5135078
    },
    zoom = 15
}) {

    const handleApiLoaded = (map, maps) => {
        console.log(maps);
        let marker = new maps.Marker({
            position: center,
            map,
            title: 'HQ'
        });
      }

    return (
        <div style={{
            height: '100vh',
            width: '100%'
        }}>
            <GoogleMapReact
                bootstrapURLKeys={{
                    key: 'AIzaSyBDqlW1EIlePcA48oLVV_kYQJXm9dQ75uw'
                }}
                defaultCenter={center}
                defaultZoom={zoom}

                yesIWantToUseGoogleMapApiInternals
                onGoogleApiLoaded={({ map, maps }) => handleApiLoaded(map, maps)}
            >

            </GoogleMapReact>
        </div>
    )
}

```
