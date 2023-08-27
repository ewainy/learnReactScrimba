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

### Notes App: Note Summary Title

Right now in our code, you can see when we're mapping over the notes, we grab the index and we display note and then the index plus one.
 Unfortunately this doesn't give our users any idea as to the contents of that note, so your challenge is to try and figure out a way to display only the first line of the notes body property. By the way, note.body is the text that is showing up here in the UI editor and that first line should be pulled out and used as the summary that shows up here in our sidebar. 

#### Challenge: Display first line of note

```jsx
export default function Sidebar(props) {
   /**
    * Challenge: Try to figure out a way to display only the
    * first line of note.body as the note summary in the
    * sidebar.
    *
    * Hint 1: note.body has "invisible" newline characters
    * in the text every time there's a new line shown. E.g.
    * the text in Note 1 is:
    * "# Note summary\n\nBeginning of the note"
    *
    * Hint 2: See if you can split the string into an array
    * using the "\n" newline character as the divider
    */
  
   const noteElements = props.notes.map((note, index) => (
       <div key={note.id}>
           <div
              
               className={`title ${
                   note.id === props.currentNote.id ? "selected-note" : ""
               }`}
               onClick={() => props.setCurrentNoteId(note.id)}
           >
            //   <h4 className="text-snippet">Note {index + 1}</h4>  // this changes from this to (see below)
 <h4 className="text-snippet">{note.body.split("\n")[0]}</h4> // becomes this


           </div>
       </div>
   ))


   return (
       <section className="pane sidebar">
           <div className="sidebar--header">
               <h3>Notes</h3>
               <button className="new-note" onClick={props.newNote}>+</button>
           </div>
           {noteElements}
       </section>
   )
}

```
For this challenge, all we need to do is add some JavaScript in curly braces, we have access to our note.body from our map function, our note.body is our string. We will use
.split on the new line character\n, that will return an array, and we'll say at the index of 0, which should access the very first line of our note.


### Notes App: Bump recent note to the top

* Note, the update function below is a completely different approach than what is covered in the course *

A sensible feature to add to our app: if the user ever edits a note in the sidebar, it should pop up to the top of the list.

We have a function that exists that we can tap into whenever the user is editing a note and that's our function called updateNote.
Currently we're using the map method so that the array of notes stays in place in that array, in other words if they edit note 1 it will always stay in the third position because that's just how .map works. It returns a new array where every original item is at the same index in the new array as it was in the original array, so if we're wanting to rearrange this array there may be a way to use .map to do that; however this could be complex so an alternative approach is advisable.


```jsx
export default function App() {
   /**
    * Challenge: When the user edits a note, reposition
    * it in the list of notes to the top of the list
    */
   const [notes, setNotes] = React.useState(
       () => JSON.parse(localStorage.getItem("notes")) || []
   )
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
  
 /*  function updateNote(text) {
       // This does not rearrange the notes
       setNotes(oldNotes => oldNotes.map(oldNote => {
           return oldNote.id === currentNoteId
               ? { ...oldNote, body: text }
               : oldNote
       }))
       
   } */

```
New updateNote function:
```jsx
function updateNote(text) {
    setNotes((oldNotes) => {
        // Step 1: Find the note with the currentNoteId and create a copy with updated body
        const updatedNote = { ...oldNotes.find((note) => note.id === currentNoteId) };
        updatedNote.body = text;

        // Step 2: Remove the note with currentNoteId from the oldNotes array using filter
        const filteredNotes = oldNotes.filter((note) => note.id !== currentNoteId);

        // Step 3: Concatenate the updatedNote to the top of the filteredNotes array
        return [updatedNote, ...filteredNotes];
    });
}

```

This approach is designed to:

1. Find the note with the `currentNoteId` and create a copy of it with the updated `body` property.

2. Use the `filter` method to remove the note with the `currentNoteId` from the `oldNotes` array.

