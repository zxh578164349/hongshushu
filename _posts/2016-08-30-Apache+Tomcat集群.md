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


## Apache + Tomcat 動靜分離  

動靜分離有2種:mod_proxy 與 mod_jk,前者是apache自帶的,後者需要另外下載一箇mod_jk文件,而本人用的是：mod_jk-apache-2.2.4.so  
把它放在apache的modules文件夾裏面,修改apacher的httpd.conf配置文件 ,加入如下代碼:  

{% highlight ruby %}
LoadModule jk_module modules/mod_jk-apache-2.2.4.so
<IfModule jk_module>
JKWorkersFile conf/workers.properties
JkMountFile conf/uriworkermap.properties
JkLogFile logs/mod_jk.log
JkLogLevel error
</IfModule>
{% endhighlight %}

代碼中,還要用到2箇文件,workers.properties  uriworkermap.properties 內容如下  


workers.properties  
{% highlight ruby %}
#========负载均衡控制器========
worker.list=lbServer,status


#========tomcat1========
worker.tomcat2.type=ajp13
worker.tomcat2.host=172.17.18.173
worker.tomcat2.port=8019
worker.tomcat2.lbfactor=10
#worker.tomcat2.cachesize=5

#========tomcat2========
worker.tomcat1.type=ajp13
worker.tomcat1.host=172.17.18.173
worker.tomcat1.port=18009
worker.tomcat1.lbfactor=10
#worker.tomcat1.cachesize=5

worker.lbServer.type=lb
worker.lbServer.balance_workers=tomcat1,tomcat2
worker.lbServer.sticky_session=1
worker.lbServer.sticky_session_force=0
worker.status.type=status
{% endhighlight %}

uriworkermap.properties  
{% highlight ruby %} 
#========lbServer,statuss與workers.properties的要相對應=============        
/*=lbServer                  #交給tomcat動態文件處理
/jkstatus=status             #查看apache後臺狀態

#交給apache靜態文件處理
!/Login/jquery/*=lbServer
!/Login/css/*=lbServer
!/Login/images/*=lbServer
!/Login/nprogress/*=lbServer
!/Login/uploadify/*=lbServer
!/Login/uploadfile/*=lbServer
!/Login/bootstrap/*=lbServer
!/Login/loginpage/*=lbServer
{% endhighlight %}

<font style="color:red;font-style:italic">注意,workers.propertiesk中的tomcat1  tomcat2兩箇名字要在tomcat各自實例tomcat1,tomcat2中的server.xml文件標明出來,如下:  </font>

![]({{ site.baseurl }}/files/images/20160830/006.png)


## Session共享  

tomcat1,tomcat2的server.xml的<Host></Host>加入如下代碼  

{% highlight ruby %}
   <!-- Session共享配置 -->
   <Cluster className="org.apache.catalina.ha.tcp.SimpleTcpCluster" channelSendOptions="8">
   <Manager className="org.apache.catalina.ha.session.DeltaManager" expireSessionsOnShutdown="false" notifyListenersOnReplication="true"/>
   <Channel className="org.apache.catalina.tribes.group.GroupChannel">
    <!-- 配置membership服务,有规律的发送脉冲广播 -->
    <Membership className="org.apache.catalina.tribes.membership.McastService" address="228.0.0.4" port="45564" frequency="500" dropTime="3000" bind="172.17.18.173"/>
    <!-- receiver组件,用于从其他成员接收复制的数据信息 -->
    <Receiver className="org.apache.catalina.tribes.transport.nio.NioReceiver"  
      address="172.17.18.173" port="4001" autoBind="100" selectorTimeout="5000"  
      maxThreads="6"/>
    <!-- sender组件,用于给组中其他成员发送复制的数据信息 -->
    <Sender className="org.apache.catalina.tribes.transport.ReplicationTransmitter">
     <Transport className="org.apache.catalina.tribes.transport.nio.PooledParallelSender"/>
    </Sender>
    <!-- 消息处理组件,用于改变通道的操作行为 -->
    <Interceptor className="org.apache.catalina.tribes.group.interceptors.TcpFailureDetector"/>
    <Interceptor className="org.apache.catalina.tribes.group.interceptors.MessageDispatch15Interceptor"/>
   </Channel>
   <!-- 过滤器,过滤了任何对静态页面、图形、js的请求,这些请求不会修改会话 -->
   <Valve className="org.apache.catalina.ha.tcp.ReplicationValve" filter=".*\.gif;.*\.js;.*\.jpg;.*\.htm;.*\.html;.*\.txt;"/>
   <!-- 过滤器,同mod_jk一起使用,在故障转移期间保证会话的粘性 -->
   <Valve className="org.apache.catalina.ha.session.JvmRouteBinderValve"/>
   <Deployer className="org.apache.catalina.ha.deploy.FarmWarDeployer"  
     tempDir="/tmp/war-temp/" deployDir="/tmp/war-deploy/" watchDir="/tmp/war-listen/"  
     watchEnabled="false" />
   
  <ClusterListener className="org.apache.catalina.ha.session.ClusterSessionListener"/>
  </Cluster>
{% endhighlight %}

<font style="color:red;font-style:italic">注意, membership bind="172.17.18.173"  Receiver address="172.17.18.173" 都是本機IP</font>
 

















