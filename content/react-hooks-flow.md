+++
title = "React Hooks Flow"
author = "M. Nindra Zaka"
date = 2020-12-15T05:00:00Z
type = "post"
description = "Understanding React hooks flow"
+++

{{< figure src="/images/post/react-hooks-flow.jpg" caption="Photo by [Mike Lewis HeadSmart Media](https://unsplash.com/photos/waAAaeC9hns)" >}}

Before we enter the world of function components, we have a class component instead. On that type of component, we have the component lifecycle method. That makes us understand what is going on in our component. So, here is the order of lifecycle method in a component :

{{< figure src="/images/post/component-lifecycle-functions.png" caption="Photo by [Rangle](https://rangle.github.io/react-training/react-lifecycles/)" >}}

### Mount Phase :

Mount phase is the phase that happens when a component is rendered for the first time. Here is what happens when a component is mounting :

#### 1. componentWillMount

Before React render the component, it will run a special method called `componentWillMount`

#### 2. render

React will run the render method in the component to get React object component, compare the result with the previous object and get the difference, after that, it will apply the difference to the DOM and paint it to the screen

#### 3. componentDidMount

The component is mounted, and showed on the screen, after that, React will run a special method called `componentDidMount`

### Update Phase :

Update phase happens when a component is rerendered, whatever it rerendered because of changes of the props, changes of the state, or because the parent component is rerendered. Here is what happens when a component is updating

#### 1. componentWillReceiveProps

When the component starts to update, it will run a method called `componentWillReceiveProps`, this step is where the component receives the next props that will be given on the next render

#### 2. shouldComponentUpdate

Before actually update the component, React will run a method called `shouldComponentUpdate`, in this method, we have to return a boolean that indicates whether the component should be an update or not. So, basically, we can cancel component update in this method

#### 3. componentWillUpdate

After we decide to update the component in `shouldComponentUpdate`, React will run a method called `component will update`, this method will be run before the update actually happen

#### 4. render

React will run the render method in the component to get React object component, compare the result with the previous object and get the difference, after that, it will apply the difference to the DOM and paint it to the screen

#### 5. componentDidUpdate

So the component is updated and we can see the changes on the screen. After the component updated, React will run a method called `componentDidUpdate`, so we can do something after a component updated

### Unmount Phase :

Unmount phase happens when a component is removed from DOM. Here is what happens

#### 1. componentWillUnmount

Before react remove the component from virtual dom, it will run a special method called `componentWillUnmount`

The method name is so clear to explain the lifecycle of a component when it is in the mount, update, and unmount phase. But how about in function component? does the function component has a lifecycle? well, let's talk about it

### What is Hook Flow

Function component has "lifecycle" too, but we do not call it "lifecycle", instead, we call it hooks flow. Function components still have the same phase as the class component, but instead of calling the lifecycle method on each phase, the function component will call useEffect and useLayoutEffect hook on each phase. So let's explain it one by one

{{< figure src="/images/post/hook-flow.png" caption="Photo by [donavon](https://github.com/donavon/hook-flow)" >}}

### Mount Phase

Mount phase is the phase that happens when a component is rendered for the first time. Here is what happens when a component is mounting :

#### 1. Run lazy initializers

At this step, we run lazy initializers, the function that we pass to `useState` and `useReducer`

#### 2. Render

React will render the component that we create, and doing [reconcilation](https://reactjs.org/docs/reconciliation.html) to know what is the difference between React component object and the DOM

#### 3. React Update DOM

After React know what is the difference between the component object and the DOM, React will commit that difference to the DOM

#### 4. Run Layout Effect

Layout effect is the function that we write in `useLayoutEffect`. So this function will be called before the browser paint the changes that we made to the screen

#### 5. Browser Paint Screen

After running the layout effect, the browser will paint the changes that we made to the screen

#### 6. Run Effect

The screen is changed, now is the time to run the function that we write in `useEffect`

### Update Phase

Update phase happens when a component is rerendered, whatever it rerendered because of changes of the props, changes of the state, or because the parent component is rerendered. Here is what happens when a component is updating

#### 1. Render

React will render the component function to get the component object and doing [reconcilation](https://reactjs.org/docs/reconciliation.html) to know what is the difference between React component object and the DOM

#### 2. React Update DOM

After React know what is the difference between the component object and the DOM, React will commit that difference to the DOM

#### 3. Cleanup Layout Effect

At the update phase, before we run the layout effect, we have to clean up the layout effect by running the function that we returned at `useLayoutEffect`

#### 4. Run Layout Effect

Layout effect is the function that we write in `useLayoutEffect`. So this function will be called before the browser paint the changes that we made to the screen

#### 5. Browser Paint Screen

After running the layout effect, the browser will paint the changes that we made to the screen

#### 6. Cleanup Effect

At the update phase, before we run effect, we have to clean up the effect by running the function that we returned at `useEffect`

#### 7. Run Effect

The screen is changed, now is the time to run the function that we write in `useEffect`

### Unmount Phase

Unmount phase happens when a component is removed from DOM. Here is what happens

#### 1. Cleanup Layout Effect

Before the component removed, React will run the cleanup layout effect by running the returned function inside `useLayoutEffect`

#### 2. Cleanup Effect

After running the layout effect, React will run the cleanup effect by running the returned function inside `useEffect`
