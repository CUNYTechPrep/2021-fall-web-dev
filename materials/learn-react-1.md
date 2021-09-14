# Learn React 1

## Step 1. Getting started

Open up your terminal.

Make sure node and npm are working. Run the following commands:

```bash
node --version
```

It should display version 14.x (or 16.x).

```bash
npx --version
```

Any version means it is installed.

If these are not installed install the latest version of node.

  - https://github.com/CUNYTechPrep/guides#development-environment-setup
  
## Step 2. Create and Launch a React project

In your terminal, make a directory for your projects, and change directory to it. I like to keep my files in a directory named `ctp` under my home directory.

```bash
cd ~
mkdir ctp
cd ctp
```

> you only have to `mkdir ctp` if this directory doesn't already exist

Now we're going to create our first react app:

```bash
npx create-react-app learn-react-1
cd learn-react-1
npm start
```

If your browser opened up automatically and you see the spinning logo, then everything is working.

## Step 3. Look through the code

Open up the `learn-react-1` directory in your text editor.

Let's take a look at `/public/index.html` specifically the `<div id="root"></div>`

Now let's go to the `/src/index.js` and let's delete all of the code in this file. This is the entry point to our React app. No need to worry about the rest of the files in src for now. We will eventually delete them all, and create new files ourselves.

> When you delete all of the code and save the file, what happened in the browser?

## Step 4. Hello World

In `/src/index.js` add the following code:

```JSX
import React from 'react'
import ReactDOM from 'react-dom'

ReactDOM.render(
  <h1>Hello World!</h1>,
  document.getElementById("root")
);
```

Let's break this code down.

- import statements
- ReactDOM.render(reactElement, htmlDOMContainer)
- JSX

> This is not HTML, it is JSX.

Try to change the HTML, add some other tags. Try the following examples and observe the results.

```JSX
<H1>Hello World!</H1>
```

```JSX
<div>
  <h1>Hello World!</h1>
  <h1>Hi Again</h1>
</div>
```

```JSX
<div>
  <h1>Hello World!
  <h1>Hi Again</h1>
</div>
```

```JSX
<h1>Hello World!</h1>
<h1>Hi Again</h1>
```

- all standard HTML tags are available in lowercase version
- all tags must be closed
- all JSX must have a single parent tag

## Step 5. Functional Components

The point of a component based system is to create reusable componenets. Let's make the following changes to our code:

```JSX
import React from 'react'
import ReactDOM from 'react-dom'

function Welcome() {
  return (
    <div>
      <h1>Welcome CTP Class</h1>
      <p>My first component</p>
    </div>
  );
}

ReactDOM.render(
  <Welcome />,
  document.getElementById("root")
);
```

What did we just do?

- components must begin with capital letters
- notice the `( )` in the return
- Try <Welcome></Welcome>
- is <Welcome> a standard HTML tag?
	
We can and should create many small components, that we put together. Let's make the following changes:

```JSX
...

function Student() {
  return <div>- Sally is in class</div>;
}

function Welcome() {
  return (
    <div>
      <h1>Welcome CTP Class</h1>
      <p>Who is in class?</p>
      <Student />
    </div>
  );
}

...
```

- Try adding multiple `Student` tags in the Welcome component. What happens?

## Step 6. Props and embedding JavaScript in JSX

A component like Student is a lot more useful if we can dynamically change its content. We will do that now:

```JSX
...

function Student(props) {
  return <div>- { props.name } is in class</div>;
}

function Welcome() {
  return (
    <div>
      <h1>Welcome CTP Class</h1>
      <p>Who is in class?</p>
      <Student name="Sally" />
      <Student />
      <Student />
    </div>
  );
}

...
```

- add different names to the last two `Student` tags
- `{  }` allows us to embed javascript within JSX
- `props` are always passed to components
- `props` are immutable, what does that mean?
- try changing `props.name = "Jose"`, what happens?

## Step 7. HTML Attributes like `<div class="red">` and adding CSS

