---
layout: post
title: HTML&CSS学习笔记
---
##HTML标签备忘录(注释方式:`<!--comments-->`)

* `<img src="url" />`  
`<img>`是Html语言中少有的自闭合标签,自闭合的不同之处在于它在开标签结尾有一个`/`符号用来关闭自己  

* `<input type='' name=''>`  
HTML中,`<input>`是不需要闭合的,但在XHTML中,是需要闭合的



* `<a href="url"><img src="url" /></a>`  
上面的是为图片增加超链接地址的形式,只需在`<img>`标签外加一层`<a>`即可,其它标签增加链接的方式类似

* `<em></em>`  
`<em>`标签用来强调文本,实际效果为"斜体":把文本定义为强调的内容.

* `<strong></strong>`  
`<strong>`标签用来加粗文本:把文本定义为语气更强的强调的内容

* `<table></table>`

	表格的标签层次:

	第一层是`<table></table>`,这是表格的主体,所有表格的信息全部在这个标签内.

	第二层包含`<thead></thead>`和`<tbody></tbody>`.`<thead>`主要是表头的信息

	第三层主要是`<tr>`标签,代表表格的某一行

	第四层是`<th> <td>`标签,`<th>`包含表头数据,字体加粗;`<td>`包含普通表格数据,内容默认

		<table border="1px">
			<thead>//表头
				<tr>//表行
					<th colspan="2">//横跨两列
						Famous Monsters by Birth Year
					</th>
				</tr>
				<tr>//表行
					<th>Famous Monster</th>//表格数据
					<th>Birth Year</th>
				</tr>
			</thead>
			<tbody>//表身
				<tr>
					<td>King Kong</td>
					<td>1933</td>     
				</tr>
				
				<tr>
					<td>Dracula</td>
					<td>1897</td>
				</tr>
				
				<tr>
					<td>Bride of Frankenstein</td>
					<td>1935</td>
				</tr>
			</tbody>
		</table>  
	上述代码效果如下:
	<table border="1px">
	<thead>
	<tr>
		<th colspan="2">
			Famous Monsters by Birth Year
		</th>
	</tr>
	<tr>
		<th>Famous Monster</th>
		<th>Birth Year</th>
	</tr>
	</thead>
	<tbody>
		<tr>
		<td>King Kong</td>
		<td>1933</td>     
		</tr>
		<tr>
		<td>Dracula</td>
		<td>1897</td>
		</tr>
		<tr>
		<td>Bride of Frankenstein</td>
		<td>1935</td>
		</tr>
	</tbody>
	</table>

##CSS备忘(注释方式:`/*comments*/`)
---
* 分离HTML与CSS的两大理由

	* 能减少对不同HTML元素采用相同样式的代码重复率(例如:不必为了每一个元素都定义一次style="color:red"的样式),一次更改,全部生效;

	* 能对不同的网页应用相同的样式表(只要挂载上.css即可,内联的css样式无法剥离出来重复使用)  

* CSS的几种载入方式:
	1. 内联在元素内部(分散):

			<p style="color:red">Red</p>
	2. 集中写在HTML的`<style></style>`标签中:

			<style>
				p {
					color:purple;
				}
			</style>
	3. 通过`<link />`标签挂载外部css文件(`<link />`标签也是**自闭合**的):

			<link type="text/css" rel="stylesheet" href="stylesheet.css" />
			<link rel="stylesheet"
			href="http://s3.amazonaws.com/codecademy-content/courses/ltp/css/bootstrap.css">
			<link rel="stylesheet" href="main.css">
这三个文件的加载顺序是自上而下的,若有冲突的地方,最后加载的会覆盖先前加载的
* `px`与`em`的区别:  
`px`是指代的`"pixel"`,即像素.当一个元素的大小单位为`px`,是按像素的个数来排列的.`em`则是根据浏览器的默认解析大小来展示的,比如大部分浏览器上默认`1em=16px`,那么如果默认值更改,`1em`的值也会变动.

