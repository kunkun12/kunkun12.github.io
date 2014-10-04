---
layout: post
title: "javascript bind function"
date: 2014-03-12 15:22:44 +0800
comments: true
categories: Javascript bind this scope
tag: javascript
---
记得半年之前在微博上看到一个问题，有人问如何才能证明自己真正理解了Javascript，有大神回答，最起码要能明白并写出bind 函数。当然这里的Javascript 仅仅是指的ECMASCRIPT本身，并不包含浏览器相关的那些。那么这篇博客这里就以bind开始吧。
首先搬出三个概念、

- 作用域：表示变量或函数起作用的区域，作用域链-Javascript的作用域链也是按有顺序查询的。在访问变量时，先查看本地作用域是否有此变量，如果没有则依次向上一级作用域查找直到全局作用域。如果全局作用域也没有此变量，那么将会在全局作用域中声明这个变量;
- 上下文：又可以理解为上下文对象，表示**当前代码执行时所处的环境**。即是this变量所指代的对象;
- 闭包：表示能访问和操作其他本地作用域的变量的表达式（通常是函数）;

在Javascript中`this`关键字是非常重要的，我之前也花费了不少时间来理解Javascript中的this是如何工作的。每个函数都有一个this变量，简单来说函数this是一个指针，它指向是不固定的，而是动态的，指向调用这个函数的对象，而这个对象就是上下文。我们都知道，Javascript里面的有作用域的概念，就是变量能够访问到的范围，this则是指向当前作用域的。通过this访问变量的时候会在当前作用域上查找。这篇博文是关于bind函数的，这个也是我们经常用的一个东西。

###提出问题
说起Javascript，很难避免的提到了闭包，每个闭包拥有自己的作用域以及this 变量闭包里面的this 并不是指向原来的对象。例如

		var getUserComments = function(callback) {
		    var numOfCommets = 34;
		    callback({ comments: numOfCommets });
		};
		var User = {
		    fullName: 'kunkun',
		    print: function() {
		        getUserComments(function(data) {
		            console.log(this.fullName + ' made ' + data.comments + ' comments');
		        });
		    }
		};
		User.print();

这里假设getUserComments 是一个异步的函数，通过一个回调函数来接受处理getUserComments返回的结果，在User对象里面我传入了一个匿名函数-闭包(即刚才说的回调的函数)，执行完毕的结果如下。

		undefined made 34 comments

可以看出这里的在调用print的时候。在刚才的回调函数里面`this.fullName`取值为undefined，可以看出此时这里面this 不再指向User对象了(其实是指向全局对比如在浏览器环境下的window);但是我期望的返回结果为
`kunkun made 34 comments`可以如下解决。

		var that= this;
		getUserComments(function(data) {
		    console.log(that.fullName + ' made ' + data.comments + ' comments');
		});

在执行print的时候，由于print是由User来调用的因此，print 里面的 this 是指向的User对象的，于是把this 赋值给力that ，所以that 这里保存了User的地址。所以在调用fullname的时候 ，就是User作用域下的fullname。

但是这种方法感觉有点混乱，同时也要定义像that这样新的变量，如果这样的情况出现多次的话，难免造成迷糊的。那么如何提前给这个函数固定好作用域呢。

####设置函数的作用域

这里提供了一种方式来设置函数的作用域：使用apply 或者call函数，这两个方法是在Function.prototype里面定义的，因此任何一个函数都继承了这两个方法，apply和call的第一个参数就是要给方法绑定的对象(即该方法调用时this指向的对象)。下面给定义一个getLength方法来获取fullname的length

		var getLength = function() {
		    console.log(this.fullName.length);
		}
		getLength.apply(User);

结果是这种方式的输出结果是6，说明我的目的是达到了，如果我们直接调用`getLength`方法呢，`getLength()`则会返回爆出异常，这种情况下 `getLength`是在最外层定义的，因此调用的时候this指向的是全局对象，但是全局对象中没有`fullName`这个属性，因此此时`this.fullName`为undfined，那么`this.fullName.length`则相当于执行了undfined.length，这是必然要爆出异常的。

