---
layout: post
title: "ECMAScript 5 学习总结"
date: 2014-03-05 08:31:52 +0800
comments: true
categories: ES5
tag: javascript
---

话说[ES6](https://github.com/lukehoban/es6features)的草稿都出来了，貌似好多人对ES5 还没有全面的了解，下面将我学习的关于ES5的一些东西总结一下。
ECMAScript 5.1(或者成为ES5 、ECMAScript 5)，在2009年发布草稿，2011发布正式版本，最初在2011年发布的Firefox 4上得到支持。但是万恶的IE（6、7、8） 并不支持ES5，因此这也一定程度上阻止了ES5的普及。但是我们可以通过ployfill的形式来让IE8支持ES5的一些功能，比如引入[shim](https://github.com/es-shims/es5-shim);

###浏览器对ES5的支持情况。
ES5出现以前，用的最多的是ES3(ES4 流产了). 总体上当今主流的浏览器都已经支持了ES5.比如常见的ie9+，chrome 19+，safari5+，firefox4+，opera 12+，android 3+，Blackberry Browser 7+，IOS safari 5+，opera mobile 12+，ie mobile 10+,例外是IE 9不支持ES 5.1的严格模式，IE10 已经支持。Safari 5.1+在严格模式下不支持0宽字符，比如`var $\u200c = 'a'` 或者`{_\u200d: 'vuur'}`.IE8 是ES3的脚本引擎，但是支持ES5的一些特性，比如`JSON`、`Object.defineProperty`,`Object.getOwnPropertyDescriptor`,然而 我们最好还是要把IE8 称为ES3的浏览器。当然如果对于不支持ES 5的浏览器可以使用[shim](https://github.com/es-shims/es5-shim) 来进行扩展，以提供支持。下面开始描述ES 5的那些东西。

####数组或者是对象中的最后元素允许添加逗号。

	var myObject = {
	    foo:'foo',
	    bar:'bar', //no error
	};

	var myArray = [1,2,3,];
	               
	console.log(myArray.length) //logs 3, regardless of trailing comma 

####使用\ 分割来实现多行字符串

		//no error in ES5 scripting engines
		var stringLiteral = "Lorem ipsum dolor sit amet, consectetur \ 
		elit, sed do eiusmod tempor incididunt ut labore et dolore \
		magna aliqua. Ut enim ad minim veniam, quis nostrud \
		exercitation ullamco laboris nisi ut aliquip ex ea";

####对象属性的名字允许使用限定字符。

		var myObject = {function:'function',new:'new'};

		console.log(myObject.function,myObject.new); //logs 'function new'

####处理JSON 的方法 `JSON.parse`  `JSON.stringify`

####原生的字符串去空格方法`string.trim()`
ES 5 提供String.trim()来去除字符串首尾的空格。

 		'   foo   '.trim(); // 'foo'
需要关注的是，有些浏览器已经提供了 `trimRight()`,`trimLeft()`,但是这并不是正式的标准。

#### 通过[i]来访问字符串中的字符。

	'ES5'[1]; //  "S"

ES3中 是不允许这种方式来进行访问的

#### `undefined`,`NaN` 以及`Infinty`是只读的常量。
在ES5 之前的版本中这三个变量是可读写的。于是可能给undefined赋值，这样undefined，就不是真的undefined了。
#### 数组相关
判断数组类型的最佳方法:` Array.isArray([])`
####Array.prototype 新增的方法。
- Array.prototype.every()
- Array.prototype.filter()
- Array.prototype.forEach()
- Array.prototype.indexOf()
- Array.prototype.lastIndexOf()
- Array.prototype.map()
- Array.prototype.reduce()
- Array.prototype.some()

同时也可以在字符串以及类数组中使用这些方法，比如

		//在字符串中使用
		[].forEach.call('ABC',console.log);

		//argument  
		var myFunction = function(a,b,c){
		    [].forEach.call(arguments,console.log);
		};
		myFunction(1,2,3);

####为数组的 `forEach()`, `every()`, `some()`, `filter()`, `map()`中的回调函数指定this(上下文);这几个函数的第三个参数即callback中的this对象，下面代码中我设置了一个 `arrayLikeObject` 对象，作为forEach Callback的this。

	var arrayLikeObject = {0:'f',1:'o',2:'g',length:3};

	[].forEach.call(arrayLikeObject,function(item){
	    
	    // this = arrayLikeObject
	    console.log(item + ' total: ' + this.length);
	    
	},arrayLikeObject); //注意第三个参数 传递了也是arrayLikeObject


#### 属性访问器 getter setter

		var myObject = {
		    //name:'', WOULD CAUSE ERROR
		    get name() { //notice use of get to indicate accessor property 
		        console.log('read name');
		    },
		    set name(value) { //notice use of set to indicate accessor property
		        console.log('wrote to name the value: ' + value);
		    }

		};

		myObject.name = 'codylindley'; //调用 set 
		myObject.name; //调用get

####对象属性的配置项
ES5中新的特性让对象的属性具有可只读readable，可只写writable、可配置性configurable,可遍历enumerable，通过`Object.getOwnPropertyDescriptor()` 可以查看特性的描述信息。

		var accessor = {
		    get name(){},
		    set name(){}
		};

		var data = {name:'Cody Lindley'};

		//logs {get: function, set: function, enumerable: true, configurable: true}
		console.log(Object.getOwnPropertyDescriptor(accessor,'name'));
		    
		//logs {value: "Cody Lindley", writable: true, enumerable: true, configurable: true}
		console.log(Object.getOwnPropertyDescriptor(data,'name'));

默认情况下Object.defineProperty() Object.defineProperties()`writable: true, enumerable: true, configurable: true。`如果是通过属性访问器来定义的属性 默认特性为 `enumerable: true, configurable:true` 。

####新的 定义/修改对象的语法
通过`Object.defineProperty()` 以及 `Object.defineProperties()`两个函数来定义对象的属性以及属性的特性。下面是通过`Object.defineProperty()`为data对象定义两个属性`age`和`name`.

		var data = {}; //create data object
		Object.defineProperty( //在data 对象上定义属性age
		    data,
		    "age", 
		    {
		        value: 38,//配置属性的特性 包括value writable enumerable configurable
		        writable: true,
		        enumerable: true,
		        configurable: true
		    }
		);

		var accessor = {}; 
		Object.defineProperty( //以get set的形式来定义属性 name
		    accessor,
		    "name", 
		    {
		        get: function () {},
		        set: function (value){},
		        enumerable: true,
		        configurable: true
		    }
		);

		//{value: 38, writable: true, enumerable: true, configurable: true} 
		console.log(Object.getOwnPropertyDescriptor(data,'age'));

		//{get: function, set: function, enumerable: true, configurable: true}
		console.log(Object.getOwnPropertyDescriptor(accessor,'name'));

与上不同的是`Object.defineProperties()` 可以同时定义多组属性

		var cody = {}; //
		Object.defineProperties( //定义两个属性
		    cody, 
		    {
		        "age": {
		            value: 38,
		            writable: true,
		            enumerable: true,
		            configurable: true
		        },
		        "name": {
		            get: function(){},
		            set:function(value){},
		            enumerable: true,
		            configurable: true
		        }
		    }
		);

		//{value: 38, writable: true, enumerable: true, configurable: true} 
		console.log(Object.getOwnPropertyDescriptor(cody,'age'));

		// {get: function, set: function, enumerable: true, configurable: true}
		console.log(Object.getOwnPropertyDescriptor(cody,'name'));
注意 通过`Object.defineProperty()` 以及`Object.defineProperties()`定义的属性默认配置为`writable: false, enumerable: false, configurable: false`.定义的访问器属性默认配置`enumerable: false, configurable:false`

####使用`Object.keys` 以及`Object.getOwnPropertyNames()`获取对象的属性的name。
`Object.keys` 以及`Object.getOwnPropertyNames()`用来获取对象中的属性的name，不包括继承来的属性，两者区别是`Object.keys` 只能获取可遍历的属性的名字的数组，`Object.getOwnPropertyNames() `可以获得所有的属性的名字，作为字符串数组返回。

		var person = {};
		Object.defineProperties(person, 
		    {
		        "name": {value: 'Cody Lindley',enumerable: true},
		        "age": {value: 38,enumerable: false}
		    }
		);

		//只返回可以可遍历的属性 
		console.log(Object.keys(person)); //logs ["name"]

		//return array of all properties, including ones that are non-enumerable
		console.log(Object.getOwnPropertyNames(person)); //returns ["name", "age"]

#### 属性保护 extending、 sealing、freezing。
在ES5中 对象可以被设置为non-extensible, sealed, 以及 frozen。这是三种不同的属性保护级别，可以通过`Object.preventExtensions(myObject)`, `Object.seal(myObject)`,  `Object.freeze(myObject)`来进行设置。

- Object.preventExtensions() 让对象无法新增属性。
- Object.seal()包括了Object.preventExtensions()的功能，同时还具有让已经存在的属性不可修改配置也就说不能使用Object.defineProperty以及Object.defineProperties 来修改属性的配置，但是修改属性的值依然是可以修改的。
- Object.freeze() 包括了Object.preventExtensions(),Object.seal()的功能，同时为对象的所有数据属性将 writable 特性设置为 false。 当 writable 为 false 时，无法更改数据属性值。（如果 属性值是对象的话，还是可以改变的）

					var  p={
								user:"kunkun"
							}
					Object.seal(p);
					p.name='du';
					console.log(p.name);//undefined 并没有增加
					Object.freeze(p);
					p.user='baokun'
					console.log(p.user);// 输出kunkun 还是原来的值发现并没有被修改

同时也提供三个函数来对这三种保护级别进行判断

- Object.isExtensible()
- Object.isSealed()
- Object.isFrozen()

####使用Object.create()显式继承。
在ES3中创建对象的方式主要有以下三种

- var objectViaObjectLiteral = {};
- var objectViaObjectConstructor = new Object();
- var objectViaCustomConstructor = new(function Person(){});

ES5中新增了Object.create函数来让开发者现实地设置对象以及对象继承的原型。第一个参数为对象的原型
	
	var object = Object.create(Object.prototype); //创建对象 与{} 、 new Object()功能相同。

object.create的主要作用继承指定的对象来创建新的对象，如果不传递参数，则默认值为Object.prototype。如果传递null 表示对象将会不继承于任何东西，则创建一个真正的空对象

		var person = {alive: true};
		var cody = Object.create(person, {
		    name: {
		        configurable: true,
		        enumerable: true,
		        writeable: true,
		        value: 'cody'
		    }
		});

		console.log(cody.alive); // true

		//
		console.log(Object.getPrototypeOf(cody).alive); //logs true

上面的例子中，可以看到create也传递了第二个参数来为对象增加属性,第二个参数与Object.defineProperties()结果是一样的。

####使用getPrototypeOf() 来获取对象的原型。
ES3中有个非标准的__proto__ 来获取对象的原型金，浏览器支持不兼容，ES5中有了标准化。

		Object.getPrototypeOf(new Object('foo')) === String.prototype // true

####bind ：为函数绑定上下文。

在ES3中通过Apply或者call来实现 比如

		Function.prototype.bind = function(obj) { 
		   var method = this, 
		   temp = function() { 
		     return method.apply(obj,arguments); 
		   }; 
		  
		   return temp; 
		} 

ES 5中有了正式的bind

		var myObject = {foo:'foo'};
		var myFunction = function(){
		    console.log(this,arguments[0],arguments[1],arguments[2]);
		};
		var usingBind = myFunction.bind(myObject,1,2,3); 
		usingBind(); // {foo="foo"} 1 2 3

#### use strict 严格模式

JavaScript代码标志为“严格模式”，则其中运行的所有代码都必然是严格模式下的。
其一：如果在语法检测时发现语法问题，则整个代码块失效，并导致一个语法异常。
其二：如果在运行期出现了违反严格模式的代码，则抛出执行异常。

- 主要规则 
- 定义变量必须带var 、
- 不能在JSON定义重复的key、
- 函数不能有重复的参数、
- 不能改变arguments的值