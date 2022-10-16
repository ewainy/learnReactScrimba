# learningReact
Re-learning React, following the course on Scrimba "Learn React for Free" by Bob Ziroll. This course teaches React 17.


## Static Pages in React

### Why React? 
- **It's composable**
    - In the old days of web development, a single page on a website was usually a single html page. A lot of the time those web pages would be 1000s and 1000s of lines long. With React, we can create our own custom components (such as a navigation bar) in their own files and piece them together to create something larger. Like lego bricks. This means that our code becomes a lot more **maintainable** and **flexible**.
    - Example of component: 
    ```jsx
    function MainContent() {
    return (
        <h1>"I'm learning React!"</h1>
    )}
    
    ReactDOM.render(
    <div>
        <Navbar />
        <MainContent />
    </div>,
    document.getElementById("root"))
    ```
- **It's declarative**
    - The opposite of declarative is imperative. When a program is declarative, we can simply tell it what should be done. "Just tell me what to do and I'll worry about how I get it done"
    - It's opposite, imperative is where we tell it *how* it should be done. "Describe to me every step on how to do something, and I will do it"
    - **Declarative in React:**
    ```jsx
    ReactDOM.render(<h1>Hello, React!</h1>, document.getElementById("root"))
    ```````
    - **Imperative in Vanilla JavaScript:**
    ```javascript
   
    let header = document.createElement('h1')
    header.className = 'header'
    header.textContent = 'Hello Vanilla JavaScript'
    document.getElementById('root').appendChild(header)
    ```
 ### Understanding React & JSX
Bob Ziroll covers this in his course. JSX returns plain JavaScript objects, with ReactDOM render's job to take React Elements and interpret them in a way that turns them into real DOM elements that the browser can understand.

