# What is the difference between var, let and const?

### Problems using var

You can use **var** to declare the variable with the same name **again** without errors, which will lead to bugs easily.

```javascript
var numberOfPens = 1;
var numberOfPens = 2; // no errors..
```

var is **functional scoped**.

Using var will leak the specific variable scoped to the window(global scope) while in **if/for...(non-function)** block.

```javascript
for(var i = 1; i < 10; i++){
  //...
}
console.log(i); // => 10
```

[IIFE(Immediately-Invoked-Function-Expression)](http://benalman.com/news/2010/11/immediately-invoked-function-expression/) creates a scope where nothing is going to leak into the global scope.

Tempero Dead Zone: variable hoisting.

```javascript
console.log(numberOfVote); // undefied
var numberOfVote = 3000;

// variable hoisting make it equal to
var numberOfVote;
console.log(numberOfVote); // undefied
numberOfVote = 3000;

// but if using let or const
console.log(numberOfVote); // Uncaught ReferenceError: numberOfVote is not defined
let numberOfVote = 3000;
```

### let and const

They can only be declared **once**.

```javascript
let numberOfPens = 1;
let numberOfPens = 2; // Uncaught SyntaxError: Identifier 'numberOfPens' has already been declared
```

let and const are **block** scoped

```javascript
{
  let user = "Kevin";
}
console.log(user); // Uncaught ReferenceError: user is not defined
```

A const variable cannot be updated.

But the **property** of the const variable(which is an object) **can** be updated. But still, the entire object is immutable.

```javascript
const person = {
  name: 'Kevin',
  height: 179,
  weight: 70
};
person.weight = 75;     // no error
person.gender = 'male'; // no error
person = {
  glasses: true, // Uncaught TypeError: Assignment to constant variable
}
```

You can use **Object.freeze()** to make even the property of an object immutable.

```javascript
const kevin = Object.freeze(person);
```

Examples in for-loop.

```javascript
// using var, 'i' will be overwritten in every loop
for(var i = 1; i < 10; i++){
  setTimeout(function(){
    console.log(i); // => all 10
  }, 1000);
}
// but if using let, i is independent in every single loop
for(let i = 1; i < 10; i++){
  setTimeout(function(){
    console.log(i); // => 1~9
  }, 1000);
}
```

Overall suggestions:

1. Use **const** by default
2. Only use **let** if rebinding is needed
3. **var** should NOT be uesd in ES6
