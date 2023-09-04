
# การ Publish โปรเจคไปใช้งาน 

1. รันคำสั่ง `npm run build` 
2. เราจะได้โฟลเดอร์ชื่อ **build** ซึ่งมีไฟล์ต่างๆ อยู่ด้านใน เช่น `index.html`
3. นำไฟล์ทั้งหมดไปวางใน web server ที่ต้องการ

> การเปิดไฟล์ index.html โดยตรงอาจจะทำให้ไม่สามารถโหลดไฟล์บางตัวในโปรเจค ทำให้ application ไม่สามารถทำงานได้อย่างสมบูรณ์ 

ดูเพิ่มเติมในส่วน [Create-react-app: Deployment](https://create-react-app.dev/docs/deployment/)

## ถ้าอยากทดสอบรันจริงๆตอนนี้...

รัน server ง่ายๆ ได้ด้วยคำสั่ง 

```bash
serve -s build
```

## เพิ่มเติม

- [การ deploy โปรเจค create-react-app ไปไว้ในที่ที่ไม่ใช่ Web root](https://nextflow.in.th/2020/react-deploy-with-relative-path-create-react-app/)
