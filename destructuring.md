# What destructing brings us convenience?

:fire: Destructing allows us to extract multiple data from arrays, objects and maps, sets into our own variables at a time.

### Array Destructing

```javascript
const [a, b, c] = ['hello', 'world', 'I am kephin'];

// default values
const [a=1, b=2, c=3] = [1, , 3];

// swapping values
[a, b] = [b, a];

// ignoring some returned values
const getNumbers = () => [50, 100, 150, 200];
const [a, , b, c] = getNumbers();  // a -> 50, b -> 150, c -> 200
```

### Object Destructing

```javascript
const kephin = {
  firstName: 'Kevin',
  lastName: 'Hsaio',
  links: {
    social: {
      facebook: 'https://facebook.com/kevinhsaio'
    },
    career:{
      github: 'https://github.com/kephin',
      linkedIn: 'https://www.linkedin.com/kevinhsaio'
    }
  }
};

const { github, linkedIn } = kephin.links.career;

// you can rename from the original object properties
const { github: gh, linkedIn: li } = kephin.links.career;

// assignment without declaration should braces around the statement
let a, b;
({ a, b } = { a: 1, b: 2 });

// default values
var {a = 10, b = 5} = { a: 3 }; // a -> 3, b -> 5

//What is this?
const {w: width = 200, h: height = 100} = {w: 500};
console.log(width, height) // => 500, 100
```

Exercise: Destructing functions - multiple returns and named defaults

```javascript
function convertCurrency(amount) {
  const converted = {
    USD: amount * 0.76,
    GPB: amount * 0.53,
    AUD: amount * 1.01,
    MEX: amount * 13.30
  };
  return converted;
}
const hundo = convertCurrency(100);
console.log(hundo.USD);
// Multiple returns
const { USD, GPB, AUD, MEX } = convertCurrency(100);
console.log(USD); // -> 76

//You can pass an object to an function, so that the order of the parameters is non-sensitive.
function tipCalc({ total = 100, tip = 0.15, tax = 0.13 } = {}) {
  return total + (tip * total) + (tax * total);
}
console.log(tipCalc({ tip: 0.20, total: 200 })); // -> 266
console.log(tipCalc()); // -> 128
```
