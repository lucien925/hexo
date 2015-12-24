title: CSS与百分比的那些故事
date: 2015-11-11 22:28:32
tags:
---
在CSS中，有许多属性是的量值是可以设置为百分比的，它们通常表示为父级元素的该属性或自身的元素的其他属性的该百分比计算之后得到的值。当我们细细去回想那些可以使用百分比属性值的表现行为的时候，总感觉不是那么清晰，所以，现在我们来总结下CSS与百分比之间的一些奇妙而又有趣的联系。
<!--more-->
## 包含块
在 CSS2.1 中，很多框的定位和尺寸的计算，都取决于一个矩形的边界，这个矩形，被称作是包含块( containing block )，而包含块又是一个相对的概念，包含块的判定标准，我们可以用一张图来看：

{% asset_img container-block.png  Containing-block %}

关于包含块的具体描述可以[点击这里](http://www.w3help.org/zh-cn/kb/008/"Containing block")
## 基于包含块
### 基于包含块的宽度
有这么一类属性，当给它们设置百分比值时，它们的参照是包含块的宽度。

* width
* padding
* left
* right
* text-indent
* max-width
* min-width

### 重点应用
我们可以将width百分比应用在移动端响应式布局上，bootstrap的栅格布局就是很好的应用；
padding百分比可以帮我们实现一个当宽度不确定时，实现宽/高 = 1的布局；
left和top百分比是实现垂直居中的很重要的一部分；
min-width和max-width可以让我们对那些SPA应用或者单页滚动效果有比较好的把控。

### 基于包含块高度

* top
* bottom
* height
* max-height
* min-height

### 基于元素本身
* transform

### 基于元素的字体大小

* line-height

如果一个元素的字体大小为16px,那么如果给他设置line-heigth：100%，那么line-height的计算值为16px.
在这里要提下昨天在知乎专栏里面看到的一个问题:想要让站点的文字默认为两倍行高。下面哪个 line-height 值是最佳实现方式？

* 200%
* 2em
* 2
* double

答案是 2 ，想想为什么吧？

### 基于元素的行高

* vertical-align

### 其他
#### background-position
`background-position`的百分比有两个值，分别为水平方向和垂直方向的值，当使用百分比的时候，元素的位移会同时应用于元素和图像，比如`background-position: 50% 50%`将相当于将元素的（50% 50%）位置和图像的（50% 50%）对其。
#### font-size
当给`font-size`设置百分比的时候，他的计算值是相对其父级元素的font-size的值来计算的。比如：

	<div class="parent">
		<div class="child"></div>
	</div>
	.parent {
		font-size: 20px;
	}
	.child {
		font-size: 110%; //大小为：20px*110% = 22px
	}

## 总结
CSS百分比单位的知识点是一个很常用的知识点，把日常用到的css属性的百分比单位做一个总结来帮助自己能在今后项目代码中，更加灵活的运用。