but these resources are helpful to include:
- [Introducing JSX - React Docs](https://reactjs.org/docs/introducing-jsx.html)
- [React.js Basics – The DOM, Components, and Declarative Views Explained - freeCodeCamp](https://www.freecodecamp.org/news/reactjs-basics-dom-components-declarative-views/)

### React 18
React 18 brought some new changes, one being how it renders it's root element. ReactDOM.render is no longer supported in React 18. Use createRoot instead.

- **React version <18**
``` jsx
import React from "react"
import ReactDOM from "react-dom"

const navbar = (
    <nav>
        <h1>Emma's Website</h1>
        <ul>
            <li>Menu</li>
            <li>About</li>
            <li>Contact</li>
        </ul>
    </nav>
)

ReactDOM.render(navbar, document.getElementById("root"))
```
- **React version 18**
```jsx
import React from "react"
import ReactDOM from "react-dom/client" // new change

const navbar = (
    <nav>
        <h1>Emma's Website</h1>
        <ul>
            <li>Menu</li>
            <li>About</li>
            <li>Contact</li>
        </ul>
    </nav>
)



// ReactDOM.render(navbar, document.getElementById("root")) // Old way
ReactDOM.createRoot(document.getElementById("root")).render(navbar) // New way 1 option, 1 line
//
const root = ReactDOM.createRoot(document.getElementById("root")) // New way - another option, seperate
root.render(navbar)

```

## Static Page in React - Part 1 - Markup
<img width="600px" alt="Scrimba slide of website to create" src="https://user-images.githubusercontent.com/77060368/195993009-7d08aed2-067b-4efb-bd33-dc580308e7ff.png" height="400px">


```jsx
/*
Challenge: Starting from scratch, build and render the 
HTML for our section project. Check the Google slide for 
what you're trying to build.

We'll be adding styling to it later.

Hints:
* The React logo is a file in the project tree, so you can
  access it by using `src="./react-logo.png" in your image
  element
* You can also set the `width` attribute of the image element
  just like in HTML. In the slide, I have it set to 40px
 */

import React from 'react'
import ReactDOM from 'react-dom'

const page () {
    return (
    <div>
    <img src ="./react-logo.png" width="40px"></img>
    <h1>Fun Facts about React</h1>
    <ul>
        <li>Was first released in 2013</li>
        <li>Was originally created by Jordan Walke</li>
        <li>Has well over 100K stars on GitHub</li>
        <li>Is maintained by Facebook</li>
        <li>Powers thousands of enterprise apps, including mobile apps</li>
    </ul>
    </div>
  
    )
}

ReactDOM.render(
    page, document.getElementById('root')
)
```

## Pop Quiz 

1. Why do we need to `import React from "react"` in our files?
React is what defines JSX

2. If I were to console.log(page) in index.js, what would show up?
A JavaScript object. React elements that describe what React should
eventually add to the real DOM for us.

3. What's wrong with this code:
```
const page = (
    <h1>Hello</h1>
    <p>This is my website!</p>
)
```
We need our JSX to be nested under a single parent element

4. What does it mean for something to be "declarative" instead of "imperative"?
Declarative means I can tell the computer WHAT to do 
and expect it to handle the details. Imperative means I need
to tell it HOW to do each step.

5. What does it mean for something to be "composable"?
We have small pieces that we can put together to make something
larger/greater than the individual pieces.

## Vite Setup for React
Pronounced 'Veet' is French for fast/ quick and is next generation frontend tooling. This resource [Create React App with Vite](https://scrimba.com/articles/create-react-app-with-vite/) covers setting up with Vite and the difference between Vite and the standard CRA (create-react-app) 

<img width="407" alt="Vite setup" src="https://user-images.githubusercontent.com/77060368/195995293-2e025792-437e-4f7d-8531-27af24ccdefd.png">

## Project Overview
<img width="481" alt="Figma design for React page" src="https://user-images.githubusercontent.com/77060368/196040147-8f389fc5-d7e5-4e5a-91d5-808eed921242.png">

Create a static React page based on this Figma design:https://www.figma.com/file/xA1rJVQOorqMW6xjGdBLcI/ReactFacts

## Project - Navbar & Styling
### React Navbar Component `components/Navbar.js`
```jsx
import React from "react"

export default function Navbar() {
    return ( 
    <header>
        <nav className = 'main-nav'>
            <img className = 'logo' alt= 'React logo' src='images/
                react-icon-small.png'/>
             <h3 className = 'logo-text'>ReactFacts</h3>
             <h4 className = 'nav-title'>React Course - Project 1</h4>
        </nav>
    </header>
    )
}
```
### React Navbar Styles `style.css`
```css
* {
    box-sizing: border-box;
}

body {
margin: 0;
font-family: 'Inter';
}

.main-nav {
display: flex;
align-items: center;
width: 550px;
height: 91px;
background: #21222A;
}

.logo {
width: 28.93px;
height: 28.93px;
margin-left: 30px;
margin-right: 5px;
}

.logo-text {
margin-right: auto;
font-weight: 700;
font-size: 22px;
color: #61DAFB;
}

.nav-title {
font-weight: 600;
font-size: 16px;
text-align: right;
margin-right: 30px;
color: #DEEBF8;
}
```
## Project - Main & Styling
### React Main Component `components/Main.js`
```jsx
/**
Challenge: Build the main section!

Skip 2 aspects of the design for now:
1. The colored bullets in the list
2. The larger React logo on the side

Those will be separate challenges coming up.
*/

import React from "react"

export default function Main() {
    return (
      return (
        <div className = 'main-div'>
            <h1 className = 'main-title'>Fun Facts about React</h1>
            <ul className = 'main-facts'>
                <li>Was first released in 2013</li>
                <li>Was originally created by Jordan Walke</li>
                <li>Has well over 100K stars on GitHub</li>
                <li>Is maintained by Facebook</li>
                <li>Powers thousands of enterprise apps, including mobile apps</li>
            </ul>
        </div>
    )
}
}
```
### React Main Styling

```css
body {
    margin: 0;
    font-family: Inter, sans-serif;
    background-color: #282D35;
}

.main-div {
 /* background-color: #282D35; // This doesnt take up the whole section, so will apply to body tag instead for now */
 display: flex;
 flex-direction: column;
}

.main-title {
margin-left: 30px;
margin-top: 50px;
font-weight: 700;
font-size: 39px;
color:#FFFFFF;
letter-spacing: -0.05em;
}

.main-facts {
    margin-left: 30px;
    color: #FFFFFF;
    width: 400px;
}

.main-facts > li {
font-weight: 400;
font-size: 16px;
line-height: 19px;
padding-block: 10px; 
}
  
```
## Colour the bullets

With the pseudo selector [::marker](https://developer.mozilla.org/en-US/docs/Web/CSS/::marker)
Selects bullets on list items, making it much easier for us to style.

```css
.main-facts > li::marker {
    font-size: 1.6rem;
    color: #61DAFB;
}
```

## Add the background image

```css
  background-image: url('images/react-icon-large.png');
  background-repeat:no-repeat;
  background-position: right 75%;
```

## Solo Project
Build a digital business card based off this Figma design https://www.figma.com/file/4ctPLUvIn5b5Ep6YPOZWWd/Digital-Business-Card?node-id=0%3A1
Requirements: Build from scratch, seperate components for:
- Info (photo, name, role, email etc)
- About
- Interests
- Footer with social links
> **Note**
> I used the figma design as a basic reference, I changed the colour pallette completely as I like colours 💜

## Solo Project Result
<img width="350" alt="Digital Business Card" src="https://user-images.githubusercontent.com/77060368/196059851-b6267f0b-3d24-4baf-8814-bb4e323bd7f4.png">

## Solo Project Code
`App.js`
```jsx
import React from "react"
import Info from "./components/Info"
import About from "./components/About"
import Interests from "./components/Interests"
import Footer from "./components/Footer"

export default function App() {
    return (
        <div className="container">
          <Info/>
          <About/>
          <Interests/>
          <Footer/>
        </div>
    )
}
```
## Components
`Info.js`
```jsx
import React from 'react'
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome'
    import { faEnvelope } from '@fortawesome/free-regular-svg-icons'
  
  
export default function Info () {
    return (
        <div className = 'info'>
            <img src='./Emma.jpeg' alt='Profile image of Emma with a colourful swirled background. Emma has long blonde wavy hair, a heart-shaped face with big dark brown eyes and wearing a big smile.' className='profile-image'/>
            <h1 className= 'name'>Emma Wain</h1>
            <h3 className= 'role'>Frontend Developer</h3>
        <button className= 'email'><FontAwesomeIcon icon={faEnvelope}/><a href = "mailto: abc@example.com"> Email</a>
        </button>
        </div>
        
    )
}
```

`About.js`
```jsx
import React from 'react'

export default function About () {
    return (
        <div className ='about'>
           
            <h2 className= 'about-title'>About</h2>
           
           <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed eget tellus metus. Phasellus porttitor justo quam, sed tempor dolor mattis quis. Nam dapibus, orci ac congue sodales, ligula sem vulputate ante, sed posuere ligula enim ac massa. Morbi tincidunt leo non condimentum venenatis. Vivamus facilisis, risus at placerat posuere, lectus elit pellentesque quam, id hendrerit lacus sem id leo. Aenean ultricies fermentum aliquam.
           </p>
       
        </div>
        
    )
}
```
`Interests.js`
```jsx
import React from 'react'

export default function Interests () {
    return (
        <div className ='interests'>
           
            <h2 className= 'interest-title'>Interests</h2>
           
           <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed eget tellus metus. Phasellus porttitor justo quam, sed tempor dolor mattis quis. 
           </p>
       
        </div>
        
    )
}
```

`Footer.js`
```jsx
import React from 'react'
  import { FontAwesomeIcon } from '@fortawesome/react-fontawesome'
    import { faTwitterSquare, faLinkedin, faGithub} from '@fortawesome/free-brands-svg-icons'
  
export default function Footer () {
    return (
        <footer>
           
        <div>
        <a href='https://twitter.com/ewainy'><FontAwesomeIcon icon={faTwitterSquare} className= 'icon'/></a>
       
        <a href='https://linkedin.com/ewainy'><FontAwesomeIcon icon={faLinkedin} className= 'icon'/></a>
       
        <a href='https://github.com/ewainy'><FontAwesomeIcon icon={faGithub} className= 'icon'/></a>
    </div>
        </footer>
        
    )
}
```

## Styling
```css
* {
    box-sizing: border-box;
}

body {
   background-color: #d6b5f5;
}
.container {
margin: 10% 15%;
    background-color:#a75fe9;
}


.info {
      
     display:flex;
    align-items: center;
    flex-direction: column; 
    background-color:#a75fe9;
    
}

.profile-image {
   height: 50%;
   width:50%;
   border-radius: 50%;

   margin: 30px; 

}

.name {
     color: white;
}

.name, .role {
  margin:0; 
}

.role {
    color: #cdffee;  
    font-weight:400;
}

.email {
padding: 9px 13px 9px 11px;
margin: 9px;
width: 250px;
height: 34px;




/* white */

background: #FFFFFF;
/* gray/300 */

border: 1px solid #D1D5DB;
/* shadow/sm */

box-shadow: 0px 1px 2px rgba(0, 0, 0, 0.05);
border-radius: 6px;
}

.email a {
    text-decoration:none;
    color:black;
}

.email a:hover {
   text-decoration:underline;
   text-decoration-thickness: 2px;
   font-weight:700;
   color:#943de4;
}

.about, .interests {
color: white;   
margin: 0% 10%;
    
}

h2 {
    margin-bottom:0px;
    font-size: 16px;
line-height: 150%;
}

p {
    margin-top:5px;
    font-size: 10.6px;
line-height: 150%;
padding-bottom: 7px;

}

.interests p {
    padding-bottom: 40px;
}


footer {
    background-color:#943de4;
     display:flex;
    justify-content:center;
 
}

footer div {
    height: 50px;
    width: 250px;
    display:flex;
    justify-content:space-around;
    align-items: center;
   
}

.icon {
    height: 25px;
    width:25px;
    color:#cdffee;

}

.icon:hover{
    color:white;
    border:2px white solid;
    padding:1px;
}
```

## To Do 
These are the improvements I would like to make yet:
- Make responsive
- Do accessibility check
