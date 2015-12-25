title: 学习ECMAScript6系列之箭头函数
date: 2015-12-24 22:05:17
tags:
---
**Arrow Function**让我想起了python的`lambda`函数,其实很多语言都支持lambda函数，包括javascript,但是javascript的lambda函数就显得那么的无味。es6的Arrow Function的到来这改变了我们书写函数表达式的方式，也给javascript带来了真正的lambda函数式。
<!--more-->
### 介绍
下面通过一小段简单的代码来体验下 Arrow Function.
```
$('#id').click(function() {
	$(this).each(function(o) {
		console.log(o);
	})
})
```
这个是在ES5中的一段jQuery写法，但是在ES6中可以这样写：
```
$('#id').click(() => {
	$(this).each(o => {
		console.log(o)
	})
})
```
什么感觉，不知道你是什么感受，反正我很爽，不用写那个又长看起来又很怪(cuo)的`function`。

### 语法
我觉得Arrow Function 算是所有ES6的features中最简单的一个，简单又实用(简直就是我的小水杯)，我都有点按捺不住心中的小激动。还是赶紧快快的进入语法学习吧！
Arrow Function 语法可以简单的概括为：

	ArrowFunction : ArrowParameters => ConciseBody

相当任性的简洁，用前面的那个栗子来说明一下。
click方法闯入的回调函数就是一个`ArrowFunction`，那么在那么ArrowFunction中的括号就是一个`ArrowParameters`,那么那个大括号括起来的自然就是`ConciseBody`(官方给的名称，译为:简洁的函数体)。ArrowParameters和ConciseBody之间用一个**=>**相连。这就组成了一个ArrowFunction。当然还有一些注意点。
如果我们的`ArrowParameters`的参数个数为0，也就是不传入参数的话，那么需要一个空的小括号来表明这是一个无参的函数;如果这个参数为一个，则可以省略小括号；如果参数个数大于一，这一定要使用小括号括起来。
```
$('#id').hover(() => {
	....
})  // 参数个数为0
$('#id').hover(e => {
	...
}) // 参数个数为1，可以省略小括号
$('.class').each((ele, index) => {
	...
}) // 参数为多个，则不可以省略小括号
```
还可以配合reset参数和变量解构一起使用使用:
```
const nums = (...num) => nums
nums(1,2,3,4);   // [1,2,3,4]

const person = ({name, age}) => {
	return 'name:' + name + ',age:' +age;
}
person({name: 'summer', age: 19}) // name:summer,age:19
```
下面来看看`ConciseBody`。
如果我们的ConciseBody就是我们要返回的值的时候，就可以直接在`=>`后面加上你要返回的内容。
```
const value = o => o.value;
//等同于
function value(o) {
	return o.value;
}.bind(this)
```
如果我们要返回一个对象的话，那么这个对象外要包上一堆小括号，以免js解析器将返回的对象解析成函数体:
```
const newObject = () => ({});
//等同于
function newObject() {
	return {};
}
```
### Arrow Function带来的变化
前面我们简单的说了下Arrow Function的一些语法知识点和一些需要注意的点，现在就来分析下 Arrow Function 带来的真正的改变。

* `this`对象是定义的时候所在的对象，而不是使用时所在的对象。
```
const handler = {
	value: 1,
	addValue: function() {
		setTimeout(() => {
			this.value ++
		}, 1000)
	}
}
``` 
这段代码中setTimeout中回调如果是匿名函数(普通函数)的话，那么其中的this是指向全局对象的，因为是函数调用模式(关于this的指向问题可以看我另外一篇博客)，但是使用了箭头函数，那么其中的this指向就是定义的函数所在的对象，也就是`handler`。还有一个问题需要注意的是，在箭头函数中，this其实是不存在，也就是我们利用apply, call , bind等修改函数内部this指向的方法是不管用的。还有`super`,`arguments`,`new.target`这三个变量在建有函数中也是不存在的。

* 箭头函数不可以当做构造函数来使用，也就是说不能使用new命令
```
const Perosn = (name, age) => {
	this.name = name;
	this.age = age;
}
const p = new Perosn('summer', 19);  //TypeError 
```

* 不可以使用`yield`命令，所以箭头函数不能用做Generator函数

### 结语
关于箭头函数的内容就讲这么多吧！其实还有很多箭头函数的使用场景值得我们去归纳总结。文章篇幅比较短，随着日后的学习，新知识的掌握，再对文章进行补充吧！




