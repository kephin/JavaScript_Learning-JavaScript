#Can arrow functions replace the old functions?

### Introduction of Arror function
1. Using arrow functions can have 3 benifits:
  + **concise** than regular functions
  + they have **implicit return** which allows us to write nifty one liners
  + it does NOT re-bind the value of **this**
2. How to use arrow functions

  ```javascript
  const firstNames = ['Kevin', 'Jessie', 'Kiles'];

  //convensional way
  const fullNames = firstNames.map(function(firstName){
    return (`${firstName} Hsiao`);
  });
  //remove 'function' and add 'fat arrow'
  const fullNames_1 = firstNames.map((firstName) => {
    return (`${firstName} Hsiao`);
  });
  //you can even remove the parentheses if there's only one argument
  const fullNames_2 = firstNames.map(firstName => {
    return (`${firstName} Hsiao`);
  });
  //if the function simply returns a value and there is nothing else in the function body
  //you can just remove the `braces` and `return` with implicit returns
  const fullNames_3 = firstNames.map(firstName => `${firstName} Hsiao` );

  //but if you don't have any parameter, empty parenthesis are used as a placeholder
  const fullNames_4 = firstNames.map(() => `Kevin Hsiao` );
  //or using underscore
  const fullNames_5 = firstNames.map(_ => `Kevin Hsiao` );
  ```
3. Arrow functions are anonymous, you can however put them in a variable

  ```javascript
  const sayMyName = (name) => {alert(`Hello, ${name}!`)};
  sayMyName('Kevin');
  ```
4. Implicit return with an object literal, we put a parentheses around the object

  ```javascript
  const race = 'tennis';
  const winners = ['Novak Djokovic', 'Roger Federal', 'Rafael Nadal'];
  const win = winners.map((winner, i) => ({name: winner, race, place: i + 1}));
  console.table(win);
  ```
5. Filter method

  ```javascript
  const ages = [19, 39, 40, 20, 30, 66, 22, 13, 9, 79];
  const young = ages.filter(age => age <= 18);
  ```
6. :fire: In arrow functions, 'this' does not get rebonded; Instead, 'this' inherits the value of 'this' from the parents. So we don't have to worried about the scope changing.

  ```javascript
  const box = document.querySelector('.box');
  box.addEventListener('click', function(){
    this.classList.toggle('opening'); //'this' gets rebounded to the box element
    setTimeout(_ => {
      this.classList.toggle('open'); //'this' inherits from its parents, which is the box element
    }, 500);
  });
  ```
7. Default functions argument

  ```javascript
  function calculateBill(total, tax = 0.13, tip = 0.15){
    return total + (total * tax) + (total * tip);
  };
  const totalBill = calculateBill(100);
  // pass undefied to use default value (this is actually how ES6 check)
  totalBill = calculateBill(100, undefied, 0.3);
  ```

### When NOT to use arrow functions
1. When you want to bind "this" with the element by event

  ```javascript
  button.addEventListener('click', () => {
    console.log(this === window); // => true
  });
  button.addEventListener('click', function(){
    console.log(this === button); // => true
  });
  ```
2. When you want to use argument

  ```javascript
  const getRandomNumber_arrow = () => {
    return arguments[0]; // => "Error: arguments is not defined
  };

  const getRandomNumber = function() {
    return arguments[0];
  };

  getRandomNumber(1, 2, 3); // => 1
  ```
3. When defining methods on an object

  ```javascript
  //By calling the method, 'this' becomes the object that method belongs to.
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
      this.hobbies.forEach((hobby) => {
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
  const cat = new MyCat('Mew');
  cat.sayCatName(); // => 'Mew'
  ```
4. Invoking constructors

  ```javascript
  const Message = (text) => {
    console.log(this); // => undefined
  };
  const Message = function(text) {
    console.log(this); // => object
    console.log(this instanceof Message); // => true
  };
  ```

###Others
  + What is the data type of `document.getElementsByTagName`? => NodeList, which is **NOT** an array
  + Get all data-* attribute by .dataset
  + Exircise:

    ```javascript
    // Select all the list items on the page and convert to array
    // Filter for only the elements that contain the word 'flexbox'
    // map down to a list of time strings
    // map to an array of seconds
    // reduce to get total

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
  