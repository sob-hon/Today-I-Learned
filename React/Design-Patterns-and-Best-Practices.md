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
  React’s `unidirectional data flo`w and component-based model make it easier to manage and synchronize state changes. By passing data down through `props` and using `state within components`, React ensures that the `UI stays consistent with the underlying data`.


## Differentiating between declarative and imperative programming

One of the reasons why React is so powerful is that it enforces a `declarative programming paradigm`.

The easiest way to approach this is to think about **imperative programming** as a `way of describing how things work`, and **declarative programming** as a `way of describing what you want to achieve`.

### **imperative approach**

```js
const toUpperCase = (input) => {
  const output = [];

  for (let i = 0; i < input.length; i++) {
    output.push(input[i].toUpperCase());
  }
  return output;
};
```

### **declarative approach**

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
Chapter 1 5

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
