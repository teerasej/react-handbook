
# การจัดการ Event ของ Component 

เราสามารถใช้ method ของ Class ผูกเข้ากับ event attribute ของ HTML ได้โดยตรง

## ไฟล์เริ่มต้น

```js
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <script src="https:/unpkg.com/react@16.8.0/umd/react.development.js"></script>
    <script src="https:/unpkg.com/react-dom@16.8.0/umd/react-dom.development.js"></script>
    <script src="https:/unpkg.com/@babel/standalone/babel.min.js"></script>
    <title>Nextflow React Playground</title>
</head>

<body>

    <style>
        #header {
            font-size: 30px;
        }

        .red {
            color: red;
        }
    </style>

    <div id="root"></div>
    <script type="text/babel">

        let city = 'Bangkok';

        const Hello = (props) => {
            return (
                <div className="red">
                    <h1>Hello {props.library}</h1>
                    <p>{props.message}</p>
                </div>
            )
        }

        const Food = ({ name }) => <h2>{name}</h2>;

        class App extends React.Component {
            state = {
                loggedIn: false
            }

            // declare logged in function


            // declare logged out function 


            render() {

                let userLogInStatus;

                if(this.state.loggedIn === true){
                    userLogInStatus = <h1>User is logged in</h1>;
                } else {
                    userLogInStatus = <p>Please log in before use</p>;
                }

                return (
                    <div>
                        {userLogInStatus}
                        <Food name="Tom Yam Goong" />
                        <Food name="Khao Pad Sapparod" />
                    </div>
                )
            }
        }


        ReactDOM.render(
            (
                <div>
                    <App />
                    <Hello library="Angular" message="Yep!" />
                </div>
            ),
            document.getElementById("root")
        );

    </script>
</body>

</html>
```

## ตัวอย่าง แบบใช้ Arrow Function

```html
<script type="text/babel">

        const Food = ({name}) => <h2>{name}</h2>;
    
        class App extends React.Component {
    
            state = {
                loggedIn: false
            }
    
            logIn = () => {
                this.setState({ loggedIn: true});
            }
    
            logOut = () => {
                this.setState({ loggedIn: false});
            }
    
            render() {
                return (
                    <div>
                        <button onClick={this.logIn}>Log In</button>
                        <button onClick={this.logOut}>Log Out</button>
                        <div>User is {this.state.loggedIn ? "logged in" : "not logged in"}</div>
                        <Food name="Tom Yam Goong"/>
                        <Food name ="Khao Pad Sapparod"/>
                    </div>
                )
            }
        }
    
        ReactDOM.render(
            <App />,
            document.getElementById("root")
        );
    
    </script>
```

### คำอธิบาย 

สังเกตว่าใน Component Class เราประกาศ method ขึ้นมา 2 method 

```js
logIn = () => {
    this.setState({ loggedIn: true});
}

logOut = () => {
    this.setState({ loggedIn: false});
}
```

คำสั่ง `this.setState({})` จะเป็นการกำหนดค่าให้กับ property `state` ใหม่ และทำให้ React ทำการ render component ตัวนั้นอีกครั้ง

### การผูก method กับ HTML Event (Event Binding)

เราสามารถกำหนด method ให้กับ event attribute ของ HTML ทั่วไปได้เลย

- สังเกตว่า `onClick={}` ไม่ใช่ `onClick="{}"`
- เรียกชื่อได้โดยตรง 

```html
<button onClick={this.logIn}>Log In</button>
```

## ตัวอย่างแบบใช้ Method ปกติ

ในกรณีที่เราไม่อยากประกาศ method แบบ Arrow function เช่น

```js
// ใช้แบบนี้
logIn() {
    this.setState({ loggedIn: true});
}

// แทนที่จะเป็นแบบนี้
logIn = () => {
    this.setState({ loggedIn: true});
} 
```

ตอน binding ใน JSX เราต้องใช้ Arrow function ร่วมกันด้วย เช่น 

```html
<button onClick={e => this.logIn()}>Log In</button>
```

ตัวอย่างแบบเต็ม

```html
<script type="text/babel">

        const Food = ({name}) => <h2>{name}</h2>;
    
        class App extends React.Component {
    
            state = {
                loggedIn: false
            }
    
            logIn() {
                this.setState({ loggedIn: true});
            }
    
            logOut() {
                this.setState({ loggedIn: false});
            }
    
            render() {
                return (
                    <div>
                        <button onClick={e => this.logIn()}>Log In</button>
                        <button onClick={e => this.logOut()}>Log Out</button>
                        <div>User is {this.state.loggedIn ? "logged in" : "not logged in"}</div>
                        <Food name="Tom Yam Goong"/>
                        <Food name ="Khao Pad Sapparod"/>
                    </div>
                )
            }
        }
    
        ReactDOM.render(
            <App />,
            document.getElementById("root")
        );
    
    </script>
```
