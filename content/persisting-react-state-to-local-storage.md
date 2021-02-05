+++
title = "Persisting React State to Local Storage"
author = "M. Nindra Zaka"
date = 2021-01-17T05:00:00Z
type = "post"
description = "How i persist React state to local storage and keep them sync"
+++

{{< figure src="/images/post/persisting-react-state-to-local-storage.jpeg" caption="Photo by [Monirul Islam Shakil](https://unsplash.com/photos/uuhD96VBp4k)" >}}

Last week, i develop a feature that need to persist some state into local storage. So, i will share you how i do it. But as a note, i will give you simple example instead of the actual code to simplify it so we can focus to the problem

Back to the persisting React state to local storage. The current approach to do it is something like this :

```jsx {linenos=table}
const ButtonCounter = () => {
  const defaultCounter = localStorage.getItem('counter')

  const [counter, setCounter] = React.useState(
    defaultCounter ? JSON.parse(defaultCounter) : 0
  )

  const handlePlusButtonClick = () => {
    setCounter(counter + 1)
    localStorage.setItem('counter', JSON.stringify(counter + 1))
  }

  const handleMinusButtonClick = () => {
    setCounter(counter - 1)
    localStorage.setItem('counter', JSON.stringify(counter - 1))
  }

  return (
    <>
      <p>counter : ${counter}</p>
      <button onClick={handlePlusButtonClick}>+</button>
      <button onClick={handleMinusButtonClick}>-</button>
    </>
  )
}
```

There is a Button Counter component that can show the current counter it has using state. It also can increase and decrease its counter by clicking the plus and minus button. When we click the increase or decrease button, we will set the local storage item with the same value of the state

So far, there are no problem that will cause this code become buggy. But, how about if we forget to set the local storage item or we set it with the wrong value ? the value of the state and the local storage will not sync and will be difference. This can easly happen if our component getting bigger, the number of state that we maintain is increasing. It happens because we need to "manually" change the local storage after we change the state.

### Keep state and local storage sync using useEffect

To solve the previous problem, we need to find a way to automatically change the local storage item whenever the state is changes. Luckly, there is a way to do it using React useEffect hook. We just need to move the `localStorage.setItem` into useEffect :

```jsx {linenos=table}
const ButtonCounter = () => {
  const defaultCounter = localStorage.getItem('counter')

  const [counter, setCounter] = React.useState(
    defaultCounter ? JSON.parse(defaultCounter) : 0
  )

  React.useEffect(() => {
    localStorage.setItem('counter', JSON.stringify(counter))
  }, [counter])

  const handlePlusButtonClick = () => {
    setCounter(counter + 1)
  }

  const handleMinusButtonClick = () => {
    setCounter(counter - 1)
  }

  return (
    <>
      <p>counter : ${counter}</p>
      <button onClick={handlePlusButtonClick}>+</button>
      <button onClick={handleMinusButtonClick}>-</button>
    </>
  )
}
```

As you can see, whenever the counter state is changed, it will run the side effect that will persist the counter state to local storage. And boom, we dont need to bother changing the local storage manually, we just need to focus to the state, and useEffect will do its job to save the state to local storage

### Even better, make it to custom hook

To simplify our implementation and make it reusable on other component, we can make a custom hook called `useLocalStorageState` that can persist any state into local storage. It receive key for the local storage and initial value for the state :

```jsx {linenos=table}
const useLocalStorageState = (key, initialValue) => {
  const defaultValue = localStorage.getItem(key)
  const [state, setState] = React.useState(
    defaultValue ? JSON.parse(defaultValue) : initialValue
  )

  React.useEffect(() => {
    if (state !== undefined) localStorage.setItem(key, JSON.stringify(state))
  }, [key, state])

  return [state, setState]
}

const ButtonCounter = () => {
  const [counter, setCounter] = useLocalStorageState('counter', 0)

  const handlePlusButtonClick = () => {
    setCounter(counter + 1)
  }

  const handleMinusButtonClick = () => {
    setCounter(counter - 1)
  }

  return (
    <>
      <p>counter : ${counter}</p>
      <button onClick={handlePlusButtonClick}>+</button>
      <button onClick={handleMinusButtonClick}>-</button>
    </>
  )
}
```

Using this hook, we can persist any state that we want to local storage, without even touching the `localStorage` API, cool ! So, that how i do it, thanks
