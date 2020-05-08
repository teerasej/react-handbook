
# เชื่อมต่อ StatChart เข้ากับ redux 

เปิดไฟล์ `src/components/StatChart.js`

เราจะทำการเชื่อมต่อ StatChart component เข้ากับ redux ด้วยวิธีการเดียวกับ MapBranch

```js
import React, { Component } from 'react'
import { Card } from 'antd';
import Chart from 'react-google-charts';

// เรียกใช้ connect function 
import { connect } from 'react-redux'


export class StatChart extends Component {

    render() {

        // สร้าง array ขึ้นมาเพื่อเป็นข้อมูลให้ Chart
        let finalChartData = [];

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
                        {/* เอา Array มาใช้กับ props ชื่อ data ของ Chart*/}
                        data={finalChartData}

                        rootProps={{ 'data-testid': '1' }}
                    />
                </Card>
            </div>
        )
    }
}

// ทำการกำหนดข้อมูลให้กับ this.props ผ่าน mapStateToProps 
const mapStateToProps = (state) => ({
    selectedBranch: state.selectedBranch
})

const mapDispatchToProps = {

}

// เชื่อมต่อ mapStateToProps และ mapDispatchToProps เข้ากับ StatChart component ผ่านฟังก์ชั่น connect
export default connect(mapStateToProps, mapDispatchToProps)(StatChart);

```