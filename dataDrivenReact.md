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
