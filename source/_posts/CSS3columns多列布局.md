title: CSS3 Multi Column 多列布局
date: 2015-11-10 18:19:08
tags:
- CSS
- HTML
categories: 前端
---
多行布局可以很方便的使用css3所提供的Multi Column来帮我们创建。在现在大力推崇flex-box的时代，我们也应该不要忘了CSS3在布局以及多行布局上给我们带来的惊喜，所以今天，我们就来一起看看CSS3的Multi Column.
<!-- more -->
## 介绍

随着web应用的复杂化，我们常常需要在页面中实现比较多的复杂布局，尤其是多列布局，而传统的使用浮动和绝对定位已经很难适应需求的变化了，而且这也不是最简单和省时的。事实上，使用css来实现多列布局一直是一个非常尴尬的选择，但是在最新的CSS3中，css提供了columns来帮助我们更好的来实现多列布局。

## 浏览器支持
我们来看看** columns **的浏览器支持情况(你可以在[Can i use](http://caniuse.com/#search=column"Can i use")中查看)：
* IE 10+
* Firefox 5+
* Chrome 12+
* Safari 3.1+
* Opera 11.5+

其中** Safari **是9.0和** IE **10以上才全部支持** Columns ** ,其他的浏览器版本都只是部分支持,Firefox需要加上-moz-的前缀,Chrome和Safari需要加上-webkit-的前缀.

## 快速开始
### HTML代码
	<html>
		<body>
		<div class="columns-warp">
			这里是一段文字这里是一段文字这里是一段文字这里是一段文字这里是一段文字这里是一段文字这里是一段文字这里是一段文字这里是一段文字这里是一段文字这里是一段文字这里是一段文字这里是一段文字这里是一段文字这里是一段文字这里是一段文字这里是一段文字这里是一段文字这里是一段文字这里是一段文字这里是一段文字这里是一段文字这里是一段文字这里是一段文字这里是一段文字
		</div>
		</body>
	</html>

### CSS代码
	.columns-warp {
		column-count: 4;
    	column-gap: 10px;
	}
### 效果
{% asset_img 1.png %}
我们给外边容器的样式设置`column-count` `column-gap`之后，里面的内容分成了四列，并且他们之间都产生了10px的间距。
## 相关属性介绍
### column-count & column-width
{% asset_img column-width.png %}
我们可以通过设置column-count来设置元素里面的内容的列数，里面的内容将自动根据所设置的内容进行均匀分成4部分排布。`column-width`的作用是设置每列的宽度，这连个属性不适合被一起设置，当设置了`column-count`之后`conumn-width`会失效，相反的，当只设置了`column-width`，那么浏览器会据元素的宽度来除以`column-width`,从而得到`column-count`。
我们可以同过columns属性来定义它们两个:

	.column-wrap {
    	columns: auto 100px; //column-width:100px;column-count:auto
    }

### column-gap
{% asset_img column-gap.png %}
可以通过设置`column-gap`来设置没列之间的间隔，这个数值可以是CSS中的标准单位值，但是不能是负数。
### column-rule
{% asset_img column-rule.png %}
`column-rule`定义了列与列之间的隔离边框样式,他的书写方式和CSS中的border属性书写方式很像，它是三个属性的结合。

	column-rule-width: 10px;
  	column-rule-color: #000;
  	column-rule-style: solid;

同时也可以简写为:

	column-rule: 10px solid #000;

### column-fill
** tips **:这个属性只有Firefox才支持，我们可以在Firefox中查看这个属性所带来的变化。
当我们给一个多行元素** 设置高度 ** 的时候，你可以控制内容填充每一列的方式。它有两个值`auto`和`balance`。当我们设置auto的时候，填充的内容将会尽最大可能的填满整个整列，所以内容每一列的高度加起来就是元素的高度（如比如元素高为100px,而每行的行高为18px,这个时候，每列最多能排5行，还剩 10px的内容没有排满，这个时候，每行的行高就会相应的减小来匹配这第六行的高度能正好填满整列），而当设置为`balance`的时候，一列如果未填满的话，将从另一列开始继续填充。
CSS Ticker上面给了一个非常形象的比喻：每列容器好比一个杯子，而我们将一大瓶橘汁倒进这些杯子里面，我们可以选择一直倒，直到杯子里的橘汁都满到杯口，或者给杯子留点空间，方便我们拿出来，那么前者就是`auto`,后者就是`balance`。
{% asset_img column-fill.png %}
### column-span
`column-span`可以允许我们跨过整行，它有两个值：`column-span:all|none`。
{% asset_img column-span.png %}
	h1 {
		column-span:all;
	}

## 总结
CSS3 Multi Column 给我创造和使用多列布局带来了很多便利，我们可以很轻松的就实现原来在CSS2.1中难以实现的布局。在实践中，我们需要不断总结和探索Multi Column带来的一些解决方案和问题，实在美中不足的是现代浏览器对CSS3 Multi Column的支持还不是那么完美，我相信，如此给力的特性一定会被各大浏览器开发商完美实现。
关于CSS3 Multi Column的内容就写到了这里，这个也是我昨天查阅了很多资料写出来的，可能内容会有不完整和错误，还请各位看官多多支持，谢谢支持~~~~~~~
