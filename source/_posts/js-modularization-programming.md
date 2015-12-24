title: 学习javascript 模块化的总结(一)
date: 2015-11-15 14:59:13
tags:
- JavaScript
categories: 前端
---
最近在看React的东西，里面提到了现在最流行的前端模块加载器`webpack`和`browersify`。它们都是遵循AMD规范的模块加载器，也是前端工程化的一个重要标杆，所以写下这篇博客，算是这个阶段对Js模块化编程的一点小小梳理和总结吧!
<!--more-->
## 模块化历程
Long time ago，我们编写Javascript代码都是这样的：

	function a() {
		console.log('this is a')
	}
	function b() {
		console.log('this is b')
	}
	function c() {
		console.log('this is c')
	}
	..... ):
这样编写代码会出现很多问题，比如全局作用域的污染，于是乎，又出现了一种新的写法,叫做`立即执行函数`(Immediately-Invoked Function Expression,IIFE) ：

	var a = (function() {

			function b() {
				...
			}
			function c() {
				...
			}
			return {
				fb: b,
				fc: c
			}
		})();
它利用了函数可以在其内部创建一个局部作用域的特性来创建一个闭包，将所有要执行的代码绑定在其内部的活动对象上,我们还可以传入输入全局对象来配置依赖：

	var a = (function(_) {
		_.filter(....)

		})(underscore)
## 利用Script标签加载

上面的IIFE帮我们解决了Javascript依赖于全局作用域来进行连接的问题，实现了封装。好吧，接着问题又来了，我们组有a、b、c、d、e、f、g各自写了一个模块，那么我们在项目main中引用它们:

	//code in project main
	<script src="a.js"></script>
	<script src="b.js"></script>
	<script src="c.js"></script>
	<script src="d.js"></script>
	<script src="e.js"></script>
	<script src="f.js"></script>
	<script src="g.js"></script>
这里有几个问题：
* 难以维护
* 如果这些模块之间有明确的调用关系的话（比如f需要用到a中的代码），那么必须保证调用的要加载在被调用的后面
* 我们需要向服务器请求7次，这个，你懂得

## CommonJS规范
CommonJS最先实现的在服务端，Let's check it:

	// add.js
	function add(a, b) {
		return a + b
	}
	exports.add = add

	// main.js
	var method = require('./add')
	method.add(1, 2) // 输出为 3
Node.js也是实现了CommonJS的规范来加载模块(详情看官网)，然而CommonJS的规范是使用在服务端([CommonJS](http://www.commonjs.org/ 'CommonJS'))。CommonJS主要分为模块引用、模块定义和模块标识。
### 模块引入
CommonJS定义了一个require()的全局函数来导入我们定义的模块倒当前上下文中。
### 模块导出
当我们写了一个模块后，想把它导出来，我们就使用module和exports，exports是module的属性，而module代表当前模块。

	//module
	function a() {
		console.log('a')
	}

	module.exports = a  // var _a = require('a')  a()
	 // or
	exports.a = a       //var _a = require('a') _a.a()
### 模块标识
模块标识就是传递给require()的参数，它必须符合小驼峰命名法，或者是以** .、.. ** 开头路径或绝对路径，若以.js结尾的可以省略扩展名。
## AMD
在服务端我们可以使用CommonJS所规定的模块加载方式来加载模块，那么在浏览器端是否也用CommonJS来实现加载端模块呢？答案是否定的，在sever端，由于服务器的性能各方面要强与client端，而且在后端加载模块是相当于在本地加载，加载速度杠杠的。那么我们在浏览器上加载这些模块，一方面，我的模块代码都在服务器上，各种请求解析加载什么的，异步加载模块的方式显然用CommonJS的标准是很难实现的，那么需求来了，我们如何实现在浏览器端实现模块的加载，AMD来了。

在AMD规范下，我们定义模块是这样的：

	define(['module/moduleA','module/moduleB'], function(moduleA, moduleB) {
		//  ...
		var a = moduleA.methodA();
		var b = moduleB.methodB();
		return {
			a: a,
			b: b
		}
	})
define函数用于定义模块，第一个参数定义我们要定义的模块所依赖的模块，第二个参数传入传入一个回调函数，在依赖模块加载完成后传入回调函数中使用。让后通过require函数调用：

	require(['moudle/moduleC'], function(moduleC) {
		console.log(moduleC.a, moduleC.b);
	})

### Require.js

Request.js是一个实现了AMD的前端模块加载库。
关于Require.js的使用方法就不多说了，查看[中文Docs](http://www.requirejs.cn/)。需要提的是require.js用了一种在CommonJS中来定义模块的方式来方便我们加载CommonJS规范的模块：

	define(function(require, exports, module) {
		var a = require('module/moduleA');
		exports.a = new a();
	})
