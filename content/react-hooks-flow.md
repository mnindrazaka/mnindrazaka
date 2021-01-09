+++
title = "React Hooks Flow"
author = "M. Nindra Zaka"
date = 2020-12-15T05:00:00Z
type = "post"
description = "Understanding React hooks flow"
+++

{{< figure src="/images/post/react-hooks-flow.jpg" caption="Photo by [Mike Lewis HeadSmart Media](https://unsplash.com/photos/waAAaeC9hns)" >}}

Before we enter the world of function component, we have class component instead. On that type of component, we have component lifecycle method. Thats make us understand whats going on in our component. So, here is the order of lifecycle method in a component :

### Mount Phase :

Mount phase is the phase that happen when component is rendered at the first time. Here is whats happen when a component is mounting :

#### 1. componentWillMount

Before React render the component, it will run special method called `componentWillMount`

#### 2. render

After that, React will run render method in the component to get React object component, compare the result with previous object and get the difference, after that, it will apply the difference to the DOM and paint it to the screen

#### 3. componentDidMount

Component is mounted, and showed in the screen, after that, React will run special method called `componentDidMount`

Update Phase :

1. componentWillReceiveProps
2. shouldComponentUpdate
3. componentWillUpdate
4. render
5. componentDidUpdate

Unmount Phase :

1. componentWillUnmount

The method name is so clear to explain the lifecycle of a component when its in mount, update, and unmount phase. But how about in function component ? does function component has lifecycle ? well, lets talk about it

### What is Hook Flow

Function component has "lifecycle" too, but we not call it "lifecycle", instead, we call it hook flow. Function component still have the same phase like class component, but instead off calling lifecycle method on each phase, function component will call useEffect and useLayoutEffect hook on each phase. So lets explain it one by one

{{< figure src="/images/post/hook-flow.png" caption="Photo by [donavon](https://github.com/donavon/hook-flow)" >}}

### Mount Phase

Mount phase is the phase that happen when component is rendered at the first time. Here is whats happen when a component is mounting :

#### 1. Run lazy initializers

At this step, we run lazy initializers, the function that we pass to `useState` and `useReducer`

#### 2. Render

React will render the component that we create, and doing [reconcilation](https://reactjs.org/docs/reconciliation.html) to know what is the difference between React component object and the DOM

#### 3. React Update DOM

After React know what is the difference between component object and the DOM, React will commit that difference to the DOM

#### 4. Run Layout Effect

Layout effect is the function that we write in `useLayoutEffect`. So this function will be called before the browser paint the changes that we made to the screen

#### 5. Browser Paint Screen

After running layout effect, the browser will paint the changes that we made to the screen

#### 6. Run Effect

Screen is changed, now is the time to run function that we write in `useEffect`

### Update Phase

Update phase is happen when a component is rerendered, whatever its rerendered because of changes of the props, changes of the state, or because the parent component is rerendered. Here is what happen when a component is updating

#### 1. Render

React will render the component function to get the component object and doing [reconcilation](https://reactjs.org/docs/reconciliation.html) to know what is the difference between React component object and the DOM

#### 2. React Update DOM

After React know what is the difference between component object and the DOM, React will commit that difference to the DOM

#### 3. Cleanup Layout Effect

At update phase, before we run layout effect, we have to cleanup layout effect by running the function that we returned at `useLayoutEffect`

#### 4. Run Layout Effect

Layout effect is the function that we write in `useLayoutEffect`. So this function will be called before the browser paint the changes that we made to the screen

#### 5. Browser Paint Screen

After running layout effect, the browser will paint the changes that we made to the screen

#### 6. Cleanup Effect

At update phase, before we run effect, we have to cleanup effect by running the function that we returned at `useEffect`

#### 7. Run Effect

Screen is changed, now is the time to run function that we write in `useEffect`

### Unmount Phase

Unmount phase happen when component is removed from DOM. Here is whats happen

#### 1. Cleanup Layout Effect

Before component removed, React will run cleanup layout effect by running the returned function inside `useLayoutEffect`

#### 2. Cleanup Effect

After running layout effect, React will run cleanup effect by running the returned function inside `useEffect`
