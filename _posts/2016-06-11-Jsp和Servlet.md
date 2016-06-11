---
layout: post
title: Jsp与Servlet
category: jsp
tags: [面试]
---
## 什么是Servlet

Servlet就是一个小程序，一个里面集成各种业务处理方法的对象。

## Servlet生命周期

1.实例化

2.init()对象初始化

3.servce()运行各种业务的方法的

4.destroy()对象销毁  


## 什么是Jsp

Jsp是一种网页的格式，它不仅可以有html页面代码，也可以集成各种的业务处理方法，所以它其实也是Servlet。Servlet是分离到后台的业务处理方法，而Jsp是集成了Servlet的Html页面。所以，Jsp与Servlet一样也会有一个生命周期：  
init(),servece(),destory().

## Jsp常用内置对象

request  response session  application

1request

客户端请求的信息，被封装在这个对象里面。在请求完成之前，request一直有效
2response

服务器反馈给用户的信息，被封装在这个对象里面，它是HttpServletResponse的实例对象

3session

用户从请求到离开服务器所形成的一次会话内容

4applecation

可实现不不同用户的数据共享，可以存放一些全局变量，例如记录登录网站的人数。它在服务器开启期间一直存在，直到服务器关闭时候，才销毁

## Jsp声明一个变量

<%! int i=0%>

##Jsp引用一个变量

<%= username%>

## Jsp声明方法
<%  
     代码块
%>
