title: ionic 环境搭建 （android mac）
date: 2015-01-05 00:02:14
tags: ionic
---

### android基础环境 JAVA 、androidSDK，ant 

### 配置环境变量 `sudo vim ~/.bash_profile` 或者`sudo vim .profile `

		export  PATH=${PATH}:/Users/adsage/work/adt/sdk/platform-tools:/Users/adsage/work/adt/sdk/tools
		ANDROID_HOME=$HOME/work/adt/sdk
		export ANDROID_HOME

### 使环境变量生效 `source .bash_profile`

### 将android SDK目录权限设置为可写 

		chmod -R 777 sdk/

### 安装nodeJS 以及相关包

		npm install -g cordova ionic

#### 创建一个项目 

		ionic start myApp tabs

### 编译并在真机上运行

		cd myApp
		$ ionic platform add android
		$ ionic build android
		$ ionic run android 

### 程序已部署至真机 完成。

