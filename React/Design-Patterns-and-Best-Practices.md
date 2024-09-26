# React 18 Design Pattern and Best Practices

## Design, build and deploy production-ready web applications with React by leveraging industry-best practices

### Written by Carlos Santana Roldan

## What is React and why Meta(ex. Facebook) engineers made it?

As mentioned in react docs, React is a `JavaScript library` for rendering `user interfaces (UI)`. But the main question here is what issues made meta engineers think of a new way?
There were several issues that started the Idea:

1. `Inefficient DOM Manipulation`:

- Problem:
  Facebook's web application were becoming `increasingly dynamic and complex` leading to frequent updates to the Document Object Model (DOM).<br/>
  Direct manipulation of the DOM is slow and can lead to performance bottlenecks.

- Solution:
  React introduced the `Virtual DOM` to optimize and manage the updates more efficiently. By comparing the current and previous states of the Virtual DOM, React can batch updates and apply only the necessary changes to the real DOM, improving performance significantly.

2. `Component Reusability`:

- Problem:
  The lack of a component-based architecture made it `hard to reused code`. Developers often had to `duplicate codes` across different parts of the application leading to `maintenance issues`.

- Solution:
  React component-based architecture allowed engineers to `encapsulate UI logic within individual components`. Each component is `self-contained`, managing it's `own state and rendering`, which simplifies the `development and maintenance`.

3. `Declarative vs. Imperative`:

- Problem:
  Imperative programming model made developers `explicitly specify steps to update the UI`, led to code that was `difficult to read`, `understand` and `debug`.

- Solution:
  React's declarative programming model allow developers to describe what the UI should look like for a given state, then React `handle the underlying updates`, making the code `more predictable` and `easier to read and debug`.

4. `State Synchronization`:

- Problem:
  Synchronizing state across different parts of the application was `challenging`. When the state changed in one part of the application, ensuring that all affected parts of the UI were `updated correctly was difficult and error-prone`.

- Solution:
  React’s `unidirectional data flow` and component-based model make it easier to manage and synchronize state changes. By passing data down through `props` and using `state within components`, React ensures that the `UI stays consistent with the underlying data`.

## Differentiating between declarative and imperative programming

One of the reasons why React is so powerful is that it enforces a `declarative programming paradigm`.

The easiest way to approach this is to think about **imperative programming** as a `way of describing how things work`, and **declarative programming** as a `way of describing what you want to achieve`.

### **Imperative approach**

```js
const toUpperCase = (input) => {
  const output = [];

  for (let i = 0; i < input.length; i++) {
    output.push(input[i].toUpperCase());
  }
  return output;
};
```

### **Declarative approach**

```js
const toUpperCase = (input) => input.map((word) => word.toUpperCase());
```

Imagine a simple UI component such as a toggle button. When you click it, it turns green (on) if it was previously gray (off), and switches to gray (off) if it was previously green (on).
The imperative way of doing this would be as follows:

```js
// imperative
const toggleButton = document.querySelector('#toggle')
toggleButton.addEventListener('click', () => {
  if (toggleButton.classList.contains('on')) {
    toggleButton.classList.remove('on')
    toggleButton.classList.add('off')
  } else {
    toggleButton.classList.remove('off')
    toggleButton.classList.add('on')
  }
})

// declarative
// To turn on the Toggle
<Toggle on />
// To turn off the toggle
<Toggle />
```

## How React elements work

Elements are `lightweight immutable descriptions` of what should be rendered, while components are `more complex stateful objects` responsible for generating elements.

As mentioned previously, React follows a declarative paradigm, and there’s no need to tell it how to interact with the DOM; you declare what you want to see on the screen, and React does the job for you. One of the tools that makes this process `more expressive and readable is JSX`, which allows you to write `HTML-like syntax `directly in your JavaScript code.

## JSX

### **Root**

One important note worth mentioning is that since JSX elements get `translated into JavaScript functions`, and `you cannot return two functions in JavaScript`, whenever you have multiple elements at the same level, you are forced to wrap them in a parent.

