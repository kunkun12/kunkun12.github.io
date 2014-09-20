---
layout: post
title: "javascript replace"
date: 2014-02-9 09:31:13 +0800
comments: true
categories:	javascript replace
tag: javascript
---
###简单介绍
`StringObject.replace(searchValue,replaceValue)`
StringObject:字符串
searchValue：字符串或正则表达式
replaceValue:字符串或者函数
###字符串替换字符串
`'I am loser!'.replace('loser','hero')//I am hero!`

直接使用字符串能让自己从loser变成hero，但是如果有2个loser就不能一起变成hero了。

`'I am loser,You are loser'.replace('loser','hero');//I am hero,You are loser `

正则表达式替换为字符串、并将正则的global属性改为true则可以让所有loser都变为hero

` 'I am loser,You are loser'.replace(/loser/g,'hero')//I am hero,You are hero `
###有趣的替换字符
####使用$&字符给匹配字符加大括号
		var sStr='讨论一下正则表达式中的replace的用法';
		sStr.replace(/正则表达式/,'{$&}');//讨论一下{正则表达式}中的replace的用法
####使用$`和$'字符替换内容,$`位于匹配子串左侧的所有文本。$'位于匹配子串右侧的所有文本。
		'abc'.replace(/b/,"$`");//aac
		'abc'.replace(/b/,"$'");//acc
####使用分组匹配组合新的字符串
		'nimojs@126.com'.replace(/(.+)(@)(.*)/,"$2$1")//@nimojs
####replaceValue参数可以是一个函数
`StringObject.replace(searchValue,replaceValue)`中的**replaceValue**可以是一个函数.如果replaceValue是一个函数的话那么，这个函数的arguments会有n+3个参数（n为正则匹配到的次数)
		
		function logArguments(){    
    	console.log(arguments);//["nimojs@126.com", "nimojs", "@", "126.com", 0, "nimojs@126.com"] 
   		 return '返回值会替换掉匹配到的目标'
		}
		console.log(
	    'nimojs@126.com'.replace(/(.+)(@)(.*)/,logArguments)
		)
**参数分别为**

1. 匹配到的字符串（此例为nimojs@126.com,推荐修改上面代码的正则来查看匹配到的字符帮助理解)
2. 如果正则使用了分组匹配就为多个否则无此参数。（此例的参数就分别为"nimojs", "@", "126.com"。推荐修改正则为/nimo/查看控制台中返回的arguments值）
3. 匹配字符串的对应索引位置（此例为0）
4. 原始字符串