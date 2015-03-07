title: 高性能动画
date: 2014-10-04 08:49:54
tags: JavaScript
---

现代浏览器在完成以下四种属性的动画时，消耗成本较低： position（位置）， scale（比例缩放）, rotation（旋转） 和 opacity（透明度）。如果你对其他的属性设置动画，你就需要对你的冒险负责。而且你的动画将可能达不到流畅的60fps

### 通过DevTools从文档对象模型到像素级别的观察

当你用 Chrome DevTools 的 timeline 来查看的话，你可以看到跟一下相似的模式

![](http://ww4.sinaimg.cn/bmiddle/6c92bd39gw1ekyxoznj95j20mb0dj760.jpg)

浏览器处理的过程很简单：计算元素的样式（重新计算样式），生成每个元素的几何形状和位置（布局），绘制图层中的每个像素（初始化绘图并且进行绘图）并且将图层绘制到屏幕上（图层的合成）。

为了生成流畅的动画，你需要让浏览器尽可能少地工作，而最好的方式就是指改变影响合成的属性——transform 和 opacity。瀑布流越高 ，浏览器为了计算每个像素，就做的越多。

### 对布局进行动画

当你改变一个元素的时候，浏览器可能需要计算布局（位置和大小），这将影响到所有被这项改变影响的元素。如果你改变了一个元素，那么其他元素的几何结构可能会需要重新计算。举个例子，如果你改变 *<html>*元素的width属性，那么它所有的子元素都将被影响。由于元素的溢出和相互之间的影响，改变可能导致文档树的布局自下而上地被重新计算。

文档树越大，计算布局所花费的时间就越长。所以你应该尽力避免对那些影响布局的的属性设置动画。

项目中一些常见的，会引起布局变化的属性CSS属性：


> width	height
> padding	margin
> display	border-width
> border	top
> position	font-size
> float	text-align
> overflow-y	font-weight
> overflow	left
> font-family	line-height
> vertical-align	right
> clear	white-space
> bottom	min-height

### 绘图属性的动画

对一个元素的改变也可能引起绘图，而在现代浏览器中，主要的绘制工作会在软件光栅化中进行。这取决于元素在你的应用中如何分层。挨着被改变的元素旁边的其他元素也可能需要被重新绘制。

有很多属性会引起元素的绘制，但以下这些是最常见的属性:

> color	border-style
> visibility	background
> text-decoration	background-image
> background-position	background-repeat
> outline-color	outline
> outline-style	border-radius
> outline-width	box-shadow
> background-size


如果你在元素中对以上的属性设置动画，那么将会引起重绘，并且元素所属的图层将提交给GPU进行处理。对于移动端设备来说，这代价是非常昂贵的，因为它们的CPU的处理能力明显弱于桌面端。这意味着，任务将用更长的时间来完成；并且CPU和GPU之间的带宽是有限的，所以数据的上传需要花费很长的一段时间。

###  合成属性的动画

有一个CSS属性，你可能认为它会引起重绘，但有时候并不会。就是：opacity. 当GPU在合成元素的纹理结构的时候，会以一个较低的alpha值去处理opacity的改变。它的条件是，元素必须是图层中唯一的一个元素。如果它和其它的元素组合在一起，那么对opacity的改变也会让GPU（错误地）淡化其它的元素。

在Blink和WebKit内核的浏览器中，对于在CSS的transition或者animation中有opacity的改变的元素，将会为其创建一个图层。但也有很多开发者使用translateZ(0)或者translate3d(0,0,0)来人为地强制性地创建一个图层。 强制创建一个图层可以确保图层被绘制完毕并且可以在动画开始的时候，马上进入就绪状态。

一个元素的变换，归结为改变它的位置，旋转角度和缩放。通常，位置的改变是使用left和top属性来改变的。问题是，如前面所述，left和top都会引起图层的变化，并且它的代价是昂贵的。更好的解决方案是使用不会引起图层变化的translate属性。

### 命令式动画vs说明式动画

开发者常常需要决定它们的动画用JavaScript（命令式）来完成还是用CSS（说明式）来完成。

### 命令式（javascript）

命令式动画主要的优点同时也是它主要的缺点的是：它在浏览器主进程的JavaScript中运行。主进程已经忙于运行其他的JavaScript，样式的计算，布局还有绘制。所以进程内存在这资源竞争。这实质上增加了掉帧的风险，可能这一帧是你认为最重要的帧。

JavaScript中的动画可以为你提供更多的控制：开始，暂停，回放，中断和取消等细节。


### 声明式（CSS)

作为替代的方案，你可以用CSS来实现你的渐变和动画。最主要的好处就是，浏览器会对动画进行优化。如果有需要，它会创建图层。并且可以在主进程之外完成一些操作。它最主要的缺点就是CSS动画相对于Javascript动画而言，缺乏表现力。并且很难有意义地组织动画，这意味着创造动画会带来较高的复杂度和错误率。



总结：目前动画 优先使用  opacity 来改变透明度.  transfrom——*translate* *rotate* *scale*