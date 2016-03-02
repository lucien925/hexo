title: javascript中的this指向问题
date: 2015-12-25 19:25:27
tags:
- javascript
- this
---
Javascript中的this指向问题应该算是前端面试中最喜欢被问到的问题之一吧！网上相关的文章数不胜数，做为前端新人，自己也稍微的扯下对Javascript中this的指向的一些理解和总结吧！
<!--more-->
### 前言
当调用一个函数时，这个函数会附加的接受两个参数:arguments(本文不做介绍)和this，大多关于讲解this的文章都是要么说浅了，只是举几个例子就完事了;要么就是说深了，让人看完云里雾里的。我觉得要理入Javascript的this的指向问题，就一定要说到函数的四种调用模式。

### 函数调用
当我们函数只是做为一个函数被调用的时候，函数调用时其内部的this指向(绑定)的是window对象。这个在Javascript被设计之初就被认为是这门语言的一个设计错误。在ES6中，我们可以通过箭头函数来讲函数中的this绑定为函数定义时的上下文。
```
function call_with_function() {
	console.log(this);
}

call_with_function(); //window
```
我们再来看个例子:
```
var message = 'window message';
var log = {
	message: 'log message:',
	output: function() {
		setTimeout(function() {
			console.log(this.message);	
		}, 1000);
	}
}
// "window message"
```
结果为`window message`,这个是很多新手都会犯的一个低级失误，`setTimeout`中的回调函数给人的感觉是通过`log.output()`方法来调用的，那么在这个方法中，this的指向应该是log。其实不然，我们再深入一步去查看代码会发现，this是在`setTimeout`中的回调函数中被调用的，也就是说，这种是函数调用模式，所以其中的this应该是指向window全局对象。
我们可以通过ES6的箭头函数以及后面要讲到的ES5的bind方法来实现原先想要实现的效果:
```
var message = 'window message';
var log = {
	message: 'log message:',
	output: function() {
		setTimeout(() => {
			console.log(this.message);
		}, 1000);
	}
}
log.output()
// "log message"
```
### 方法调用模式
在上面的例子中，`output`是log对象的一个属性，它的值是一个函数表达式，通过`log.output()`可以执行这个函数表达式所代表的函数，我们把这种函数叫做对象的方法。
显然，通过对象调用的方法，其执行时所绑定的this是指向调用这个方法的对象。还是以上面那个例子来说明下:
```
var message = 'window message';
var log = {
	message: 'log message:',
	output: function() {
		console.log(this.message);
	}
}
log.output()
// "log message"
```
个人觉得，这种调用模式是最好理解的，所以点到为止。
### 构造函数调用模式
构造函数调用在写法上只是在函数调用的基础上加了`new`关键字，并用变量接受返回的对象。我们可以说一说我们在构造函数前面使用new关键字的作用就能明白其内部的this指向是谁了。
当使用new关键字时:

* 创建一个新对象

* 将构造函数的作用域赋给新对象

* 执行构造函数中的代码

* 返回新对象
简单来说就是使用new关键字会将构造函数的prototype连接到这个新对象上，并把this指向这个对象。要想理解这几个步骤，就要对作用域的相关知识有所了解，可以自行谷歌。

### apply、call、bind调用模式
Javascript的函数存在**定义时上下文**、**执行时上下文**和**函数上下文可改变**的特征。而`apply、call、bind`这正是利用了函数上下文可改变的特点来给函数内部的this绑定指定对象。
```
var message = 'window message';
var log = {
	message: 'log message:',
	output: function(tip1, tip2, tip3) {
		console.log(tip1 + tip2 + tip3 + this.message);
	}
}
log.output.apply(window, ['this ', 'is ', 'from '])  // "this is from window message"
log.output.call(window, 'this ', 'is ', 'from ')  // "this is from window message"
```
上面两个例子中output内部的this的指向被我们分别使用apply和call改变了，它们的第一个参数是要绑定给this的值。apply的第二个参数是一个参数数组，是一个要传给调用apply的函数的参数数组。而call是从第二个参数开始的参数就是依次要传入给调用call函数的参数列表。
我们要要提到的是ES5函数的bind()方法，它返回一个绑定this到指定对象的新函数，它的第一个参数和apply和call一样，都是要给this绑定的值，后面的参数是要结合新函数所传入的参数一起传入到绑定bind()的函数中。说起来有点绕口，但是理解并不难。它的一个很重要的特征是它返回一个绑定了this的新函数而不是像call和apply一样，绑定并执行。

### 结语
其实要在某个场景下要分辨出当前的this指向的对象是什么，我们只需要仔细的分析下代码，然后找出这个this所在的函数的调用放失是什么就能很好的理解了。其实，这些点并不需要去记忆，结合Javacript的其他知识体系能帮我们更好的掌握这个知识点，说不定还有其他的很多收获。
整片文章是以理论体系为主，并没有结合大量的例子来说明，如果有同学或前辈觉得我在某些说法上有错误或不妥的话，欢迎留言。~~Thanks。

