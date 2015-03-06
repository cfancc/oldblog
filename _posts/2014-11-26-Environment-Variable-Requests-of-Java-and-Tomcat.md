---
layout: post
title: JDK及Tomcat环境变量配置详解
---
相信很多刚接触JavaWeb开发的人最先接触的就是JDK和Tomcat的安装和配置，这其中最重要的一项就是这俩货的环境变量的配置。关于这俩货的环境变量的配置，大抵网络上都是差不多的说辞，先配这个再配那个，完全是你抄我我抄你。没几个能说个明白为什么配置这个，为什么这么配，最后怒查各种官方文档和说明，终于整了个秃子头上的虱子-- 一清二楚。以下说明在Win7 64位平台下，以JDK 1.6，Tomcat 6.0为例。



##JDK环境变量配置
1. **Path（可选配）：**  
	还是扒一下官方JDK安装文档吧：  
	> Set the PATH variable if you want to be able to conveniently run the JDK executables (javac.exe, java.exe, javadoc.exe, etc.) from any directory without having to type the full path of the command. If you don't set the PATH variable, you need to specify the full path to the executable every time you run it, such as:
	> 	C:"\Program Files\Java\jdk1.6.0_<version>\bin\javac"
	> 	MyClass.java
	> 	It's useful to set the PATH permanently so it will persist after rebooting.

	***配置这项的作用？***

	我就纯翻译了,官方文档写的比较清楚: 
	如果你想要在任意目录下方便的调用JDK的执行文件(javac.exe, java.exe, javadoc.exe, 等等)，并且不想输入这个文件的路径全称，你就需要配置这个PATH环境变量.如果你不配置的话，你在执行一个文件的时候需要敲一长串的路径名，比如下面这样:

    	C:\Program Files\Java\jdk1.6.0_<version>\bin\javac MyClass.java

	如果你想随时随地在CMD中只要敲击执行文件的名字就能运行，那么就需要配置PATH这一项。配置完这一项后，你使用JDK下的一些文件就如同使用ping工具一样简单了
	
	***如何配置?***

	简单来说就是，系统的PATH环境变量里增加一下JDK的bin目录路径。在PATH变量的最后增加一项类似于：“C:\ProgramFiles\Java\jdk1.6.0_<version>\bin”的东西。如果没有就自己创建一个PATH变量，如果PATH变量中已经有其它路径了，那么新增路径要与前面的路径用英文分号“;”隔开

