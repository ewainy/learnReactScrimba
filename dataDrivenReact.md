# Data Driven React
This next section will cover how to use data in our web pages. In regular HTML and the previous React project we have built, all of the content that we're displaying on the page 
has been hard coded directly into the React components. This has some major limitations which will be covered in this section. Any e-commerce site, 
blog or any webpage that uses lists is probably displaying this from a database, instead of being hardcoded.

## What we will learn
- Concept of props and syntax
- Create react components from an array of data

## AirBnB Clone Project
Use this Figma file as reference: https://www.figma.com/file/4YjrygFEXOcDp9AAnVFv7o/Airbnb-Experiences?node-id=2%3A2

### Navbar Component

```jsx
import React from "react"



export default function Navbar() {
    return (
        <header>
            <nav>
                <img src='images/airbnb-logo.png' className ="logo" />
                </nav>
        </header>
    )
}
```
### Navbar Styles
```css
nav {
    height: 70px;
    display: flex;
    align-items: center;
    box-shadow: 0px 2.98256px 7.4564px rgba(0, 0, 0, 0.1);
}


.logo {
    width: 82px;
    height:25.35px;
    margin-left:5%;
    
}
```
## Hero Component
```jsx
import React from "react"

export default function Hero() {
    return (
        <section className='hero'>
            <img src="../images/photo-grid.png" className="hero-grid"/>
            <h1>Online Experiences</h1>
            <p>Join unique interactive activities led by one-of-a-kind hosts—all without leaving home.</p>
        </section>
    )
}
```
### Hero Styles
```css
.hero {
    display:flex;
    flex-direction: column;
    font-family:'poppins';
    margin-top: 5%;
  
}

.hero-grid {
    height: 70%;
    width: 70%;
    align-self:center;
    
}

.hero h1 {
   font-size: 36px;
    line-height: 110%;
}

.hero h1, p {
    margin-left:5%;
}

.hero p {
    margin-top: 0;
}
```
<img width="415" alt="Airbnb clone navbar and hero section" src="https://user-images.githubusercontent.com/77060368/196277152-20532112-d30a-4a2b-8b7d-bca097536326.png">

## Card Component
```jsx
import React from "react"
/*
Challenge: Build the Card component
For now, hard-code in the data (like 
the rating, title, price, etc.)

Notes:
- Only render 1 instance (I already did this for you)
- The star icon and photo (katie-zaferes.png) are in the images 
  folder for your use
- Make sure to include:
    - image
    - star icon (star.png), rating, and review count
    - title
    - cost/person
- The main purpose of this challenge is to show you where our limitations
  currently are, so don't worry about the fact that you're hard-coding all
  this data into the component.
*/

export default function Card() {
    return (
        <div className="card">
        <div className='card-status'>SOLD OUT</div>
            <img src="../images/katie-zaferes.png" className="card-image" />
            <div className="card-stats">
                <img src="../images/star.png" className="card-star" />
                <span> 5.0 </span>
                <span className='grey'> (6) •</span>
                <span className='grey'> USA</span>
           
            <p>Life Lessons with Katie Zaferes</p>
            <p><span>From $136</span> / person</p>
             </div>
        </div>
    )
}
 ```
 
### Card Styles
```css
/*.card {
    box-shadow: 0px 2.98256px 7.4564px rgba(0, 0, 0, 0.1);
    border-radius:9px;
    width:176px;
    padding-bottom: 5px;
}*/


.card-status {
    background-color: white;
    width: 60px;
    padding: 5px;
    position: relative;
    top: 30px;
    left: 8px;
    font-size:10px;
}

.card-image {
    width: 176px;
    border-radius: 9px;
}

.card-stats {
    font-size:12px;
    line-height: 110%;    
}

.card-star {
position:relative;
top: 1px;
width: 14px;
height: 14px;
border-radius: 0.5px;
}

.grey {
    color: #918E9B;
    font-weight: 300;
}

p span {
    font-weight: 700;
}
```
<img width="160" alt="Card Component" src="https://user-images.githubusercontent.com/77060368/196288091-f88dd873-a1fa-4005-9a3f-a209bf49c400.png">

## Props Part 1 - Understanding The Concept
As you would have noticed, we have hardcoded the card component details, which means that it's not reusable. We can change that by using `props`.

### Primer For Understanding Props
** What's wrong with this function? **
```js
function addTwoNumbersTogether() {
    return 1 + 2
}
```
It's limited in it's capabilities, if we call this function, it will always return 3. It's not reusable.
<br>
If we add some parameters to the function, we can dynamically add and return the sum of 2 numbers that we pass in when calling the function.

```js
function addTwoNumbersTogether(a,b) {
    return a + b
}
```
We can see that, passing additional information to our elements or functions allows us to reuse them in multiple different ways.

## Props - Create a Contact Component
<img width="504" alt="Cat contact cards" src="https://user-images.githubusercontent.com/77060368/197335421-1fe8ecc9-3f88-4ccc-a682-f93241ddb43b.png">

