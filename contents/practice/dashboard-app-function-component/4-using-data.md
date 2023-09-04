
# ใช้งานข้อมูลกับแผนที่

## 1. สร้างข้อมูลจำลอง

สร้างไฟล์ `src/models/branchModel.js`

```js
const headquarter = {
  id: 1,
  name: 'Silom',
  position: {
    lat: 13.7200452,
    lng: 100.5135078
  },
  chartData: {
    chartField: ['Month','Amount'],
    datas: [
      { month: 'February', amount: 10392 },
      { month: 'March', amount: 18239 },
      { month: 'April', amount: 14290 },
      { month: 'May', amount: 23912 },
      { month: 'June', amount: 26167 },
      { month: 'July', amount: 28199 },
    ]
  }
}

let branches = [
  headquarter, {
    id: 2,    
    name: 'Cholburi',
    position: {
      lat: 13.1247584,
      lng: 100.9133127
    },
    chartData: {
      chartField: ['Month','Amount'],
      datas: [
        { month: 'February', amount: 70032 },
        { month: 'March', amount: 54789 },
        { month: 'April', amount: 62789 },
        { month: 'May', amount: 89272 },
        { month: 'June', amount: 100328 },
        { month: 'July', amount: 128903 },
      ]
    }
  }
]

export default {
  branches,
  headquarter
}
```

## 2. ปักหมุดตามข้อมูลที่มี

เปิดไฟล์ `src/components/MapBranch.js`

เราจะ import javascript มาจาก `branchModel.js`

```js
import BranchModel from "../models/branchModel";
```

และใช้ในการสร้าง Marker

```js
const handleApiLoaded = (map, maps) => {
    BranchModel.branches.forEach(branch => {
        let marker = new maps.Marker({
            position: branch.position,
            map,
            title: branch.name
        });
    });
}
```

## 3. ซูมให้ครอบคลุมทุกตำแหน่งที่ปักหมุด

เราจะใช้ Google Maps API `maps.LatLngBounds()` และ `map.fitBounds(bounds);` ในการซูมให้ครอบคลุมหมุดในแผนที่ทุกตัว

```js
const handleApiLoaded = (map, maps) => {

    let bounds = new maps.LatLngBounds();

    BranchModel.branches.forEach(branch => {
      let marker = new maps.Marker({
        position: branch.position,
        map,
        title: branch.name
      });

      
      bounds.extend(branch.position);
      // Alternative
      // bounds.extend(new maps.LatLng(branch.lat, branch.lng);
    });

    map.fitBounds(bounds); 
  }
```

### ไฟล์เต็ม MapBranch.js

```js
import React from 'react'
import GoogleMapReact from 'google-map-react';
import BranchModel from "../models/branchModel";

export default function MapBranch({
    center = {
        lat: 13.7200452,
        lng: 100.5135078
    },
    zoom = 15
}) {

    const handleApiLoaded = (map, maps) => {
        let bounds = new maps.LatLngBounds();

        BranchModel.branches.forEach(branch => {
            let marker = new maps.Marker({
                position: branch.position,
                map,
                title: branch.name
            });


            bounds.extend(branch.position);
            
        });

        map.fitBounds(bounds);
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