* `font-family`属性下可以放多种字体,**以逗号分割**.
*假若前面的字体无效,会依次尝试使用后面的字体.字体中间有空格的话,需要将字体的全名用双引号包起来哦:`font-family: "Times New Roman", Times, serif;(待验证)`*

* 全局选择器(`*`):顾名思义,该选择器所对应的样式对所有元素生效
* `color`的值:  
	颜色属性可以用三种方式表示:  
	 - 颜色的名字,但是只有140种颜色可选  [CSS颜色参考](http://www.crockford.com/wrrrld/color.html "CSS颜色参考")  
	 - RGB值 从`0`到`255` 格式:`rgb(102,153,0)`  
	 - Hex数值 从`00`到`ff` 格式:`#ffffff`  
* `>`符号的使用  
这个符号是表示筛选直系亲属的意思.`div > p`选择的是`<div>`标签下的最上层的`<p>`段落,而不包括`<div>`下的所有`<p>`段落

* 指向性更强的选择器将会覆盖指向性较弱的选择器内容  
比如:`ul li p {}`的指向性要强于`p {}`的指向性,这时候,强的将会覆盖和弱的冲突的样式

* 选择器的优先级  
**ID选择器 > 类选择器 > HTML标签选择器 > 通用选择器(`*`)**

* 拟类选择器(`pseudo-class selector`)  
拟类选择器用来选择那些不属于文档树静态结构的东西.比如一个`<a>`元素的点击状态,有被点击过和没被点击过两种.我们如果想要对它的不同状态附加样式,就要用到"拟类选择器".

		selector:pseudo-class_selector {
			property:value;
		}  
		
	* 常用的拟类选择器:  
	`a:link`:未被访问的链接.  
	`a:visted`:已被访问的链接.  
	`a:hover`:鼠标悬停在链接上时.  
	`p:first-child`:做为父元素的**第1个**儿子的p  (所有符合条件的p)  
	`p:nth-child(n)`:做为父元素的**第n个**儿子的p  (所有符合条件的p)

* **元素的定位**
	* 不管是块元素还是内联元素都是长方形的盒子
	
	* 两者之间可以通过`display:inline;`或者`display:block;`互相转换。
	
	* 浮动元素不占任何正常文档流空间，而浮动元素的定位还是基于正常的文档流，然后从文档流中抽出并尽可能远的移动至左侧或者右侧。文字内容会围绕在浮动元素周围。**当一个元素从正常文档流中抽出后，仍然在文档流中的其他元素将忽略该元素并填补他原先的空间**
	
	* 基于文档流，我们可以很容易理解以下定位模式：  
	
		*  相对定位(*relative*)，即相对于元素在文档中的位置进行偏移。但保留原占位符。  
	
		*  绝对定位(*absolute*),即完全脱离文档流，原占位不保留，相对于position属性非static值的最近父级元素进行偏移。  
	
		*  固定定位(*fixed*)，即完全脱离文档流，相对于浏览器的内容窗口进行偏移。
	
	
	* `display`属性的运用:
		* `block`:这将使元素成为块盒子.将不允许任何元素紧靠着着它,它将占去一整行
		* `inline-block`:虽然是个块盒子,但是允许其它元素在同一行里挨着它
		* `inline`:彻彻底底的和其它元素座落在同一行,不会格式化为块盒子.它只会占用自己需要的宽度,绝不多占.**如果将一个块元素强制显示为`inline`样式,则不会保留该块元素的面积(比如设置了`height`,`width`都无效,不过`border`还存在).元素所占的宽度将取决于元素内部内容的长度.**
		* `none`:很明显了,`display`设为`none`,直接不显示了好吗,包括它的孩子们都没了好吗?
	* 盒子模型:  
		![盒子模型](/images/box-model.png)
		![IE盒子与w3c盒子的区别](/images/boxdifference-between-IE-and-w3c.png)
		* `margin:(上 右 下 左)`:
			* `margin:auto`会告诉文档自动将元素的左边空白和右边空白设为相等,即在页面中将它居中
			* `margin-top;margin-right;margin-bottom;margin-left` 是单独指定某个方向的边距空白的,下面的几种也是这样的形式.
		* `border:(width style color)`:边框的定义
		* `padding:(上 右 下 左)`:**是定义`border`距离`content`有多远.所以改变`padding`能看到元素边界面积的变化.**`background-color`的区域同`content`和`padding`相同(不确定,当div未设置宽高时,即使padding有值,content无值,border以内的内容无法显示--**一种推测:***因为padding是定义border距离content的距离,当content无值的时候,当然padding也就无效了,所以上面的这个问题可以这么解释*)
		* `content:(上 右 下 左)`:很明显这就是正文内容了,比如段落标签中的正文内容就是它所包裹的文字
	* 浮动(`float`)和清除浮动(`clear`)  

		* 浮动(`float`):left | right | none(默认值) | inherit(*任何的版本的 Internet Explorer(包括 IE8)都不支持属性值 "inherit"*)  

			**浮动可以理解为让某个元素脱离标准流,漂浮在标准流之上,和标准流不在一个层次上**
			+ A元素浮动后,会脱离文档流,它下面(这个下面指的是HTML代码中的先后顺序)的未脱离文档流的元素B会顶替原来A在文档流中的位置
			+ A、B两个元素都浮动，且浮动方向一致，那么哪那个元素最先浮动，哪个元素最靠近边缘，后浮动的元素紧挨着前一个元素浮动(这里的先后指的是HTML代码中的先后顺序)
		* 清除浮动(`clear`):none | left | right | both  

			所谓的清除浮动,就是让该元素的某一边或左右两边不允许存在浮动元素,比如`clear:left`,它会检测自己的左边有没有浮动元素,有的话就把自己换到下一横列去(**注意:这里是对自身位置做出调整**),举个栗子:  
			比如,这是两个右浮动的元素div1和div2,如果想让div2下移到div1下面该怎么办?(下面两个图片摘自[http://www.cnblogs.com/iyangyuan/archive/2013/03/27/2983813.html](http://www.cnblogs.com/iyangyuan/archive/2013/03/27/2983813.html "经验分享：CSS浮动(float,clear)通俗讲解"))  
			![清除前](/images/clear-float.png)  
			此时就该对div2的样式设置`clear:right`,相当于告诉div2:"看看右边有没有浮动元素,有就把自己挪到下一行去"  
			![清除后前](/images/clear-float-2.png)  
			**PS:`clear` 与`margin-top`混合使用时在不同浏览器中显示是不一样的，所以尽量避免这两个属性在同一元素上同时使用。**
	
	* 定位(`position`)
		* `position:static`  
			如果一个元素不指定`position`类型,则默认是`static`的.它遵循正常的文档流对象，对象占用文档空间，该定位方式下，top、right、bottom、left、z-index等属性是无效的。

		* `position:relative`(未脱离文档流,原位置仍保留,占着茅坑)  
			当一个元素被定义为此类型时,它将会在它本来应该坐落的位置上进行相对移动.(相对于文档流中的位置)

		* `positon:absolute`(脱离文档流,原位置不保留)  
			当一个元素被定义为此类型时,则此原素的位置和它离它最近祖先元素相关,并且这个祖先元素不可以定义为`static`(默认值)-**这种父元素称作"包含块"**,否则就找更远的祖先元素,如果都没有,那就以初始包含块为基准,也就是浏览器视窗渲染HTML的空间为基准,左上角为原点,通过调整(`left`,`right`,`top`,`bottom`)来定位

		* `position:fixed`(脱离文档流)  
			脱离了文档流，并且能够根据`top`,`right`,`left`,`bottom`属性进行定位，fixed是根据窗口为原点进行偏移定位的，也就是说它不会根据滚动条的滚动而进行偏移,像是黏在了屏幕上,无论怎么滚动,位置不变