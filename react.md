# React Lifecycle

## Mounting
- [constructor(props)](https://reactjs.org/docs/react-component.html#constructor)
- [**static getDerivedStateFromProps(props, state)**](https://reactjs.org/docs/react-component.html#static-getderivedstatefromprops)
- [**render()**](https://reactjs.org/docs/react-component.html#render)
- **React updates DOM and refs**
- [componentDidMount()](https://reactjs.org/docs/react-component.html#componentdidmount)

## Updating
- Cause:
  - Different props
  - [setState(updaterFn, callback)](https://reactjs.org/docs/react-component.html#setstate)
  - [forceUpdate(callback)](https://reactjs.org/docs/react-component.html#forceupdate)
- [**static getDerivedStateFromProps(props, state)**](https://reactjs.org/docs/react-component.html#static-getderivedstatefromprops)
  - We use it to handle:
    - Undo
    - EntryValue numeric formatting, EntryTitle, etc.
    - Sliders
- [shouldComponentUpdate(nextProps, nextState)](https://reactjs.org/docs/react-component.html#shouldcomponentupdate)
  - it's not fired if it's from forceUpdate()
- [**render()**](https://reactjs.org/docs/react-component.html#render)
- [getSnapshotBeforeUpdate(prevProps, prevState)](https://reactjs.org/docs/react-component.html#getsnapshotbeforeupdate)
  -  "Pre-commit phase" can read the DOM.
- **React updates DOM and refs**
- [componentDidUpdate(prevProps, prevState, snapshot)](https://reactjs.org/docs/react-component.html#componentdidupdate)

## Unmounting
- [componentWillUnmount()](https://reactjs.org/docs/react-component.html#componentwillunmount)

## Error
- [static getDerivedStateFromError()](https://reactjs.org/docs/react-component.html#static-getderivedstatefromerror)
- [componentDidCatch(error, info)](https://reactjs.org/docs/react-component.html#componentdidcatch)


### Source
https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/


## Concepts
- React devs badly named Controlled vs. Uncontrolled component.
A better name would have just been saying "State Controlled Component".