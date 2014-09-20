---
layout: post
title: "跨域资源共享的10种方式[转]"
date: 2014-01-27 11:01:13 +0800
comments: true
categories: 跨域
tag: 跨域
---
在客户端编程语言中，如JavaScript和ActionScript，同源策略是一个很重要的安全理念，它在保证数据的安全性方面有着重要的意义。同 源策略规定跨域之间的脚本是隔离的，一个域的脚本不能访问和操作另外一个域的绝大部分属性和方法。那么什么叫相同域，什么叫不同的域呢？

###同源策略

在客户端编程语言中，如JavaScript和ActionScript，同源策略是一个很重要的安全理念，它在保证数据的安全性方面有着重要的意义。同源策略规定跨域之间的脚本是隔离的，一个域的脚本不能访问和操作另外一个域的绝大部分属性和方法

那么什么叫相同域，什么叫不同的域呢？当两个域具有相同的协议(如http), 相同的端口(如80)，相同的host（如www.example.org)，那么我们就可以认为它们是相同的域。比如
` http://www.example.org/ `和` http://www.example.org/sub/ `是同域，而`http://www.example.org`, `https://www.example.org`, `http://www.example.org:8080`, `http://sub.example.org`中的任何两个都将构成跨域。同源策略还应该对一些特殊情况做处理，比如限制file协议下脚本的访问权限。本地的HTML文件在浏览器中是通过file协议打开的，如果脚本能通过file协议访问到硬盘上其它任意文件，就会出现安全隐患，目前IE8还有这样的隐患。

受到同源策略的影响，跨域资源共享就会受到制约。但是随着人们的实践和浏览器的进步，目前在跨域请求的技巧上，有很多宝贵经验的沉淀和积累。这里我把跨域资源共享分成两种，一种是单向的数据请求，还有一种是双向的消息通信。接下来我将罗列出常见的一些跨域方式，以下跨域实例的源代码可以从这里获得。

###单向跨域

####JSONP

JSONP (JSON with Padding)是一个简单高效的跨域方式，HTML中的script标签可以加载并执行其他域的JavaScript，于是我们可以通过script标记来动态加载其他域的资源。例如我要从域A的页面pageA加载域B的数据，那么在域B的页面pageB中我以JavaScript的形式声明pageA需要的数据，然后在pageA中用script标签把pageB加载进来，那么pageB中的脚本就会得以执行。JSONP在此基础上加入了回调函数，pageB加载完之后会执行pageA中定义的函数，所需要的数据会以参数的形式传递给该函数。JSONP易于实现，但是也会存在一些安全隐患，如果第三方的脚本随意地执行，那么它就可以篡改页面内容，截获敏感数据。但是在受信任的双方传递数据，JSONP是非常合适的选择。

####Flash URLLoader

Flash有自己的一套安全策略，服务器可以通过crossdomain.xml文件来声明能被哪些域的SWF文件访问，SWF也可以通过API来确定自身能被哪些域的SWF加载。当跨域访问资源时，例如从域www.a.com请求域www.b.com上的数据，我们可以借助Flash来发送HTTP请求。首先，修改域www.b.com上的crossdomain.xml(一般存放在根目录，如果没有需要手动创建) ，把www.a.com加入到白名单。其次，通过Flash URLLoader发送HTTP请求，最后，通过Flash API把响应结果传递给JavaScript。Flash URLLoader是一种很普遍的跨域解决方案，不过需要支持iOS的话，这个方案就无能为力了。

####Access Control

Access Control是比较超越的跨域方式，目前只在很少的浏览器中得以支持，这些浏览器可以发送一个跨域的HTTP请求（Firefox, Google Chrome等通过XMLHTTPRequest实现，IE8下通过XDomainRequest实现），请求的响应必须包含一个Access-Control-Allow-Origin的HTTP响应头，该响应头声明了请求域的可访问权限。例如www.a.com对www.b.com下的asset.php发送了一个跨域的HTTP请求，那么asset.php必须加入如下的响应头：

header("Access-Control-Allow-Origin: http://www.a.com");
		
####window.name

window对象的name属性是一个很特别的属性，当该window的location变化，然后重新加载，它的name属性可以依然保持不变。那么我们可以在页面A中用iframe加载其他域的页面B，而页面B中用JavaScript把需要传递的数据赋值给window.name，iframe加载完成之后，页面A修改iframe的地址，将其变成同域的一个地址，然后就可以读出window.name的值了。这个方式非常适合单向的数据请求，而且协议简单、安全。不会像JSONP那样不做限制地执行外部脚本。

