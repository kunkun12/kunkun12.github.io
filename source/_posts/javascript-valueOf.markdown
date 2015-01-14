---
layout: post
title: "javascript valueOf方法"
date: 2014-03-09 20:29:03 +0800
comments: true
categories: javascript
tag: JavaScript
---
如果将对象用于Javascript的关系比较运算符，比如`<`、`<=`，javascript会首先调用对象的valueOf方法，如果这个方法返回一个原始值，则直接比较原始值。定义一个类Person

		function person(age,name){
			this.age=age;
			this.valueOf=function(){
				return this.age;
			}
		}

然后新建两个Person的实例。

		var person1=new person(12,'张三');
		var person2=new person(13,'李四');
		console.log(person1<person2) //true
		console.log(person1>person2)//false

如果构造函数没有valueOf方法，以上对象无轮如何比较都是输出false
此外在对两个对象求和的话，也会尝试调用valueOf方法

比如
		var person1=new person(12,'张三');
		var person2=new person(13,'李四');
		console.log(person1+person2) //25

当然如果构造函数没有valueOf方法,则会对象会先自动调用的toString方法。。默认情况下对象使用的是Object的toString方法的话则输出`[object Object]`;我们也可以重写toString方法。

			function person(age,name){
				this.age=age;
				this.name=name;
				this.toString=function(){
					return this.name;
				}
			}

			var person1=new person(12,'张三');
			var person2=new person(13,'李四');
			console.log(person1+person2);//张三李四。

如果一个构造函数中同时重写了valueOf和toString的话则只调用valueOf返回的结果参与运算.如下

		function person(age,name){
			this.age=age;
			this.valueOf=function(){
				return this.age;
			}
			this.toString=function(){
				return this.name;
			}
		}

		var person1=new person(12,'张三');
		var person2=new person(13,'李四');
		console.log(person1+person2);//25
