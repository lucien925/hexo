title: 学习javascript 模块化的总结(二)
date: 2015-12-06 12:06:20
tags:
- Javascript
---
前文简单的总结了下前端模块化的一些发展历程和CommonJS规范、AMD规范以及一些模块加载库的实现，这篇文章将介绍下**browserify**和**webpack**及以用法。当然，前端模块化最让人惊喜的莫过于ES6的模块化加载方案，后续学习时，也会在ES6学习系列的文章中总结一下。
<!--more-->
## 介绍
### browserify
**browserify**是一个使用node.js编写的模块管理器，你可以使用类似于node.js中的require()来在我们的项目中引入你想要依赖或者使用的模块。browserify托管在npm上，可以使用:
	
	npm install browserify -g
来安装browserify。那么如何使用browserify呢，下面通过一小段代码来感受一下:

```
// a.js
function a() {
	console.log('a');
}
modules.export = a;
```

```
// b.js
var a = require('./a.js');
a(); // a
```
通过：

	browserify b.js -o bundle.js
来将b.js的代码和b.js中reuqire进来的模块进行打包进一个文件(bundle.js)中。
### webpack
**webpack**的出现让前端开发重新定义了模块这一词儿。在webpack中，css、图片等静态资源都可以被打包成模块。webpack还支持在模块打包时的插件处理机制。在工程的根目录下新建**webpack.config.js**文件，在文件中配置相关的选项:
```
// webpack.config.js
module.exports = {
    entry: "./entry.js",
    output: {
        path: __dirname,
        filename: "bundle.js"
    },
    module: {
        loaders: [
            { test: /\.css$/, loader: "style!css" }
        ]
    }
};
```
在module.loaders中，我们使用了`style`和`css`两个loader来帮我们处理在浏览器中加载css文件。在webpack社区中，开源的各种插件已经足够满足我们日常的开发需求。

