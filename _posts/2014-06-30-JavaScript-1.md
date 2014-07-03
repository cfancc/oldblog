---
layout: post
title: JavaScript学习笔记
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
##JavaScript原型(prototype)对象
* 原型是用来跟踪一个类的变化的对象
举个例子:
如果创建了一个`Student`类:
	
		function Student(name,age){
			this.name = name;
			this.age = age;
		}
	假设通过此类实例化了一个对象:`xiaoming`
	
		var xiaoming = new Student();
	那么小明拥有的属性包括`name`和`age`,如果我想给`xiaoming`添加一个`carrer`属性,可以使用常用的方法,即:`xiaoming.carrer = "student"`.  
	但是如果通过这个类实例化了很多个不同的`student`对象,并且都要给他们加上`carrer`属性,采用之前的方法就比较繁琐了.为了在不修改类的条件下,简单的得到这种效果,就需要利用到原型了.
	
		Student.prototype.carrer = 'student';
	这样在之后所有`student`创建的对象中,都存在一个`carrer`属性,并且值为`student`.
* 原型链在继承中的应用
	JS语言中的继承,是利用原型的方式进行实现了,因为继承关系可以表现为`父-子-孙...`的链接方式,相应的就有了原型链(`prototype chain`)的概念.

		//定义一个Person类
		function Person(name,carrer){
		    this.name = name;
		    this.carrer= carrer;
		}
		//定义一个Student类
		function Student(name){
		    this.name =name;
		    this.carrer = "student";
		}
		//定义一个CollegeStudent类
		function CollegeStudent(name){
		    this.name = name;
		    this.carrer ="student";
		    this.education = "college";
		}
		//Student继承自Person
		Student.prototype = new Person();
		//CollegeStudent继承自Student
		CollegeStudent.prototype = new Student();
		//生成一个CollegeStudent对象:xiaoming
		var xiaoming = new CollegeStudent("xiaoming");
		
		//输出小明的姓名
		console.log(xiaoming.name);
		//输出小明的职业
		console.log(xiaoming.carrer);
		//输出小明的教育程度
		console.log(xiaoming.education);