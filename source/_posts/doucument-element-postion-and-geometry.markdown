---
layout: post
title: "文档元素的几何形状以及位置判定"
date: 2014-03-09 21:00:15 +0800
comments: true
categories: Html
tag: Html
---
元素的位置是以像素来度量的,向右代表X坐标增加，向下代表Y坐标增加，但是有两个不同的点作为坐标系的原点，元素的X和Y坐标可以相对于文档的左上角，或者相对于其中文档视口的左上角.

“视口”是我们人眼看到的页面,只是实际显示文档内容的一部分：他不包括浏览器的外壳，(如菜单，工具条，标签页等),另一个是文档坐标，他的原点是页面的左上角，不随着页面的滚动发生变化，而视口坐标系的原点位置相对浏览器窗体是不发生变化的，当页面中没有滚动条时候，视口坐标系原点与文档坐标系原点重合的。然后如果存在滚动条的话。向下滚动页面，则某一元素的文档坐标是不发生变化的，视口的Y坐标变小。同理向右滚动的话，元素相对时候X坐标变小。

文档坐标比视口坐标更加基础，并且不随着用户滚动页面而发生变化，当使用CSS指定元素位置是使用了文档坐标吗，但是客户端编程中视口坐标是非常常见的，简单的查询元素在显示页面中位置方法是返回视口坐标系的位置。

为了再坐标系之前进行互相转换，我们需要判定浏览器窗口滚动条的位置，windows对象的pageXOffset和pageYOffset，属性在所有浏览器中提供了这些值除了IE8 以及更低版本的浏览器，不过可以使用scrollleft和scrollTop属性来获取滚动条的位置，正常情况下使用document.documentElemt来获得，在怪异模式下使用document.body对象来获得。下面封装了一个获取浏览器中滚动条位置的方法，并且兼容所有的浏览器

### 获得浏览中滚动条的位置

		function getScrollOffset(w){
					w=w||window;
					if(w.pageXOffset)return {
						x:w.pageXOffset,
						y:w.pageYOffset
					}
				var d=w.document;
				if(document.compatMode=='CSS1Compact'){
					return {x:d.document.scrollLeft,y:d.document.scrollTop}
				}

				return {
					x:d.body.scrollLeft,
					y:d.body.scrollTop
				}
		};

### 查询窗口视口的尺寸，确定当前文档中哪些部分是可见的。

		function getViewportSize(w){
			w=w||window;
			//ie8+
			if(w.innerWidth !=null)return{w:w.innerWidth ,h:w.innerHeight}
			 //ie标准模式以及其他浏览器
				var d=w.document;
			if(document.compatMode=='CSS1Compact'){
				return {
					w:d.document.clientWidth,
					h:d.document.clientHeight
				}
			}
			// 怪异模式下的浏览器
			return{
				w:d.body.document.clientWidth,
				h:d.body.document.clientHeight
			}

		}

### 查询元素的尺寸
判定一个元素位置最简单的方法是调用他的getBoundingClientRect()方法，该方法是在IE5中引入的，而现在当前的浏览器中都实现了，返回一个有left、right、top 、bottom 属性的对象。left,top表示左上角的坐标，right、bottom表示右下角的坐标。这个方法返回的是元素在视口坐标中的位置。为了转化为相对文档文档坐标需要加上滚动的偏移量。通过上面的getScrollOffset(w)获取滚动条的位置。

		var box=e.getBoundingClientRect();
		var offsets=getScrollOffset();
		var x=box.left;
		var y=box.top;

很多浏览器中还可以获得width和height属性但是IE中未实现。getBoundingClientRect()返回的值包括内边距和边框，但是不包含外边距。
使用elementFromPoint查询视口中指定位置有什么元素。参数为视口坐标系下的x,y坐标

任何HTML元素的只读属性offsetWidth 和offsetHeight以CSS像素返回他的屏幕的尺寸，返回的尺寸包括元素的边框和内边距，不包括外边距。所有的HTML元素用有OffsetLeft和offsetTop属性来返回元素的X和Y坐标，这些值都是文档坐标。对于已经定位的元素的后代和一些其他的元素，这些属性返回是相对于祖先元素而非文档，offsetParent指定这些元素相对的父元素，如果offsetParent为null这些属性都是文档坐标，一般来说用offsetLeft和OffsetTop来计算元素e的位置需要一个循环

		function getElmentPosition(e){
			var x=0,y=0;
			while(e){
				x+=e.offsetLeft;
				y+=e.offsetTop;
				e=e.offsetParentl
			}
			return{
				x:x,
				y:y
			}
		}

所有HTML元素定义了如下与位置相关的属性。

offsetwidth		clientWidth 	scrollWidth
offsetHeight	clientHeight	scrollHeight
offsetLeft		clientLeft		scrollLeft
offsetTop		clientTop		scrollTop
offsetParent

offsetHeight、offsetwidth包含内边距和边框、clientWidth、clientHeight不包含边框，只包含内容和内边距。scrollWidth、scrollHeight内容+内边距+任何内容溢出的尺寸。其中scrollLeft、scrollTop指定滚动条的位置。当文档中包含滚动条且有内容溢出时上面的getElmentPosition(e) 不在起作用了，因为没有把滚动条考虑进去。offsetLeft offsetTop为文档坐标包含了滚动条的位置	修改版本如下

		function getElementPos(elt){
			var x=0,y=0;
			for(var e=elt;e!=null;e=e.offsetParent){
				x+=e.offsetLeft；
				y+e.offsetTop;
			}
			//offsetLeft offsetTop为文档坐标包含了滚动条的位置，再次循环所有的祖先元素，
			//减去滚动条的偏移量，并转换为视口坐标
			for(var e=elt.parentNode;e!=null&&e.nodeType==1;e=e.parentNode){
				x-=e.scrollLeft;
				y-=e.scrollTop;
			}
			return {
					x:x;
					y:y
			}
		}



-  文档的高度 document.body.offsetHeight
-  视口的高度 window.innerHeight
-  滚动条滑动的高度 window.scrollY

