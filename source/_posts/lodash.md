title: javascript工具库-lodash.js
date: 2015-12-25 20:54:39
tags: 
- javascript
- tools
---
如果你曾经使用过**underscore**,那么你一定会喜欢今天我要介绍的**lodash**。
{% asset_img lodash.png %}
<!-- more -->
## 介绍
**lodash**起初是由**underscore**演变而来的。因为与underscore的其他贡献者意见相斥，于是乎lodash的开发者就更具自己的理解，开发出lodash这个javascript工具库。

于underscore一样，lodash也沿用了 _ 作为全局访问对象来对工具库函数api的快速访问。

最新版的lodash是4.0版本，比较重要的改变是这个版本也是不再对IE6-8的支持，相关的信息可以[点击查看](https://github.com/lodash/lodash/wiki/Changelog).

## 对比
想对比与underscore，lodash的带给我的惊喜可不止是一点点。
### API文档
早些时候，当我发现lodash这个工具库的之前，我还是一直使用underscore作为必备首选。但是当我查看了lodash的文档和官方介绍后，毅然的决定抛弃underscore。

单方面的个人感受，当我查看相关api文档时，lodash的文档着实详细易看。参数的类型描述、返回结果的类型和给的示例都非常详细，对比与我这个写代码几乎不写注释的人来说，结结实实的打脸啊。
### 延迟计算
lodash的执行效率则是最受好评的一个点。lodash的作者也提到了，lodash的最初目标是提供更多的"一致的跨浏览器的行为，并改善性能"。lodash采用的延迟计算的策略，具体是怎么个实现方式呢，我们俩看一段代码:

```
function priceLt(x) {
   return function(item) { return item.price < x; };
}
var gems = [
   { name: 'Sunstone', price: 4 }, { name: 'Amethyst', price: 15 },
   { name: 'Prehnite', price: 20}, { name: 'Sugilite', price: 7  },
   { name: 'Diopside', price: 3 }, { name: 'Feldspar', price: 13 },
   { name: 'Dioptase', price: 2 }, { name: 'Sapphire', price: 20 }
];

var chosen = _(gems).filter(priceLt(10)).take(3).value();
```
它的运行示意图是:
{% asset_img lodash-naive.gif %}

上面一共读取了8个元素，然后我们取出我们需要的3个元素。不难发现，其实我们只需要读取前5个元素，就能找到我们想要的那3个结果。那么它原本应该的结果应该是:

{% asset_img grafika.gif %}

那么延迟计算的特点在于，它将我们在链式执行筛选函数的条件都延迟到最后一个筛选条件一并执行(这些筛选条件必须连续)，这个有点类似于浏览器对repaint/reflow的优化所干的事。
上面代码用伪代码就可以近似表示为:

```
var takeNum = 3, result = [];
for(var i = 0, len = gems.length; i < len; i++) {
	if(priceLt(gems[i])) {
		reuslt.push(gems[i]);
		if(reuslt.length == 3) {
			break;
		}
	}
}
```
关于lodash延迟计算的一些讲解，我觉得[How to Speed Up Lo-Dash ×100? Introducing Lazy Evaluation.](http://filimanjaro.com/blog/2014/introducing-lazy-evaluation/)这篇文章讲的很有意识，作者的语言和理解比较易懂。

## 结语
其实也可以挑一些有特点的api来适当的说明下lodash的api上的特点，但是个人觉得再怎么将还不如翻一翻官方文档，之前也说到了lodash的官方文档带给我的感受了。
在知乎上，很多大牛都觉的underscore的源码很是值得一读和重新实现，但现在觉得lodash的源码更是值得细细专研下(未压缩版的lodash源码的注释意识十分详细)。代码规范、如何实现延迟计算、函数式编程的思想，学以致用的最高境界也当要学透。