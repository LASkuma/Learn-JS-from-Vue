- [Learn-JS-from-Vue](#learn-js-from-vue)
- [Object](#object)
    - [Properties](#properties)
        - [Property Descriptors](#property-descriptors)
            - [Related functions](#related-functions)
        - [Symbol Properties](#symbol-properties)
            - [Related functions](#related-functions)
        - [More about enumerability](#more-about-enumerability)
            - [Related functions](#related-functions)
    - [Prototype chain](#prototype-chain)
        - [Inheritance in prototype fashion](#inheritance-in-prototype-fashion)
        - [ES6 Classes](#es6-classes)
        - [What's the difference between prototype, \_\_proto\_\_ and Object.getPrototypeOf?](#whats-the-difference-between-prototype----proto---and-objectgetprototypeof)
            - [object.\_\_proto\_\_ and Object.getPrototypeOf(object)](#object--proto---and-objectgetprototypeofobject)
            - [Func.prototype](#funcprototype)

# Learn-JS-from-Vue

What I learned from reading Vue's source code.

# Object

## Properties

At first, I thought those Javascript Objects **without functions** are simply **key-value pairs**, and properties are equal to keys. However, Javascript Object Property Descriptors successfully blew up my mind. What is that?

### Property Descriptors

The **Property Descriptors** describes the trait of each property. There are 6 kinds of [**Property Descriptors**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty).

1.  `configurable`

    Defines whether the descriptors could be modified for the **property**.

2.  `enumerable`

    Defines whether the **property** can be accessed by several enumerate functions. For example, when `enumerable` is set to **false** for one property, the property won't appear in the array returned by calling `Object.keys` or accessed by the `for...in` loop.

3.  `value`

    Defines the value for the **property**. Cannot be set together with `get`. This is what I firstly thought about JS Objects, simple **key-value** pairs.

4.  `writable`

    Defines the mutability for the **property**, can only be set together with `value`.

5.  `get`

    Accepts a function that would behave as the getter for the **property**. In Vue, `get` is used to gather UI, watch and computed property **dependencies** of `data` object so that it can know which part of UI should be rerendered after `data` modification.

6.  `set`

    Accepts a function that would behave as the setter for the **property**. In Vue, `set` is used to notify all the **dependencies** to update when the `data` that they depends on has been set to a different value.

#### Related functions

```javascript
Object.defineProperty(object, propertyName, options);
```

Pass the **Property Descriptors** as an Object into **options** to set the property in your style.

```javascript
Object.getOwnPropertyDescriptor(object, propertyName);
```

Get the **Descriptors** for the specified **property**.

See more examples on [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)

### Symbol Properties

Besides **strings**, Javascript Object **properties** can also be **Symbols**. The common pitfall for **Symbol Properties** is their **enumerability**. They cannot be accessed with `Object.keys` or `for...in` loop event if you explicitly set `enumerable` descriptor to `true` for the **Symbol Property**.

#### Related functions

```javascript
Reflect.ownKeys(object);
```

Return all the Symbol and non-Symbol `Properties` no matter they are `enumerable` or not.

```javascript
Object.getOwnPropertySymbols(object);
```

Return `Symbol Properties` only, no matter they are `enumerable` or not.

### More about enumerability

**Enumerability** is important when you want to iterate through all or part the properties in any Objects. However, you still need to make sure whether you want to iterate thgouth the Object's **own properties** or properties also on its **prototype chain**.

#### Related functions

```javascript
Object.keys(object);
```

Return **non-Symbol** `Properties` that are `enumerable`, for object's **own properties** only.

```javascript
for (let prop in object) {
  // Do something
}
```

Return **non-Symbol** `Properties` that are `enumerable`, for object's **own properties** and its **prototype chain**.

```javascript
Object.getOwnPropertyNames(object);
```

Return all **non-Symbol** `Properties`, for object's **own properties** only.

## Prototype chain

It's surprising enough for beginners to know that Javascript is **not** a **class based** language. For detailed information, check [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain).

### Inheritance in prototype fashion

It's totally different from inheritance in class based languages. Let's use the simplified example on MDN to get the main idea.

```javascript
```

### ES6 Classes

Fortunately, ES6 gives developers a [**class**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes) style way to define classes. However, it's only a **syntax sugar** on prototype chains.

### What's the difference between prototype, \_\_proto\_\_ and Object.getPrototypeOf?

#### object.\_\_proto\_\_ and Object.getPrototypeOf(object)

They are **same**. However, object.\_\_proto\_\_ is not coined in any spec, yet most of the modern browsers supports it. These two would return the **prototype** for the specified **object**.

#### Func.prototype

This gives you a way to extend the **constructor function** of the **prototype**. This makes **Mixin Pattern** possible.

```javascript
function A() {
  this.a = 1;
  this.b = 2;
}

const a1 = new A();
// a1: { a: 1, b: 2 }

A.ptototype.c = 3;
const a2 = new A();
// a1: { a: 1, b: 2, c: 3 }
// a2: { a: 1, b: 2, c: 3 }
```

The behavior of `A.prototype = {...}` is unpredictable to me. Still trying to figure it out.

```javascript
function A() {
  this.a = 1;
  this.b = 2;
}

let a1 = new A();
// a: { a: 1, b: 2}

A.prototype = {
  c: 3,
};

let a2 = new A();
// a1: { a: 1, b: 2 }
// a2: { a: 1, b: 2, c: 3 }

A.prototype.d = 4;

let a3 = new A();
// a1: { a: 1, b: 2 } and both a.c, a.d are undefined
// a2: { a: 1, b: 2, c: 3, d: 4 }
// a3: { a: 1, b: 2, c: 3, d: 4 }
```
