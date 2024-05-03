# Design Patterns

Design patterns are `concepts(solutions)` we can use to solve `commonly recurring issues` in software architecture.

# Best Practices

Best Practices are a set of `guidelines` or `recommendations (tips and tricks)` that are considered ideal or optimal in software development.

### **FYI: In this learning we want to learn about the `design patterns`**

## Module pattern

As your application and codebase grow, it becomes increasingly important to keep your code `maintainable` and `separated`.<br/>
The module pattern allows you to split up your code into `smaller` and `reusable` pieces.

ES2015 introduced built-in JavaScript modules. A module is a file containing JavaScript code and makes it easy to `expose (export)` and `hide (encapsulate)` certain values.

The module pattern provides a way to have both `public` and `private` pieces with the `export` keyword. This protects values from leaking into the global scope or ending up in a naming collision.

### Implementation

There a few ways we can use modules:

### HTML tag

When adding JavaScript to HTML files directly, you can use modules by adding the `type="module"` attribute to the script tags.

#### **index.html**

```html
<html>
  <head>
    <meta charset="UTF-8" />
    <script src="index.js" type="module"></script>
    <link href="./styles.css" rel="stylesheet" />
  </head>
</html>
```

#### **math.js**

```js
const secret = "This is just in this file and encapsulated";

export function sum(x, y) {
  return x + y;
}

export function multiply(x, y) {
  return x * y;
}

export function subtract(x, y) {
  return x - y;
}

export function divide(x, y) {
  return x / y;
}
```

#### **index.js**

```js
import { sum, subtract, divide, multiply, secret } from "./math.js";

console.log("Sum", sum(1, 2)); // 3
console.log("Subtract", subtract(1, 2)); // -1
console.log("Divide", divide(1, 2)); // 0.5
console.log("Multiply", multiply(1, 2)); // 2
console.log("Sum", secret); // Error in SyntaxError: The requested module '/math.js' does not provide an export named 'secret'
```

### Node

In Node, you can use ES6 modules either by:

- Using the `.mjs` extension
- Adding `"type": "module"` to your `package.json` 

#### **package.json**

```json
{
  "name": "node-starter",
  "version": "0.0.0",
  "type": "module",
}
```

## Tradeoffs

🟢 `Encapsulation`: The values within a module are scoped to that specific module. Values that aren't explicitly exported are not available outside of the module.

🟢 `Reusability`: We can reuse modules throughout our application
