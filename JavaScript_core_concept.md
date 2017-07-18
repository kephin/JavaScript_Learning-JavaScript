## JavaScript開發實戰：核心概念

### 物件、變數與型別

- JS的物件內只有屬性(內有primitive type & reference type)
- 存取不存在的變數 -> Error: Not defined
- 存取不存在的屬性 -> undefined
- 變數在宣告後會自動變成物件容器的屬性(但無法刪除)
- 用畫圖的方式理解called by value reference(非常棒的方法)

  ```javascript
  // Q1
  var a = { 'a1': 1, 'a2': 2 };
  var b = [];
  b.push(a);
  b.push(a);
  b[0]['a1'] = 2;
  b[1]['a1']; // -> 2

  //Q2
  var a = { 'a1': 1, 'a2': 2 };
  var b = [];
  b.push(a);
  b[0]['a1'] = 2;
  a['a1']; // -> 2

  //Q3
  var a = { x: 1 };
  var b = a;
  a = { x: 2 };
  b.x; // -> 1

  //Q4
  var a = { x: 1 };
  var p = a.x;
  p = 2;
  a.x; // -> 1

  //Q5
  var a = { x: 1 };
  var b = a;
  a.x = a = { x: 2 }; // { x: 2 }同時指向a與a.x
  a.x; // -> 2
  b.x; // -> { x: 2 }
  
  //注意！ p是一個變數，指向a變數指向的物件
  //Q6
  var a = 1;
  function test(p) {
    p = 2;
  }
  test(a);
  a; // -> 1

  // Q7
  var a = {'a1': 1, 'a2': 2};
  function test(p) {
    p.a1 = 2;
  }
  test(a);
  a.a1 // -> 2
  ```

- Dynamically create properties in object

  ```javascript
  /*
  [ 課後練習題 ] 請把答案上傳至 Gist 並貼連結上來！
  請完成以下 function a() {} 函式的程式撰寫，讓你的程式可以執行以下程式碼，並且得到預期的結果。
  ※ 傳入數字的範圍只有 0 ~ 9，且回傳型別必須為 number
  a(2)[0](3).run() === 203
  a(2)[3](3).run() === 233
  a(5)[8](2).run() === 582
  a(9)[9](9).run() === 999
  */

  function a(first) {
    const obj = {};
    for (let second = 0; second < 10; second++) {
      obj[second] = function(third) {
        return {
          run() { return Number(`${first}${second}${third}`); },
        };
      };
    }
    return obj;
  }
  ```

### 型別系統

- 只要建立物件就會有型別
- Primitive type的屬性可以在primitive wrapper object內定義

```javascript
Number.prototype.name = 'kevin';
const a = 1;
a.name; // -> 'kevin'
a.name = 'john'; // won't work
a.name; // -> 'kevin'
```

#### primitive type

**1. Number**

  - 精準度只到21位
  - 避免用JS計算小數

```javascript
// String to Number
+'900'; // -> 900
Number('912'); // -> 912
Number('100a'); // -> NaN
parseInt('100a'); // -> 100
parseInt('100,000'); // -> 100
parseInt('$100,000'); // -> NaN

// 轉換二進位
(100).toString(2);
100..toString(2); // don't use it

// 取小數第幾位
+(2.99).toFixed(1); // -> 3

// 最大安全整數
Number.MAX_SAFE_INTEGER
```

**2. String**

**3. Boolean**

|falsy values|
|---|
|false|
|0|
|''(空字串)|
|null|
|undefined|
|NaN|

  - 一律用 === 或 !==
  - 兩個物件不能相比，一定是false

```javascript
var a = !!(''); // -> false
var a = !!('0'); // -> true

var a = [];
var b = a;
a === b; // -> true

var c = [];
a === c; // -> false
```

**4. Null**

  - Don't use Null
  - 判斷物件是否為null

```javascript
if( a === null) {
  //...
}
```

**5. undefined**

  - undefined是型別(內建型別)
  - 也是一個物件(是undefined型別的值)
  - undefined是一個全域變數(也是一個屬性: window.undefined === undefined)
  - undefined cannot be redefined(除了IE8..)

#### Reference type

**Native Objects:** Object, Array, Date, Number, String...

**Host Objects:** window, process, global

- 盡量使用{}宣告，比new Object()好

```javascript
// 用new的問題
new Object(); // Object
new Object('kevin'); // Number
new Object(1); // String
```


Array
- 盡量使用[]宣告，比new Array()好

