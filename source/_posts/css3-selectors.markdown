---
layout: post
title: "CSS3 选择器总结"
date: 2014-03-24 11:47:21 +0800
comments: true
categories: CSS
tag: CSS
---
> 在学习新技术的同时不要忘记复习一下那些最基础的东西

####基本选择器
- 通配符选择器(*) 通配符选择器是用来选择所有元素，，也可以选择某个元素下的所有元素
- 元素选择器 根据tag名字来选择 比如 p body div 等等
- 类选择器 .className
- id选择器 #ID
- 后代选择器 A B (A、B分别为选择器 中间) 有空格
- 子元素选择器(A>B)(IE6不支持子元素选择器)。
- 相邻兄弟元素选择器(E + F)
> 相邻兄弟选择器可以选择紧接在另一元素后的元素，而且他们具有一个相同的父元素，换句话说，EF两元素具有一个相同的父元素，而且Ｆ元素在Ｅ元素后面，而且相邻，这样我们就可以使用相邻兄弟元素选择器来选择Ｆ元素。
- 通用兄弟选择器（Ｅ >Ｆ）
> 通用兄弟元素选择器是CSS3新增加一种选择器，这种选择器将选择某元素后面的所有兄弟元素，他们也和相邻兄弟元素类似，需要在同一个父元素之中，换句话说，E和F元素是属于同一父元素之内，并且F元素在Ｅ元素之后，那么E ~ F 选择器将选择中所有Ｅ元素后面的Ｆ元素。比如下面的代码：
- 群组选择器（selector1,selector2,...,selectorN）
> 群组选择器是将具有相同样式的元素分组在一起，每个选择器之间使用逗号“，”隔开，如上面所示selector1,selector2,...,selectorN。这个逗号告诉浏览器，规则中包含多个不同的选择器，如果不有这个逗号，那么所表达的意就完全不同了，省去逗号就成了我们前面所说的后代选择器，这一点大家在使用中千万要小心加小心。

####属性选择器

- E[attr]：只使用属性名，而不论这个属性值是什么，你就可以使用这个属性选择器
- E[attr="value"]：指定属性名，并指定了该属性的属性值为value；
- E[attr~="value"]：指定属性名，并且具有属性值，此属性值是一个词列表，并且以空格隔开，其中词列表中包含了一个value词，而且等号前面的“~”不能不写;属性选择器中有波浪（〜）时属性值有value时就相匹配，没有波浪（〜）时属性值要完全是value时才匹配。
- E[attr^="value"]：属性值是以value开头的；
- E[attr$="value"]：而且属性值是以value结束的；
- E[attr*="value"]：而且属值中包含了value；
- E[attr|="value"]：属性值是value或者以“value-”开头的值

####伪类选择器
#####动态伪类

- :hover用于当用户把鼠标移动到元素上面时的效果；
- :active用于用户点击元素那一下的效果（正发生在点的那一下，松开鼠标左键此动作也就完成了）
- :focus用于元素成为焦点，这个经常用在表单元素上。
#####UI元素状态伪类

- :enabled 表单元素可用（比如"type="text"有enable和disabled两种状态，前者为可写状态后者为不可状态）
- :disabled	
- :checked 表单元素选中适用于radio checked
#####CSS3的:nth选择器

- :fist-child选择某个元素的第一个子元素；
- :last-child选择某个元素的最后一个子元素；
- :nth-child()选择某个元素的一个或多个特定的子元素；

> * :nth-child(length);参数是具体数字
> * :nth-child(n);参数是n,n从0开始计算
> * :nth-child(n*length)n的倍数选择，n从0开始算
> * :nth-child(n+length);选择大于length后面的元素
> * :nth-child(-n+length)选择小于length前面的元素
> * :nth-child(n*length+1);表示隔n选一

- :nth-last-child()选择某个元素的一个或多个特定的子元素，从这个元素的最后一个子元素开始算；
- :nth-of-type()选择指定的元素；
- :nth-last-of-type()选择指定的元素，从元素的最后一个开始计算；
- :first-of-type选择一个上级元素下的第一个同类子元素；
- :last-of-type选择一个上级元素的最后一个同类子元素；
- :only-child选择的元素是它的父元素的唯一一个了元素；
- :only-of-type选择一个元素是它的上级元素的唯一一个相同类型的子元素；
- :empty是用来选择没有任何内容的元素，这里没有内容指的是一点内容都没有，哪怕是一个空格.

#####否定选择器（:not）selctorA:not(slectorB)

> 用来过滤元素，比如要选择除了type为button之外的任何input可以使用 input:not([type="button"])
使用
#####伪元素
CSS 3之前使用*:*在CSS3中使用*::*

- ::first-line选择元素内容的第一行
- ::first-letter选择文本块的第一个字母，比如用来实现首字下沉。
- ::before和::after这两个主要用来给元素的前面或后面插入内容，这两个常用"content"配合使用，比如流行的清除浮动。

		.clearfix:before,
		.clearfix:after {
			content: ".";
			display: block;
			height: 0;
			visibility: hidden;
		}
		.clearfix:after {clear: both;}
		.clearfix {zoom: 1;}
-::selection用来改变浏览网页选中文的默认效果