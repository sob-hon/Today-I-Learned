# Prototype Pattern

The prototype pattern is a useful way to `share properties among many objects of the same type`. The prototype is an object that’s native to JavaScript, and can be accessed by objects through the prototype chain.

## Implementation

With [`factory pattern`](https://github.com/sob-hon/Today-I-Learned/blob/main/JavaScript-Design-Patterns/factory.md), when we wanted to create many dogs it was like this:

```js
const createDog = (name, age) => ({
  name,
  age,
  bark() {
    console.log(`${name} is barking!`);
  },
  wagTail() {
    console.log(`${name} is wagging their tail!`);
  },
});

const dog1 = createDog("Max", 4);
const dog2 = createDog("Sam", 2);
const dog3 = createDog("Joy", 6);
const dog4 = createDog("Spot", 8);
```

To show actually what happens, let's see the created objects:

![factory-downside](./assets/factory-downside.png)

Under the hood, we're creating two new functions for `each dog` object, which `uses memory`.

We can use the `Prototype Pattern` to share these methods among many dog objects.

```js
class Dog {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  bark() {
    console.log(`${this.name} is barking!`);
  }
  wagTail() {
    console.log(`${this.name} is wagging their tail!`);
  }
}

const dog1 = new Dog("Max", 4);
const dog2 = new Dog("Sam", 2);
const dog3 = new Dog("Joy", 6);
const dog4 = new Dog("Spot", 8);
```

As you see now we use `'Prototype Chain'` to access the `bark` and `wagTail` methods.

![prototype](./assets/prototype-pattern.png)

## \_\_proto\_\_

The prototype pattern is very powerful when working with objects that should have access to the same properties. Instead of creating a duplicate of the property each time, we can simply add the property to the `prototype`, since all instances have access to the `prototype object`.

Since all instances have access to the prototype, it’s easy to add properties to the prototype `even after creating the instances`.

```js
class Dog {
  constructor(name) {
    this.name = name;
  }

  bark() {
    return `Woof!`;
  }
}

const dog1 = new Dog("Daisy");
const dog2 = new Dog("Max");
const dog3 = new Dog("Spot");

Dog.prototype.play = () => console.log("Playing now!");

dog1.play(); // Playing now!
```

The term `prototype chain` indicates that there could be `more than one` step. Indeed! So far, we’ve only seen how we can access properties that are directly available on the first object that \_\_proto\_\_ has a reference to. However, `prototypes themselves also have a __proto__ object!`:

```js
class Dog {
  constructor(name) {
    this.name = name;
  }
  bark() {
    console.log("Woof!");
  }
}

/* 
When we use extends, it must call super() in its constructor. 
And also it points out our new instance's __proto__ to superclass (Dog in this case). 
*/
class SuperDog extends Dog {
  constructor(name) {
    /* 
    The super(name) statement is used to call the constructor of the superclass (Dog in this case)
    and pass the name parameter to it. This ensures that the name property of the Dog superclass is initialized correctly.
    */
    super(name);
  }

  fly() {
    console.log("Flying!");
  }
}

const dog1 = new SuperDog("Daisy");
dog1.bark(); // Woof!
dog1.fly(); // Flying!
```

We have access to the `bark` method, as we extended the `Dog` class. The value of `__proto__` on the prototype of `SuperDog` points to the `Dog.prototype` object!

![prototype-chain](./assets/prototype-chain.webp)

<mark>Note</mark>: It gets clear why it’s called a prototype chain: when we try to access a property that’s not directly available on the object, JavaScript `recursively` walks down all the objects that `__proto__` points to, until it finds the property!

## Object.create

The `Object.create` method lets us create a `new object`, to which we can `explicitly pass the value of its prototype`.

```js
const dog = {
  bark() {
    return `Woof!`;
  },
};

const pet1 = Object.create(dog);
pet1.bark(); // Woof!
console.log("Direct properties on pet1: ", Object.keys(pet1)); // []
console.log("Properties on pet1's prototype: ", Object.keys(pet1.__proto__)); // ['bark']
```

Perfect! `Object.create` is a simple way to let objects directly inherit properties from other objects, by specifying the newly created object’s prototype. The new object can access the new properties by walking down the prototype chain.

### Tradeoffs

🟢 `Memory efficient` : The prototype chain allows us to access properties that aren't directly defined on the object itself, we can avoid `duplication of methods and properties`, thus reducing the amount of memory used.

🔴 `Readability` : When a class has been extended many times, it can be difficult to know where certain properties come from.

For example, if we have a `BorderCollie` class that extends all the way to the Animal class, it can be `difficult to trace back where certain properties came from`.

![readability-of-prototype](./assets/readability-of-prototype.png)
