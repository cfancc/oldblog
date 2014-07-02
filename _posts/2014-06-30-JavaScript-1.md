---
layout: post
title: JavaScript学习
---
## JavaScript创建对象的几种方法
---
1. 直接对象初始化器`{}`来创建:  

		var person = {
			name:"CC",
			age:24
			};

2. 采用构造函数
	- 采用系统自带的`Object()`构造函数:

			var person = new Object();
			person.name="CC";
			person.nage=24;
		或初始化时带参数

			var person = new Object({
			name:"CC",
			age:24
			}); 
	- 自定义构造函数:

			function Person(name,age){
				this.name = name;
				this.age = age
			}

---
##JavaScript原型(prototype)
原型是用来跟踪一个类的变化的对象

举个例子:

如果创建了一个`Person`类:

	function Person(name,age){
		this.name = name;
		this.age = age;
	}
假设通过此类实例化了一个对象:`xiaoming`

	var xiaoming = new Person();
那么小明拥有的属性包括`name`和`age`,如果我想给`xiaoming`添加一个`carrer`属性,可以使用常用的方法,即:`xiaoming.carrer = "student"`