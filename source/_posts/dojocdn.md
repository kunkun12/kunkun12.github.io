title: dojocdn
date: 2014-11-22 13:05:34
tags: Dojo
---

谷歌为dojo提供了CDN服务，可惜我朝无法正常访问，于是自己在国内空间 -@七牛云存储 上部署了一份。。感谢

dojo的最新版本 1.10.2

<http://dojocdn.qiniudn.com/dojo-1.10.2/dojo/dojo.js>

未压缩版本

<http://dojocdn.qiniudn.com/dojo-1.10.2/dojo/dojo.js.uncompressed.js>


本目录与dojo源码的目录一致，同时包含了`dojo` `dijit` `dojox` `dgrid` `put-selector` `xstyle` `dstore` 目录结构如下

		http://dojocdn.qiniudn.com/dojo-1.10.2
												|dojo
												|dijit
												|dojox
												|dgrid
												|put-selector
												|xstyle
												|dstore

可以通过`dgrid/*`,`put-selector/*`,`xstyle!xxx.html`,`dstore/*`引用对应的插件，无需额外的配置

如果要找dojo中的主题

## claro.css

<http://dojocdn.qiniudn.com/dojo-1.10.2/dijit/themes/claro/claro.css>

## tundar主题

<http://dojocdn.qiniudn.com/dojo-1.10.2/dijit/themes/tundra/tundra.css>

## soria主题
http://dojocdn.qiniudn.com/dojo-1.10.2/dijit/themes/soria/soria.css

可以通过`dgrid/*`,`put-selector/*`,`xstyle!xxx.html`,`dstore/*`引用对应的插件，无需额外的配置

[以创建grid为例的Demo](http://jsbin.com/gogezu/2/edit?html,js,output)