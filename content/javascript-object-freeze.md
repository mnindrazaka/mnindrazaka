+++
title = "Javascript Object.freeze() function"
author = "M. Nindra Zaka"
date = 2021-02-16T05:00:00Z
type = "post"
description = "Understanding what is Object.freeze() and why we need it"
+++

{{< figure src="/images/post/javascript-object-freeze.jpeg" caption="Photo by [Should Wang](https://unsplash.com/photos/eGa8CozDfVc)" >}}

```javascript {linenos=table}
let book = {
  title: "Ego is the Enemy",
  author: "Ryan Holiday",
};

// we can reassign the object
book = {
  title: "Grit: The Power of Passion and Perseverance",
  author: "Angela Duckworth",
};

// we can also reassign the object property
book.title = "Rich Dad Poor Dad";
```

### Try to use const to make object immutable

```javascript {linenos=table}
const book = {
  title: "Ego is the Enemy",
  author: "Ryan Holiday",
};

// we can't reassign the object
book = {
  title: "Grit: The Power of Passion and Perseverance",
  author: "Angela Duckworth",
};

// but we still can reassign the object property
book.title = "Rich Dad Poor Dad";
```

### Try to use Object.freeze() to make object immutable

```javascript {linenos=table}
let book = {
  title: "Ego is the Enemy",
  author: "Ryan Holiday",
};

Object.freeze(book);

// we can reassign the object
book = {
  title: "Grit: The Power of Passion and Perseverance",
  author: "Angela Duckworth",
};

// but we can't reassign the object property
book.title = "Rich Dad Poor Dad";
```

### Combine const and Object.freeze()

```javascript {linenos=table}
const book = {
  title: "Ego is the Enemy",
  author: "Ryan Holiday",
};

Object.freeze(book);

// we can't reassign the object
book = {
  title: "Grit: The Power of Passion and Perseverance",
  author: "Angela Duckworth",
};

// and we also can't reassign the object property
book.title = "Rich Dad Poor Dad";
```