In this example we get error:

```js
<div />
<div />
```

**Adjacent JSX elements must be wrapped in an enclosing tag.**

We can use a div wrapper to get rid of error but it makes us an unnecessary DOM element:

```js
<div>
  <div />
  <div />
</div>
```

The best way is to use `Fragment`:

```js
import { Fragment } from "react";
return (
  <Fragment>
    <h1>An h1 heading</h1>
    Some text here.
    <h2>An h2 heading</h2>
    More text here. Even more text here.
  </Fragment>
);
```

Fragment `won’t render anything visible on the DOM`

We can also use `empty tags(<></>)`:

```js
return (
  <>
    <h1>An h1 heading</h1>
    Some text here.
    <h2>An h2 heading</h2>
    More text here. Even more text here.
  </>
);
```

**_They are the same but with Fragment we can have `key` on the element_**

```js
function Blog() {
  return posts.map((post) => (
    <Fragment key={post.id}>
      <PostTitle title={post.title} />
      <PostBody body={post.body} />
    </Fragment>
  ));
}
```

### JSX rendering patters

#### Multiline

Whenever we have nested elements, we should always go multiline:

```js
<div>
  <Header />
  <div>
    <Main content={...} />
  </div>
</div>
```

Instead of following:

```js
<div><Header /><div><Main content={...} /></div></div>
```

**The exception is if the children are `not elements` such as `text` or `variables`. In that case, it makes sense to remain on the same line and avoid adding noise to the markup, as follows:**

```js
<div>
  <Alert>{message}</Alert>
  <Button>Close</Button>
</div>
```

#### Multi-properties

A common pattern is to write each attribute on a new line, with one level of indentation, and
then align the closing bracket with the opening tag:

```js
<button
  foo="bar"
  veryLongPropertyName="baz"
  onSomething={this.handleSomething}
/>
```

## Functional Programming

### First-class functions(HoFs):

JavaScript has first-class functions because they are treated like any other variable, meaning you can `pass a function as a parameter` to other functions, or it can be `returned by another function` and be assigned as a value to a variable.

```js
const add = (x, y) => x + y;
const log =
  (fn) =>
  (...args) => {
    return fn(...args);
  };
const logAdd = log(add);
logAdd(2, 3); // returns 5
```

### Purity:

A function is pure when there are `no side effects`, which means that the `function does not change anything that is not local to the function itself`.

```js
// pure function
const add = (x, y) => x + y;

// impure function - Running add(1) twice, we get two different results. The first time we get 1, but the second time we get 2
let x = 0;
const add = (y) => (x = x + y);
```

### Immutability:

We have seen how to write pure functions that don’t mutate the state, but what if we need to change the value of a variable? In FP, a function, instead of changing the value of a variable, `creates a new variable with a new value and returns it`.

This way of working with data is called `immutability`. An immutable value is a `value that cannot be changed`.

```js
// It changes value of the given array
const add3 = (arr) => arr.push(3);
const myArr = [1, 2];
add3(myArr); // [1, 2, 3]
add3(myArr); // [1, 2, 3, 3]

// It doesn't change the given array and returns a new one
const add3 = (arr) => arr.concat(3);
const myArr = [1, 2];
const result1 = add3(myArr); // [1, 2, 3]
const result2 = add3(myArr); // [1, 2, 3]
```

### Currying:

Currying is the process of converting a function that takes `multiple arguments` into a function `one argument at a time` and `returning another function`.

```js
const add = (x, y) => x + y;

// Currying
const add = (x) => (y) => x + y;
const add1 = add(1);
add1(2); // 3
add1(3); // 4
```

### Composition:

Functions (and components) `can be combined to produce new functions with more advanced features and properties`.

```js
const add = (x, y) => x + y;
const square = (x) => x * x;

// Composition add & square function
const addAndSquare = (x, y) => square(add(x, y))
```