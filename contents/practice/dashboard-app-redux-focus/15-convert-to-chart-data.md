
# แปลงข้อมูลให้อยู่ในรูปแบบที่ Chart เอาไปใช้งานได้ 

เปิดไฟล์ `src/components/StatChart.js`

เนื่องจาก Google Chart component รับข้อมูลในรูปแบบเฉพาะของมันเอง [ลองดูตัวอย่างได้ที่นี่](https://developers.google.com/chart/interactive/docs/gallery/linechart)

เราจึงต้องเอาข้อมูลจาก this.props.selectedBranch มาแปลงนิดหน่อย ก่อนเอาไปใช้จริง 

```js
import React, { Component } from 'react'
import { Card } from 'antd';
import Chart from 'react-google-charts';

import { connect } from 'react-redux'


export class StatChart extends Component {

    render() {

        let finalChartData = [];

        // ถ้ามีข้อมูลจาก this.props.selectedBranch ให้ทำการแปลงข้อมูล
        if (this.props.selectedBranch) {

            let branch = this.props.selectedBranch;

            console.log('Selected Branch:', branch);

            finalChartData.push(branch.chartData.chartField);

            let chartDatas = branch.chartData.datas;

            if (chartDatas && chartDatas.length > 0) {
                chartDatas.forEach(data => {
                    finalChartData.push([data.month, data.amount]);
                })
            } else {
                finalChartData.push(['', 0]);
            }
        }



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
                        data={finalChartData}

                        rootProps={{ 'data-testid': '1' }}
                    />
                </Card>
            </div>
        )
    }
}

const mapStateToProps = (state) => ({
    selectedBranch: state.selectedBranch
})

const mapDispatchToProps = {

}

export default connect(mapStateToProps, mapDispatchToProps)(StatChart);

```