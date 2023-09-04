

# ใช้งาน Action method ใน MapBranch

เปิด์ไฟล์ `src/components/MapBranch.js` 

เราจะทำการเรียกใช้ action method ที่สร้างขึ้น มาใช้ใน MapBranch component 

```js
import { getBranchLocation, showBranchChart } from '../redux/actions';
```

เราจะทำการเพิ่มฟังก์ชั่น `this.props.markerClick` ผ่าน `mapStateToDispatch` 

```js
const mapDispatchToProps = (dispatch) => {
  return {
    initData: () => getBranchLocation(dispatch),
    // เพิ่มฟังก์ชั่น `this.props.markerClick` ซึ่งจะรับค่า branchData และ dispatch ไปเรียกใช้ showBranchChart 
    markerClick: (branchData) => showBranchChart(branchData, dispatch)
  }
}
```

ใน function `handleApiLoaded` เราจะมีการเรียกใช้ this.props.markerClick โดยส่งค่าของ สาขาที่ใช้ในการวนลูปลงไปด้วย 

```js
handleApiLoaded(map, maps) {
    let bounds = new maps.LatLngBounds();
    let branches = this.props.allBranches;

    branches.forEach(branch => {
      let marker = new maps.Marker({
        position: branch.position,
        map,
        title: branch.name,
        id: branch.id
      });

      marker.addListener('click', () => { this.props.markerClick(branch) })

      
      bounds.extend(branch.position);
     
    });

    map.fitBounds(bounds); 
  }
```

## ไฟล์เต็ม MapBranch.js 

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
    let branches = this.props.allBranches;

    branches.forEach(branch => {
      let marker = new maps.Marker({
        position: branch.position,
        map,
        title: branch.name,
        id: branch.id
      });

      marker.addListener('click', () => { this.props.markerClick(branch) })

      
      bounds.extend(branch.position);
     
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
    initData: () => getBranchLocation(dispatch),
    markerClick: (branchData) => showBranchChart(branchData, dispatch)
  }
}


export default connect(mapStateToProps,mapDispatchToProps)(MapBranch);
```