```javascript
// 用new的問題
new Array(10); // -> [undefined * 10]
new Array(10, 20); // -> [10, 20]
```

Date

- JSON傳回來的時間用GMT的毫秒數比較好

```javascript
new Date(0); // 從1970/1/1在格林威治時區(GMT)開始計算的毫秒數
new Date('2017-07-01'); //GMT
new Date('2017/07/01'); //本地時間
new Date(2017, 6, 15+107, 12, 32, 22);
```

RefExp
- 用 / ... /宣告比new RegExp()好，不然會有需要skip\的問題

### JAVASCRIPT 基礎物件概念

- 所有的物件都必須要跟root object(**window** in brower, **global** in node)有關聯，不然會被回收

- 關於效能調教

  ```javascript
  // $('h2')執行兩次，但是做同樣的事
  $('h2').hide('slow');
  $('h2').show('slow');

  // 做一次就好，將結果的記憶體位址傳給變數h2
  const h2 = $('h2');
  h2.hide('slow');
  h2.show('slow');
  ```

#### Function

- 當宣告function後，已經污染了全域變數

  ```javascript
  // 宣告後，window.func即有值，且無法delete
  var func = function(){ };

  console.dir(func); // 列出所有此function的屬性
  ```

- 盡量用IIFE，不汙染全域變數。並且可適當的釋放變數到全域，像是jQuery的$
- hoisting

  ```javascript
  //Q1
  var book = 'MVC4';
  function MyOldBook() {
    console.log(book);
    var book = 'MVC2';
    console.log(book);
  }
  MyOldBook();
  // will become
  function MyOldBook() {
    var book;
    console.log(book); // undefined
    book = 'MVC2';
    console.log(book); // MVC2
  }

  //Q2
  (function GetBook() {
    function test() { return 2; }
    var test = function(){ return 1; }
    return test(); // 1
  })();
  // will become
  (function GetBook() {
    function test() { return 2; }
    var test;
    test = function(){ return 1; }
    return test(); // 1
  })();

  //Q3
  (function GetBook() {
    var test = function(){ return 1; }
    function test() { return 2; }
    return test(); // 1
  })();
  // will become
  (function GetBook() {
    var test;
    function test() { return 2; }
    test = function(){ return 1; }
    return test(); // 1
  })();
  ```

#### Callback Pattern

```javascript
function get(url, cb) {
  var html = getSomething(url);
  cb(html);
}
```

#### Closure

- 外層函式宣告的變數可以在內層函式中取用

  ```javascript
  function MyFunction(){
    let counter = 0; //counter變數會被保留住(不被回收)，因為內層的function有用到它
    let i; //則會被回收，因為沒用到
    return {
      GetCount(){
        return ++counter;
      }
    };
  }

  //撰寫完成後，必須可以正常執行以下程式碼：
  var o = MyFunc();
  o.GetCount(); // 輸出 1
  o.GetCount(); // 輸出 2
  o.GetCount(); // 輸出 3 ( 依此類推 )

  //若想要回收counter
  o = undefined;
  ```

- Exercise

  ```javascript
  /*
  [ 課後練習題 1 ]
  請依據以下範本撰寫一個 MyFunc 函式，撰寫完成後，必須可以正常執行以下程式碼：
  var o = MyFunc();
  o.get_a() // 回傳 undefined
  o.set_a(3);
  o.get_a(); // 回傳 9
  o.set_a(4);
  o.get_a(); // 回傳 16

  o.get_b() // 回傳 0
  o.set_b(99);
  o.get_b(); // 回傳 99
  */

  function MyFunc() {
      let a;
      let b = 0;
      return {
        get_a() { return a },
        set_a(number) { a = number * number },
        get_b() { return b },
        set_b(number) { b = number },
      }
  }

  /*
  [ 課後練習題 2 ]
  請完成以下 API 設計，讓你的程式可以執行以下程式碼，並且得到預期的結果。串接 add 的次數沒有上限，最後一個 API 是 result() 才會回傳結果。
  $(1).add(2).add(3).add(99).result() === 105
  $(1).add(2).add(3).result() === 6
  */

  function $(baseNumber) {
    let total = baseNumber;
    const obj = {};
    obj.add = function(number) {
      total += number;
      return obj;
    };
    obj.result = function() { return total; };
    return obj;
  }
  ```

### JavaScript物件導向基礎

- Prototype-base Object-Oriented

### 建議看一些知名framework、library的source code