3. Concatenate the updated note with the filtered `oldNotes` array to ensure that the updated note is placed at the top.

Here's a breakdown of how this code works:

1. We use `setNotes` with a callback function to update the `notes` state based on the previous state (`oldNotes`).

2. Within the callback, we find the note with the `currentNoteId` in the `oldNotes` array and create a copy of it with the updated `body`.

3. We use the `filter` method to create a new array (`filteredNotes`) that excludes the note with the `currentNoteId`. This effectively removes the old version of the note from the array.

4. Finally, we return a new array where the `updatedNote` is placed at the beginning (top) followed by the `filteredNotes`. This arrangement ensures that the most recently updated note is at the top of the list.

This approach achieves the goal of updating the note and placing it at the top of the list while maintaining immutability.

### Notes App: Delete note

The very last feature we're going to add to our notes app is the ability to delete notes.
In the sidebar I added a button that has an icon inside and this icon has a trash icon that appears when you hover over one of the notes in the list. 
Right now clicking it doesn't do anything that's gonna be your challenge, however you can see that clicking the trash icon is also changing selected note from the sidebar and that caused me to run into a bug and so I started some of this code for you. We've got the function deleteNote that will receive an event and the note ID. I wrote this line ‘event.stop propagation’ without going in-depth. What it does is it says when this trash icon handles the click event stop propagating that click events to the parents, like the sidebar div that's holding this entire note. 
In other words, this trash icon is a child element of the note at the side next to it and so when I click the trash currently it's propagating that click event through to the parent which is also handling a click event now the fact that it is still propagating that should tell you that my trash icon is not correctly using this deleteNote click event.


#### Challenge: Complete and implement the deleteNote function
Hints:
- 1. What array method can be used to return a new array that has filtered out an item based on a condition?
- 2. Notice the parameters being passed to the function  and think about how both of those parameters
can be passed in during the onClick event handler


Before we work on the code here, let's test to make sure that we can successfully add the event listener to the trash icon button. I'm going to just add a console log that says deleted note and then maybe I'll add the note ID just to make sure that I'm successfully passing down the correct note.

```jsx
 function deleteNote(event, noteId) {
       event.stopPropagation()
       console.log("deleted note", noteId)
       // Your code here
   }
```
Then we need to pass this function down through props to our sidebar component. We'll add a new prop called deleteNote which will just be the deleteNote function.
```jsx
 <Sidebar
                   notes={notes}
                   currentNote={findCurrentNote()}
                   setCurrentNoteId={setCurrentNoteId}
                   newNote={createNewNote}
                   deleteNote={deleteNote}
               />
```

and then in the sidebar.js file we're already receiving props so I will add an on-click event handler here.
`sidebar.js`
```jsx
import React from "react"


export default function Sidebar(props) {
   const noteElements = props.notes.map((note, index) => (
       <div key={note.id}>
           <div
              
               className={`title ${
                   note.id === props.currentNote.id ? "selected-note" : ""
               }`}
               onClick={() => props.setCurrentNoteId(note.id)}
           >
               <h4 className="text-snippet">{note.body.split("\n")[0]}</h4>
               <button
                   className="delete-btn"
                   onClick={(event) => props.deleteNote(event, note.id)} // our onClick event handler -  
// See explanation below
               >
                   <i className="gg-trash trash-icon"></i>
               </button>
           </div>
       </div>
   ))


   return (
       <section className="pane sidebar">
           <div className="sidebar--header">
               <h3>Notes</h3>
               <button className="new-note" onClick={props.newNote}>+</button>
           </div>
           {noteElements}
       </section>
   )
}

```
Your first inclination might have been to just pass props.deleteNote here, however that's why I provided you the hint about what parameters we're passing to this function.
`By default, whatever function we pass to an event handler will receive the event as its parameter` and in order to pass something else along with that, I either need to do some kind of magic with .bind, which I'm not going to get into… or more simply I can say this is my function that I want to call, this entire anonymous function, and the first line or only line of that function will call deleteNote. That way I can take the event that I'm receiving as a part of the on-click callback function and pass it along to my deleteNote function but then I can also tell it to pass the notes ID. Remember we're here inside of .map, and we have access to this note variable which has an ID property and that's what we're passing down to deleteNote as we're calling it. 

