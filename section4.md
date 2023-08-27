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

## Markdown Notes App
Introducing the markdown notes app, this app already has some bones set up for you:
You can see that there's a list of notes that will be on the left,  you can create new notes  with the plus 
button.
On the right there's a markdown editor where you can toggle between preview and write mode. This editor is built by another team that made a package called react- mde markdown editor and we've simply implemented that package into our code.
On each one of these notes I can type new things into the note, I can switch back and forth between preview and write mode, and everything here is Markdown so when I bold something it will surround it with the double asterisks, when I go into preview mode it will render that as HTML and so we see a bolded paragraph.
As it stands this notes app has some very limited features; first of all we see that it's just calling our notes ‘note 1’, ‘note 2’, ‘note 3’ etc. We also have no way to delete any notes, and if I refresh this app all my notes disappear.

### First challenge: To understand the code base!
//////////// FILL IN

### Features to add:
<img width="1014" alt="Features to add" src="https://github.com/ewainy/learnReactScrimba/assets/77060368/44c9260b-98af-42a3-b165-b12633328dd4">

### Feature: Sync notes with Local Storage
MDN docs for reference:https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage

Essentially you'll be using `localStorage.getItem()` and `localStorage.setItem()` to interact with the local storage API.
When you're using getItem, you provide a ‘key’ and that key is like an object - a way to access the data that you saved (in our case I probably recommend calling this notes instead of key) and then when it's time to set the item in local storage, you will both access the key where you want to save this and the value that you want to save.
Now it's important to remember that value has to be a string when it is saved in the local storage, so if you have something that's a little more complex like an array or an object you'll need to use `json.stringify` to turn that array or object into a stringified version or rather a json version that can be saved in local storage, and when it comes time to pull that out of local storage and use it as regular JavaScript, you'll need to essentially reverse that by using `json.parse`
<img width="1015" alt="Using local storage" src="https://github.com/ewainy/learnReactScrimba/assets/77060368/c26eaf74-c353-4034-8e77-8e4bf0634558">

```jsx
import React from "react"
import Sidebar from "./components/Sidebar"
import Editor from "./components/Editor"
import { data } from "./data"
import Split from "react-split"
import {nanoid} from "nanoid"


export default function App() {
   /**
    * Challenge:
    * 1. Every time the `notes` array changes, save it
    *    in localStorage. You'll need to use JSON.stringify()
    *    to turn the array into a string to save in localStorage.
    * 2. When the app first loads, initialize the notes state
    *    with the notes saved in localStorage. You'll need to
    *    use JSON.parse() to turn the stringified array back
    *    into a real JS array.
    */
  
   const [notes, setNotes] = React.useState(/*Your code here*/ || [])
   const [currentNoteId, setCurrentNoteId] = React.useState(
       (notes[0] && notes[0].id) || ""
   )
  
   function createNewNote() {
       const newNote = {
           id: nanoid(),
           body: "# Type your markdown note's title here"
       }
       setNotes(prevNotes => [newNote, ...prevNotes])
       setCurrentNoteId(newNote.id)
   }

```
#### Challenge
1. Every time the `notes` array changes, save it  in localStorage. You'll need to use JSON.stringify()
 to turn the array into a string to save in localStorage.

2) When the app first loads, initialize the notes state with the notes saved in localStorage. You'll need to
use JSON.parse() to turn the stringified array back into a real JS array.

``` jsx
 const [notes, setNotes] = React.useState(
      2) JSON.parse(localStorage.getItem("notes")) || []
   )
   const [currentNoteId, setCurrentNoteId] = React.useState(
       (notes[0] && notes[0].id) || ""
   )
  
1)   React.useEffect(() => {
       localStorage.setItem("notes", JSON.stringify(notes))
   }, [notes])
  
   function createNewNote() {
       const newNote = {
           id: nanoid(),
           body: "# Type your markdown note's title here"
       }
       setNotes(prevNotes => [newNote, ...prevNotes])
       setCurrentNoteId(newNote.id)
   }

```

