+++
title = "Why We Can't Write If Statement In JSX"
author = "M. Nindra Zaka"
date = 2020-11-28T05:00:00Z
type = "post"

+++

{{< figure src="/images/post/why-we-cant-write-if-statement-in-jsx.jpg" caption="Photo by [Bruce Mars](https://unsplash.com/photos/xj8qrWvuOEs)" >}}

I remember the time when i want to show some element in jsx based on condition. Naturally, if we want to make a condition, we will use if statement. So, our component will look like this : 

{{< highlight jsx "linenos=table" >}}
const Toggle = () => {
  const [on, setOn] = React.useState(false)
  
  const toggle = () => setOn(!on)

  return (
    <div>
      <p>the button is { if (on) { return 'on' } else { return 'off' } }</p>
      <button onClick={toggle}>toggle</button>
    </div>
  )
}
{{< /highlight >}}

But, after we create an if statement in component return, we start to get some error like this

```jsx
Unexpected token (8:25)
```

Why we get that error ? because it's technically correct. We want to show that element based on its condition. So, why we get that error ? Ok, lets refactor that if statement using ternary operator

{{< highlight jsx "linenos=table" >}}
const Toggle = () => {
  const [on, setOn] = React.useState(false)
  
  const toggle = () => setOn(!on)

  return (
    <div>
      <p>the button is { on ? 'on' : 'off' }</p>
      <button onClick={toggle}>toggle</button>
    </div>
  )
}
{{< /highlight >}}

If we try its in code editor, we will notice that its working correctly, but why ? its the same logic, we just change if statement to ternary and its magically working. So here is the explanation that i get from epic react

### We have to know how JSX works

Before we discuss about why we cant use if statemet in jsx, we have to know [what is jsx and why we need it](https://mnindrazaka.netlify.app/how-to-enter-jsx-world-smoothly/). If you feel tired to read it, here is my summary to help you

JSX is sugar syntax to help us write `React.createElement()`. So if we write jsx code like this

{{< highlight jsx "linenos=table" >}}
<div>lorem</div>
{{< /highlight >}}

It will get translated by babel to become like this

{{< highlight javascript "linenos=table" >}}
React.createElement("div", { children: "lorem" })
{{< /highlight >}}

If you understand what i explained, continue your read, but if you confuse, you can read my detail explanation about jsx

### If statement in JSX

Ok, lets get back to the problem, we want to know why we can't use if statement in JSX. Lets write some component using JSX with if statement within it

{{< highlight jsx "linenos=table" >}}
const Toggle = () => {
  const [on, setOn] = React.useState(false)
  
  const toggle = () => setOn(!on)

  return (
    <div>
      <p>the button is { if (on) { return 'on' } else { return 'off' } }</p>
      <button onClick={toggle}>toggle</button>
    </div>
  )
}
{{< /highlight >}}

Now, lets think how it will get translated into `React.createElement` syntax, so it will get translated into this

{{< highlight jsx "linenos=table" >}}
const Toggle = () => {
  const [on, setOn] = React.useState(false);

  const toggle = () => setOn(!on);

  return React.createElement("div", null,React.createElement("p", null, "the button is ", if(on) { return 'on' } else { return 'off' }), React.createElement("button", {
    onClick: toggle
  }, "toggle"));
};
{{< /highlight >}}

See the problem ? no ? ok, i will help you. The problem is that if statement will write to the `React.createElement` function parameter. And technically, there are no syntax that can't make us write if statement as function parameter. Go try it if you want to proof it. So, this is the problem

### Ternary operator in JSX

Ok, now lets try if we use ternary statement. Here is the example

{{< highlight jsx "linenos=table" >}}
const Toggle = () => {
  const [on, setOn] = React.useState(false)
  
  const toggle = () => setOn(!on)

  return (
    <div>
      <p>the button is { on ? 'on' : 'off' }</p>
      <button onClick={toggle}>toggle</button>
    </div>
  )
}
{{< /highlight >}}

Now, lets translate it into `React.createElement`

{{< highlight jsx "linenos=table" >}}
const Toggle = () => {
  const [on, setOn] = React.useState(false);

  const toggle = () => setOn(!on);

  return React.createElement("div", null,React.createElement("p", null, "the button is ", on ? 'on' : 'off'), React.createElement("button", {
    onClick: toggle
  }, "toggle"));
};
{{< /highlight >}}

You will see that after we translate it, we will use ternary operator in `React.createElement` function parameter. And, technically, we can use ternary operator as function parameter. Again, try it if you want to proof it


### Summary

The summary is, we cant use if statement in jsx because that if statement will be translated and write as function parameter of `React.createElement`, and we cant do that. Hopefully my explanation will help you to understand why we cant write if statement in JSX