+++
title = "Javascript Function .call() and .apply()"
author = "M. Nindra Zaka"
date = 2021-02-07T05:00:00Z
type = "post"
description = "Difference between .call() and .apply() in javascript function"
+++

{{< figure src="/images/post/javascript-function-call-and-apply.jpg" caption="Photo by [James Padolsey](https://unsplash.com/photos/fBn27VI9rgc)" >}}

### What is function .call()

Before we explain about "what is function .call()", lets take a look at following code

```javascript {linenos=table}
const book1 = {
  title: "Ego is the Enemy",
  author: "Ryan Holiday",
  printInfo: function () {
    console.log(
      "the book title is " + this.title + " and written by " + this.author
    );
  },
};

book1.printInfo(); // the book title is Ego is the Enemy and written by Ryan Holiday
```

We have an object called `book1`, it has `title` and `author`. Also, it has `printInfo` function that will log the info of the book to console, that function can be used to give information about the summary of the book

After that, we decide to create another object called `book2`, it also has `title` and `author` but with difference value. But, for some reason, it doesn't has `printInfo` function to log its info to console. But yet, we still want to do it

```javascript {linenos=table}
const book2 = {
  title: "Grit : The Power of Passion and Perseverance",
  author: "Angela Duckworth",
};
```

Since `book2` doesn't has `printInfo` function, how if we can "borrow" the `printInfo` function on `book1` and use it on `book2` ? Take a look at following code

```javascript {linenos=table}
const book1 = {
  title: "Ego is the Enemy",
  author: "Ryan Holiday",
  printInfo: function () {
    console.log(
      "the book title is " + this.title + " and written by " + this.author
    );
  },
};

const book2 = {
  title: "Grit : The Power of Passion and Perseverance",
  author: "Angela Duckworth",
};

book1.printInfo.call(book2); // the book title is Grit : The Power of Passion and Perseverance and written by Angela Duckworth
```

We can use another object's function and use it on difference object using `.call()`. We just need to put the object we want to use, for example `book2`

### .call() with arguments

We also can put arguments when using `.call()` function, here is the example :

```javascript {linenos=table}
const book1 = {
  title: "Ego is the Enemy",
  author: "Ryan Holiday",
  printInfo: function (store, price) {
    console.log(
      "the book title is " +
        this.title +
        " and written by " +
        this.author +
        ". You can buy it on " +
        store +
        " with " +
        price
    );
  },
};

book1.printInfo("amazon", "$10"); // the book title is Ego is the Enemy and written by Ryan Holiday. You can buy it on amazon with $10
```

```javascript {linenos=table}
const book1 = {
  title: "Ego is the Enemy",
  author: "Ryan Holiday",
  printInfo: function (store, price) {
    console.log(
      "the book title is " +
        this.title +
        " and written by " +
        this.author +
        ". You can buy it on " +
        store +
        " with " +
        price
    );
  },
};

const book2 = {
  title: "Grit : The Power of Passion and Perseverance",
  author: "Angela Duckworth",
};

book1.printInfo.call(book2, "amazon", "$10"); // the book title is Grit : The Power of Passion and Perseverance and written by Angela Duckworth. You can buy it on amazon with $10
```

### How about function .apply()

`.apply()` is similar with `.call()`, we can use it to use another function in an object to another object. The difference between `.apply()` and `.call()` is the way we pass an arguments to it, we use array in `.apply()`

```javascript {linenos=table}
const book1 = {
  title: "Ego is the Enemy",
  author: "Ryan Holiday",
  printInfo: function (store, price) {
    console.log(
      "the book title is " +
        this.title +
        " and written by " +
        this.author +
        ". You can buy it on " +
        store +
        " with " +
        price
    );
  },
};

const book2 = {
  title: "Grit : The Power of Passion and Perseverance",
  author: "Angela Duckworth",
};

book1.printInfo.call(book2, ["amazon", "$10"]); // the book title is Grit : The Power of Passion and Perseverance and written by Angela Duckworth. You can buy it on amazon with $10
```
