# Can arrow functions replace the old functions?

### Introduction of Arror function

Using arrow functions can have 3 benifits:

1. **concise** than regular functions
2. they have **implicit return** which allows us to write nifty one liners
3. it does NOT re-bind the value of **this** (important!)

How to use arrow functions

```javascript
const firstNames = ['Kevin', 'Jessie', 'Kiles'];

//convensional way
const fullNames = firstNames.map(function(firstName){
  return (`${firstName} Hsiao`);
});
//remove 'function' and add 'fat arrow'
const fullNames = firstNames.map((firstName) => {
  return (`${firstName} Hsiao`);
});
//you can even remove the parentheses if there's only one argument
const fullNames = firstNames.map(firstName => {
  return (`${firstName} Hsiao`);
});
//if the function simply returns a value and there is nothing else in the function body
//you can just remove the `braces` and `return` with implicit returns
const fullNames = firstNames.map(firstName => `${firstName} Hsiao` );

//but if you don't have any parameter, empty parenthesis are used as a placeholder
const fullNames = firstNames.map(() => `Kevin Hsiao` );
//or using underscore
const fullNames = firstNames.map(_ => `Kevin Hsiao` );
```

:fire: If we want to implicit returns with an object, we need to put a parentheses around the object

```javascript
const winners = ['Novak Djokovic', 'Roger Federal', 'Rafael Nadal'];
const win = winners.map((winner, i) => ({ name: winner, place: i + 1 }));
console.table(win);
```

:fire::fire: In arrow functions, **this** does not get rebonded; Instead, **this** inherits the value of **this** from the parents. So we don't have to worried about the scope changing.

```javascript
const box = document.querySelector('.box');
box.addEventListener('click', function(){
  this.classList.toggle('opening'); //'this' gets rebounded to the box element
  setTimeout(_ => {
    this.classList.toggle('open'); //'this' inherits from its parents, which is the box element
  }, 500);
});
```

Default functions argument, we can pass **undefied** to use default value

```javascript
function calculateBill(total, tax = 0.13, tip = 0.15){
  return total + (total * tax) + (total * tip);
};
const totalBill = calculateBill(100);
// pass undefied to use default value (this is actually how ES6 check)
totalBill = calculateBill(100, undefied, 0.3);
```

### When NOT to use arrow functions?

- When you want to bind **this** with the element by event

  ```javascript
  button.addEventListener('click', () => {
    console.log(this === window); // => true
  });
  button.addEventListener('click', function(){
    console.log(this === button); // => true
  });
  ```

- When defining a method on an object. By calling the method, **this** becomes the object that method belongs to.

  ```javascript
  //Case 1: Object literal
  const person = {
    firstName: 'Kevin',
    hobbies: ['Sports', 'Coding'],
    showHobbies: () => {
      console.log(this === window); // => true
    }
  };

  const person = {
    firstName: 'Kevin',
    hobbies: ['Sports', 'Coding'],
    //Shorthand syntax of showHobbies: function(){
    showHobbies() {
      console.log(this === person); // => true
      this.hobbies.map(function(){
        console.log(this === window); // => true
        })
      this.hobbies.forEach( hobby => {
        console.log(this === person); // => true
        console.log(`${this.firstName} likes ${hobby}`);
      });
    }
  };

  //Case 2: Object prototype
  function MyCat(name) {
    this.catName = name;
  }
  MyCat.prototype.sayCatName = () => {
    console.log(this === window); // => true
    return this.catName;
  };
  //use the old school function expression
  MyCat.prototype.sayCatName = function() {
    console.log(this === cat); // => true
    return this.catName;
  };
  ```

- When you want to use arguments

  ```javascript
  const getRandomNumber_arrow = () => {
    return arguments[0]; // => "Error: arguments is not defined
  };

  const getRandomNumber = function() {
    return arguments[0];
  };

  getRandomNumber(1, 2, 3); // => 1
  ```

Invoking constructors

```javascript
const Message = (text) => {
  console.log(this); // => undefined
};
const Message = function(text) {
  console.log(this); // => object
  console.log(this instanceof Message); // => true
};
```

### Exercise

Tips:

1. What is the data type of `document.getElementsByTagName`? => NodeList, which is **NOT** an array
2. Get all data-* attribute by .dataset

Question:

1. Select all the list items on the page and convert to array
2. Filter for only the elements that contain the word 'flexbox'
3. Map down to a list of time strings
4. Map to an array of seconds
5. Reduce to get total

```html
<ul>
  <li data-time="5:17">Flexbox Video</li>
  <li data-time="8:22">Flexbox Video</li>
  <li data-time="3:34">Redux Video</li>
  <li data-time="5:23">Flexbox Video</li>
  <li data-time="7:12">Flexbox Video</li>
  <li data-time="7:24">Redux Video</li>
  <li data-time="6:46">Flexbox Video</li>
  <li data-time="4:45">Flexbox Video</li>
  <li data-time="4:40">Flexbox Video</li>
  <li data-time="7:58">Redux Video</li>
  <li data-time="11:51">Flexbox Video</li>
</ul>
```

Answer:

```javascript
//WesBos
const items = Array.from(document.querySelectorAll('[data-time]'))
  .filter(item => item.textContent.includes('Flexbox'))
  .map(item => item.dataset.time) // data-time
  .map(timecode => {
    const parts = timecode.split(':').map(part => parseFloat(part));
    return (parts[0] * 60) + parts[1];
  })
  .reduce((runningTotal, seconds) => runningTotal + seconds, 0);

//Kevin
const fullData = Array.from(document.getElementsByTagName('li'))
  .filter(data => data.innerText.includes('Flexbox'))
  .map(data => data.getAttribute('data-time'))
  .map(data => {
    const seperate = data.split(':');
    return seperate[0] * 60 + seperate[1] * 1;
  })
  .reduce((prev, curr) => prev + curr, 0);
```
