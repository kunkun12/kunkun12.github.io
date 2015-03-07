---
layout: post
title: "git账户配置以及多账户配置"
date: 2014-03-02 11:42:56 +0800
comments: true
categories: Git
tag: Git
---
在使用git的时候，git与远程服务器是一般通过ssh传输的(也支持ftp，https)，我们在管理远程分支之前 需要在本机上创建ssh-key密钥对，并把其中的公钥添加到github中。

###单用户情况
如果你就会一直在你的计算计算机使用一个远程的Git服务器，并且账号是一个，比较简单，生成key的时候也没有太大注意的地方，直接运行如下的第一步然后按回车就可以了
  1、在 gitbash上运行  ssh-keygen -t  rsa  -C  "Github账户邮箱"
  2、接下来会提示输入key的名字 默认名字为id_rsa .默认就行了
  3、然后会提示输入口令，这里口令与Github中的密码无关，随便输入可以为空。
  4. 如果在第二步中的没有重新命名的话，则忽略此步骤，ssh agent默认只读取id_rsa，为了让SSH识别新的私钥，需将其添加到SSH agent中 
    ssh-add id_rsa
如果出现Could not open a connection to your authentication agent的错误，就试着先用以下命令：

	ssh-agent bash
	ssh-add id_rsa
添加完之后 登陆Github   点击 网页右上侧的 Account Setting 按钮 - 选择 ssh-keys  点击Add SSH Key  ，在title中输入名字，然后将公约即id_rsa.pub添加到ssh-key处。

在git bash的命令行中输入 

ssh -T git@github.com 如果能正常访问即可

	$ ssh -T git@github.com
	Enter passphrase for key 'C:/Users/kunkun/.ssh/github.rsa':
	Hi kunkun01! You've successfully authenticated, but GitHub does not provide shel
	l access.

###多账户配置

 多账户又分为两种情况  

 * 针对同一个服务器的同用户（比如 我平时开发开源的小东东，有的是一个账号是公司的账号对外开源项目用的，另外我自己也比较崇尚开源，所以自己也有了Github账号)
 * 针对不同服务器的用户(现在pass平台 部署应用都是通过git来管理的，比如常见的Openshift，Heroku appfog等，在这里我也注册了账号）

 在我们访问git服务器的时候，如果通过ssh的方式话，访问不同的服务器要使用不同的ssh-key。经过在第一步的过程中，在创建ssh-key的默认命名为id_rsa，如果使用不同的账户的，必须得给不同的key设置不同的名字，否则如果继续使用默认名字的话，会把之前的id_rsa覆盖掉。
 具体操作如下 user2是我的另外一个Github账户
####1、新建user2的SSH Key
	 #新建SSH key：
	$ cd ~/.ssh     # 切换到C:\Users\Administrator\.ssh
	ssh-keygen -t rsa -C "mywork@email.com"  # 新建工作的SSH key
	# 设置名称为id_rsa_work
	Enter file in which to save the key (/c/Users/Administrator/.ssh/id_rsa): id_rsa_work  
####2、新密钥添加到SSH agent中
因为默认只读取id_rsa，为了让SSH识别新的私钥，需将其添加到SSH agent中：

	ssh-add ~/.ssh/id_rsa_work
上面提到了，如果出现Could not open a connection to your authentication agent的错误，就试着用以下命令：

	ssh-agent bash
	ssh-add ~/.ssh/id_rsa_work
####3、修改config文件 将账户以及git服务器与对应的密钥关联。在`C:\Users\kunkun\.ssh`目录下找到config文件，如果没有就创建名字为config的文本文件（无后缀名），然后修改如下： 比如我的config配置如下：

	# 该文件用于配置私钥对应的服务器
	# Default github user(first@mail.com)
	 Host github.com
	 HostName github.com
	 User git
	 IdentityFile C:/Users/Administrator/.ssh/id_rsa

	 # second user(second@mail.com)
	 # 建一个github别名，新建的帐号使用这个别名做克隆和更新
	 Host github2
	 HostName github.com
	 User git
	 IdentityFile C:/Users/Administrator/.ssh/id_rsa_work
其规则就是：从上至下读取config的内容，在每个Host下寻找对应的私钥。这里将GitHub SSH仓库地址中的git@github.com替换成新建的Host别名如：github2，那么原地址是：git@github.com:kunkun/Mywork.git，替换后应该是：github2:kunkun/Mywork.git。

####4、用记事本打开新生成的~/.ssh/id_rsa2.pub 和id_rsa_work 复制里面的内容、登陆Github网站，将里面的内容添加到分别加入对应的ssh keys中。

####5、测试：
	
	$ ssh -T git@github.com
	Hi BeginMan! You've successfully authenticated, but GitHub does not provide shel
	l access.

	$ ssh -T github2
	Hi BeginMan! You've successfully authenticated, but GitHub does not provide shel
	l access.


克隆的时候将Github.com替换为对应的别名即可
比如我的第二个账户中的一个仓库地址为git@github.com:kunkun12/PiesLayer.git。但是我以上的配置中使用

 git clone git@github2:kunkun12/PiesLayer.git


 注意：如果你只是通过这篇文章中所述配置了Host，那么你多个账号下面的提交用户会是一个人，所以需要通过命令git config --global --unset user.email删除用户账户设置，在每一个repo下面使用git config --local user.email '你的github邮箱@mail.com' 命令单独设置用户账户信息
