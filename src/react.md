# React

[Step by step guide for writing awesome React components](https://codeburst.io/step-by-step-guide-for-writing-awesome-react-components-210c6def902b)
[Our Best Practices for Writing React Components](https://engineering.musefind.com/our-best-practices-for-writing-react-components-dec3eb5c3fc8)
[How to write great React](https://medium.com/swlh/how-to-write-great-react-c4f23f2f3f4f)
[Structuring React Projects](https://blog.bitsrc.io/structuring-a-react-project-a-definitive-guide-ac9a754df5eb)
[Optimal file structure for React applications](https://medium.com/@Charles_Stover/optimal-file-structure-for-react-applications-f3e35ad0a145)
[Performance](https://reactjs.org/docs/optimizing-performance.html)
[Twitter Performance Guide](https://medium.com/@paularmstrong/twitter-lite-and-high-performance-react-progressive-web-apps-at-scale-d28a00e780a3)

### Code Splitting

- Very important to split up code to avoid slow initial load time caused by huge bundles.
- `Lazy Loading` is the process of only load the assets currently needed by the user.
- Dynamic `import()`
- `React.lazy()`
- [Route-based (React Router) code splitting](https://reactjs.org/docs/code-splitting.html#route-based-code-splitting)
  - Does not support Named Exports yet, but there's a [way around that](https://reactjs.org/docs/code-splitting.html#named-exports).

[Code Splitting](https://facebook.github.io/create-react-app/docs/code-splitting)
[Code Splitting](https://serverless-stack.com/chapters/code-splitting-in-create-react-app.html)

- Pure Functions:
  1. Do not change its inputs.
  2. Always returns the same result for the same input.
- unidirectional dataflow
- React is [Declarative Language](https://codeburst.io/declarative-vs-imperative-programming-a8a7c93d9ad2?gi=47d7d1fcbbcc)
- React is unaware of changes made to the DOM outside of React. It determines updates based on its own internal representation, and if the same DOM nodes are manipulated by another library, React gets confused and has no way to recover.

### Higher-Order Components (HOC)

- A component usually takes data (props/state) and output UI. A higher-order component takes a component and output a component.

## Lifecycle Methods

- `constructor()`
  - `constructor()` is only needed to initialize local state and bind methods. Load data, set up subscription. Pass in `props` and
- `render()` - called during `props/state` changes.
  - `render()` is the only _required_ method.
  - `render()` Should be _pure_ - does not modify component state, returns the same result each time, and does not modify browser. Perform actions in other methods.
- `componentDidMount()`
- `componentDidUpdate(prevProps, prevState)` - not called for initial render.
  - _If calling `setState()` in here, then it must be wrapped in a condition comparing to `prevProps/prevState`. Or it may enter an infinite loop!_
- `componentWillUnmount()`
  - Do not `setState()` here because the component will never be re-rendered.
  - Remembering to remove any event listeners the plugin registered to prevent memory leaks.

## Props

- `props` are read-only.
- A component should never modify its `props`, but received changed props from its ancestors.
- React Rule: _All React components must act like pure functions with respect to their props._

## State

- State updates are merged, and merges are _shallow_ - does not change un-referenced property, but completely replaces referenced ones.
- `setState()` enqueues changes, tells React that this component and its children need to be re-rendered with the updated state.
- `setState()` does not always immediately update the component, so do no read `this.state` immediately after.
- Use `componentDidUpdate` or a callback `setState(updater, callback)`, either of which are guaranteed to fire after the update has been applied. [See Example](https://reactjs.org/docs/state-and-lifecycle.html#state-updates-may-be-asynchronous).
- Forms: Controlled Components
  - State should reflect form values changes and events.
  - [Handling Multiple Events](https://reactjs.org/docs/forms.html#handling-multiple-inputs)
- [Lifting State Up](https://reactjs.org/docs/lifting-state-up.html):
  - If several components need to reflect the same changing data, lift the shared state up to their closest common ancestor.
  - If something can be derived from either props or state, it probably shouldn’t be in the state.

### Fragment

- Fragments let you group a list of children without adding extra nodes to the DOM.
  ```js
      <React.Fragment key={...}>
        <li>Item 1</li>
        <li>Item 2</li>
      </React.Fragment>
  ```
- Can _ONLY_ pass `key` as attribute.
- Shorter Syntax: `<>...</>` - but this syntax cannot have `key`.

## Template

- Class Components

  ```js
  class Comp extends React.Component {
    constructor(props) {
      super(props);
      this.handleChange = this.handleChange.bind(this);
      this.state = { property: '' };
    }

    componentDidMount() {}
    componentDidUpdate(prevProps, prevState) {}

    handleChange(e) {
      this.setState({ property: e.target.value });
    }

    render() {
      const variable = this.state.property;

      return (
        <form>
          <input value={property} onChange={this.handleChange} />
        </form>
      );
    }
  }
  ```

### form submission

put `onSubmit` on <form onSubmit={}> instead of `<button>`.
This will allow `ENTER` to submit form.

### react props

If passing in multiple props, use de-structuring `const {prop1, prop2, etc...} = this.props;`

class components gets `props` automatically;
functional components needs to pass in `props`, and no need for `this` anymore;

functional components can be de-structured in function parameter like `({props1, props2, etc...})`

```js
handleChange = (e) => {
  this.setState({
    // obj.["key"]
    [e.target.id]: e.target.value,
  });
};
<input id="whatever" onChange={this.handleChange} />;
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
const stuff = [];
const setStuff = () => {};
```

setStuff _completely replaces_ original state.
So can't simply add to state. Must output the whole thing

### useEffect

useEffect runs every time component renders or re-renders
runs _after_ pages renders.

second argument of useEffect is an [] of options. React will re-run the effect even if just one of them is different.

`useEffect(()=>{}, [state])` with the second parameter allows you to set up multiple actions that only runs when each invidual state changes.

```js
import React, { Component } from 'react';

class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      users: [],
    };
  }

  // Using async...await for fetch inside lifecycle methods
  async componentDidMount() {
    // Call self-hosted API to get users response
    const res = await fetch('/users');
    const users = await res.json();
    this.setState({ users });
  }

  render() {
    return (
      <div className="App">
        {this.state.users.map((user) => (
          <p>{user}</p>
        ))}
      </div>
    );
  }
}

function HooksApp() {
  const [count, setCount] = useState(0);
  /**
   * Unlike the setState method found in class components,
   * useState does not automatically merge update objects.
   * You can replicate this behavior by combining the function
   * updater form with object spread syntax:
   */
  setCount((prevCount) => {
    // Object.assign would also work
    return { ...prevCount, ...updatedValues };
  });
  /**
   * Another option is useReducer, which is more suited for
   * managing state objects that contain multiple sub-values.
   */

  function brokenIncrement() {
    setCount(count + 1);
    setCount(count + 1);
  }

  function increment() {
    setCount((count) => count + 1); // functional
    setCount((count) => count + 1); // functional

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
  );
}

export default App;
```

## Redux

- [Summary](https://www.academind.com/learn/react/redux-vs-context-api/)
- `connect()` lets you pass global state to any component in your application, without manually passing that data via props.
- Redux Thunk - used to support asynchronous actions.
