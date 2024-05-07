# Design Patterns

Design patterns are `concepts(solutions)` we can use to solve `commonly recurring issues` in software architecture.

# Best Practices

Best Practices are a set of `guidelines` or `recommendations (tips and tricks)` that are considered ideal or optimal in software development.

### **FYI: In this learning we want to learn about the `design patterns`**

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

# Singleton

The singleton pattern is a design pattern that ensures that a class can only have `one instance` of itself. <br />
This single instance can be shared throughout our application, which makes Singletons great for `managing global state` in an application.

## Implementation

```js
let instance;
let counter = 0;

// 1. Creating the `Counter` class, which contains a `constructor`, `getInstance`, `getCount`, `increment` and `decrement` method.

class Counter {
  constructor() {
    if (instance) {
      throw new Error("You can only create one instance!");
    }
    this.counter = counter;
    instance = this;
  }

  getCount() {
    return this.counter;
  }

  increment() {
    return ++this.counter;
  }

  decrement() {
    return --this.counter;
  }
}

// 2. Setting a variable equal to the the frozen newly instantiated object, by using the built-in `Object.freeze` method.
// This ensures that the newly created instance is not modifiable.
const singletonCounter = Object.freeze(new Counter());

// 3. Exporting the variable as the `default` value within the file to make it globally accessible.
export default singletonCounter;
```

![Singleton](./assets/Singleton.gif)

<mark>Note</mark> : We can also implement it with `Objects`:

```js
let counter = 0;

export default Object.freeze({
  getCount: () => counter,
  increment: () => ++counter,
  decrement: () => --counter,
});
```

### Tradeoffs

🟢 `Memory` : Restricting the instantiation to just one instance could potentially `save a lot of memory space`. Instead of having to set up memory for a new instance each time, we only have to set up memory for that one instance, which is referenced throughout the application.

🔴 `Unnecessary` : ES6 modules are singleton by default. We no longer need to explicitly create singletons to achieve this global, non-modifiable behavior.

🔴 `Global Scope Pollution` : The global behavior of Singletons is essentially the same as a global variable. Global Scope Pollution can end up in `accidentally overwriting` the value of a global variable, which can lead to a lot of `unexpected behavior`.

# Proxy

With a Proxy object, we get more `control` over the `interactions` with certain objects.

Normally, we access object properties and set them with `dot notation`. But without any validation or control. But Proxy is a middleware we can have and apply any functionality that is desired.

![proxy](./assets/proxy.gif)

## Implementation

In JavaScript, we can easily create a new proxy by using the built-in `Proxy` object.

![proxy-object](./assets/proxy-object.png)

### The Proxy object receives two arguments:

1. The `target` object

2. A `handler` object, which we can use to add functionality to the proxy. This object comes with some built-in functions that we can use, such as get and set. <br /><br />

```js
const person = {
  name: "John Doe",
  age: 42,
  email: "john@doe.com",
  country: "Canada",
};

const personProxy = new Proxy(person, {
  get: (target, prop) => {
    console.log(`The value of ${prop} is ${target[prop]}`);
    return target[prop];
  },
  set: (target, prop, value) => {
    console.log(`Changed ${prop} from ${target[prop]} to ${value}`);
    target[prop] = value;
    return true;
  },
});
```

