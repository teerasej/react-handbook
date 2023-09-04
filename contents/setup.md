
# Setup your machine 

ให้ทำตามครบทุกขั้นตอนเพื่อให้แน่ใจว่า ระบบและซอฟต์แวร์สามารถทำงานได้อย่างไม่มีปัญหา **และควรทำล่วงหน้าก่อนวันอบรมสักอย่างน้อย 4-5 วัน เพราะถ้าพบว่าเครื่องของตัวเองถูก block หรือจำกัดสิทธิ์ จะได้ทำเรื่องกับฝ่าย Security หรือ Admin เพื่อดำเนินการให้สามารถติดตั้งและใช้งานได้ตามปกติ**

## ⚠️ ข้อควรระวัง เรื่องการใช้เครื่องคอมขององค์กร มาใช้ในการอบรม

- **แนะนำว่าสำหรับองค์กรที่มีการ block การทำงานผ่านอินเตอร์เน็ต เช่นการ block port หรือ certificate ให้เตรียม Account สำหรับเชื่อมต่ออินเตอร์เน็ตวงนอกให้ผู้เข้าอบรมได้เลย**
- การอบรมจะมีการรันคำสั่งผ่าน โปรแกรม Command Prompt หรือ Terminal และมีโปรเซสที่ทำการสร้างและแก้ไขไฟล์ที่อยู่บนระบบปฏิบัติการ (เช่น NodeJS process) ให้แน่ใจว่าระบบปฏิบัติการไม่ได้มีการบล๊อคการทำงาน และสามารถทำงานได้อย่างไม่มีปัญหา
- ระหว่างการอบรม จะมีการเชื่อมต่ออินเตอร์เน็ต และมีการดาวน์โหลดโค้ด รวมถึงติดตั้ง, setup, และ install โปรแกรมเพิ่มเติม **ควรให้แน่ใจว่าคอมพิวเตอร์ทีี่ใช้ไม่มีการปิดกั้นการทำงานดังกล่าว**
- หากผู้เข้าอบรมเลือกใช้วิธีการดาวน์โหลด NodeJS มาเป็นไฟล์ zip และติดตั้งด้วยตัวเอง ให้แน่ใจว่า:
     - ได้ทำการตั้งค่า PATH ให้กับ NodeJS ใน System Environment หรือ zsh ได้เรียบร้อย
     - สามารถสร้างโปรเจคโดยใช้คำสั่ง `npx create-react-app my-app` ใน Command prompt, Visual Studio Code หรือ Terminal ได้ออย่างไม่มีปัญหา

> ⚠️⚠️⚠️ หากทางผู้ดูแลระบบไม่ได้มีการอำนวยความสะดวกในการผ่อนปรนตามสถานการณ์ด้านบน ทางผู้เข้าอบรมอาจจะไม่สามารถใช้คอมพิวเตอร์นั้นเรียนรู้ หรือทำ workshop บางส่วนได้ จึงขอแจ้งมาเพื่อทราบครับ

## สำหรับ Computer

ทำการดาวน์โหลดตัวติดตั้ง และดำเนินการติดตั้งให้เรียบร้อย

1. Node Runtime รุ่น LTS ([Download](https://nodejs.org/en/download) | [คลิปแนะนำการติดตั้ง](https://www.youtube.com/watch?v=GET7GPha6gM))
2. Git Client ([Download](http://git-scm.com/download/) | [คลิปแนะนำการติดตั้ง](https://www.youtube.com/watch?v=fPOoIZbDKmE))
3. Visual Studio Code ([Download](https://code.visualstudio.com/))
   - เสร็จแล้วติดตั้งส่วนเสริม 
     1. [Nextflow React Native Extension Pack](https://marketplace.visualstudio.com/items?itemName=teerasej.nextflow-react-native-pack) 
4. Web Browser สามารถเลือกระหว่าง 2 อย่างนี้ 
   1. Google Chrome ([Download](https://www.google.com/chrome/))
   2. Microsoft Edge ([Download](https://www.microsoft.com/en-us/edge/download))
5. แล้วก็ติดตั้ง Extension บน Web Browser ตามรายการต่อไปนี้ 
   1. [React Developer Tools](https://chrome.google.com/webstore/detail/react-developer-tools)
   2. [Redux Dev Tools](https://chrome.google.com/webstore/detail/redux-devtools)



