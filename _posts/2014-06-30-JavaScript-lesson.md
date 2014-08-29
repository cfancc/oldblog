---
layout: post
title: JavaScript基础学习笔记
---
#JavaScript基础学习笔记
---
## JavaScript中的传值与传址
* 赋值语句中  
JavaScript中对基本类型的传递是传基本值,基本类型包括:`Boolean,String,Number,Undefined and Null`.  
**注意:**只有字面量(literal)的`Boolean,String,Number,Undefined(实际值:undefined) and Null(实际值:null)`才是,举个例子:  

		var b = new Boolean(false);
		alert(typeof b);//"object"(不是基本类型)
		var s = new String("hello");
		alert(typeof s);//"object"(不是基本类型)
对于复合类型的传递是传地址值,也就是引用.比如:  

		var a = new Object();
		a.age = 10;
		var b = a;
		b.age = 18;
		alert(a.age);//"18"这里将a对象的地址传给b,b中修改属性就会导致a的变化

* 函数参数传递中
JavaScript中参数的传递全部是传基本值(包括复合类型,比如对象),这样在函数内所做的修改是不会对外部造成影响  

		// 修改原始类型的参数值
		var p = 2; 
		
		function f(p){
		    p = 3;
		}
		
		f(p);
		p // 2
		
		// 修改复合类型的参数值
		var o = [1,2,3];
		
		function f(o){
		    o = [2,3,4];
		}
		
		f(o);
		o // [1, 2, 3]
**注意:**虽然参数本身是传值传递，但是对于复合类型的变量来说，属性值是传址传递（pass by reference），也就是说，属性值是通过地址读取的。所以在函数体内修改复合类型变量的属性值，会影响到函数外部。

		// 修改对象的属性值
		var o = { p:1 };
		
		function f(obj){
		    obj.p = 2;
		}
		
		f(o);
		o.p // 2
		
		// 修改数组的属性值
		var a = [1,2,3];
		
		function f(a){
		    a[0]=4;
		}
		
		f(a);
		a // [4,2,3]
根据上述复合对象的属性是传址的,可以利用这一点:若想让某个变量传地址,可以把它当成window对象的一个属性,示例如下:  

		var p = 1;
		function f(prop){
			window[prop] = 2;
		}
		f('p');
		alert(p);//2

## JavaScript创建对象的几种方法

* 直接对象初始化器`{}`来创建:  

		var person = {
			name:"CC",
			age:24
			};

* 采用构造函数
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
* 原型是用来跟踪一个类的变化的对象**(待修正)**
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
		function Person(name){
		    this.name = name;
		    this.species= 'human';
		}
		//定义一个Student类
		function Student(name){
		    this.name =name;
		    this.carrer = "student";
		}
		//定义一个CollegeStudent类
		function CollegeStudent(name){
		    this.name = name;
		    this.education = "college";
		}
		//Student继承自Person
		Student.prototype = new Person();
		//CollegeStudent继承自Student
		CollegeStudent.prototype = new Student();
		//生成一个CollegeStudent对象:xiaoming
		var xiaoming = new CollegeStudent("xiaoming");
		var xiaowang = new CollegeStudent("xiaowang");
		xiaowang.__proto__.species = "not human";
		console.dir(xiaowang);
		//输出小明的姓名
		console.log(xiaoming.name);//xiaoming
		//输出小明的种族
		console.log(xiaoming.species);//not human
		//输出小明的职业
		console.log(xiaoming.carrer);//student
		//输出小明的教育程度
		console.log(xiaoming.education);//college