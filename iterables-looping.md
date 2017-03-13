#Iterables and looping

###The for loop
1. Original, syntax not good enough

  ```javascript
  const cuts = ['Chuck', 'Brisket', 'Shank', 'Short Rib'];
  for (let i = 0; i < cuts.length; i++) {
    console.log(cuts[i]); // => Chuck, Brisket, Shank, Short Rib
  }
  ```
2. array.forEach(), can't abort or skip the loop -> break or continue dosen't work

  ```javascript
  cuts.forEach((cut) => {
    console.log(cut);
    if(cut === 'Brisket') {
      continue; // doesn't work
    }
  })
  ```
3. for-in loop iterates over all data, including methods or properties added to the prototype

  ```javascript
  Array.prototype.shuffle = function(){
    //...
  }
  cuts.shop = 'MM MEats';
  for (const index in cuts) {
    console.log(cuts[index]); // => Chuck, Brisket, Shank, Short Rib, MM MEats, function(){...}
  }
  ```
4. :fire: for-of loop interates any type of data except objects

  ```javascript
  for (const cut of cuts) {
    if(const cut === 'Brisket') {
      continue;
    }
    console.log(cut);
  }
  ```

###More about for-of
1. Want to get the index in for-of? We can destructuring every returning **cuts.entries()**

  ```javascript
  for (const [index, cut] of cuts.entries()) {
      console.log(`${index + 1}: ${cut}`);
  }
  ```
2. :fire: You can use for-of with any data even though that isn't an array, **as long as it is iterable**, like **DOM collection(Nodelist)**, **arguments**, **string**, **array**, **generator**, **map**, **set**...etc. However you can turn those arrayish data into a true array using Array.from() and then applied those built-in methods

  + Examples:
    ```javascript
    //for-of arguments
    function addUpNumbers() {
      let total = 0;
      for (const num of arguments) {
        total += num;
      }
      return total;
    }
    addUpNumbers(10,23,52,34,12,13,123);

    //for-of string
    const name = 'Wes Bos';
    for (const char of name) {
      console.log(char);
    }

    //for-of DOM collection
    const ps = document.querySelectorAll('p');
    for (const paragraph of ps) {
      paragraph.addEventListener('click', function() {
        console.log(this.textContent);
      });
    }
    ```

###for-of objects
  + Using Object.entries() and destructuring every key-value pair
  
  ```javascript
  const apple = {
    color: 'Red',
    size: 'Medium',
    weight: 50,
    sugar: 10
  };

  for (const [key, value] of Object.entries(apple)) {
    console.log(key + ': ' + value); // "color:Red", "size:Medium"...
  }
  ```
  