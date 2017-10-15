---
layout: post
title: "Media Queries"
date: 2014-03-05 23:02:31 +0800
comments: true
categories: CSS
tag: CSS

---
媒体查询在CSS2中就有了，CSS3中得到了进一步增强，简单来说我们可以设置不同类型的媒体条件，并根据对应的条件，给相应符合条件的媒体调用相对应的样式表，说白了就是就是根据设备当前本身的环境(比如屏幕的长宽，设备的种类等）来对元素应用响应的CSS文件，但是所有的CSS 文件总是要下载到本地的。

#### 引入方式

- link方法引入 

		<link rel="stylesheet" type="text/css" href="../css/print.css" media="print" />

- xml方式引入

		 <?xml-stylesheet rel="stylesheet" media="screen" href="css/style.css" ？>

- @import方式引入

	   		@import url("css/reset.css") screen;

- @media引入

		@media screen{
		     选择器{
			属性：属性值；
		     }
		   }

#### 如何定义查询条件
媒体类型包括如下几种

- all 所有设备可用
- braille 盲文
- embossed
- handheld
- print 打印
- projection 投影
- screen 显示器
- speech
- tty 电传打字机 
- tv 电视

#### 最大宽度Max Width,max-height,max-width

		<link rel="stylesheet" media="screen and (max-width:600px)" href="small.css" type="text/css" />

也可在style 标签或者单独的CSS文件里面:

			@media only screen and (max-device-width: 480px) {
					div#wrapper {
						width: 400px;
					}

					div#header {
						background-image: url(media-queries-phone.jpg);
						height: 93px;
						position: relative;
					}

					div#header h1 {
						font-size: 140%;
					}

					#content {
						float: none;
						width: 100%;
					}

					#navigation {
						float:none;
						width: auto;
					}
				}

#### 最小宽度Min Width min-width,min-height

  	 	<link rel="stylesheet" media="screen and (min-width:900px)" href="big.css" type="text/css"  />

#### and not only关键字

##### 多个Media Queries使用查询取交集
 		<link rel="stylesheet" media="screen and (min-width:600px) and (max-width:900px)" href="style.css" type="text/css" />

##### not用于排除符合表达式的设备。

		<link rel="stylesheet" media="not print and (max-width: 1200px)" href="print.css" type="text/css" />

##### only用来定某种特定的媒体类型

		 <link rel="stylesheet" media="only screen and (max-device-width:240px)" href="android240.css" type="text/css" />

#### Device Width设备屏幕的输出宽度、即是设备的实际分辨率，也就是指可视面积分辨率、可以使用
max-device-width、min-device-width


#### orientation 横屏或者竖屏

#### device-pixel-ratio 设备的宽高比`min-device-pixel-ratio`，`max-device-pixel-ratio`

#### resolution 分辨率 `min-resolution`  `max-resolution`。

可以使用设备的宽高比，输出的最大最小宽度来判断设备的类型，甚至是具体的某一款设备。比如如下
使用media query为android手机在不同分辨率提供特定样式，这样就可以解决屏幕分辨率的不同给android手机的页面重构问题。

		 /*240px的宽度*/
		  <link rel="stylesheet" media="only screen and (max-device-width:240px)" href="android240.css" type="text/css" />
		  /*360px的宽度*/
		  <link rel="stylesheet" media="only screen and (min-device-width:241px) and (max-device-width:360px)" href="android360.css" type="text/css" />
		  /*480px的宽度*/
		  <link rel="stylesheet" media="only screen and (min-device-width:361px) and (max-device-width:480px)" href="android480.css" type="text/css" />。


总结：media query 就是让我们根据屏幕的大小来对HTML元素应用适合当前屏幕的样式，能够在不同尺寸的屏幕，甚至是不同的设备上达到较好的用户体验。