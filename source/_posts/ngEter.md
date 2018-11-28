title: angularJS 实现ngEnter
date: 2014-12-30 23:38:40
tags: AngularJS 
categories: AngularJS
---

Ng中有一些内置的指令，来响应我们的操作事件，比如ng-click,ng-change ng-select等等，但是我发现众多指令中缺少一个 当键盘按下的指令，比如 按回车的指令，比如我在一个搜索框中输入关键字之后按回车键，然后自动进行关键字来搜索，虽然ng中没有自带这个指令，但是我们很容易的定义一个这样的指令。下面就是一个ng-enter指令来完成这样的操作，使用的时候与ng-click这等方法类似，传递一个函数即可。当绑定该指令的元素处于焦点状态的时候，按下回车键就会触发 ng-enter的函数。

		app.directive('ngEnter', function () {
		    return function (scope, element, attrs) {
		        element.bind("keydown keypress", function (event) {
		            if(event.which === 13) {
		                scope.$apply(function (){
		                    scope.$eval(attrs.ngEnter);
		                });
		 
		                event.preventDefault();
		            }
		        });
		    };
		});

关于 `keydown`,`keypress`可以参阅<http://www.cnblogs.com/silence516/archive/2013/01/25/2876611.html>

使用方式 `<input ng-enter='myfunction()'/>`