`App.js`

```jsx
import React from "react"




function App() {
    return (
        <div className="contacts">
        
            <div className="contact-card">
                <img src="./images/mr-whiskerson.png"/>
                <h3>Mr. Whiskerson</h3>
                <div className="info-group">
                    <img src="./images/phone-icon.png" />
                    <p>(212) 555-1234</p>
                </div>
                <div className="info-group">
                    <img src="./images/mail-icon.png" />
                    <p>mr.whiskaz@catnap.meow</p>
                </div>
            </div>
            
            <div className="contact-card">
                <img src="./images/fluffykins.png"/>
                <h3>Fluffykins</h3>
                <div className="info-group">
                    <img src="./images/phone-icon.png" />
                    <p>(212) 555-2345</p>
                </div>
                <div className="info-group">
                    <img src="./images/mail-icon.png" />
                    <p>fluff@me.com</p>
                </div>
            </div>
            
            <div className="contact-card">
                <img src="./images/felix.png"/>
                <h3>Felix</h3>
                <div className="info-group">
                    <img src="./images/phone-icon.png" />
                    <p>(212) 555-4567</p>
                </div>
                <div className="info-group">
                    <img src="./images/mail-icon.png" />
                    <p>thecat@hotmail.com</p>
                </div>
            </div>
            
            <div className="contact-card">
                <img src="./images/pumpkin.png"/>
                <h3>Pumpkin</h3>
                <div className="info-group">
                    <img src="./images/phone-icon.png" />
                    <p>(0800) CAT KING</p>
                </div>
                <div className="info-group">
                    <img src="./images/mail-icon.png" />
                    <p>pumpkin@scrimba.com</p>
                </div>
            </div>
            
        </div>
    )
}

export default App
```
### Challenge -
- Create a Contact.js component in another file
- Move one of the contact card divs below into that file
- import and render 4 instances of that contact card
    - Think ahead: What's the problem with doing it this way?

`Contact.js`
```jsx
import React from "react"

export default function Contact() {
    return (
        <div className="contact-card">
            <img src="./images/mr-whiskerson.png"/>
            <h3>Mr. Whiskerson</h3>
            <div className="info-group">
                <img src="./images/phone-icon.png" />
                <p>(212) 555-1234</p>
            </div>
            <div className="info-group">
                <img src="./images/mail-icon.png" />
                <p>mr.whiskaz@catnap.meow</p>
            </div>
        </div>
    )
}
```

`App.js`
```jsx
import React from "react"
import Contact from "./Contact"


function App() {
    return (
        <div className="contacts">
            <Contact 
                img="./images/mr-whiskerson.png"
                name="Mr. Whiskerson"
                phone="(212) 555-1234"
                email="mr.whiskaz@catnap.meow"
            />
            <Contact 
                img="./images/fluffykins.png"
                name="Fluffykins"
                phone="(212) 555-2345"
                email="fluff@me.com"
            />
            <Contact 
                img="./images/felix.png"
                name="Felix"
                phone="(212) 555-4567"
                email="thecat@hotmail.com"
            />
            <Contact 
                img="./images/pumpkin.png"
                name="Pumpkin"
                phone="(0800) CAT KING"
                email="pumpkin@scrimba.com"
            />
        </div>
    )
}
export default App
```
- First we render 4 instances of the contact component. Then we add the properties to the components.
- Next we need to go back and edit the contact component file to receive these properties (a.k.a. props).

`Contact.js`
```jsx
import React from "react"

export default function Contact(props) { 
    return (
        <div className="contact-card">
            <img src="./images/mr-whiskerson.png"/>
            <h3>Mr. Whiskerson</h3>
            <div className="info-group">
                <img src="./images/phone-icon.png" />
                <p>(212) 555-1234</p>
            </div>
            <div className="info-group">
                <img src="./images/mail-icon.png" />
                <p>mr.whiskaz@catnap.meow</p>
            </div>
        </div>
    )
}
```
- Just like a normal JavaScript function that takes parameters, we can place props in our function Contact(props), we can call 'props' whatever we want, this is just convention.
- If we were to console.log(props) we will see the information from our Contact instances we rendered in `App.js` as an Object.
- Challenge - How might we change the Contact component to display all 4 cat contacts and their individual information?

## Receiving Props in a Component

`Contact.js`
```jsx
import React from "react"

export default function Contact(props) { 
    return (
        <div className="contact-card">
            <img src={props.image}/>
            <h3>{props.name}</h3>
            <div className="info-group">
                <img src="./images/phone-icon.png" />
                <p>{props.phone}</p>
            </div>
            <div className="info-group">
                <img src="./images/mail-icon.png" />
                <p>{props.email}</p>
            </div>
        </div>
    )
}
```
## Prop Quiz 😆 🥁
1. What do props help us accomplish?
Make a component more reusable.


