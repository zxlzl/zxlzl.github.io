---
title: NodeJs学习笔记1  
date: 2017-12-03 18:21:08  
categories: node  
tags:
---

# 一、Node.js简介

## 1.1 简介  

&emsp; V8引擎本身就是用于Chrome浏览器的JS解释部分，但是RyanDahl把V8搬到了服务器上，用于做服务器的软件。  
&emsp; Node.js是一个让JavaScript运行在服务器端的开发平台，它让JavaScript的触角伸到了服务器端，可以与PHP、JSP、Python、Ruby平起平坐。  
&emsp; 与Node不同：  
<table><td bgcolor=#FFFF00><font color=red>&emsp; Node.js不是一种独立的语言</font>，与PHP、JSP、Python、Perl、Ruby的“既是语言，也是平台”不同，Node.js的<font color=red>使用JavaScript进行编程</font>，运行在JavaScript引擎上（V8）。</td>
</table>
<table><td bgcolor=#FFFF00>&emsp; 与PHP、JSP等相比（PHP、JSP、.net都需要运行在服务器程序上，Apache、Naginx、Tomcat、IIS。），<font color=red>Node.js跳过了Apache、Naginx、IIS等HTTP服务器，它自己不用建设在任何服务器软件之上</font>。Node.js的许多设计理念与经典架构(LAMP = Linux + Apache + MySQL + PHP)有着很大的不同，可以提供强大的伸缩能力。Node.js没有web容器。</td></table>

* **Node.js是花最小的硬件成本，追求更高的并发，更高的处理性能。**

## 1.2 特点

&emsp; 所谓的特点，就是Node.js是如何解决服务器高性能瓶颈问题的。
<table><td bgcolor=#FF99CC>单线程</td><table>
&emsp; 在Java、PHP或者.net等服务器端语言中，会为每一个客户端连接创建一个新的线程。而每个线程需要耗费大约2MB内存。也就是说，理论上，一个8GB内存的服务器可以同时连接的最大用户数为4000个左右。要让Web应用程序支持更多的用户，就需要增加服务器的数量，而Web应用程序的硬件成本当然就上升了。
&emsp; Node.js不为每个客户连接创建一个新的线程，而仅仅使用一个线程。当有用户连接了，就触发一个内部事件，通过非阻塞I/O、事件驱动机制，让Node.js程序宏观上也是并行的。使用Node.js，一个8GB内存的服务器，可以同时处理超过4万用户的连接。
&emsp; 另外，带线程的带来的好处，还有操作系统完全不再有线程创建、销毁的时间开销。
&emsp; 坏处就是一个用户造成了线程的崩溃，整个服务都崩溃了，其他人也崩溃了。

