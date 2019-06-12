# React & Redux

# React - A Declarative, Component-based JavaScript library for building User Interfaces
* It is a JS library that aims to simplify development of visual interfaces
* Its primary goal is to divide the UI into a collection of components
* React only chose to implement the View layer instead of the full MVC stack
* React made it easy to integrate into an existing codebase
* Corollary technologies, like Redux and GraphQL, can be integrated with React

## The main 4 concepts of React
1. Components
2. JSX
3. State
4. Props

## Use create-react-app

```js
npx create-react-app <app-name>
```
When you run *npx create-react-app app-name* , npx is going to download the most recent *create-react-app* release, run it, and then remove it from your system. This is great because you will never have an outdated version on your system, and every time you run it, you're getting the latest and greatest code available

### create-react-app commands
* npm start: start the app
* npm run build : to build the React application files in the build folder, ready to be deployed to a server
* npm test : to run the testing suite using Jest
* npm eject : to eject from create-react-app

## Important JS for using React
### 1. **Variables** - var, let & const
I. var
* If you don't initialize the variable when you declare it, it will have the undefined value until you assign a value to it.
```js
var a //typeof a === 'undefined'
```
* You can redeclare the variable many times, overriding it:
```js
var a = 1
var a = 2
```
* *var* is a function scoped

II. let
* *let* is a block scoped & can be reassigned

III. const
* *const* is a block scoped & can't be reassigned
* Object & Array can be mutated

### 2. Arrow functions

```js
const myFunc = function(a, b) {
  return a + b
}

const myFunc = (a, b) => {
  return a + b
}

const myFunc = (a, b) => a + b

const myFunc = a => a + 5

const myFunc = () => ({ value: 'test' })
```

### 3. this in arrow functions
* When defined as a method of an object, in a regular function this refers to the object, so you
can do:
```js
const car = {
  model: 'Fiesta',
  manufacturer: 'Ford',
  fullName: function() {
    return `${this.manufacturer} ${this.model}`
  }
}

// calling car.fullName() will return "Ford Fiesta"
```

* The *this* scope with arrow functions is inherited from the execution context. An arrow function does not bind this at all.
* Due to this, arrow functions are not suited as object methods
* Arrow functions cannot be used as constructors either, when instantiating an object will raise a *TypeError*
```js
const car = {
  model: 'Fiesta',
  manufacturer: 'Ford',
  fullName: () => {
    return `${this.manufacturer} ${this.model}`
  }
}

// calling car.fullName() will return "undefined undefined"
```

* If you rely on this in an event handler, a regular function is necessary:
```js
const link = document.querySelector('#link')
  link.addEventListener('click', () => {
    // this === window
})
```
```js
const link = document.querySelector('#link')
  link.addEventListener('click', function() {
    // this === link
})
```

### 4. Objects and Arrays using Rest and Spread
**Spread operator**
* An array, an object or a string can be expended using the spread operator
```js
// Array
const a = [1, 2, 3]
const b = [...a, 4, 5, 6]
const c = [...a]

// Object
const newObj = { ...oldObj } // clone an object

// String
const str = 'hey'
const arr = [...str] // ['h', 'e', 'y']
```

* Use an array as function argument:
```js
const f = (foo, bar) => {}

const a = [1, 2]
f(...a)
```

**Rest operator**
* The **rest element** is useful when working with **array destructuring**:

```js
const numbers = [1, 2, 3, 4, 5]
[first, second, ...others] = numbers
// first = 1, second = 2, others = [3, 4, 5]
```

```js
const { first, second, ...others } = {
  first: 1,
  second: 2,
  third: 3,
  fourth: 4,
  fifth: 5
}

first // 1
second // 2
others // { third: 3, fourth: 4, fifth: 5 }
```

* Spread properties allow to create a new object by combining the properties of the object passed after the spread operator:
```js
const items = { first, second, ...others }
items //{ first: 1, second: 2, third: 3, fourth: 4, fifth: 5 }
```

* Given an object, using the destructuring syntax you can extract just some values and put them into named variables:
```js
const person = {
  firstName: 'Tom',
  lastName: 'Cruise',
  actor: true,
  age: 54 //made up
} 

const { firstName: name, age } = person //name: Tom, age: 54
// name and age contain the desired values.
```

