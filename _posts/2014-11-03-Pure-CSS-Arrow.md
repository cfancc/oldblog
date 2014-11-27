---
layout: post
title: CSS实现纯色箭头
---
#CSS实现纯色箭头
---
因为项目需要画一些流程图,流程块很容易实现,只需要对一个div操作一下就好,但是流程箭头却不容易实现,从网上搜寻了一部分资料,用CSS可以完美实现.
最终效果如下:
<div>
<div id="lcshaft" style="margin-left: auto;margin-right: auto;width:2px;height:30px;background-color:black;">
</div>
<div id="lcarrowhead" style="margin-left: auto;margin-right: auto;width: 0;height: 0;border-left: 8px solid transparent;border-right: 8px solid transparent;border-top: 12px solid black;">
</div>
</div>
###HTML代码
    <div>
		<!--创建箭头的柄-->
		<div class="shaft"></div>
		<!--创建箭头的头-->
		<div class="arrowhead"></div>
	</div>

###CSS代码
    .shaft{/*画柄*/
		margin-left: auto;
		margin-right: auto;/*这两项auto是自动居中*/
		width:2px;
		height:30px;
		background-color:black;/*填充柄为黑色*/
    }

    .arrowhead{/*画箭头*/
		margin-left: auto;
		margin-right: auto;/*这两项auto是自动居中*/
		width: 0;
		height: 0;/*将content设为0宽0高,可以创造出四个三角形环绕的div*/
		border-left: 8px solid 	transparent;/*将border左侧(三角形)隐藏*/
		border-right: 8px solid transparent;/*将border右侧(三角形)隐藏*/
		border-top: 12px solid black;/*将border顶侧(三角形)填充黑色*/
    }

当然这是通过两个div才实现的,一个div也可以实现,是利用伪元素的方式,可我不大擅长CSS,所以只能有机会再实现了!
