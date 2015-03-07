title: ionic 环境搭建 （mac）
date: 2015-01-05 00:02:14
tags: Ionic
---

### android基础环境 JAVA 、ADT (Android SDK tools) （该过程不详细叙述）

### 配置环境变量 `sudo vim ~/.bash_profile` 或者`sudo vim .profile `

		export  PATH=${PATH}:/Users/adsage/work/adt/sdk/platform-tools:/Users/adsage/work/adt/sdk/tools
		ANDROID_HOME=$HOME/work/adt/sdk
		export ANDROID_HOME

### 使环境变量生效 `source .bash_profile`

### 将android SDK目录权限设置为可写 

		chmod -R 777 sdk/

### android构建依赖ant

		brew update &&brew install ant

###  下载android API 和 镜像

－ 使用android SDK manager 下载android的API 和镜像（当然也可以离线下载） 
－ 使用Android Virtual manager 创建android的虚拟机

### 安装nodeJS 以及相关包

		sudo npm install -g cordova ionic


#### 创建一个项目 

		ionic start myApp tabs

### 编译并在真机上运行

		cd myApp
		$ ionic platform add android
		$ ionic build android
		$ ionic run android 或者ionic emulate android 在模拟上跑

### 程序已部署至真机 完成。

### ios 
另外 如果是IOS环境下 需要有IOS环境 在app store上下载安装XCODE即可, 另外在构建过程中还会提示缺少相关的npm包 比如

		sudo npm install -g ios-sim
		sudo npm install -g ios-deploy

然后 运行 

		ionic platform add ios
		ionic build ios
		ionic run ios 或者ionic emulate ios 在模拟上跑
