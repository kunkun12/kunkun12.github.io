title: css margin折叠
date: 2014-09-23 14:10:50
tags: css
---

### margin折叠-Collapsing margin

所谓margin折叠(Collapsing margin)，就是我们常说的盒子垂直边距叠加，具体是指在同一个块级格式化上下文（简称BFC）中，毗邻的两个或者多个垂直外边距会发生互相折叠的现象。其中所说的毗邻可以归结为以下两点

+	两个或者多个外边距没有被非空内容、padding、border或者clear分隔开。 
+	两个或者多个外边距所归属的元素都处于普通流中，而非浮动元素或者绝对定位元素。

### 常见发生margin折叠的情况有：

* 一个盒子元素的top margin与第一个子盒子元素的top margin。
* 一个盒子元素的bottom margin与最后一个子盒子元素的bottom margin， 但须在该盒子元素的height为auto的情况下 * 一个盒子元素的bottom margin与紧接着的下一个盒子元素的top margin。
* 一个盒子元素的top margin与其自身的bottom margin，但须满足没创建 BFC、零min-height、零或者“auto”的height、没有普通流的子盒子元素。

### margin折叠的后的外边距计算：

* 参加折叠的元素指都为正，取其中较大的值; 
* 参加折叠的元素值都为负，取绝对值较大的，然后负向位移; 
* 参加折叠的元素有正有负，取出正值中最大的值+负值中绝对值最大的值; 
* 元素自身的 margin-bottom 和 margin-top 相邻时也会折叠，自身 
margin-bottom 和 margin-top 相邻，只能是自身内容为空，垂直方向上 border、padding 为 0; 
* 相邻的 margin 要一起参与计算，不得分步计算，要注意，相邻的元素不一定非
要是兄弟节点，父子节点也可以，即使不是兄弟父子节点也可以相邻。而且，在计算时，相邻的 margin 要一起参与计算，不得分步计算。 

``` 
<div style="margin:50px 0; background:green; width:50px;"> 
<div style="margin:-60px 0;"> 
   <div style="margin:150px 0;">A</div> </div> </div> 
<div style="margin:-100px 0;background:green; width:50px;"> 
<div style="margin:-120px 0;"> 
    <div style="margin:200px 0;">B</div> </div> </div> ```


错误的计算方式：分别算 A 和其父元素的折叠，然后与其父元素的父元素的折叠，这个值算出来之后，应该是 90px。依此法算出 B 的为 80px；然后，A和B折叠，margin 为 90px。  请注意，多个 margin 相邻折叠成一个 margin，计算的时候，应该取所有相关的值一起计算，而不能分开分步来算。  以上例子中，A 和 B 之间的 margin 折叠产生的 margin，是6个相邻 margin 折叠的结果。将其 margin 值分为两组：    正值：50px，150px，200px   负值：-60px，-100px，-120px  根据有正有负时的计算规则，正值的最大值为 200px，负值中绝对值最大的是 -120px，所以，最终折叠后的 margin 应该是 200 + (-120) = 80px。


**创建了BFC(块级格式化上下文)的元素，不和内部的子元素发生margin 折叠**

