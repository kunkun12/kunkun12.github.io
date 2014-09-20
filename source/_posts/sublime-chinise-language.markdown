---
layout: post
title: "sublime 输出对中文的支持"
date: 2014-01-26 21:27:15 +0800
comments: true
categories: Sublime
tag: Sublime
---
今天用sublime 写Python的Demo，当编辑代码完成之后按Ctrl +B，能完成Python的解释，并在控制台输出结果，但是如果输出内容中包含中文的话则会有错误发生，错误如下

``` [Decode error - output not utf-8] ```

大概猜出是编码的问题，于是百度了解决方案为，在Sublime插件包里修改 ` Python.sublime-build ` 这个文件在`C:\Users\kunkun\AppData\Roaming\Sublime Text 2\Packages\Python`下面.在最后一行里面添加·` "encoding": "cp936" `,原来的文件为：


		{
			"cmd": ["python", "-u", "$file"],
			"file_regex": "^[ ]*File \"(...*?)\", line ([0-9]*)",
			"selector": "source.python"
		}


修改后的文件
 
		{
		  "cmd": ["python", "-u", "$file"],
		  "file_regex": "^[ ]*File \"(...*?)\", line ([0-9]*)",
		   "selector": "source.python",
		   "encoding": "cp936"
		}


改完之后则可以正常输出中文了。


如果nodejs在控制台输出中文乱码的问题

修改Nodejs.sublime-build文件中的encoding为utf-8

