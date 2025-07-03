
# การ Publish โปรเจคไปใช้งาน 

1. รันคำสั่ง
```
npm run build
```
4. เราจะได้โฟลเดอร์ชื่อ **build** ซึ่งมีไฟล์ต่างๆ อยู่ด้านใน เช่น `index.html`
5. นำไฟล์ทั้งหมดไปวางใน web server ที่ต้องการ

> การเปิดไฟล์ index.html โดยตรงอาจจะทำให้ไม่สามารถโหลดไฟล์บางตัวในโปรเจค ทำให้ application ไม่สามารถทำงานได้อย่างสมบูรณ์ 

ดูเพิ่มเติมในส่วน [Create-react-app: Deployment](https://create-react-app.dev/docs/deployment/)

## ถ้าอยากทดสอบรันจริงๆตอนนี้...

รันคำสั่งด้านล่าง เพื่อติดตั้ง package ที่ชื่อว่า `serve` ซึ่งเป็น static file server 

```bash
npm install -g serve
```

จากนั้น run server ด้วยไฟล์ที่อยู่ใน folder `build` ได้ด้วยคำสั่ง 

```bash
serve -s build
```

## เพิ่มเติม

- [การ deploy โปรเจค create-react-app ไปไว้ในที่ที่ไม่ใช่ Web root](https://nextflow.in.th/2020/react-deploy-with-relative-path-create-react-app/)
- [Deploy create-react-app ไปที่ Microsoft IIS (English)](https://dev.to/sumitkharche/how-to-deploy-react-application-on-iis-server-1ied)
