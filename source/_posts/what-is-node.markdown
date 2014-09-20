---
layout: post
title: "Node.JS 究竟是什么?"
date: 2014-03-01 17:32:05 +0800
comments: true
categories: node
tag: node
---

###Node旨在解决服务器能够处理的并发连接的最大数量的问题。
  Node官网首页有一句话就是对Node是什么以及node解决什么问题做出了介绍，原文如下

	Node.js is a platform built on Chrome's JavaScript runtime for easily building fast, 
	scalable network applications. Node.js uses an event-driven, 
	non-blocking I/O model that makes it lightweight and efficient,
    perfect for data-intensive real-time applications that run across distributed devices.

 大概意思是说Node是基于Chrome的javascript的引擎，旨在提供一种简单的构建可伸缩网络程序的方法，node特点是基于事件驱动，非阻塞I/O，同时还有一个特点没有直接提到就是单线程。

在 Java™ 和 PHP 这类语言中，每个连接都会生成一个新线程，每个新线程可能需要 2 MB 的配套内存。在一个拥有 8 GB RAM 的系统上，理论上最大的并发连接数量是 4,000 个用户。随着您的客户群的增长，如果希望您的 Web 应用程序支持更多用户，那么，您必须添加更多服务器。当然，这会增加服务器成本、流量成本和人工成本等成本。除这些成本上升外，还有一个潜在技术问题，即用户可能针对每个请求使用不同的服务器，因此，任何共享资源都必须在所有服务器之间共享。鉴于上述所有原因，整个 Web 应用
Node 解决这个问题的方法是：更改连接到服务器的方式。每个连接发射一个在 Node 引擎的进程中运行的事件，而不是为每个连接生成一个新的 OS 线程（并为其分配一些配套内存）。Node 声称它绝不会死锁，因为它根本不允许使用锁，它不会直接阻塞 I/O 调用。Node 还宣称，运行它的服务器能支持数万个并发连接。

###为什么选择是Javascript 语言

* V8 JavaScript 引擎是 Google 用于其 Chrome 浏览器的底层 JavaScript 引擎。很少有人考虑 JavaScript 在客户机上实际做了些什么？实际上，JavaScript 引擎负责解释并执行代码。Google 使用 V8 创建了一个用 C++ 编写的超快解释器，该解释器拥有另一个独特特征；可以下载该引擎并将其嵌入任何 应用程序。
* JavaScript 是一种很棒的事件驱动编程语言，因为它允许使用匿名函数和闭包，更重要的是，任何写过代码的人都熟悉它的语法。事件发生时调用的回调函数可以在捕获事件处进行编写。这样可以使代码容易编写和维护，没有复杂的面向对象框架，没有接口，没有过度设计的可能性。只需监听事件，编写一个回调函数，其他事情都可以交给系统处理！
由于V8正好可以满足上述提到Node几个特点，以及Javascript独特优势，以及Javascript的大量用户，所以NodeJS就这样地产生了。

###Node适合做什么工作。实时 高并发I/O，弱密集逻辑计算

* RESTful API
* 新浪微博、twiter这样高并发的应用
* 编写网络小工具。
* 聊天程序
* 游戏后台
* 股票等实时信息展示网站。

同时Node有数万的计的第三方模块，Node 的一个特性是 Node Package Module，这是一个内置功能，用于安装和管理 Node 模块。它自动处理依赖项，因此您可以确定：您想要安装的任何模块都将正确安装并包含必要的依赖项。它还支持将您自己的模块发布到 Node 社区，假如您选择加入社区并编写自己的模块的话。您可以将 NPM 视为一种允许轻松扩展 Node 功能的方法，不必担心这会破坏您的 Node 安装。同样，如果您选择深入学习 Node，那么 NPM 将是您的 Node 解决方案的一个重要组成部分。

优秀的设计+众多开发者的支持，应该是Node平台火起来的原因吧。