---
title: Ajax学习知识点  
date: 2018-03-01 21:50:08  
categories: ajax  
tags:
---

## Ajax基础
* 什么是服务器
    * 网页浏览过程分析
    * 如何配置自己的服务器程序(AMP)
* 什么是Ajax
    * 无刷新数据读取
        * 用户注册、在线聊天室
        * 异步、同步
* 使用Ajax
    * 基础：请求并显示静态TXT文件
        * 字符集编码
        * 缓存、阻止缓存
    * 动态数据：请求JS(或json)文件
        * eval的使用
        * DOM创建元素
    * 局部刷新：请求并显示部分网页文件

## Ajax原理
* HTTP请求方法
    * GET——用于获取数据(如：浏览帖子)
    * POST——用于上传数据(如：用户注册)
    * GET、POST区别
        * get是在url里面传数据：安全性、容量
        * 缓存

## 编写Ajax
* 创建对象
    * ActiveVObject("Microsoft.XMLHTTP")
* 连接服务器
    * open方法(方法、文件名、异步传输)
        * 同步和异步
* 发送请求
    * send()
* 请求状态监控
    * onreadystatechange事件
        * readyState属性：请求状态  
        > 0(未初始化) 还没调用open方法  
        > 1(载入)已调用send方法，正在发送请求  
        > 2(载入完成)send()方法完成，已收到全部相应内容    
        > 3(解析) 正在解析响应内容  
        > 4(完成)响应内容解析完成，可以在客户端调用了 

        * status属性：请求结果
        * responseText

## Ajax数据
* 数据类型
    * 什么叫数据类型——英语、中文
    * XML、Json
* 字符集
    * 所有文件字符集相同