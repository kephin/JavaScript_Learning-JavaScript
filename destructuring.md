#What destructing brings us convenience?

###Basic of Destructing
:fire: Destructing allows us to extract multiple data from arrays, objects and maps, sets into our own variables at a time.

1. Destructing objects

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
  //You can rename from the original object properties
  const {github: gh, linkedIn: li} = kephin.links.career;
  console.log(gh, li); // => https://github.com/kephin, https://www.linkedin.com/kevinhsaio

  //Default settings
  const settings = {
    width: 400,
    height: 300
  };
  const {width = 200, height = 200, color = 'blue', fontSize = 24} = settings;
  
  //What is this?
  const {w: width = 200, h: height = 100} = {w: 500};
  console.log(width, height) // => 500, 100
  ```
2. Destructing arrays

  ```javascript
  const info = ['Kevin', 179, 70];
  const [name, height, weight] = info;
  ```

### Tricks using destructing
3. Swapping values

  ```javascript
  let frontend = 'Kevin';
  let backend = 'Sean';

  [frontend, backend] = [backend, frontend];
  ```
4. Destructing functions - multiple returns and named defaults

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
  const {USD, GPB, AUD, MEX} = convertCurrency(100);
  console.log(USD); // => 76

  //You can pass an object to an function, so that the order of the parameters is non-sensitive.
  function tipCalc({ total = 100, tip = 0.15, tax = 0.13 } = {}) {
    return total + (tip * total) + (tax * total);
  }
  console.log(tipCalc({ tip: 0.20, total: 200 })); // => 266
  ```
