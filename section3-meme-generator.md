# Dynamic Websites

## Static V Dynamic


Figma link: https://www.figma.com/file/MoLwFPHNHJVrzdFurxHzNV/Meme-Generator?type=design&node-id=0-1&mode=design&t=A9kJufa55my3BpoT-0

The primary thing that sets apart a static website like we've been building so far with web applications is the ability for the user to actually interact with our page in order for a user to interact with our page we have to be listening for different events on that page and then reacting when those events happen.

## Event Listeners

React Event Listeners: [https://legacy.reactjs.org/docs/events.html#mouse-events]

JavaScript:
``` js
element.addEventListener("click", myFunction);

function myFunction() {
  alert ("Hello World!");
}
```

HTML:
``` html
<button onclick="myFunction()">Click me</button>
```

React:
``` jsx

    function handleClick() {
        console.log("I was clicked!")
    }
    
    return (
        <div>
            <button onClick={handleClick}>Click me</button>
        </div>
    )

```

## Current Conundrum

### Quick Challenge (1)
We have a locally defined array with two strings in: thing one and thing two. Map over the things array, generate a paragraph element of every item in the array and then render those items below the button.

```jsx
import React from 'react';
import ReactDOM from 'react-dom';

function App() {
    const thingsArray = ["Thing 1", "Thing 2"]
    const thingsElements = thingsArray.map(thing => <p key={thing}>{thing}</p>)
    
    return (
        <div>
            <button>Add Item</button>
            {thingsElements}
        </div>
    )
}

ReactDOM.render(<App />, document.getElementById('root'));

```

### Challenge 2

 Add an event listener to the button so when it is clicked, it adds another thing to our array.
 * Hint: use the array length + 1 to determine the number of the "Thing" being added. Also, have your event listener
   console.log(thingsArray) after adding the new item to the array
   
 * Spoiler: the page won't update when new things get added to the array!

``` jsx
function App() {
    const thingsArray = ["Thing 1", "Thing 2"]
    const thingsElements = thingsArray.map(thing => <p key={thing}>{thing}</p>)
    
    function addItem() {
        const newThingText = `Thing ${thingsArray.length + 1}`
        thingsArray.push(newThingText)
        console.log(thingsArray)
    }
    
    return (
        <div>
            <button onClick={addItem}>Add Item</button>
            {thingsElements}
        </div>
    )
}

ReactDOM.render(<App />, document.getElementById('root'));

```

(Quoted from Bob Ziroll) 
Notice: Our array is changing but of course what we see on the screen is not and this brings us to our conundrum at this point React is not looking at local arrays that are just saved within a component to determine whether or not something should get to be rendered or rather if the return here should run again with an updated value for things elements actually maybe more specifically I'm determining my things elements way up here at the top back when things array is just two items long and then it sort of cementing this things elements into its memory and no matter what I do to change things array it's not going to on early days I thought maybe if I put this: `jsx const thingsElements = thingsArray.map(thing => <p key={thing}>{thing}</p>)`  below add item or something like that it might make a difference but that's not going to change the fact that this line of code will only run one time and that's the very first time.

Now one thing that might be tempting to do is inside of our add item function to say well let's not just update our array let's actually select the elements on the screen using document.getElementById and finding out where the container for these items is and then pushing an element to that list and the truth is when you're just using plain `JavaScript` that's essentially what you have to do but if you think back to one of our earlier sections we talked about how React is `declarative` and this is one of the major benefits of having a declarative library like React it makes it so that all we really need to do is update our data and React will automatically well react to that change and update our UI to display what has changed in the data all we need to be in charge of is making sure that the data updates correctly and React will handle the rest however for that to work we need to access something called `React state` and state will allow us to sort of hook into the component and make it so that whenever we update our state, which is really just values, that we're saving inside of this component it makes it so React will update the user interface based on any changes that happen to those values that are being saved in state.

Truthfully if you think of pretty much any website that you interact with, if it were made in React, any kind of interaction you had would change the state. The first thing that comes to mind is maybe a playlist or a recipe or you hit the little heart icon or maybe a bookmark icon and it fills it in and likely sends a message out to a database that updates the fact that you have liked a recipe or a song.
Any of those interactions in React are going to be updating state which then makes it so that React can update the user interface to responds to however you interact with the page.

So now lets update this using state and you can see how it will actually start to work again the way we originally expected.

#### With State
``` jsx
function App() {
    const [things, setThings] = React.useState(["Thing 1", "Thing 2"])
    
    function addItem() {
        const newThingText = `Thing ${things.length + 1}`
        setThings(prevState => [...prevState, newThingText])
    }
    
    const thingsElements = things.map(thing => <p key={thing}>{thing}</p>)
    
    return (
        <div>
            <button onClick={addItem}>Add Item</button>
            {thingsElements}
        </div>
    )
}

ReactDOM.render(<App />, document.getElementById('root'));

```


## Props Vs State
<img width="500" alt="Props" src="https://github.com/ewainy/learnReactScrimba/assets/77060368/9ae2157e-575a-4741-a800-490f59c5471b">

<img width="500" alt="State" src="https://github.com/ewainy/learnReactScrimba/assets/77060368/642ecd5a-c42b-4f10-a02b-96bdae3a2747">

## Props vs State Quiz

1. How would you describe the concept of "state"?
A way for React to remember saved values from within a component.
This is similar to declaring variables from within a component,
with a few added bonuses (which we'll get to later)


2. When would you want to use props instead of state?
Anytime you want to pass data into a component so that
component can determine what will get displayed on the
screen.


3. When would you want to use state instead of props?
Anytime you want a component to maintain some values from
within the component. (And "remember" those values even
when React re-renders the component).


4. What does "immutable" mean? Are props immutable? Is state immutable?
Unchanging. Props are immutable. State is mutable.


## useState
useState is a React Hook. If we console.log useState('hello') we will get an array with a default value if you specified one in the parentheses such as the string 'hello', followed by a function. 

### useState Array Destructuring
We can destructure the useState array immediately when receiving it as a variable. 

``` js
const [value, function] = useState()

const [value, setValue] = useState(1)
```

### useState changing state with a callback function

Note: If you ever need the old value of state to help you determine the new value of state,  you should pass a callback function to your state setter function instead of using state directly. This callback function will receive the old value of state as its parameter, which you can then use to determine your new
value of state.
     
#### Using State directly 
``` jsx

export default function App() {
    const [count, setCount] = React.useState(0)
    
    function add() {
        setCount(count + 1)
    }
    
    function subtract() {
        setCount(count - 1)
    }
    
    return (
        <div className="counter">
            <button className="counter--minus" onClick={subtract}>–</button>
            <div className="counter--count">
                <h1>{count}</h1>
            </div>
            <button className="counter--plus" onClick={add}>+</button>
        </div>
    )
}

```

#### Using Callback function:

``` jsx

    const [count, setCount] = React.useState(0)

    function add() {
        setCount(prevCount => prevCount + 1)
    }
   
    
    function subtract() {
        setCount(prevCount => prevCount - 1)
    }
```

### Changing State Quiz

1. You have 2 options for what you can pass in to a
   state setter function (e.g. `setCount`). What are they?
   
a. New value of state (setCount(42))
b. Callback function - whatever the callback function 
   returns === new value of state


2. When would you want to pass the first option (from answer
   above) to the state setter function?
Whenever you don't need the previous value of state to determine
what the new value of state should be.


3. When would you want to pass the second option (from answer
   above) to the state setter function?
Whenever you DO need the previous value to determine the new value



### Complex State: Arrays

#### Challenge 1


Convert the code below to use an array held in state instead of a local variable. Initialize the state array with the same 2 items below. Don't worry about fixing `addItem` quite yet.
  

``` jsx

function App() {
  
   // const thingsArray = ["Thing 1", "Thing 2"] // Array local variable 

    const [thingsArray, setThingsArray] = React.useState(["Thing 1", "Thing 2"]) // New array held in state

    function addItem() {
        // We'll work on this next
        // const newThingText = `Thing ${thingsArray.length + 1}`
        // thingsArray.push(newThingText)
        // document.getElementById()
        // console.log(thingsArray)
    }
    
    const thingsElements = thingsArray.map(thing => <p key={thing}>{thing}</p>)
    
    return (
        <div>
            <button onClick={addItem}>Add Item</button>
            {thingsElements}
        </div>
    )
}
``` 

The last piece we need to fix is making it so that our add item button will add something to our things array however, there is a small gotcha in here, and that is that it's tempting to want to do something like things array.push() but we cant do that.  
> What is wrong with doing array.push()?
> Hint: We can refer to our previous Props v State explanations

The reason why we cant do array.push() is we should `never ever directly modify our state`. In this case, our state is things array and we would be directly modifying it if we used .push(), it is a destructive action and therefore we would be changing the things array which is not a good idea and will not work. We know we need to use set things array in order to modify our state.

#### Challenge 2 

Write the addItem function.
- We know we need to use set things array in order to modify our state.
> What are the two options we can put in the setThingsArray?

1. We can either have a <new value> which completely replaces the old value of our state with the new value
2. or we can provide it with a <callback function> and that callback function will receive as its parameter, the old version of state, which we can use to determine the new version of state.

> Which option should we use?

- If we went with the <new value> option, we wouldnt be able to add anything to our existing array. We would simply be replacing the existing array with a new value.

- As we want to add an item to the array, we should use the <callback function> option to see what our old state value, or old array is so that we can use that value and add to it for our new array.

- Let’s do our callback function as an arrow function, we can put prevThingsArray. Let's figure out what we need to add or what we need to return in this callback function to correctly update our array with the new thing that we want to add. Remember every time we click add item, we want to determine the length of the array and just add 'Thing' + the new length of the array, so it should automatically put 'Thing 3' when we next click on the add item button.
```jsx
   setThingsArray(prevThingsArray => what do we put here?)
```

#### Caution

Another really tempting thing to do at this point is to say prevThingsArray.push however again this is not correct this prev things array is still a reference to our existing state and using .push() on it is not a good idea because that would be modifying state directly, not only that but .push() actually doesn't return the array that is modified it returns the new length of the array so this would be like replacing all of our state with a number which would just be 3 so in two ways this really isn't going to work for us.

```jsx
// DONT DO THIS
   setThingsArray(prevThingsArray => prevThingsArray.push())
```
Remember we need to return a new version of state and our state starts out as an array, in fact our program expects thingsArray to be an array because we're using `.map` on it so we need to make sure that when we are setting a new array that we return a new array, this is a new version of state. Fortunately with es6 there's a really easy way for us to `spread` in our existing array, which we have access to with preThingsArray, which by itself would get an array of everything that used to be in there - so this is not yet updating it, however we can very simply add a new item to the end of our array, using some JavaScript to determine the array length and what our Thing + number should be.

```jsx
  setThingsArray(prevThingsArray => {
            return [...prevThingsArray, `Thing ${prevThingsArray.length + 1}`]
        })
```


``` jsx

function App() {
  

    const [thingsArray, setThingsArray] = React.useState(["Thing 1", "Thing 2"]) // New array held in state

  /*  function addItem() {
        // const newThingText = `Thing ${thingsArray.length + 1}`
        // thingsArray.push(newThingText)
        // document.getElementById()
        // console.log(thingsArray)
    } */

/* New addItem function */
  function addItem() {
          setThingsArray(prevThingsArray => {
            return [...prevThingsArray, `Thing ${prevThingsArray.length + 1}`]
        })
    }
    
    const thingsElements = thingsArray.map(thing => <p key={thing}>{thing}</p>)
    
    return (
        <div>
            <button onClick={addItem}>Add Item</button>
            {thingsElements}
        </div>
    )
}
``` 
### Complex State: Objects

#### Challenge 1
Fill in the values in the markup using the properties of our state object. (Ignore `isFavorite` for now)
     
``` jsx

import React from "react"

export default function App() {
    const [contact, setContact] = React.useState({
        firstName: "John",
        lastName: "Doe",
        phone: "+1 (719) 555-1212",
        email: "itsmyrealname@example.com",
        isFavorite: false
    })
   
    
    function toggleFavorite() {
        console.log("Toggle Favorite")
    }
    
    return (
        <main>
            <article className="card">
                <img src="./images/user.png" className="card--image" />
                <div className="card--info">
                    <img 
                        src={`../images/star-empty.png`} 
                        className="card--favorite"
                        onClick={toggleFavorite}
                    />
                    <h2 className="card--name">
                        // John Doe
                        {contact.firstName} {contact.lastName}
                    </h2>
                    <p className="card--contact">
                        // +1 (719) 555-1212</p>
                            {contact.phone} 
                    <p className="card--contact">
                            // itsmyrealname@example.com</p>
                        {contact.email}
                </div>
                
            </article>
        </main>
    )
}

```

#### Challenge 2

Use a ternary to determine which star image filename should be used based on the `contact.isFavorite` property
- `true` => "star-filled.png"
-  `false` => "star-empty.png"
Then use the starIcon value to display the correct image

   ``` jsx

   // let starIcon = // Your code here
   let starIcon = contact.isFavorite ? "star-filled.png" : "star-empty.png";

    
    function toggleFavorite() {
        console.log("Toggle Favorite")
    }

       return (
        <main>
            <article className="card">
                <img src="./images/user.png" className="card--image" />
                <div className="card--info">
                    <img 
                        src={`../images/${starIcon`} // changed to variable
                        className="card--favorite"
                        onClick={toggleFavorite}
                    />

   ```

  ### Complex State: Updating State Objects

Our goal is to be able to click the star icon and have it flip our is favorite value from true to false or false the true. However currently our is favorite property is nested inside of this object, when we call setContact we need to provide a new version of state to replace the old version, however in our case we don't want to replace any of this other information only the is favourite value.

Many people's first thought is to use setContact, look at the previousContact state, and return where the is favorite property is the opposite of the previous contact’s. However, there is a major problem with doing it this way….


``` jsx

export default function App() {
    const [contact, setContact] = React.useState({
        firstName: "John",
        lastName: "Doe",
        phone: "+1 (719) 555-1212",
        email: "itsmyrealname@example.com",
        isFavorite: true
    })
    
    let starIcon = contact.isFavorite ? "star-filled.png" : "star-empty.png"
    
    function toggleFavorite() {
        setContact(prevContact => {
            return {
                isFavorite: !prevContact.isFavorite  // can you guess what is wrong with this way?
            }
        })
    }

```

Remember that in this callback function, whatever we return from that function will become the new replacement for our contact state. Currently we have an object with five properties: first name, last name, phone, email and is favorite but what I'm replacing it with is an object that only has one property is favorite, so if we click our toggle favorite you'll see that all of our other information just disappears, however it does look like our star is working it can click our star and it is correctly flipping back but obviously this is a bug .

If you have had experience in class-based React this might seem confusing to you, in class-based React, when you use this.setState it would automatically figure out how to combine your old object with the new property that you're trying to replace it with. However using hooks with React, use state no longer does this. 

The main point is that we need to make sure that we bring in all of the properties of our old object and then replace the one or two or however many properties we need to replace. A very simple way for us to do that is to `spread` in all of the properties of our old contact (and in this case we have access to the old contact using prevContact) using our spread operator and overwrite our isFavourite with whatever we need it to be.


   ``` jsx

   export default function App() {
    const [contact, setContact] = React.useState({
        firstName: "John",
        lastName: "Doe",
        phone: "+1 (719) 555-1212",
        email: "itsmyrealname@example.com",
        isFavorite: false
    })
    
    let starIcon = contact.isFavorite ? "star-filled.png" : "star-empty.png"
    
  function toggleFavorite() {
        setContact(prevContact => ({
            ...prevContact,
            isFavorite: !prevContact.isFavorite
        }))
    }
   ```

### Refactor State: Meme Generator
#### Challenge 

Update our state to save the meme-related data as an object called `meme`. It should have the following 3 properties:
- topText, bottomText, randomImage.
The 2 text states can default to empty strings for now, and randomImage should default to "http://i.imgflip.com/1bij.jpg"
<br>
Next, create a new state variable called `allMemeImages` which will default to `memesData`, which we imported.
<br>
Lastly, update the `getMemeImage` function and the markup to reflect our newly reformed state object and array in the correct way.

``` jsx

/// Code Before:

import React from "react"
import memesData from "../memesData.js"

export default function Meme() {
    
    
    const [memeImage, setMemeImage] = React.useState("http://i.imgflip.com/1bij.jpg")
    
    
    function getMemeImage() {
        const memesArray = memesData.data.memes
        const randomNumber = Math.floor(Math.random() * memesArray.length)
        setMemeImage(memesArray[randomNumber].url)
        
    }
    
    return (
        <main>
            <div className="form">
                <input 
                    type="text"
                    placeholder="Top text"
                    className="form--input"
                />
                <input 
                    type="text"
                    placeholder="Bottom text"
                    className="form--input"
                />
                <button 
                    className="form--button"
                    onClick={getMemeImage}
                >
                    Get a new meme image 🖼
                </button>
            </div>
            <img src={memeImage} className="meme--image" />
        </main>
    )
}
```

Code after:
``` jsx
import React from "react"
import memesData from "../memesData.js"

export default function Meme() {
 
    
    // const [memeImage, setMemeImage] = React.useState("http://i.imgflip.com/1bij.jpg")
    const [meme, setMeme] = React.useState({
        topText: "",
        bottomText: "",
        randomImage: "http://i.imgflip.com/1bij.jpg" 
    })
    const [allMemeImages, setAllMemeImages] = React.useState(memesData)
    
    
    function getMemeImage() {
        const memesArray = allMemeImages.data.memes
        const randomNumber = Math.floor(Math.random() * memesArray.length)
        const url = memesArray[randomNumber].url
        setMeme(prevMeme => ({
            ...prevMeme,
            randomImage: url
        }))
        
    }
    
    return (
        <main>
            <div className="form">
                <input 
                    type="text"
                    placeholder="Top text"
                    className="form--input"
                />
                <input 
                    type="text"
                    placeholder="Bottom text"
                    className="form--input"
                />
                <button 
                    className="form--button"
                    onClick={getMemeImage}
                >
                    Get a new meme image 🖼
                </button>
            </div>
            <img src={meme.randomImage} className="meme--image" />
        </main>
    )
}
```

### Passing State as Props

#### Challenge:
- Create a new component named Count
- It should receive a prop called `number`, whose value is the current value of our count
 - Have the component render the whole div.counter--count and display the incoming prop `number`
 - Replace the div.counter--count below with an instance of the new Count component

Before:
```jsx
import React from "react"

export default function App() {
    const [count, setCount] = React.useState(0)
    
    function add() {
        setCount(prevCount => prevCount + 1)
    }
    
    function subtract() {
        setCount(prevCount => prevCount - 1)
    }
    
   
    return (
        <div className="counter">
            <button className="counter--minus" onClick={subtract}>–</button>
            <div className="counter--count">
                <h1>{count}</h1>
            </div>
            <button className="counter--plus" onClick={add}>+</button>
        </div>
    )
}
}
```
`Count`
``` jsx
import React from "react"

export default function Count(props) {

    return (
       
            <div className="counter--count">
                <h1>{props.number}</h1>
            </div>
    )
}
```
`App`
```jsx

/* New change: replacing counter--count with Count component */

import Count from './Count'

    return (
        <div className="counter">
            <button className="counter--minus" onClick={subtract}>–</button>
          <Count number={count} />
            <button className="counter--plus" onClick={add}>+</button>
        </div>
    )
```
Theres a really cool thing that React does for us, that is whenever state changes it will re-render the component where the state exists and any child components that may rely on state to be working correctly.

### Setting State from Child Components

#### Challenge:
- Move the star image into its own component (Star)
- It should receive a prop called `isFilled` that it uses to determine which icon it will display
- Import and render that component, passing the value of `isFavorite` to the new `isFilled` prop.
- Don't worry about the abiliity to flip this value quite yet. Instead, you can test if it's working by manually changing `isFavorite` in state.

Before:
```jsx
import React from "react"

export default function App() {
    const [contact, setContact] = React.useState({
        firstName: "John",
        lastName: "Doe",
        phone: "+1 (719) 555-1212",
        email: "itsmyrealname@example.com",
        isFavorite: false
    })
    
    
    
    let starIcon = contact.isFavorite ? "star-filled.png" : "star-empty.png"
    
    function toggleFavorite() {
        setContact(prevContact => ({
            ...prevContact,
            isFavorite: !prevContact.isFavorite
        }))
    }
    
    return (
        <main>
            <article className="card">
                <img src="./images/user.png" className="card--image" />
                <div className="card--info">
                    <img 
                        src={`../images/${starIcon}`} 
                        className="card--favorite"
                        onClick={toggleFavorite}
                    />
                    <h2 className="card--name">
                        {contact.firstName} {contact.lastName}
                    </h2>
                    <p className="card--contact">{contact.phone}</p>
                    <p className="card--contact">{contact.email}</p>
                </div>
                
            </article>
        </main>
    )
}
```
`Star`

``` jsx

import React from "react"

export default function Star(props) {

// let starIcon = contact.isFavorite ? "star-filled.png" : "star-empty.png" // We dont have access to contact, we are receiving a prop 'isFilled'

const starIcon = props.isFilled? "star-filled.png" : "star-empty.png"

    return (
        <img 
                        src={`../images/${starIcon}`} 
                        className="card--favorite"
                        // onClick={props.???}
                    />
    )
}

```
`App`
``` jsx
Import Star from './Star'

  return (

        <main>
            <article className="card">
                <img src="./images/user.png" className="card--image" />
                <div className="card--info">
                    <Star isFilled={contact.isFavorite} /> // onClick={toggleFavorite} 
                    <h2 className="card--name">
                        {contact.firstName} {contact.lastName}
                    </h2>
                    <p className="card--contact">{contact.phone}</p>
                    <p className="card--contact">{contact.email}</p>
                </div>
                
            </article>
        </main>
    )

```
#### Challenge Conundrum
The conundrum that we have is that we have a child component that's receiving the value of isFavorite through props but it is not receiving the ability to change that state so think for second how can I give my child component the ability to make changes to the state that lives inside the parent component which is App?
One thing that can be tricky when your first learning React is the thought that you can just add a click event listener on your custom (star) component so what if I just said `onClick={togglFavorite}` you'll see well it's not quite working, *why do you think that this isn't enough for this to work?*

#### Event Listeners & Custom Component
Remember when we have a component that we created a custom component that we created all of the properties that we pass it are custom properties, so simply putting `onClick` doesn't magically register it as an event listener the onClick attribute needs to exist on `native DOM elements` like these ones that begin with the lower case letters that's because these are what will actually get created into real DOM elements by React.
However Star with a capital S is not a real DOM element, instead what's happening is React is looking at the Star component and it's rendering an image, this image has the ability to receive an onClick event listener, so what we could do is still pass onClick here but realize that onClick is just a custom prop that happens to be called the same name as the event listener. In fact oftentimes this will be changed to `handleClick` just to make it very obvious that it's not a native event listener and then over in our Star component we can add a real onClick event listener and say the value of this onClick will be the function that comes from props.handleClick.
In this case our app component is passing this toggleFavorite function to a child component and allowing that child component to run it whenever a certain event like the click event happens. However it's important to note that the context in which the toggleFavorite function exists is still here in the parent, which means that it can change the state that lives inside the parent.

``` jsx

import React from "react"

export default function Star(props) {

// let starIcon = contact.isFavorite ? "star-filled.png" : "star-empty.png" // We dont have access to contact, we are receiving a prop 'isFilled'

const starIcon = props.isFilled? "star-filled.png" : "star-empty.png"

    return (
        <img 
                        src={`../images/${starIcon}`} 
                        className="card--favorite"
                        onClick={props.handleClick}
                    />
    )
}

```
`App`
``` jsx

Import Star from './Star'

  return (

        <main>
            <article className="card">
                <img src="./images/user.png" className="card--image" />
                <div className="card--info">
                    <Star isFilled={contact.isFavorite} handleClick={toggleFavorite}  /> 
                    <h2 className="card--name">
                        {contact.firstName} {contact.lastName}
                    </h2>
                    <p className="card--contact">{contact.phone}</p>
                    <p className="card--contact">{contact.email}</p>
                </div>
                
            </article>
        </main>
    )

```
**Recap:**
We created our toggleFavorite function and we passed it to our custom component in a custom prop called handleClick 
Over in the star component, it's receiving props and it's registering a real event listener onClick whose functional value is the function that we received through props.handleClick 

The ability to pass state setter functions like this one down to children components is especially crucial in React and that due to the fact that the way that React's hierarchy is set up when it passes data.

### Passing Data Around

**A high level look at how data is passed from one component to another in React**

Let's imagine we have a fairly simple app which has the following components just represented by rectangles for now in this tree this top component represents a top-level component for example our app component in a system setup like this and the lines indicate that this top component is rendering these other components in some cases that component may just render regular DOM elements like divs and h1's or it may render additional custom components like we see in this box where it's rendering two other components 


The image represents a parent child relationship between these components let's imagine on one of these bottom level components we initialize some state and then we find that a sibling component in other words a component that is also rendered by the same parent is in need of some of the data that lives in the other components state well what exactly can we do then?
<img width="500" alt="Passing data to componenets siblings" src="https://github.com/ewainy/learnReactScrimba/assets/77060368/35e605a3-a9c2-4a64-8034-f7e787132b5d">


Is there a way for us to pass that state to a sibling component? It turns out that no, this is not possible and that's because if you look in the file of a component that has a sibling component it has no reference or knowledge about a sibling component on its own, only its immediate parent component knows that both of these components exist

#### Raising State Up
What is it that we need to do if we're facing a situation like this? Well what if we took our state in this child component and we *raised* it up into the parent component? Now that it's up in the parent component, do we have a way in React to pass data from this parent component to the child components? Yes we do, that happens through *props* so now the parent component is in charge of the state, it passes the necessary state down to the child components that require it and they can simply consume the state.
If that state were ever to change React will update this component where the state changed and it will also update any children components as well and so therefore if that state changes that's being passed through props the child component can update as well.
<img width="500" alt="Passing data through props" src="https://github.com/ewainy/learnReactScrimba/assets/77060368/6dc163a4-7cd4-477c-9bfe-fe17298abf3d">


As your application gets more and more complex you will find yourself having another component that sometimes needs access to that state as well and just like before there's no way for us to pass the state or any kind of data from one sibling component to another component, but it's also important to note that there also is no way to pass data upwards in React, a child component has no idea that this other, let’s say uncle or aunt, component exists.

<img width="500" alt="Passing data uncle aunt component" src="https://github.com/ewainy/learnReactScrimba/assets/77060368/ef4131a1-d0a1-435f-a0d2-ff963f312212">


Therefore there's no way to pass data state that way either, so this brings us back to the same solution we have before we simply *raise state up* a level and we pass more props downward.


Now over time this can get pretty tedious especially if your state ends up raising up multiple levels of the component that needs it, in which case React does offer a solution to this called `context` there's also third-party state management systems like `redux` that can help solve this problem.


#### Data flow: Keep state as local as you can
Understanding that this is the way data flows in React can be really crucial in helping you architect your application in a way that you can share state amongst only the components that need it, in fact that's an important distinction to make at this point it wouldn't be a great idea to initialize state way up near the top of your component if you don't have components along the entire tree that need it, now I'm not saying that every component needs to have access to that state for it to work, but if this component down here is the only one that needs state there's no reason for me to put state in its parent or its grandparent and then pass stuff down through props as a rule of thumb **keep state as local as you can** or in other words *keep it as closely tied to the component or components that need it as you possibly can*.

#### Further Reading: 
[Sharing State Between Components](https://react.dev/learn/sharing-state-between-components)

### Boxes Challenges 


TO FILL IN


### Conditional Rendering Quiz

1. What is "conditional rendering"?
When we want to only sometimes display something on the page
based on a condition of some sort


2. When would you use &&?
When you want to either display something or NOT display it


3. When would you use a ternary?
When you need to decide which thing among 2 options to display


4. What if you need to decide between > 2 options on
   what to display?
Use an `if...else if... else` conditional or a `switch` statement


function App() {
    let someVar
    if () {
        someVar = <SomeJSX />
    } else if() {
        ...
    } else {
        ...
    }
    return (
        <div>{someVar}</div>
    )
}

## Forms in React

The main difference in React forms is that instead of waiting until the very end of the process of filling out the form when the form is submitted and then gathering the data, instead what we do is we create `state` and every keystroke change or checkbox change or radio button change etc we update state and therefore we are watching these inputs, every keystroke or every change that's made to our form then when the time to submit comes there's no more work really to be done, we've already have gathered the data and we simply submit that to our API and pass in the state that we have been tracking all along.

#### Watching for Input Changes in React

As a reminder in vanilla JavaScript we would have a submit button and when we click that submit button it would run a function, gather all the data at that time and then submit it to an API or wherever.
What we'll be doing in React instead is maintaining up-to-date state for every change, to accomplish this there's a few things we need to do,  first of all we need some states to hold the current data that's typed into an input box, just like we've been learning we can do *const [first name, setFirstName] = React.useState(“”)* we will set an empty string as the default because we would expect our input to be an empty input box in the beginning,  but now what we need to do is listen for any changes that happen: think what event listener we might use to accomplish this? 


#### onChange
Input elements have an event called `onChange` and we can set that equal to a function. Let's call it handleChange, let's declare that function and for now let's just console.log “changed” - we can see every keystroke that we make is running our function.


#### Event Object
Now one thing we haven't spent much time talking about is this parameter that we can actually receive as a part of our event listener function, in this case we don't get to decide what actually gets passed to our handleChange event listener function, instead what will get passed to it by the browser is an `event object`. This event object is really large, it has a lot of information in it, however, there’s one property on this event object we care most about, and that is one called `target`. 
If we log `event.target` we'll see that it gives us the HTML element that triggered this event, now it looks like HTML in the console but in reality it's just the DOM object that triggered this event, that DOM object has properties that we care a lot about, for example the `value` property.

Example:
```jsx
import React from "react"

export default function Form() {
    const [firstName, setFirstName] = React.useState("")
    
    console.log(firstName)
    
    function handleChange(event) {
        console.log(event.target.value)
         
        setFirstName(event.target.value) // * Challenge: update the firstName state on every keystroke
    }
    
    return (
        <form>
            <input
                type="text"
                placeholder="First Name"
                onChange={handleChange}
            />
        </form>
    )
}
```
#### Challenge:
Track the applicant's last name as well - Notice this is *not* DRY and not practical. 

``` jsx
import React from "react"

export default function Form() {
    const [firstName, setFirstName] = React.useState("")
    const [lastName, setLastName] = React.useState("")
    
    console.log(firstName, lastName)
    
    function handleFirstNameChange(event) {
        setFirstName(event.target.value)
    }
    
    function handleLastNameChange(event) {
        setLastName(event.target.value)
    }
    
    return (
        <form>
            <input
                type="text"
                placeholder="First Name"
                onChange={handleFirstNameChange}
            />
            <input
                type="text"
                placeholder="Last Name"
                onChange={handleLastNameChange}
            />
        </form>
    )
}
```

Everything is working but we can clearly see this is not ideal, especially when you think of some of maybe the crazy forms that you filled out before that have let's say 20 or maybe even upwards of 50 input boxes, it's not going to work great for us to have a separate change function for each one of those and a separate piece of state for each one of those. So next what we'll do is we'll learn how to combine our state into an object and how to use the event parameter that we're receiving in our event handlers to determine which property of that state object we should be updating.


### Forms State Object
Let's see how we can improve our form and how we're managing state so that when the time comes to add let's say 20 or 30 more inputs we don't end up drowning in duplicated code. Once you get to 3/4 or more inputs, it starts to make a lot more sense to `save your state in an object` instead of in separate variables, a benefit of doing that is it will receive one setter function, which we're going to learn how to use to update the correct piece of state in the object.


Let's just dive into the syntax to make more sense of this (example below):
Here let's change our function back to handleChange so we can figure out a way to make it more universal and I'll set my onChange to work for both of those, let’s also get rid of our extra piece of state and instead we're going to choose to save an object as our default and then object will have a first name property as an empty string and a last name property as an empty string. Then I'll change first name to something more generic like formData and setFormData 

Example:
```jsx
import React from "react"

export default function Form() {
    const [formData, setFormData] = React.useState(
        {firstName: "", lastName: ""}
    )
    
    function handleChange(event) {
        // new code to be filled in
    }
    
    return (
        <form>
            <input
                type="text"
                placeholder="First Name"
                onChange={handleChange}
            />
            <input
                type="text"
                placeholder="Last Name"
                onChange={handleChange}
            />
        </form>
    )
}
```

If you remember back when we talked about the event that we're receiving in our handleChange event listener function, we saw how I can look at the value property to get the specific value that was typed into the input box, however at this point because an object is what we're saving in state, I can't simply say setFormData to event.target.value because that would erase my object and replace it with just a simple string and because I'm using the same handle change function on both of my inputs I need a way for the handleChange event to distinguish between which input it was that triggered that event.

#### Input Name Property
Well it turns out that a good practice that we're neglecting to do right now is to give each of our inputs a `name` property, this is true of regular HTML forms in any case but in React it's going to play a special role for us. What I can do is make the name property match exactly the property name in the state that we're saving and now I have access to `event.target.name` …Let's trigger the event to happen in the first name property and you can see it logs the first name property there, if I do it in the last name it logs last name well this is a great start we have access to the property of state that we want to change and we still have access to event.target.value which is the value that we want to change it to so now we have everything we need to call set form data and update our object correctly.

``` jsx
import React from "react"

export default function Form() {
    const [formData, setFormData] = React.useState(
        {firstName: "", lastName: ""}
    )
    
    function handleChange(event) {
        console.log(event.target.name)
    }
    
    return (
        <form>
            <input
                type="text"
                placeholder="First Name"
                onChange={handleChange}
                name="firstName"
            />
            <input
                type="text"
                placeholder="Last Name"
                onChange={handleChange}
                name="lastName"
            />
        </form>
    )
}
```
Let's go ahead and call setFormData, before when we were simply updating a string we didn't care about what the previous version of that string was because we were just going to overwrite it with whatever was in the input box, this time around however we do need to know what the old version of state was because we have other properties that we need to maintain instead of just overwrite. So I'll do a callback function in here, I'll use an arrow function and call my previous state with a prevFormData arrow function and open the function, my goal is to return a new object and I want to make sure I keep all of the old objects intact so I'm going to spread out the previous form data, then I'm going to overwrite the specific property that we're trying to override or update in this handleChange event listener.

### Computed Properties
Now on first inclination, you might try something like let's use event.target.name as the key and event.target.value as the value.  Of course we can see in our editor this is a syntax error - however with ESX there is a feature introduced called `computed properties` and what this allows me to do is to surround my key here in square brackets.
Long story short it makes it so I can turn a dynamic string like something saved in a variable and use it as the property name for my object. let me add back my console log so we can look at our state every time it changes, let's save, we can see in the console that it's empty to start with and as I change the first name my object is updating correctly my last name is also still a part of my state as I update the last name that works correctly, my entire state is staying intact.



``` jsx
import React from "react"

export default function Form() {
    const [formData, setFormData] = React.useState(
        {firstName: "", lastName: ""}
    )
    
    console.log(formData)
    
    function handleChange(event) {
        setFormData(prevFormData => {
            return {
                ...prevFormData,
                [event.target.name]: event.target.value   // computed properties
            }
        })
    }
    
    return (
        <form>
            <input
                type="text"
                placeholder="First Name"
                onChange={handleChange}
                name="firstName"
            />
            <input
                type="text"
                placeholder="Last Name"
                onChange={handleChange}
                name="lastName"
            />
        </form>
    )
}

```
### Controlled Inputs : Controlled Components
Something you might come across as you're learning about React is something called `controlled components` (refer to React docs for more context)

When we're talking about maintaining state inside of a component there's a common concept called the `single source of truth` the idea is that the state that you're maintaining in your component should be the single source of truth.

Currently in the markup inside of our inputs, each of these inputs in effect is holding its own state, think how in a regular HTML form no React involved at all when the user types into an input box essentially that input box is maintaining its own state… Well in this case we have two pieces of state, one is being held by the input box just in regular HTML and the other is updating on every keystroke held in our React code, of course we have set it up so that this state of our input box then perfectly gets mirrored by our state in React however a good practice in React is to make it so that your *React state is what drives the state that's visible inside the input box*


#### Value Property
All this really boils down to is simply adding a `value` property to each one of our inputs so here at the bottom of each one I'll add value equals and then a set of curly braces and then all we need to do is add the piece of state in other words formData. and then the name of that specific input into that value, so we'll add formData.firstName then last name and email etc. visually we're not going to see any difference here conceptually however when I type into the first name ‘Bob’ the value of this input box is no longer being controlled by the input but rather by React. Every change that I make runs my handleChange function, which then updates the correct piece of state which re-renders my form and it sets the value of my form input to be whatever state is, so now state is in the driver's seat telling the input box what to display rather than the input box telling the state what to be.
The main reason to learn this about controlled components is because sometimes if you don't set this up correctly React might sometimes complain about having uncontrolled components, fortunately the concept itself is more difficult to understand than actually implementing it so if you don't feel like diving deep into understanding controlled components just remember that you need to: `set the value of your inputs to be equal to your state that represents that input value`

```jsx
import React from "react"

export default function Form() {
    const [formData, setFormData] = React.useState(
        {firstName: "", lastName: "", email: ""}
    )
    
    function handleChange(event) {
        setFormData(prevFormData => {
            return {
                ...prevFormData,
                [event.target.name]: event.target.value
            }
        })
    }
    
    return (
        <form>
            <input
                type="text"
                placeholder="First Name"
                onChange={handleChange}
                name="firstName"
                value={formData.firstName}
            />
            <input
                type="text"
                placeholder="Last Name"
                onChange={handleChange}
                name="lastName"
                value={formData.lastName}
            />
            <input
                type="email"
                placeholder="Email"
                onChange={handleChange}
                name="email"
                value={formData.email}
            />
        </form>
    )
}
```
### Forms in React: TextArea
In HTML, text area is different to input, it isnt self closing: <textarea></textarea>
In React, they’ve chosen to make it similar to the text based input, making it a self closing element.

Example:
   ```jsx <textarea
               value={formData.comments}
               placeholder="Comments"
               onChange={handleChange}
               name="comments"
           />
```

### Forms in React: Checkbox

Checkboxes aren't just an HTML element on their own called checkbox. They are an input with a `type` of checkbox. Checkboxes are fundamentally different than what we have been working with so far with our text inputs because they just hold boolean values in other words there's not going to be some kind of string text of my checkbox instead there is a `checked` property that will either be true or false based on whether the user has checked the box or not.


#### Labels
When we have a label that is supposed to be tied to another input you have a couple options: you could nest the input directly inside of the children of the label, what that does is if the label gets clicked it will automatically propagate that click down to the input, or keep the label as its own separate element and then point that label using htmlFor to the ID of an input so that they can be associated with one another.

``` jsx
<input
               type="checkbox"
               id="isFriendly"
           />
           <label htmlFor="isFriendly">Are you friendly?</label>
```
Continued:
Anyway so now we have a checkbox that asks the person if they're friendly or not and we want to maintain this in state well because it's a checkbox we're not maintaining a string so up in state I'm going to add another piece of state, we'll call it isFriendly and set it to true as the default. 
As briefly mentioned, to associate our state isFriendly with our checkbox we're not going to use something like a value property but instead we're going to use a `checked` property now because a checkbox is either checked or unchecked the value that we put in here for checked needs to be a boolean, or at least whatever you put in will be interpreted as a boolean, so I'm going to say checked = {formData. isFriendly}


If we look closely at our handleChange function, we have only been looking at event.target.value - however with a checkbox we are not looking at the value property anymore we're looking at a checked property. Let’s rewrite our function so we can accommodate this.


A best practice that we should be following :  not to put the entire event.target.value and event.target.name inside of our setFormData(), a much better way to handle this is to `destructure event.target` and pull out the values that we need so we need name and value from event.target and then I can switch this to name and switch this to value and then as I mentioned when we're handling a checkbox there's a few more things we need to pass in. One property that will come in is this type property on all of our inputs we have a type text email and down here our type of checkbox so let's bring in that type so we can check whether or not the input that's making this handleChange function run is a type of checkbox and if it is we're also going to need the checked property and this is either going to be true or false depending on how the user has interacted with the checkbox. 
Now comes a bit of a trick when I'm setting my form data I want everything else essentially to be the same, however the piece of state that I want to update if it's a checkbox in our case isFriendly property should not take on value but rather should take on the value of *checked* so I can use a ternary here that says is the type equal to checkbox ? if so I want you to use the checked property but if not I want you to use the value property.


``` jsx
import React from "react"


export default function Form() {
   const [formData, setFormData] = React.useState(
       {
           firstName: "",
           lastName: "",
           email: "",
           comments: "",
           isFriendly: true
       }
   )
  
   function handleChange(event) {
       const {name, value, type, checked} = event.target // Better Way -Destructured
       
setFormData(prevFormData => {
          return {
               ...prevFormData,
               // [event.target.name]: event.target.value // Less Ideal


               [name]: type === "checkbox" ? checked : value // Better Way
           }
       })
   }
  
   return (
       <form>
           <input
               type="text"
               placeholder="First Name"
               onChange={handleChange}
               name="firstName"
               value={formData.firstName}
           />
           <input
               type="text"
               placeholder="Last Name"
               onChange={handleChange}
               name="lastName"
               value={formData.lastName}
           />
           <input
               type="email"
               placeholder="Email"
               onChange={handleChange}
               name="email"
               value={formData.email}
           />
           <textarea
               value={formData.comments}
               placeholder="Comments"
               onChange={handleChange}
               name="comments"
           />
           <input
               type="checkbox"
               id="isFriendly"
               checked={formData.isFriendly}
               onChange={handleChange}
               name="isFriendly"
           />
           <label htmlFor="isFriendly">Are you friendly?</label>
           <br />
       </form>
   )
}

```





### Forms in React: Radio Buttons
In React, radio buttons are kind of an interesting combination of checkboxes and text inputs.
Let's talk about how we can hook these radio buttons into our form and connect it with our state, first we need a place in state to hold all of this information - let’s add another property to our state called employment and where checkboxes are either true or false radio buttons similar to inputs will have some kind of text value to the. In this case we want employment to be some kind of string either unemployed, part-time or full-time so on a high level overview what will happen is when the user clicks one of these options we will be watching for changes in these inputs and when a change happens it will take the value of that specific radio button that was clicked and set our state to that value.


In order to associate our inputs with the correct piece of state and also to make it so that we can't click all of our inputs at once, we need to add a name property to each one of these inputs like we've done before. We will set the name equal to employment which exactly matches the property that we are saving in state - the reason we're giving the same name to all three of these radio buttons is first of all in HTML that's the way that we make it so only one of these radio buttons can be selected at any given time, but it also intuitively makes sense because we're only updating one property of employment based on which one is selected. 
Each one of these inputs will have its own unique value and this value is what will actually get saved as the value in state when this specific input is selected so for the unemployed option, we will put our value as unemployed then do the same for part-time and for full-time. 
Next we need to make sure that we are listening for change events so we will add our onChange of handleChange.
There's one more small thing that we need to change in its essentially mirroring what we did when we were setting our value equal to the state, radio buttons are a little different because we need to specify what value the state should take on if the radio button is selected or checked however that means that we can't do controlled components in the same way that we did with our inputs by setting the value equal to whatever the current value of state is. 


Radio buttons are controlled in the same way that checkboxes are controlled. With our previous checkbox example, we were able to say checked = formData.isFriendly However if we try to do the exact same thing with our radio buttons we're going to run into a little bug, radio buttons differ from checkboxes in that when we are controlling the checked property we don't simply say checked = formData.employment because unlike a checkbox this is not a boolean value…
But we can make it a boolean value by checking if the form.employment is equal to the value of the specific checkbox and now React is in charge of controlling this input rather than the input having its own HTML state.


#### Recap:
- Initially the value of formData.employment is an empty string, when we click the ‘unemployed’ option it runs the onChange for handleChange which we have defined. 
- We're pulling the name value type and checked out and we're setting the form data.
- We're bringing in all the old form data, now we're using name which we had set to employment as the key for our state that we're updating and it looks at our ternary: is the type checkbox? In reality no this is not a checkbox, it's a radio and so it skips the ‘checked’ value and instead uses value.
- This value is the value that we define on our checkbox options where we set its either as unemployed part-time or full-time 
- Then when we change state, React re-renders our form. It checks to see if the current formData.employment is equal to unemployed - in our case since that's the one we chose and that becomes a true statement and therefore checked is true and all of these other checks are false that makes it so that only one of these can be checked at any given time.



``` jsx
import React from "react"

export default function Form() {
    const [formData, setFormData] = React.useState(
        {
            firstName: "", 
            lastName: "", 
            email: "", 
            comments: "", 
            isFriendly: true,
            employment: ""
        }
    )
    console.log(formData.employment)
    
    function handleChange(event) {
        const {name, value, type, checked} = event.target
        setFormData(prevFormData => {
            return {
                ...prevFormData,
                [name]: type === "checkbox" ? checked : value
            }
        })
    }
    
    return (
        <form>
            <input
                type="text"
                placeholder="First Name"
                onChange={handleChange}
                name="firstName"
                value={formData.firstName}
            />
            <input
                type="text"
                placeholder="Last Name"
                onChange={handleChange}
                name="lastName"
                value={formData.lastName}
            />
            <input
                type="email"
                placeholder="Email"
                onChange={handleChange}
                name="email"
                value={formData.email}
            />
            <textarea 
                value={formData.comments}
                placeholder="Comments"
                onChange={handleChange}
                name="comments"
            />
            <input 
                type="checkbox" 
                id="isFriendly" 
                checked={formData.isFriendly}
                onChange={handleChange}
                name="isFriendly"
            />
            <label htmlFor="isFriendly">Are you friendly?</label>
            <br />
            <br />
            
            <fieldset>
                <legend>Current employment status</legend>
                
                <input 
                    type="radio"
                    id="unemployed"
                    name="employment"
                    value="unemployed"
                  //  checked={formData.employment} // This will create a bug
                    checked={formData.employment === "unemployed"}
                    onChange={handleChange}
                />
                <label htmlFor="unemployed">Unemployed</label>
                <br />
                
                <input 
                    type="radio"
                    id="part-time"
                    name="employment"
                    value="part-time"
                    checked={formData.employment === "part-time"}
                    onChange={handleChange}
                />
                <label htmlFor="part-time">Part-time</label>
                <br />
                
                <input 
                    type="radio"
                    id="full-time"
                    name="employment"
                    value="full-time"
                    checked={formData.employment === "full-time"}
                    onChange={handleChange}
                />
                <label htmlFor="full-time">Full-time</label>
                <br />
                
            </fieldset>
        </form>
    )
}
```




### Forms in React: Select
Example select in React: 

``` jsx
  <label htmlFor="favColor">What is your favorite color?</label>
            <br />
            <select 
                id="favColor"
                value={formData.favColor}
                onChange={handleChange}
                name="favColor"
            >
                <option value="">-- Choose --</option>
                <option value="red">Red</option>
                <option value="orange">Orange</option>
                <option value="yellow">Yellow</option>
                <option value="green">Green</option>
                <option value="blue">Blue</option>
                <option value="indigo">Indigo</option>
                <option value="violet">Violet</option>
            </select>
```

### Forms in React: Submitting the form
> By default, any <button> inside a <form> will submit it in React. It will trigger the form's onSubmit event handler.

Now just like in vanilla JavaScript if you have built a form and gathered the data in Javascript you know that clicking the submit button will actually refresh the page and it'll put all of the form values as part of a query string in the URL that all comes back to the original way that forms were submitted with an action property. 


So the first thing we will always want to do in React when we're handling a form submit is to take that event object that will get passed to this event handler and run event.prevent default()
Preventing the default means that it won't refresh our page and essentially re-render our app with all of these default values and state. Prevent default just stops all of that from happening and makes it so that we can then run code the way that we actually want to.


``` jsx
import React from "react"

export default function Form() {
    const [formData, setFormData] = React.useState(
        {
            firstName: "", 
            lastName: "", 
            email: "", 
            comments: "", 
            isFriendly: true,
            employment: "",
            favColor: ""
        }
    )
    
    function handleChange(event) {
        const {name, value, type, checked} = event.target
        setFormData(prevFormData => {
            return {
                ...prevFormData,
                [name]: type === "checkbox" ? checked : value
            }
        })
    }
    
    function handleSubmit(event) {
        event.preventDefault()
        // submitToApi(formData)
        console.log(formData)
    }
    
    return (
        <form onSubmit={handleSubmit}>
            <input
                type="text"
                placeholder="First Name"
                onChange={handleChange}
                name="firstName"
                value={formData.firstName}
            />
            <input
                type="text"
                placeholder="Last Name"
                onChange={handleChange}
                name="lastName"
                value={formData.lastName}
            />
            <input
                type="email"
                placeholder="Email"
                onChange={handleChange}
                name="email"
                value={formData.email}
            />
            <textarea 
                value={formData.comments}
                placeholder="Comments"
                onChange={handleChange}
                name="comments"
            />
            <input 
                type="checkbox" 
                id="isFriendly" 
                checked={formData.isFriendly}
                onChange={handleChange}
                name="isFriendly"
            />
            <label htmlFor="isFriendly">Are you friendly?</label>
            <br />
            <br />
            
            <fieldset>
                <legend>Current employment status</legend>
                <input 
                    type="radio"
                    id="unemployed"
                    name="employment"
                    value="unemployed"
                    checked={formData.employment === "unemployed"}
                    onChange={handleChange}
                />
                <label htmlFor="unemployed">Unemployed</label>
                <br />
                
                <input 
                    type="radio"
                    id="part-time"
                    name="employment"
                    value="part-time"
                    checked={formData.employment === "part-time"}
                    onChange={handleChange}
                />
                <label htmlFor="part-time">Part-time</label>
                <br />
                
                <input 
                    type="radio"
                    id="full-time"
                    name="employment"
                    value="full-time"
                    checked={formData.employment === "full-time"}
                    onChange={handleChange}
                />
                <label htmlFor="full-time">Full-time</label>
                <br />
            </fieldset>
            <br />
            
            <label htmlFor="favColor">What is your favorite color?</label>
            <br />
            <select 
                id="favColor" 
                value={formData.favColor}
                onChange={handleChange}
                name="favColor"
            >
                <option value="red">Red</option>
                <option value="orange">Orange</option>
                <option value="yellow">Yellow</option>
                <option value="green">Green</option>
                <option value="blue">Blue</option>
                <option value="indigo">Indigo</option>
                <option value="violet">Violet</option>
            </select>
            <br />
            <br />
            <button>Submit</button>
        </form>
    )
}

```

### Forms: Quiz!
​​1. In a vanilla JS app, at what point in the form submission process do you gather all the data from the filled-out form?
Right before the form is submitted.


2. In a React app, when do you gather all the data from the filled-out form?
As the form is being filled out. The data is all held in local state.


3. Which attribute in the form elements (value, name, onChange, etc.) should match the property name being held in state for that input?
`name` property.


4. What's different about saving the data from a checkbox element vs. other form elements?
A checkbox uses the `checked` property to determine what should be saved in state. Other form elements use the `value` property instead.


5. How do you watch for a form submit? How can you trigger a form submit?
- Can watch for the submit with an onSubmit handler on the `form` element.
- Can trigger the form submit with a button click.

### Sign Up Form Practice

Challenge: Connect the form to local state
     - 1. Create a state object to store the 4 values we need to save.
     - 2. Create a single handleChange function that can manage the state of all the inputs and set it up correctly
     - 3. When the user clicks "Sign up", check if the password & confirmation match each other. If so, log "Successfully signed up" to the console. If not, log "passwords to not match" to the console.
     - 4. Also when submitting the form, if the person checked the "newsletter" checkbox, log "Thanks for signing up for our newsletter!" to the console.


``` jsx
import React from "react"

export default function App() {
    
    const [formData, setFormData] = React.useState({
        email: "",
        password: "",
        confirmPassword: "",
        yesEmail: false
        
    })
    
 
    
    function handleChange(event) {
        const {name, type, value, checked} = event.target
        setFormData(prevFormData => {
            return {
                ...prevFormData,
                [name]: type === "checkbox" ? checked : value
            }
        })
    }
    
    function handleSubmit(event) {
        event.preventDefault()
        
   if (formData.password === formData.confirmPassword) {
       console.log('Successfully signed up')
   } else {
        console.log('Passwords do not match')
        }
        
        
        if (formData.yesEmail) {
            console.log('Thanks for signing up for our newsletter')
    }
    }
    
    return (
        <div className="form-container">
            <form className="form" onSubmit={handleSubmit}>
                <input 
                    type="email" 
                    placeholder="Email address"
                    className="form--input"
                    name = 'email'
                    onChange = {handleChange}
                    value = {formData.email}
                />
                <input 
                    type="password" 
                    placeholder="Password"
                    className="form--input"
                    name = 'password'
                    onChange = {handleChange}
                       value = {formData.password}
                />
                <input 
                    type="password" 
                    placeholder="Confirm password"
                    className="form--input"
                    name = 'confirmPassword'
                    onChange = {handleChange}
                    value = {formData.confirmPassword}
                />
                
                <div className="form--marketing">
                    <input
                        id="okayToEmail"
                        type="checkbox"
                        name = 'yesEmail'
                        checked= {formData.okayToEmail}
                        onChange = {handleChange}
                        
                    />
                    <label htmlFor="okayToEmail">I want to join the newsletter</label>
                </div>
                <button 
                    className="form--submit"
                >
                    Sign up
                </button>
            </form>
        </div>
    )
}
```
### Project: Add Text to Image

``` jsx
import React from "react"
import memesData from "../memesData.js"

export default function Meme() {
    /**
     * Challenge: 
     * 1. Set up the text inputs to save to
     *    the `topText` and `bottomText` state variables.
     * 2. Replace the hard-coded text on the image with
     *    the text being saved to state.
     */
    
    const [meme, setMeme] = React.useState({
        topText: "",
        bottomText: "",
        randomImage: "http://i.imgflip.com/1bij.jpg" 
    })
    const [allMemeImages, setAllMemeImages] = React.useState(memesData)
    
    
    function getMemeImage() {
        const memesArray = allMemeImages.data.memes
        const randomNumber = Math.floor(Math.random() * memesArray.length)
        const url = memesArray[randomNumber].url
        setMeme(prevMeme => ({
            ...prevMeme,
            randomImage: url
        }))
        
    }
    
    function handleChange(event) {
        const {name, value} = event.target
        setMeme(prevMeme => ({
            ...prevMeme,
            [name]: value
        }))
    }
    
    return (
        <main>
            <div className="form">
                <input 
                    type="text"
                    placeholder="Top text"
                    className="form--input"
                    name="topText"
                    value={meme.topText}
                    onChange={handleChange}
                />
                <input 
                    type="text"
                    placeholder="Bottom text"
                    className="form--input"
                    name="bottomText"
                    value={meme.bottomText}
                    onChange={handleChange}
                />
                <button 
                    className="form--button"
                    onClick={getMemeImage}
                >
                    Get a new meme image 🖼
                </button>
            </div>
            <div className="meme">
                <img src={meme.randomImage} className="meme--image" />
                <h2 className="meme--text top">{meme.topText}</h2>
                <h2 className="meme--text bottom">{meme.bottomText}</h2>
            </div>
        </main>
    )
}
```
### Making API Calls:

A very common thing you'll need to do in React is interact with an API that lives outside of your application. Usually what you're doing is either requesting information from an API or sometimes submitting information. Whenever you're requesting information from an API in React, usually you want to display or use that information somehow so getting data from an API in React consists of essentially two parts:
First of all you need to get the data and that would be using something like fetch or another tool for getting data like Axios
2. The second step would be to then take that data and save it to state. Once it's saved in state your application can interact with it, it can display it on the screen and all of the other benefits that come with saving something in state 

Let's see some preliminary syntax: *Spoiler alert* you will be shown why it's broken to do it this way!

To start us off, let's do a fetch request to the Star wars API, console.log the data and then change it to save the data to state.

Now we’re going to illuminate what is going on behind the scenes right now, we're going to console log ‘component rendered;’ every time the app component renders or re-renders by React it will run this console log. If we open our console hit refresh: we've got an infinite loop of component rendering.
Now think for a second- why was this console log running over and over and over again?

The reason we were stuck in an infinite loop is because this fetch lives on sort of the top level of our component. Because of that anytime the component renders it's going to call fetch and every time it calls fetch it's going to set the Star wars data which is updating our state and therefore causing React to re-render this component and then it's running fetch again setting the state re rendering the component calling fetch again setting the state re rendering the component etc. We will look at how to handle side effects in React next.

``` jsx

import React from "react"

export default function App() {
    const [starWarsData, setStarWarsData] = React.useState({})
    
    console.log("Component rendered")
    
    fetch("https://swapi.dev/api/people/1")
        .then(res => res.json())
        .then(data => setStarWarsData(data))
    
    return (
        <div>
            <pre>{JSON.stringify(starWarsData, null, 2)}</pre>
        </div>
    )
}

```

### Side effects in React - Intro to useEffect()

<img width="700" alt="React's primary tasks" src="https://github.com/ewainy/learnReactScrimba/assets/77060368/72003138-66b6-4781-bb23-687efa3a6e45">


First of all React is in charge of working with the Dom and the browser to render some kind of user interface to the page. The React team has created a really simple interface to allow us as developers to accomplish this with JSX and elements just like HTML, under the hood of course, React is taking that JSX and turning it into elements that can be displayed on the page.

Another task which we have mostly been learning about in this section is to manage state for us, and if you think carefully about it it's managing that state between the render cycles. In other words React can remember state values from one render to the next. So that's another thing that React does, and we are allowed to hook into that state using the useState hook. 

Then naturally it's next task is sort of a combination of these two, it needs to `keep the user interface updated whenever state changes` there are certainly other tasks that React is accomplishing but you can probably boil the main three tasks down to these three.

<img width="700" alt="React cant handle" src="https://github.com/ewainy/learnReactScrimba/assets/77060368/00d0da46-e3bf-4ba4-bd63-646a69bffd70">

#### What is React not able to handle on its own?
The major one that we're going to be discussing is any kind of side effects - or I put outside effects here,  all that really means is anything that lives outside of React's purview or reach. So some examples of this are local storage on your own browser, for example we can of course use code to access local storage but React has no real hand in interfacing with local storage.


Any kind of API or database interactions that occur in our code, we can write that code to interact with APIs but React is not in charge of that code, or rather React has no way to know which APIs we are going to be reaching out to.

Similarly any kind of subscriptions you might have, if you include web sockets in your application, for example you have a chat application that updates live.
And as it turns out, even if you have two different pieces of internal state that you need to sync together or have the state pieces of state react to a change in another piece of state, React can manage the state of each one behind the scenes, but it's not really looking at any kind of crossover between those two states. 
So in the end, it boils down to basically anything that React is not in charge of, can be considered a side effect or an outside effect.


So using this API interaction for an example if we look at our code from before I commented
Essentially just doing what it's told it's trying to put something on the screen it's trying to manage state and it's trying to keep state and the screen in sync. If state ever changes, it needs to re-render the component.

```jsx
import React from "react"


export default function App() {
   const [starWarsData, setStarWarsData] = React.useState({})
  
   // console.log("Component rendered")
  
   // fetch("https://swapi.dev/api/people/1")
   //     .then(res => res.json())
   //     .then(data => setStarWarsData(data))
      
   // side effects
  
   return (
       <div>
           <pre>{JSON.stringify(starWarsData, null, 2)}</pre>
       </div>
   )
}

```

As a side note if this were receiving props and somehow those props were also to change, for example if those props were a piece of state in a higher up component that then changed, this component would also re- render. So with our code living here on the top level of our component (commented out), React really doesn't have a way to stop us from setting the state here (setStarWarsData) causing the re-render, which would then set the state again causing the re- render and so forth in that infinite loop.


So the React team gives us a really nice hook called useEffect.
In the documentation this is called the effect hook and I like to think of it as a tool that react has given us developers to use sort of like a blank canvas that allows us to interract outside of the React ecosystem. Which again, mostly consists of state, props, and the user interface it puts on the page.
I like to think of useEffect as being a tool that allows us to synchronize React state with those outside systems like like local storage or APIs

### useEffect documentation/ links
> https://react.dev/reference/react/useEffect
> https://react.dev/learn/synchronizing-with-effects
> https://react.dev/learn/you-might-not-need-an-effect
> https://overreacted.io/a-complete-guide-to-useeffect/

### useEffect Syntax and Default Behaviour

UseEffect has one required parameter and one optional second parameter.
The required first parameter is a function - a callback function.
This function acts as a place for us to put our side effects code in, the little example we have here, this fetch request is considered a side effect and that's because it's reaching out side of React’s ecosystem, but also trying to set some state in the process.

Let's move this fetch request inside of our useEffect hook - and that's it.
Inside of the callback function that we provide to useEffect, we're allowed to put our code that has other side effects. Side effects that interact with systems outside of React, and maybe more specifically, interact with those outside systems to keep them in sync with our local state in this component.

But- we’re only halfway there.


``` jsx
import React from "react"


export default function App() {
   const [starWarsData, setStarWarsData] = React.useState({})
  
   console.log("Component rendered")
  
  
   // fetch("https://swapi.dev/api/people/1")
   //     .then(res => res.json())
   //     .then(data => setStarWarsData(data))
      
   
   // side effects
   React.useEffect(function() {
       fetch("https://swapi.dev/api/people/1")
           .then(res => res.json())
           .then(data => setStarWarsData(data))
   }, ???)
  
   return (
       <div>
           <pre>{JSON.stringify(starWarsData, null, 2)}</pre>
       </div>
   )
}
```

In our code, in our call back function, if we don't provide a second parameter to our React useEffect function - there's almost no difference between putting our code inside of useEffect, and putting it outside of useEffect on the top level of our component. There is one small difference, and that is that anything that we put inside of this callback function is guaranteed to run only after the user interface has been painted to the screen. In other words, only after React has taken our user interface and created real elements on our page. 
So this callback function is guaranteed to run in a specific order only after this has been placed on the DOM, but for our purposes here there's basically no difference, we're still stuck in an infinite loop. 

So in the next section we're going to start diving into useEffect and understanding first why we need to provide the second parameter and then what that second parameter is and how it solves this problem.

### useEffect Dependencies Array 

To try and demonstrate exactly what's happening within useEffect I came up with the simplest little counter app that just has a single button that says ‘add’ and something that displays what the current count is in state. The reason I did this was just to make it so that we had a really easy way to trigger re-renders and state changes on our app.
 I also replaced our useEffect callback function with just a console log that lets us know that this effect function ran. You can already see what's happening down in the console, but if I refresh the site, well first we get the component rendered console log and then we get the effect ran console log, as we talked about before, the function we pass to useEffect will run after every render of our component.

It essentially makes it no different than moving our console log outside onto the top level of our function just like we have here, which means if we ever wanted to change the state within our useEffect, we get caught in that loop, like we've talked about a few times already.
Now with my little counter, I can trigger a manual re-render by just clicking the add button, and we can see that when I click the add button it updates the state behind the scenes. So it changes from 0 to 1 and it re-renders our component, now with count equalling the number one, you get our console log which rendered because it's a function call here on the top level of our component.
Then it would have skipped down here and rendered our UI so that it could display the correct count, now that count is set to 1, and last of all it ran our effect, which of course runs after every render. As we talked about, the first parameter to useEffect is this function but there's a second parameter which although I mentioned is optional is something that you will almost always include and that is something called a `dependencies array`.

The array that we passed as a second parameter to useEffect will contain values that if they change from one render to the next, will cause this effect to run.
In other words, it limits the number of times that this effect will run, or rather it determines when this effect will run instead of running after every single render.

Now if I leave this as an empty array it effectively tells React “I want to run this function on the very first render of my component but then there are no dependencies to watch and trigger this effect to run again” so it ends up being it runs once when the component first loads and that's it never again.
We can see this in action by refreshing our app, seeing that we did get effect ran and then adding to our count and from here adding to our count is no longer running this effect, and that's because we have an empty array which is not looking for any changes between one render and the next.

```jsx
import React from "react"


export default function App() {
   const [starWarsData, setStarWarsData] = React.useState({})
   const [count, setCount] = React.useState(0)
  
   console.log("Component rendered")
  
   // side effects
   React.useEffect(function() {
       console.log("Effect ran")
       // fetch("https://swapi.dev/api/people/1")
       //     .then(res => res.json())
       //     .then(data => console.log(data))
   }, [])
  
   return (
       <div>
           <pre>{JSON.stringify(starWarsData, null, 2)}</pre>
           <h2>The count is {count}</h2>
           <button onClick={() => setCount(prevCount => prevCount + 1)}>Add</button>
       </div>
   )
}


```
Now if I wanted to run this effect every time count changed I would have to make sure that my effect knew that count was one of the dependencies that signaled the effect to run again.

```jsx
​​// side effects
   React.useEffect(function() {
       console.log("Effect ran")
       // fetch("https://swapi.dev/api/people/1")
       //     .then(res => res.json())
       //     .then(data => console.log(data))
           }, [count]) 

```

So now when I hit refresh it will run once, which is why we get “effect ran” in the console, and now every time I hit ‘add’ I'm also running my effect again.


Under the hood this is what's happening: when I hit refresh, state starts out as zero and the component will take count and replace it with a zero everywhere it can find it. So here in the dependencies array,  we would have an array with the number 0 in the first index, then when I click add, React will update the count from 0 to 1 and it will rerun my function or in other words re-render my app component where everywhere I'm using ‘count’ is now 1.

### useEffect Quiz

1. What is a "side effect" in React? What are some examples?
- Any code that affects an outside system.
- local storage, API, websockets, two states to keep in sync


2. What is NOT a "side effect" in React? Examples?
- Anything that React is in charge of.
- Maintaining state, keeping the UI in sync with the data,
 render DOM elements


3. When does React run your useEffect function? When does it NOT run
  the effect function?
- As soon as the component loads (first render)
- On every re-render of the component (assuming no dependencies array)
- Will NOT run the effect when the values of the dependencies in the
 array stay the same between renders


4. How would you explain what the "dependencies array" is?
- Second parameter to the useEffect function
- A way for React to know whether it should re-run the effect function

### useEffect: When to Use Dependencies

Challenge:   Combine `count` with the request URL so pressing the "Get Next Character" button will get a new character from the Star Wars API. * Remember: don't forget to consider the dependencies array!

```jsx
    
import React from "react"


export default function App() {
   const [starWarsData, setStarWarsData] = React.useState({})
   const [count, setCount] = React.useState(1)


  
   React.useEffect(function() {
       console.log("Effect ran")
       fetch("https://swapi.dev/api/people/1")
           .then(res => res.json())
           .then(data => setStarWarsData(data))
   }, [])
  
   return (
       <div>
           <h2>The count is {count}</h2>
           <button onClick={() => setCount(prevCount => prevCount + 1)}>Get Next Character</button>
           <pre>{JSON.stringify(starWarsData, null, 2)}</pre>
       </div>
   )
}
```
Changed section:
```jsx 
   React.useEffect(function() {
       console.log("Effect ran")
       fetch(`https://swapi.dev/api/people/${count}`)
           .then(res => res.json())
           .then(data => setStarWarsData(data))
   }, [count])

```

After the first render, it went to the API and got the character with the ID of 1 we can even see here in our UI, it says the count is 1. Then I'll click ‘get next character’ button, the count is 2 and after a little bit we get our next character, the one with the ID of 2, which is C3PO.  So when I click ‘get next character’ again, it updates count, updating count re-runs my function or rather re-renders my component. The effect then looks at the old array, which was an array with one item with the number of 2 in it, and then it looks at the new array which is an array with the number of 3 in it and notices that something has changed which is then a trigger to run my function again with the new number of 3 as count.


### useEffect Cleanup Function - 
One thing you should always try to be aware of when you are interfacing with side effects using useEffect is any potential consequences that might happen if you don't `clean up` the things that you do in that side effect.
This is just one example where we're adding an event listener that is not getting cleaned up when this component unmounts.


Another instance could be, you are creating a web socket connection with maybe a chat API and you have a little chat app that will update your screen automatically every time there's a new chat message on the server. Well when you create that subscription to the chat API, and then try to unmount the component, it's always a good idea to then sever that websocket connection as a way to `clean up` the effect that you have created in your useEffect.



The useEffect cleanup is pretty easy to do, remember we have a function as our first parameter to useEffect but currently we're not actually returning anything from that function. Well as it turns out, what we can return from useEffect can be a function.

So when React runs our useEffect function here, it will receive in return another function that it can use to then clean up any side effects that you might have created.
In reality it has no idea what the side effects are that we created so what we put in the body of our cleanup function should be something that we write to clean up our own side effects.
In the case of adding an event listener, there's a sister method called remove event listener, now with remove event listener we need to pass the exact same function that we provided when we added it. And now, with this cleanup function, when we resize the browser, we won't get a memory leak.

```jsx
import React from "react"


export default function WindowTracker() {
  
   const [windowWidth, setWindowWidth] = React.useState(window.innerWidth)
  
   React.useEffect(() => {
       function watchWidth() {
           console.log("Resizing...")
           setWindowWidth(window.innerWidth)
       }


       window.addEventListener("resize", watchWidth)
      
       return function() {
           console.log("Cleaning up...")
           window.removeEventListener("resize", watchWidth)
       }
   }, [])
  
   return (
       <h1>Window width: {windowWidth}</h1>
   )
}
```


Our app component is deciding when the window tracker component should be rendered as soon as we toggle that on and it renders it to the screen it sets the window with state, it determines what that should be based on the current window width at the instant that this component gets rendered.
Remember, useEffect will only run after the DOM has been painted, or in other words once the H1 has been rendered to the screen.
So after creating the state under our H1, it will register our useEffect, this useEffect has no dependencies because there's nothing inside of here that's going to make it re-register a new event listener. So it registers this event listener on the resize event of the window and then anytime I resize it is reacting to the event listener that I set up. 
Then when I toggle off the window tracker, React recognizes that this component has reached the end of its lifecycle and it's about to be removed from the DOM. So it takes the function that it received from us when it first set up our useEffect and it just runs it, in fact it runs it kind of blindly, it doesn't know what that function contains - but we as the developers are expected to successfully clean up after our side effects and so we remove that event listener

#### One last recap: 
useEffect takes two parameters; the first one is the effect that you want to run, the second one is any dependencies that React should watch for changes to rerun your effect function and that effect function is allowed to return another function that can clean up after any side effects that might be lingering. Now for many effects that you set up you might find yourself not actually needing to provide a cleanup function at all, which is fine, this is not a required part of useEffect for it to work.


### useEffect with Async function  
   useEffect takes a function as its parameter. If that function returns something, it needs to be a cleanup function. Otherwise, it should return nothing. 
If we make it an async function, it automatically returns a promise instead of a function or nothing.
Therefore, if you want to use async operations inside of useEffect, you need to define the function separately inside of the callback function, as seen below:

```jsx


  
   React.useEffect(() => {
       async function getMemes() {
           const res = await fetch("https://api.imgflip.com/get_memes")
           const data = await res.json()
           setAllMemes(data.data.memes)
       }


       getMemes()
      
      /* return () => {
          // cleanup function would go here, but not needed in this use case
       } */ 
   }, [])

```
Extra: Helpful tutorial by Todd Motto: https://ultimatecourses.com/blog/using-async-await-inside-react-use-effect-hook


<img width="1013" alt="Scrimba Section 3 recap" src="https://github.com/ewainy/learnReactScrimba/assets/77060368/091117d9-9606-42c7-a52f-5b789922bba6">

