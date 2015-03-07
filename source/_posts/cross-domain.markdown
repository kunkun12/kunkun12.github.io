---
layout: post
title: "跨域解决方案之JSONP,CORS"
date: 2014-03-14 14:47:21 +0800
comments: true
categories: 跨域
tag: 跨域
---
####何为跨域

ajax出现，能够让页面无刷新的同时调用服务，浏览器出于安全方面的考虑，在进行Ajax 访问的时候不允许XMLHttpRequest跨域调用其他页面的对象。这里指的就是不允许XMLHttpRequest跨域调用服务，所谓跨域是指如果服务的URL中协议 hostname、端口号与当前页面的这三个参数中有一个不同的话则视为跨域，如下情况

- http://www.baidu.com:6080 与http://www.baidu.com 端口号不同
- https://www.baidu.com:6080 与https://www.baidu.com:6080 协议不同
- http://www.QQ:6080 与http://www.baidu.com  hostname不同。

如上的各种组合还有好多情况，那么我们如何才能绕过浏览器的跨域限制 正常地访问服务呢。

在flash或者silverlight中，只要服务的页面下放入 跨域策略文件就可以允许不同域下flash/silverlight的页面来调用,内容大致如下

		<cross-domain-policy> 
		<allow-access-from domain="*" to-ports="507" /> 
		<allow-access-from domain="*.example.com" to-ports="507,516" /> 
		<allow-access-from domain="*.example2.com" to-ports="516-523" /> 
		<allow-access-from domain="www.example2.com" to-ports="507,516-523" /> 
		<allow-access-from domain="www.example3.com" to-ports="*" /> 
		</cross-domain-policy> 

关于Flash 的跨域情况 在这里不说解释,本文重点介绍Javascript的情况。

####JSONP
虽然浏览器默认禁止了跨域访问，但并不禁止在页面中引用其他域的JS文件，并可以自由执行引入的JS文件中的function（包括操作cookie、Dom等等）。根据这一点，可以方便地通过创建script节点的方法来实现完全跨域的通信。这种方式叫JSONP解决方案。

利用动态的script来请求服务，服务端返回函数的调用的形式，比如callback("hello world");前端如果实现已经定义好了一个callback 的函数既可以获得helloworld;下面开始具体实现。

服务端使用NodeJS+express实现。新建express项目，app.js如下

		var express = require('express');
		var http = require('http');
		var path = require('path');
		var app = express();
		app.set('port', process.env.PORT || 3000);
		app.set('port1', process.env.PORT || 3002);
		app.use(app.router);
		app.use(express.static(path.join(__dirname, 'public')))

				function add(a,b){
					return a+b;
				} 
		app.get('/add', function(req,res){
			var a=parseInt(req.query.a);
			var b=parseInt(req.query.b);
		 	res.send("addresult("+add(a,b)+")");
		});

		http.createServer(app).listen(app.get('port'), function(){
		  console.log('Express server listening on port ' + app.get('port'));
		});
		http.createServer(app).listen(app.get('port1'), function(){
		  console.log('Express server listening on port ' + app.get('port1'));
		});

为了方便演示，这里的跨域具体是指端口号不同，并且在一个文件里面开了两个server，一个3000端口，一个3002端口，这里的服务主要是实现一个加法的功能，读取两个参数，然后进行加法运算，最后返回结果用addresult包起来，比如前端传入过来的参数分别为a=3,b=7,则返回的参数为addresult(7) `app.use(express.static(path.join(__dirname, 'public')))`是将当前目录下的public 文件件配置为静态目录，接下来在public文件夹下新建crossdomain.html文件，关键代码如下。

			 var   $=function(id){
							return document.getElementById(id);
				}				
					var a=$('a').value;
					var b=$('b').value;
				    var querystring="a="+a+"&b="+b;
					var script=document.createElement('script');
					script.async=true;
					script.src="http://localhost:3000/add?"+querystring;
					document.body.appendChild(script);
				

以上$为自定义的函数,根据ID 获取元素，然后从文本框中读取值。作为服务的参数传入。接下来要定义一个名字为addresult的函数来接受返回结果：

	function addresult (data) {
		// body... 
		$("result").innerHTML=a;
	}

启动程序输入 `http://localhost:3002/crossdomain.html`，这里是在3002上的页面调用的是3000端口上的服务。视为跨域，运行之后在文本框中输入两个值我们发现实现了跨域调用。
以上是实现了一个JSONP的精简版。当然还有些问题要考虑，比如回调函数名字的问题也可以作为参数来传入。第二个问题就是要在服务执行完毕或者请求失败之后 删除动态创建的script标签。

####CORS 

随着HTML5标准的 不断完善，也推出了一种新的跨域解决方案Cross-Origin Resource Sharing (CORS)，现在已经是W3C指定的标准。允许 XMLHttpRequest 进行跨域调用，支持CORS的浏览器有

- Chrome 3+
- Firefox 3.5+
- Opera 12+
- Safari 4+
- Internet Explorer 8+ (IE 8下为   XDomainRequest对象)

这里需要服务端和客户端配合 ，要求服务端返回的返回头中必须包含：

		Access-Control-Allow-Origin: //发起请求的XMLHttpRequest所在的域
		Access-Control-Allow-Methods: //支持的请求的方法
		Access-Control-Allow-Credentials:true

同时在请求服务的时候需要将xhr的withCredentials设置为true;如

		var xhr = new XMLHttpRequest();
		xhr.open("get", "url", true);
		xhr.withCredentials = true; //设置这个参数是为了允许cookie发送

这里请求的时候会发两次http请求，第一次是option请求，判断服务端时候只是CORS 以及支持哪些行为(post,get,put等)如果满足条件话则继续进行服务请求，同样这里也以NodeJS为例写个服务端。代码如下


		function multiply(a,b){
				return a*b;
		}
		app.get("/multiply",function(req,res){
			console.log(res.query);
			var a=parseInt(req.query.a);
			var b=parseInt(req.query.b);
			res.set({
					  'Access-Control-Allow-Origin': req.headers.origin,
					  'Access-Control-Allow-Credentials': true,
					  'Access-Control-Allow-Methods':'POST, GET, PUT, DELETE, OPTIONS'
					});
		});

客户端的带代码如下

			var a=$('a').value;
			var b=$('b').value;
			var querystring="c="+a+"&d="+b;
			var xhr = new XMLHttpRequest();
				xhr.open("get", "http://localhost:3000/multiply?"+querystring, true);
				xhr.withCredentials = true;//经测试这个貌似没有什么作用
				xhr.onload =function(){
				var data = xhr.responseText;
				$("result").innerHTML="这两个数的乘积为"+data;
			}
		xhr.send();　　

####其他的跨域解决方案。还有比如window.name,iframe，img标签等方式
当然使用代理也是比较给力的，所谓代理就是相当于请求先发到当前页面所在的webserver上，在webserver上的编写一个程序转发请求到另一个本来要打算访问的服务器上，同时也可以将结果返回到浏览器端，这样浏览器就间接地访问了服务。同时代理 又分为 正向和反向两部分。。。。。额，话题扯远了。。

本博文的用到的[Demo的源码](https://github.com/kunkun12/cross-domain)

