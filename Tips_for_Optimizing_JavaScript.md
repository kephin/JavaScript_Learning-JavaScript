# Tips for optimizing JavaScript

### Ternary Conditionals

Ensure ternaries are isolated: Use parentheses to ensure the conditional is checked correctly

```javascript
console.log('Current weapon' + (isArthur ? 'Excalibur' : 'Longsword'))
```

Ternary can take action: Any executable statement can serve as a ternary's response choices.

```javascript
const devide = n => n / 2
const multiply = n => n * 2
const number = isNumber && isPrime ? devide() :
                                     multiply()
```

Build and choose functions on the fly: Ternary provide a different format for picking immediately-invoked functions

```javascript
const number = isNumber && isPrime ? function(n) {
                                       return n / 2
                                     }()
                                     :
                                     function(n) {
                                       return n * 2
                                     }()
```

Multiple actions in Ternaries: each result option provides the opportunity to execute multiple actions

```javascript
let weapon
let helmet
isArthur && isKing ? (weapon = 'Excalibur', helmet = 'Goosewhile')
                     :
                     (weapon = 'Longsword', helmet = 'Iron Helm')
```

Ternary can be nested: A ternary can hold other ternaries within each of the possible response.

```javascript
let weapon
let helmet
isArthur && isKing ? (weapon = 'Excalibur', helmet = 'Goosewhile')
                     :
                     isArcher ? (weapon = 'Longbow', helmet = 'Mail Helm')
                              : (weapon = 'Longsword', helmet = 'Iron Helm')
```

### Logical Assignment: The OR operator

When used in assignment, the OR operator will try to select the first value it encounters that is "truthy.”

The OR operator takes the leftmost “truthy” value, and if none exists, the last “falsy” value.

Logical Assignment: The AND operator

The && operator takes the rightmost “truthy” value or the first “falsy” value.

### Loop Optimization

Use cached values to curtail lengthy, repetitive access to the same data

```javascript
const arr = [1, 2, 3]
for(let i = 0; i < arr.length; i++) {
  // ...
}

for(let i = 0, len = arr.length; i < len; i++) {
  // ...
}
```

### Use Prototype for shared stuff

### Test the speed of the code

```javascript
console.time('Test starts')

console.time('1st Test')
for(let i = 0; i<arr.length; i++) {
  //...
}
console.timeEnd('1st Test')
console.time('2nd Test')
for(let i = 0; i<arr.length; i++) {
  //...
}
console.timeEnd('2nd Test')
console.timeEnd('Test starts')
```

### Speed Averaging

```javascript
function SpeedTest(testImplement, testParams, repetitions) {
  this.testImplement = testImplement;
  this.testParams = testParams;
  this.repetitions = repetitions || 10000;
  this.average = 0;
}

SpeedTest.prototype = {
  startTest: function(){
    let beginTime, endTime, sumTimes = 0
    for(let i = 1, x = this.repetitions; i < x; i++) {
      beginTime = +new Date()
      this.testImplement(this.testParams)
      endTime = +new Date()
      sumTimes = endTime - beginTime;
    }
    this.average = subTimes / this.repetitions
    return `Average execution across ${this.repetitions}: ${this.average}`
  }
}

const test = new SpeedTest(/*...*/)
```