JSX is not HTML, but it does allow you to set standard HTML attributes with slightly different names.

Instead of `class` we use `className`. React's designers did this on purpose to differnetiate JSX from HTML.

There are many ways to handle CSS in React. For now we will keep it simple and use the file `/src/index.css`

in `/src/index.css` add the following to the bottom of the file:
```css
.red-text {
  color: red;
}
```

in `/src/index.js` add the following below your import statements:

```JSX
import './index.css'
```

and make the following change:

```JSX
...

function Student(props) {
  return <div className="red-text">- { props.name } is in class</div>;
}

...
```

Functional components are really great and powerful, but sometimes we have to store a lot more logic in our components, and we would like to follow best practices and use separate functions for that. In the next step we will see how we can do that.

## Step 8. Class Components

In `/src/index.js` replace all code with the following:

```JSX
import React from 'react'
import ReactDOM from 'react-dom'
import './index.css'

class App extends React.Component {
  render() {
    return (
      <div>
        <p>The button has been clicked 0 times</p>
        <button>Click me!!!</button>
      </div>
    );
  }
}

ReactDOM.render(
  <App />,
  document.getElementById("root")
);
```

- notice the use of new ES6 syntax: `class`, `extends`
- all class components require a `render()` method, it should return JSX

**Props** are still available in class components, but they are now part available throughout the class via `this.props`

```JSX
...

class App extends React.Component {
  render() {
    return (
      <div>
        <p>The button has been clicked 0 times</p>
        <button>{ this.props.btnText }!!!</button>
      </div>
    );
  }
}

ReactDOM.render(
  <App btnText="Click me" />,
  document.getElementById("root")
);

...
```

In our current example, our intent is to update the app with the number of times that the button has been pressed. We will do this by introducing two new concepts in the following two steps.

## Step 9. Event Handling

First, let's do something when the button is clicked. 

```JSX
class App extends React.Component {
  render() {
    return (
      <div>
        <p>The button has been clicked 0 times</p>
        <button onClick={ (e) => alert('The button was pressed!') }>{ this.props.btnText }!!!</button>
      </div>
    );
  }
}
```

In regular HTML, we can use the attribute `<div onclick="...">` to run some javascript event handling code. In react we will use the `onClick` prop instead.

- notice the camel casing of standard html attributes `className`, `onClick` in react
- onClick receives a function that will run in response to the event
- what happens if you change it to just `onClick={ alert('The button was pressed!) }`
- `onClick` works with other html tags, such as img, div, h1, etc.
- `e` stands for event, it contains information about the user event, such as which mouse button was used to click.


## Step 10. State and Constructors

Now that we can handle the events like a button being clicked, we want to keep track of how many times the user has clicked the button.

Props are immutable (read-only) so we cannot use them to do that. So now we introduce another core react object that **is mutable, `state`**. 

```JSX
class App extends React.Component {
  constructor(props) {
    super(props); // required so that props work
    this.state = {
      clicks: 0,
    };
  }

  handleClick(event) {
    this.setState({
      clicks: this.state.clicks + 1
    });
  }

  render() {
    return (
      <div>
        <p>The button has been clicked { this.state.clicks } times</p>
        <button onClick={ (e) => this.handleClick(e) }>{ this.props.btnText }!!!</button>
      </div>
    );
  }
}
```

In this example we initialize state in the constructor and update it in the `handleClick` function.

- the constructor receives the props
- `super(props)` must be called in the constructor for class components to work properly
- `this.state` is initialized with an object, provide initial values to all state properties you will use.
- `handleClick` is our own function. We can call it anything we like.
- state can only be updated with the `this.setState({...})` property
- `this.state.clicks = 42` will not work as expected, try this.

Check out a similar and funny example:
https://jsbin.com/xemiboniye/1/edit?html,js,output

## Step 11. Lab: Toggle Boards

Complete the lab in this repository: https://github.com/CUNYTechPrep/react-lab-1
