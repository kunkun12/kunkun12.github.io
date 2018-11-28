title: less常用特性
date: 2015-01-11 23:53:04
tags: Less Css
categories: Less
---

less并非多么高大上的东西，无非就是个编写CSS的工具，让我们像编程一样来书写CSS，关于less常用的特性无非几个，下面简单将常用这些特性简单记录。方便以后查阅

## less编译环境 

less->CSS的方法有多种，比如使用  grunt或者lgulp等构建工具配合相应的插件来完成less的自动编译。以gulp为例，gulpfile.js的代码如下。

		var less = require('gulp-less');
		var path = require('path');
		var gulp=require('gulp');
		gulp.task('less', function () {
		  gulp.src('less/*.less')
		    .pipe(less({
		      paths: [ path.join(__dirname, 'less', 'includes') ]
		    }))
		    .pipe(gulp.dest('css'));
		});

		gulp.watch(['less/*.less'], ['less']) 
		gulp.task('default', ['less']);

通过watch 来见监听less文件的变化自动运行less任务。从而完成less->css的自动编译，可以查看生成的CSS文件


## 我常用的less 特性。

### @import 

在标准的CSS中，@import必须在所有其他类型的规则之前。但是Less不在乎你把@import语句放在什么位置。
@import语句会通过Less依赖文件扩展名的方式区别对待不同的文件：

- 如果文件有一个.css扩展名，则将它作为CSS对象，同时@import语句保持不变
- 如果有其他扩展名，则作为Less对象，然后导入它。
- 如果没有扩展名，则插入.less，然后将它作为Less文件导入包含进来。
- 使用@import (reference)导入外部文件，但是不添加导入的样式到编译输出中，只引用。

### 变量

LESS 允许开发者自定义变量，变量可以在全局样式中使用，变量使得样式修改起来更加简单。变量`@`开头，全局的变量可以在多个less文件之间共用

		@light-blue: @nice-blue + #111;
		#header {
		  color: @light-blue;
		}

生成的CSS 如下 

		.mythemes tableBorder { 
		  border: 1px solid #b5bcc7; 
		 }

同时还支持变量的计算。

		@width: 10px + 5;
		  .btn{
		    width:@width;
		  }

编译为：

	@width: 10px + 5;
	  .btn{
	    width:@width;
	  }

`变量查找以及变量作用域`：在定义一个变量两次时，只会使用最后定义的变量，Less会从当前作用域中向上搜索。这个行为类似于CSS的定义中始终使用最后定义的属性值。

		@var: 0;
		.class {
		  @var: 1;
		  .brass {
		    @var: 2;
		    three: @var;
		    @var: 3;
		  }
		  one: @var;
		}

编译后的CSS为：

		.class {
		  one: 1;
		}
		.class .brass {
		  three: 3;
		}

### Mixins（混入）

在 LESS 中，混入是指在一个 CLASS 中引入另外一个已经定义的 CLASS，就像在当前 CLASS 中增加一个属性一样。

less源文件

		.bordered {
		  border-top: dotted 1px black;
		  border-bottom: solid 2px black;
		}
		#menu a {
		  color: #111;
		  .bordered;
		}

编译后的CSS文件：

		.bordered {
		  border-top: dotted 1px black;
		  border-bottom: solid 2px black;
		}
		#menu a {
		  color: #111;
		  border-top: dotted 1px black;
		  border-bottom: solid 2px black;
		}

如果不想在CSS中保留原来的 **.bordered**

可以将 `.bordered` 修改为 `bordered()` ,同时还支持混入参数，支持默认参数。

less文件 

		.bordered(@width:5px) {
		  border-top: dotted 1px black;
		  border-bottom: solid 2px black;
		  width:@width;
		}
		#menu a {
		  color: #111;
		  .bordered;
		}
		.post a {
		  color: red;
		  .bordered(10px);
		}

编译后的文件

		#menu a {
		  color: #111;
		  border-top: dotted 1px black;
		  border-bottom: solid 2px black;
		  width: 5px;
		}
		.post a {
		  color: red;
		  border-top: dotted 1px black;
		  border-bottom: solid 2px black;
		  width: 10px;
		}

