---
layout: post
title: "在django中使用postgresQL"
date: 2014-03-15 12:16:54 +0800
comments: true
categories: django python
tag: Django
---
最近开始做的东西要用到Django，数据库是postgresQL,需要在Django的setting.py中修改相关数据库的配置文件。事先我已经装好了PostgresQL 9.2.6

####下载python连接postgresQL数据库的驱动

安装方式也有多种：根据个人喜好可以使用easy_istall或者pip方式安装

- easy_install psycopg2
- pip install psycopg2

或者自己下载<http://initd.org/psycopg/>解压之后，进入目录运行

	python setup.py install

会自动安装到Python安装目录的site-packages的目录下

这里我直接在windows安装的exe 下载地址（如果是64位的系统的话也建议下载32位的）<http://www.stickpeople.com/projects/python/win-psycopg/> 根据数据库的版本选择合适的进行下载

####修改配置文件setting.py,找到DATABASES处

		DATABASES = {
		    'default': {
		        'ENGINE': 'django.db.backends.postgresql_psycopg2',
		        'NAME': 'mydb',                      
		        'USER': 'postgres',
		        'PASSWORD': '1',
		        'HOST': 'localhost',
		        'PORT': '5432'
		    }
		}


将ENGINE修改为`django.db.backends.postgresql_psycopg2`,**NAME**为数据库的名字,**USER**为数据库的用户名，**PASSWORD**为数据库用户对应的密码，**HOST**为数据库所在的主机名，**Port**为PostgreSQL的端口号。


参考资料<https://docs.djangoproject.com/en/dev/ref/settings/#std:setting-DATABASES>