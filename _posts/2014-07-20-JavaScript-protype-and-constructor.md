---
layout: post
title: JavaScript的原型机制
---
#JavaScript的原型机制
---
Javascript中的继承不同于其它语言,想搞清楚它的继承,最主要需要理解的便是原型模式,下面是基于网上的一些资料加上我的个人理解,希望能将原型模式表述清楚.
##什么是原型
首先明确一点,JS中除了字面(`literal`)基本类型外,其它都可以看作对象.JS中一个对象的产生,不是凭空出现,自然而然的要依赖于相应的构造函数(`constructor`).构造函数是什么样的呢?比如说:  



	function Person(name) {
		this.name = name;
	}
上述的例子就是一个构造函数,当我们配合`new`关键字使用时,便能按照构造函数里的要求生成一个相应的对象:

	var a = new Person();

* `__proto__`  

	每一个生成的对象的对象都有一个属性`__proto__`,这个属性里面是什么呢?简单来说,是指向了该对象的原型(`prototype`)对象.所谓的原型对象,大体意思就是我是从你的基础上造出来的,我继承了你全部的属性和方法(属性和方法的引用),然后又加了点自己的东西,如果一个属性原型对象已经定义了值,我不用自己再来定义一遍,直接用就行了,倒有点像继承了老爹的遗产一样  

* `prototype`与`constructor`  

	从上面我们知道了,一个对象生成后,会有一个属性指向它的原型(`prototype`)对象,那这个原型对象具体是干什么的呢?这时候就得先详细说一下构造函数(`constructor`)了,在前面我们定义了一个`Person`构造函数,从JS的角度来看,函数也是对象的一种,所以说`Person`构造函数也是一种特殊的对象,所以它也拥有自己的一些属性来说明自己.  

	`prototype`就是构造函数一个很重要的属性,这个属性指向了一个原型对象,*该对象与构造函数同名(待验证),*主要表达了本构造函数(`Person`)生成的对象的最小化模版,并且原型(`prototype`)对象中所有已定义的静态属性和方法都可以被构造函数生成的对象所使用的,采用引用的方式(就是说,只要你用了本构造函数(`Person`) `new`出来一个对象,那么这个对象中必定会存在原型的属性和方法--假如不重写的话,可以通过`.`直接调用)

	同样的,原型对象中也存在着一个`constructor`属性,指向了它的构造函数,总体来说,它们之间是互相引用的关系,如下图所示:  
	![构造函数和原型对象的引用关系](/images/constructorAndprototype.jpg)

##原型的好处在哪里?

折腾了这么多,你一定会问,这么麻烦,引入原型模式的好处是在哪里呢?首先呢,因为历史的原因,JS的设计初衷是能在客户端对页面进行一些简单的操作,当时作者不想让Javascript成为一个太复杂的语言,也没打算让它成为完全面向对象的语言,因为这会增加一门语言的入门难度(喵了个咪的,原型入门难度可不小),所以不打算加入"类"机制,但是不加入类机制,又会导致在某些地方不够方便,比如"继承问题":  

我想弄一个构造函数,可以生成`Dog`对象
	
	function Dog(name) {
		this.name = name;
	}
	
	var dog1 = new Dog('大花');
	var dog2 = new Dog('大黄');

这样的一个坏处是如果我在构造函数中加一个公有属性`this.species='犬科'`:  

	function Dog(name) {
		this.name = name;
		this.species='犬科'
	}
	
	var dog1 = new Dog('大花');
	var dog2 = new Dog('大黄');
很明显,两条狗都具有了`species`属性,而且都为`犬科`,但是如果我把`dog1`的`species`属性更改后,`dog2`的`species`并不会改变:

	dog1.species = '鱼类';
	alert(dog2.species);//此处dog2的`species`属性并没有改变
这么说来,每一个实例化后的对象都独自占用了一份`species`的副本,各改各的,互不影响.这样不仅无法做到数据共享(牵一发而动全身),而且浪费了很多无辜的空间.要是有一个方法既能做到数据共享又能节省空间就好了,但是前提是,不要加入面向对象的那一套"类"机制(→_→对JS作者无语了)

终于作者想出了给构造函数加个原型对象的方法,上面的代码只要改动一点,就能达到需要的效果了:

	function Dog(name) {
		this.name = name;
		this.species='犬科'
	}
	Dog.prototype.species = "犬科";
	var dog1 = new Dog('大花');
	var dog2 = new Dog('大黄');

从我们之前看的`prototype`介绍来分析,这里面给`Dog`构造函数的原型(`prototype`)对象(相当于一个核心模版),增加了一个`species`属性.这样构造函数生成的对象,调用这个模版的`species`属性时,都访问到的是`犬科`,如果其中一个对象对自己的`__proto__`属性指向的原型对象中的`species`进行更改,那么同一构造函数下生成的所有的对象的`species`属性都会发生变化.通过这种方法就解决了之前的那个问题:"继承"

##JavaScript引擎如何来查找属性

看完上面的一大堆文字,你可能还有一个疑惑,为什么一个对象的原型对象中定义的属性,我可以用`.`直接访问到呢?下面看一下JS引擎是如何来查找属性的吧:  

	function getProperty(obj, prop) {
	if (obj.hasOwnProperty(prop))
	return obj[prop]
	
	else if (obj.__proto__ !== null)
	return getProperty(obj.__proto__, prop)
	
	else
	return undefined
	}

通过代码可以看出,当你访问一个对象的属性的时候,它先查自己有没有,没有就去它的`__proto__`属性指向的原型对象中去查,如果还没有,就去查它的原型的原型中有没有,这样一直到原型链的最顶端为止.所以:

	dog1.species //"犬科"
	dog1.__proto__.species //"犬科"

上面这两种访问方式是等价的.

##`new`的实现原理
知道了构造函数和原型对象,你可能感觉很神奇,但是更神奇的是:调用构造函数生成对象的时候发生了什么?这些都跟`new`有脱不开的关系.

`new` 运算符接受一个函数 `F` 及其参数：`new F(arguments...)`。这一过程分为三步：

1. 	**创建类的实例。**这步是把一个空的对象的`__proto__` 属性设置为 `F.prototype` 。
2. 	**初始化实例。**函数 `F` 被传入参数并调用，关键字 `this` 被设定为该实例。
3. 	**返回实例。**
	
直接来看一段`new`运算符的工作原理模拟代码:
	
	function New (F) {
		var n = { '__proto__': F.prototype }; /*第一步*/
		return function () {
			F.apply(n, arguments);            /*第二步*/
			return n;                         /*第三步*/
		};
	}

这样,一个新鲜出炉的对象就生成了!

相信在理解了原型,构造函数和`new`运算符等作用后,对于Javascript的理解会更加深刻,本文算是一个学习笔记,在学习的过程中参考了很多资料,主要是以下的一些文章,感谢他们能写出这么浅显易懂的文章,让我从中受益颇多.  

**参考资料:**  

> [图灵社区:【翻译】JavaScript原型继承工作原理](http://www.ituring.com.cn/article/56184)  
[围了个脖:揭开Javascript属性constructor/prototype的底层原理](http://blog.csdn.net/hikvision_java_gyh/article/details/8937157 "揭开Javascript属性constructor/prototype的底层原理")	  
[阮一峰:Javascript 面向对象编程（一）：封装](http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_encapsulation.html)  
[阮一峰:Javascript继承机制的设计思想](http://www.ruanyifeng.com/blog/2011/06/designing_ideas_of_inheritance_mechanism_in_javascript.html)
