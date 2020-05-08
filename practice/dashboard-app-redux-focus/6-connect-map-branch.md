
# เชื่อมต่อ MapBranch เข้ากับ redux 

ถึงแม้ว่า Component จะถูกครอบด้วย Provider แต่ Component ที่ต่อเข้ากับ Redux อย่างถูกต้องเท่านั้น ถึงจะทำงานร่วมกับ reducer ได้ 

ในส่วนนี้เราจะเชื่อมต่อ MapBranch เข้ากับ Reducer กัน

```js
import React, {Component} from 'react'
import GoogleMapReact from 'google-map-react';

// เรียกใช้ connect function ที่ redux เตรียมไว้ให้ เราจะใช้มันเชื่อม MapBranch component เข้ากับ redux
import { connect } from 'react-redux'


export class MapBranch extends Component {

  static defaultProps = {
    center: {
      lat: 13.7200452,
      lng: 100.5135078
    },
    zoom: 15
  };

  handleApiLoaded(map, maps) {
    // let bounds = new maps.LatLngBounds();
    // let branches = this.props.allBranches;

    // branches.forEach(branch => {
    //   let marker = new maps.Marker({
    //     position: branch.position,
    //     map,
    //     title: branch.name,
    //     id: branch.id
    //   });

    //   marker.addListener('click', () => { this.props.markerClick(marker.id) })

      
    //   bounds.extend(branch.position);
    // });

    // map.fitBounds(bounds); 
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

// ใช้ snippet 'reduxmap' เพื่อสร้าง mapStateToProps และ mapDispatchToProps ได้ 

// function นี้จะรับตัวแปร state มาจาก reducer
const mapStateToProps = (state) => ({
})

// function นี้จะส่ง action ไปที่ reducer ผ่าน dispatch() 
const mapDispatchToProps = (dispatch) => {
  
}

// เชื่อมต่อ mapStateToProps และ mapDispatchToProps เข้ากับ MapBranch component 
// ผ่าน connect()
// ถ้าทำถูกต้อง MapBranch จะเชื่อมเข้ากับ redux store 
export default connect(mapStateToProps,mapDispatchToProps)(MapBranch);
```