以上使用apply调用的时候 getLength也自然地调用了，那么如何给getLength指定好上下文，而不让他调用的。等以后调用的时候函数里面的this 则指向我们自己设定的对象。我们不妨把这个功能称之为 bind。

####原生的Function.prototype.bind

如上提到的一样apply和call一样，他们都是Function.prototype对象中的方法，这个是ES5里面新增的方法。因此当前主流的浏览器都是支持的包括Chrome, Firefox, Opera，safari 其中IE 要在8以上。

在此将上面的代码修改如下

		var User = {
		    fullName: 'kunkun',
		    print: function() {
		        getUserComments(function(data) {
		            console.log(this.fullName + ' made ' + data.comments + ' comments');
		        }.bind(this));
		    }
		};
		User.print();

在上面绑定的时候调用bind(this)的时候， 在print里面的，此时print是由User调用的，因此的this指向User的。bind的第一个参数就是给该匿名函数绑定作用域。同时呢也可以使用该函数为函数绑定预定义的参数

		var User = {
		    fullName: 'kunkun',
		    print: function() {
		        getUserComments(function(msg, data) {
		            console.log(this.fullName + ' made ' + data.comments + ' comments' + msg);
		        }.bind(this, 'coder'));
		    }
		};

注意在调用的时候参数的顺序是在bind绑定的参数在前面。

####Polyfill-为旧版本的浏览器增加bind函数

同时也是文章开头的那道题要求的答案。

这里的解决方案就是首先判断Function.prototype中的是否具有这个bind函数，如果没有的话在这里为Function.prototype增加一个bind属性，具体实现还是通过apply来搞定的，`Function.prototype.bind()`返回一个函数，在这个函数执行的时候动态地执行要绑定作用域的函数的apply方法

		if (!Function.prototype.bind) {
		  Function.prototype.bind = function (oThis) {
		    if (typeof this !== "function") {
		      throw new TypeError("Function.prototype.bind - what is trying to be bound is not callable");
		    }	 
		    var aArgs = Array.prototype.slice.call(arguments, 1), 
		        fToBind = this, 
		        fNOP = function () {},
		        fBound = function () {
		          return fToBind.apply(
		            this instanceof fNOP && oThis ? this : oThis, 
		            aArgs.concat(Array.prototype.slice.call(arguments))
		          );
		        };	 
		    fNOP.prototype = this.prototype;
		    fBound.prototype = new fNOP();		 
		    return fBound;
		  };
		}

每个函数在执行的时候有个arguments 对象，这是一个类数组对象，是一个传入的参数列表，通过`Array.prototype.slice.call(arguments, 1)`来获取参数列表除第一个元素之外的元素列表，返回结果是一个数组。因为bind 函数第一个参数是要绑定的对象，之后的参数是预先为函数绑定的参数，同事呢这个函数在执行的时候也会传入参数，这个参数列表也在arguments里面，于是将这个两个参数列表进行合并。

		aArgs.concat(Array.prototype.slice.call(arguments))

将该数组传入给apply的第二个参数。apply第二个参数是arguments这样类型的，当然也可以是一个数组比如apply(obj,[参数一，参数二，参数三])，而call在这方面的区别就是参数列表是如 call(user,参数一，参数二，参数三)这种形式.apply调用要将参数封装到一个数组里面，这种方式适用用不清楚参数个数的情况下。而call则要求要明确参数的个数。很明显apply更灵活、功能要强一些。

如果要了解更多信息可以参见<https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind>

最后自己瞎编了两道题

#####第一道

		var name="张三"
		function Person(name){
		this.name=name
		};

		Person.prototype.getName=function(){
		console.log(this.name)
		};

		var p=new Person("李四");
		 p.getName();

		 var m=p.getName;

		 m();


#####第二道
根据文章继续文章中使用Demo

		var getUserComments = function(callback) {
				    var numOfCommets = 34;
				    callback({ comments: numOfCommets });
				};
		var User = {
				    fullName: 'kunkun',
				    print: function() {
				        getUserComments(function(data) {
				            console.log(this.fullName + ' made ' + data.comments + ' comments');
				        }.bind(this));
				    }
				};
		var p=	User.print;
			p()