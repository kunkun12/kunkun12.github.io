---
layout: post
title: "html 5 meta tag"
date: 2014-03-11 08:51:47 +0800
comments: true
categories: html5
tag: Html
---
### meta的作用
meta是用来在HTML文档中模拟HTTP协议的响应头报文。meta 标签位于网页的`<head>。。。</head>`中，meta 标签的用处很多。meta 的属性有两种：name和http-equiv。name属性主要用于描述网页，对应于content（网页内容），以便于搜索引擎机器人查找、分类,搜索引擎都使用网上机器人自动查找meta值来给网页分类）。这其中最重要的是description（站点摘要）和keywords（分类关键词），所以应该给每页加一个meta值，w3c如是说

- <meta> 元素可提供有关页面的元信息（meta-information），比如针对搜索引擎和更新频度的描述和关键词。
- <meta> 标签位于文档的头部，不包含任何内容。<meta> 标签的属性定义了与文档相关联的名称/值对。

每个meta标签必须具有content属性，定义与 http-equiv 或 name 属性相关的元信息。

### 可选属性
#### http-equiv 属性
http-equiv 属性为名称/值对提供了名称。并指示服务器在发送实际的文档之前先在要传送给浏览器的 MIME 文档头部包含名称/值对。
当服务器向浏览器发送文档时，会先发送许多名称/值对。虽然有些服务器会发送许多这种名称/值对，但是所有服务器都至少要发送一个：content-type:text/html。这将告诉浏览器准备接受一个 HTML 文档。
使用带有 http-equiv 属性的 <meta> 标签时，服务器将把名称/值对添加到发送给浏览器的内容头部。包括`content-type` ,`expires`,`refresh` ,`set-cookie`例如，添加：

		<meta http-equiv="charset" content="iso-8859-1">
		<meta http-equiv="expires" content="31 Dec 2008">

这样发送到浏览器的头部就应该包含：

		content-type: text/html
		charset:iso-8859-1
		expires:31 Dec 2008

5秒刷新一次页面

		<meta http-equiv="refresh" content="5" />

#### content 属性

- content 属性提供了名称/值对中的值。该值可以是任何有效的字符串。
- content 属性始终要和 name 属性或 http-equiv 属性一起使用。

#### name 属性

name 属性提供了名称/值对中的名称。HTML 和 XHTML 标签都没有指定任何预先定义的 <meta> 名称。通常情况下，您可以自由使用对自己和源文档的读者来说富有意义的名称。
"keywords" 是一个经常被用到的名称。它为文档定义了一组关键字。某些搜索引擎在遇到这些关键字时，会用这些关键字对文档进行分类。
类似这样的 meta 标签可能对于进入搜索引擎的索引有帮助：

		<meta name="keywords" content="HTML,ASP,PHP,SQL">
		<meta name="KEYWords" contect="">向搜索引擎说明你的网页的关键词； 
		<meta name="DEscription" contect="">告诉搜索引擎你的站点的主要内容； 
		<meta name="Author" contect="你的姓名">告诉搜索引擎你的站点的制作的作者； 
		<meta name="Robots" contect= "all|none|index|noindex|follow|nofollow"> 

其中的属性说明如下： 

- 设定为all：文件将被检索，且页面上的链接可以被查询； 
- 设定为none：文件将不被检索，且页面上的链接不可以被查询； 
- 设定为index：文件将被检索； 
- 设定为follow：页面上的链接可以被查询； 
- 设定为noindex：文件将不被检索，但页面上的链接可以被查询； 
- 设定为nofollow：文件将不被检索，页面上的链接可以被查询。 

另外一点需要注意的是在HTML4中定义页面的字符集需要，放在content-type对应的content里面

		<meta http-equiv="content-type" content="text/html; charset=ISO-8859-1">

HTML5中新增了 charset属性 ，使用更简单

		<meta charset="utf-8">
