# The magic of Spread Operators and Rest Parameters

### What is spread operators for?

:fire: Spread operator(only for **iterables**) allows an expression to be expanded in places where **multiple arguments**(for function calls) or **multiple elements**(for array literals) or **multiple variables**(for destructuring assignment) are expected.

```javascript
// multiple arguments (for function calls)
const arg = [1,2,3];
myFunction(...arg);

// multiple elements (for array literals)
const all = [...arr, ...'kevin', 'Hsiao', 5];
```

Copy an array

```javascript
// truely copy an array (Not a refferenced one)
const arr = [1, 2, 3];
const arr_2 = [...arr]; // like arr.slice()
```

Better way to concatenate arrays

```javascript
const arr_1 = arr_2.concat(arr_1);
const arr_1 = arr_2.push(...arr_1);
arr1 = [...arr1, ...arr2];
```

Turn a nodelist into an array, just like Array.from()

```javascript
const people = Array.from(document.querySelectorAll('.people p'));
// equals to
const people = [...document.querySelectorAll('.people p')];
```

Examples

```html
<body>
  <h2 class="jump">SPREADS!</h2>
  <script>
  const header = document.querySelector('.jump');
  const spanWrap = word => [...word]
    .map(char => `<span>${char}</span>`).join('');

  header.innerHTML = spanWrap(header.textContent);
  </script>
</body>
```

### Rest parameters

:fire: Rest parameters allows us to represent an indefinite number of arguments as an array.

It creates an actual array from the arguments passed to a function, so we can use methods like reduce() or sort() on it. That is to say, it turns a **comma separated list** into an **array**.

```javascript
const add = (...numbers) => numbers.reduce((a, b) => a + b, 0);
console.log(add(2, 3, 4, 5, 6, 7)); // => 27

const add = (...numbers) => numbers.sort((a, b) => a - b);
console.log(sort(2, 11, 4, 24, 6, 1)); // => [1, 2, 4, 6, 11, 24]
```
  
>Spread operator take an iterable and unpack it into multiple items. However rest parameters do the exactly opposite, it takes multiple things, and pack it into an array.