`App.js`

So now let's go to ‘app.js’ and finish implementing the deleteNote function. We are going to use array.filter, but in our case we're only going to filter out the one item based on this noteID.

Since I'm updating my state, I'm going to call setNotes. I do need access to my old array of notes so I'll use the callback function with oldNotes as the parameter and I want to return a new array that results from calling oldNotes.filter. 
notes.filter takes a callback function and whatever we return from this callback function needs to be a boolean to indicate whether the current item we’re iterating over in the original array should be included in the new array or not.
So we'll say that for each note we're looking at, I want to make sure I include items whose ID property does not equal the note ID that we're trying to delete. 

So once again we're looking at all of our notes, we want to run the filter method, we're going to look at each note in that array and if the ID does not equal the one we're trying to delete then this will result in true. Which means I do want it included in the new array that I'm returning from  .filter which means essentially if it's not the one that I click delete on, it will leave it alone and continue to exist in the array.
However, for the one note that I did click delete on, note.ID will equal noteID and therefore this will be false and therefore that note that I click delete on will not be included in the array and thus it'll get removed from our state.

```jsx
function deleteNote(event, noteId) {
       event.stopPropagation()
       setNotes(oldNotes => oldNotes.filter(note => note.id !== noteId))
   }
```

``` jsx
import React from "react"
import Sidebar from "./components/Sidebar"
import Editor from "./components/Editor"
import { data } from "./data"
import Split from "react-split"
import {nanoid} from "nanoid"


export default function App() {
   const [notes, setNotes] = React.useState(
       () => JSON.parse(localStorage.getItem("notes")) || []
   )
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
  
function updateNote(text) {
    setNotes((oldNotes) => {
        // Step 1: Find the note with the currentNoteId and create a copy with updated body
        const updatedNote = { ...oldNotes.find((note) => note.id === currentNoteId) };
        updatedNote.body = text;

        // Step 2: Remove the note with currentNoteId from the oldNotes array using filter
        const filteredNotes = oldNotes.filter((note) => note.id !== currentNoteId);

        // Step 3: Concatenate the updatedNote to the top of the filteredNotes array
        return [updatedNote, ...filteredNotes];
    });
}
  
   function deleteNote(event, noteId) {
       event.stopPropagation()
       setNotes(oldNotes => oldNotes.filter(note => note.id !== noteId))
   }
  
   function findCurrentNote() {
       return notes.find(note => {
           return note.id === currentNoteId
       }) || notes[0]
   }
  
   return (
       <main>
       {
           notes.length > 0
           ?
           <Split
               sizes={[30, 70]}
               direction="horizontal"
               className="split"
           >
               <Sidebar
                   notes={notes}
                   currentNote={findCurrentNote()}
                   setCurrentNoteId={setCurrentNoteId}
                   newNote={createNewNote}
                   deleteNote={deleteNote}
               />
               {
                   currentNoteId &&
                   notes.length > 0 &&
                   <Editor
                       currentNote={findCurrentNote()}
                       updateNote={updateNote}
                   />
               }
           </Split>
           :
           <div className="no-notes">
               <h1>You have no notes</h1>
               <button
                   className="first-note"
                   onClick={createNewNote}
               >
                   Create one now
               </button>
           </div>
          
       }
       </main>
   )
}



```

#### Reminder: Callback function + extra parameters than event
The tricky thing that may have tripped some people up was adding this callback function that calls props.deleteNote, but as a quick reminder if you ever really need extra parameters other than just the event in your callback function then you're probably going to pass a whole callback function here instead of just calling props that delete note so that you can pass whatever parameters you want to your own function.





