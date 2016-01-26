title: javascript的类型转换
date: 2016-01-25 16:37:04
tags:
- javascript
---
javascript中的数值类型转换非常灵活，这要归咎于它是一门弱类型语言，从被一设计出来就对取值类型不敏感。松散的数据类型给js带来了很大的灵活性，但是与之而来的问题也有很多...
<!--more-->
javascript将根据需要自行转换类型的值，常见的情况有对原始数据类型进行Boolean值转化，数字到字符串之间的转换等等。那么这些转换的规则在某种程度来说，有一定的规则可循。
### 类型转化规则和注意点

我们看下下面的代码:
```
7 + 'is seven'   // => '7 is seven'
10 - 'one'       // => NaN
!'hello'         // => false
```
它们都是不同类型之间的运算或转换。它们的转化规则如下:
{% asset_img jyct.jpg "图片来自《javascript权威指南》"%}
其中的一些转换规则我们需要特别注意:

* 当字符串转化为数字的时候，那些以数字表示的字符串可以转化为数字，也允许字符串前后也可以存在空格，其他的情况得到的转化结果都将为__NaN__。

* 而__true__和__false__转化为数字后的值为1和0。

* undefined、null、0、''、NaN转化为Boolean的值都为false，也有些人称这些值为"假值"。

* __+__在JS中有两种用法，一种是__一元加__，做为一元运算符，主要作为将某种数据类型转为数值类型，比如: `+"46"`;另一种用法是作为__加号__，做字符串连接和计算数值相加之用。

### 显式类型转化和隐式类型转化
两种类型转化从叫法上就可以听出是从类型转化是否"明显、清晰"来判断的。
在JS中，显式类型转化通常就是调用原生构造函数来实现:
```
Number('3')  // => 3
String(undefined) // => "undefined"
Boolean(1) => true
```
而隐式类型转化通常都是通过一些运算符来实现的。上面提到的__一元加__，就是一种隐式类型转换的产生方式。

### 对象转化为原始值
对象转化为原始值无非就是三种: String、Number和Boolean。对象转化为Boolean想对简单，直接都转化为true。我们要分析的是对象转化为String和Number。
#### toString()和valueOf()方法
toString方法是返回一个反映该对象的字符串，而valueOf返回该对象的原始值的表示形式。JS中的对象都有toString()这个方法，因为所有的JS引用类型的对象都继承自Object这个对象。Object的toString()方法返回的是"[object 对象构造函数名]"的形式。很多对象都重写了Object上的toString()方法。数组的toString()返回一个数组元素之间加逗号组成的字符串;函数则返回这个函数的实现定义方式;日期类返还一个可读的日期和时间字符串。

```
[2,3,5].toString() // => "2,3,5"
(function(x) {f(x);}).toString() // => "function(x) {\n f(x); \n}"
new Date(2010, 0, 1).toString() // => "Fri Jan 01 2010 00:00:00 GMT-0800 (PST)"
```
由于大多数对象都无法直接表示为原始值，所以valueOf()只是简单的返回对象本身，而不是一个原始值。日期类调用valueOf()的话会返回自1970年1月1日以来的毫秒数表示。
```
new Date().valueOf() // => 1453724534867
```
#### 转换步骤
javascript对象转化为字符串的转换步骤为:

* 对象调用toString()方法，如果返回的是一个原始值，则将这个原始值转化为字符串返回。

* 如果对象没有toString()方法或者返回的值不是原始值的话，那么对象将调用valueOf()
。如果这个方法存在的话，那么就调用这个方法。如果返回的是原始值，那么将这个原始值转化为字符串，并返回这个字符串。

* 如果上述步骤都无法获得一个原始值的话，那么它将会报出一个类型异常的错误。

javascript对象转化为数值的转换步骤为:

* 如果对象有valueOf()方法，并且这个方法返回一个原始值的话，那么将这个原始值转化为数字返回。 

* 否则，如果对象有toString()方法，那么调用这个方法，如果返回的是一个原始值的话，转换这个原始值为数字并返回。

* 否者，抛出一个类型异常错误。

```
var person = {
	valueOf: function() {
		console.log('valueof');
		return 10;
	},
	toString: function() {
		console.log('toString');
		return 'person';
	}
};
console.log(+person); // => 'valueof' 10
console.log(String(person)); // => 'toString' 10
```
这里存在两种特殊情况:
```
console.log(person + ''); // => 'valueOf' '10'
console.log(person == '10'); // => 'value' true
```
显然上面得出的结果和转化步骤不符，具体原因我将去查证，日后补充。


