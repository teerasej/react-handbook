

# Practice: Branch Dashboard, Redux focused

![2019-07-30_02-16-58](https://user-images.githubusercontent.com/85179/62075745-1e8ab980-b270-11e9-8b26-ce54d4ceeb89.png)

- [ดาวน์โหลด Starter Project](https://www.dropbox.com/s/6756txcy4h3umrz/branch-dashboard-starter.zip?dl=0)
- [ดาวน์โหลด Finish Project](https://www.dropbox.com/s/icx9b008g07psll/branch-dashboard-finished.zip?dl=0)

## Setup Redux ขึ้นมาในโปรเจค

1. [ดาวน์โหลดโปรเจค](1-setup.md) 
2. [สร้าง Actions](2-create-actions.md)
3. [สร้าง Reducer](3-create-reducer.md)
4. [สร้าง Redux Store และเชื่อมกับ Reducer](4-create-store.md)

## เอา Redux มาใช้กับ Component แผนที่เพื่อแสดงสาขา

5. [ติดตั้ง Redux Store เข้ากับ Component](5-setup-redux.md)
6. [เชื่อมต่อ MapBranch Component เข้ากับ Redux](6-connect-map-branch.md)
7. [สร้าง Action Method ไว้ใช้ใน MapBranch](7-create-action-function.md)
8. [ใช้ Action Method ใน MapBranch ผ่าน `mapStateToDispatch`](8-use-action-method-in-mapbranch.md)
9.  [จัดการ Payload ของ Action ใน Reducer](9-manage-payload-in-reducer.md)
10. [จัดการ state object ใน Component ผ่าน `mapStatetoProps`](10-state-allBranches.md)

## เอา Redux มาใช้กับแผนที่ และกราฟ เพื่อแสดงข้อมูลเฉพาะของสาขานั้น

11. [Action Method สำหรับส่งข้อมูลสาขาที่เลือกเข้า Reducer](11-create-action-method-for-select-branch.md)
12. [ใช้งาน Action method ใน MapBranch](12-use-action-method-in-map-branch.md) 
13. [reducer เอาข้อมูลสาขาที่ถูกเลือก อัพเดตใน state object](13-manage-branch-in-reducer.js) 
14. [เชื่อมต่อ StatChart เข้า Redux และใช้งาน state object](14-connect-StatChart.md)
15. [แปลงข้อมูลจาก state object เพื่อใช้งานใน Google Chart](15-convert-to-chart-data.md)