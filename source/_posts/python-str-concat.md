title: python笔记-python中的字符串拼接
date: 2015-12-03 22:17:22
tags:
- python
- str
categories: python
---
这些天在利用零零碎碎的一些时间在学习python，发现了python这门语言在一定程度上很想javascript。都是弱类型、动态语言，都支持高阶函数(还有我看到了`闭包`)，写法非常灵活，一上手觉得写起来非常舒服。python中管字符串类型叫`str`，而不是大众化的`String`，同样，它也是python中的一种对象，在python中`str`的写法非常灵活(pythoner管这个叫优雅):)，特别是字符串的拼接拼接，其方法之丰富，让我对python这门语言有了很大的好感，这篇博文就是总结下python中的`str` 字符串拼接。
<!--more-->
### 前言
个人使用的`python 3.4`版本，所以后面的代码实例也会使用python3的版本代码来说明。
### 字符串拼接方式
下面来介绍python3中字符串的拼接方式。
#### 利用加号进行拼接
这个方法我们在很多语言都看到过，这种方式也是最简单也是最直接的字符串拼接方法：
```
//coding with python3
>>> s = "I love " + "python"
>>> s
>>> I love python
```
#### 在字符串之间加上任意数量空格或者什么都不加
```
>>> "I love ""python"
'I love python'
>>> "I love"    "python"
'I love python'
```
这里需要强调的是，在python2中，我们在两个字符串之间加单个逗号是返回一个连接后的字符串，但是在python3中是返回一个tuple.
```
>>> # coding in python2
>>> "l love ","python"
'l love python'
------------------------
>>> # coding in python3
>>> "l love ","python"
('i love ','python')
```
#### 类C语言的printf函数
```
>>> '%s,%s' %('tom', 'dava')
'tom,dava'
```
#### list的join方法
```
>>> l = ['i', 'love', 'python']
>>> l.join(' ')
'i love python'
```
这种方式和JS中的数组join方法一样。
### 总结
这些只是我在学习python过程自己的一些关于python中的字符串拼接的一些总结，如果以后发现了其他的处理方法，再做补充。
