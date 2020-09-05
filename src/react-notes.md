# React

React is a front-end JavaScript library for building user interfaces.

## Style Guides

- [Step by step guide for writing awesome React components](https://codeburst.io/step-by-step-guide-for-writing-awesome-react-components-210c6def902b)
- [Our Best Practices for Writing React Components](https://engineering.musefind.com/our-best-practices-for-writing-react-components-dec3eb5c3fc8)
- [How to write great React](https://medium.com/swlh/how-to-write-great-react-c4f23f2f3f4f)

## Structure

- [Structuring React Projects](https://blog.bitsrc.io/structuring-a-react-project-a-definitive-guide-ac9a754df5eb)
- [Optimal file structure for React applications](https://medium.com/@Charles_Stover/optimal-file-structure-for-react-applications-f3e35ad0a145)

## Basics

- Pure Functions:
  1. Do not change its inputs.
  2. Always returns the same result for the same input.
- unidirectional dataflow
- React is [Declarative Language](https://codeburst.io/declarative-vs-imperative-programming-a8a7c93d9ad2?gi=47d7d1fcbbcc)
- React is unaware of changes made to the DOM outside of React. It determines updates based on its own internal representation, and if the same DOM nodes are manipulated by another library, React gets confused and has no way to recover.

## Syntax

- [Syntax](https://reactjs.org/docs/jsx-in-depth.html)

## Performance

- [Performance](https://reactjs.org/docs/optimizing-performance.html)
- [Twitter Performance Guide](https://medium.com/@paularmstrong/twitter-lite-and-high-performance-react-progressive-web-apps-at-scale-d28a00e780a3)

## [Hooks](https://reactjs.org/docs/hooks-intro.html)

- [Why use hooks](https://medium.com/better-programming/react-hooks-vs-classes-add2676a32f2)

## Template

- Class Components

  ```
  class Comp extends React.Component {
    constructor(props) {
      super(props);
      this.handleChange = this.handleChange.bind(this);
      this.state = {property: ''};
    }

    componentDidMount() {}
    componentDidUpdate(prevProps, prevState) {}

    handleChange(e) {
      this.setState({property: e.target.value});
    }

    render() {
      const variable = this.state.property;

      return (
        <form>
          <input
            value={property}
            onChange={this.handleChange} />
        </form>
      );
    }
  }
  ```

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

### Events/Handlers

- Syntax:

  ```
  # HTML
  onclick = "handleAction()"

  # React
  onClick = {handleAction}
  ```

- Remember to bind `this` in constructor.
- [Pass a second parameter](https://reactjs.org/docs/handling-events.html#passing-arguments-to-event-handlers) (like in a loop) by binding it directly in the caller.

### Lists

- Put `key` in the loop itself _like_ a prop, not the content function.
- `key` isn't actually a prop. If the key's value is needed, pass it separately.
- [List Creation Process](https://reactjs.org/docs/lists-and-keys.html#keys-must-only-be-unique-among-siblings)

### Composition

- Containment: `{props.children}` or other self-defined props.

### Fragment

- A common pattern in React is for a component to return multiple elements. Fragments let you group a list of children without adding extra nodes to the DOM.
- Similar to `<div>` or `<section>` in HTML, but doesn't add extra node.
  ```
  render() {
    return (
      <React.Fragment key={...}>
        <li>Item 1</li>
        <li>Item 2</li>
      </React.Fragment>
    );
  }
  ```
- Can _ONLY_ pass `key` as attribute.
- Shorter Syntax: `<>...</>` - but this syntax cannot have `key`.

### Code Splitting

- Very important to split up code to avoid slow initial load time caused by huge bundles.
- `Lazy Loading` is the process of only load the assets currently needed by the user.
- Dynamic `import()`
- `React.lazy()`
- [Route-based (React Router) code splitting](https://reactjs.org/docs/code-splitting.html#route-based-code-splitting)
  - Does not support Named Exports yet, but there's a [way around that](https://reactjs.org/docs/code-splitting.html#named-exports).

### Higher-Order Components (HOC)

- A component usually takes data (props/state) and output UI. A higher-order component takes a component and output a component.
- Used when a component's logic and structure can often be re-used, we can create a

## Advanced

### Context API

- Context provides a way to pass data through the component tree without having to pass props down manually at every level.. [See here](https://reactjs.org/docs/context.html).

React-router:

- [Active Link](https://stackoverflow.com/questions/41131450/active-link-with-react-router)

- Pass props to a component rendered by React Router

  - [1](https://tylermcginnis.com/react-router-pass-props-to-components/)
  - [2](https://til.hashrocket.com/posts/z8cimdpghg-passing-props-down-to-react-router-route)
  - [3](https://stackoverflow.com/questions/45598854/passing-props-through-react-router-v4-link)

- Nested Routes:

  - [nested routes](https://stackoverflow.com/questions/41474134/nested-routes-with-react-router-v4-v5)

- [dynamic Route path match](https://github.com/ReactTraining/react-router/blob/master/packages/react-router/docs/api/match.md)
- [match for classes](https://stackoverflow.com/questions/44634461/react-router-how-to-pass-match-object-into-a-component-declared-as-an-es6-cl)

state persistance:

1. using react router, route automatically persisted on refresh
2. if no react-router, refresh would reload state and current react "page". use [state persistence](https://reactnavigation.org/docs/en/state-persistence.html)

Code Splitting:

- [1](https://facebook.github.io/create-react-app/docs/code-splitting)
- [2](https://serverless-stack.com/chapters/code-splitting-in-create-react-app.html)
