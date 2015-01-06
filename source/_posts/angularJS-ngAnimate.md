title: angularJS 动画
date: 2015-01-06 21:08:15
tags:
---

### angularJS 动画

angularJS 官方提供了ngAnimate模块,让我们能够为现有的指令提供CSS和JAVAscript的动画。主要支持如下三种方式。

- CSS3 动画 
- CSS3 过度
- 使用javascript 动画。

###安装 

推荐使用bower安装

		bower install --save angular-animate

在代码中通过 如下方式引入

	    <script src='bower_components/angular/angular.js'></script>
		<script src='bower_components/angular-animate/angular-animate.js'></script>

 		angular.module('myApp',['ngAnimate'])

### 原理

$animate 服务基于指令发出的事件来添加特定的样式类，对于结构性动画比如新增DOM或者删除DOM元素（比如`ngRepeat,ngView,ngClude,ngSwitch,ngIf,ngCLass`)，添加相应的CSS类为 `ng-[EVENT]和ng-[EVENT]-acitve`.对于样式类动画比如（`ngClass,ngHide`）默认给动画元素的每个动画事件添加的样式类为`[CLASS]-add,[CLASS]-add-active,[CLASS]-remove,[CLASS]-remove-active`。当动画结束之后会自动删除这两个类，因此我们要实现期望的动画，根据指定的类名，定义相应的CSS即可。而CSS添加或者删除的时机由`$animate`来控制的。`$animate` 服务支持多个`Angular`的指令，无需额外的配置。所有这些预存在的支持动画的指令，都是监听指令上的事件实现的，例如当一个ng-if的元素显示到页面时，这个事件被称为`enter`事件，当`ngHide`准备显示一个元素的时候，`remove`事件就会触发。

下面为具体的指令在不同状态下出发的事件的列表。

- `ng-If` 当显示的时候触发`enter`事件，为元素添加 `ng-enter` 类，之后添加 `ng-enter-acitve`。我们可以在`ng-enter`里面定义动画之前的初始状态。`.ng-enter.ng-enter-acitve` 定义动画的类，可以为CSS3的过度（transition）或者CSS3动画（animate）。当元素移除即`ng-if的条件为false时，触发leave事件 在动画为被移除之前为元素添加 `ng-leave` 类，之后添加 `ng-leave-acitve`。可以在`ng-leave` 元素被移除之前的动画开始之前的状态，`.ng-leave.ng-leave-acitve` 为动画的隐藏样式可以为CSS3的过度（transition）或者CSS3动画（animate）下同

- ng-view 当新的路由页面显示的时候触发`enter`事件，为元素添加 `ng-enter` 类，之后添加 `ng-enter-acitve`，上一个路由页面触发leave事件 在动画为被移除之前为元素添加 `ng-leave `类，之后添加 `ng-leave-acitve`。

- `ngInclude，ngSwitch` 同上

- ngRepeat 当一项被插入到列表之后 触发enter事件，当一项从列表移除时触发remove事件，列表中的一项移动了触发move事件，可以通过定义.`ng-enter-stagger`来设置上一个列表元素进入之后的延时X秒之后插入。

- ngClass 当ngClass中的表达之求值为true时,触发add事件，在CLASSNAME 被添加之前，为元素顺序添加 `[CLASSNAME]-add和[CLASSNAME]-add-active`类，动画结束时移除这两个类，同时当ngClass求值为false时，在CLASSNAME的类被移除之前触发remove事件，为元素添加 `[CLASSNAME]-remove和[CLASSNAME]-remove-active`类。

- ngHide/ngShow 同上，只不过上面的CLASSNAME对应为ng-hide。即当ngHide为true或者ngShow为false时在元素隐藏之前，为元素顺序添加 `ng-hide-add和ng-hide-add-active`类 当ngHide为false或者ngShow为true时，在元素显示之前 为元素顺序添加` ng-hide-remove和ng-hide-remove-acitve`类。

### DEMO以 ng-repeat交错动画为例

定义CSS 

		.fade-in{
			width: 300px;
			height: 30px;
			background: red;		
			transition:2s linear all;
			transition-delay: 0;
		}
		.fade-in.ng-enter.ng-enter-active{
			opacity: 1;
		}
		.fade-in.ng-enter{
			opacity: 0;	
		}
		.fade-in.ng-enter-stagger{
			transition-delay:500ms;
			-webkit-transition-delay : 100ms;
			-webkit-transition-duration : 0;
			transition-duration:0s;
		} 

JS代码

		<body ng-app='myApp' ng-controller='myctrl' >
			<div  class='fade-in'></div>
			<button ng-click='clicl1()'>test</button>
			<ul   >
				<li ng-repeat='i in ListData' class='fade-in'>
					{{i}}
				</li>
			</ul>
			<script>
				angular.module('myApp',['ngAnimate'])
				.controller('myctrl',function($scope){
					$scope.ListData=[];
					$scope.clicl1=function(){
						for(var i=0 ;i<10;i++){
						$scope.ListData.push("这是我的测试数据第"+i+"行")
					}
					}
				});
			</script>
		</body>

可以看到 元素依次渐变显示出来。


