
# สร้าง Copilot client 

ในโปรเจคนี้เราใช้ component จาก [react-bootstrap](https://react-bootstrap.netlify.app/) นะ

## Part 1: User Interface

### 1.1 เริ่มโปรเจค

เลือกเริ่มโปรเจคได้ 2 วิธีคือ 
A. โคลนโปรเจคมาไว้ในเครื่อง หรือ 
B. ใช้ Codespace

### A. โคลนโปรเจคมาไว้ในเครื่องด้วยคำสั่งด้านล่าง

```bash
git clone -b starter https://github.com/teerasej/nextflow-openai-react-redux-simple
```
จากนั้นเข้าไปในโฟลเดอร์โปรเจค
```bash
cd nextflow-openai-react-redux-simple
```
และติดตั้ง package ที่จำเป็นด้วยคำสั่ง
```bash
npm install 
```

### B. ใช้ Codespace

1. Fork โปรเจค https://github.com/teerasej/nextflow-openai-react-redux-simple **โดยให้ติ๊ก only on branch main ออก** เพราะต้องใช้ branch starter ในการทำแลป
2. ใน Repo ที่ได้มาให้เปลี่ยน branch เป็น `starter`
3. เปิด Codespace จาก Repo ที่ fork มา

### 1.2 Building Components 

2. [สร้าง User Interface: Prompt input](2-promptinput.md)
3. [สร้าง User Interface: Chat history](3-chatroom.md)
4. [สร้าง User Interface: ChatMessage](4-message-box.md)

## Part 2: Redux

5. [สร้าง Redux store](5-store.md)
6. [สร้าง Reducer Slice](6-reducer.md)
7. [ดึงข้อมูลจาก Reducer มาใช้ด้วย `useSelector()`](7-show-chat-history.md)
8. [สร้างและเรียกใช้ Action](8-action.md)
9. สร้าง Thunk เพื่อเรียกใช้ Async Operation [Azure OpenAI Service](9-azure-openai.md) | [OpenAI API](9-openai.md) 
10. (Optional) คุยต่อเนื่อง [Azure OpenAI service](10-azure-openai-conversation.md)
