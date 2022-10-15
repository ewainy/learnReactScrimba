# learningReact
Re-learning React, following the course on Scrimba


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
Bob Ziroll covers this in his course, but these resources are helpful to include:
- [Introducing JSX - React Docs](https://reactjs.org/docs/introducing-jsx.html)
- [React.js Basics – The DOM, Components, and Declarative Views Explained - freeCodeCamp](https://www.freecodecamp.org/news/reactjs-basics-dom-components-declarative-views/)
