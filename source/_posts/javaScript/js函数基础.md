---
title: js函数基础
tags:
  - 前端
  - javaScript
category: JavaScript
date: 2020-07-10 15:06:11
updated: 2020-07-10 15:06:11
---


`js`的函数方法是一种值。使用方法与`java`有很大差别。

<!-- more -->

### 1. 函数的声明方式

#### 1.1. `function`命令声明

例如：
```javaScript
function print(s) {
    console.log(s);
}
```
在代码块中，`function`关键字后面就是函数名，括号中是需要传入的参数，`{}`中的代码函数执行的具体的函数体。

#### 1.2. 函数表达式
函数定义还可以通过变量赋值的方式：
```javaScript
var print = function (s) {
    console.log(s);
}
``` 
这种定义方式，在`function`关键字后，并没有函数名，所以，可以将其理解为，定义了一个匿名函数，并将这个函数的引用返回给了`print`变量。这个匿名函数又称为**函数表达式**。

采用这种方式定义函数时，`function`后可以不跟函数名。如果加上函数名，这个函数名只在函数内部有效。

```javaScript
var print = function p() {
    console.log(typeof p);
};

p();

print();
```
这段代码执行的时候出现了错误：
> ReferenceError: p is not defined

需要注意的一点:**采用函数表达式的方式定义函数时，需要在`{}`后加上`;`，而使用`function`命令声明不需要`;`**.

#### 1.3. `Function`构造函数

```javaScript
var add = new Function(
    'x',
    'y',
    'return x + y'
);

//等价于

function add(x, y) {
    return x + y;
}

```
上面的代码中，`Function`构造函数接收三个参数，前两个为定义的函数的参数，最后一个参数作为要定义的函数的函数体。

需要注意：**可以传递任意参数给`Function`构造函数，只有最后一个参数会被当做函数的函数体**。

#### 1.4. 函数的重复定义

跟其他语言不同，相同的函数定义不会报错，靠后的函数定义会覆盖靠前的函数定义。

```javaScript
function fun() {
    console.log("first declare");
}

function fun() {
    console.log("second declare");
}
```
控制台输出
> second declare

#### 1.5. 函数是一种可执行的值

`js`将函数看作一种值，与其他的值(数字，字符串，布尔值等等)一样，在能用数值的地方都可以使用函数，例如，赋值，将函数作为返回值......**函数只是一个可以执行的的值，此外并无特殊之处**。

```javaScript
function add(x, y) {
  return x + y;
}

// 将函数赋值给一个变量
var operator = add;

// 将函数作为参数和返回值
function a(op){
  return op;
}
a(add)(1, 1)
```
执行结果为：
> 2

#### 1.6. 函数名的提升

同变量名的提升一样，`function`命令声明函数的时候也会将函数声明**整个**提升到代码头部，所以，下面的代码就不会报错了。
```javaScript
fun();

function fun() {}
```
但是，如果是函数表达式的方式声明函数，就会报错。
```javaScript
fun();

var fun = function() {}
```
这段代码相当于：
```javaScript
var fun;
fun();

fun = function() {}
```
可以看到，当使用函数表达式进行函数声明时，被提升的只有变量`fun`,调用`fun()`的时候，变量`fun`还未赋值，所以就报错了。

还有一个点需要注意：当同时使用**函数表达式**和**命令式函数声明**，**由于函数提升，所以最后会应用函数表达式的声明**，例如：
```javaScript
var f = function () {
  console.log('1');
}

function f() {
  console.log('2');
}

f()
```
大致等同于：
```javaScript
var f;
function f() {
  console.log('2');
}

f = function () {
  console.log('1');
}
```
所以，在实际的函数声明中，命令式的声明被`var`赋值语句覆盖，所以在最后调用中输出的是
> 2

### 2. 函数的属性和方法

#### 2.1. 函数的`name`属性

函数的`name`属性会返回函数的名字。
##### 2.1.1. `function`命令式声明函数
```javaScript
function fun() {}

console.log(fun.name); 
```
控制台输出：
> fun

##### 2.1.2. `var`的函数表达式形式
如果是函数表达式的形式，则会输出变量的名字：
```javaScript
var fun = function() {}

console.log(fun.name)
```
控制台输出:
> fun

##### 2.1.3. 有具体函数名的函数表达式形式
如果使用`var`的函数表达式形式声明函数时，`function`关键字后有函数名，那么`name`属性会输出函数名，而不是变量名：
```javaScript
var fffff = function fun() {}

console.log(fffff.name);
```
控制台输出：
> fun

#### 2.2 函数的`length`属性

函数的`length`参数会返回函数定义的参数个数：
```javaScript
var fun = function (a, b) {}
console.log(fun.length);
```
控制台输出：
> 2

函数的`lenth`属性不管函数实际调用的时候传入了几个参数，都只是返回定义的函数个数。

#### 2.3 `toString()`方法

函数的`toString()`方法返回一个字符串，内容是函数的源码。
```javaScript
function fun() {
    console.log("Hello World");
}
console.log(fun.toString());
```
控制台输出：
```
function fun() {
    console.log("Hello World");
}
```
函数内部的注释也可以返回
```javaScript
function fun() {
    // 这是一个函数
    // fun（）函数
}
console.log(fun.toString());
```
控制台输出
```
function fun() {
    // 这是一个函数
    // fun（）函数
}
```

