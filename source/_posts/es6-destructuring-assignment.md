title: ECMAScript6系列之结构表达式
date: 2015-12-07 21:54:42
tags:
- Javascript
- ESMAScript6
---
**ECMAScript6解构表达式** 允许你使用类似数组或对象字面量的方式将数组和对象的属性赋值给变量。这种赋值语句非常简洁明了,我们可以用解构表达式来帮助我们书写更加“清晰”的代码。
<!--more-->
## 介绍
使用解构表达式是间非常简单的事情，我们只需要用类数组(解构类似数组，但其实是对应解构数组的一种解构)或类对象字面量的方式来将数组、对象字面量或迭代器(Iterator)的值赋值给解构解构中的变量，比如：
```
let [name, age, gender] = ['summer', 20, 'man'];
let {name, age, gender} = {name: 'summer', age: 20, gender: 'man'};
```
上面的代码就是使用解构表达式来对需要赋值的变量进行解构赋值。
## 对数组的解构
这种方式应用在嵌套较深的数组的时候显得非常游刃有余。
```
let [a, [b, [c, d]]] = [1, [2, [3, 4]]];
// 相当于
const arr = [1, [2, [3, 4]]];
let a = arr[0], b = arr[1][0], c = arr[1][1][0], d = arr[1][1][1];
```
可以通过留空来对解构的值用变量赋值：
```
let [, , gender] = ['summer', 20, 'man'];
```
还可以通过可变参数对进行赋值：
```
let [a, ...b] = [1,2,3,4,5];
console.log(a, b);
// 1, [2,3,4,5]
```
如果我们的解构结构(读起来都点绕口)的变量对应的值为空的话，那么这个变量的值为undefined，这个时候可以给变量指定一个默认的值:
```
let [name] = [];
console.log(name); //undefined
let [name = 'summer'] = [];
console.log(name); //'summer'
```
当对除数组外，其他的数据类型进行解构，会报错：
```
let [foo] = 1;
let [foo] = false;
let [foo] = NaN;
let [foo] = undefined;
let [foo] = null;
let [foo] = {};
```
### 对对象的解构
通常，我们对对象的解构有两种写法：
```
let {name, age} = {name: 'summer', age: 20};
let {name: nameA, age: ageA} = {name: 'summer', age: 20};
```
上面两种写法取决你是否需要将变量名变的和属性名一样。
对对象的解构在有些方面的写法和对数组的解构类似：
```
// 对空对象的解构
let {name} = {};
console.log(name);
// undefined

let {name, {address, [likes1, likes2]}} = {name: 'summer', details: {address: 'Jiangxi China', likes: ['play basketball', 'listen music']}};
console.log(name, address, likes1, likes2);
// 'summer','Jiangxi China','play basketball','listen music'

```
当尝试解构null或undefined的时候，就会报错：
```
let {name} = null;
// TypeError: null has no properties
```
当解构原始数据类型，将得到undefined.
```
let {name} = 'summer';
console.log(name);
// undefined
```
