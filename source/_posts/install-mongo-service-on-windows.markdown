---
layout: post
title: "在windows 下安装 MongoDb"
date: 2014-05-06 08:15:32 +0800
comments: true
categories: MongoDb
tag: MongoDb
---

> Starting in version 2.2, MongoDB does not support Windows XP



- 到官网下载[MongoDb](http://www.mongodb.org/downloads)
- 解压到制定目录。我的机器目录如下

		D:
		  mongodb
				 |mongodb							
						|mongod.cfg
						|README
						|THIRD-PARTY-NOTICES
						|bin
- 创建MongoDb的数据存储目录，可以再任意文件夹下创建，这里我在**D:/mongodb**下创建了**D:/mongodb/data/db**目录
- 启动MongoDb服务，打开命令行模式下 进入MongoDb的bin目录 运行**mongod.exe**命令

		D:\mongodb\mongodb\bin\mongod.exe --dbpath d:\mongodb\data

输出的结果如下,表示服务启动成功.然后在浏览器输入localhost:28017会打开一个MongoDb的监控网页

		Tue May 06 08:56:28.645 [initandlisten] MongoDB starting : pid=808 port=27017 db
		path=d:\mongodb\data 64-bit host=DuBaoKun
		Tue May 06 08:56:28.646 [initandlisten] db version v2.4.1
		Tue May 06 08:56:28.646 [initandlisten] git version: 1560959e9ce11a693be8b4d0d16
		0d633eee75110
		********
		Tue May 06 08:56:29.056 [initandlisten] command local.$cmd command: { create: "s
		tartup_log", size: 10485760, capped: true } ntoreturn:1 keyUpdates:0  reslen:37
		290ms
		Tue May 06 08:56:29.077 [initandlisten] waiting for connections on port 27017
		Tue May 06 08:56:29.077 [websvr] admin web console waiting for connections on po
		rt 28017

- 连接数据库 保持上述的Mongodb服务处于开启状态 在开启一个控制台，运行**D:\mongodb\mongodb\bin\mongo.exe**命令,则会连接成功了。可以按照[这篇文章](http://docs.mongodb.org/manual/tutorial/getting-started/)继续操作


###将MongoDb配置为服务

>虽然进行了以上的配置，但是之后每次使用Mongodb的时候都要先启动服务，如果配置为windows服务，让其自动启动。

- 创建日志目录：除了配置数据存储目录外，MongoDb还需要要一个日志存储的目录。在**d:\mongodb**目录下创建log目录(这个目录是任意的)
- 创建服务配置文件：在**d:\mongodb**目录下创建**mongod.cfg**文件。用记事本打开配置dbpath和logpath

		logpath=D:\mongodb\log\mongo.log
		dbpath=D:\mongodb\data\db

- 安装服务。在命令行中运行如下命令

		D:\mongodb\mongodb\bin\mongod.exe --config D:\mongodb\mongod.cfg --install
运行之后提示如下

		```
		Tue May 06 10:03:12.151 Trying to install Windows service 'MongoDB'
		Tue May 06 10:03:12.358 Service 'MongoDB' (Mongo DB) installed with command line
		'D:\mongodb\mongodb\bin\mongod.exe --config D:\mongodb\mongod.cfg --service'
		Tue May 06 10:03:12.359 Service can be started from the command line with 'net s
		tart MongoDB' ```

此命令成功之后会安装 Mongo Db的服务。并且开机会自动启动，如果要自己控制启动，在开始栏的搜索框输入services.msc可以到计算机管理服务里找个Mongo Db的服务进行设置.

> 启动服务 **net start MongoDB**

> 停止服务 **net stop MongoDB**

> 删除服务 **"C:\Program Files\MongoDB\bin\mongod.exe" --remove**

- 连接数据库 保持Mongodb服务处于开启状态 开启一个控制台，运行**D:\mongodb\mongodb\bin\mongo.exe**命令,则会连接成功了。可以按照[这篇文章](http://docs.mongodb.org/manual/tutorial/getting-started/)继续操作