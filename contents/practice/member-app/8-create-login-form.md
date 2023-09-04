
# สร้างแบบฟอร์ม ในหน้า Login 

- [Button](https://material-ui.com/components/buttons/)
- [TextField](https://material-ui.com/components/text-fields/)

```jsx

import React from 'react'
// import component มาจาก material-ui
import { Button, TextField } from '@material-ui/core'

export default function LoginPage() {
    return (

        <div>
            <h1>Login: </h1>
            <form>
                <div>
                    <TextField
                        id="email"
                        name="email"
                        label="Email"
                    />
                </div>
                <div>
                    <TextField
                        id="password"
                        name="password"
                        label="Password"
                        type="password"
                    />
                </div>
                <div>
                    <Button variant="contained" type="submit">
                        Sign in
                    </Button>
                </div>
                <div>
                    <Button>
                        Create Account?
                    </Button>
                </div>
            </form>
        </div>
    )
}

```