2. How do you pass a prop into a component?
```jsx
<MyAwesomeHeader title="???" />
```

3. Can I pass a custom prop (e.g. `blahblahblah={true}`) to a native
   DOM element? (e.g. `<div blahblahblah={true}>`) Why or why not?
 <br>
No, because the JSX we use to describe native DOM elements will
be turned into REAL DOM elements by React. And real DOM elements
only have the properties/attributes specified in the HTML specification.
(Which doesn't include properties like `blahblahblah`)


4. How do I receive props in a component?
```jsx
function Navbar(props) {
    console.log(props.blahblahblah)
    return (
        <header>
            ...
        </header>
    )
  
}
  ```
5. What data type is `props` when the component receives it?
An object!
    
## Destructuring Props
A common way of using props is to destructure the object and pull out all the properties of the props object. So we had img, name, phone & email as our properties. The destructured version will look like this: 
    
 ```jsx
    export default function Contact({img, name, phone, email}) {
    return (
        <div className="contact-card">
            <img src={img}/>
            <h3>{name}</h3>
            <div className="info-group">
                <img src="./images/phone-icon.png" />
                <p>{phone}</p>
            </div>
            <div className="info-group">
                <img src="./images/mail-icon.png" />
                <p>{email}</p>
            </div>
        </div>
    )
}
```
## Array.map()
> The map() method creates a new array populated with the results of calling a provided function on every element in the calling array. 
[MDN | Array.prototype.map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

### Challenge 1
```js

Given an array of numbers, return an array of each number, squared
*/
const nums = [1, 2, 3, 4, 5]
// -->       [1, 4, 9, 16, 25]

// Your code here
const squared = nums.map(n => n * n)

console.log(squared)
```
### Challenge 2
```js

/*
Challenge 2:
Given an array of strings, return an array where 
the first letter of each string is capitalized
*/

const names = ["alice", "bob", "charlie", "danielle"]
// -->        ["Alice", "Bob", "Charlie", "Danielle"]
// Your code here

const firstLetterCapitalized = names.map(name => name[0].toUpperCase() + name.slice(1) )
console.log(firstLetterCapitalized)

```
### Challenge 3

```js
/*
Challenge 3:
Given an array of strings, return an array of strings that wraps each
of the original strings in an HTML-like <p></p> tag.

E.g. given: ["Bulbasaur", "Charmander", "Squirtle"]
return: ["<p>Bulbasaur</p>", "<p>Charmander</p>", "<p>Squirtle</p>"]
*/

const pokemon = ["Bulbasaur", "Charmander", "Squirtle"]
// -->          ["<p>Bulbasaur</p>", "<p>Charmander</p>", "<p>Squirtle</p>"]
// Your code here

const pTag = pokemon.map(pokemon => `<p>${pokemon}</p>`)
console.log(pTag)

```
## Mapping Components
Challenge: See if you can correctly pass the necessary props to the 
Joke component in `App.js` in the .map() (and render the jokeElements array) so 
the jokes show up on the page.

`jokesData.js`
```jsx
export default [
    {
        setup: "I got my daughter a fridge for her birthday.",
        punchline: "I can't wait to see her face light up when she opens it."
    },
    {
        setup: "How did the hacker escape the police?",
        punchline: "He just ransomware!"
    },
    {
        setup: "Why don't pirates travel on mountain roads?",
        punchline: "Scurvy."
    },
    {
        setup: "Why do bees stay in the hive in the winter?",
        punchline: "Swarm."
    },
    {
        setup: "What's the best thing about Switzerland?",
        punchline: "I don't know, but the flag is a big plus!"
    }
]
```

`App.js`
```jsx
import React from "react"
import Joke from "./Joke"
import jokesData from "./jokesData" 



export default function App() {
    const jokeElements = jokesData.map(joke => {
        return <Joke setup={joke.setup} punchline={joke.punchline} />
    })
    return (
        <div>
            {jokeElements}
        </div>
    )
}
```
## Pop Quiz - Map 🗺
1. What does the `.map()` array method do?
Returns a new array. Whatever gets returned from the callback
function provided is placed at the same index in the new array.
Usually we take the items from the original array and modify them
in some way.


2. What do we usually use `.map()` for in React?
Convert an array of raw data into an array of JSX elements
that can be displayed on the page.


3. Why is using `.map()` better than just creating the components
   manually by typing them out?
It makes our code more "self-sustaining" - not requiring
additional changes whenever the data changes.

## 'key' 🔑

> A “key” is a special string attribute you need to include when creating lists of elements. Keys help React identify which items have changed, are > added, or are removed. Keys should be given to the elements inside the array to give the elements a stable identity. The best way to pick a key 
> is to use a string that uniquely identifies a list item among its siblings. Most often you would use IDs from your data as keys. [React Docs](https://reactjs.org/docs/lists-and-keys.html)

```jsx
const todoItems = todos.map((todo) =>
  <li key={todo.id}>
    {todo.text}
  </li>
);
```
