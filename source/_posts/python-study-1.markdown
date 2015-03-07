---
layout: post
title: "Python 学习笔记(1)"
date: 2014-02-8 21:56:04 +0800
comments: true
categories: Python
tag: Python
---

去年买了本Python基础教程，看了一个月，感觉Python 入门也挺easy的，也写了一些简单的代码，最近又买了本《Python入门教程》，感觉这个书不应该买的，就是太基础，书看完了,，将Python一些最基本的东西总结一下吧。将重点内容概括一下 作为读书笔记，好记性不如烂笔头啊。

####字符串

字符串有三种方式 

	1. 单引号
	2. 双引号
	3. 三引号

通过+ 完成字符串拼接
len (s) 求字符串的长度
3*"hello"复制三次字符串。

列出模块中的所有函数 dir(math)
打印文档字符串 print(math.cos.__doc__)

####类型转换
		int("3")
		str(3)
		float('3.0')
		float(3)

变量的引用赋值：让变量指向表达式的值 比如 x=2;x指向2，y=x;y也指向，改变y的值对x无影响，只是改变了y的指向而已

多种赋值 

		x,y,z=1,2,3  相当于x=1,y=2,z=3;

从 键盘读取字符串 name=input();只能获取到字符串 ，如果要读取整数的话，需要类型转换，int(a)

将字符串 首字母大写 
		'du bao kun'.capitalize(); //Du bao kun
去除收尾的空格  
		'  du  bao  kun   '.strip();// du bao kun

print 在字符串  可以传入多个参数，以分隔符来输出，可以设置sep来自定义分割符号,比如  print('du','bao','kun',sep='.')#du.bao.kun

ord 获取字符编码 比如 ord('a')#65
chr获取对应的字符 chr(97)#'a'

转义字符  \\单斜杠，\'单引号 、\"双引号、\n 回车，\r,换行\t 制表符

获取字符串的字符 通过  [index] 
字符串切片  比如 
		food='apple pie';food[0:5] #apple

 不包括索引5的字符,相当于food[:5]，如果起始索引忽略的话则相当于0，如果最后一个索引忽略，相当于 为字符串长度。

负数索引， 比如 'food[-9,-4]' #apple

字符串测试函数:'s.endwith' ,'s.startwith','s.isalnum'只包含字字母或者数字isalpha 
只包含字母，'isdecimal'只包含十进制的数字的字符
'isdigital'只包含数字，'isidentifier'只包含合法字符，'islower'只包含小写字母，'isnumric'只包含数字，'istitle'大小写符合头衔要求
'isupper'只包含大写字母、't in s s'包含字符串t
cout(t) 字符串中t出现的次数

搜索函数：find(t) 如果没有找到子串t则返回-1、 rfind(t)、index(t)、rindex(t)没有找到会爆出异常

要范文索引为i的字符 ，通过 's[i]'或者 's[i:i+1]'

同时还有改变大小写、剥除函数、拆分函数，替换函数

**format**	函数 一下输出结果均为Jack like rose

		print('{0} like {1} '.format('jack','rose'))
		print('{first} like {second} '.format(second='rose',first='jack'))

eval可以解析 表达式的值 比如 'eval('5+3')'结果为8
布尔逻辑 ：'not  and  or    ==、'
短路求值  如果 true or  x ，则表达式x值不再计算。同理如果 false and x，x表达是也不进行计算。
流程控制  if /else,if/elif/else

		if x:
		    print('x')
		else
		    print('y')
条件表达式

		reply=‘yuck’if food =='lamb' else 'yum'
相当于

		if food =='lamb'
		    reply='yuck'
		else
		    replay='yum'

循环  
		for i in range(10):
		      print (i)
while 

		i=0
		while i<10:
			print(i)
			i=i+1

python 使用缩进来表示代码块，要在Python中 表示代码块，必须以同样程度缩进代码块的每一行，但Python 允许你编写包含任意数量语句的代码块,但注意冒号

函数定义 

	    def say_hello():
	        print ('hello')
默认值

		def greet(name,greeting ='hello'):
		    print(name+greeting)
		greet('kunkun')
		greet('kunkun','你好')
输出结果 

	    kunkunhello
	    kunkun你好
关键字参数

		greet(greeting='nihao',name='kunkun')



