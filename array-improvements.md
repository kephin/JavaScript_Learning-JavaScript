# What's new about Array?

### Array improvements

1. Array.from(): will take arrayish data and turn it into a true array, and it also takes a second argument which is the map function

  ```javascript
  //DOM collection
  const people = document.querySelectorAll('p');
  const peopleArray = Array.from(people, person => person.textContent);
  console.log(perpleArray);

  //arguments
  function sumAll(){
    return Array.from(arguments).reduce((prev, next) => prev + next, 0);
  }
  ```
2. Array.of()

  ```javascript
  const ages = Array.of(12,4,23,62,34);
  console.log(ages); // => [12,4,23,62,34]
  ```
3. .find() and : when you want to find specific data that comes back from API

  ```javascript
  const posts = [
     {
        "code":"BAcyDyQwcXX",
        "caption":"Lunch #hamont",
        "likes":56,
        "id":"1161022966406956503",
        "display_src":"https://scontent.cdninstagram.com/1443393332.jpg"
     },
     {
        "code":"BAcJeJrQca9",
        "caption":"Snow! #lifewithsnickers",
        "likes":59,
        "id":"1160844458347054781",
        "display_src":"https://scontent.cdninstagram.com/735653395.jpg"
     },
     ...
  ]

  const code = 'BAcJeJrQca9';
  const post = posts.find(post => post.code === code); // return single object find first
  const multiple_posts = post.filter(post => post.likes == 60); // return an array of objects
  ```
4. .findIndex(): when you wnat to remove data that comes back from API

  ```javascript
  const postIndex = posts.findIndex(post => post.code === code); // eturn the index of the object
  ```
5. .every() and .some()

  ```javascript
  const ages = [32, 15, 19, 12];
  const allAdults = ages.every(age => age >= 18); // => false
  const atLeastOneAdults = ages.some(age => age >= 18); // => true
  ```

6. Filter method

```javascript
const ages = [19, 39, 40, 20, 30, 66, 22, 13, 9, 79];
const young = ages.filter(age => age <= 18);
```
