title: ubuntu 下使用n 管理node 版本遇到的问题
date: 2016-01-21 17:09:13
tags:
---

ubuntu 中使用n 来管理node，首先根据github<https://github.com/tj/n>上面的提示进行安装 

在非 root 用户的话 ，运行node提示找不到命令（root下可以，然而使用 Linux是不鼓励在root 下操作的) 
可以确认环境变量没有生效，可是装n之后会自动把
`export N_PREFIX="$HOME/n"; [[ :$PATH: == *":$N_PREFIX/bin:"* ]] || PATH+=":$N_PREFIX/bin"`  
添加到.bashrc下了。来自动设置了相关的环境变量。
于是 运行下 `source ~/.bashrc` 就可以了 环境变量生效了.可以正常运行弄的，然而 重新打开一个终端以后 又失效了。

在登录的时候会系统自动执行`~/.profile` ,在`~/.profile` 有

        # if running bash
        if [ -n "$BASH_VERSION" ]; then
            # include .bashrc if it exists
            if [ -f "$HOME/.bashrc" ]; then
            . "$HOME/.bashrc"
            fi
        fi

就是终端连接的时候会自动执行  `~/.bashrc `,理论上来说是环境变量会自动生效才是。可以猜测在执行 `~/.bashrc` 的时候 出了问题 ，导致 n相关的环境变量没有生效。
于是把~/.bashrc的那一行代码 `export N_PREFIX="$HOME/n"; [[ :$PATH: == *":$N_PREFIX/bin:"* ]] || PATH+=":$N_PREFIX/bin"`
拷贝到 ~/.profile 下即可使环境变量生效了。重新连接以后 运行node 就可以了。。