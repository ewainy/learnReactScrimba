# Section 4: Practice, Practice, Practice

## Projects: 
This section is all about practicing and applying our newly learned skills. In this final section, we will work on 3 projects:
- Light/Dark Mode Toggle
- Markdown Apple Notes 
- Dice Game Tenzies
<img width="1011" alt="light:dark mode" src="https://github.com/ewainy/learnReactScrimba/assets/77060368/ef355d79-c1e3-44e2-8f71-75d74a515cfc">
<img width="1013" alt="Markdown Notes" src="https://github.com/ewainy/learnReactScrimba/assets/77060368/050f930e-ea07-4d18-9df5-faceac4be40b">
<img width="1014" alt="Tenzies" src="https://github.com/ewainy/learnReactScrimba/assets/77060368/cabb1170-9e4c-4cd9-ab53-b89fd0bdfbe2">

## Dark/light mode to the React Facts site

### Challenge: Add a dark/light mode to the React Facts site


Imagine you've been handed this code base [(Scrimba Lesson)](https://scrimba.com/learn/learnreact/warm-up-add-dark-light-modes-to-reactfacts-site-co5924409bb476cc78b0d818a) which includes
all the design elements for a light and dark mode already,
but the team needs you to add the functionality to it so
it can switch from light to dark mode when the toggle is
flipped.


#### Advice:
1. Spend a good amount of time looking through the existing
code base to make sure you understand how everything is
working
- a. Check the markup starting in index.js -> App.js
   -> Main.js and Navbar.js
- b. Follow the CSS classNames to the style.css file
   and make sure you understand how the styles should
   be applied
-   c. Look closely at the conditional classNames in
   the JSX to help you decide what kind of props
   the components need to receive


2. Think carefully about which components need state.
This will help you decide where to write the code
initializing state and how to pass that state to
the components that need access to it.
  - a. Also think about how state will need to change,
   and see if you can find the hint in the code as
   to how/where that should happen.

`App.js`
```jsx
import React from "react"
import Navbar from "./components/Navbar"
import Main from "./components/Main"

export default function App() {
    return (
        <div className="container">
            <Navbar />
            <Main />
        </div>
    )
}
```
`Main.js`
```jsx
import React from "react"

export default function Main(props) {
    return (
        <main className={props.darkMode ? "dark" : ""}>
            <h1 className="main--title">Fun facts about React</h1>
            <ul className="main--facts">
                <li>Was first released in 2013</li>
                <li>Was originally created by Jordan Walke</li>
                <li>Has well over 100K stars on GitHub</li>
                <li>Is maintained by Facebook</li>
                <li>Powers thousands of enterprise apps, including mobile apps</li>
            </ul>
        </main>
    )
}
```
`Navbar.js`
```jsx
import React from "react"

export default function Navbar(props) {
    return (
         <nav 
            className={props.darkMode ? "dark": ""}
        >
            <img 
                className="nav--logo_icon"
                src="./images/react-icon-small.png"
            />
            <h3 className="nav--logo_text">ReactFacts</h3>
            
            <div 
                className="toggler" 
            >
                <p className="toggler--light">Light</p>
                <div 
                    className="toggler--slider"
                    onClick={props.toggleDarkMode}
                >
                    <div className="toggler--slider--circle"></div>
                </div>
                <p className="toggler--dark">Dark</p>
            </div>
        </nav>
    )
}
```
Looking at Main & Navbar, we can see that they are expecting some props. We have both components expecting darkmode as props and we can see this changes the className of our styles, depending on whether dark mode is selected or not. The navbar is also expecting a toggleDarkMode function. 


Firstly, we should create our state on our parent, App.js. 
We can then pass our darkMode prop to both our components.
Nabar is also expecting toggleDarkMode, a function that will change our darkMode state.
To set our Dark Mode when toggle is clicked, we should look at the previous state and then return the opposite.

```jsx
import React from "react"
import Navbar from "./components/Navbar"
import Main from "./components/Main"

export default function App() {
    const [darkMode, setDarkMode] = React.useState(true) // creating our state with a boolean.
    
    function toggleDarkMode() {
        setDarkMode(prevMode => !prevMode)
    }
    
    return (
        <div className="container">
            <Navbar 
                darkMode={darkMode} 
                toggleDarkMode={toggleDarkMode}
            />
            <Main darkMode={darkMode} />
        </div>
    )
}
```


