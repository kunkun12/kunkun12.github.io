---
layout: post
title: "underscore学习总结"
date: 2014-03-03 08:33:19 +0800
comments: true
categories: underscore
tag: JavaScript
---

### underscore介绍
[underscore](https://github.com/jashkenas/underscore/)是一个很小很强大的函数库，帮助程序猿不用扩展内置的JS对象即可实现一系列常用的功能。对于“如果我面前是一个空白的HTML 页面，我想想立刻开发出网页，我需要做什么”这样的问题，underscore给出了答案。

underscore 提供了80多个 工具函数，来提供了常见的功能。比如`map` ,`select`,`invoke`,以及更专业的助手	函数绑定，javascript模板，深度相等测试。如果当今流行的浏览器中如果已经存内置方法，他会优先调用浏览器原生的方法比如ES5中新增的`forEach`，`map`，`reduce`，`filter`，`every`，`some`, `indexOf` 等。

underscore的函数主要包含了**Collections**,**Arrays**,**Functions**,**Object**,**Utility**,**Chaining** 等6块内容。对开发者来说是个很强的轮子。

### underscore如何使用
相关文档

[帮助文档](http://underscorejs.org/)

[带注释的源码](http://underscorejs.org/docs/underscore.html)

[Github](https://github.com/jashkenas/underscore);

获取方法：

[开发环境版本](http://underscorejs.org/underscore.js) 未压缩

[生产环境版本](http://underscorejs.org/underscore-min.js) 经过压缩混淆

也可以使用工具安装的方式来获取

- Node.js npm install underscore
- Require.js require(["underscore"], ...
- Bower bowser install underscore
- Component component install jashkenas/underscore


### 使用Demo

之前使用过Jquery的时候我们学会了用$,使用underscore也同样的容易，只需要一个下划线_即可，
比如

	_.each([1, 2, 3], alert);
	=> alerts each number in turn...
	_.each({one: 1, two: 2, three: 3}, alert);
	=> alerts each number value in turn...

看到函数名字基本上就能猜到功能了，为了方便全面了解功能，这里仅列出函数的名字，仅对不熟悉的做一下解释，个别函数值列出了源码，其实看源码能学到更多。
#### Collections(数组，对象以及类数组可以调用的方法)

- `each`
- `map`
- `reduce`
- `reduceright`
- `find`
- `filter` 
- `every`
- `some` 以上几个都是ES5 中的东东 就不在介绍了
- `where`:选出所有的符合条件的
- `findWhere`:只返回第一个符合条件的元素
- `reject` 筛选出不符合条件的
- `contains`
- `invoke`依次传入集合中的方法，来调用一个函数
- `pluck` 提取某个字段的集合
- `max`	根据集合中的某一属性或者对其操作的结果，提取结果最大的元素。如果第二个参数省略，则根据数组中值得大小进行比较。比如

		var stooges = [{name: 'moe', age: 40}, {name: 'larry', age: 50}, {name: 'curly', age: 60}];
		_.max(stooges, function(stooge){ return stooge.age; });
		=> {name: 'curly', age: 60};

- `min` 根据集合中的某一属性或者对其操作的结果，提取结果最小的元素。
- `sortBy`
- `groupBy`
- `countBy`
- `indexBy` 给集合添加索引
- `shuffle`打乱集合元素的顺序
- `sample`从数组中随机子数组
- `toArray`。将集合转化为数组

####Array Functions

- `first`
- `initial` 返回前几个元素比如 

		`_.initial([5, 4, 3, 2, 1]); => [5, 4, 3, 2]`

- `last`
- `rest`
- `compact` 返回一个数组的拷贝，其中不包括值为false的元素 比如

		_.compact([0, 1, false, 2, '', 3]);
		=> [1, 2, 3]

- `flatten`，把嵌套数组中的元素提取出来做为数组的元素。如果第二个参数为true 仅进行浅提取。比如

		_.flatten([1, [2], [3, [[4]]]]);
		=> [1, 2, 3, 4];

		_.flatten([1, [2], [3, [[4]]]], true);
		=> [1, 2, 3, [[4]]];

- `without`
- `partition` 将一个数组分组，其中的元素第一个满足第二个参数，第二个是不满足的
- `union` 求数组的并集
- `intersection`  交集
- `difference` 差集
- `uniq` 去掉数组中重复的元素
- `zip` 看Demo

		_.zip(['moe', 'larry', 'curly'], [30, 40, 50], [true, false, false]);
		=> [["moe", 30, true], ["larry", 40, false], ["curly", 50, false]]

- `object` 把数组转为对象。

		_.object(['moe', 'larry', 'curly'], [30, 40, 50]);
		=> {moe: 30, larry: 40, curly: 50}

		_.object([['moe', 30], ['larry', 40], ['curly', 50]]);
		=> {moe: 30, larry: 40, curly: 50}

- `indexOf`
- `lastIndexOf
- `sortedIndex`
- `range` 产生一个连续的数组

#### Functions

- `bind` 将函数绑定到具体的对象，并且可以绑定参数

		var func = function(greeting){ return greeting + ': ' + this.name };
		func = _.bind(func, {name: 'moe'}, 'hi');
		func();
		=> 'hi: moe'
- `bindAll` 同时绑定对象的方法，只需要指定相应的方法即可

		var buttonView = {
		  label  : 'underscore',
		  onClick: function(){ alert('clicked: ' + this.label); },
		  onHover: function(){ console.log('hovering: ' + this.label); }
		};
		_.bindAll(buttonView, 'onClick', 'onHover');
		// When the button is clicked, this.label will have the correct value.
		jQuery('#underscore_button').bind('click', buttonView.onClick);

- `partial` 绑定部分的参数。返回一个函数

		var add = function(a, b) { return a + b; };
		add5 = _.partial(add, 5);
		add5(10);
		=> 15

- `memioze` 缓存计算结果。
- `delay` 延迟执行，类似于setTimeOut
- `defer`也是延迟执行方法，不同的是他能保证在当前堆栈中的所有的代码跑完之后再执行function。其实就是setTimeout(fn,0);
- `throttle` throttle这个单词的意思是使减速，用于控制频繁触发的 function的的频率，比如，拖动页面滚动条时scroll方法会以很高的频率触发，如果在scroll的处理事件中做了很费时的操作，会导致浏览器假死，如果使用了throttle后，function被触发的频率可以降低。

		var throttled = _.throttle(updatePosition, 100);
		$(window).scroll(throttled);

- `debounce` 对于频繁处理的时间，只在第一次触发(是否触发取决于immdiate 参数)，和事件频繁触发最后一次触发(有最多wait的延时)。拿滚动事件为例，滚动事件50ms触发一次，如果设置wait为100ms。则在最后一次触发scroll事件时，也就是停止滚动时，在100ms后触发function。如果immediate参数为true，开始滚动时也会触发function

	var lazyLayout = _.debounce(calculateLayout, 300);
	$(window).resize(lazyLayout);
- `once` once能确保func只调用一次，如果用func返回一个什么对象，源码也比较简单，无非就是用一个标志位来标示是否运行过，缓存返回值
- `after` 创建一个新的函数，当func反复调用时，count次才调用一次，比如：

	var afterA = _.after(3,a);
	afterA();//调用
	afterA();//不alert
	afterA();//不alert
	afterA();//调用

- `now` 返回当前的时间戳  _.now = Date.now || function() { return new Date().getTime(); };
- `wrap`
- `compose`

####Objects

- `keys` 返回 对象的key的数组

	_.keys({one: 1, two: 2, three: 3});
	=> ["one", "two", "three"]

- `values` 返回对象的value数组

	_.values({one: 1, two: 2, three: 3});
	=> [1, 2, 3]

- `pairs` 转为[key,valu]的集合

	_.values({one: 1, two: 2, three: 3});
	=> [1, 2, 3]

- `invert` 将 key和value反转
- `functions`返回对象中方法的集合
- `extend` 扩展对象的属性。

	_.extend({name: 'moe'}, {age: 50});
	=> {name: 'moe', age: 50}

- `pick` 筛选字段，只选满足传入的key的字段

	_.pick({name: 'moe', age: 50, userid: 'moe1'}, 'name', 'age');
	=> {name: 'moe', age: 50}

- `omit` 与上面相反
- `defaults`
- `clone`
- `tap` 对象的拦截器，用于在对象方法链中，调试对象 
- `has`
- `matches`
- `property`
 
	var moe = {name: 'moe'};
	'moe' === _.property('name')(moe);
	=> true

- `isEqual` 深度比较是否相等

		var moe   = {name: 'moe', luckyNumbers: [13, 27, 34]};
		var clone = {name: 'moe', luckyNumbers: [13, 27, 34]};
		moe == clone;
		=> false
		_.isEqual(moe, clone);
		=> true

- `isEmpty` 判断对象是否为{}

		_.isEmpty([1, 2, 3]);
		=> false
		_.isEmpty({});
		=> true

- `isElement` 判断是否为dom 元素

	 _.isElement = function(obj) {
	    return !!(obj && obj.nodeType === 1);
	  };

- `isArray`
- `isObject`
- `isArguments`
- `isFunction`
- `isString`
- `isNumber`
- `isDate`
- `isRegExp`
 以上就不解释了，看源码吧 

		   each(['Arguments', 'Function', 'String', 'Number', 'Date', 'RegExp'], function(name) {
		    _['is' + name] = function(obj) {
		      return toString.call(obj) == '[object ' + name + ']';
		    };
		  });
- `isFinite` 是否为有限数

	  _.isFinite = function(obj) {
	    return isFinite(obj) && !isNaN(parseFloat(obj));
	  };

- `isBoolean` 是否为bool值

	  _.isBoolean = function(obj) {
	    return obj === true || obj === false || toString.call(obj) == '[object Boolean]';
	  };

- `isNaN`  注意typeof(NaA)为number

		  _.isNaN = function(obj) {
		    return _.isNumber(obj) && obj != +obj;
		  };

- `isNull` 

		_.isNull = function(obj) {
		    return obj === null;
		  };

- `isUndefined`

		 _.isUndefined = function(obj) {
		    return obj === void 0;
		  };


####UTILITY FUNCTIONS

- `noConflict` 这个 与jQuery中的类似，将_ 还原为之前代表的意思，以后使用

	var underscore = _.noConflict();

		  _.noConflict = function() {
		    root._ = previousUnderscore;
		    return this;
		  };

- `identify` 返回用作参数的相同值。数学 f(x) = x、这个函数看着没有什么用途，但使用整个Underscore作为默认的迭代器

		 _.identity = function(value) {
		    return value;
		  };

- `constant`。创建一个返回与传入参数相同值得函数

		_.constant = function(value) {
		    return function () {
		      return value;
		    };
		  };

- `times` 调用给定的迭代函数n次 

			  _.times = function(n, iterator, context) {
			    var accum = Array(Math.max(0, n));
			    for (var i = 0; i < n; i++) accum[i] = iterator.call(context, i);
			    return accum;
			  };
			 _(3).times(function(n){ dosomething });


- `radom` 返回随机数
	
		  _.random = function(min, max) {
		    if (max == null) {
		      max = min;
		      min = 0;
		    }
		    return min + Math.floor(Math.random() * (max - min + 1));
		  };

- `mixin` 扩展underscore中的方法

		_.mixin({
		  capitalize: function(string) {
		    return string.charAt(0).toUpperCase() + string.substring(1).toLowerCase();
		  }
		});
		_("fabio").capitalize();
		=> "Fabio"

- `uniqueId` 为一个对象生成一个唯一的ID 可以自己加前缀
- `escape`转义HTML字符串，替换&, <, >, ", ',  /字符 替换为&amp;, &lt;, &gt;, &quot;, &#x27

		_.escape('Curly, Larry & Moe'); 
		=> "Curly, Larry &amp; Moe" 

- `unescape` 与上相反 替换 &amp;, &lt;, &gt;, &quot;, &#x27; 替换为&, <, >, ", ',  /
- `result` _.result(object, property)  如果第二个参数是函数的话则调用这个函数，并将第一个参数作为函数的上下文，否则返回object[property]
- `template` javascript模板编译函数使用<%= … %> 替换变量，<% … %>执行JS代码， <%- … %>让HTML被转义。

####Chaining
可以根据自己的喜好决定是函数式编程还是采用面向对象的方式来使用underscore，比如下的两种方式

		_.map([1, 2, 3], function(n){ return n * 2; });
		_([1, 2, 3]).map(function(n){ return n * 2; });


`value`  提取出_(obj)对象的值。比如

		_([1, 2, 3]).value();
		=> [1, 2, 3]

####OOP
 如果_作为函数被调用的话，会返回一个包装的好的对象，这个对象拥有当前版本Underscore的所有方法。同时这个对象还可以通过调用chain实现对一个对象的包裹

链式调用

				var lyrics = [
				  {line: 1, words: "I'm a lumberjack and I'm okay"},
				  {line: 2, words: "I sleep all night and I work all day"},
				  {line: 3, words: "He's a lumberjack and he's okay"},
				  {line: 4, words: "He sleeps all night and he works all day"}
				];

				_.chain(lyrics)
				  .map(function(line) { return line.words.split(' '); })
				  .flatten()
				  .reduce(function(counts, word) {
				    counts[word] = (counts[word] || 0) + 1;
				    return counts;
				  }, {})
				  .value();
				=> {lumberjack: 2, all: 4, night: 2 ... }

chain 的源码 

		_.chain = function(obj) {
		    return _(obj).chain();
		  };

同时下面还为_原型增加了方法

		  _.extend(_.prototype, {
		    chain: function() {
		      this._chain = true;
		      return this;
		    },
		    value: function() {
		      return this._wrapped;
		    }
		  });

上面的例子中开始调用_.chain(lyrics),是调用的 _.chain(obj)。然后经过_(obj) 返回一个示例。接下来调用的是原型上的方法chain 将 this._chain = true; 然后返回this 这样就可以进行链式调用的，第一次调用的_对象的chain，第二次调用的是_函数的原型对象的方法。接下来是一句神奇的代码

		_.mixin(_);

然后调用了 _mixin为把_中的所有方法都赋给了_.prototype.于是经过包装的对象都是经过 new _(obj)的，具有了_对象的中的方法

_mixin的源码 ，将传入对象的方法都添加到_.prototype上

		  _.mixin = function(obj) {
		    each(_.functions(obj), function(name) {
		      var func = _[name] = obj[name];
		      _.prototype[name] = function() {
		        var args = [this._wrapped];
		        push.apply(args, arguments);
		        return result.call(this, func.apply(_, args));
		      };
		    });
		  };

看看执行_()发生了什么,回到源码开头

		 var _ = function(obj) {
		    if (obj instanceof _) return obj; //如果传入的对象是 _的实例
		    if (!(this instanceof _)) return new _(obj); // 如果不是通过new _()的，则new new _(obj) 一个实例继续调用
		    this._wrapped = obj;// 传入其他参数 new 实例的时候传入的参数
		  };


这是一个单例模式，通过创建一个underscore对象，如果以包裹的形式来调用的话。会将包裹的对象设置为内部的_wrapped变量。
