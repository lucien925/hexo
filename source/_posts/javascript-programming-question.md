title: javascript-programming-question
date: 2016-01-25 14:01:25
tags:
- javascript
- programming
---
在网上看到一些不错的javascript编程题，于是我也想拿来练练手。这篇博客也将会不断更新。
{% asset_img jpq.jpg %}
<!--more-->
### 题目与解答

1. 找出数组中最大的元素(使用Math.max函数)。
```
var arr = [1,2,3,7,4,3,2,4,5];
console.log(Math.max.apply(null, arr)); //7
```
2. 转化一个数字数组为function数组(每个function都弹出相应的数字)。
```
var numberArray = [1,2,3,4,5,6,7], 
	functionArray = [],
	i = -1,
	len = numberArray.length;
while(++i < len) {
	functionArray.push((function(num) {
		return function() {
			console.log(num)
		}
	})(numberArray[i]))
}
```
3. 给object数组进行排序(排序对象是每个元素对象的属性个数)。
```
var i = -1,
	len = obj.length;
obj.sort(function(prev, next) {
	var prevN = Object.keys(prev).length,
		nextN = Object.keys(next).length;
	return prevN === nextN ? 0 : (prevN > nextN ? 1 : -1);
});
```
4. 利用Javascipt打印Fibonacci数(不使用全局变量)。
```
function fibonacci(n) {
	var a = [],
		i = 2,
		j = 0;
	a[0] = 0, a[1] = 1;
	for(; i < n; i++) {
		a[i] = a[i - 1] + a[i - 2];
	}
	while(j < n) {
		console.log(a[j]);
		j++;
	}
}
```
5. 实现如下语法功能：var a = (5).plus(3).minus(6); //2 。
```
Number.prototype.plus = function(num) {
	return this + a;
}
Number.prototype.minus = function(num) {
	return this - num;
}
```
6. 实现如下语法的功能：var a = add(2)(3)(4); //9 
```
function add(num) {
	
}
```