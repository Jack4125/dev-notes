---
title: "React"
tags: "React, Hooks, Redux"
---

### form submission

put `onSubmit` on <form onSubmit={}> instead of `<button>`.
This will allow `ENTER` to submit form.

### react props

If passing in multiple props, use de-structuring `const {prop1, prop2, etc...} = this.props;`

class components gets `props` automatically;
functional components needs to pass in `props`, and no need for `this` anymore;

functional components can be de-structured in function parameter like `({props1, props2, etc...})`

```js
handleChange = e => {
  this.setState({
    // obj.["key"]
    [e.target.id]: e.target.value,
  })
}
;<input id="whatever" onChange={this.handleChange} />
```

### react state

never modify state directly.
always _replace_ entire array by using `this.setState({ new: new })`
can be achieved by with de-structuring `{...state, newItem}`

override state with multiple pairs

```js

state = {
  one: [],
  two: []
}

{
  ...state,
  three: [...state.three, newValue]
}
```

### pass in parameters without invoking function immediately

surround function with another anonymous function:
`onClick={()=>{myFunction(parameters)}}`

### class lifecycle methods diagram

https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/

Infinite loop error: result of updating state inside `ComponentDidUpdate` method.

### Higher Order Component

wraps around exported component `export default hoc(myComponent);`

# Axios vs fetch

fetch returns `object`, so has one more step of `obj.json()` before res can be used.

axios returns `json` first time, but data is inside `res.data`

### Hooks

`useState()` is just a function.
Each useState returns two things: a name for the state, and a function used to edit the state.
`const [stuff, setStuff] = useState([])`
is the same as

_`setStuff`_ has two versions of parameters.
One is directly passing in what you want the state to be.
Two is passing in a function that returns what you want to set, with `currentState` as parameter, which will ensure that state is always _up to date_ before being changed or `...`ed.

```js
const stuff = []
const setStuff = () => {}
```

setStuff _completely replaces_ original state.
So can't simply add to state. Must output the whole thing

### useEffect

useEffect runs every time component renders or re-renders
runs _after_ pages renders.

second argument of useEffect is an [] of options. React will re-run the effect even if just one of them is different.

`useEffect(()=>{}, [state])` with the second parameter allows you to set up multiple actions that only runs when each invidual state changes.

```js
import React, { Component } from "react"

class App extends Component {
  constructor(props) {
    super(props)
    this.state = {
      users: [],
    }
  }

  // Using async...await for fetch inside lifecycle methods
  async componentDidMount() {
    // Call self-hosted API to get users response
    const res = await fetch("/users")
    const users = await res.json()
    this.setState({ users })
  }

  render() {
    return (
      <div className="App">
        {this.state.users.map(user => (
          <p>{user}</p>
        ))}
      </div>
    )
  }
}

function HooksApp() {
  const [count, setCount] = useState(0)
  /**
   * Unlike the setState method found in class components,
   * useState does not automatically merge update objects.
   * You can replicate this behavior by combining the function
   * updater form with object spread syntax:
   */
  setCount(prevCount => {
    // Object.assign would also work
    return { ...prevCount, ...updatedValues }
  })
  /**
   * Another option is useReducer, which is more suited for
   * managing state objects that contain multiple sub-values.
   */

  function brokenIncrement() {
    setCount(count + 1)
    setCount(count + 1)
  }

  function increment() {
    setCount(count => count + 1) // functional
    setCount(count => count + 1) // functional

    /**
     * useState hook is asynchronous, just like setState,
     * so in the case above count does not get updated before
     * the next count is added. Always use functional way if you
     * are working with data from the state.
     * https://reactjs.org/docs/hooks-reference.html#functional-updates
     */
  }

  return (
    <div>
      <div>{count}</div>
      <button onClick={brokenIncrement}>Broken increment</button>
      <button onClick={increment}>Increment</button>
    </div>
  )
}

export default App
```

## Redux

- [Summary](https://www.academind.com/learn/react/redux-vs-context-api/)
- `connect()` lets you pass global state to any component in your application, without manually passing that data via props.
- Redux Thunk - used to support asynchronous actions.