像 JavaScript 中 arguments一样，Mixins 也有这样一个变量：@arguments。@arguments 在 Mixins 中具是一个很特别的参数，当 Mixins 引用这个参数时，该参数表示所有的变量，很多情况下，这个参数可以省去你很多代码。

		.box1(@x:0,@y:0,@blur:1px,@color:#000){ 
		   -moz-box-shadow: @arguments; 
		   -webkit-box-shadow: @arguments; 
		   box-shadow: @arguments; 
		 } 
		 .mybox { 
		     .box1(10px,12px,13px,red); 
		 }

编译后：

		.mybox {
		  -moz-box-shadow: 10px 12px 13px red;
		  -webkit-box-shadow: 10px 12px 13px red;
		  box-shadow: 10px 12px 13px red;
		}

### 嵌套

在写CSS的时候 为了避免造成CSS污染 我们会采用命名空间的机制，在less中可以通过嵌套来解决。

less文件 

		.myspace{
			 .class1{
			 	width:20px
			 }
			 .class2{
			 	height:30px
			 }
		 }
生成的CSS文件

		.myspace .class1 {
		  width: 20px;
		}
		.myspace .class2 {
		  height: 30px;
		}

在嵌套使用时可以通过 &访问上一级元素

		#bundle {
		  .button {
		    display: block;
		    border: 1px solid black;
		    background-color: grey;
		    &:hover {
		      background-color: white
		    }
		  }
		}

编译之后为

		#bundle .button {
		  display: block;
		  border: 1px solid black;
		  background-color: grey;
		}
		#bundle .button:hover {
		  background-color: white;
		}

在less中可以如果将mix和嵌套相结合。可以使用`>`来进行导入

比如 less文件

		.myspace{
		   .class1{
		   width:20px
		   }
		   .class2{
		   height:30px
		   }
		 }
		 .redbutton{
			 color:red;
			 .myspace > .class2;
		 }
CSS 文件

		myspace .class1 {
		  width: 20px;
		}
		.myspace .class2 {
		  height: 30px;
		}
		.redbutton {
		  color: red;
		  height:
		}



另外还有一些关于运算的函数 感觉意义不大 比如颜色计算的fadeIn等。

### Comments（注释）

LESS 对注释也提供了支持，JavaScript 中的注释方法一样，主要有两种方式：单行注释和多行注释，这与 ，我们这里不做详细的说明，只强调一点：LESS 中单行注释 (// 单行注释 ) 是不能显示在编译后的 CSS 中，所以*如果你的注释是针对样式说明的请使用多行注释*

### extend

extend是一个Less伪类，它会合并它所在的选择其和它所匹配的引用。

		nav ul {
		  &:extend(.inline);
		  background: blue;
		}

在上面设置的规则中，:extend选择器会在.inline类出现的地方在.inline上应用"扩展选择器"(也就是nav ul)。声明块保持原样，不会带有任何引用扩展(因为扩展并不是CSS)。
	
extend即为把当前的选择的器应用到相应的选择器定义的样式中，有点像组合的意思，跟混合的作用类似，基本是复用之前定义的样式，mix是将选择的样式引进来，extend是将当前的选择器。添加到之前定义的样式处。（有点绕啊）看代码吧

less文件

	nav ul {
		  &:extend(.inline);
		  background: blue;
		}
		.inline {
		  color: red;
		}


输出CSS：


		nav ul {
		  background: blue;
		}
		.inline,
		nav ul {
		  color: red;
		}

另外还有一些关于运算的函数 感觉意义不大 比如颜色计算的fadeIn等。

### Comments（注释）

LESS 对注释也提供了支持，JavaScript 中的注释方法一样，主要有两种方式：单行注释和多行注释，这与 ，我们这里不做详细的说明，只强调一点：LESS 中单行注释 (// 单行注释 ) 是不能显示在编译后的 CSS 中，所以*如果你的注释是针对样式说明的请使用多行注释*