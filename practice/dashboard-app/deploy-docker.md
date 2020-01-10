# Build และ Deploy ลง Docker

## 1. ติดตั้ง Docker for Desktop

1. จัดการสมัคร[สมาชิก Docker Hub ที่นี่](https://hub.docker.com/)
2. [ดาวน์โหลดและติดตั้ง Docker for Desktop](https://hub.docker.com/?overlay=onboarding)

## 2. ติดตั้ง Docker Extension สำหรับ Visual Studio Code

1. ถ้ายังไม่ได้ติดตั้ง Visual Studio Code ให้[ดาวน์โหลดตัวติดตั้งที่นี่](https://code.visualstudio.com/) และติดตั้งให้เรียบร้อย 
2. [คลิก Link นี้](https://marketplace.visualstudio.com/items?itemName=teerasej.nextflow-aws-docker-training-pack) และกดติดตั้ง

## 3. ดาวน์โหลดโปรเจคตัวอย่าง 

1. [ดาวน์โหลดโปรเจคจากที่นี่](https://www.dropbox.com/s/ifelitoz5awv73r/react-web-project-for-docker.zip?dl=0)
2. ทดสอบติดตั้ง node module และรันทดสอบการทำงาน

## 4. สร้างไฟล์ dockerfile

สร้าง `dockerfile` ไว้ในโปรเจค

```yml
# docker image ต้นแบบที่จะดึงมาใช้สร้าง image ของเรา
FROM node:12.10.0-alpine

# รันคำสั่ง สร้างโฟลเดอร์ /app
RUN mkdir /app

# ตั้ง working directory เป็น /app 
WORKDIR /app

# เพิ่ม `/app/node_modules/.bin` ให้กับ $PATH
ENV PATH /app/node_modules/.bin:$PATH

# คัดลอกไฟล์ package.json ในโปรเจค ไปไว้ในโฟลเดอร์ /app บน docker image
COPY package.json /app/package.json

# รันคำสั่งติดตั้ง node_module แบบเงียบ
RUN npm install --silent
RUN npm install react-scripts@3.0.1 -g --silent

# คัดลอกไฟล์ในโปรเจค ไปไว้ในโฟลเดอร์ /app บน docker image
COPY . /app

# รันคำสั่ง build 
RUN npm run build

# เปิดพอร์ท 3000
EXPOSE 3000

# start app
CMD ["npm", "start"]

```

### สร้างไฟล์ `.dockerignore`

สร้างไฟล์ `.docker_ignore` เพื่อไม่ให้ docker คัดลอกโฟลเดอร์​ `node_module` ไปไว้ใน image

```
node_modules
```

## 5. สร้างไฟล์ docker image 

1. คลิกขวาที่ไฟล์ **dockerfile** และเลือกคำสั่ง **build image...**
2. ตั้งชื่อให้ image เช่น **teerase/dashboardreact@latest**

## 6. รัน docker image เป็น container

รันคำสั่งเพื่อสร้าง container

```
docker run -p [host port]:[container port] --rm [image name]:[tag]

// เช่น 

docker run -p 3001:3000 --rm teerasej/dashboardreact:lastest

// หรือแบบด้านล่าง จะใช้ tag ชื่อ latest เสมอ

docker run -p 3001:3000 --rm teerasej/dashboardreact
```

- `-p 3001:3000` เปิด port 3001 ให้ host (คือเครื่องที่รัน docker container) และเปิด port 3000 ให้ container ตัวอื่น
- `--rm` ลบ container เมื่อสั่งหยุดทำงาน

