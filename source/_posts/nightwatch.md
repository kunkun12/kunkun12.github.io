title: web 端到端测试工具 Nightwatch.js
date: 2017-10-15 11:56:40
tags: 
---

### Nightwath 介绍

说起end to end 测试,可以理解为模拟用户的行为观察页面、点击页面是否与预期的结果相符，一般常用的是Selenium，，它提供了API用于自动化的测试，Selenium使用的是一套标准的 W3C 的 Web Driver API （一套用于自动化测试的协议、该协议已经成了事实的标准 ），而Nightwatch(官网地址 http://nightwatchjs.org) 也是在这套协议基础之上提供的一套NodeJS的API，特点是语法简洁、使用方便，支持各种浏览器。官方文档写的非常详细，这里做一下简单的介绍。下面是官方给出的工作原理图,猫头鹰就是表示nightwatch，客户端浏览器通过Web Driver协议与 Selenium交互，Selenium与nightwatch相互通信来完成整个过程，nightwatch会往浏览器中注入代码，来操作Dom，然后通过websocket完成与测试服务端的通信来展示测试任务的结果。

![](http://nightwatchjs.org/img/operation.png)

### 环境搭建

#### 安装 Nightwatch 。使用npm全局安装或者本地安装都行

    npm install -g nigthwatch

或者 
    ` npm install nightwatch`

#### Selenium Server 

这里需要调用Selenium Server 提供的API，需要去下载Selenium Server的jar包(http://selenium-release.storage.googleapis.com/index.html), 下载最新的`selenium-server-standalone-{VERSION}.jar`即可,当然需要本机有Java环境  Java version要在7以上。

### 浏览器驱动下载
Selenium 支持不同的浏览器，对于制定的浏览器需要下载特定的 驱动，这里以chrome为例 下载地址 （https://sites.google.com/a/chromium.org/chromedriver/downloads)。
这里为了方便把下载的Selenium Server 以及 chromedriver 统一放到项目的 bin目录下 随项目一起提交了。

#### 配置以及运行测试用例

新建一个配置文件内容如下
    

    {
      "src_folders": [
        "e2e"
      ],
      "output_folder": "reports",
      "selenium": {
        "start_process": true,
        "server_path": "./bin/selenium-server.jar",
        "log_path": "",
        "port": 4444,
        "cli_args": {
          "webdriver.chrome.driver": "./bin/chromedriver"
        }
      },
      "test_settings": {
        "default": {
          "launch_url": "http://localhost",
          "selenium_port": 4444,
          "selenium_host": "127.0.0.1",
          "silent": true,
          "screenshots": {
            "enabled": true,
            "path": "."
          },
          "globals": {
            "waitForConditionTimeout": 1000 
          },
          "desiredCapabilities": {
            "browserName": "chrome",
            "chromeOptions": {
              "args": [
              "--user-agent=Mozilla/5.0 (Linux; Android 5.0; SM-G900P Build/LRX21T) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Mobile Safari/537.36",
                "--window-size=360,640" 
              ]
            },
            "javascriptEnabled": true,
            "acceptSslCerts": true
          }
        }
      }
    }

http://nightwatchjs.org/gettingstarted/ 官网上有配置文件的具体介绍，不在详细叙述，需要是设置selenium 以及 浏览器驱动的路径，这里用的是相对目录
新建 `nightwatch.conf.js`文件里面放入

    module.exports = (function(settings) {
      settings.test_workers = false;
      return settings;
    })(require('./nightwatch.json'));

此时在项目目录下运行 nightwatch 会自动启动自动化测试程序，运行测试用例，下面编写一个简单的测试用例,新建 e2e/test.js文件，内容如下

      module.exports = {
      'Demo test' : function (browser) {
        browser
          .url("http://www.baidu.com")
          .waitForElementVisible('body', 1000)
          .setValue('#index-kw',"面向对象")
          .click('#index-bn')
          .assert.containsText('body', '面向对象')
          .saveScreenshot('./test.png')
          .end();
      }
    };


终端下执行 `nightwatch` 即可运行，查看测试报告，并得到一张截图。
*如果是本地安装的nightwatch 执行`./node_modules/.bin/nightwatch` * 也可以通过npm script 形式来执行。该项目的目录结构如下

    ── bin
    │   ├── chromedriver
    │   └── selenium-server.jar
    ├── e2e
    │   └── test.js
    ├── nightwatch.conf.js
    ├── nightwatch.json
    ├── package.json




环境搭建好了，接下来就是编写测试用例，官方有详细的开发者指导，以及API文档。另外Nightwatch 也可以进行单元测试、与mocha 、chai等知名的工具结合使用。

#### 踩坑

- 英文的文档弄懂 nightwatch、Selenium Server、浏览器驱动 三者之前的关系，以及运行原理
- 在chrome测试模拟点击的时候确保 被点击的dom是可见的，否则会报错。
- 一些nightwatch 没有提供的模拟器操作，比如将某个dom scroll into View，需要使用 execute 方法 自己注入js代码来完成。


