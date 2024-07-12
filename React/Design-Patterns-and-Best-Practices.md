# React 18 Design Pattern and Best Practices

## Design, build and deploy production-ready web applications with React by leveraging industry-best practices

### Written by Carlos Santana Roldan

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