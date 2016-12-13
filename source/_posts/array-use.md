---
title: 数组的操作
date: 2016-09-01 17:05:00
categories: 代码如诗，前端如画
tags: javascript
---

### 数组的创建

```javascript
var arrayObj = []; //普通数组创建
var arrayObj = new Array();　//创建一个数组
var arrayObj = new Array([size]);　//创建一个数组并指定长度，注意不是上限，是长度
var arrayObj = new Array([element0[, element1[, ...[, elementN]]]]);　//创建一个数组并赋值
```
### 数组的添加
<!-- more -->

- `push()`方法将一个或多个新元素添加到数组结尾，并返回数组新长度,数组不变
```javascript
var arr = [1]
console.log(arr.push(2)) //2
console.log(arr) //[1, 2]
```
- `unshift()`方法将一个或多个新元素添加到数组头部，并返回数组新长度,数组不变
```javascript
var arr = [1]
console.log(arr.unshift(2)) //2
console.log(arr) //[2, 1]
```
- `splice()`将一个或多个新元素插入到数组的指定位置，插入位置的元素自动后移，返回""。
```javascript
var arr = [1,2,3,4,5]
arr.splice(2,0,'insert') //表示在第二个位置插入，删除0个元素,返回[]
console.log(arr) //[1, 2, "insert", 3, 4, 5]

var newArr = [1,2,3,4,5]
newArr.splice(2,1,'insert') //表示删除第二个位置后的1个元素并插入
console.log(newArr) //[1, 2, "insert", 4, 5]
```
### 数组的删除

- `pop()`移除最后一个元素并返回该元素值
- `shift()`移除最前一个元素并返回该元素值，数组中元素自动前移
- `splice(deletePos,deleteCount)`删除从指定位置deletePos开始的指定数量deleteCount的元素，数组形式返回所移除的元素

### 数组的截取和合并

- `concat()`将多个数组（也可以是字符串，或者是数组和字符串的混合）连接为一个数组，返回连接好的新的数组
```javascript
var a = [1]
var b = [2]
a.concat(b) //[1,2] 既不是a也不是b
```
- `slice(start, [end])`以数组的形式返回数组的一部分，注意不包括 end 对应的元素，如果省略 end 将复制 start 之后的所有元素

### 数组的拷贝
```javascript
arrayObj.slice(0); //返回数组的拷贝数组，注意是一个新的数组，不是指向
arrayObj.concat(); //返回数组的拷贝数组，注意是一个新的数组，不是指向
```

### 数组元素的排序
```javascript
arrayObj.reverse(); //反转元素（最前的排到最后、最后的排到最前），返回数组地址
arrayObj.sort(); //对数组元素排序，返回数组地址
```
### 数组元素的字符串化
`join()`方法是一个非常实用的方法，它把当前Array的每个元素都用指定的字符串连接起来，然后返回连接后的字符串：

```javascript
arrayObj.join(separator); //返回字符串，这个字符串将数组的每一个元素值连接在一起，中间用 separator 隔开。
var arr = ['A', 'B', 'C', 1, 2, 3];
arr.join('-'); // 'A-B-C-1-2-3'
```
### 数组的查找
- indexOf()
- lastIndexOf()
- `find()`方法，用于找出第一个符合条件的数组成员

  find方法的回调函数可以接受三个参数，依次为当前的值、当前的位置和原数组。
  ```javascript
  [1, 5, 10, 15].find(function(value, index, arr) {
    return value > 9;
  }) // 10
  ```
- `findIndex()`返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回-1。

  ```javascript
  [1, 5, 10, 15].findIndex(function(value, index, arr) {
    return value > 9;
  }) // 2
  ```

### 判断是否为数组
- `typeof` 操作符

  ```javascript
  var arr=new Array("1","2","3","4","5");
  alert(typeof(arr));  // Object
  ```
- `instanceof()`运算符会返回一个 Boolean 值，指出对象是否是特定类的一个实例。

  ```javascript
  var arrayStr=new Array("1","2","3","4","5");
  alert(arrayStr instanceof Array);  //true
  ```
