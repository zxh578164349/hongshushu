---
layout: post
title: Apache+Tomcat集群
category: apache tomcat
tags: [apache tomcat]
---

## tomcat多个实例  

1 在tomcat安装文件夹中，建立一个文件夹instances用于存放实例,例如tomcat1与tomcat2,分别建立文件夹tomcat1,tomcat2  

2 复制tomcat中的conf文件夹放置在tomcat1与tomcat2,修改tomcat1中conf\server.xml配置文件，修改3处端口  

![]({{ site.baseurl }}/files/images/20160830/111.png) 
![]({{ site.baseurl }}/files/images/20160830/2222.png)   
![]({{ site.baseurl }}/files/images/20160830/333.png)  

tomcat2中的conf\server.xml也同樣修改成tomcat1不同的端口  

3 在tomat1與tomcat2中新建webapps文件夾,並且把項目放置在其中  

4 創建啓動文件startup.bat啓動文件,放置在tomcat1與tomcat2中,代碼如下:

{% highlight ruby %}
@echo off
if "%OS%" == "Windows_NT" setlocal
rem ---------------------------------------------------------------------------
rem CATALINA服务启动脚本
rem ---------------------------------------------------------------------------
rem 定义CATALINA_BASE和CATALINA_HOME。CATALINA_BASE:当前目录，CATALINA_HOME:tomcat目录
set "CATALINA_BASE=%cd%"
rem 设置启动文件
set "TOMCAT_START=%CATALINA_HOME%\bin\startup.bat"
rem 启动文件
call "%TOMCAT_START%"
:end
{% endhighlight %}

最後,同時啓動tomcat1與tomcat2的startup.bat ,檢查端口是否有衝突,如果沒有,基本上就可以啓動成功   

![]({{ site.baseurl }}/files/images/20160830/004.png)   
![]({{ site.baseurl }}/files/images/20160830/005.png) 

    


