### 3. 函数的作用域

#### 3.1. 函数内部可以读取外部的全局变量

对于顶层函数来说，，函数外部声明的变量就是全局变量，它可以在内部读取。
```javaScript
var v = 1;

function f() {
    console.log(v);
}
f();
```
输出结果：
>1

#### 3.2. 函数外部无法读取函数内部定义的局部变量
```javaScript
function f() {
    var v = 1;
}
console.log(v);
```
输出结果:
>ReferenceError: v is not defined
#### 3.3. 函数内部的变量会覆盖函数外部的同名函数
```javaScript
var v = 1;

function f() {
    var v = 2;
    console.log(v);
}
f();
console.log(v);
```
输出结果：
>2
>
>1

#### 3.4. 函数内部的变量提升
函数内部也会发生"变量提升"现象，在函数内部使用`var`定义的变量，不管在什么位置，变量声明都会被提升到函数体的头部。

```javaScript
function f(x) {
    if (x > 1) {
        var temp = x++;
    }
}
```
这段代码等同于：
```javaScript
function f() {
    var temp;
    if (x > 1) {
        temp = x++
    }
}
```

#### 3.5. 函数本身的作用域

函数本身也是一个值，所以函数也有他自己的作用域，它的作用域和变量一样，就是它所定义时所在的作用域，与它在运行时所在的作用域无关。
```javaScript
var a = 1;
var x = function () {
    console.log(a);
};

function f4() {
    var a = 2;
    x();
}

f4()
```
输出结果为:
>1

上述代码中，函数`x`在函数`f4`的外部定义，所以函数`x`的作用域就为函数`f4`的外部，在`f4`函数内部调用`x()`时，函数`x`不会提取`f4`内部的局部变量`a`，而是从其外部提取全局变量`a`，所以，最后输出的结果为`1`。
```javaScript
function foo() {
  var x = 1;
  function bar() {
    console.log(x);
  }
  return bar;
}

var x = 2;
var f = foo();
f() // 1
```
`foo()`的返回值是函数`bar`，与前面类似，函数`bar`的作用域在`foo`内部，所以在调用`bar()`的时候会去`foo`函数中提取`x`的值，所以最后输出`1`;

### 4. 函数的参数

#### 4.1. 参数省略

在`js`中，函数在运行时传入多少个参数都不会报错，省略的参数会自动变为`undefined`。

```javaScript
function f6(a) {
    return a;
}

console.log(f6(5)) //5
console.log(f6()); //undefined
```
控制台输出：
>5
>
>undefined


需要注意：省略参数时，**无法省略靠前的参数，如果一定要省略靠前的参数，保留靠后的参数，只能通过显式地给靠前的参数传入`undefined`来实现**。

```javaScript
function f7(a, b) {
    return a;
}

console.log(f7(, 5));
console.log(f7(undefined, 5));
```
`f7(, 5)`的调用会报错：
>SyntaxError: Unexpected token ','
`f7(undefined, 5)`的调用输出：
>undefined

#### 4.2. `js`与`java`一样，只有值传递

如果是基本类型，参数传递时，会将作为参数的值拷贝了一份作为参数，所以，参数在函数内部的操作并不会影响原来变量的值。

如果是复杂类型，例如，对象，数组之类的引用变量，在作为参数被传递给函数时，实际上是将变量所指向的地址拷贝了一份传递给了函数，所以，在函数内部的操作实际上也是对原对象本身的操作，所以会对原对象产生改变。

**由于这部分内容和`java`十分相似，所以不再赘述**。

#### 4.3. 可变参数

##### 4.3.1. 使用`arguments`对象访问或操作参数

`js`函数允许传递不定数量的参数，对于不确定个数的参数，需要用`arguments`对象。

可以通过下标来访问函数运行时传入的参数：
```javaScript
function f8(a, b) {
    console.log(arguments[0]);
    console.log(arguments[1]);
    console.log(arguments[2]);
}

f8(1, 1);
```
结果输出：
```
1
1
undefined
```

非严格模式下，`arguments`对象可以被修改。
```javaScript
function f9(a, b) {
    arguments[0] = 4;
    arguments[1] = 5;
    return a + b;
}

console.log(f9(1, 2));
```

输出结果:
>9

但是，在严格模式下，实际结果就不会这样了：
```javaScript
function f10(a, b) {
    'use strict'
    arguments[0] = 4;
    arguments[1] = 5;

    console.log(arguments[0] + arguments[1]);
    return a + b;
}
```
输出结果：
```
9
2
```
可以发现，在严格模式下对`arguments`对象的修改不会反映到函数实际的参数上。

##### 4.3.2. `arguments`是一个参数，但不是数组

虽然可以通过使用`arguments`下标的形式访问函数参数，但是，`arguments`并不是一个数组，而是一个对象，因为，数组专有的方法，不能在`arguments`直接使用。例如`slice`和`forEach`


以上是`js`函数部分的笔记。