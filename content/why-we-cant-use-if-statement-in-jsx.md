+++
title = "Why We Can't Write If Statement In JSX"
author = "M. Nindra Zaka"
date = 2020-11-28T05:00:00Z
type = "post"
+++

{{< figure src="/images/post/why-we-cant-use-if-statement-in-jsx.jpg" caption="Photo by [Bruce Mars](https://unsplash.com/photos/xj8qrWvuOEs)" >}}

I remember the time when i want to show an element in jsx based on condition. At that time, i am still new to React and JSX, i started to think "Ok, i want to show this element based on condition, i think i am gonna use if statement". So, my component will look like this : 

{{< highlight jsx "linenos=table,hl_lines=4"  >}}
const Toggle = () => {
  const [on, setOn] = React.useState(false)
  const toggle = () => setOn(!on)
  return <button onClick={toggle}>the button is { if (on) return 'on' else return 'off' }</button>
  )
}
{{< /highlight >}}

Focus on line 6, on that line, i want to show "on" text if `on` state is `true`, if not then i want to show "off" text
but, after i create an if statement in component return, i start to get an error message like this

```jsx
Unexpected token (4:50)
```

Why i get that error ? because it's technically correct. I want to show that element based on its condition. So, why i get that error ? Ok, lets refactor that if statement to ternary operator

{{< highlight jsx "linenos=table" >}}
const Toggle = () => {
  const [on, setOn] = React.useState(false)
  const toggle = () => setOn(!on)
  return <button onClick={toggle}>the button is { on ? 'on' : 'off' }</button>
}
{{< /highlight >}}

When i try it and see the result, i notice that it is working correctly, but why ? it is using the same logic, i just change if statement to ternary and it is magically working. I was confuse and stopped questioning that problem by just using ternary operator. After a long time i get the reason from kent c dodds in epic react

### JSX is just sugar syntax to React.createElement()

JSX is just a sugar syntax to help us write `React.createElement()` faster. So if we write jsx code like this

{{< highlight jsx "linenos=table" >}}
<div id="message">hello world</div>
{{< /highlight >}}

It will get translated by babel to become like this

{{< highlight javascript "linenos=table" >}}
React.createElement("div", { id: "message", children: "hello world" })
{{< /highlight >}}

If you understand what i explained, continue your read, but if you confuse, you can read my detail explanation about [jsx](https://mnindrazaka.com/how-to-enter-jsx-world-smoothly/)

### If statement in JSX

Ok, lets get back to the problem, we want to know why we can't use if statement in JSX. Lets write some component using JSX with if statement within it

{{< highlight jsx "linenos=table" >}}
const Toggle = () => {
  const [on, setOn] = React.useState(false)
  const toggle = () => setOn(!on)
  return <button onClick={toggle}>the button is { if (on) return 'on' else return 'off' }</button>
}
{{< /highlight >}}

Now, lets think how it will get translated into `React.createElement` syntax, so it will get translated into this

{{< highlight jsx "linenos=table" >}}
const Toggle = () => {
  const [on, setOn] = React.useState(false);
  const toggle = () => setOn(!on);
  return React.createElement("button", {
    onClick: toggle
  }, "the button is ", if (on) return 'on' else return 'off');
};
{{< /highlight >}}

See the problem ? no ? ok, i will help you. The problem is that the if statement will be written to the `React.createElement` function parameter. And technically, we can't write if statement as function parameter. Go try it if you want to proof it.

### Ternary operator in JSX

Ok, now lets try if we use ternary operator. Here is the same component but using ternary operator instead of if statement

{{< highlight jsx "linenos=table" >}}
const Toggle = () => {
  const [on, setOn] = React.useState(false)
  const toggle = () => setOn(!on)
  return <button onClick={toggle}>the button is { on ? 'on' : 'off' }</button>
}
{{< /highlight >}}

Now, lets translate it into `React.createElement`

{{< highlight jsx "linenos=table" >}}
const Toggle = () => {
  const [on, setOn] = React.useState(false);
  const toggle = () => setOn(!on);
  return React.createElement("button", {
    onClick: toggle
  }, "the button is ", on ? 'on' : 'off');
};
{{< /highlight >}}

You will see that after we translate it, we will use ternary operator in `React.createElement` function parameter. And, technically, we can use ternary operator as function parameter. Again, try it if you want to proof it

### Summary

The summary is, we cant use if statement in jsx because that if statement will be written as function parameter of `React.createElement`, and we cant do that. This is the same reason why we can't use `switch case` and `for` statement inside jsx. Hopefully my explanation will help you to understand why we cant write if statement in JSX