* The syntax also works on arrays:
```js
const a = [1, 2, 3, 4, 5]
const [first, second] = a
```

* This statement creates 3 new variables by getting the items with index 0, 1, 4 from the array a :
```js
const [first, second, , , fifth] = a
```

### 5. Template literals

```js
const str1 = `something`
const str2 = 
  `first part \
  second part` 
// first part second part
const str3 = `do ${str1}: ${1 + 2}` // do something: 3
```

### 6. Template tags
* It's actually used by lots of popular libraries around, like Styled Components or Apollo, the GraphQL client/server lib, so it's essential to understand how it works
* In Styled Components template tags are used to define CSS strings:
```js
const Button = styled.button`
font-size: 1.5em;
background-color: black;
color: white;
`
```
* In Apollo template tags are used to define a GraphQL query schema:
```js
const query = gql`
  query {
  ...
  }
`

function gpl(literals, ...expressions) {}
```

```js
function interpolate(literals, ...expressions) {
  let string = ``
  for (const [i, val] of expressions) {
    string += literals[i] + val
  } 
  string += literals[literals.length - 1]
  return string
}
```

### 7. Classes

```js
// Main Class or Supper Class
class Person {
  constructor(firstName, lastName, age, likes = []) {
    this.firstName = firstName
    this.lastName = lastName
    this.age = age
    this.likes = likes
  }

  getBio() {
    let bio = `${this.firstName} is ${this.age}.`
    this.likes.forEach(like => bio += ` he/she likes ${like}.`)
    return bio
  }

  set fullName(fullName) {
    const names = fullName.split(' ')
    this.firstName = names[0]
    this.lastName = names[1]
  }

  get fullName() {
    return `${this.firstName} ${this.lastName}`
  }

  //======================================================
  // Normally methods are defined on the instance, not on the class
  // Static methods are executed on the class instead.
  //======================================================
  static genericGetBio() {
    return 'Hello'
  }
}

// Subclass
class Employee extends Person {
  constructor(firstName, lastName, age, position, likes) {
    // need to use super() for using the properties of super class constructor
    super(firstName, lastName, age, likes)
    this.position = position
  }

  // Override on getBio()
  getBio() {
    return `${this.fullName} is a ${this.position}`
  }

  // subclass method
  getYearsLeft() {
    return 65 -this.age
  }
}

class Student extends Person {
  constructor(firstName, lastName, age, grade) {
    super(firstName, lastName, age)
    this.grade = grade
  }

  getBio() {
    const status =  (this.grade >= 70) ? 'passing' : 'failing'
    return `${this.firstName} is ${status} the class.`
  }

  updateGrade(change) {
    this.grade += change
  }
}

// Instantiate
// myPerson - instance or object
//================ Person - Super Class ================
const myPerson = new Person('Farah', 'Islam', 12, ['Teaching', 'Biking'])
// console.log(myPerson)
console.log(myPerson.getBio())
// myPerson.setName('Zayan Ahmed')
// console.log(myPerson)

//============ Employee - Subclass ================
const employee = new Employee('Foyez', 'Ahmed', 26, 'Cricketer', ['Teaching', 'Biking'])
// console.log(employee)
console.log(employee.getBio()) // Employee's getBio()
employee.fullName = 'Zayan Ahmed' // Person's setName()
console.log(employee.getBio()) // Employee's getBio()
console.log(employee.getYearsLeft()) 

//================ Student - Subclass ================
const student = new Student('Rafeh', 'Siddique', 15, 80)
console.log(student.getBio())
student.updateGrade(-20)
console.log(student.getBio())
```

### 8. Promises
`Promises are one way to deal with asynchronous code in JavaScript, without writing too many callbacks`

