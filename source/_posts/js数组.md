---
title: js数组学习笔记
tags:
  - 前端
  - javaScript
  - js数组
category: javaScript
date: 2020-07-10 14:31:37
updated: 2020-07-10 14:31:37
---


`js`语言中的数组，与`java`这种强类型语言的数组差别很大。
<!-- more -->

### 1. 数组中可以存放任意类型的元素

```javaScript
var arr = [
    {a: 1},
    [1, 2, 3],
    function() {return true;}
];

console.log(arr);
```
结果输出：
```
 [ { a: 1 }, [ 1, 2, 3 ], [Function] ]
```
### 2. 数组的本质是一种特殊的对象

在使用`typeof`获取数组的数据类型的时候:
```javaScript
console.log(typeof []);
```
输出：
>`object`

数组的特殊性就在于，它的键值是一组连续的整数（0，1，2）
```javaScript
var arr2 = ['abc', 'def', 'ghi'];
console.log(Object,keys(arr2));
```
结果输出：
>`[ '0', '1', '2' ]`

### 3. `length`属性

数组的`length`属性就是数组中最大的数字键+1。清空数组的一个有效的方法就是将数组的`length`设置为0。
```javaScript
var arr3 = ['abc', 'def', 'ghi'];
arr3.length = 0;
console.log(arr3);
```
输出:
>`[]`

### 4. 使用`forEach`方法便利数组
数组有一个`forEach`方法也可以用来遍历数组。
```javaScript
var arr4 = ['abc', 'def', 'ghi'];
arr3.forEach(function (el) {
    console.log(el);
});
```
结果输出：
```
abc
def
ghi
```

### 5. 数组的空位
当数组的某个元素是空值，即数组的两个逗号之间没有任何值，那么称这个数组有空位。
```javaScript
var arr5 = [1, , 3];
console.log(arr5.length);
```
结果为：
>3

数组的空位一定是在两个逗号之间的，当逗号出现在数组的末尾，就不会出现空位。

#### 5.1. 读取数组的空位会得到`undefined`
```javaScript
var arr6 = [, , ,];
console.log(arr6[1]);
```
结果为：
>`undefined`

#### 5.2. 遍历数组时，空位会被跳过
```javaScript
var a = [, , ,];

a.forEach(function (x, i) {
  console.log(i + '. ' + x);
})
// 不产生任何输出

for (var i in a) {
  console.log(i);
}
// 不产生任何输出

Object.keys(a)
// []
```
上面的代码不会产生输出。

#### 5.3. `undefined`在遍历时不会被跳过
```javaScript
var a = [undefined, undefined, undefined];

a.forEach(function (x, i) {
  console.log(i + '. ' + x);
});

for (var i in a) {
  console.log(i);
}

console.log(Object.keys(a));
```
输出结果为：
```
0. undefined
1. undefined
2. undefined

0
1
2

['0', '1', '2']
```

### 6. 类数组对象

如果一个对象的键值都是正整数或0，并且拥有`length`属性，那么这个对象就和数组很想。在语法上被称为“类似数组的对象”。

```javaScript
var obj = {
  0: 'a',
  1: 'b',
  2: 'c',
  length: 3
};
```

上面代码中的`obj`对象拥有全部为正整数或0的键值，还有`length`属性，是一个类数组对象，但它并不是数组，因为他们没有数组特有的方法。例如，通过`obj`对象调用`push`方法：

```javaScript
obj.push('d');
```
报错了:
>`TypeError: obj.push is not a function`

而且，类数组对象的`length`属性是个假的`length`属性，它就是一个普通的对象属性，它不会因为成员的变化而变化。

```javaScript
obj[4] = 'e';
console.log(obj.length);
```
结果为：
>3

在为`obj`属性添加一个元素后，`length`属性并没有变化。这更说明了，`obj`不是一个数组对象。

#### 6.1 通过数组`slice`方法将类数组对象变为真正的数组

```javaScript
var arrObj = Array.prototype.slice.call(obj);

arrObj.push('f');
console.log(arrObj.length);
```
输出结果：
>4

通过将类数组对象作为数组`slice`方法的参数，可以返回一个数组对象，此时，就可以使用`push`方法，并且，`length`属性也会随着成员的变化而变化。

#### 6.2 在类数组对象上使用数组的方法

除了将类数组对象转为数组对象，还有一个方法可以使用数组的方法，可以通过`call()`把数组的方法放到类数组对象上。
```javaScript
Array.prototype.forEach.call(obj, function (el) {
    console.log(el);
})
```
输出的结果为：
```
a
b
c
```
需要注意，这种**将数组的方法"嫁接"到类数组对象上来调用`forEach`的方法性能要比直接在数组对象上调用要差，最好是将类数组对象转化为真正的数组对象，然后去调用`forEach`。**

`js`数组部分的笔记到此为止。