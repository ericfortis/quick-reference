# React Lifecycle

## Mounting
1. [constructor(props)](https://reactjs.org/docs/react-component.html#constructor)
2. [**static getDerivedStateFromProps(props, state)**](https://reactjs.org/docs/react-component.html#static-getderivedstatefromprops)
3. [**render()**](https://reactjs.org/docs/react-component.html#render)
4. **React updates DOM and refs**
5. [componentDidMount()](https://reactjs.org/docs/react-component.html#componentdidmount)

## Updating
1. Cause:
  - Different props
  - [setState(updaterFn, callback)](https://reactjs.org/docs/react-component.html#setstate)
  - [forceUpdate(callback)](https://reactjs.org/docs/react-component.html#forceupdate)
2. [**static getDerivedStateFromProps(props, state)**](https://reactjs.org/docs/react-component.html#static-getderivedstatefromprops)
  - We use it to handle, Undo, EntryValue numeric formatting, EntryTitle, etc., Sliders
3. [shouldComponentUpdate(nextProps, nextState)](https://reactjs.org/docs/react-component.html#shouldcomponentupdate)
  - it's not fired if it's from forceUpdate()
4. [**render()**](https://reactjs.org/docs/react-component.html#render)
5. [getSnapshotBeforeUpdate(prevProps, prevState)](https://reactjs.org/docs/react-component.html#getsnapshotbeforeupdate)
  -  "Pre-commit phase" can read the DOM.
6. **React updates DOM and refs**
7. [componentDidUpdate(prevProps, prevState, snapshot)](https://reactjs.org/docs/react-component.html#componentdidupdate)

## Unmounting
1. [componentWillUnmount()](https://reactjs.org/docs/react-component.html#componentwillunmount)

## Error
1. [static getDerivedStateFromError()](https://reactjs.org/docs/react-component.html#static-getderivedstatefromerror)
2. [componentDidCatch(error, info)](https://reactjs.org/docs/react-component.html#componentdidcatch)



## Notes
- Instead of Controlled vs. Uncontrolled component I would call it "State Controlled Component".
- Source: https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/
