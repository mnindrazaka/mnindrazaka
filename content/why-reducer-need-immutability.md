+++
title = "Why Reducer Need Immutability"
author = "M. Nindra Zaka"
date = 2021-03-05T05:00:00Z
type = "post"
description = "Understanding why reducer is not allowed to mutate the previous state and must return a new object"
+++

{{< figure src="/images/post/why-reducer-need-immutability.jpeg" caption="Photo by [Georg Bommeli](https://unsplash.com/photos/ybtUqjybcjE)" >}}

### Immutability

Here is the common way to write a reducer :

```javascript {linenos=table}
function booksReducer(prevState, action) {
  switch (action.type) {
    case 'add':
      return [...prevState, action.book];
    default:
      throw new Error();
  }
}
```

On that reducer, if we want to `add` a book, we need to create a new array that will spread previous books, and add a new one. So basically, it will create a new array. Why we dont just push the book object to the previous state ? for example :

```javascript {linenos=table}
function booksReducer(prevState, action) {
  switch (action.type) {
    case 'add':
      prevState.push(action.book)
      return prevState
    default:
      throw new Error();
  }
}
```

It will be so easy and to the point. So why we not do it ? The answer is, if we do it, the component that consume that reducer will not be rerendered. Because the component think that the state is not changed at all

### Why The Component Not Rerendered If We Just Change The Previous State ?

The component is not rerendered because they think that the state is not changed at all. This happens because component will check the previous state and the current state using `shallow equality check`, this means, it just compare the reference of the state. If it a new reference, so there is update on the state, but if it the same reference, so the state is not changed at all. Let see the comparison below :

```javascript {linenos=table}
function booksReducer(prevState, action) {
  switch (action.type) {
    case 'add':
      // This will create a new reference. Component will rerendered
      return [...prevState, action.book];

      // This will use the previous reference. Component will not rerendered
      prevState.push(action.book)
      return prevState

    default:
      throw new Error();
  }
}
```

### Then, Why the Component Use Shallow Equality Check ?

The next question is, why the component use shallow equality check to check is the state is new or not. Why dont just compare its value instead (deep equality check) ? Well, if the component need to check the value instead of the reference, it will take a longer time to determine if it a new state or not. Because it need to check every property of the state one by one. This will make the rerendering process longer. So thats why it just compare the reference instead of the value

### Summary 

The reducer need to return a new object because it using shallow equality check, its mean the check the reference of the state. If it new reference they will think that the state is updated. They use shallow equality check to make the checking process faster, so they dont need to compare the object value one by one by its property