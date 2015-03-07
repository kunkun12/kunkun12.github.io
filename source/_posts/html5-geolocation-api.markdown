---
layout: post
title: "HTML5 Geolocation API-获取设备的地理位置"
date: 2014-03-14 07:59:57 +0800
comments: true
categories: html5
tag: Html
---
###地理位置API介绍

地理位置API能够神奇地定位出你正待在世界的什么地方，并且在你允许的情况下，将你的位置信息分享给你信任的人，关于内部的定位原理，主要通过三种手段：

1. IP地址
2. GPS定位
3. MAC地址
4. GSM基站网络
5. 用户定义的地址位置。

###地理位置获取流程：

1、用户打开需要获取地理位置的web应用。
2、应用向浏览器请求地理位置，浏览器弹出询问窗口询问用户是否共享地理位置。
3、假设用户允许，浏览器从设别查询相关信息。
4、浏览器将相关信息发送到一个信任的位置服务器，服务器返回具体的地理位置。

###监测浏览器支持情况

如果浏览器支持地理位置API话，在全局的 navigator对象上回有一个名字为geolocation的属性，反之，navigator 对象上该属性为undefined 。于是可以编写如下函数

		function suport_geolocation(){
			return !!navigator.geolocation
		}

或者使用一个专门监测HTML5 特性的库Modernizr，提供的方法来监测浏览器是否支持地理位置API，如下

		if(Modernizr.geolocation){
			
		}
		else{
			
		}

###navigator对象中的方法，

####getCurrentPosition函数
这个函数是通过navigator.geolocation对象来调用的，所以在脚本中需要先取得此对象。这个函数接受一个必选参数和两个可选参数。

		void getCurrentPosition(

				in PositionCallback successCallBack,
				in optional PositionErrorCallback errorCallback,
				in optional PositionOptios options
		);

. successCallBack：必选参数,为浏览器指明位置数据可用时应调用的函数，即收到实际位置信息并进行处理的地方。
. errorCallback：可选参数，出错处理
. options：可选参数，用来调整HTML5 Geolocation服务的数据收集方式。

示例

		navigator.geolocation.getCurrentPosition(updateLocation,handleeLocationError);

这里updateLocation就是接收位置信息并进行重的函数，handleeLocationError是进行错误处理的函数。updateLocation只接受一个参数：位置对象。这个对象包含坐标(coords)和一个获取位置数据时的时间戳。以下是坐标的主要特性：

- latitude(纬度)
- longitude(经度)
- accuracy(准确度)

		function updateLocation(position) {
		        var latitude = position.coords.latitude;
		        var longitude = position.coords.longitude;

		        if (!latitude || !longitude) {
		            document.getElementById("status").innerHTML = "HTML5 Geolocation is supported in your browser, but location is currently not available.";
		            return;
		        }

		        document.getElementById("latitude").innerHTML = latitude;
		        document.getElementById("longitude").innerHTML = longitude;
		    }
handleeLocationError()函数

HTML5定义了一些错误编号

- PERMISSION_DENIED(错误编号为1)——用户选择拒绝浏览器获得其位置信息。
- POSITION_UNAVAILABLE(错误编号为2)——尝试获取用户位置数据失败。
- TIMEOUT(错误编号为3)——设置了可选的timeout值，获取用户位置超时。

		 function handleeLocationError(error)
		    {
		        switch (error.code) {
		            case 0: alert(error.message); break;
		            case 1: alert(error.message); break;
		            case 2: alert(error.message); break;
		            case 3: alert(error.message); break;
		        }
		    }

options：可选的地理定位请求特性

- enableHighAccuracy:如果启用该参数，则通知浏览器启用HTML5 Geolocation服务的高精度模式，参数的默认值为false.
- timeout:可选值，单位为ms，告诉浏览器计算当前位置所允许的最长时间。默认值为Infinity，即为无穷大或无限制。
- maximumAge:这个值表示浏览器重新计算位置的时间间隔。它也是一个以ms为单位的值，默认为零，这意味着浏览器每次请求时必须立即重新计算位置。

###watchPosition函数

		navigator.geolocation.watchPosition(updateLocation,handleLocationError);

监听位置的变化，如果位置发生变化则调用updateLocation函数，如果程序不再需要接收用户的位置信息，则可以调用`navigator.geolocation.clearWatch(watchId)`,例子如下

		var watchId = navigator.geolocation.watchPosition(updateLocation, handleeLocationError);
		navigator.geolocation.clearWatch(watchId);


很多情况下，获得的位置在地图上在来展示，可视化地展示出我们所在的位置，[参见demo](http://developers.arcgis.com/javascript/samples/exp_geolocate/)



