# The magic of Spread Operators and Rest Parameters

### What is spread operators for

:fire: Spread operator(only for **iterables**): allows an expression to be expanded in places where **multiple arguments**(for function calls) or **multiple elements**(for array literals) or **multiple variables**(for destructuring assignment) are expected

```javascript
//multiple arguments (for function calls)
myFunction(...iterableObject);

//multiple elements (for array literals)
[...iterableObject_1, ...arr, ...'kevin', 'Hsiao', 5]

//multiple variables (for destructuring assignment)
for(const [...iterableObject, index] of Object.entries(post)){...}

//truely copy an array(Not a refferenced one)
const arr = [1, 2, 3];
const arr_2 = [...arr]; // like arr.slice()

//better push
const arr_1 = arr_2.push(...arr_1);
```

Turn a nodelist into an array, just like Array.from()

```javascript
const people = Array.from(document.querySelectorAll('.people p'));
const people = [...document.querySelectorAll('.people p')];
```

Examples

```html
<body>
  <h2 class="jump">SPREADS!</h2>
  <script>
  const header = document.querySelector('.jump');
  header.innerHTML = spanWrap(header.textContent);

  const spanWrap = word => [...word]
    .map(letter => `<span>${letter}</span>`).join('');
  </script>
</body>
```

### And rest parameters

:fire: Rest parameters allows us to represent an indefinite number of arguments as an array.

It creates an actual array from the arguments passed to a function, so we can use methods like reduce() or sort() on it. That is to say, it turns a **comma separated list** into an **array**.

```javascript
const add = (...numbers) => numbers.reduce((a, b) => a + b);
console.log(add(2, 3, 4, 5, 6, 7)); // => 27

const add = (...numbers) => numbers.sort((a, b) => a - b);
console.log(sort(2, 11, 4, 24, 6, 1)); // => [1, 2, 4, 6, 11, 24]
```
  
### Conclusion

:fire: Spread operator take an iterable and unpack it into multiple items. However rest parameters do the exactly opposite, it takes multiple things, and pack it into an array.