- `Array.isArray()`用来判断某个值是否为数组。如果是，则返回 true，否则返回 false。

  ```javascript
  // 下面的函数调用都返回 true
  Array.isArray([]);
  Array.isArray([1]);
  Array.isArray(new Array());
  // 鲜为人知的事实：其实 Array.prototype 也是一个数组。
  Array.isArray(Array.prototype); 
  ```
### 数组迭代

- `filter()`使用指定的函数测试所有元素，并创建一个包含所有通过测试的元素的新数组。

  ```javascript
  function isBigEnough(element) {
    return element >= 10;
  }
  var filtered = [12, 5, 8, 130, 44].filter(isBigEnough);
  // filtered is [12, 130, 44]
  ```
- `forEach()`让数组的每一项都执行一次给定的函数。

  ```javascript
  function logArrayElements(element, index, array) {
      console.log("a[" + index + "] = " + element);
  }
  [2, 5, 9].forEach(logArrayElements);
  // logs:
  // a[0] = 2
  // a[1] = 5
  // a[2] = 9
  ```
- `every()`测试数组的所有元素是否都通过了指定函数的测试。

  ```javascript
  //检测数组中的所有元素是否都大于 10
  function isBigEnough(element, index, array) {
    return (element >= 10);
  }
  var passed = [12, 5, 8, 130, 44].every(isBigEnough);
  // passed is false
  passed = [12, 54, 18, 130, 44].every(isBigEnough);
  // passed is true
  ```

- `map()`返回一个由原数组中的每个元素调用一个指定方法后的返回值组成的新数组。

  ```javascript
  const arr = [1,2,3]
  arr.map((item,index) => {
    console.log(item)
  })
  ```

- `some()`测试数组中的某些元素是否通过了指定函数的测试。

  ```javascript
  //检测在数组中是否有元素大于 10。
  function isBigEnough(element, index, array) {
    return (element >= 10);
  }
  var passed = [2, 5, 8, 1, 4].some(isBigEnough);
  // passed is false
  passed = [12, 5, 8, 1, 4].some(isBigEnough);
  // passed is true
  ```


- `reduce()`接收一个函数作为累加器（accumulator），数组中的每个值（从左到右）开始缩减，最终为一个值。

  语法: arr.reduce(callback,[initialValue]) callback:执行数组中每个值的函数，包含四个参数

  - `previousValue` 上一次调用回调返回的值，或者是提供的初始值（initialValue）
  - `currentValue` 数组中当前被处理的元素
  - `index` 当前元素在数组中的索引
  - `array` 调用 reduce 的数组 initialValue: 作为第一次调用 callback 的第一个参数。

  ```javascript
  var total = [0, 1, 2, 3].reduce(function(a, b) {
    return a + b;
  });
  // total == 6
  ```

- `Array.from()`用于将两类对象转为真正的数组：类似数组的对象（array-like object）和可遍历（iterable）的对象（包括ES6新增的数据结构Set和Map）。
  
  ```javascript
  let arrayLike = {
    '0': 'a',
    '1': 'b',
    '2': 'c',
    length: 3
  };

  // ES6的写法
  let arr2 = Array.from(arrayLike); // ['a', 'b', 'c']
  ```
- `Array.of()`方法用于将一组值，转换为数组。

  ```javascript
  Array.of(3, 11, 8) // [3,11,8]
  Array.of(3) // [3]
  Array.of(3).length // 1
  ```

- `fill()`方法使用给定值，填充一个数组。

  ```javascript
  ['a', 'b', 'c'].fill(7)
  // [7, 7, 7]
  new Array(3).fill(7)
  // [7, 7, 7]
  ```

  `fill`方法还可以接受第二个和第三个参数，用于指定填充的起始位置和结束位置。

  ```javascript
  ['a', 'b', 'c'].fill(7, 1, 2)
  // ['a', 7, 'c']
  ```

### 遍历数组

- ·keys()`是对键名的遍历

  ```javascript
  for (let index of ['a', 'b'].keys()) {
    console.log(index);
  }
  // 0
  // 1
  ```

- `values()`是对键值的遍历

```javascript
for (let elem of ['a', 'b'].values()) {
  console.log(elem);
}
// 'a'
// 'b'
```

- `entries()`是对键值对的遍历。

```javascript
for (let [index, elem] of ['a', 'b'].entries()) {
  console.log(index, elem);
}
// 0 "a"
// 1 "b"
```

- includes()