####server proxy

在数据提供方没有提供对JSONP协议或者window.name协议的支持，也没有对其它域开放访问权限时，我们可以通过server proxy的方式来抓取数据。例如当www.a.com域下的页面需要请求www.b.com下的资源文件asset.txt时，直接发送一个指向www.b.com/asset.txt的ajax请求肯定是会被浏览器阻止。这时，我们在www.a.com下配一个代理，然后把ajax请求绑定到这个代理路径下，例如www.a.com/proxy/, 然后这个代理发送HTTP请求访问www.b.com下的asset.txt，跨域的HTTP请求是在服务器端进行的，客户端并没有产生跨域的ajax请求。这个跨域方式不需要和目标资源签订协议，带有侵略性，另外需要注意的是实践中应该对这个代理实施一定程度的保护，比如限制他人使用或者使用频率。

###双向跨域

####document.domain

通过修改document的domain属性，我们可以在域和子域或者不同的子域之间通信。同域策略认为域和子域隶属于不同的域，比如www.a.com和sub.a.com是不同的域，这时，我们无法在www.a.com下的页面中调用sub.a.com中定义的JavaScript方法。但是当我们把它们document的domain属性都修改为a.com，浏览器就会认为它们处于同一个域下，那么我们就可以互相调用对方的method来通信了。

####FIM – Fragment Identitier Messaging

不同的域之间，JavaScript只能做很有限的访问和操作，其实我们利用这些有限的访问权限就可以达到跨域通信的目的了。FIM (Fragment Identitier Messaging)就是在这个大前提下被发明的。父窗口可以对iframe进行URL读写，iframe也可以读写父窗口的URL，URL有一部分被称为frag，就是#号及其后面的字符，它一般用于浏览器锚点定位，Server端并不关心这部分，应该说HTTP请求过程中不会携带frag，所以这部分的修改不会产生HTTP请求，但是会产生浏览器历史记录。FIM的原理就是改变URL的frag部分来进行双向通信。每个window通过改变其他window的location来发送消息，并通过监听自己的URL的变化来接收消息。这个方式的通信会造成一些不必要的浏览器历史记录，而且有些浏览器不支持onhashchange事件，需要轮询来获知URL的改变，最后，URL在浏览器下有长度限制，这个制约了每次传送的数据量。

####Flash LocalConnection

页面上的双向通信也可以通过Flash来解决，Flash API中有LocalConnection这个类，该类允许两个SWF之间通过进程通信，这时SWF可以播放在独立的Flash Player或者AIR中，也可以嵌在HTML页面或者是PDF中。遵循这个通信原则，我们可以在不同域的HTML页面各自嵌套一个SWF来达到相互传递数据的目的了。SWF通过LocalConnection交换数据是很快的，但是每次的数据量有40kb的大小限制。用这种方式来跨域通信过于复杂，而且需要了2个SWF文件，实用性不强。

####window.postMessage

window.postMessage是HTML5定义的一个很新的方法，这个方法可以很方便地跨window通信。由于它是一个很新的方法，所以在很旧和比较旧的浏览器中都无法使用。

####Cross Frame

Cross Frame是FIM的一个变种，它借助了一个空白的iframe，不会产生多余的浏览器历史记录，也不需要轮询URL的改变，在可用性和性能上都做了很大的改观。它的基本原理大致是这样的，假设在域www.a.com上有页面A.html和一个空白代理页面proxyA.html, 另一个域www.b.com上有个页面B.html和一个空白代理页面proxyB.html，A.html需要向B.html中发送消息时，页面会创建一个隐藏的iframe, iframe的src指向proxyB.html并把message作为URL frag，由于B.html和proxyB.html是同域，所以在iframe加载完成之后，B.html可以获得iframe的URL，然后解析出message，并移除该iframe。当B.html需要向A.html发送消息时，原理一样。Cross Frame是很好的双向通信方式，而且安全高效，但是它在Opera中无法使用，不过在Opera下面我们可以使用更简单的window.postMessage来代替。

###总结

跨域的方法很多，不同的应用场景我们都可以找到一个最合适的解决方案。比如单向的数据请求，我们应该优先选择JSONP或者window.name，双向通信我们采取Cross Frame，在未与数据提供方没有达成通信协议的情况下我们也可以用server proxy的方式来抓取数据。

[转载地址]('http://www.woiweb.net/10-cross-domain-methods.html')