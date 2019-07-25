
# Debugging React with VSCode

1. ติดตั้ง Extension ชื่อ [Debugger for Chrome](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome)
2. กลับมาที่ VSCode และเปิดส่วนของ Debug และคลิกเพื่อสร้าง Configuration ใหม่

![2019-07-25_23-40-14](https://user-images.githubusercontent.com/85179/61892322-bbc6b480-af35-11e9-88a0-9184d00df959.png)

3. จากรายการ เลือกโปรไฟล์เป็น **Chrome** 

![2019-07-25_23-44-23](https://user-images.githubusercontent.com/85179/61892544-38599300-af36-11e9-9c42-08b2f533aac0.png)


4. ทำการแก้ไข เลข IP และ Port ที่ต้องการเปิดใช้งานให้ เรียบร้อย 

    เช่น ในที่นี้แอพเรารันใช้งานที่ `localhost:3000`

```js
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "type": "chrome",
            "request": "launch",
            "name": "Launch Chrome against localhost",
            "url": "http://localhost:3000",
            "webRoot": "${workspaceFolder}"
        }
    ]
}
```

5. เสร็จเรียบร้อยแล้ว ให้แน่ใจว่าได้รันแอพพลิเคชั่นตามที่กำหนดไว้ใน configuration จริง 
6. กดปุ่ม Run Application ในส่วน Debug 
7. วาง Breakpoint ให้ Code ได้ตามสะดวก และทดสอบใช้งาน