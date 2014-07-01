## JavaScript创建对象的几种方法
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