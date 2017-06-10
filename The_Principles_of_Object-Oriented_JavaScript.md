# The Principles of Object-Oriented JavaScript

|#|Topics|
|---|---|
|1.|[Primitive and Reference Types](#primitive-and-reference-types)|
|2.|[Functions](#functions)|
|3.|[Understanding Objects](#understanding-objects)|
|4.|[Constructors and Prototypes](#constructors-and-prototypes)|
|5.|[Inheritance](#inheritance)|
|6.|[Object Patterns](#object-patterns)|

### Primitive and Reference Types



### Functions



### Understanding Objects

:cyclone: Objects in JavaScript are **dynamic**, meaning that they can change at any point during code execution.

#### How to detect properties in object?

```javascript
const person = { name: 'kevin' };
console.log('name' in person); // -> true
```

But **in** operator checks for both own properties and prototype properties. So use **hasOwnProperty()** to detect only the own properties.

```javascript
console.log('toString' in person); // -> true
console.log(person.hasOwnProperty('toString')); // -> false
```

#### Removing properties

```javascript
const person = { name: 'kevin' };
delete person.name;
console.log(person.name); // -> undefined
```

#### Enumeration

By default, all properties that you ad to an object are **enumerable**, which means that you can iterate over them using a for-in loop. But many **native properties** are not enumerable by default.

```javascript
const person = { name: 'kevin' };
console.log('name' in person); // -> true
console.log(person.propertyIsEnumerable('name')); // -> true

const numbers = [1, 2, 3];
console.log('length' in numbers); // -> true
console.log(numbers.propertyIsEnumerable('length')); // -> false
```

#### Types of Properties

There are two difference types of properties: data properties and accessor properties. Data properties contain a value, whereas accessor properties define a function to call when the property is read(called a **getter**) and the property is written to(call a **setter**).

```javascript
const person = {
  _name: 'kevin',
  // getter
  get name() { return this._name },
  // setter
  set name(newNuame) { this._name = newName; },
};
```

#### Property Attributes

There are two property attributes shared between data and accessor attributes: **enumerable** and **configurable**(they are both set to **true** by default when you declare on an object).

enumerable: can be iterate in for...in loop or not
configurable: the ability to **delete property** or **change property attributes**(except writable!)

We can change property attributes by `Object.defineProperty()` or `Object.defineProperties()`. Note that if the property attribute is set to be non-configurable(configurable: false), then effectively this property is locked down. You can no longer change it back again!

```javascript
const person = {
  name: 'kevin',
  age: 30,
}
// change a property attribute
Object.defineProperty(person, 'name', {
  enumerable: false,
  configurable: false,
})

// or change property attributes once
Object.defineProperties(person, {
  name: {
    enumerable: false,
    configurable: false,
  },
  age: {
    enumerable: false,
    configurable: false,
  }
})

// cannot make non-configurable property configurable again
Object.defineProperty(person, 'age', { configurable: true }); // error!
```

**Data Property Attributes**: value, writable

When defining a new property with Object.defineProperty(), it's important to specify all attributes, because all attributes are automatically set to false in default.

```javascript
const person = {};
Object.defineProperty(person, 'name', {
  value: 'kevin',
  writable: true,
  configurable: true,
  enumerable: true,
});
// equals to
const person = { name: 'kevin' };
```

**Accessor Property Attributes**: get, set

The advantage of using accessor property attributes instead of object literal notation to define accessor properties is that you can also define those properties on existing objects.

```javascript
const person = { _name: 'kevin' };
Object.defineProperty(person, 'name', {
  get() { return this._name },
  set(newName) { this._name = newName },
  configurable: true,
  enumerable: true,
});

// equals to
const person = {
  _name: 'keivn',
  get name() { return this._name },
  set name(value) { this._name = value },
};
```

#### Retrieving Property Attributes

```javascript
const person = {
  _name: 'kevin'
  get name() { return this._name };
  set name(value) { this._name = value };
};
console.log(Object.getOwnPropertyDescriptor(person, '_name')); // -> Object { value: "kevin", writable: true, enumerable: true, configurable: true }
console.log(Object.getOwnPropertyDescriptor(person, 'name')); // -> Object { enumerable: true, configurable: true, get: function, set: function }
```

### Preventing Object Modification

Objects, just like properties, have internal attributes that govern their behavior.

**Preventing Extensions**

```javascript
const person = { name: 'kevin' };
Object.preventExtensions(person);
person.age = 30; // not working

console.log(Object.isExtensible(person)); // -> false
```

**Sealing Objects**

Non-extensible and non-configurable. So we can't add or remove properties. Sealed objects are JavaScript's way of giving us the control without using classes.

```javascript
const person = { name: 'kevin' };
Object.seal(person);

delete person.name; // not working
Object.defineProperty(person, 'name', {
  value: 'hello', // not working
  enumerable: false, // not working
  configurable: false, // not working
  // BUT!!!!
  writable: false, // it works!
});
person.name = 'hello'; // works

console.log(Object.isExtensible(person)); // -> false
console.log(Object.isSealed(person)); // -> true
```

**Freezing Objects**

If an object is frozen, we can't add or remove properties, we can't change properties' type, we can't write to any data properties. In essence, a frozen object is sealed and the data properties are also read-only.

```javascript
const person = { name: 'kevin' };
Object.freeze(person);

delete person.name; // not working
Object.defineProperty(person, 'name', {
  value: 'hello', // not working
  enumerable: false, // not working
  configurable: false, // not working
  writable: false, // NOT working
});
person.name = 'hello'; // NOT working

console.log(Object.isExtensible(person)); // -> false
console.log(Object.isSealed(person)); // -> true
console.log(Object.isFrozen(person)); // -> true
```

### Constructors and Prototypes



### Inheritance



### Object Patterns
