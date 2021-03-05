+++
title = "Optimize Large List Using Windowing"
author = "M. Nindra Zaka"
date = 2021-03-03T05:00:00Z
type = "post"
description = "Learn how to optimize render performance of large list using windowing"
+++

{{< figure src="/images/post/optimize-large-list-using-windowing.jpeg" caption="Photo by [Marc-Olivier Jodoin](https://unsplash.com/photos/NqOInJ-ttqM)" >}}

### The Problems

There is a condition that we need to render a large amount of data into a list, for example, 10.000 data. It will take a little time before the items show in the browser. Try to hit the show button below and notice that there is a delay before the data show

<iframe src="https://codesandbox.io/embed/snowy-waterfall-0m8nx?fontsize=14&hidenavigation=1&theme=dark"
  style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
  title="mnindrazaka-render-large-list"
  allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
  sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
></iframe>

Did you notice the delay ? This happens because the browser need to create 10.000 DOM node and put them into document. The amount of data is too much that cause the browser busy to render it. 

So, we need to reduce the number of data that we render. For example, if you change the data to only 1.000, it will no delay again. Try to change the number of data to only 1.000 and hit the show button

Ok, the previous solution is working, we not allow to render so much data. But, in this case, we still need to render 10.000 data, not only 1.000. How can we solve that ? the solution is, using windowing

### What is Windowing

Windowing is a technique to optimize the rendering performance of large list by only showing item that visible in the window. When user scroll, it will render the next item. For example, if we need to render 10.000 items, the browser will only render the items that can be seen on the screen, for example 10 items. As user scrolling, that items will be changed with the next items

### How is The Comparison

To implement the windowing, i use [react-window](https://github.com/bvaughn/react-window). This is the comparison

<iframe src="https://codesandbox.io/embed/mnindrazaka-react-window-7qc2q?fontsize=14&hidenavigation=1&theme=dark"
     style="width:100%; height:500px; border:0; border-radius: 4px; overflow:hidden;"
     title="mnindrazaka-react-window"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>

### The Disadvantage

Because the item is rendered when we scrolling, sometimes it will show flashy feel, because there is a chance that the items still in progress of rendering as we scroll down

### Summary

We can use windowing to improve the rendering performance of large list. But, we only need to use it when we want to render a large amount of data.
