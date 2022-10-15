# learningReact
Re-learning React, following the course on Scrimba


## Static Pages in React

### Why React? 
- **It's composable**
    - In the old days of web development, a single page on a website was usually a single html page. A lot of the time those web pages would be 1000s and 1000s of lines long. With React, we can create our own custom components (such as a navigation bar) in their own files and piece them together to create something larger. Like lego bricks. This means that our code becomes a lot more **maintainable** and **flexible**.
    - Example of component: 
    ```jsx
    - function MainContent() {
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
