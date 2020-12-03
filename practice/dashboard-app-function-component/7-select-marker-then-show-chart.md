
# สร้างกลไก Action: กดเลือกหมุดและแสดง Chart


## 1. ตรวจจับการคลิกเลือกหมุด marker

1. กำหนด `branch.id` เข้าไปใน Marker ในชื่อของ `branchId`
2. เพิ่ม function `markerClick` เข้าไปเป็น EventListener ของ Marker ทุกตัว

```js
const markerClick = (marker) => {
  let branchId = marker.get('branchId');
  let branchTitle = marker.get('title');

  console.log(`Selected branch: ${branchId} ${branchTitle}`);
}

const handleApiLoaded = (map, maps) => {

  //..

  branches.forEach(branch => {
    let marker = new maps.Marker({
      position: branch.position,
      map,
      title: branch.name,
      // กำหนด id ของสาขา
      branchId: branch.id
    });

    // กำหนด event listenter สำหรับการคลิกเลือก
    marker.addListener('click', () => { markerClick(marker) })

    
    bounds.extend(branch.position);
  });

  //..
}
```

### ไฟล์เต็ม MapBranch.js

```js
import React from 'react'
import GoogleMapReact from 'google-map-react';
import { useSelector } from 'react-redux'

export default function MapBranch({
    center = {
        lat: 13.7200452,
        lng: 100.5135078
    },
    zoom = 15
}) {


    const branches = useSelector(state => state.branches);
    console.log('braches: ' + branches);

    const markerClick = (marker) => {
        let branchId = marker.get('branchId');
        let branchTitle = marker.get('title');

        console.log(`Selected branch: ${branchId} ${branchTitle}`);
    }

    const handleApiLoaded = (map, maps) => {
        let bounds = new maps.LatLngBounds();

        branches.forEach(branch => {
            let marker = new maps.Marker({
                position: branch.position,
                map,
                title: branch.name,
                // กำหนด id ของสาขา
                branchId: branch.id
            });

            // กำหนด event listenter สำหรับการคลิกเลือก
            marker.addListener('click', () => { markerClick(marker) })

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

## 3. เรียกใช้ dispatch function จาก React Hook ชื่อ `useDispatch()`

เริ่มจาก 

```js
import { useSelector } from 'react-redux'
import { useDispatch } from 'react-redux'

// หรือ

import { useSelector, useDispatch } from 'react-redux'
```


```js
// เรียกใช้ชื่อ action ที่สร้างเตรียมเอาไว้
import Action from "../redux/action";

//..

const markerClick = (marker) => {
  let branchId = marker.get('branchId');
  let branchTitle = marker.get('title');

  console.log(`Selected branch: ${branchId} ${branchTitle}`);
  
  // กำหนด type ด้วยชื่อ action
  // และ dispatch object ตัวนี้ออกไป เรียกเจ้า object นี้ว่า action
  dispatch({ type: Action.SHOW_BRANCH_DATA, payload: branchId });
}
```


## 4. เช็ค Payload ของ Action ที่เกิดขึ้นใน Reducer

เปิดไฟล์ `src/redux/reducer.js`

แล้วลอง log ตัว parameter `payload`

```js
case Action.SHOW_BRANCH_DATA: {
       
    console.log(`branchId: ${payload}`)
    return { ...state }
}
```

### ไฟล์เต็ม dashboard.reducer.js

```js
import branchModel from "../models/branchModel"
import Action from './action' 

const initialState = {
    branches: branchModel.branches
}

export default (state = initialState, { type, payload }) => {
    switch (type) {

    case Action.SHOW_BRANCH_DATA:
        console.log(`branchId: ${payload}`)
        return { ...state }

    default:
        return state
    }
}

```

## 5. กำหนด state เริ่มต้นให้ chart

เปิดไฟล์ `src/redux/reducer.js` 

และกำหนดค่าใน `initialState` สำหรับ **StatChart** component

```js
import Action from "./action";
import BranchModel from "../models/branchModel";

const initialState = {
    branches: BranchModel.branches,
    branchDataInChart: [['Month', 'Amount'], ['', 0]]
}
//...
```

เปิดไฟล์ `src/components/StatChart.js`

เริ่มจากการ import React Hook ชื่อ `useSelector` เพื่อเข้าถึงข้อมูลจาก Redux store

```js
import { useSelector } from 'react-redux'
```

และเราจะเรียกค่า `branchDataInChart` มาจาก state 

```js
let branchDataChart = useSelector(state => state.branchDataInChart)
```

และเราจะนำตัวแปรนี้มาแสดงใน Chart component 

### ไฟล์เต็ม StatChart.js

```js
import React from 'react'
import { Card } from 'antd'
import Chart from "react-google-charts"
import { useSelector } from 'react-redux'

export default function StatChart() {

    let branchDataChart = useSelector(state => state.branchDataInChart)

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

                        // แทนที่ข้อมูลที่ได้จาก Redux store 
                        data={branchDataChart}
                        
                        rootProps={{ 'data-testid': '1' }}
                    />
            </Card>
        </div>
    )
}

```

## 6. ค้นหาข้อมูล Branch และกำหนดค่าที่เหมาะสมให้กับ chart data

เปิดไฟล์ `src/redux/reducer.js`

ในส่วนของ case  **Actions.ActionTypes.SHOW_BRANCH_DATA** เราจะค้นหา และกำหนดค่าให้ `state.branchDataInChart` ใหม่

```js
case Action.SHOW_BRANCH_DATA: {

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

### ไฟล์เต็ม reducer.js

```js
import branchModel from "../models/branchModel"
import Action from './action'


const initialState = {
    branches: branchModel.branches,
    branchDataInChart: [['Month', 'Amount'], ['', 0]]
}

export default (state = initialState, { type, payload }) => {
    switch (type) {

        case Action.SHOW_BRANCH_DATA: {

            console.log(`branchId: ${payload}`)

            let selectingBranch = state.branches.find(branch => {
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