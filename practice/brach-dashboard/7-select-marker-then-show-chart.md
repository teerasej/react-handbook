
# สร้างกลไก Action: กดเลือกหมุดและแสดง Chart

## 1. เพิ่มรายละเอียดของข้อมูลสาขา

เปิดไฟล์ `src/models/branchModel.js` และปรับโค้ดให้เป็นแบบด้านล่าง

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
};
```

## 2. ตรวจจับการคลิกเลือกหมุด marker

เราจะ:
1. กำหนด `branch.id` เข้าไปใน Marker ในชื่อของ `branchId`
2. เพิ่ม function `markerClick` เข้าไปเป็น EventListener ของ Marker ทุกตัว

```js
markerClick = (marker) => {
  console.log(`Selected branch: ${marker.get('title')}`);
}

handleApiLoaded(map, maps) {

  //..
  let branches = this.props.branches;

  branches.forEach(branch => {
    let marker = new maps.Marker({
      position: branch.position,
      map,
      title: branch.name,
      // กำหนด id ของสาขา
      branchId: branch.id
    });

    // กำหนด event listenter สำหรับการคลิกเลือก
    marker.addListener('click', () => { this.markerClick(marker) })

    
    bounds.extend(branch.position);
  });

  //..
}
```

### ไฟล์เต็ม

```js
import React, { Component } from 'react'
import GoogleMapReact from 'google-map-react';

import { connect } from "react-redux";

class MapBranch extends Component {

  static defaultProps = {
    // Kerry Siam Seaport Location
    center: {
      lat: 13.7200452,
      lng: 100.5135078
    },
    zoom: 15
  };

  markerClick = (marker) => {
    console.log(`Selected branch: ${marker.get('title')}`);
  }

