
# การจัดการ Event ของ Component 

เราสามารถใช้ function ผูกเข้ากับ **Event props** ของ JSX ได้โดยตรง

## ไฟล์เริ่มต้น

```jsx

        const HelloFunction = (props) => {

            const sayHi = () => {
                console.log('Hi')
            }

            return (
                <div>
                    <button onClick={sayHi}>Hi</button>
                </div>
            )
        }

        class HelloClass extends React.Component {
           
            sayHi = () => {
                console.log('Hi')
            }

            render() {
                return (
                    <div>
                        <button onClick={sayHi}>Hi</button>
                    </div>
                )
            }
        }


        ReactDOM.render(
            (
                <div>
                    <HelloClass/>
                    <HelloFunction/>
                </div>
            ),
            document.getElementById("root")
        );

```
