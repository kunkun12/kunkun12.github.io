---
layout: post
title: "Phonegap创建移动应用"
date: 2014-03-16 00:32:58 +0800
comments: true
categories: Phonegap
tag: Phonegap
---
前段时间学习了Phonegap开发，本篇作为记录，希望对初道者有帮助,本篇主要介绍Phonegap以及环境的搭建。下篇结合Angluar 开发小Demo
###Phonegap介绍
Phonegap 作为一个开源的框架来帮助开发者使用Javascript/HTML5/CSS 开发跨平台的移动应用。他可以根据指定的移动平台打包为对应的包，支持的平台种类如下。

- iOS
- Android
- Windows 8
- Windows Phone 7 and 8
- BlackBerry 5.x+
- WebOS
- Symbian
- Tizen
- Firefox OS（3.4版本开始支持的）

### 插件
Phonegap 本质上是在本地原生应用程序中包裹了一个Webview 来支持使用HTML5 进行开发。但是这种方式本质上是不支持访问设备的功能，比如访问文件系统，或者照相机,但是Phonegap通过桥接机制允许Webview中的Javascript 来调用原生的代码，于是Phonegap就拥有了许多插件来支持访问设备API比如

- 加速度计
- 相机
- 联系人
- 文件系统
- 媒体文件的播放 以及录音
- 网络
- 存储

同时Phonegap 可以允许开发者通过扩展插件来访问更多的设备API比如

- 二维码扫描
- 蓝牙
- 推送通知
- 语音合成
- 日历

在之前版本的Phonegap中 包括了一些预编译的插件。随着Phonegap 3的到来，一个新的插件架构取代了原来的插件访问。同时有一个注册中心来创建兼容Phonegap 3的插件

####HTML/CSS /Javascript 移动开发基础。
Phonegap本身提供的大部分功能是提供了一些非可视化的功能组件，比如访问文件系统，网络能力，地理定位，等等。Phonegap本身并没有提供构建图形界面的能力，对于此，我们必须使用HTML、CSS来构建，为了更方便地开发中应用，因此推荐结合JS 框架来进行组织程序的开发。这些框架包括

- jQuery Mobile
- Ionic
- Sencha Touch
- Kendo UI Complete
- AppGyver’s Steroids
- Enyo
- TopCoat

jQuery Mobile 这个就不用说了，简单理解Jquery的移动版本，AngluarJS 是一个比较火的MMV框架，Ionic也是AngluarJS构建的，这里我采用的是AngluarJs。开发之前还要了解一下面的这个问题。
####PhoneGap Or Cordova的关系

在学习Phonegap的过程中，我们经常会碰到Cordova 这个词，事实上，我们使用命令行工具创建的Phonegap工程其实是Cordova，。Cordova是贡献给Apache后的开源项目，是从PhoneGap中抽出的核心代码，是驱动PhoneGap的核心引擎。Cordova提供了一组设备相关的API，通过这组API，移动应用能够以JavaScript访问原生的设备功能，如摄像头、麦克风等。
Cordova还提供了一组统一的JavaScript类库，以及为这些类库所用的设备相关的原生后台代码。你可以把他想象成类似于Webkit和Google Chrome的关系。在大部分情况下这两个名字是可以交换的，随着Phonegap作为Adobe的商标，这个开源的项目需要一个新的名字，因此就有了Cordova，Phonegap继续作为Adobe的商业产品，大部分仍然将这个整体的项目称为Phonegap。

- [Phonega 官网](http://phonegap.com/)
- [cordova 官网](http://cordova.apache.org/)

####Phonegap 开发环境搭建。
这里我要基于Phonegap开发能够运行在android上的程序。
####建环境的基本准备:
1. java JDK、配置Java环境变量
2. 下载[android ADT Bundle](https://developer.android.com/sdk/index.html?hl=sk)(里面是一个继承了安卓开发环境的Eclipse)，下载之后将android SDK下的tools的目录添加到环境变量中，启动下载包里面的eclipse 创建AVD
3. Ant 打包工具(windows 下载推荐下载 [winant](https://code.google.com/p/winant/))
4. [NodeJS](http://nodejs.org/)
5. 安装phonegap

		npm install -g cordova

	或者

		npm install -g phonegap
cordova和phonegap这两个命令是通用的。`npm install -g phonegap` 是3.0之后的新出的一种安装方式，这里我使用是phonegap。

####创建Helloword
在命令行下运行

		phonegap create my-app
		cd my-app
		phonegap run android

如果有android 设备连接了电脑的话，这时候会自动将应用部署到设备上，如果没有设备的话，会自动启用刚才创建的模拟器（注意 前面配置过程中一定要把android SDK下的tool 目录添加到环境变量中，并且确保已经创建了模拟器（AVD））到此为止一个Phonegap 版本的Hello World 已经开发完毕。

下面看进入my-app的目录，进入之后我们发现有5个文件夹

- www              : 文件夹  开发的 HTML5 ; CSS ; JS 文件都拷贝到这下面，以后我们开发HTML5的应用主要在这个文件夹下进行
- plugins          : 文件夹  存放的是phonegap插件比如文件,摄像头等插件都下载到这里。
- merges          : （这个我还不了解）
- platforms      : 文件夹 存放的是编译好后的android文件,(如果这个文件夹为空,需要你在命令行编译一次才能生成. 如上面 phonegap run android)
- .cordova        : 存放的是配置文件 。

将这个工程 导入到 Eclipse 下可以查看更多的工程信息。