<mark>Note</mark>: We can use built-in `Reflect` object (it's an object containing all the needed methods for proxy handling) to make it `easier` to `manipulate` the target object.

![proxy-with-reflect](./assets/proxy-with-reflect.gif)

## Tradeoffs

## Advantages

![advantages-of-proxy](./assets/advantages-of-proxy-pattern.jpeg)

### 🟢 `Enhanced Security and Access Control` :

```js
// Define the user object representing user profile
const user = {
  name: "John",
  email: "john@example.com",
  role: "user",
  isAdmin: false,
};

// Define a handler for the Proxy to implement access control
const accessControlHandler = {
  get: function (target, property) {
    // Check if the property is sensitive and user is not an admin
    if (property === "email" && !target.isAdmin) {
      throw new Error(
        "Access denied: You do not have permission to access sensitive information"
      );
    } else {
      // Allow access to non-sensitive properties or if user is an admin
      return target[property];
    }
  },
  set: function (target, property, value) {
    // Check if user is an admin before allowing property modification
    if (!target.isAdmin) {
      throw new Error(
        "Access denied: You do not have permission to modify properties"
      );
    } else {
      // Allow modification if user is an admin
      target[property] = value;
      return true;
    }
  },
};

// Simulate a user with admin privileges
user.isAdmin = true;

// Create a Proxy with access control mechanisms
const securedUser = new Proxy(user, accessControlHandler);

// Attempt to access properties
console.log(securedUser.name); // Output: John (allowed)
console.log(securedUser.email); // Throws: Error: Access denied: You do not have permission to access sensitive information
console.log(securedUser.role); // Output: user (allowed)

// Attempt to modify properties
securedUser.name = "Jane"; // Throws: Error: Access denied: You do not have permission to modify properties
securedUser.email = "jane@example.com"; // Throws: Error: Access denied: You do not have permission to modify properties
securedUser.role = "admin"; // Throws: Error: Access denied: You do not have permission to modify properties
```

### 🟢 `Enhanced Security and Access Control` :

```js
// Define the user object
const user = {
  username: "john_doe",
  email: "john.doe@example.com",
  age: 25,
};

// Create a Proxy for the user object with data validation
const userProxy = new Proxy(user, {
  // Trap for setting properties
  set: (obj, prop, value) => {
    // Data validation based on the property being set
    if (prop === "username") {
      // Username must be alphanumeric with underscores
      if (!/^[a-zA-Z0-9_]+$/.test(value)) {
        throw new Error(
          "Username can only contain letters, numbers, and underscores."
        );
      }
    }

    if (prop === "email") {
      // Email must be in a valid format
      if (!/^[\w.-]+@[a-zA-Z]+\.[a-zA-Z]{2,}$/.test(value)) {
        throw new Error("Please provide a valid email address.");
      }
    }

    if (prop === "age") {
      // Age must be a positive integer
      if (!Number.isInteger(value) || value <= 0) {
        throw new Error("Age must be a positive integer.");
      }
    }

    // If data validation passes, allow the property to be set
    return Reflect.set(obj, prop, value);
  },
});

// Attempt to set properties with invalid values
try {
  userProxy.username = "john@doe"; // Throws an error
} catch (error) {
  console.error(error.message); // Output: Username can only contain letters, numbers, and underscores.
}

try {
  userProxy.email = "invalid.email"; // Throws an error
} catch (error) {
  console.error(error.message); // Output: Please provide a valid email address.
}

try {
  userProxy.age = -25; // Throws an error
} catch (error) {
  console.error(error.message); // Output: Age must be a positive integer.
}

// Log the user object after successful property assignments
console.log(userProxy);
```

### 🟢 `Caching and Optimization` :

```js
// Function to be cached (example: a costly computation)
function expensiveComputation(n) {
  console.log(`Performing expensive computation for ${n}`);
  return n * 2;
}

// Create a cache object
const cache = {};

// Create a Proxy to implement caching
const cachedExpensiveComputation = new Proxy(expensiveComputation, {
  apply: function (target, thisArg, args) {
    // Generate a unique key based on function name and arguments
    const cacheKey = `${target.name}_${args.join("_")}`;

    // Check if result exists in cache
    if (cache[cacheKey] !== undefined) {
      console.log(`Returning cached result for ${args}`);
      return cache[cacheKey];
    } else {
      // Call the original function and store result in cache
      const result = Reflect.apply(target, thisArg, args);
      console.log(`Caching result for ${args}`);
      cache[cacheKey] = result;
      return result;
    }
  },
});

// Test the cached function
console.log(cachedExpensiveComputation(5)); // Performs computation, caches result, returns 10
console.log(cachedExpensiveComputation(5)); // Returns cached result 10
console.log(cachedExpensiveComputation(10)); // Performs computation, caches result, returns 20
console.log(cachedExpensiveComputation(10)); // Returns cached result 20
```

### 🟢 `Lazy Loading and Performance Optimization` :

```js
// Simulated asynchronous function to fetch resource data
function fetchResource(resourceName) {
  console.log(`Fetching ${resourceName}...`);
  // Simulate a delay in fetching the resource
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(`Data for ${resourceName}`);
    }, 1000); // Simulate 1 second delay
  });
}

// Create a Proxy for lazy loading resources
const lazyLoadedResources = new Proxy(
  {},
  {
    get: function (target, resourceName) {
      // Check if resource is already loaded
      if (!target[resourceName]) {
        // If resource is not loaded, fetch it and store the promise in the target
        target[resourceName] = fetchResource(resourceName);
      }
      // Return the promise for the resource
      return target[resourceName];
    },
  }
);

// Access resources lazily
lazyLoadedResources.resource1.then((data) => {
  console.log("Resource 1:", data);
});

lazyLoadedResources.resource2.then((data) => {
  console.log("Resource 2:", data);
});

// Output:
// Fetching resource1...
// Fetching resource2...
// (After 1 second)
// Resource 1: Data for resource1
// Resource 2: Data for resource2
```

## Disadvantages

### 🔴 `Performance Overhead`:

Using proxies can introduce a `slight performance overhead` due to the `interception of property access and modification`. While this overhead is usually negligible, it can be a concern in highly performance-critical applications.


### 🔴 `Debugging Challenges`:

Debugging code that `heavily` relies on proxies can be more challenging, as the behavior of proxies can add an additional layer of complexity to the debugging process.

### 🔴 `Overuse and Over-engineering`:

It’s important to use proxies `judiciously`. Overusing proxies or implementing them for simple tasks that can be accomplished with traditional methods may lead to over-engineering and unnecessarily complex code.