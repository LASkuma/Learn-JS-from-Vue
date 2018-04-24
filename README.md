- [Learn-JS-from-Vue](#learn-js-from-vue)
- [Object](#object)
  - [Properties](#properties)
    - [Property Descriptors](#property-descriptors)
      - [Related functions](#related-functions)
    - [Symbol Properties](#symbol-properties)
      - [Related functions](#related-functions)

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
