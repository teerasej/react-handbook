
# ใช้งาน allBranches ที่ได้จาก Reducer

ตอนนี้ reducer ได้เพิ่มข้อมูลที่มาจาก action ของ Web API ไว้ใน state object ชื่อว่า `allBranches` แล้ว 

ตอนนี้ Component ที่เชื่อมต่อกับ Redux ผ่านฟังก์ชั่น `connect()` จะสามารถรับ state object ที่มีข้อมูลดังกล่าวได้ 

เปิดไฟล์ `src/components/MapBranch.js`

เราจะทำการเขียน `mapStateToProps` เพื่อกำหนดค่าที่ได้จาก state เข้าไปใน props ของ component

```js
const mapStateToProps = (state) => ({
  // กำหนดค่า state.allBranches ที่ได้จาก reducer ในชื่อ allBranches ของ this.props ใน component
  // เราจะสามารถเรียกใช้ข้อมูลส่วนนี้ ใน component ได้จาก this.props.allBranches
  allBranches: state.allBranches
})
```

จากนั้นเราก็เอาข้อมูลที่ได้จาก `this.props.allBranches` มาวนลูปใช้งานใน `MapBranch` component 



## ไฟล์สมบูรณ์ `src/components/MapBranch.js`

```js
import React, {Component} from 'react'
import GoogleMapReact from 'google-map-react';

import { connect } from 'react-redux'
import { getBranchLocation, showBranchChart } from '../redux/actions';


export class MapBranch extends Component {

  componentDidMount() {
    this.props.initData();
  }

  static defaultProps = {
    center: {
      lat: 13.7200452,
      lng: 100.5135078
    },
    zoom: 15
  };

  handleApiLoaded(map, maps) {
    let bounds = new maps.LatLngBounds();
    // ดึงค่าจาก this.props.allBranches ซึ่งข้อมูลในตัวแปรนี้ ได้จากการกำหนดค่าจาก mapStateToProps
    let branches = this.props.allBranches;

    // เอาข้อมูลสาขาที่ได้มาวนลูป สร้างหมุดปักบนแผนที่
    branches.forEach(branch => {
      let marker = new maps.Marker({
        position: branch.position,
        map,
        title: branch.name,
        id: branch.id
      });

      // marker.addListener('click', () => { this.props.markerClick(branch)) })

      
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

const mapStateToProps = (state) => ({
  allBranches: state.allBranches
})

const mapDispatchToProps = (dispatch) => {
  return {
    initData: () => getBranchLocation(dispatch)
  }
}


export default connect(mapStateToProps,mapDispatchToProps)(MapBranch);
```