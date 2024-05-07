# Module pattern

As your application and codebase grow, it becomes increasingly important to keep your code `maintainable` and `separated`.<br/>
The module pattern allows you to split up your code into `smaller` and `reusable` pieces.

ES2015 introduced built-in JavaScript modules. A module is a file containing JavaScript code and makes it easy to `expose (export)` and `hide (encapsulate)` certain values.

The module pattern provides a way to have both `public` and `private` pieces with the `export` keyword. This protects values from leaking into the global scope or ending up in a naming collision.

## Implementation

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
  "type": "module"
}
```

## Rename imports

In some cases, we need to use a name other than the function name itself. This is how we do it:

```js
import {
  add as addValues, // rename add to addValues
  multiply as multiplyValues, // rename multiply to multiplyValues
  subtract,
  square,
} from "./math.js";
```

## Default imports VS. Named imports

Besides named exports, which are exports defined with just the `export` keyword, you can also use a default export. You can `only have one` default export per module.

#### **math.js**

```js
// default export
export default function add(x, y) {
  return x + y;
}

export function multiply(x) {
  return x * 2;
}

export function subtract(x, y) {
  return x - y;
}

export function square(x) {
  return x * x;
}
```

Previously, we had to use the brackets for our named exports: `import { module } from 'module'`. With a default export, we can import the value without the brackets: `import module from 'module'`:

#### **index.js**

```js
import add, { multiply, subtract, square } from "./math.js";
```

Since JavaScript knows that this value is always the value that was exported by default, we can give the imported default value another name than the name we exported it with:

#### **index.js**

```js
import addValues, { multiply, subtract, square } from "./math.js";
```

## Import all

We can also import all exports from a module, meaning all named exports and the default export, by using an asterisk `*` and giving the name we want to import the module as. The value of the import is equal to an `object containing all the imported values`. Say that I want to import the entire module as `math`.

#### **math.js**

```js
import * as math from "./math.js";

math.default(7, 8); // this is the add function cause it's exported as the default
math.multiply(8, 9);
math.subtract(10, 3);
math.square(3);
```

In this case, we’re importing all exports from a module. Be careful when doing this, since you may end up `unnecessarily` importing values.

### Tradeoffs

🟢 `Encapsulation` : The values within a module are scoped to that specific module. Values that aren't explicitly exported are not available outside of the module.

🟢 `Reusability` : We can reuse modules throughout our application

## Dynamic import

When importing all modules on the top of a file, all modules get loaded before the rest of the file. In some cases, we only need to import a module based on a certain condition. With a `dynamic import`, we can import modules `on demand`:

```js
import("module").then((module) => {
  module.default();
  module.namedExport();
});

// Or with async/await
(async () => {
  const module = await import("module");
  module.default();
  module.namedExport();
})();
```

Let’s dynamically import the `math.js` example used in the previous paragraphs.<br/>
The module only gets loaded, if the user clicks on the button.

```js
const button = document.getElementById("btn");
const anotherButton = document.getElementById("anotherBtn");

button.addEventListener("click", () => {
  import("./math.js").then((module) => {
    console.log("Add: ", module.add(1, 2));
    console.log("Multiply: ", module.multiply(3, 2));

    const button = document.getElementById("btn");
    button.innerHTML = "Check the console";
  });
});

/*************************** */
/**** Or with async/await ****/
/*************************** */
anotherButton.addEventListener("click", async () => {
  const { add, multiply } = await import("./math.js");
  console.log("Add: ", add(1, 2));
  console.log("Multiply: ", multiply(3, 2));
});
```

By dynamically importing modules, we can `reduce the page load time`. We only have to load, parse, and compile the code that the user really needs, `when the user needs it`.