  handleApiLoaded(map, maps) {

    let bounds = new maps.LatLngBounds();
    let branches = this.props.branches;

    branches.forEach(branch => {
      let marker = new maps.Marker({
        position: branch.position,
        map,
        title: branch.name,
        branchId: branch.id
      });

      marker.addListener('click', () => { this.markerClick(marker) })

      
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
  branches: state.branches
})

const mapDispatchToProps = {
  
}


export default connect(mapStateToProps, mapDispatchToProps)(MapBranch)
```

## 3. สร้าง function ให้กับ props ใน `mapDispatchToProps` เพื่อใช้ส่ง Action เข้า Reducer

เริ่มจาก 

```js
import actions from '../redux/actions';
```

และเราจะเรียกใช้ function `props.showBranchDataInChart` เพื่อส่ง branch ID ไปให้ reducer 

**function นี้เราจะสร้างขึ้นในส่วน `mapDispatchToProps` ของ Redux ด้านล่างของไฟล์**

```js
markerClick = (marker) => {
  console.log(`Selected branch: ${marker.get('branchId')} ${marker.get('title')}`);

  let branchId = marker.get('branchId');
  this.props.showBranchDataInChart(branchId);
}
```

แก้จาก 

```js
const mapDispatchToProps = {
}
```

เป็น

```js
const mapDispatchToProps = dispatch => {
  return {
    showBranchDataInChart: (branchId) => dispatch(actions.showBranchData(branchId))
  }
}
```

ไหนๆ ก็ไหนๆ ลองแว้บกลับไปดูที่ `src/redux/actions.js` ว่ามี function ชื่อ `showBranchData` อยู่จริงไหม 

```js
const showBranchData = (payload) => ({
    type: ActionTypes.SHOW_BRANCH_DATA,
    payload: payload
})
```

แล้วแก้ชื่อ parameter  `payload` ที่ รับเข้ามาให้เห็นชัดเจนขึ้น

```js
const showBranchData = (branchId) => ({
    type: ActionTypes.SHOW_BRANCH_DATA,
    payload: branchId
})
```

## 4. เช็ค Payload ของ Action ที่เกิดขึ้นใน Reducer

เปิดไฟล์ `src/redux/dashboard.reducer.js`

แล้วลอง log ตัว parameter `payload`

```js
case Actions.ActionTypes.SHOW_BRANCH_DATA: {
       
    console.log(`branchId: ${payload}`)

    return { ...state }
}
```

### ไฟล์เต็ม dashboard.reducer.js

```js

import Actions from "./actions";
import BranchModel from "../models/branchModel";

const initialState = {
    branches: BranchModel.branches
}

export default (state = initialState, { type, payload }) => {
    switch (type) {

    case Actions.ActionTypes.SHOW_BRANCH_DATA: {

        console.log(`branchId: ${payload}`)

        return { }
    }
        
    default:
        return state
    }
}
```

## 5. กำหนด state เริ่มต้นให้ chart

เปิดไฟล์ `src/redux/dashboard.reducer.js` 

และกำหนดค่าใน `initialState` สำหรับ **StatChart** component

```js
import Actions from "./actions";
import BranchModel from "../models/branchModel";

const initialState = {
    branches: BranchModel.branches,
    branchDataInChart: [['Month', 'Amount'], ['', 0]]
}
//...
```

เปิดไฟล์ `src/components/StatChart.js`

และเขียน `mapStateToProps` ให้ดึงค่า `state.branchDataInChart` มากำหนดให้ `props.chartData`

```js
const mapStateToProps = (state) => ({
    chartData: state.branchDataInChart
})
```

### ไฟล์เต็ม StatChart.js

```js
import React, { Component } from 'react'
import { Card } from 'antd';
import Chart from 'react-google-charts';
import { connect } from 'react-redux'

 class StatChart extends Component {
    render() {
        return (
            <div>
                <Card title="Chart" style={{
                    width: '100%'
                }}>
                    <Chart
                        width={'100%'}
                        height={'400px'}
                        chartType="Line"
                        loader={<div>Loading Chart</div>}
                        data={this.props.chartData}
                        options={{
                            hAxis: {
                                title: 'Month',
                            },
                            vAxis: {
                                title: 'Amount',
                            },
                        }}
                        rootProps={{ 'data-testid': '1' }}
                    />
                </Card>
            </div>
        )
    }
}

const mapStateToProps = (state) => ({
    chartData: state.branchDataInChart
})

const mapDispatchToProps = {
    
}

export default connect(mapStateToProps, mapDispatchToProps)(StatChart);
```

## 6. ค้นหาข้อมูล Branch และกำหนดค่าที่เหมาะสมให้กับ chart data

เปิดไฟล์ `src/redux/dashboard.reducer.js`

ในส่วนของ case  **Actions.ActionTypes.SHOW_BRANCH_DATA** เราจะค้นหา และกำหนดค่าให้ `state.branchDataInChart` ใหม่

```js
case Actions.ActionTypes.SHOW_BRANCH_DATA: {

            console.log(`branchId: ${payload}`)

            let selectingBranch = BranchModel.branches.find(branch => {
                return branch.id === payload;
            })

            if (selectingBranch) {
                let finalChartData = [];

                finalChartData.push(selectingBranch.chartData.chartField);

                let chartDatas = selectingBranch.chartData.datas;

                if (chartDatas && chartDatas.length > 0) {
                    chartDatas.forEach(data => {
                        finalChartData.push([data.month, data.amount]);
                    })
                } else {
                    finalChartData.push(['', 0]);
                }


                return {
                   ...state,
                    branchDataInChart: finalChartData
                }

            } else {
                return { ...state }
            }
        }
```

### ไฟล์เต็ม dashboard.reducer.js

```js
import Actions from "./actions";
import BranchModel from "../models/branchModel";

const initialState = {
    branches: BranchModel.branches,
    branchDataInChart: [['Month', 'Amount'], ['', 0]]
}

export default (state = initialState, { type, payload }) => {
    switch (type) {

        case Actions.ActionTypes.SHOW_BRANCH_DATA: {

            console.log(`branchId: ${payload}`)

            let selectingBranch = BranchModel.branches.find(branch => {
                return branch.id === payload;
            })

            if (selectingBranch) {
                let finalChartData = [];

                finalChartData.push(selectingBranch.chartData.chartField);

                let chartDatas = selectingBranch.chartData.datas;

                if (chartDatas && chartDatas.length > 0) {
                    chartDatas.forEach(data => {
                        finalChartData.push([data.month, data.amount]);
                    })
                } else {
                    finalChartData.push(['', 0]);
                }


                return {
                    ...state,
                    branchDataInChart: finalChartData
                }

            } else {
                return { ...state }
            }
        }

        default:
            return state
    }
}
```