title: ESMAScript6系列之let-const命令
date: 2015-12-07 21:55:12
tags:
- Javascript
- ESMAScript6
---
`ECMAScript6`在2015年6月份正式定稿，这也就预示着以ECMAScript为语言的核心的javascript进入了一个新的系列。这个系列的文章就记录了我学习的`ECMAScript6`的一些笔记和总结。

<!--more-->
## let-const命令

### 不存在变脸作用域提升
```
## part 1
console.log(a);   //undefined  
var a = 1;
## part 2
console.log(a); //ReferenceError
let a = 1;
## part 3
console.log(a);  //ReferenceError
const a = 1;
```
### 暂时性死区
在ECMAScript6中规定，一旦在代码区块中有变量的声明，那么在变量声明语句之前使用该变量就会报错。
对于在代码块中用`let`和`const`声明的变量来说，改代码块就相当形成了一个`封闭作用域`，凡是在声明命令之前使用这些变量，都是不合法的。这种情况在语法上叫做**暂时性死区**。
```
if(true) {
    name = "淡夏的绿茶";   //ReferenceError
    let name;
}
```

### 不允许重复声明
这个是指我们在一个块级作用域内(注意：在ECMAScript6中，使用`let`和`const`声明的变量会在该代码块中形成块级作用域)用`let`、`const`和`var`声明变量的时候，不能重复使用`let`和`const`声明。
```
var name = "淡夏的绿茶";
let name = "淡夏的绿茶"; // ReferenceError
```
```
function printName (name) {
    let name = "淡夏的绿茶"; // ReferenceError
    return name;
}
```
### 块级作用域
在前面我们也提到了ECMAScript6中块级作用域的一些概念。举几个例子吧：
```
// example 1
function summer () {
    var name = "summer";
    return name;
}

if (true) {
    var age = 20;
}
console.log(name); // ReferenceError
console.log(age);  // 20
```
```
// example 2
if (true) {
    let name = "summer";
}
console.log(name); // ReferenceError
```
第一个例子是说在之前版本中，只存在全局作用域和函数作用域了，所以在出函数中定义的变量都会被绑定在window这个全局对象上，这个会污染全局的命名空间，特别在我们引入大量第三方库的时候。第二个例子是说，我们在if后面的代码块中用let声明了一个变量name，那么name只能在if后面的代码块中被访问。
在ECMAscript6中，给`let`和`const`声明的变量所在代码块形成一个块级作用域，这些变量只能在这个块级作用域中被访问。这个块级作用域只是针对let 和 const声明的变量，对var声明的变量，还是只支持全局作用域和函数作用域。
### const常量
`const`用来声明常量，这个就表示const声明的变量是不能被修改的，而且const变量一旦生命就必须初始化。
当const变量指向一个对象的时候，这个变量保存的是这个对象的内存空间的地址(学过C的同学应该不陌生),所以我们只要保证改地址不变的基础上，可以改变这个对象的内容(属性和方法)。
```
const PI = 3.14;
PI = 3;  // TypeError

const arr = [];
arr.push("summer");
console.log(arr);  // ["summer"]
```

### 全局对象属性
如果用`let`和`const`在全局作用域上声明变量，不会被添加到全局对象上。
```
// global.js

let name = "summer";
console.log(window.name); //undefined
```