```js
// =================== CALLBACK ===========================
function getNumber(num, cb) {
  if(typeof num === 'number') {
    cb(undefined, num * 2)
  } else {
    cb('Number must be provided.')
  }
}

getNumber(6, (err, data) => {
  if(err) {
    console.log(err)
  } else {
    getNumber(data, (err, data) => {
      if(err) {
        console.log(err)
      } else {
        getNumber(data, (err, data) => {
          if(err) {
            console.log(err)
          } else {
            console.log(data)
          }
        })
      }
    })
  }
})

// ===================PROMISE===========================
const getNumberPromise = num => new Promise((res, rej) => {
  if(typeof num === 'number') {
    res(num * 2)
  } else {
    rej('Number must be provided.')
  }
})

// Promise Chaining
getNumberPromise(6)
  .then(data => getNumberPromise(data))
  .then(data => getNumberPromise(data))
  .then(data => console.log(`Promise: ${data}`))
  .catch(err => console.log(`Promise: ${err}`))

const processData = async () => {
  let data = await getNumberPromise(6)
  data = await getNumberPromise(data)
  data = await getNumberPromise(data)
  return data
}

processData()
  .then(data => console.log(`Async/Await: ${data}`))
  .catch(err => console.log(`Async/Await: ${err}`))
  
// ===================GENERATOR===========================
const getNumberGen = (generator) => {
  const gen = generator()

  function handle(yielded) {
    if(!yielded.done) {
      if(typeof yielded.value === 'number') {
        handle(gen.next(yielded.value * 2))
      }
    }
  }

  return handle(gen.next())
}

getNumberGen(function* () {
  let num = yield 6
  num = yield num
  num = yield num
  console.log(`Generator: ${num}`)
})
```

Example of the Fetch API

```js
const status = response => {
  if (response.status => 200 && response.status < 300) {
    return Promise.resolve(response)
  }

  return Promise.reject(new Error(response.statusText))
}

const json = response => response.json()

fetch('/todos.json')
  .then(status)
  .then(json)
  .then(data => {
    console.log('Request succeeded with JSON response', data)
  })
  .catch(error => {
    console.log('Request failed', error)
  })
```

Handling errors
```js
new Promise((resolve, reject) => {
  throw new Error('Error')
}).catch(err => {
  console.error(err)
})

// or
new Promise((resolve, reject) => {
  reject('Error')
}).catch(err => {
  console.error(err)
})
```

**Cascading errors**
```js
new Promise((resolve, reject) => {
  throw new Error('Error')
})
  .catch(err => {
    throw new Error('Error')
  })
  .catch(err => {
    console.log(err)
  })
```

**Promise.all()**
```js
const f1 = fetch('/something.json')
const f2 = fetch('/something2.json')

Promise.all([f1, f2])
  .then(([res1, res2]) => {
    console.log('Result', res1, res2)
  })
```

## ES Modules - ECMAScript standard for working with modules

```js
<script type="module" src="index.js"></script>
import package from 'module-name'
const package = require('module-name')
```

**Module**
`A module is a JavaScript file that exports one or more values (objects, functions or variables), using the export keyword.`

```js
// uppercase.js
export default str => str.toUpperCase()

import toUpperCase from './uppercase.js'
```

```js
// default export
export default str => str.toUpperCase()

// multiple export
const a = 1
const b = 2
const c = 3

export { a, b, c }
```

```js
import toUpperCase from 'https://flavio-es-modules-example.glitch.me/uppercase.js'
import * from 'module'
import { a, b } from 'module'
import { a, b as two } from 'module'
import React, { Component } from 'module'
```

**CORS**
`Cross-Origin Resource Sharing, the way to let clients and servers communicate even if they are not on the same domain.`
Modules are fetched using cors.

**Browsers that do not support modules**
```js
<script type="module" src="module.js"></script>
<script nomodule src="fallback.js"></script>
```

## Single Page Applications - SPA
**SPA:** SPA only load the application code (HTML, CSS, JavaScript)
once, and when you interact with the application, what generally happens is that JavaScript intercepts the browser events and instead of making a new request to the server that then returns a new document, the client requests some JSON or performs an action on the server but the page that the user sees is never completely wiped away, and behaves more like a desktop application.
React Applications are also called Single Page Applications.
**Examples:**
* Gmail
* Google Maps
* Facebook
* Twitter
* Google Drive