![多线程](http://img.blog.csdn.net/20171202214112906?) &emsp; &emsp; &emsp; ![单线程](http://img.blog.csdn.net/20171202214322932?)

&emsp; 多线程、单线程的一个对比。
&emsp; 也就是说，单线程也能造成宏观上的“并发”。
<table><td bgcolor=#FF99CC>非阻塞I/O   non-blocking I/O </td><table>
&emsp; 例如，当在访问数据库取得数据的时候，需要一段时间。在传统的单线程处理机制中，在执行了访问数据库代码之后，整个线程都将暂停下来，等待数据库返回结果，才能执行后面的代码。<font color=red>也就是说，I/O阻塞了代码的执行，极大地降低了程序的执行效率。</font>
&emsp; 由于Node.js中采用了非阻塞型I/O机制，因此在执行了访问数据库的代码之后，将立即转而执行其后面的代码，把数据库返回结果的处理代码放在回调函数中，从而提高了程序的执行效率。
&emsp; 当某个I/O执行完毕时，将以事件的形式通知执行I/O操作的线程，线程执行这个事件的回调函数。为了处理异步I/O，线程必须有事件循环，不断的检查有没有未处理的事件，依次予以处理。
&emsp; 阻塞模式下，一个线程只能处理一项任务，要想提高吞吐量必须通过多线程。<font color=red>而非阻塞模式下，一个线程永远在执行计算操作，这个线程的CPU核心利用率永远是100%</font>。所以，这是一种特别有哲理的解决方案：与其人多，但是好多人闲着；还不如一个人玩命，往死里干活儿。

<table><td bgcolor=#FF99CC>&emsp; 事件驱动event-driven </td><table>
&emsp; 在Node中，客户端请求建立连接，提交数据等行为，会触发相应的事件。在Node中，在一个时刻，只能执行一个事件回调函数，但是在执行一个事件回调函数的中途，可以转而处理其他事件（比如，又有新用户连接了），然后返回继续执行原事件的回调函数，这种处理机制，称为“事件环”机制。
&emsp; Node.js底层是C++（V8也是C++写的）。<font color=red>底层代码中，近半数都用于事件队列、回调函数队列的构建</font>。用事件驱动来完成服务器的任务调度。用一个线程，担负起了处理非常多的任务的使命。

![调度](http://img.blog.csdn.net/20171202214716848?)

<table><td bgcolor=#CCFFFF>&emsp; <font color=red>单线程</font>，单线程的好处，减少了内存开销，操作系统的内存换页。
&emsp; 如果某一个事情，进入了，但是被I/O阻塞了，所以这个线程就阻塞了。
&emsp; <font color=red> 非阻塞I/O</font>，不会傻等I/O语句结束，而会执行后面的语句。
&emsp; 非阻塞就能解决问题了么？比如执行着小红的业务，执行过程中，小刚的I/O回调完成了，此时怎么办？？
&emsp; <font color=red>事件机制，事件环</font>，不管是新用户的请求，还是老用户的I/O完成，都将以事件方式加入事件环，等待调度。 </td><table>
&emsp; 说是三个特点，实际上是一个特点，离开谁都不行，都玩儿不转了。
&emsp; Node.js很像抠门的餐厅老板，只聘请1个服务员，服务很多人。结果，比很多服务员效率还高。
&emsp; Node.js中所有的I/O都是异步的，回调函数，套回调函数。

## 1.3 适合开发什么？

&emsp; Node.js适合用来开发什么样的应用程序呢？
&emsp; 善于I/O，不善于计算。因为Node.js最擅长的就是任务调度，如果你的业务有很多的CPU计算，实际上也相当于这个计算阻塞了这个单线程，就不适合Node开发。
&emsp; <font color=red>当应用程序需要处理大量并发的I/O，而在向客户端发出响应之前，应用程序内部并不需要进行非常复杂的处理的时候，Node.js非常适合。Node.js也非常适合与websocket配合，开发长连接的实时交互应用程序。</font>
比如：

 1. 用户表单手机
 2.  考试系统
 3. 聊天室
 4. 图文直播
 5. 提供JSON的API（为前台Angular使用）

## 1.4 Node.js无法挑战老牌3P（php、jsp、asp）

![对比图](http://img.blog.csdn.net/20171203141158354?)

# 二、Node.js安装

&emsp; Node.js和Java非常像，跨平台的。不管是Linux还是windows编程是完全一致的（有一些不一样，比如路径的表述）。Linux版本的Node.js环境和windows环境是不一样的，但是编程语言一样。很像Java虚拟机。
&emsp; 安装版本6.9.4。
&emsp; 装完之后，在环境变量中查看
&emsp; 

![环境变量](http://img.blog.csdn.net/20171203141527438?)

&emsp; 环境变量，就已经自动的填写进去了，就是我们node安装的目录。
<table><td bgcolor=#FFFF99>&emsp; 什么叫做环境变量？就是在系统的任何目录下，都能运行c:\program files\nodejs里面的程序。 </td><table>
&emsp; 在cmd中，输入node -v就能够查看nodejs的版本。我们现在的盘符，不在安装目录下，但是也能够运行，这就是因为有系统环境变量。系统的环境变量已经有了c:\program files\nodejs了，所以，这个文件夹中的node.exe就能够在任何盘符运行。
&emsp; 

![安装后的node](http://img.blog.csdn.net/20171203141739359?)

 
&emsp; 运行文件，就要用node命令来运行(进入node代码所在文件夹，使用node 相对地址)：
&emsp; 

![挂起状态](http://img.blog.csdn.net/20171203141937348?)

&emsp; Node.js是服务器的程序，写的js语句，都将运行在服务器上。返回给客户的，都是已经处理好的纯html。

01_HelloWorld.js

``` 
//require表示引包，引包就是引用自己的一个特殊功能
var http = require("http");
//创建服务器，参数是一个回调函数，表示如果有请求进来，要做什么
var server = http.createServer(function(req,res){
	//req表示请求，request;  res表示响应，response
	//设置HTTP头部，状态码是200，文件类型是html，字符集是utf8
	res.writeHead(200,{"Content-type":"text/html;charset=UTF-8"});
	res.end("哈哈哈哈，我买了五个iPhone" + (1+2+3) + "s");
});

//运行服务器，监听3000端口（端口号可以任改）
server.listen(3000,"127.0.0.1");
```

&emsp; 如果想修改程序，必须中断当前运行的服务器，重新node一次，刷新，才行。
&emsp; <font color=red>ctrl+c，就可以打断挂起的服务器程序</font>。此时按上箭头，能够快速调用最近的node命令。
&emsp; 本地的一个js，无法直接拖入浏览器运行，但是node可以让任何一个js文件，都可以通过它来运行。也就是说，<font color=red>node就是一个js的执行环境</font>。
&emsp; 要跑起来一个服务器，这个服务器的脚本，要以.js存储，是一个js文件。用node命令运行这个js文件即可。
<table><td bgcolor=#FFFF99> Node.js没有根目录的概念，因为它根本没有任何的web容器。 </td><table>
&emsp; 让node.js提供一个静态服务，是非常难的。
&emsp; 也就是说，node.js中，如果看见一个网址是 `127.0.0.1:3000/fang` ，那么一定有一个文件夹，叫做fang，可能/fang的物理文件。
&emsp; URL和真实物理文件，是没有关系的。URL是通过了Node的顶层路由设计，呈递某一个静态文件的。

# 三、HTTP模块

&emsp; Node.js中，将很多的功能划分为了一个个mudule，大陆的书翻译为模块；台湾的书翻译为模组。
&emsp; 这是因为，有一些程序需要使用fs功能（文件读取功能），有一些不用，所以为了效率，需要使用什么东西就require来引用什么东西。

``` 
//这个案例简单讲解http模块
//引用模块
var http = require("http");

//创建一个服务器，回调函数表示接收到请求之后做的事情
var server = http.createServer(function(req,res){
	//req参数表示请求，res表示响应
	console.log("服务器接收到了请求" + req.url);
	res.end();
});
//监听端口
server.listen(3000,"127.0.0.1");
```

![](http://img.blog.csdn.net/20171203142036283?)

&emsp; 设置一个响应头：

``` 
res.writeHead(200,{"Content-Type":"text/plain;charset=UTF8"});
```

![返回头部内容](http://img.blog.csdn.net/20171203142128175?)

&emsp; req里面能够使用的东西：最关键的就是req.url属性，表示用户的请求URL地址。所有的路由设计，都是通过req.url来实现的。
&emsp; 我们比较关心的不是拿到URL，而是识别这个URL。
&emsp; <font color=#008000>识别URL，用到两个新模块，第一个就是url模块，第二个就是querystring模块</font>

&emsp; 字符串查询，用querystring处理

``` 
querystring.parse('foo=bar&baz=qux&baz=quux&corge') // returns { foo: 'bar', baz: ['qux', 'quux'], corge: '' }
// Suppose gbkDecodeURIComponent function already exists,
// it can decode `gbk` encoding string
querystring.parse('w=%D6%D0%CE%C4&foo=bar', null, null, { decodeURIComponent: gbkDecodeURIComponent }) // returns { w: '中文', foo: 'bar' }
```
