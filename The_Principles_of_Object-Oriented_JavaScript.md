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
function cheers(height, weight) {
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

#### Constructors

A constructor is simply a function that is used with a key word, **new**, to create an object. The advantage of constructors is that objects created with the same constructor contain the same properties and methods.

The **this** object is automatically created by **new** when we call the constructor.

Make sure to always call constructor with **new**, otherwise, we risk changing the global object instead of the newly created object.

Actually, an error will occur if we call a constructor with **this** in strict mode without using "new". Because strict mode doesn't assign *this* to the global object, which means **this** remains *undefined*. And an error occurs when we attempt to create a property on undefined.

```javascript
// define a constructor (no difference from other normal functions)
function Person(name) {
  this.name = name;
}

// now, kevin is the instance of new Person type
const kevin = new Person('Kevin');
console.log(kevin instanceof Person); // -> true

// missing "new"
const john = Person('John');
// name property will be in the global object
console.log(name); // -> "John"
```

>constructors alone don't eliminate code redundancy. It would be more efficient if all of the instances shared the same method.

#### Prototypes

Prototype is shared among all of the object instances, and those instances can access properties of the prototype.

```javascript
const person = { name: 'kevin' };

console.log(name in person); // -> true
console.log(person.hasOwnProperty('name')); // -> true
console.log('hasOwnProperty' in person); // -> true
console.log(person.hasOwnProperty('hasOwnProperty')); // -> false
console.log(Object.prototype.hasOwnProperty('hasOwnProperty')); // -> true
```

We can determine whether or not a property is on the prototype by using:

```javascript
const hasPrototypeProperty = (object, name) => {
  return name in object && !object.hasOwnProperty(name);
};
console.log(hasPrototypeProperty(person, 'name')); // -> false
console.log(hasPrototypeProperty(person, 'hasOwnProperty')); // -> true
```

#### The [[Prototype]] Property

An instance keeps track of its prototype through an internal property called [[Prototype]]. This property is a pointer back to the prototype object that the instance is using. The use of [[Prototype]] property lets multiple instances of an object type refer to the same prototype, which can reduce code duplication.

```javascript
const Person = function(name) {
  this.name = name
};
const kevin = new Person('kevin');

Object.getPrototypeOf(kevin) === Person.prototype; // -> true
// using object.__proto__
kevin.__proto__ === Person.prototype; // -> true
kevin.__proto__ === Object.getPrototypeOf(kevin); // -> true

Object.getPrototypeOf(kevin) === Object.prototype; // -> false
kevin.__proto__ === Object.prototype; // -> false

Person.prototype.isPrototypeOf(kevin); // -> true
```

:boom: The own property **shadows** the prototype property, so the prototype property of the same name is no longer used.
:exclamation: You cannot assign a value to a prototype property from an instance. You just **shadows** it.

```javascript
const object = {};
console.log(object.toString()); // '[object object]'

object.toString = () => '[object Custom]';
console.log(object.toString()); // '[object Custom]'

delete object.toString;
console.log(object.toString()); // '[object object]'
// delete only works on own properties
delete object.toString;
console.log(object.toString()); // '[object object]'
```

#### Using prototype with constructors

The shared nature of prototype makes them ideal for defining methods once for all objects of a given type.

```javascript
function Person(name) {
  this.name = name;
}
Person.prototype.sayName = function() {
  console.log(this.name);
}

const staff = new Person('john');
const student = new Person('kevin');
staff.sayName(); // -> 'john'
student.sayName(); // -> 'kevin'
```

We can also store other types of data on the prototype. But be careful when using reference values. Because those values are shared across instances, we don't want one instance be able to change values in other instances.

```javascript
Person.prototype.favorites = [];

const kevin = new Person('kevin');
const john = new Person('john');
kevin.favorites.push('JavaScript');
john.favorites.push('Ruby');

console.log(kevin.favorites); // -> ['JavaScript', 'Ruby']
```

Adding properties to the prototype with an object literal is more concise. But there are side effects: it will change the **constructor** property to point to Object. *This happens because the constructor property exists on the prototype, not on the object instance.*

To avoid this, restore the constructor property to a proper value when overwriting the prototype.

```javascript
Person.prototype = {
  sayName(){
    console.log(this.name);
  },
  toString(){
    return `[Person ${this.name}]`;
  }
}
console.log(kevin.constructor === Person); // -> false
console.log(kevin.constructor === Object); // -> true

Person.prototype = {
  // to avoid the side effect
  constructor: Person,
  sayName(){
    console.log(this.name);
  },
  toString(){
    return `[Person ${this.name}]`;
  }
}
console.log(kevin.constructor === Person); // -> true
```

:collision: The relationships among instances, prototypes and the constructors is that there is no direct link  between the instance and the constructor. So any disruption between the instance and the prototype will also create a disruption between the instance and the constructor.

#### Changing Prototypes

We can manipulate the prototype at any point and have those changes reflected on existing instances.

Notice that even though we freeze or seal the objects, we still can add properties on the prototype and continue extending those objects. Because the [[Prototype]] property is a pointer, and while this pointer is frozen, the object that it points to is not.

```javascript
const kevin = new Person('kevin');
const john = new Person('john');

Object.freeze(kevin);
Person.prototype.cheers = function() {
  console.log('Hello');
}

kevin.cheers(); // -> 'Hello'
john.cheers(); // -> 'Hello'
```

#### Built-in Object Prototypes

```javascript
// built-in object
Array.prototype.sum = function() {
  return this.reduce((acc, cur) => acc + cur, 0);
}

// primitive wrapper type object
String.prototype.capitalize = function() {
  return this[0].toUpperCase() + this.substring(1);
}
```

### Inheritance

#### Methods Inherited from Object.prototype

1. valueOf()

  The valueOf() method gets called whenever an operator is used on an object. By default, valueOf() simply returns the object instance. We can always define your own valueOf() method if your objects are intended to be used with operators.

  ```javascript
  const now = new Date();
  const earlier = new Date(2009, 12, 3);
  console.log(now > earlier); // -> true
  ```

  When the greater-than operator is used, the valueOf() method is called on both objects before the comparison is performed.

2. toString()

  The toString() method is called as a fallback whenever valueOf() returns a reference value instead of a primitive value. It is also implicitly called on primitive values whenever JavaScript is expecting a string.
  
#### Modifying Object.prototype

Do not modify built-in object prototypes and that goes double for Object.prototype. Not only it will cause unforeseen consequences but add enumerable properties to Object.prototype, which will show up in for-in loop.

#### Object Inheritance

The simplest type of inheritance is between objects. Object literals have Object.prototype set as their [[prototype]] implicitly, but we can also explicitly  specify [[prototype]] with the Object.create() method.

```javascript
const person = { name: 'kevin' };
// is the same as
const person = Object.create(Object.prototype, {
    name: {
      configurable: true,
      enumerable: true,
      value: 'kevin',
      writable: true,
    }
  })
```

Inheriting from other object is much more interesting.

```javascript
const person1 = {
  name: 'kevin',
  sayName(){
    console.log(this.name);
  }
};
const person2 = Object.create(person1, {
  name:{
    configurable: true,
    enumerable: true,
    value: 'john',
    writable: true,
  }
});

person1.sayName(); // -> 'kevin'
person2.sayName(); // -> 'john'
console.log(person1.hasOwnProperty('sayName')); // -> true
console.log(person1.isPrototypeOf(person2)); // -> true
console.log(person2.hasOwnProperty('sayName')); // -> false
```

The chain ends with Object.prototype, whose [[prototype]] is set to null. We can also create an object with a null [[prototype]] via Object.create().

```javascript
const nakedObject = Object.create(null);
console.log('toString' in nakedObject); // -> false
console.log('valueOf' in nakedObject); // -> false
```

This nakedObject is an object with no prototype chain. In effect, this object is a completely blank slate with no predefined properties, which make it perfect for creating a **lookup hash** without potential naming collisions with inherited property names.

#### Constructor Inheritance

Object inheritance in JavaScript is also the basis of constructor inheritance.

```javascript
// If we declare a function
function MyContructor(){
  //...
}
// What JavaScript engine does behind the scenes
MyContructor.prototype = Object.create(Object.prototype, {
  constructor:{
    configurable: true,
    enumerable: true,
    value: MyContructor,
    writable: true,
  }
})
```

Because the prototype property is writable, we can change the prototype chain by overwriting it.

```javascript
function Rectangle(length, width) {
  this.length = length;
  this.width = width;
}
Rectangle.prototype.getArea = function() { return this.length * this.width };
const rect = new Rectangle(3, 5);
rect.getArea(); // -> 15

function Square(size) {
  this.length = size;
  this.width = size;
}

/* Beware: We overwrite the Square.prototype with the instance of Rectangle and restore the constructor to Square. */
Square.prototype = new Rectangle();
Square.prototype.constructor = Square;

const square = new Square(4);
square.getArea(); // -> 16
```

But `Square.prototype` doesn't actually need to be overwritten with a Rectangle object. The Rectangle constructor isn't doing anything that is necessary for Square. In fact, the only relevant part is that Square.prototype needs to somehow link to Rectangle.prototype in order for inheritance to happen.

>That means we can simply use Object.create()!

```javascript
Square.prototype = Object.create(Rectangle.prototype, {
  constructor:{
    configurable: true,
    enumerable: true,
    writable: true,
    value: Square,
  }
});
// or
Square.prototype = Object.create(Rectangle.prototype);
Square.prototype.constructor = Square;
```

#### Constructor Stealing

We can simply call the supertype constructor form the subtype constructor using either call() or apply() to pass in the newly created object. In other words, we can steal the supertype constructor for our own object.

Below is the way to avoid redefining properties from a constructor from which you want to inherit it.

```javascript
function Rectangle(length, width) {
  this.length = length;
  this.width = width;
}

// Instead of doing this
function Square(size) {
  this.length = size;
  this.width = size;
}
// We inherit it from the Rectangle constructor
function Square(size) {
  Rectangle.call(this, size, size);
}
```

#### Accessing Supertype Methods

It is common to override supertype methods with new functionality in the subtype. But if we still want to access the supertype method? We can directly access the supertype's prototype and use either call() or apply() to execute the method on the subtype object.

```javascript
function Rectangle(length, width) {
  this.length = length;
  this.width = width;
}
Rectangle.prototype.toString = function() {
  return `[Rectangle: ${this.length} X ${this.width}]`;
}

function Square(size) {
  Rectangle.call(this, size, size);
}
Square.prototype = Object.create(Rectangle.prototype);
Square.prototype.constructor = Square;

Square.prototype.toString = function() {
  // here we call the supertype method
  const code = Rectangle.prototype.toString.call(this);
  return code.replace('Rectangle', 'Square');
}
```

>Prototype chaining works well for inheriting methods from other objects. But we cannot inherit own properties using prototypes. To inherit own properties correctly,  we can use constructor stealing.

### Object Patterns

#### The Module Pattern

```javascript
const person = (function(){
  // private data variables
  let age = 30;

  return {
    // public methods and properties
    name: 'kevin',
    getAge() { return age },
    getOlder() { return ++age },
  };
}());

person.age // -> undefined
person.name // -> 'kevin'
person.getAge() // -> 30
person.getOlder() // -> 31
```

#### Revealing Module Pattern

Arrange all variables and methods on the top of the IIFE, and simply assign them to the returned object.

```javascript
const person = (function(){
  // private data variables
  let age = 30;
  const name = 'kevin';
  const getAge = () => age;
  const getOlder = () => ++age;
  
  // assign the variables and methods to be revealed
  return { name, getAge, getOlder };
}());
```

#### Private Members for Constructors

```javascript
function Person(name) {
  // private data variables
  let age = 30;

  this.name = name;
  this.getAge = function() { return age };
  this.getOlder = function() { return ++age };
}

const kevin = new Person('kevin');
console.log(kevin.age); // -> undefined
console.log(kevin.name); // -> 'kevin'
console.log(kevin.getAge()); // -> 30
console.log(kevin.getOlder()); // -> 31

const john = new Person('john');
console.log(john.getAge()); // -> 30
```

Placing methods on an object instance is less efficient than doing so on the prototype, but his is the only approach possible when you want private,    instance-specific data.

If we want private data to be shared across all instances, we can use the hybrid approach that looks like the module pattern but uses a constructor.

```javascript
const Person = (function() {
  // everyone share the same age
  let age = 30;
  function InnerPerson(name) {
    this.name = name;
  }
  InnerPerson.prototype.getAge = function() { return age; };
  InnerPerson.prototype.getOlder = function() { return ++age; };
  return InnerPerson;
}());

const kevin = new Person('kevin');
console.log(kevin.getOlder()); // -> 31
console.log(kevin.getOlder()); // -> 32

const john = new Person('john');
// share the same variable
console.log(john.getAge()); // -> 32
```
