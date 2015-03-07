---
layout: post
title: "判断浏览器特性"
date: 2014-03-01 19:25:20 +0800
comments: true
categories: javascript
tag: JavaScript
---

###浏览器类型判断
有时候为了判断浏览器对HTML5/CSS3支持情况，我们常常会首先选择判断浏览器类型，浏览器对象中有一个navigator 对象，具有的主要属性如下
	appCodeName	--	浏览器代码名的字符串表示
	appName	--	官方浏览器名的字符串表示
	appVersion	--	浏览器版本信息的字符串表示
	cookieEnabled	--	如果启用cookie返回true，否则返回false
	javaEnabled	--	如果启用java返回true，否则返回false
	platform	--	浏览器所在计算机平台的字符串表示
	plugins	--	安装在浏览器中的插件数组
	taintEnabled	--	如果启用了数据污点返回true，否则返回false
	userAgent	--	用户代理头的字符串表示

我们其中户代理头的字符串userAgent包含了浏览器的信息，包括操作系统以及版本，以及浏览器的内核信息，通过这个属性可以判断出当前页面所处的浏览器环境，帮助针对不同的浏览器实现兼容性。
在Chrome中用户代理头的字符串内容如下
	"Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/33.0.1750.117 Safari/537.36"
firefox中则为
	"Mozilla/5.0 (Windows NT 6.1; WOW64; rv:27.0) Gecko/20100101 Firefox/27.0" 
通过正则表达式来匹配是否满足相对应的浏览器的特性

     	var Sys = {}; 
        var ua = navigator.userAgent.toLowerCase(); 
        var s; 
        (s = ua.match(/msie ([/d.]+)/)) ? Sys.ie = s[1] : 
        (s = ua.match(/firefox//([/d.]+)/)) ? Sys.firefox = s[1] : 
        (s = ua.match(/chrome//([/d.]+)/)) ? Sys.chrome = s[1] : 
        (s = ua.match(/opera.([/d.]+)/)) ? Sys.opera = s[1] : 
        (s = ua.match(/version//([/d.]+).*safari/)) ? Sys.safari = s[1] : 0;
测试代码

		if (Sys.ie) document.write('IE: ' + Sys.ie); 
        if (Sys.firefox) document.write('Firefox: ' + Sys.firefox); 
        if (Sys.chrome) document.write('Chrome: ' + Sys.chrome); 
        if (Sys.opera) document.write('Opera: ' + Sys.opera); 
        if (Sys.safari) document.write('Safari: ' + Sys.safari);

 在Jquery 1.9版本之前有个可以通过`$.browser`来判断浏览器的类型，但是之后版本中去掉了这个方法，但是可以通过 jQuery.migrate 插件来完成该功能，同时官网推荐[Modernizr](http://modernizr.com/)实现。

###  [Modernizr](http://modernizr.com/)是专门用来检测浏览器对HTML5和CSS3特性的JS库。

 引入Modernizr库之后，很方便地检测浏览的特性

	 if (Modernizr.borderradius) {
	       //do something
	    }
	        
	    if (Modernizr.csstransforms) {
	       //
	    }
在某些不支持新特性的浏览器上，Modernizr不仅仅提供了上述方式告诉你，也提供了load功能允许你加载一些[shim/polyfill](https://github.com/Modernizr/Modernizr/wiki/HTML5-Cross-Browser-Polyfills)脚本来达到支持的目的.

可以使用Modernizr的load()函数来动态加载脚本，该函数的test属性是表明要测试是否支持的新特性，如果测试成功支持的话，就加载yep属性设置的脚本，如果不支持就加载nope属性设置的脚本，不管是否支持，both属性里设置的脚本都会加载的。例子代码如下：

	Modernizr.load({
	    test: Modernizr.canvas,
	    yep:  'html5CanvasAvailable.js’,
	    nope: 'excanvas.js’, 
	    both: 'myCustomScript.js' 
	});
在该例子里，Modernizr会判断当前浏览器是否支持canvas特性，如果支持，那就会加载html5CanvasAvailable.js和myCustomScript.js这两个脚本，如果不支持，就会加载excanvas.js（用于IE9之前的版本）脚本文件以让该浏览器支持canvas功能，然后再加载myCustomScript.js脚本。

因为Modernizr可以加载脚本，所以你还可以用于其它的用途，比如，如果你引用的第三方脚本（例如提供CDN服务的Google和Microsoft提供jquery的托管）加载失败的情况下，可以加载备用的文件。下面的代码是Modernizr提供的一个加载jquery的示例：

		Modernizr.load([
		    {
		        load: '//ajax.googleapis.com/ajax/libs/jquery/1.6.4/jquery.js',
		        complete: function () {
		            if (!window.jQuery) {
		                Modernizr.load('js/libs/jquery-1.6.4.min.js');
		            }
		        }
		    },
		    {
		        // This will wait for the fallback to load and
		        // execute if it needs to.
		        load: 'needs-jQuery.js'
		    }
		]);

参考资料 [Detecting HTML5/CSS3 Features using Modernizr](http://weblogs.asp.net/dwahlin/archive/2011/11/16/detecting-html5-css3-features-using-modernizr.aspx)