2. **CLASSPATH（可选配）**

	官方JDK安装中并没有提及这一项的配置，也就是说一般是没必要配置的。这项的配置是用来供JAVA虚拟机寻找类文件提供服务的，默认是当前目录下，一般无需改动。如果真要配的话，请确定你明确这个变量的意义。

	***配置这项的作用？***
	
	> The class path tells JDK tools and applications where to find third-party and user-defined classes -- that is, classes that are not Java extensions or part of the Java platform. The class path needs to find any classes you've compiled with the javac compiler -- its default is the current directory to conveniently enable those classes to be found.

	翻译：classpath能告诉JDK工具和执行文件需要到哪里去找第三方和用户自定义的类文件。这些类文件并不是JAVA自身的扩展或者是JAVA平台的一部分。classpath类路径需要能找到用户用javac编译器编译完的任何的类文件--默认情况下是指向当前目录的，能方便找到那些需要用到的类

	
	***如何配置?***

	官方文档：[http://docs.oracle.com/javase/6/docs/technotes/tools/windows/classpath.html](http://docs.oracle.com/javase/6/docs/technotes/tools/windows/classpath.html)
	如果你需要用到CLASSPATH变量，那么记得要将“.”或者“\”(即当前根目录)添加进去，因为自定义的路径会覆盖掉默认的配置。多个路径之间用“;”（英文分号）隔开（试验了，确实如此）。至于网上说的一些要带rt.jar ,dt.jar ,tools.jar文件的路径的说法，rt.jar存在于jre\lib下，tools.jar和dt.jar是存在于jdk安装目录下的lib文件目录下。下面就来一一辟谣。  
	- rt.jar存放着JAVA平台的核心类。
		通过java -verpose命令可以看到Java平台会自动帮你加载（rt应该是RunTime的缩写，应该是运行环境的意思，充分说明了这个文件的重要性，运行环境开启必然会加载rt.jar文件）  
    
    		[Opened C:\Program Files (x86)\Java\jdk1.6.0_45\jre\lib\rt.jar]
    		[Loaded sun.misc.JavaSecurityAccess from C:\Program Files (x86)\Java	\jdk1.6.0_45
    		\jre\lib\rt.jar]
    		[Loaded java.security.AccessControlContext$1 from C:\Program Files (x86)\Java\jd
    		k1.6.0_45\jre\lib\rt.jar]

		**那么CLASSPATH里要不要写*rt.jar*呢？  
		答案是：**完全不需要！**  



	- tools.jar包含了一些非核心类，用来供JDK中提供的工具以及实用程序调用。  
	

		> These include  tools.jar, which contains non-core classes for support of the tools and utilities in the JDK.  

		这里tools.jar的描述还不尽明确，不过用WinRAR打开查看它的文件结构，就可以看到很多JDK Tools and Utilities 需要用到的类都在这里面。  

		具体的工具列表：[https://docs.oracle.com/javase/6/docs/technotes/tools/](https://docs.oracle.com/javase/6/docs/technotes/tools/)

		**那么CLASSPATH里要不要写*tools.jar*呢？**

		答案是：**一般不需要！**  

		论证1：	
		> The tools archive is the SDK's/lib/tools.jar file. The development tools add this archive to the user class path when invoking the launcher.However, this augmented user class path is only used to execute the tool. The tools that process source code, javac and javadoc, use the original class path, not the augmented version. 

		翻译：JDK启动JAVA虚拟机的时候就会将tools.jar文件添加到用户自定义类路径中。不过这个自动添加路径只能在运行相应的工具时有效。javac和javadoc处理源码时，都使用原始的类路径，而不是这个自动添加的版本。  

		论证2：
		> The tools classes in tools.jar are only used to run javac and javadoc. The tools classes are not used to resolve source code references unless tools.jar is in the user class path.  

		翻译：tools.jar中的工具类只被用来运行javac和javadoc程序。这些类将不会被用来处理源码中的引用问题除非tools.jar被添加到classpath中。（也就是说，用javac编译代码的时候不会从tools.jar寻找引用）  

		**稍微总结下：JDK会自动加载tools.jar文件，其中的类只是给JDK下的javac和javadoc工具中的某些功能用的，而且编译源码的时候不会去把这些类当作引用来搜索。所以没必要在classpath中添加此文件。**  

	- dt.jar是BeanInfo文件的DesignTime（缩写：dt）归档，BeanInfo文件用来告诉集成开发环境（IDE）如何显示Java组件还有如何让开发人员根据应用程序自定义它们。
		> Also includes dt.jar, the DesignTime archive of BeanInfo files that tell interactive development environments (IDE's) how to display the Java components and how to let the developer customize them for an application.  

		**那么CLASSPATH里要不要写*dt.jar*呢？**

		答案是：**一般不需要！**  

		论证：（这里就不自己写了，从大牛的博客上直接搬过来吧[http://www.blogjava.net/landon/archive/2011/05/15/350285.html](http://www.blogjava.net/landon/archive/2011/05/15/350285.html)）	

		> 何为DesignTime?翻译过来就是设计时。其实了解JavaBean的人都知道design time和runtime（运行时）这两个术语的含义。设计时（DesignTIme）是指在开发环境中通过添加控件，设置控件或窗体属性等方法，建立应用程序的时间。与此相对应的运行时（RunTIme）是指可以象用户那样与应用程序交互作用的时间。那么现在再理解一下上面的翻译，其实dt.jar包含了swing控件中的BeanInfo，而IDE的GUI Designer需要这些信息。
	
		> 何为BeanInfo?JavaBean和BeanInfo有很大的关系。Sun所制定的JavaBean规范，很大程度上是为IDE准备的——它让IDE能够以可视化的方式设置JavaBean的属性。如果在IDE中开发一个可视化应用程序，我们需要通过属性设置的方式对组成应用的各种组件进行定制，IDE通过属性编辑器让开发人员使用可视化的方式设置组件的属性。一般的IDE都支持JavaBean规范所定义的属性编辑器，当组件开发商发布一个组件时，它往往将组件对应的属性编辑器捆绑发行，这样开发者就可以在IDE环境下方便地利用属性编辑器对组件进行定制工作。JavaBean规范通过java.beans.PropertyEditor定义了设置JavaBean属性的方法，通过BeanInfo描述了JavaBean哪些属性是可定制的，此外还描述了可定制属性与PropertyEditor的对应关系。BeanInfo与JavaBean之间的对应关系，通过两者之间规范的命名确立：对应JavaBean的BeanInfo采用如下的命名规范：<Bean>BeanInfo。当JavaBean连同其属性编辑器相同的组件注册到IDE中后，当在开发界面中对JavaBean进行定制时，IDE就会根据JavaBean规范找到对应的BeanInfo，再根据BeanInfo中的描述信息找到JavaBean属性描述（是否开放、使用哪个属性编辑器），进而为JavaBean生成特定开发编辑界面。
	
		>哈哈。现在可以理解dt.jar了吧。其实里面主要是swing组件的BeanInfo。IDE根据这些BeanInfo显示这些组件以及开发人员如何定制他们。

		**补一句：现在的Eclipse完全能自己找到，在Eclipse下完全没有配置这个dt.jar的必要**

3. **JAVA_HOME（独立使用绿色版Tomcat必配）：字面意思就是JAVA的家。**

	***配置这项的作用？***

	JDK官方的安装说明里完全没提到对这一项的配置。但是引入此项的方便之处：一是Tomcat的需要；二是为了在其它地方引用的，比如PATH里的bin文件夹路径就可以不用写全称了：%JAVA_HOME%\bin
	第二点不用讲了，关键是第一点得说明一下：  

	    rem Environment Variable Prerequisites
		rem
		rem   **CATALINA_HOME**   May point at your Catalina "build" directory.
		rem
		rem   CATALINA_BASE   (Optional) Base directory for resolving dynamic portions
		rem   of a Catalina installation.  If not present, resolves to
		rem   the same directory that CATALINA_HOME points to.
		rem
		rem   CATALINA_OPTS   (Optional) Java runtime options used when the "start",
		rem   or "run" command is executed.
		rem
		rem   CATALINA_TMPDIR (Optional) Directory path location of temporary directory
		rem   the JVM should use (java.io.tmpdir).  Defaults to
		rem   %CATALINA_BASE%\temp.
		rem
		rem   **JAVA_HOME**   Must point at your Java Development Kit installation.
		rem   Required to run the with the "debug" argument.
		rem
		rem   **JRE_HOME**Must point at your Java Runtime installation.
		rem   Defaults to JAVA_HOME if empty.
	
	以上是Tomcat启动时对环境变量的部分要求，非可选的我都用粗体标出来了。我们先看跟JAVA相关的最后两项吧：

	**JAVA_HOME**：必须指出JDK的安装目录。当运行Debug模式时需要
	**JRE_HOME**：必须指出JRE的安装目录。如果为空则默认等于JAVA_HOME
	
	可以看出，如果我们是独立使用Tomcat，只需要配置其中的一项JAVA_HOME就可以了。如果两个都没配，那启动的时候就悲剧了：	

    	Neither the JAVA_HOME nor the JRE_HOME environment variable is defined
    	At least one of these environment variable is needed to run this program
	
	如果你安装了两个JDK版本，一个1.6，一个1.7，那么可以通过更改JAVA_HOME的指向来变更Tomcat所使用的JDK版本。
	
	***如何配置？***

	配置值就是JDK的安装目录，例如我的：C:\Program Files (x86)\Java\jdk1.6.0_45  

4. **CATALINA_HOME（可选配）**

	字面意思就是Tomcat的家
	
	***配置这项的作用？***

	在Tomcat启动时需要寻找此项，如果没有环境变量中没有找到，则把当前目录路径赋给CATALINA_HOME，然后再启动，如果你直接是在Tomcat安装目录下双击startup.bat文件运行Tomcat就没必要配置这项了。如果你想在非Tomcat安装目录下通过输入startup.bat文件的路径名来运行Tomcat，不配置此项就会提示错误：
	
    	C:\Users\Administrator>H:\Tomcat\bin\startup.bat
    	The CATALINA_HOME environment variable is not defined correctly
    	This environment variable is needed to run this program
	
	***如何配置？***

	配置值就是Tomcat的安装目录，例如我的：H:\Tomcat  

	一般这一项可以搭配着PATH进行配置，这样就可以在命令行里直接输入startup.bat直接运行Tomcat了

**总结：如果使用IDE进行开发，比如Eclipse，这些都不用配置，在Eclipse中调试开发的时候，它会自动帮你设置好；如果不适用IDE或者IDE不够智能，自己按情况来选择，通常只需要配置JAVA_HOME、CATALINA_HOME即可**