---
layout: post
title: "7行代码实现异步函数顺序执行-No Callback"
date: 2014-03-24 10:55:56 +0800
comments: true
categories: javascript
tag: javascript
---
首先假设我们有两个函数，分别按顺序被调用，但是他们都会操作一个相同的变量，第一个函数式为这个变量赋值，第二个函数是使用这个变量：代码如下

		var value;
		var A = function() {
		    setTimeout(function() {
		        value = 10;
		    }, 200);
		}
		var B = function() {
		    console.log(value);
		}

如果我们先后调用`A()` ,`B()` 我们将会得到输出结果为	`undefined`，因为函数A式异步的设置变量的值，在执行B的时候，还没有为value赋值，但是我们可以给A传递一个回调函数，带赋值之后执行这个函数，如下

		var value;
		var A = function(callback) {
		  setTimeout(function() {
		    value = 10;
		    callback();
		  }, 200);
		};
		var B = function() {
		  console.log(value);
		};
		 
		A(function() {
		  B();
		});

这种方式是可行的，但是假设，如果我要执行四个甚至更异步的方法，这样一直传递回调函数的话将会让代码变得杂乱，并且代码也会失去美观，于是这就是传说中的callback hell，就像这样


		A(function() {
			B(function(){
				 C(function(){
				  		D(function(){
				  			E();
				  		});
				  	});
				  });
				});

可以考虑写一个小的函数，来帮助我们来完成这个过程，首先从最简单的开始

		var queue = function(funcs) {
		    
		}

我们可以通过queue([A,B]);分别实现A和B的顺序调用。分别获得每个函数，然后执行

		var queue = function(funcs) {
		    var f = funcs.shift();
		    f();
		}

如果执行这些代码话,将会得到如下的异常`TypeError: undefined is not a function.`,因为A需要接受一个回调函数作为参数，但是并没有传递参数，所以会报错了，下面定义一个next函数作为A的参数进行传递

		var queue = function(funcs) {
		    var next = function() {
		        // ...
		    };
		    var f = funcs.shift();
		    f(next);
		};

next函数是在A执行结束之后执行的，这样只是执行了函数A，如果要执行数组里面的每一个函数可以做如下调整

		var queue = function(funcs) {
		    var next = function() {
		        var f = funcs.shift();
		        f(next);
		    };
		    next();
		};

到此为止我们基本上已经实现了我们的目标了。通过queue([A,B]);可以实现A和B的顺序执行，即A执行完之后才执行B。以上代码中的`shift`是一个关键的函数，他移除并返回数组中的第一个元素，执行多次之后，这个数组最后悔变为空的，因此最终会导致错误，为了验证这个，假设这里有两个函数，我们不知道他们的执行顺序，它们都会接受一个回调函数，并在函数的末尾执行回调函数。

		var A = function(callback) {
		    setTimeout(function() {
		        value = 10;
		        callback();
		    }, 200);
		};
		var B = function(callback) {
		    console.log(value);
		    callback();
		};

如果按照顺序调用queue([A,B])的话，我们会得到`TypeError: undefined is not a function`。为了避免这种情况的发生，在next函数里面要检查funcs是否为空

		var queue = function(funcs) {
		    (function next() {
		        if(funcs.length > 0) {
		            var f = funcs.shift();
		            f(next);
		        }
		    })();
		};

下面考虑一下更多的情况，这个函数里面的this是在闭包里面的，默认是指向了全局的window对象，如果我们可以给他绑定我们自己的作用域会更好一些比如：

		var queue = function(funcs, scope) {
		    (function next() {
		          if(funcs.length > 0) {
		              var f = funcs.shift();
		              f.apply(scope, [next]);
		          }
		    })();

我用 `apply` 替换了`f(next)`来实现设置函数的作用域以及传递next作为参数，最后一步就是考虑参数函数之间的传递，首先我们不知道传递的参数的个数，于是使用`arguments`变量，这个变量存在每个函数中的，是一个ArrayLike对象、里面存储了执行改函数时传递过来的参数列表，因为apply函数的第二个参数需要接受一个真的数组，来传递参数列表。可以使用`Array.prototype.slice.call(arguments, 0)` 搞定

		var queue = function(funcs, scope) {
		    (function next() {
		          if(funcs.length > 0) {
		              var f = funcs.shift();
		              f.apply(scope, [next].concat(Array.prototype.slice.call(arguments, 0)));
		          }
		    })();
		};

到此为止这个功能已经完全实现了让异步函数的同步执行，可以调用方式如下

				var obj = {
				    value: null
				};
				 
				queue([
				    function(callback) {
				        var self = this;
			        setTimeout(function() {
			            self.value = 10;
			            callback(20);
			        }, 200);
			    },
			    function(callback, add) {
			        console.log(this.value + add);
			        callback();
			    },
			    function() {
			        console.log(obj.value);
			    }
			], obj);

函数的执行结果 为

		30
		10

言之就是把这些异步的函数放在一个数组里，然后通过next里面的shift来获取函数并传递next并实现了依次调用、最后列出queue函数的源码

		var queue = function(funcs, scope) {
		    (function next() {
		          if(funcs.length > 0) {
		              funcs.shift().apply(scope || {}, [next].concat(Array.prototype.slice.call(arguments, 0)));
		          }
		    })();
		};

原文地址：<http://krasimirtsonev.com/blog/article/7-lines-JavaScript-library-for-calling-asynchronous-functions>