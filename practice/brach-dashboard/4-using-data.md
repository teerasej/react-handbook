
# ใช้งานข้อมูลกับแผนที่

## 1. สร้างข้อมูลจำลอง

สร้างไฟล์ `src/models/branchModel.js`

```js
const headquarter = {
  name: 'Silom',
  position: {
    lat: 13.7200452,
    lng: 100.5135078
  }
}

let branches = [
  headquarter, {
    name: 'Cholburi',
    position: {
      lat: 13.1247584,
      lng: 100.9133127
    }
  }
]

export default {
  branches,
  headquarter
}; 
```

## 2. ปักหมุดตามข้อมูลที่มี

เปิดไฟล์ `src/components/MapBranch.js`

เราจะ import javascript มาจาก `branchModel.js`

```js
import BranchModel from "../models/branchModel";
```

และใช้ในการสร้าง Marker

```js
handleApiLoaded(map, maps) {
    BranchModel.branches.forEach(branch => {
        new maps.Marker({
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
handleApiLoaded(map, maps) {

    let bounds = new maps.LatLngBounds();

    BranchModel.branches.forEach(branch => {
      new maps.Marker({
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
import React, { Component } from 'react'
import GoogleMapReact from 'google-map-react';

import BranchModel from "../models/branchModel";

class MapBranch extends Component {

  static defaultProps = {
    // Kerry Siam Seaport Location
    center: {
      lat: 13.7200452,
      lng: 100.5135078
    },
    zoom: 15
  };

  handleApiLoaded(map, maps) {

    let bounds = new maps.LatLngBounds();

    BranchModel.branches.forEach(branch => {
      new maps.Marker({
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

  render() {
    return (
      <div style={{
        height: '100vh',
        width: '100%'
      }}>
        <GoogleMapReact
          bootstrapURLKeys={{
            key: 'AIzaSyBDqlW1EIlePcA48oLVV_kYQJXm9dQ75uw'
          }}
          defaultCenter={this.props.center}
          defaultZoom={this.props.zoom}
          yesIWantToUseGoogleMapApiInternals
          onGoogleApiLoaded={({ map, maps }) => this.handleApiLoaded(map, maps)}
        >

        </GoogleMapReact>
      </div>
    )
  }
}

export default MapBranch
```