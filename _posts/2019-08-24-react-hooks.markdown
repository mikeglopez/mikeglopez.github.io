---
layout: post
title:  "React Hooks"
date:   2019-08-24 12:04:18 -0600
categories: react
featured-img: error
---
This will just be a quick introduction into using React hooks. Hooks are a new addition in React 16.8 that let you use state while avoiding writing classes. _Hooks are optional! As per the React documentation, there are no plans to remove classes from React._

You may be wondering what the point of using hooks is if there are no breaking changes. Hooks are useful for many reasons.

* Avoid the issue of creating a class component soley to handle state and data for the functional components it renders.
* Code using hooks minifies better than code using class components
* If you write a function component and realize you need to add state, you can use hooks instead of rewriting the function as a class
* Make it easy to reuse stateful logic, not state itself, so you can keep your code DRY _Don't Repeat Yourself_

Here's an example you may be familiar with, a React class component.

This component renders a simple shopping list app.


```javascript
import React from 'react';
import './App.css';

class App extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      list: []
    };

    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleSubmit(e) {
    e.preventDefault();
    this.setState({list: [...this.state.list, document.getElementById('listItem').value]});
  }

  componentDidMount() {
    document.title = `Shopping List: ${this.state.list.length}`;
  }

  render() {
    return (
      <div className="App">
        <h1>My Shopping List: {this.state.list.length} Items</h1>
        <form onSubmit={this.handleSubmit}>
          <input type="text" id="listItem" /> <input type="submit" value="Add" id="submit" />
        </form>
        {this.state.list.map((item, index) => {
          return <p key={index}>{item}</p>
        })}
      </div>
    );
  }
}

export default App;
```


The functional component below produces the same app and looks much cleaner.


```javascript
import React, { useState, useEffect } from 'react';
import './App.css';

function App() {
  const [list, addToList] = useState([]);

  const handleSubmit = (e) => {
    e.preventDefault();
    addToList([...list, document.getElementById('listItem').value]);
  }

  useEffect(() => {
    document.title = `Shopping List: ${list.length}`;
  });

  return (
    <div className="App">
      <h1>My Shopping List: {list.length} Items</h1>
      <form onSubmit={handleSubmit}>
        <input type="text" id="listItem" /> <input type="submit" value="Add" id="submit" />
      </form>
      {list.map((item, index) => {
        return <p key={index}>{item}</p>
      })}
    </div>
  );
}

export default App;
```


Let's get started!


```javascript
import React, { useState, useEffect } from 'react';
import './App.css';
```


To begin, make sure you've updated to **React 16.8**. In your file, import `useState` and `useEffect`.


```javascript
function App() {
  const [list, addToList] = useState([]);
}
```


Hooks can only be called from React function components. Create a function component and add the `useState` hook. Using array destructuring, assign `useState` to an array of the current state value, and a function that will let you update the state value. `useState` takes an argument of the state's initial value.

Next, you'll add the `useEffect` hook.


```javascript
useEffect(() => {
  document.title = `Shopping List: ${list.length}`;
});
```


`useEffect` is the hook equivalent to `componentDidMount`, `componentDidUpdate` and `componentWillUnmount`. Above, we're setting the document title each time React updates the DOM.


Now we're going to call the update function:

```javascript
const handleSubmit = (e) => {
    e.preventDefault();
    addToList([...list, document.getElementById('listItem').value]);
  }
```

```javascript
  return (
    <div className="App">
      <h1>My Shopping List: {list.length} Items</h1>
      <form onSubmit={handleSubmit}>
        <input type="text" id="listItem" /> <input type="submit" value="Add" id="submit" />
      </form>
      {list.map((item, index) => {
        return <p key={index}>{item}</p>
      })}
    </div>
  );
```


I created a `list` state variable and `addToList` function in my state declaration. Now, I can use `addToList` to update state in the same way you would use `this.setState`. To read state, instead of `this.state.list`, all you have to do is use `list` directly.


Enjoy!