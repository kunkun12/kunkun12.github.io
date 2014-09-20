---
layout: post
title: "html5 canvas 2d绘图API 学习总结"
date: 2014-03-10 08:18:24 +0800
comments: true
categories: html5 canvas
tag: html5
---

###基本知识
canvas本身提供的属性和方法很少 属性有hegight,width(这个属性与通过CSS设置的是不同的),绘图功能主要是存在通过canvas.getContext()返回的上下文中.有个一个toDataUrl()方法, 可以将canvas的转为一定格式的图片。下面主要介绍，context2d里面的方法。之前学过C语言绘图，貌似方法很类似

获取绘图上下文

 		var context =canvas.getContext("2d");

canvas元素绘制图像的时候有两种方法，分别是

- context.fill()//填充
- context.stroke()//绘制边框

style:在进行图形绘制前，要设置好绘图的样式

- context.fillStyle//填充的样式
- context.strokeStyle//边框样式
- context.lineWidth//图形边框宽度

颜色的表示方式:

- 直接用颜色名称:"red" "green" "blue"
- 十六进制颜色值: "#EEEEFF"
- rgb(1-255,1-255,1-255)
- rgba(1-255,1-255,1-255,透明度)
默认情况下坐标原点为canvas的左上角，水平向右为X

#####绘制矩形  context.fillRect(x,y,width,height)  strokeRect(x,y,width,height)
- *x:矩形起点横坐标*
- *y:矩形起点纵坐标*
- *width:矩形长度*
- *height:矩形高度*

