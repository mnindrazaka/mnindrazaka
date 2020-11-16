+++
title = "How to Enter JSX World Smoothly"
image = "/images/post/how-to-enter-jsx-world-smoothly.jpg"
author = "M. Nindra Zaka"
date = 2020-11-09T05:00:00Z
description = "This is meta description"
# categories = ["Meta Data"]
type = "post"

+++

One problem that has to be faced by the new engineer when learning about React for the first time is understanding what is JSX and why we need it. When someone asks me to teach them about react, I always hard to explain JSX

Last week, I join the Epic React course created by Kent C Dodds. He explained how we can smoothly move from HTML to JSX, and he did an awesome job explaining it. After I finish the React fundamental topic, now I clearly understand what is JSX and why we need that. Also, if anyone asks me about JSX, I will never confuse about how to explain it to them

So, when you come to me and ask me about JSX. This is how I will explain it

### Todays, HTML is generated in the browser by javascript

Back in the old days, HTML is generated on the server-side and sent to the client browser. So that the document sent by the server is containing complete HTML structure

But today, the server is just sending a prototype structure of HTML, and let the frontend framework generate the complete HTML structure on the client-side

### Programmatically add HTML element with javascript

If the server just sending a prototype structure of HTML, so in the client, we can generate the complete structure with javascript. So, how can we do that? We can simply use these javascript DOM API to create a new element

```html
<script>
const div = document.createElement('div')
</script>
```

After we create an element, we can add it to the browser like this

```javascript
document.body.append(div);
```

It is so simple. We just create an element using `document.createElement` and add it to browser using `document.body.append`. Here is the full HTML code of what we did 

```javascript
const div = document.createElement('div')
document.body.append(div);
```

### Let's do it in React way

Let's get into React, before we use JSX, we can create an element using purely react API. Like this

```javascript
const div = React.createElement('div')
```

And then, we can render the element that we created like this

```javascript
ReactDOM.render(div);
```

So, without JSX, we can create an element and render it to the browser. Here is the full code

```javascript
const div = React.createElement('div')
ReactDOM.render(div);
```

### Tired of writing many syntaxes? you can use JSX instead

From the previous point, we already know that we can create an element using `React.createElement`. But, imagine that we need to build a full website layout using that function. That will make us extremely tired. So JSX comes to the rescue. Instead of using `React.createElement`, we can use JSX syntax like this 

```html
<div></div>
```


But, how can our browser understand that syntax? because basically, we write an HTML syntax in javascript. Don't worry, that's why we need babel to translate this

```html
<div></div>
```

into this

```javascript
React.createElement('div')
```

### Summary

That's what I get from epic react. I love how kent explains JSX. That makes the barrier to understand JSX breakable. And we don't realize that we already learn React and JSX

Want to know more? just go to the Epic React website and purchase the course. It's totally worth it