title: 梳理CSS3-animation的知识点
date: 2015-12-28 11:27:04
tags: 
- CSS3
- animation
---
最初开始学习前端的时候，看懂CSS3的animation的实现的动画效果和编写animation动画代码的简洁名了，惊叹不已。转眼，学习前端已经有一年多的时间，使用CSS3动画也有段时间了，其中的知识点，也有必要做个梳理了。
<!--more-->
### 前言
CSS3动画在现在的各种节日活动的业务中使用非常普遍，较好的体验效果和比较低的性能损耗让CSS3逐渐的取代了flash来完成一些复杂的动画效果，尤其是移动端，各大平台对CSS3动画的支持也让它成为主流的选择。
其实在CSS3中，我们可以通过translate和transform的组合来实现一些简单的动画的效果，但是对于应用场景比较复杂的动画效果，这种组合也就力不从心了。所以在我们在讲到CSS动画的时候大部分都是指CSS3的animation。这篇文章也是如此。
### 介绍
简单的从w3c上面扒了下CSS3 animation的属性:

* animation-name

* animation-duration

* animation-delay

* animation-iteration-count

* animtion-direction

* animation-play-state

* animation-fill-mode

* animation-timing-function

浏览器的支持程度:
{% asset_img support.png %}

### 使用
上面列举的属性有些真的是太简单了，baid都有一大堆，就别google了，下面介绍的都是自己有时会用错的或者是有些需要注意使用的属性。
`animation-iteration-count`是指定动画要执行的次数，可以给定一个大于0的正整数n,或者是infinite,表示无限次播放。
`animation-fill-mode`是表示动画之外的状态。怎么理解呢？我个人的理解为就是动画执行完成之后的结束之后的状态，好吧，这么说还是有点绕口，通过各例子来说明下。

查看效果: (在线查看)[http://sandbox.runjs.cn/show/5lqxwwfn]

`animation-play-state`是指定animation的执行状态的。它有两个可选值running和pause。它们分别表示动画的执行态和暂停态，默认是running。

`animation-timing-function`是**检索或设置**动画的过度类型，这是官方版的解释，简单的所就是动画执行的速率，这里面有什么ease、ease-in等等的，还可以指定三次贝塞尔曲线函数来作为值。

animation根具所给定的值来决定动画在执行时间内的运动速率轨迹。这里值得一提是一个叫做steps(number, [start|end])的函数值。在W3CSchool的介绍文档里面并没有发现有这个值的介绍，但是在实际运用中，却发现了很多steps的运用场景，所以，这里重点提一下。
文档上称之为阶跃函数(stepping function)。step函数可以接受两个值来作为参数，第一个是一个数值(大于0的正整数)来指定在每个关键帧之间所发生的阶跃次数;第二个是一个可选值，指定的值可以是end或者start，分别表示在阶跃间隔的开始和结束发生阶跃变化。
这么说还是不太好理解，我们用代码来描述下:

```css
.step {
	animation-name: step;
	animation-duration: 10s;
	animation-timing-function: steps(3, end);
	animation-iteration-count: infinite;
}

@keyframes step {
	0% {
		background: red;
	}
	100% {
		background: black;
	}
}
```
效果查看: (查看效果)[http://sandbox.runjs.cn/show/cstdcahv]
我们发现动画帧的变化跳过了黑色的那个关键帧。如果把steps的第二个参数该为start,那么相应的显示为红色的那个关键帧也会被直接跳过不显示。

我们用张W3C上的图来解释下上面出现结果的原因:

{% asset_img step.png %}

我们可以根据上面的实例代码总结出来,steps的第一个参数是把keyframes中指定的关键帧间又做了个切分，第二个参数是更具第一个参数对切分后的总关键帧数来做起点帧或终点帧的跳过。

**steps的第一个参数是作用于keysframe的两个关键帧之间的，而不是整个keyframes**
这点是个要认真注意的误区。
比如说，上面的代码中，我们给steps的第一个参数的值为3，那么step的keyframes就被划分为0%, 50%, 100%三个关键帧。如果上面的step的keyframes改为:

```
@keyframes step {
	0% {
		background: red;
	}
	50% {
		background: green;
	}
	100% {
		background: black;
	}
}
```
那么他的关键帧切分就为：0%, 25%, 50%, 75%, 100%。确保在keyframes中定义的两个关键帧之间切分后能表示为3个关键帧。

animation-timing-function还可以指定为step-start或step-end，它们等同于steps(1, start)和steps(1, end);

### 结语
本来是想好好写下animation的一些知识点的，但是写完之后发现这篇文章感觉写的有点打酱油的味道了。但是还是作为一次小小的知识总结来写吧！虽说不够详细，但是也是个人对于一些基础知识点的把握程度来进行梳理，有些就一笔带过，有的就讲下自己的理解。
日后如果发现关于CSS3 animation的有趣的知识点的话，还是会再补充的。  ~~~完。




