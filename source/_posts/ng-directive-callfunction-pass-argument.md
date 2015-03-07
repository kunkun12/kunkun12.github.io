title: angluarJS 指令与控制器中的方法的调用。
date: 2015-01-14 16:52:42
tags: AngularJS
---

### 指令调用控制器中的方式（给指令的事件绑定执行函数）
angluarJS中，如果指令内部的事件触发，这个时候需要调用外部controller中的方法，可以在指令的隔离作用域中使用`&`将controller中的方法传递到指令中，供指令调用，需要注意的事，如果传递参数的话（比如自定义的ngChange事件触发，会把当前的选择项作为参数传递到控制器中）这个时候在声明指令时候,需要填写方法调用的函数表达式，并且如果包含函数的话，需要在调用改方法的时候指明参数，该参数名字与在指令中调用中传递的参数JSON的key一致（指令中必须传递JSON的形式来调用）

		  	angular.module("app1", [])
		  .controller("Ctrl1", function($scope, $element)
		  {
		    $scope.someThingChanged = function(arg1,arg2)
		    {
		         console.log(arg1,arg2);
		    }
		  })
		  .directive("myDirective", function($timeout)
		  {
		    return {
		      scope: {
		        ngChange: '&'
		      },
		      link: function(scope, element, attrs)
		      {
		        $timeout(function()
		        {
		          scope.ngChange({
		            name:'张三',
		            age:28
		          });
		        });
		      }
		    }
		  })


声明指令

 		<div my-directive ng-change="someThingChanged(name,age)"></div>
 		调用时候上面输出 ：  张三 28

###    控制器调用指令的方法。

使用双向绑定的形式，首先在控制器新建一个空对象，在指令中将方法赋值给这个对象的一个属性，这样在控制器用就可以获取这个方法，进而进行调用。

		var app = angular.module('plunker', []);
		app.controller('MainCtrl', function($scope) {
		  $scope.focusinControl = {
		  };
		});
		app.directive('focusin', function factory() {
		  return {
		    restrict: 'E',
		    replace: true,
		    template: '<div>A:{{internalControl}}</div>',
		    scope: {
		      control: '='
		    },
		    link  : function (scope, element, attrs) {
		      scope.internalControl = scope.control || {};
		      scope.internalControl.takenTablets = 0;
		      scope.internalControl.takeTablet = function() {
		        scope.internalControl.takenTablets += 1;  
		      }
		    }
		  };
		});

html 

		<focusin control="focusinControl"></focusin>
