---
layout: post
title: "测试驱动开发：QUnit 介绍"
date: 2014-02-07 08:07:25 +0800
comments: true
categories: 单元测试 QUnit
categories: QUnit
---
在Javascript项目的开发流程中，测试驱动开发广为流传，测试驱动能够让我们节省编码的时间，本篇文章主要介绍[Qunit](http://qunitjs.com/),一个测试驱动的开发框架，来帮助我们发现程序的错误以及bug

###为什么要使用测试驱动开发
首先在最初使用单元测试的时候，这可能是一个繁杂和琐碎的过程，包括单元测试环境的搭建，以及为每一个要测试的函数编写测试代码，都会花费很多的时间，但是要相信单元测试最终会帮主我们来减少在一个项目周期中用来调试代码的时间。同时也会减少半夜接到电话被拉过去修改bug的麻烦，长远来说，这个过程会让你受益更多比如 跨浏览器的测试，使用这种方式能够让你在不同的浏览器中进行测试。总之编码之前，测试先行一个比较好的习惯。

随着Github等类似的代码分享平台的出现，现在开源的东西也越来越多了，怎么才能证明这些东西的可用性呢，因为开源模块的作者会编写单元测试的代码。当我们clone完这个模块之后 然后跑一边单元测试代码，会让自己开源的模块增强可用性。同时我们在为开源项目贡献代码的时候也建议附上单元测试的程序，以增强自己的代码被接受的可能性。

用Unit test 来代替调试，同时也能有利于团队合作，当他人看你写的模块的时候，看一下写的单元测试代码就明白该模块的功能了。单元测试能让我们更快地了解每个关键函数的功能。

话说回来，当项目完成之后 又发现了bug，当我们在修改完一个bug之后，怎样才能说明bug修改好了呢，单元测试这个时候也可以为我们提供帮助，几行单元测试就能证明这个bug被修复了，无需多言。

###为什么选择[Qunit](http://qunitjs.com/)?

原因是这个框架很简单上手，也是单元测试新手的最佳选择，就像Jquery那么easy，对于初学者来说，[Qunit](http://qunitjs.com/)跟jQuery是也是出自同一人之手。[Qunit](http://qunitjs.com/)也是用来测试jQuery 本身的单元测试框架， Qunit对于jQuery开发者也是单元测试框架的最佳选择。当然还有更多javascript单元测试框架比如 [mocha](http://visionmedia.github.io/mocha/),[jasmine](http://pivotal.github.io/jasmine/),断言库[chai](http://chaijs.com/),[should.js](https://github.com/visionmedia/should.js)、[Expect](https://github.com/LearnBoost/expect.js)、[better-assert](better-assert)等

###开始搭建环境
第一步就是先下载[Qunit](http://qunitjs.com/)的Javascript 和CSS文件。地址如下

		<link rel=“stylesheet" href="//code.jquery.com/qunit/qunit-1.14.0.css" type="text/css" media="screen">
		<script src=“//code.jquery.com/qunit/qunit-1.14.0.js”></script>

新建三个文件test.html、Calculator.js,test.js 我们用来展示单元测试的结果。Calculator.js编写我们要实现功能的程序，出于演示目的这里编写一个计算器的程序.test.js编写单元测试的代码。test.html代码如下

		<!doctype html>
		<html lang="en">
		<head>
			<meta charset="UTF-8">
			<title>Unit Test</title>
			<link rel="stylesheet" href="http://code.jquery.com/qunit/qunit-1.14.0.css" type="text/css" media="screen">
			<script src="http://code.jquery.com/qunit/qunit-1.14.0.js"></script>
		</head>
		<body>
				     <h1 id="qunit-header">QUnit Test Suite</h1>
		   			 <h2 id="qunit-banner"></h2>
		   			 <div id="qunit-testrunner-toolbar"></div>
		   			 <h2 id="qunit-userAgent"></h2>
		  		     <ol id="qunit-tests"></ol>

		  		     
		  		     <script type="text/javascript" src='./Calculator.js'></script>
		  		     <script type="text/javascript" src='./test.js'></script>
		</body>
		</html>	

其中body里面几个html元素是为了展示测试结果使用的，测试代码执行的时候，Qunit会把测试结果放到这些元素中，结合内置一些CSS 让我们方便地了解哪些测试通过了，哪些没有通过。
Calculator.js主要包含两个简单的函数 multiply返回两个数的乘积，isOdd判断一个数是不是奇数。

		function multiply(a,b) {
		  return a*b;
		}
		function isOdd(n) {
		   return Math.abs(n) % 2 == 1;
		}

test.js就是我们要写的测试了。代码如下

		test('isOdd()', function() {
		    ok(isOdd(3), 'Three is an odd number');
		    ok(isOdd(2), 'Two is not an odd number');
		});

		test('multiply()', function() {
		    strictEqual(multiply(2,2), '4', '2 multiplied by 2 is 4');
		    equal(multiply(2,3), 6, '3 multiplied by 2 is 6');
		});

 到此为止就可以运行了，结果是 2个通过，两个失败。

我们很容易的看出，测试就是调用了一个名字为test的函数，包含两个参数，第一个参数是对测试函数的描述，第二个参数是一个回调函数，里面的ok，strictEqual，equal 分别是对测试例子结果的断言，对于strictEqual，equal来说比如我要测试一个函数，第一个是被测试函数的调用的返回结果，第二个是期待的结果，第三个是对本次测试示例的描述，如果测试结果与自己想要的结果一致，则本次单元测试通过，否则 。。。。。对于ok断言主要是判断被测试的函数返回值是否为真。如果为真则通过。否则。。。。
值得要说一下的是，strictEqual 是判断严格相等相当于`===`比如4===4返回true，4=='4'则返回false，equal是只要可以转为相等的结果则通过相当于`==`，会对操作数进行类型转换 比如4=='4' 返回true

除此之外还有其他的[断言函数](http://api.qunitjs.com/category/assert/)可用，这里就不介绍了。官网写的灰常清楚了

- `deepEqual()` ：深度相等判断，适用于原生类型，数组，对象 正则表达式，以及日期 函数类型
- `equal()` ：非严格相等，
- `notDeepEqual` ：与`deepEqual`功能相反
- `notPropEqual()` 严格的比较对象属性，不相等则通过。
- `notStrictEqual()` 严格的比较，不严格相等则返回通过
- `ok()`  第一个参数为true 则通过测试。
- `propEqual()` 使用=== 来比较对象内部的属性，与`deepEqual()` 不同的是`propEqual()`可以用来比较两个具有不同原型以及构造函数的对象。
- `strictEqual()` 同时比较值和类型 
- `throws()` 要求抛出异常。
 
下载本篇博客的[源码](https://github.com/kunkun12/Qunit-Demo)