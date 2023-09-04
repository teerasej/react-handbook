
# สร้าง OpenAI client 

ในโปรเจคนี้เราใช้ component จาก [react-bootstrap](https://react-bootstrap.netlify.app/) นะ

## Part 1: User Interface

1. โคลนโปรเจคด้วยคำสั่งด้านล่าง

```bash
git clone -b starter https://github.com/teerasej/nextflow-openai-react-redux-simple
cd nextflow-openai-react-redux-simple
npm install 
```

2. [สร้าง PromptInput](2-promptinput.md)
3. [สร้าง Chatroom](3-chatroom.md)
4. [สร้าง ChatMessage](4-message-box.md)

## Part 2: Redux

5. [สร้าง Redux store](5-store.md)
6. [สร้าง Reducer Slice](6-reducer.md)
7. [ดึงข้อมูลจาก Reducer มาใช้ด้วย `useSelector()`](7-show-chat-history.md)
8. [สร้างและเรียกใช้ Action](8-action.md)
9. [สร้าง Thunk เพื่อเรียกใช้ OpenAI API](9-openai.md)