In order to interact with the local storage every time the notes array changes, we will want to set up a side effect in React with useEffect. We will provide a function for the first parameter, in the second parameter - the dependencies array, we want it to run every time the notes array changes. Every time the notes array changes, we want to use local storage.setitem, updating the notes key inside of local storage and we can use json.stringify to stringify the notes so that our array can essentially be turned into a string and successfully save to local storage.

Now that it's saved in local storage part 2 is for us to actually access that local storage, so that we can in a way preload our app with whatever notes we've previously saved in local storage. So we should initialize state by accessing local storage, getting the item with the key of notes and because this will come back as a stringified value, we need to use json.parse on the value that gets pulled in from local storage.

### Lazy State Initialization

Regarding how we're initializing our state when we're pulling from local storage like this
```jsx
   const [notes, setNotes] = React.useState(
       JSON.parse(localStorage.getItem("notes")) || []
   )

```


 Now of course whatever value we put inside of React useState here in the beginning is just an initial value.So really we know that it will only set this value when the app first loads, after it loads, React is going to maintain any changing state internally behind the scenes.
However there is something that we need to see and I can illustrate that by creating some new state. In this case it really doesn't matter what it is.

``` jsx
   const [state, setState] = React.useState(console.log("State initialization"))

```

I'll open up my console and when I hit refresh, we can see that we get that console log running ‘state initialization’ but now we want you to think about the nature of React and what happens when any other state in the app changes. 
For example if I add a new note - anytime state changes like adding a new note, the entire app component will get re-rendered, well if React is in charge of saving the initial state in the background, then it might seem intuitive that it's not going to run this line again. 
```jsx 
const [state, setState] = React.useState(console.log("State initialization"))

```
However if I maybe add a new note, we can see that it does run this line again and that's because all that's really happening is React is re-running the entire function in the background.
It is ignoring the state that is trying to re-initialize here, but if there is code such as running a console log - or in our example getting something from local storage, it's going to run that code again. 
Even if it doesn't use the value as it's new initial state, because it's maintaining that state elsewhere in the background.

Now for direct commands in JavaScript, this is not a big deal at all. For example, running this console log is so minuscule in the amount of time it takes for the browser to run this line that it's completely negligible. 
So even when we're making changes in our app UI,  you can see that it kept running that state initialization console log, but because it takes so little effort for the browser to run that code, it's not a big deal. 
However, something like localStorage. getItem is a bit more of an expensive call - or rather it takes more effort for the browser to dip into local storage and get something every single state change that happens in our app.
As such, React has implemented a way for us to really easily make it so that any expensive code that might be running inside of our state initialization can happen only one time and this is called `lazy state initialization` it sounds a lot more complicated than it is, 

Because in the end all I need to do is instead of providing a value, I can provide a function that returns the value.

So here if I provide a function that returns console log of state initialization:
```jsx
const [state, setState] = React.useState(
       function() {
           return console.log("State initialization")
       }
   )

```

and hit refresh you get the very first state initialization but then any changes I make don't rerun that code. 
In the end this might seem more verbose than we had, but if we make use of arrow functions and implicit returns, all I really need to do is put a set of parentheses and an arrow 
```jsx
 const [state, setState] = React.useState(() => console.log("State initialization"))

```



#### Challenge: Lazily initialize our `notes` state so it doesn't reach into localStorage on every single re-render of the App component

```jsx
export default function App() {


   const [notes, setNotes] = React.useState( 
() => JSON.parse(localStorage.getItem("notes")) || []  
   )  // All it needs for lazy initialization is an arrow function


   const [currentNoteId, setCurrentNoteId] = React.useState(
       (notes[0] && notes[0].id) || ""
   )
  
   React.useEffect(() => {
       localStorage.setItem("notes", JSON.stringify(notes))
   }, [notes])
  
   function createNewNote() {
       const newNote = {
           id: nanoid(),
           body: "# Type your markdown note's title here"
       }
       setNotes(prevNotes => [newNote, ...prevNotes])
       setCurrentNoteId(newNote.id)
   }

```








