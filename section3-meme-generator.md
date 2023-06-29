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
