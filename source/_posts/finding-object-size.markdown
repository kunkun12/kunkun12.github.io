---
layout: post
title: "ECMASCRIPT 5 Object.keys "
date: 2014-03-19 08:14:29 +0800
comments: true
categories: JavaScript
tag: JavaScript
---

####问题提出 我要获取一个对象包含属性的个数
在我们使用Javascript的过程中、我们经常把Object当做散列或者Map来使用。有时候我想知道一个对象当前存了多少东西，假设我要分析一下 图书馆里图书的数据 这里有个作者与对应图书的集合 ，这个对象包含了几项内容：如下

		var bookAuthors = {
		    "Farmer Giles of Ham": "J.R.R. Tolkien",
		    "Out of the Silent Planet": "C.S. Lewis",
		    "The Place of the Lion": "Charles Williams",
		    "Poetic Diction": "Owen Barfield"
		};

作为分析的一部分，我们像一个API发送图书作者的数据，不幸的是 这个API只能接受一个包含100本左右的图书对象，于是我们需要向一个号的解决方案，在向API发送之前想告诉它一共包含了多少本书。
####解决方案，使用for in 变量所有属性，hasOwnProperty判断是不是属于当前对象本身的
于是最初的设计可能如下

		function countProperties (obj) {
		    var count = 0;

		    for (var property in obj) {
		        if (Object.prototype.hasOwnProperty.call(obj, property)) {
		            count++;
		        }
		    }

		    return count;
		}
		var bookCount = countProperties(bookAuthors);
		// Outputs: 4
		console.log(bookCount);

之所以使用hasOwnProperty 是因为 for in 不但会对象本身的属性，同时也会遍历继承而来的属性 判断

####Object.keys
当然这种 方案确实是可行的，那么如果能避免手工的计算属性的个数岂不是会更好，Javascript能不能直接获取呢。在ES5 中Object 有了一个 keys 属性是专门获取 对象属性的name并且返回一个数组。这里只返回这个对象本身的属性 不包含继承过来 ，作用等同于

	  	for (var property in obj) {
			 if (Object.prototype.hasOwnProperty.call(obj, property)) {			          
			}
		}

如果要获取一个对象属性的个数 获取Keys之后，通过数组的length属性就可以获取了。值得一提的是Object.keys(obj) ,只能通过这种方式，他是Object对象中的方法（有点是静态的意思），并不是在Object.prototype上的 因此我们自己定义的对象时没有这个方法的，所以正确的使用方式如下

		var bookCount = Object.keys(bookAuthors).length;
		// Outputs: 4
		console.log(bookCount);

错误的使用方式

		var bookCount = bookAuthors.keys().length; //会报错的，自己声明的对象时不具有这个方法的


####其他-Object.getOwnPropertyNames

同时也是在ES5中 有个类似的方法 Object.defineProperties，两者也稍有区别

Object.keys 以及Object.getOwnPropertyNames()用来获取对象中的属性的name，不包括继承来的属性，两者区别是Object.keys 只能获取可遍历的属性的名字的数组。举个例子

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

####ployFill 为不支持ES5的浏览器提供支持 
以下摘自[es5-shim](https://github.com/kunkun12/es5-shim/blob/master/es5-shim.js)

		   var hasDontEnumBug = true,
		        hasProtoEnumBug = (function () {}).propertyIsEnumerable('prototype'),
		        dontEnums = [
		            "toString",
		            "toLocaleString",
		            "valueOf",
		            "hasOwnProperty",
		            "isPrototypeOf",
		            "propertyIsEnumerable",
		            "constructor"
		        ],
		        dontEnumsLength = dontEnums.length;
		    for (var key in {"toString": null}) {
		        hasDontEnumBug = false;
		    }
		    Object.keys = function keys(object) {
		        var isFunction = _toString(object) === '[object Function]',
		            isObject = object !== null && typeof object === 'object';

		        if (!isObject && !isFunction) {
		            throw new TypeError("Object.keys called on a non-object");
		        }

		        var keys = [],
		            skipProto = hasProtoEnumBug && isFunction;
		        for (var name in object) {
		            if (!(skipProto && name === 'prototype') && owns(object, name)) {
		                keys.push(name);
		            }
		        }
		        if (hasDontEnumBug) {
		            var ctor = object.constructor,
		                skipConstructor = ctor && ctor.prototype === object;
		            for (var i = 0; i < dontEnumsLength; i++) {
		                var dontEnum = dontEnums[i];
		                if (!(skipConstructor && dontEnum === 'constructor') && owns(object, dontEnum)) {
		                    keys.push(dontEnum);
		                }
		            }
		        }
		        return keys;
		    };

		}