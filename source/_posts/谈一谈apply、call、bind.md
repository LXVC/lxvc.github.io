---
title: 谈一谈 apply、call、bind
date: 2018-09-06 21:26:54
tags: [JavaScript]
---

大家都知道，JavaScript 是一门弱类型动态语言，运行期间才会去做数据类型的检查。一定程度上，这会给我们的项目带来很多不确定性，但是凡事有利有弊，善于利用它的动态性特点也能给我带来相当的便利，写出很多骚操作~
<!-- more -->

其中把语言本身的动态性演绎得淋漓尽致的非 `Function.prototype.apply` `Function.prototype.call` `Function.prototype.bind` 这三个方法莫属。