[查看Demo](http://kunkun12.github.io/CanvasDemo/Rect.html)

#####清除矩形区域 context.clearRect(x,y,width,height)

- *x:清除矩形起点横坐标*
- *y:清除矩形起点纵坐标*
- *width:清除矩形长度*
- *height:清除矩形高度*

[查看Demo](http://kunkun12.github.io/CanvasDemo/clearRect.html)

#####圆弧context.arc(x, y, radius, starAngle,endAngle, anticlockwise)

- *x:圆心的x坐标*
- *y:圆心的y坐标*
- *straAngle:开始角度*
- *endAngle:结束角度*
- *anticlockwise:是否逆时针（true）为逆时针，(false)为顺时针*

 [查看demo](http://kunkun12.com/CanvasDemo/drawline.html)

#####路径  context.beginPath()    context.closePath()

- 系统默认在绘制第一个路径的开始点为beginPath
- 如果画完前面的路径没有重新指定beginPath，那么画第其他路径的时候会将前面最近指定的beginPath后的全部路径重新绘制
- 每次调用context.fill（）的时候会自动把当次绘制的路径的开始点和结束点相连，接着填充封闭的部分

 [查看demo](http://kunkun12.github.io/CanvasDemo/closepath.html)
####绘制线段 context.moveTo(x,y)  context.lineTo(x,y)

- *x:x坐标*
- *y:y坐标*

每次画线都从moveTo的点到lineTo的点，
如果没有moveTo那么第一次lineTo的效果和moveTo一样，
每次lineTo后如果没有moveTo，那么下次lineTo的开始点为前一次lineTo的结束点

[查看demo](http://kunkun12.github.io/CanvasDemo/drawarc.html)

#####绘制贝塞尔曲线（贝济埃、bezier） context.bezierCurveTo(cp1x,cp1y,cp2x,cp2y,x,y) 

- *cp1x:第一个控制点x坐标*
- *cp1y:第一个控制点y坐标*
- *cp2y:第二个控制点y坐标*
- *x:终点x坐标*
- *y:终点y坐标*

#####绘制二次样条曲线 context.quadraticCurveTo(qcpx,qcpy,qx,qy)

- *qcpx:二次样条曲线控制点x坐标*
- *qcpy:二次样条曲线控制点y坐标*
- *qx:二次样条曲线终点x坐标*
- *qy:二次样条曲线终点y坐标*

[查看demo](http://kunkun12.github.io/CanvasDemo/bezier.html)

#####线性渐变 var lg= context.createLinearGradient(xStart,yStart,xEnd,yEnd);线性渐变颜色lg.addColorStop(offset,color)

- *xstart:渐变开始点x坐标*
- *ystart:渐变开始点y坐标*
- *xEnd:渐变结束点x坐标*
- *yEnd:渐变结束点y坐标*
- *offset:设定的颜色离渐变结束点的偏移量(0~1)*
- *color:绘制时要使用的颜色*

#####径向渐变（发散）var rg=context.createRadialGradient(xStart,yStart,radiusStart,xEnd,yEnd,radiusEnd)
#####径向渐变（发散）颜色rg.addColorStop(offset,color)

- *xStart:发散开始圆心x坐标*
- *yStart:发散开始圆心y坐标*
- *radiusStart:发散开始圆的半径*
- *xEnd:发散结束圆心的x坐标*
- *yEnd:发散结束圆心的y坐标*
- *radiusEnd:发散结束圆的半径*
- *offset:设定的颜色离渐变结束点的偏移量(0~1)*
- *color:绘制时要使用的颜色*

[渐变Demo](http://kunkun12.github.io/CanvasDemo/Gradient.html)

#####图形变换(坐标系变换)

1、平移context.translate(x,y)

- x:坐标原点向x轴方向平移x
- y:坐标原点向y轴方向平移y

2、缩放context.scale(x,y)

- x:x坐标轴按x比例缩放
- y:y坐标轴按y比例缩放

3、旋转context.rotate(angle)

-  angle:坐标轴旋转x角度（角度变化模型和画圆的模型一样)

#####矩阵变换 context.transform(m11,m12,m21,m22,dx,dy)
 所谓的矩阵变换其实是context内实现平移，缩放，旋转的一种机制,他的主要原理就是矩阵相乘
[参见demo](http://kunkun12.github.io/CanvasDemo/transform.html)

图形组合 context.globalCompositeOperation=type

图形组合就是两个图形相互叠加了图形的表现形式,是后画的覆盖掉先画的呢，还是相互重叠的部分不显示等等，至于怎么显示就取决于type的值了
type：

        source-over（默认值）:在原有图形上绘制新图形

        destination-over:在原有图形下绘制新图形

        source-in:显示原有图形和新图形的交集，新图形在上，所以颜色为新图形的颜色

        destination-in:显示原有图形和新图形的交集，原有图形在上，所以颜色为原有图形的颜色

        source-out:只显示新图形非交集部分

        destination-out:只显示原有图形非交集部分

        source-atop:显示原有图形和交集部分，新图形在上，所以交集部分的颜色为新图形的颜色

        destination-atop:显示新图形和交集部分，新图形在下，所以交集部分的颜色为原有图形的颜色

        lighter:原有图形和新图形都显示，交集部分做颜色叠加

        xor:重叠飞部分不现实

        copy:只显示新图形

#####给图形绘制阴影

- context.shadowOffsetX :阴影的横向位移量（默认值为0）
- context.shadowOffsetY :阴影的纵向位移量（默认值为0）
- context.shadowColor :阴影的颜色
- context.shadowBlur :阴影的模糊范围（值越大越模糊）

#####绘制图像 

- 绘图：context.drawImage

- 图像平铺：context.createPattern(image,type)

- 图像裁剪：context.clip()

- 像素处理：var imagedata=context.getImageData(sx,sy,sw,sh)

- 设置像素颜色：context.putImageData(imagedata,dx,dy,dirtyX,dirtyY,dirtyWidth,dirtyHeight)

#####绘制文字

- 填充文字：context.fillText(text,x,y)  
- 绘制文字轮廓 context.strokeText(text,x,y) 

- text:要绘制的文字
- x:文字起点的x坐标轴
- y:文字起点的y坐标轴
- context.font:设置字体样式
- context.textAlign:水平对齐方式
		start、end、right、center
- context.textBaseline:垂直对齐方式：top、hanging、middle、alphabetic、ideographic、bottom

#####保存和恢复上下文状态，主要包括坐标系，裁剪区域等 

- 保存：context.save()
- 恢复：context.restore()

#####保存文件  canvas.toDataURL(MIME)

在canvas中绘出的图片只是canvas标签而已，并非是真正的图片，是不能右键，另存为的，我们可以利用canvas.toDataURL()这个方法把canvas绘制的图形生成一幅图片，生成图片后，就能对图片进行相应的操作了。