## Overriding the navigation
`Since you get rid of the default browser navigation, URLs must be managed manually. This part of an application is called the router (like React Router, Ember).

"Broken back button" issue: when navigating inside the application the URL didn't change (since the browser default navigation was hijacked) and hitting the back button.

This problem can now be solved using the History API offered by browsers, but most of the time you'll use a library that internally uses that API, like React Router.

## Declarative Programming - what you do rather than how you do
React is declarative.
Declarative means:
* you can build Web interfaces without even touching the DOM directly
* you can have an event system without having to interact with the actual DOM Events

Example: HTML, SQL, React, etc

In iterative, you tell the browser exactly what to do, instead of telling it what you need.

The React declarative approach abstracts that for us. We just tell React we want a component to be rendered in a specific way, and we never have to interact with the DOM to reference it later.

## Immutability
`In programming, a variable is immutable when its value cannot change after it's created.`

Strings are immutable by default, when you change them in reality you create a new string and assign it to the same variable name.

An immutable variable can never be changed. To update its value, you create a new variable. This applies to  objects and arrays.

For example, you should never mutate the state property of a component directly, but only through the setState() method.

In Redux, you never mutate the state directly, but only through reducers, which are functions.

## Purity
`In JavaScript, when a function does not mutate objects but just returns a new object, it's called a pure function`

React applies this concept to components. A React component is a pure component when its output is only dependant on its props.

All functional components are pure components:
```js
const Button = props => {
  return <button>{props.message}</button>
}
```

Classes components can be pure if their output only depends on the props:
```js
class Button extends React.Component {
  render() {
    return <button>{this.props.message}</button>
  }
}
```

## Composition
In programming, composition allows you to build more complex functionality by combining small and focused functions.

For example, think about using map() to create a new array from an initial set, and then filtering the result using filter() :

```js
const list = ['Apple', 'Orange', 'Egg']
list.map(item => item[0]).filter(item => item === 'A') //'A'
```

In React, composition allows you to have some pretty cool advantages.
You create small and lean components and use them to compose more functionality on top of them. 

## Create specialized version of a component
Use an outer component to expand and specialize a more generic component:

```js
const Button = props => {
  return <button>{props.text}</button>
}

const submitButton = () => {
  return <Button text="Submit" />
}

const LoginButton = () => {
  return <Button text="Login" />
}
```

## Pass methods as props
```js
const Button = props => {
  return <button onClick={props.onClickHandler}>{props.text}</button>
}

const LoginButton = props => {
  return <Button text="Login" onClickHandler={props.onClickHandler} />
}

const Container = () => {
  const onClickHandler = () => {
    alert('clicked')
  }

  return <LoginButton onClickHandler={onClickHandler} />
}
```

## Using children
The props.children property allows you to inject components inside other components.
The component needs to output props.children in its JSX:

```js
const Sidebar = props => {
  return <aside>{props.children}</aside>
}
```
and you embed more components into it in a transparent way:
```js
<Sidebar>
  <Link title="First link" />
  <Link title="Second link" />
</Sidebar>
```

## Higher order components
When a component receives a component as a prop and returns a component, it's called higher order component.

## The Virtual DOM
The DOM (Document Object Model) is a Tree representation of the page, starting from the `<html>` tag, going down into every child, which are called nodes.

It's kept in the browser memory, and directly linked to what you see in a page. The DOM has an API that you can use to traverse it, access every single node, filter them, modify them.

```js
document.getElementById(id)
document.getElementsByTagName(name)
document.createElement(name)
parentNode.appendChild(node)
element.innerHTML
element.style.left
element.setAttribute()
element.getAttribute()
element.addEventListener()
window.content
window.onload
window.dump()
window.scrollTo()
```

The Virtual DOM is a technique that React uses to optimize interacting with
the browser.

React keeps a copy of the DOM representation, for what concerns the React rendering: the Virtual DOM

Every time the DOM changes, the browser has to do two intensive operations: repaint (visual or content changes to an element that do not affect the layout and positioning relative to other elements) and reflow (recalculate the layout of a portion of the page - or the whole page layout).

React uses a Virtual DOM to help the browser use less resources when changes need to be done on a page.

When you call `setState()` on a Component, specifying a state different than the previous one, React marks that Component as dirty. This is key: React only updates when a Component changes the state explicitly.