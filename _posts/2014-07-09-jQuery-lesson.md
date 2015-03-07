---
layout: post
title: jQuery学习笔记
---
- 启动函数  
	`$(document).ready();`
	* -`$()` 表示,这是跟jQuery相关的东西;
	* -`document` 把它放到两个括号间是告诉我们是对HTML的`document`对象进行处理;
	* -`.ready();` 这个函数是在`document`对象载入完毕后立刻执行的;



	与`window.onload`的不同:  
	- `$.ready()`比`onload`执行的要早，在DOM树结构完成后便会执行`ready()`里的函数。而`onload`会在页面加载完毕后才执行，明显就会慢了一个档次

	- `$.ready()`可以执行多个。如果一段代码中有多个`$.ready()`,那么加载次序是按从上到下的顺序执行的。而`onload`只能执行一次，有多个的话只执行最后面的，默认覆盖了前面的函数

- 强大的选择器
- 连缀式写法

- 灵活的Ajax

- 事件处理

- promise对象