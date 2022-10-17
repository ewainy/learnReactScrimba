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
