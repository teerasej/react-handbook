
# สร้าง OpenAI client 

ในโปรเจคนี้เราใช้ component จาก [react-bootstrap](https://react-bootstrap.netlify.app/) นะ

## Part 1

1. โคลนโปรเจคด้วยคำสั่งด้านล่าง

```bash
git clone -b starter https://github.com/teerasej/nextflow-openai-react-redux-simple
cd nextflow-openai-react-redux-simple
npm install 
```

2. [สร้าง PromptInput](2-promptinput.md)
3. [สร้าง Chatroom](3-chatroom.md)
4. [สร้าง ChatMessage](4-message-box.md)
5. [สร้าง Redux store](5-store.md)
6. [สร้างและเรียกใช้ Reducer](6-reducer.md)
7. [สร้างและเรียกใช้ Action](7-action.md)

## Part 2

ถ้าต้องการเริ่มจาก template สามารถ clone จาก branch ด้านล่างได้ 

```bash
git clone -b finish-redux https://github.com/teerasej/nextflow-openai-react-redux-simple
cd nextflow-openai-react-redux-simple
npm install 
```

8. [ใช้งาน react-hook-form](8-form.md)
9. [เรียกใช้ OpenAI API](9-openai.md)