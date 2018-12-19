---
layout: post
title: "如何使用Node+express创建Rest API"
date: 2014-03-15 23:48:08 +0800
comments: true
categories: API 设计
tag:  Node express
---
### 安装环境

1 首先确保你的机器上已经安装了NodeJS环境，如果没有安装的话 点击此处[下载](http://nodejs.org/)
2 安装 express 

	npm install express -g

3 创建express 项目

	express myapp

### API设计

这里只是实现一个基本的计划任务列表，一个计划包含的信息有 名字，描述，截至日期，任务状态，以及一个唯一的标示符，API使用JSON作为数据传输的格式，下面具体描述下API的功能。

- GET /tasks/ 返回计划任务列表
- GET /tasks/:id  根据指定的任务id 返回计划任务的信息
- POST /tasks/ 接受传进来的JSON数据来创建一个新的计划任务
- PUT /tasks/:id 更新计划任务信息
- DELETE /tasks/:id 根据指定的计划Id 删除计划任务

这些方法的返回如下

- 访问get请求之外的api ，如果执行成功返回http 状态符为200
- 如果访问不存的计划信息 则返回http 状态符为 404

### 实现

#### 数据
这里仅是为了展示如何创建 Web api，因此尽可能简化每一个步骤，这个例子中计划内容都是一个Javascript数组存储在内存中的，我仅仅是创建了一个TaskRepository 创建了一个简单的javascript数组，TaskRepository的接口描述如下：

		function TaskRepository() {}
		/**
		 * Find a task by id
		 * Param: id of the task to find
		 * Returns: the task corresponding to the specified id
		 */
		TaskRepository.prototype.find = function (id) {}
		/**
		 * Find the index of a task
		 * Param: id of the task to find
		 * Returns: the index of the task identified by id
		 */
		TaskRepository.prototype.findIndex = function (id) {}
		/**
		 * Retrieve all tasks
		 * Returns: array of tasks
		 */
		TaskRepository.prototype.findAll = function () {
		    return this.tasks;
		}
		/**
		 * Save a task (create or update)
		 * Param: task the task to save
		 */
		TaskRepository.prototype.save = function (task) {}
		/**
		 * Remove a task
		 * Param: id the of the task to remove
		 */
		TaskRepository.prototype.remove = function (id) {}

#### 创建express server

		var express = require('express');
		var app = express();
		app.configure(function() {
		    app.use(express.bodyParser()); // used to parse JSON object given in the request body
		});

#### 获取计划的列表 get

		app.get('/tasks', function (request, response) {
		    response.json({tasks: taskRepository.findAll()});
		});

#### 根据指定的id获取一个计划的信息 get

		app.get('/tasks/:id', function (request, response) {
		    var taskId = request.params.id;
		    try {
		        response.json(taskRepository.find(taskId));
		    } catch (exeception) {
		        response.send(404);
		    }
		});

#### 创建一个新的计划 post

		app.post('/tasks', function (request, response) {
		    var task = request.body;
		    taskRepository.save({
		        title: task.title || 'Default title',
		        description: task.description || 'Default description',
		        dueDate: task.dueDate,
		        status: task.status || 'not completed'
		    });
		    response.send(200);
		});

#### 更新计划信息 put

		app.put('/tasks/:id', function (request, response) {
		    var task = request.body;
		    var taskId = request.params.id;
		    try {
		        var persistedTask = taskRepository.find(taskId);
		        taskRepository.save({
		            taskId: persistedTask.taskId,
		            title: task.title || persistedTask.title,
		            description: task.description || persistedTask.description,
		            dueDate: task.dueDate || persistedTask.dueDate,
		            status: task.status || persistedTask.status
		        });
		        response.send(200);
		    } catch (exception) {
		        response.send(404);
		    }
		});

#### 删除计划delete

		app.delete('/tasks/:id', function (request, response) {
		    try {
		        taskRepository.remove(request.params.id);
		        response.send(200);
		    } catch (exeception) {
		        response.send(404);
		    }
		});

#### 启动express server

		app.listen(8080)

### 测试 这里使用 curl 执行http请求访问rest api

首先启动程序

		node rest_api.js

最初的情况下访问计划列表应该空的

		$ curl -i http://localhost:8080/tasks/
		HTTP/1.1 200 OK
		X-Powered-By: Express
		Content-Type: application/json; charset=utf-8
		Content-Length: 17
		Date: Sun, 10 Feb 2013 12:37:45 GMT
		Connection: keep-alive
		 
		{
		  "tasks": []
		}
#### 创建计划

		$ curl -i -X POST http://localhost:8080/tasks --data '{}' -H "Content-Type: application/json"
		HTTP/1.1 200 OK
		X-Powered-By: Express
		Content-Type: text/plain
		Content-Length: 2
		Date: Sun, 10 Feb 2013 12:39:13 GMT
		Connection: keep-alive
		 
		OK
#### 更新计划

		$ curl -i -X PUT http://localhost:8080/tasks/1 --data '{"description":"blabla"}' -H "Content-Type: application/json"
		HTTP/1.1 200 OK
		X-Powered-By: Express
		Content-Type: text/plain
		Content-Length: 2
		Date: Sun, 10 Feb 2013 12:42:39 GMT
		Connection: keep-alive
		OK

#### 删除计划

		$ curl -i -X DELETE http://localhost:8080/tasks/1
		HTTP/1.1 200 OK
		X-Powered-By: Express
		Content-Type: text/plain
		Content-Length: 2
		Date: Sun, 10 Feb 2013 12:43:31 GMT
		Connection: keep-alive
		 
		OK

这里介绍了用express创建rest api的大致过程，而实际过程中要复杂一些，比如存储数据使用数据库(mongodb 或者传统的关系数据库）来解决，有的也会包括一些复杂的逻辑信息。
