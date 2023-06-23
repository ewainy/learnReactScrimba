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
