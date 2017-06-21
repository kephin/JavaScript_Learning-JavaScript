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

**Primitive types**: directly contains the primitive value, **rather than a pointer to an object**. If you set one variable equal to another, each variable get its own copy of the data. Because each variable uses it own storage space, changes to one variable do not affect on the other.

Boolean, Number, String, Null, Undefined

```javascript
let person; // assign undefined automatically
console.log(typeof person); // -> 'undefined'

// But!
console.log(typeof null); // -> 'object'
// So the best way to identify if the value is null
console.log(value === null); // true or false

// changing one variable does not affect the other one
const number_1 = 1;
const number_2 = number_1;
number_1 = 2;
console.log(number_2); // -> still 1
```

**Reference types**: when you assign an object to a value, you're actually assigning a pointer.

The best way to dereference objects is to set the object variable to **null**.

```javascript
const objct = new Object();
// do something
object = null; // dereference

// both object_1 and object_2 point to the same object
const object_1 = new Object();
const object_2 = object_1;
object_1.sayHi = 'Hello!';
console.log(object_2.sayHi); // -> 'Hello!'
```

>We can modify objects whenever you want, even though we did not define them in the first place.

**Built-in objects**: Array, Date, Error, Function, Object, RegExp

  ```javascript
  const now = new Date();
  const error = new Error('Something bad happened');
  ```

- Object and array literals

  ```javascript
  const person = new Object();
  person.name = 'kevin';
  person['passport number'] = 'A102284572';
  // is the same as
  const person = { name: 'kevin', age: 30, 'passport number': 'A102284572' };

  const colors = new Array('red', 'blue', 'yellow');
  // is the same as
  const colors = ['red', 'blue', 'yellow'];
  ```

- Function literals

  ```javascript
  const func = new Function('value', 'return value;');
  // is the same as
  function func(value) { return value; };
  const func = value => value;
  ```

- Regular expression literals: no need to worry about escaping characters within strings.

  ```javascript
  const re = new RegExp('\\d+');
  // is the same as
  const re = /\d+/g;
  ```

Dot notation and bracket notation: Bracket notation is very useful when you do dynamically decide which property to access.

Identifying reference types

```javascript
const arr = [];
const obj = {};
const func = value => value;

// typeof always returns 'object'
console.log(typeof arr); // -> 'object'
console.log(typeof obj); // -> 'object'
console.log(typeof func); // -> 'object'

// use instanceof
console.log(arr instanceof Array); // -> true
console.log(arr instanceof Object); // -> true
console.log(obj instanceof Object); // -> true
console.log(func instanceof Function); // -> true
console.log(func instanceof Object); // -> true
```

The best way to identify array

```javascript
console.log(Array.isArray(arr)); // -> true
```

:cyclone: Primitive wrapper types: String, Numbers, Boolean

The primitive wrapper types are reference types that are automatically created behind the scenes whenever strings, numbers or booleans are read. The reason why primitive wrapper types existed is the uses of methods in primitive types.

```javascript
const name = 'kevin';
const firstChar = name.charAt(0);
console.log(firstChar); // -> 'k'

// this is what happens behind the scenes
const name = 'kevin';
//const temp = new String(name);
const firstChar = temp.charAt(0);
//temp = null;
console.log(firstChar); // -> 'k'
```

### Functions

**typeof** operator will return 'function' for any object with [[Call]] property.

#### Declaration v.s Expression

```javascript
// declaration (will hoist)
function add(num1, num2) {
  return num1 + num2;
}
// expression (will NOT hoist)
const minus = function(num1, num2) {
  return num1 - num2;
};
const add = (num1, num2) => num1 + num2;
```

#### Functions as variables

#### Parameters

You can pass any number of parameters to any function without causing an error. That's because function parameters are actually stored as an *array-like* structure called **arguments**.

But ES6 **rest** syntax is replacing arguments. You can even use *arguments* with arrow function.

```javascript
// arguments
function sum() {
  var result = 0;
  for(var index = 0; index < arguments.length; index++){
    result += arguments[index];
  }
  return result;
}

// ES6 rest
const sum = (...nums) => nums.reduce((prev, acc) => prev + acc);
```

#### Overloading

JavaScript doesn't have overloading because it can accept any number of parameters and the types of parameters a function takes are not specified at all.

However, we can mimic it function overloading by `typeof` and `instanceof`operator or `arguments[] === 1`.

```javascript
function cheers(message) {
  if( typeof message === 'undefined') console.log('Hello!');
  else{
    console.log(message);
  }
}
```

#### this

The main reason to use **this** when defining object is to eliminate tight coupling. **this** represents the calling object for the function.

#### Changing this

1. **call** method: it will executes the function with a particular **this** value and with specific parameters.

2. **apply** method: it is exactly the same as call method except that it accept only two parameters: **this** and an array or array-like object of parameters to pass to the function.

3. **bind** method: it only bind a particular **this** but do not execute it. And we can also pass in any parameters later.

```javascript
function cheers(height, weight){
  const BMI = Math.round(weight / (height / 100) ** 2);
  console.log(`${this.name}: My BMI is ${BMI}`)
}
const person_1 = { name: 'Kevin' };
const person_2 = { name: 'John' };

// call
cheers.call(person_1, 178, 70); // -> Kevin: My BMI is 22
cheers.call(person_2, 175, 72); // -> John: My BMI is 24
// apply
cheers.apply(person_1, [178, 70]); // -> Kevin: My BMI is 22
cheers.apply(person_2, [175, 72]); // -> John: My BMI is 24
// bind
const cheersPerson_1 = cheers.bind(person_1);
cheersPerson_1(178, 70); // -> Kevin: My BMI is 22
const cheersPerson_2 = cheers.bind(person_2, 175, 72);
cheersPerson_2(); // -> John: My BMI is 24
```

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
